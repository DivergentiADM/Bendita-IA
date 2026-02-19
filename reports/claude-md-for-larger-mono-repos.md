# Entendiendo la Carga de CLAUDE.md en Monorepos Grandes

Cuando se trabaja con Claude Code en un monorepo, entender cómo se cargan los archivos CLAUDE.md en el contexto es crucial para organizar las instrucciones del proyecto de manera efectiva.

<table width="100%">
<tr>
<td><a href="../">← Volver a las Mejores Prácticas de Claude Code</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## Los Dos Mecanismos de Carga

Claude Code usa dos mecanismos distintos para cargar archivos CLAUDE.md:

### 1. Carga Ascendente (HACIA ARRIBA en el árbol de directorios)

Cuando inicias Claude Code, recorre **hacia arriba** desde tu directorio de trabajo actual hacia la raíz del sistema de archivos y carga cada CLAUDE.md que encuentre a lo largo del camino. Estos archivos se cargan **inmediatamente al inicio**.

### 2. Carga Descendente (HACIA ABAJO en el árbol de directorios)

Los archivos CLAUDE.md en subdirectorios por debajo de tu directorio de trabajo actual **NO se cargan al inicio**. Solo se incluyen cuando Claude lee archivos en esos subdirectorios durante tu sesión. Esto se conoce como **carga lazy**.

## Estructura de Ejemplo de Monorepo

Considera un monorepo típico con directorios separados para diferentes componentes:

```
/mymonorepo/
├── CLAUDE.md          # Instrucciones a nivel raíz (compartidas en todos los componentes)
├── frontend/
│   └── CLAUDE.md      # Instrucciones específicas del frontend
├── backend/
│   └── CLAUDE.md      # Instrucciones específicas del backend
└── api/
    └── CLAUDE.md      # Instrucciones específicas de la API
```

## Escenario 1: Ejecutar Claude Code desde el Directorio Raíz

Cuando ejecutas Claude Code desde `/mymonorepo/`:

```bash
cd /mymonorepo
claude
```

| Archivo | ¿Cargado al Inicio? | Razón |
|---------|--------------------|----|
| `/mymonorepo/CLAUDE.md` | Sí | Es tu directorio de trabajo actual |
| `/mymonorepo/frontend/CLAUDE.md` | No | Cargado solo cuando lees/editas archivos en `frontend/` |
| `/mymonorepo/backend/CLAUDE.md` | No | Cargado solo cuando lees/editas archivos en `backend/` |
| `/mymonorepo/api/CLAUDE.md` | No | Cargado solo cuando lees/editas archivos en `api/` |

## Escenario 2: Ejecutar Claude Code desde un Directorio de Componente

Cuando ejecutas Claude Code desde `/mymonorepo/frontend/`:

```bash
cd /mymonorepo/frontend
claude
```

| Archivo | ¿Cargado al Inicio? | Razón |
|---------|--------------------|----|
| `/mymonorepo/CLAUDE.md` | Sí | Es un directorio ancestro |
| `/mymonorepo/frontend/CLAUDE.md` | Sí | Es tu directorio de trabajo actual |
| `/mymonorepo/backend/CLAUDE.md` | No | Rama diferente del árbol de directorios |
| `/mymonorepo/api/CLAUDE.md` | No | Rama diferente del árbol de directorios |

## Conclusiones Clave

1. **Los ancestros siempre se cargan al inicio** — Claude recorre HACIA ARRIBA el árbol de directorios y carga todos los archivos CLAUDE.md que encuentre. Esto garantiza que siempre tengas acceso a las instrucciones a nivel raíz para todo el repositorio.

2. **Los descendientes se cargan de forma lazy** — Los archivos CLAUDE.md en subdirectorios solo se cargan cuando interactúas con archivos en esos subdirectorios. Esto evita que el contexto irrelevante infle tu sesión.

3. **Los hermanos nunca se cargan** — Si estás trabajando en `frontend/`, no obtendrás `backend/CLAUDE.md` ni `api/CLAUDE.md` cargados en el contexto.

4. **CLAUDE.md Global** — También puedes colocar un CLAUDE.md en `~/.claude/CLAUDE.md` en tu carpeta de inicio, que se aplica a TODAS las sesiones de Claude Code independientemente del proyecto.

## Por Qué Este Diseño Funciona para Monorepos

Esta estrategia de carga está diseñada intencionalmente para monorepos grandes:

- **Las instrucciones compartidas se propagan hacia abajo** — El CLAUDE.md a nivel raíz contiene convenciones de todo el repositorio, estándares de codificación y patrones comunes que se aplican en todas partes.

- **Las instrucciones específicas de componentes permanecen aisladas** — Los desarrolladores del frontend no necesitan que las instrucciones específicas del backend ensucien su contexto, y viceversa.

- **El contexto está optimizado** — Al cargar de forma lazy los archivos CLAUDE.md descendientes, Claude Code evita cargar potencialmente cientos de kilobytes de instrucciones irrelevantes al inicio.

## Mejores Prácticas

1. **Coloca las convenciones compartidas en el CLAUDE.md raíz** — Estándares de codificación, formatos de mensajes de commit, plantillas de PR y otras guías para todo el repositorio.

2. **Coloca las instrucciones específicas de componentes en el CLAUDE.md del componente** — Patrones específicos del framework, arquitectura de componentes, convenciones de pruebas únicas para ese componente.

3. **Usa CLAUDE.local.md para preferencias personales** — Agrégalo a `.gitignore` para instrucciones que no deben compartirse con el equipo.

---

## Fuentes

- [Documentación de Claude Code - Cómo Claude Busca Memorias](https://code.claude.com/docs/en/memory#how-claude-looks-up-memories)
- [Boris Cherny en X - Aclaración sobre la Carga de CLAUDE.md](https://x.com/bcherny/status/2016339448863355206)
