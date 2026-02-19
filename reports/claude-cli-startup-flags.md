# Referencia de Banderas de Inicio CLI de Claude Code

Una referencia completa de todas las banderas de línea de comandos disponibles al lanzar Claude Code desde el terminal.

<table width="100%">
<tr>
<td><a href="../">← Volver a las Mejores Prácticas de Claude Code</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## Tabla de Contenidos

1. [Gestión de Sesiones](#gestión-de-sesiones)
2. [Modelo y Configuración](#modelo-y-configuración)
3. [Permisos y Seguridad](#permisos-y-seguridad)
4. [Salida y Formato](#salida-y-formato)
5. [Prompt del Sistema](#prompt-del-sistema)
6. [Agente y Subagente](#agente-y-subagente)
7. [MCP y Plugins](#mcp-y-plugins)
8. [Directorio y Espacio de Trabajo](#directorio-y-espacio-de-trabajo)
9. [Presupuesto y Límites](#presupuesto-y-límites)
10. [Integración](#integración)
11. [Inicialización y Mantenimiento](#inicialización-y-mantenimiento)
12. [Depuración y Diagnósticos](#depuración-y-diagnósticos)
13. [Anulación de Configuración](#anulación-de-configuración)
14. [Versión y Ayuda](#versión-y-ayuda)
15. [Subcomandos](#subcomandos)
16. [Variables de Entorno](#variables-de-entorno)

---

## Gestión de Sesiones

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--continue` | `-c` | Continuar la conversación más reciente en el directorio actual |
| `--resume` | `-r` | Reanudar una sesión específica por ID o nombre, o mostrar el selector interactivo |
| `--from-pr <NÚMERO\|URL>` | | Reanudar sesiones vinculadas a un PR específico de GitHub |
| `--fork-session` | | Crear un nuevo ID de sesión al reanudar (usar con `--resume` o `--continue`) |
| `--session-id <UUID>` | | Usar un ID de sesión específico (debe ser UUID válido) |
| `--no-session-persistence` | | Deshabilitar la persistencia de sesión (solo modo print) |
| `--remote` | | Crear una nueva sesión web en claude.ai |
| `--teleport` | | Reanudar una sesión web en tu terminal local |

---

## Modelo y Configuración

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--model <NOMBRE>` | | Establecer el modelo con alias (`sonnet`, `opus`, `haiku`) o ID de modelo completo |
| `--fallback-model <NOMBRE>` | | Modelo de respaldo automático cuando el predeterminado está sobrecargado (solo modo print) |
| `--betas <LISTA>` | | Encabezados beta a incluir en las solicitudes de API (solo usuarios con clave API) |

---

## Permisos y Seguridad

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--dangerously-skip-permissions` | | Omitir TODOS los avisos de permisos. Usar con extrema precaución |
| `--allow-dangerously-skip-permissions` | | Habilitar la omisión de permisos como opción sin activarla |
| `--permission-mode <MODO>` | | Comenzar en el modo de permisos especificado: `default`, `plan`, `acceptEdits`, `bypassPermissions` |
| `--allowedTools <HERRAMIENTAS>` | | Herramientas que se ejecutan sin aviso (sintaxis de regla de permisos) |
| `--disallowedTools <HERRAMIENTAS>` | | Herramientas eliminadas completamente del contexto del modelo |
| `--tools <HERRAMIENTAS>` | | Restringir qué herramientas integradas puede usar Claude (usa `""` para deshabilitar todas) |
| `--permission-prompt-tool <HERRAMIENTA>` | | Especificar herramienta MCP para manejar avisos de permisos en modo no interactivo |

---

## Salida y Formato

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--print` | `-p` | Imprimir la respuesta sin modo interactivo (modo headless/SDK) |
| `--output-format <FORMATO>` | | Formato de salida: `text`, `json`, `stream-json` |
| `--input-format <FORMATO>` | | Formato de entrada: `text`, `stream-json` |
| `--json-schema <ESQUEMA>` | | Obtener JSON validado que coincida con el esquema (solo modo print) |
| `--include-partial-messages` | | Incluir eventos de streaming parciales (requiere `--print` y `--output-format=stream-json`) |
| `--verbose` | | Habilitar registro detallado con salida completa turno a turno |

---

## Prompt del Sistema

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--system-prompt <TEXTO>` | | Reemplazar todo el prompt del sistema con texto personalizado |
| `--system-prompt-file <RUTA>` | | Cargar el prompt del sistema desde un archivo, reemplazando el predeterminado (solo modo print) |
| `--append-system-prompt <TEXTO>` | | Añadir texto personalizado al prompt del sistema predeterminado |
| `--append-system-prompt-file <RUTA>` | | Añadir el contenido del archivo al prompt predeterminado (solo modo print) |

---

## Agente y Subagente

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--agent <NOMBRE>` | | Especificar un agente para la sesión actual |
| `--agents <JSON>` | | Definir subagentes personalizados dinámicamente mediante JSON |
| `--teammate-mode <MODO>` | | Establecer la visualización del equipo de agentes: `auto`, `in-process`, `tmux` |

---

## MCP y Plugins

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--mcp-config <RUTA\|JSON>` | | Cargar servidores MCP desde un archivo JSON o cadena |
| `--strict-mcp-config` | | Usar solo servidores MCP de `--mcp-config`, ignorar todos los demás |
| `--plugin-dir <RUTA>` | | Cargar plugins desde el directorio solo para esta sesión (repetible) |

---

## Directorio y Espacio de Trabajo

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--add-dir <RUTA>` | | Añadir directorios de trabajo adicionales para que Claude acceda |

---

## Presupuesto y Límites

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--max-budget-usd <MONTO>` | | Monto máximo en dólares para llamadas a la API antes de detenerse (solo modo print) |
| `--max-turns <NÚMERO>` | | Limitar el número de turnos agénticos (solo modo print) |

---

## Integración

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--chrome` | | Habilitar la integración del navegador Chrome para automatización web |
| `--no-chrome` | | Deshabilitar la integración del navegador Chrome para esta sesión |
| `--ide` | | Conectarse automáticamente al IDE al inicio si hay exactamente un IDE válido disponible |

---

## Inicialización y Mantenimiento

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--init` | | Ejecutar hooks de inicialización e iniciar el modo interactivo |
| `--init-only` | | Ejecutar hooks de inicialización y salir (sin sesión interactiva) |
| `--maintenance` | | Ejecutar hooks de mantenimiento y salir |

---

## Depuración y Diagnósticos

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--debug <CATEGORÍAS>` | | Habilitar el modo de depuración con filtrado opcional de categorías (por ejemplo, `"api,hooks"`) |

---

## Anulación de Configuración

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--settings <RUTA\|JSON>` | | Ruta al archivo JSON de configuración o cadena JSON a cargar |
| `--setting-sources <LISTA>` | | Lista de fuentes a cargar separadas por comas: `user`, `project`, `local` |
| `--disable-slash-commands` | | Deshabilitar todas las habilidades y comandos slash para esta sesión |

---

## Versión y Ayuda

| Bandera | Corta | Descripción |
|---------|-------|-------------|
| `--version` | `-v` | Mostrar el número de versión |
| `--help` | `-h` | Mostrar información de ayuda |

---

## Subcomandos

Estos no son banderas sino subcomandos de nivel superior que se ejecutan como `claude <subcomando>`:

| Subcomando | Descripción |
|------------|-------------|
| `claude` | Iniciar el REPL interactivo |
| `claude "consulta"` | Iniciar el REPL con el prompt inicial |
| `claude update` | Actualizar a la última versión |
| `claude mcp` | Configurar servidores MCP (`add`, `remove`, `list`, `get`, `enable`) |
| `claude doctor` | Ejecutar diagnósticos desde la línea de comandos |

---

## Variables de Entorno

Estas variables de entorno modifican el comportamiento de Claude Code al inicio:

| Variable | Descripción |
|----------|-------------|
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` | Habilitar los equipos de agentes experimentales |
| `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1` | Deshabilitar las características beta experimentales |
| `CLAUDE_CODE_TMPDIR` | Anular el directorio temporal para archivos internos |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | Deshabilitar la funcionalidad de tareas en segundo plano |
| `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` | Habilitar la carga de CLAUDE.md de directorios adicionales |
| `DISABLE_AUTOUPDATER=1` | Deshabilitar las actualizaciones automáticas |
| `MAX_THINKING_TOKENS` | Limitar el presupuesto de tokens de pensamiento (establecer en `0` para deshabilitar) |
| `CLAUDE_CODE_EFFORT_LEVEL` | Controlar la profundidad del pensamiento: `low`, `medium`, `high` |
| `USE_BUILTIN_RIPGREP=0` | Usar el ripgrep del sistema en lugar del integrado (Alpine Linux) |

---

## Fuentes

- [Referencia CLI de Claude Code](https://code.claude.com/docs/en/cli-reference)
- [Modo Headless de Claude Code](https://code.claude.com/docs/en/headless)
- [Configuración de Claude Code](https://code.claude.com/docs/en/setup)
- [CHANGELOG de Claude Code](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Flujos de Trabajo Comunes de Claude Code](https://code.claude.com/docs/en/common-workflows)
