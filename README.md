# claude-code-best-practice
La práctica hace a Claude perfecto

<a href="https://github.com/shanraisshan/claude-code-best-practice/stargazers">
  <img src="https://img.shields.io/github/stars/shanraisshan/claude-code-best-practice?style=social" alt="Estrellas de GitHub">
</a>

<p align="center">
  <img src="!/claude-jumping.svg" alt="Mascota de Claude Code saltando" width="120" height="100">
</p>

<table align="center">
  <tr>
    <td><img src="!/boris-cherny.png" alt="Creador de Claude Code" width="100%"></td>
    <td><img src="!/boris-cherny-2.png" alt="Creador de Claude Code" width="100%"></td>
  </tr>
</table>


## CONCEPTOS

> **Nota:** Los comandos slash personalizados han sido fusionados con las habilidades. Los archivos en `.claude/commands/` todavía funcionan, pero las habilidades (`.claude/skills/`) son recomendadas ya que admiten características adicionales como archivos de soporte, control de invocación y ejecución de subagentes.

- **[Habilidades](https://code.claude.com/docs/en/skills)** - Conocimiento reutilizable, flujos de trabajo y comandos slash que Claude puede cargar bajo demanda o que invocas con `/skill-name`
- **[Agentes](https://code.claude.com/docs/en/sub-agents)** - Agentes personalizados en `.claude/agents/` con su propio nombre, color, herramientas, permisos y modelo — usables como agente principal predeterminado (`"agent"` en settings.json) o como subagentes aislados mediante la herramienta Task
- **[Memoria](https://code.claude.com/docs/en/memory)** - Contexto persistente mediante archivos CLAUDE.md e importaciones `@path` que Claude ve en cada sesión
- **[Reglas](https://code.claude.com/docs/en/memory#modular-rules-with-clauderules)** - Instrucciones modulares por tema en `.claude/rules/*.md` con alcance de ruta opcional mediante frontmatter
- **[Hooks](https://code.claude.com/docs/en/hooks)** - Scripts deterministas que se ejecutan fuera del bucle agéntico en eventos específicos
- **[Servidores MCP](https://code.claude.com/docs/en/mcp)** - Conexiones del Protocolo de Contexto de Modelos a herramientas externas, bases de datos y APIs
- **[Plugins](https://code.claude.com/docs/en/plugins)** - Paquetes distribuibles que agrupan habilidades, subagentes, hooks y servidores MCP
- **[Mercados](https://code.claude.com/docs/en/discover-plugins)** - Alojar y descubrir colecciones de plugins
- **[Sandbox](https://code.claude.com/docs/en/sandbox)** - Entorno de ejecución con aislamiento de archivos y red que mejora la seguridad mientras reduce los avisos de permisos
- **[Estilos de Salida](https://code.claude.com/docs/en/output-styles)** - Tono y formato de respuesta configurables — Explicativo, Aprendizaje o Personalizado
- **[Configuración](https://code.claude.com/docs/en/settings)** - Sistema de configuración jerárquico para el comportamiento de Claude Code (37 ajustes, 84 variables de entorno)
- **[Permisos](https://code.claude.com/docs/en/iam)** - Control de acceso detallado para herramientas y operaciones con sintaxis de comodines

**Descripción General de Extensiones:** Consulta [Extender Claude Code](https://code.claude.com/docs/en/features-overview) para saber cuándo usar cada característica y cómo se combinan.

## MI EXPERIENCIA

■ **Flujos de Trabajo**
- Claude.md no debe exceder las 150+ líneas. (todavía no es 100% garantizado)
- usa comandos para tus flujos de trabajo en lugar de agentes
- ten subagentes específicos de características (contexto extra) con habilidades (divulgación progresiva) en lugar de qa general, ingeniero backend.
- /memory, /rules, constitution.md no garantiza nada
- haz /compact manual a máximo 50%
- siempre comienza con el modo de planificación
- las subtareas deben ser tan pequeñas que puedan completarse en menos del 50% del contexto
- cc vanilla es mejor que cualquier flujo de trabajo con tareas más pequeñas
- haz commit frecuentemente, tan pronto como se complete una tarea, haz commit.

■ **Utilidades**
- Terminal iTerm en lugar de IDE (problema de crash)
- Wispr Flow para prompts por voz (productividad 10x)
- claude-code-voice-hooks para retroalimentación de Claude
- línea de estado para conciencia del contexto y compactación rápida
- git worktrees para desarrollo paralelo
- /permissions con sintaxis de comodines (`Bash(npm run *)`, `Edit(/docs/**)`) en lugar de dangerously-skip-permissions
- /sandbox para reducir avisos de permisos con aislamiento de archivos y red
- estilos de salida: usa Explicativo cuando aprendes una nueva base de código, Aprendizaje para coaching
- /keybindings para reasignar cualquier tecla, recarga en vivo de la configuración

■ **Depuración**
- /doctor
- siempre pide a Claude que ejecute el terminal (del que quieres ver los logs) como tarea en segundo plano para mejor depuración
- usa mcp (Claude en Chrome, playwright, chrome dev tool) para que Claude vea los logs de la consola de Chrome por sí mismo
- proporciona capturas de pantalla del problema

## CONSEJOS DE BORIS CHERNY (CREADOR DE CLAUDE CODE)
- [Feb 2026 - 12 Consejos](reports/claude-boris-tips-feb-26.md) ([Hilo de Reddit](https://www.reddit.com/r/ClaudeAI/comments/1r2m8ma/12_claude_code_tips_from_creator_of_claude_code/))

## INGENIERÍA DE CONTEXTO
- [Humanlayer - Cómo escribir un buen Claude.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
- [Claude.md para monorepos más grandes - Boris Cherny en X](https://github.com/shanraisshan/claude-code-best-practice/blob/main/reports/claude-md-for-larger-mono-repos.md)

## FLUJOS DE TRABAJO
- [RPI](workflow/rpi/rpi-workflow.md)
- [Flujo de trabajo Boris Feb26](https://x.com/bcherny/status/2017742741636321619)
- [Plugin Ralph con sandbox](https://www.youtube.com/watch?v=eAtvoGlpeRU)
- [Human Layer RPI - Research Plan Implement](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)
- [AgentOs - 2026 es exagerado (Brian Casel)](https://www.youtube.com/watch?v=0hdFJA-ho3c)
- [Github Speckit](https://github.com/github/spec-kit)
- [GSD - Get Shit Done](https://github.com/glittercowboy/get-shit-done)
- [OpenSpec OPSX](https://github.com/Fission-AI/OpenSpec/blob/main/docs/opsx.md)
- [Superpower](https://github.com/obra/superpowers)
- [Flujo de Trabajo de Andrej Karpathy](https://github.com/forrestchang/andrej-karpathy-skills)
- [Flujo de Trabajo del Creador del Bot Clawd](https://www.youtube.com/watch?v=8lF7HmQ_RgY)




## INSPIRACIÓN EN CARACTERÍSTICAS DE CLAUDE CODE

- [Tareas de Claude Code - inspirado en beats](https://www.reddit.com/r/ClaudeAI/comments/1qkjznp/anthropic_replaced_claude_codes_old_todos_with/) [Inspiración](https://github.com/steveyegge/beads)
- [Plugin Ralph](https://x.com/GeoffreyHuntley/status/2015031262692753449)

## ARQUITECTURA COMANDO + HABILIDAD + SUBAGENTE

<p align="center">
  <img src="!/command-skill-agent-flow.svg" alt="Flujo de Arquitectura Comando Habilidad Agente" width="600">
</p>

| Componente | Rol | Ejemplo |
|------------|-----|---------|
| **Comando** | Punto de entrada, interacción con el usuario | `/weather-orchestrator` |
| **Agente** | Orquesta el flujo de trabajo con habilidades precargadas | agente `weather` |
| **Habilidades** | Conocimiento del dominio inyectado al inicio | `weather-fetcher`, `weather-transformer` |

**Cuándo usar:** Flujos de trabajo de múltiples pasos • Inyección de conocimiento específico del dominio • Tareas secuenciales • Componentes reutilizables

**Por qué funciona:** Divulgación progresiva • Contexto de ejecución único • Separación limpia • Reutilizabilidad

Consulta [weather-orchestration-architecture](weather-orchestration/weather-orchestration-architecture.md) para detalles de implementación.

## TÉRMINOS DE IA

| | | | | |
|---|---|---|---|---|
| Ingeniería Agéntica | AI Slop | Hinchazón de Contexto | Ingeniería de Contexto | Degradación de Contexto |
| Zona Muerta | Alucinación | Scaffolding | Orquestación | Vibe Coding |

[**Ver Lista Completa →**](https://github.com/shanraisshan/claude-code-codex-cursor-gemini/blob/main/reports/ai-terms.md)

## BANDERAS DE INICIO CLI

| | | | | |
|---|---|---|---|---|
| `--dangerously-skip-permissions` | `--model` | `--print` | `--resume` | `--continue` |
| `--system-prompt` | `--verbose` | `--debug` | `--init` | `--max-turns` |

[**Ver Lista Completa →**](reports/claude-cli-startup-flags.md)

## COMANDOS DE CLAUDE

| | | | | |
|---|---|---|---|---|
| `/compact` | `/context` | `/model` | `/plan` | `/config` |
| `/clear` | `/cost` | `/memory` | `/doctor` | `/rewind` |

[**Ver Lista Completa →**](reports/claude-commands.md)


## CONFIGURACIÓN DE CLAUDE

| | |
|---|---|
| [**Configuración de Claude**](reports/claude-settings.md) | [**Configuración Global vs de Proyecto**](reports/claude-global-vs-project-settings.md) |


## SERVIDORES MCP PARA USO DIARIO

> *"Me excedí con 15 servidores MCP pensando que más = mejor. Terminé usando solo 4 diariamente."* — [r/mcp](https://reddit.com/r/mcp/comments/1mj0fxs/) (682 votos positivos)

| Servidor MCP | Qué Hace | Recursos |
|--------------|---------|---------|
| [**Context7**](https://github.com/upstash/context7) | Obtiene documentación actualizada de librerías en el contexto. Previene APIs alucinadas por datos de entrenamiento obsoletos | [Reddit: "por lejos el mejor MCP para programar"](https://reddit.com/r/mcp/comments/1qarjqm/) · [npm](https://www.npmjs.com/package/@upstash/context7-mcp) |
| [**Playwright**](https://github.com/microsoft/playwright-mcp) | Automatización del navegador — implementar, probar y verificar características de UI de forma autónoma. Capturas de pantalla, navegación, pruebas de formularios | [Reddit: esencial para frontend](https://reddit.com/r/mcp/comments/1m59pk0/) · [Docs](https://playwright.dev/) |
| [**Claude en Chrome**](https://github.com/nicobailon/claude-code-in-chrome-mcp) | Conecta a Claude con tu navegador Chrome real — inspeccionar consola, red, DOM. Depura lo que los usuarios realmente ven | [Reddit: "un cambio de juego" para depuración](https://reddit.com/r/mcp/comments/1qarjqm/5_mcps_that_have_genuinely_made_me_10x_faster/nza0i7t/) · [Informe de Comparación](reports/claude-in-chrome-v-chrome-devtools-mcp.md) |
| [**DeepWiki**](https://github.com/devanshusemwal/deepwiki-mcp) | Obtiene documentación estructurada estilo wiki para cualquier repositorio de GitHub — arquitectura, superficie de API, relaciones | [Reddit: "ponlo detrás de una puerta de enlace con Context7"](https://reddit.com/r/mcp/comments/1qarjqm/) |
| [**Excalidraw**](https://github.com/antonpk1/excalidraw-mcp-app) | Genera diagramas de arquitectura, diagramas de flujo y diseños de sistemas como bocetos dibujados a mano en Excalidraw desde prompts | [GitHub](https://github.com/antonpk1/excalidraw-mcp-app) |

Investigación (Context7/DeepWiki) -> Depuración (Playwright/Chrome) -> Documentación (Excalidraw)

## REPORTES

| Reporte | Descripción |
|---------|-------------|
| [SDK de Agente vs Prompts del Sistema CLI](reports/claude-agent-sdk-vs-cli-system-prompts.md) | Por qué el CLI de Claude y las salidas del SDK de Agente pueden diferir — arquitectura del prompt del sistema y determinismo |
| [Comparación de MCP de Automatización del Navegador](reports/claude-in-chrome-v-chrome-devtools-mcp.md) | Comparación de Playwright, Chrome DevTools y Claude en Chrome para pruebas automatizadas |
| [Banderas de Inicio CLI de Claude Code](reports/claude-cli-startup-flags.md) | Referencia completa de todas las banderas CLI, subcomandos y variables de entorno |
| [Referencia de Comandos de Claude Code](reports/claude-commands.md) | Referencia completa de todos los comandos slash, atajos de teclado y modos de entrada |
| [Referencia de Configuración de Claude Code](reports/claude-settings.md) | Guía completa de todas las opciones de configuración de `settings.json` |
| [Carga de CLAUDE.md en Monorepos](reports/claude-md-for-larger-mono-repos.md) | Entendiendo el comportamiento de carga ascendente vs descendente para archivos CLAUDE.md |
| [Configuración Global vs de Proyecto](reports/claude-global-vs-project-settings.md) | Qué características son solo globales (`~/.claude/`) vs de doble alcance, incluyendo Tareas y Equipos de Agentes |
| [Descubrimiento de Habilidades en Monorepos](reports/claude-skills-for-larger-mono-repos.md) | Cómo se descubren y cargan las habilidades en proyectos de monorepo grandes |
| [Frontmatter de Memoria de Agente](reports/claude-agent-memory.md) | Alcances de memoria persistente (`user`, `project`, `local`) para subagentes — permitiendo a los agentes aprender entre sesiones |
| [12 Consejos de Personalización de Boris Cherny](reports/claude-boris-tips-feb-26.md) | 12 formas de personalizar Claude Code — desde la configuración del terminal hasta plugins, agentes, hooks y estilos de salida |
