# Referencia de Comandos de Claude Code

Una referencia completa de todos los comandos slash disponibles en el modo interactivo de Claude Code.

<table width="100%">
<tr>
<td><a href="../">← Volver a las Mejores Prácticas de Claude Code</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## Tabla de Contenidos

1. [Gestión de Sesiones](#gestión-de-sesiones)
2. [Contexto y Costo](#contexto-y-costo)
3. [Modelo y Planificación](#modelo-y-planificación)
4. [Proyecto y Memoria](#proyecto-y-memoria)
5. [Configuración](#configuración)
6. [Extensiones e Integraciones](#extensiones-e-integraciones)
7. [Diagnósticos y Depuración](#diagnósticos-y-depuración)
8. [Importar / Exportar](#importar--exportar)
9. [Autenticación](#autenticación)
10. [Modos de Entrada y Prefijos](#modos-de-entrada-y-prefijos)
11. [Comandos Dinámicos](#comandos-dinámicos)
12. [Banderas CLI](#banderas-cli)
13. [Atajos de Teclado](#atajos-de-teclado)

---

## Gestión de Sesiones

| Comando | Descripción |
|---------|-------------|
| `/clear` | Borrar el historial de conversación y comenzar de nuevo |
| `/compact [instrucciones]` | Comprimir la conversación para liberar la ventana de contexto. Las instrucciones opcionales enfocan la compactación en temas específicos |
| `/rename <nombre>` | Renombrar la sesión actual para una identificación más fácil |
| `/resume [sesión]` | Reanudar una conversación anterior por ID o nombre, o abrir el selector de sesiones |
| `/rewind` | Rebobinar la conversación y/o el código a un punto anterior, o resumir desde un mensaje seleccionado |
| `/fork` | Bifurcar la conversación actual en una nueva sesión |
| `/teleport` | Reanudar una sesión remota desde claude.ai (solo suscriptores) |
| `/exit` | Salir del REPL |

---

## Contexto y Costo

| Comando | Descripción |
|---------|-------------|
| `/context` | Visualizar el uso actual del contexto como una cuadrícula de colores con conteos de tokens y porcentajes |
| `/cost` | Mostrar estadísticas de uso de tokens y gasto para la sesión actual |
| `/usage` | Mostrar los límites de uso del plan y el estado de límite de velocidad (solo planes de suscripción) |
| `/stats` | Visualizar el uso diario, historial de sesiones, rachas y preferencias de modelos. Admite filtrado por rango de fechas |

---

## Modelo y Planificación

| Comando | Descripción |
|---------|-------------|
| `/model` | Cambiar modelos (haiku, sonnet, opus) y ajustar el nivel de esfuerzo de Opus 4.6 con las teclas de flecha |
| `/plan` | Entrar en modo de planificación de solo lectura donde Claude sugiere enfoques sin realizar cambios |
| `/fast` | Alternar el modo rápido — el mismo modelo Opus 4.6 con salida más rápida |

---

## Proyecto y Memoria

| Comando | Descripción |
|---------|-------------|
| `/init` | Inicializar un nuevo proyecto con guía CLAUDE.md |
| `/memory` | Ver y editar archivos de memoria CLAUDE.md (alcance de usuario, proyecto y local) |

---

## Configuración

| Comando | Descripción |
|---------|-------------|
| `/config` | Abrir la interfaz de Configuración interactiva con funcionalidad de búsqueda |
| `/permissions` | Ver o actualizar los permisos de herramientas |
| `/theme` | Cambiar el tema de color |
| `/vim` | Habilitar el modo de edición estilo vim |
| `/terminal-setup` | Habilitar shift+enter para saltos de línea en terminales de IDE, Apple Terminal, Warp y Alacritty |
| `/keybindings` | Personalizar atajos de teclado por contexto, crear secuencias de acordes |
| `/statusline` | Configurar la interfaz de línea de estado de Claude Code |
| `/sandbox` | Configurar el sandboxing con el estado de las dependencias |

---

## Extensiones e Integraciones

| Comando | Descripción |
|---------|-------------|
| `/agents` | Gestionar subagentes personalizados — ver, crear, editar, eliminar |
| `/skills` | Ver las habilidades disponibles y sus descripciones |
| `/hooks` | Interfaz interactiva para gestionar hooks |
| `/mcp` | Gestionar conexiones de servidores MCP — añadir, habilitar, listar, obtener información, autenticación OAuth |
| `/plugin` | Gestionar plugins — instalar, desinstalar, habilitar, deshabilitar, explorar mercados |
| `/ide` | Conectar a la integración del IDE |

---

## Diagnósticos y Depuración

| Comando | Descripción |
|---------|-------------|
| `/doctor` | Comprobar la salud de tu instalación de Claude Code. Detecta permisos inalcanzables, problemas de configuración y actualizaciones |
| `/debug [descripción]` | Solucionar problemas de la sesión actual leyendo el log de depuración de la sesión |
| `/tasks` | Listar y gestionar tareas en segundo plano |
| `/todos` | Listar los elementos TODO actuales |
| `/help` | Mostrar todos los comandos slash disponibles y ayuda de uso |
| `/feedback` | Generar una URL de issue de GitHub para reportar errores o comentarios |

---

## Importar / Exportar

| Comando | Descripción |
|---------|-------------|
| `/copy` | Copiar la última respuesta del asistente al portapapeles |
| `/export [nombre-archivo]` | Exportar la conversación actual a un archivo o al portapapeles |

---

## Autenticación

| Comando | Descripción |
|---------|-------------|
| `/login` | Autenticarse con Claude Code mediante OAuth |
| `/logout` | Cerrar sesión de Claude Code |

---

## Modos de Entrada y Prefijos

Estos son prefijos especiales que puedes escribir en el prompt, no son comandos slash en sí:

| Prefijo | Descripción |
|---------|-------------|
| `/` | Activar el autocompletado de comandos o habilidades |
| `!` | Modo Bash — ejecutar comandos shell directamente y añadir la salida a la conversación |
| `@` | Mención de ruta de archivo — activar el autocompletado de rutas de archivo para contexto |

---

## Comandos Dinámicos

Estos comandos no están integrados sino que se descubren en tiempo de ejecución desde tu configuración:

### Prompts de MCP

Los servidores MCP pueden exponer prompts que aparecen como comandos:

```
/mcp__<nombre-servidor>__<nombre-prompt>
```

### Comandos de Plugin

Los plugins instalados pueden proporcionar sus propios comandos, con espacio de nombres por nombre de plugin:

```
/nombre-plugin:nombre-comando
```

### Habilidades Personalizadas

Las habilidades definidas en `.claude/skills/` aparecen como comandos invocables:

```
/nombre-habilidad
```

---

## Banderas CLI

Estas banderas se usan al lanzar Claude Code desde el terminal, no como comandos interactivos:

| Bandera | Descripción |
|---------|-------------|
| `--doctor` | Ejecutar diagnósticos desde la línea de comandos |
| `--debug` | Lanzar en modo de depuración con detalles de ejecución de hooks |
| `--resume` | Reanudar la sesión más reciente |
| `--plan` | Iniciar en modo de planificación |
| `--init` | Inicializar el repositorio con la configuración de CLAUDE.md |
| `--init-only` | Ejecutar solo la inicialización del repositorio, luego salir |
| `--maintenance` | Ejecutar operaciones de mantenimiento del repositorio |
| `--from-pr <url>` | Reanudar una sesión vinculada a un PR específico de GitHub |

---

## Atajos de Teclado

### Navegación y Control

| Atajo | Descripción |
|-------|-------------|
| `Ctrl+C` | Cancelar la entrada o generación actual |
| `Ctrl+D` | Salir de la sesión de Claude Code |
| `Ctrl+L` | Limpiar la pantalla del terminal |
| `Ctrl+R` | Búsqueda inversa en el historial de comandos |
| `Ctrl+O` | Alternar la salida detallada |
| `Esc` + `Esc` | Rebobinar o resumir |

### Cambio de Modelo y Modo

| Atajo | Descripción |
|-------|-------------|
| `Option+P` / `Alt+P` | Cambiar modelo |
| `Option+T` / `Alt+T` | Alternar el pensamiento extendido |
| `Shift+Tab` / `Alt+M` | Alternar los modos de permisos |
| `Ctrl+B` | Tareas en ejecución en segundo plano |
| `Ctrl+T` | Alternar la lista de tareas |

### Edición de Texto

| Atajo | Descripción |
|-------|-------------|
| `Ctrl+G` | Abrir el prompt en el editor de texto predeterminado |
| `Ctrl+V` / `Cmd+V` | Pegar imagen desde el portapapeles |
| `Ctrl+K` | Eliminar hasta el final de la línea |
| `Ctrl+U` | Eliminar toda la línea |
| `Ctrl+Y` | Pegar texto eliminado |
| `Alt+Y` | Ciclar el historial de pegado |

### Entrada Multilínea

| Atajo | Descripción |
|-------|-------------|
| `\` + `Enter` | Escape rápido para multilínea |
| `Option+Enter` | Multilínea predeterminado en macOS |
| `Shift+Enter` | Multilínea (iTerm2, WezTerm, Ghostty, Kitty) |
| `Ctrl+J` | Carácter de avance de línea para multilínea |

---

## Fuentes

- [Modo Interactivo de Claude Code](https://code.claude.com/docs/en/interactive-mode)
- [Referencia CLI de Claude Code](https://code.claude.com/docs/en/cli-reference)
- [Comandos Slash de Claude Code](https://code.claude.com/docs/en/slash-commands)
- [CHANGELOG de Claude Code](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Flujos de Trabajo Comunes de Claude Code](https://code.claude.com/docs/en/common-workflows)
