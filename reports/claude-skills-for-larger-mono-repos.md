# Entendiendo el Descubrimiento de Habilidades de Claude en Monorepos Grandes

Cuando se trabaja con Claude Code en un monorepo, entender cómo se descubren y cargan las habilidades en el contexto es crucial para organizar las capacidades específicas del proyecto de manera efectiva.

<table width="100%">
<tr>
<td><a href="../">← Volver a las Mejores Prácticas de Claude Code</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## Diferencia Importante con CLAUDE.md

**Las habilidades NO tienen el mismo comportamiento de carga que los archivos CLAUDE.md.** Mientras que los archivos CLAUDE.md recorren HACIA ARRIBA el árbol de directorios (carga ascendente), las habilidades usan un mecanismo de descubrimiento diferente enfocado en directorios anidados dentro de tu proyecto.

## Cómo Se Descubren las Habilidades

### 1. Ubicaciones Estándar de Habilidades

Las habilidades se cargan desde estas ubicaciones fijas según el alcance:

| Ubicación | Ruta | Aplica a |
|-----------|------|---------|
| Empresa | Configuración gestionada | Todos los usuarios de la organización |
| Personal | `~/.claude/skills/<skill-name>/SKILL.md` | Todos tus proyectos |
| Proyecto | `.claude/skills/<skill-name>/SKILL.md` | Solo este proyecto |
| Plugin | `<plugin>/skills/<skill-name>/SKILL.md` | Donde el plugin esté habilitado |

### 2. Descubrimiento Automático desde Directorios Anidados

Cuando trabajas con archivos en subdirectorios, Claude Code descubre automáticamente habilidades desde directorios `.claude/skills/` anidados. Por ejemplo, si estás editando un archivo en `packages/frontend/`, Claude Code también busca habilidades en `packages/frontend/.claude/skills/`.

Esto soporta configuraciones de monorepo donde los paquetes tienen sus propias habilidades.

## Estructura de Ejemplo de Monorepo

Considera un monorepo típico con paquetes separados:

```
/mymonorepo/
├── .claude/
│   └── skills/
│       └── shared-conventions/SKILL.md    # Habilidad a nivel de proyecto
├── packages/
│   ├── frontend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── react-patterns/SKILL.md  # Habilidad específica del frontend
│   │   └── src/
│   │       └── App.tsx
│   ├── backend/
│   │   ├── .claude/
│   │   │   └── skills/
│   │   │       └── api-design/SKILL.md      # Habilidad específica del backend
│   │   └── src/
│   └── shared/
│       ├── .claude/
│       │   └── skills/
│       │       └── utils-patterns/SKILL.md  # Habilidad de utilidades compartidas
│       └── src/
```

## Escenario 1: Recién Iniciado Claude en la Raíz (Sin Archivos Editados Aún)

Cuando ejecutas Claude Code desde `/mymonorepo/` y aún no has editado ningún archivo:

```bash
cd /mymonorepo
claude
# Recién iniciado - sin archivos editados todavía
```

| Habilidad | ¿En Contexto? | Razón |
|-----------|--------------|-------|
| `shared-conventions` | **Sí** | Habilidad a nivel de proyecto en `.claude/skills/` raíz |
| `react-patterns` | **No** | No descubierta - no has trabajado con archivos en `packages/frontend/` |
| `api-design` | **No** | No descubierta - no has trabajado con archivos en `packages/backend/` |
| `utils-patterns` | **No** | No descubierta - no has trabajado con archivos en `packages/shared/` |

## Escenario 2: Después de Editar Archivos en un Paquete

Después de pedirle a Claude que edite `packages/frontend/src/App.tsx`:

| Habilidad | ¿En Contexto? | Razón |
|-----------|--------------|-------|
| `shared-conventions` | **Sí** | Habilidad a nivel de proyecto en `.claude/skills/` raíz |
| `react-patterns` | **Sí** | Descubierta al editar archivos en `packages/frontend/` |
| `api-design` | **No** | Todavía no descubierta - no has trabajado con archivos en `packages/backend/` |
| `utils-patterns` | **No** | Todavía no descubierta - no has trabajado con archivos en `packages/shared/` |

**Información clave**: Las habilidades anidadas se descubren **bajo demanda** cuando trabajas con archivos en esos directorios. No se precargan al inicio de la sesión.

## Comportamiento Clave: Descripción vs Contenido Completo

Las descripciones de habilidades se cargan en el contexto para que Claude sepa qué está disponible, pero **el contenido completo de la habilidad solo se carga cuando se invoca**. Esta es una optimización importante:

- **Descripciones**: Siempre en contexto (dentro del presupuesto de caracteres)
- **Contenido completo**: Cargado bajo demanda cuando se invoca la habilidad

> Nota: Los subagentes con habilidades precargadas funcionan de manera diferente - el contenido completo de la habilidad se inyecta al inicio.

## Orden de Prioridad (Cuando las Habilidades Comparten Nombres)

Cuando las habilidades comparten el mismo nombre entre niveles, las ubicaciones de mayor prioridad ganan:

| Prioridad | Ubicación | Alcance |
|-----------|----------|---------|
| 1 (mayor) | Empresa | Toda la organización |
| 2 | Personal (`~/.claude/skills/`) | Todos tus proyectos |
| 3 (menor) | Proyecto (`.claude/skills/`) | Solo este proyecto |

Las habilidades de plugins usan un espacio de nombres `plugin-name:skill-name`, por lo que no pueden entrar en conflicto con otros niveles.

## Por Qué Este Diseño Funciona para Monorepos

- **Las habilidades específicas del paquete permanecen aisladas** — Los desarrolladores del frontend que trabajan en `packages/frontend/` obtienen habilidades específicas del frontend sin que las habilidades del backend ensucien el contexto.

- **El descubrimiento automático reduce la configuración** — No es necesario registrar explícitamente habilidades a nivel de paquete; se descubren cuando trabajas en esos directorios.

- **El contexto está optimizado** — Solo las descripciones de habilidades se cargan inicialmente, y las habilidades anidadas se descubren bajo demanda.

- **Los equipos pueden mantener sus propias habilidades** — Cada equipo de paquete puede definir habilidades específicas de su dominio sin coordinar con otros equipos.

## Consideraciones del Presupuesto de Caracteres

Las descripciones de habilidades se cargan en el contexto hasta un presupuesto de caracteres (predeterminado 15,000 caracteres). En monorepos grandes con muchos paquetes y habilidades, puedes alcanzar este límite.

- Ejecuta `/context` para verificar advertencias sobre habilidades excluidas
- Establece la variable de entorno `SLASH_COMMAND_TOOL_CHAR_BUDGET` para aumentar el límite

## Mejores Prácticas

1. **Coloca flujos de trabajo compartidos en `.claude/skills/` raíz** — Convenciones de todo el repositorio, flujos de trabajo de commit y patrones compartidos.

2. **Coloca habilidades específicas del paquete en `.claude/skills/` del paquete** — Patrones específicos del framework, convenciones de componentes, utilidades de pruebas únicas para ese paquete.

3. **Usa `disable-model-invocation: true` para habilidades peligrosas** — Las habilidades de despliegue o destructivas deben requerir invocación explícita del usuario.

4. **Mantén las descripciones de habilidades concisas** — Las descripciones siempre están en el contexto (hasta el presupuesto de caracteres), por lo que las descripciones verbosas desperdician espacio de contexto.

5. **Usa espacios de nombres en los nombres de habilidades** — Considera prefijar con nombres de paquetes (por ejemplo, `frontend-review`, `backend-deploy`) para evitar confusiones.

## Comparación: Carga de Habilidades vs CLAUDE.md

| Comportamiento | CLAUDE.md | Habilidades |
|----------------|-----------|-------------|
| Carga ascendente (HACIA ARRIBA en el árbol de directorios) | Sí | No |
| Descubrimiento anidado/descendente (HACIA ABAJO en el árbol de directorios) | Sí (lazy) | Sí (descubrimiento automático) |
| Ubicación global | `~/.claude/CLAUDE.md` | `~/.claude/skills/` |
| Ubicación del proyecto | `.claude/` o raíz del repositorio | `.claude/skills/` |
| Carga de contenido | Contenido completo | Solo descripción (completo al invocar) |

---

## Fuentes

- [Documentación de Claude Code - Extender Claude con Habilidades](https://code.claude.com/docs/en/skills)
- [Documentación de Claude Code - Descubrimiento Automático desde Directorios Anidados](https://code.claude.com/docs/en/skills#automatic-discovery-from-nested-directories)
