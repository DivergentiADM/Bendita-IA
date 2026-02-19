# Claude Code: Frontmatter de Memoria de Agente

Memoria persistente para subagentes — permitiendo a los agentes aprender, recordar y construir conocimiento entre sesiones.

<table width="100%">
<tr>
<td><a href="../">← Volver a las Mejores Prácticas de Claude Code</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Descripción General

Introducido en **Claude Code v2.1.33** (febrero de 2026), el campo frontmatter `memory` le da a cada subagente su propio almacén persistente de conocimiento basado en markdown. Antes de esto, cada invocación de agente comenzaba desde cero.

```yaml
---
name: code-reviewer
description: Revisa el código en busca de calidad y mejores prácticas
tools: Read, Write, Edit, Bash
model: sonnet
memory: user
---

Eres un revisor de código. A medida que revisas el código, actualiza tu memoria de agente con
patrones, convenciones y problemas recurrentes que descubras.
```

---

## Alcances de Memoria

| Alcance | Ubicación de Almacenamiento | Control de Versiones | Compartido | Mejor Para |
|---------|----------------------------|--------------------|-----------|-----------|
| `user` | `~/.claude/agent-memory/<agent-name>/` | No | No | Conocimiento entre proyectos (recomendado por defecto) |
| `project` | `.claude/agent-memory/<agent-name>/` | Sí | Sí | Conocimiento específico del proyecto que el equipo debe compartir |
| `local` | `.claude/agent-memory-local/<agent-name>/` | No (ignorado por git) | No | Conocimiento específico del proyecto que es personal |

Estos alcances reflejan la jerarquía de configuración (`~/.claude/settings.json` → `.claude/settings.json` → `.claude/settings.local.json`).

---

## Cómo Funciona

1. **Al inicio**: Las primeras 200 líneas de `MEMORY.md` se inyectan en el prompt del sistema del agente
2. **Acceso a herramientas**: `Read`, `Write`, `Edit` se habilitan automáticamente para que el agente pueda gestionar su memoria
3. **Durante la ejecución**: El agente lee y escribe en su directorio de memoria libremente
4. **Curación**: Si `MEMORY.md` supera las 200 líneas, el agente mueve los detalles a archivos temáticos específicos

```
~/.claude/agent-memory/code-reviewer/     # ejemplo de alcance user
├── MEMORY.md                              # Archivo principal (las primeras 200 líneas se cargan)
├── react-patterns.md                      # Archivo específico por tema
└── security-checklist.md                  # Archivo específico por tema
```

---

## Memoria de Agente vs Otros Sistemas de Memoria

| Sistema | Quién Escribe | Quién Lee | Alcance |
|---------|--------------|-----------|---------|
| **CLAUDE.md** | Tú (manualmente) | Claude principal + todos los agentes | Proyecto |
| **Memoria automática** | Claude principal (auto) | Solo Claude principal | Por proyecto por usuario |
| **Comando `/memory`** | Tú (mediante editor) | Solo Claude principal | Por proyecto por usuario |
| **Memoria de agente** | El agente mismo | Solo ese agente específico | Configurable (user/project/local) |

Estos sistemas son **complementarios** — un agente lee tanto CLAUDE.md (contexto del proyecto) como su propia memoria (conocimiento específico del agente).

---

## Ejemplo Práctico

```yaml
---
name: api-developer
description: Implementa endpoints de API siguiendo las convenciones del equipo
tools: Read, Write, Edit, Bash
model: sonnet
memory: project
skills:
  - api-conventions
  - error-handling-patterns
---

Implementa endpoints de API. Sigue las convenciones de tus habilidades precargadas.
A medida que trabajas, guarda las decisiones arquitectónicas y patrones en tu memoria.
```

Esto combina **habilidades** (conocimiento estático al inicio) con **memoria** (conocimiento dinámico construido con el tiempo).

---

## Consejos

- **Indicar el uso de la memoria** — Incluye instrucciones explícitas: `"Antes de comenzar, revisa tu memoria. Después de completar, actualiza tu memoria con lo que aprendiste."`
- **Solicitar verificaciones de memoria** al invocar agentes: `"Revisa este PR y comprueba tu memoria en busca de patrones que hayas visto antes."`
- **Elegir el alcance correcto** — `user` para entre proyectos, `project` para compartir en equipo, `local` para uso personal

---

## Fuentes

- [Crear subagentes personalizados — Documentación de Claude Code](https://code.claude.com/docs/en/sub-agents)
- [Gestionar la memoria de Claude — Documentación de Claude Code](https://code.claude.com/docs/en/memory)
- [Notas de lanzamiento de Claude Code v2.1.33](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
