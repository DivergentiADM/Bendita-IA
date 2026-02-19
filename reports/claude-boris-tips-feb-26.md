# 12 Formas de Personalizar Claude Code — Consejos de Boris Cherny

Un resumen de los consejos de personalización compartidos por Boris Cherny ([@bcherny](https://x.com/bcherny)), creador de Claude Code, el 12 de febrero de 2026.

<table width="100%">
<tr>
<td><a href="../">← Volver a las Mejores Prácticas de Claude Code</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Contexto

Boris Cherny destacó que la personalización es una de las cosas que más aman los ingenieros de Claude Code — hooks, plugins, LSPs, MCPs, habilidades, esfuerzo, agentes personalizados, líneas de estado, estilos de salida y más. Compartió 12 formas prácticas en que los desarrolladores y equipos están personalizando sus configuraciones.

<img src="assets/boris/0.webp" alt="Tweet introductorio de Boris Cherny" width="50%" />

---

## 1/ Configura Tu Terminal

Configura tu terminal para la mejor experiencia con Claude Code:

- **Tema**: Ejecuta `/config` para establecer modo claro/oscuro
- **Notificaciones**: Activa las notificaciones para iTerm2 o usa un hook de notificación personalizado
- **Saltos de línea**: Si usas Claude Code en un terminal de IDE, Apple Terminal, Warp o Alacritty, ejecuta `/terminal-setup` para habilitar shift+enter para saltos de línea (para no tener que escribir `\`)
- **Modo Vim**: Ejecuta `/vim`

<img src="assets/boris/1.webp" alt="Configura tu terminal" width="50%" />

---

## 2/ Ajusta el Nivel de Esfuerzo

Ejecuta `/model` para elegir tu nivel de esfuerzo preferido:

- **Bajo** — menos tokens, respuestas más rápidas
- **Medio** — comportamiento equilibrado
- **Alto** — más tokens, más inteligencia

Preferencia de Boris: Alto para todo.

<img src="assets/boris/2.webp" alt="Ajusta el nivel de esfuerzo" width="50%" />

---

## 3/ Instala Plugins, MCPs y Habilidades

Los plugins te permiten instalar LSPs (disponibles para todos los lenguajes principales), MCPs, habilidades, agentes y hooks personalizados.

Instala desde el mercado oficial de plugins de Anthropic, o crea tu propio mercado para tu empresa. Registra el `settings.json` en tu base de código para añadir automáticamente los mercados para tu equipo.

Ejecuta `/plugin` para comenzar.

<img src="assets/boris/3.webp" alt="Instala Plugins, MCPs y Habilidades" width="50%" />

---

## 4/ Crea Agentes Personalizados

Coloca archivos `.md` en `.claude/agents` para crear agentes personalizados. Cada agente puede tener un nombre personalizado, color, conjunto de herramientas, herramientas pre-permitidas y pre-rechazadas, modo de permisos y modelo.

También puedes establecer el agente predeterminado para la conversación principal usando el campo `"agent"` en `settings.json` o la bandera `--agent`.

Ejecuta `/agents` para comenzar.

<img src="assets/boris/4.webp" alt="Crea agentes personalizados" width="50%" />

---

## 5/ Pre-aprueba Permisos Comunes

Claude Code usa un sistema de permisos que combina detección de inyección de prompts, análisis estático, sandboxing y supervisión humana.

De fábrica, un pequeño conjunto de comandos seguros están pre-aprobados. Para pre-aprobar más, ejecuta `/permissions` y añade a las listas de permitidos y bloqueados. Regístralos en el `settings.json` de tu equipo.

Se admite sintaxis completa de comodines — por ejemplo, `Bash(bun run *)` o `Edit(/docs/**)`.

<img src="assets/boris/5.webp" alt="Pre-aprueba permisos comunes" width="50%" />

---

## 6/ Activa el Sandboxing

Opta por el entorno de ejecución sandbox de código abierto de Claude Code para mejorar la seguridad mientras reduces los avisos de permisos.

Ejecuta `/sandbox` para activarlo. El sandboxing se ejecuta en tu máquina y admite tanto el aislamiento de archivos como de red.

<img src="assets/boris/6.webp" alt="Activa el sandboxing" width="50%" />

---

## 7/ Añade una Línea de Estado

Las líneas de estado personalizadas aparecen justo debajo del compositor, mostrando el modelo, directorio, contexto restante, costo y cualquier otra cosa que quieras ver mientras trabajas.

Cada miembro del equipo puede tener una línea de estado diferente. Usa `/statusline` para que Claude genere una basada en tu `.bashrc`/`.zshrc`.

<img src="assets/boris/7.webp" alt="Añade una línea de estado" width="50%" />

---

## 8/ Personaliza Tus Atajos de Teclado

Cada atajo de teclado en Claude Code es personalizable. Ejecuta `/keybindings` para reasignar cualquier tecla. La configuración se recarga en vivo para que puedas ver cómo se siente inmediatamente.

<img src="assets/boris/8.webp" alt="Personaliza tus atajos de teclado" width="50%" />

---

## 9/ Configura Hooks

Los hooks te permiten engancharte de manera determinista al ciclo de vida de Claude:

- Enrutar automáticamente las solicitudes de permisos a Slack o Opus
- Indicar a Claude que siga cuando llega al final de un turno (incluso puedes iniciar un agente o usar un prompt para decidir si Claude debe seguir)
- Pre-procesar o post-procesar llamadas a herramientas, por ejemplo, para añadir tu propio registro

Pídele a Claude que añada un hook para comenzar.

<img src="assets/boris/9.webp" alt="Configura hooks" width="50%" />

---

## 10/ Personaliza Tus Verbos del Spinner

Personaliza tus verbos del spinner para añadir o reemplazar la lista predeterminada con tus propios verbos. Registra el `settings.json` en el control de versiones para compartir los verbos con tu equipo.

<img src="assets/boris/10.webp" alt="Personaliza tus verbos del spinner" width="50%" />

---

## 11/ Usa Estilos de Salida

Ejecuta `/config` y establece un estilo de salida para que Claude responda usando un tono o formato diferente.

- **Explicativo** — recomendado cuando te familiarizas con una nueva base de código, para que Claude explique los frameworks y patrones de código mientras trabaja
- **Aprendizaje** — para que Claude te guíe mientras realizas cambios de código
- **Personalizado** — crea estilos de salida personalizados para ajustar la voz de Claude

<img src="assets/boris/11.webp" alt="Usa estilos de salida" width="50%" />

---

## 12/ ¡Personaliza Todo!

Claude Code funciona muy bien de fábrica, pero cuando personalizas, registra tu `settings.json` en git para que tu equipo también se beneficie. La configuración se admite en múltiples niveles:

- Para tu base de código
- Para una subcarpeta
- Solo para ti
- Mediante políticas a nivel empresarial

Con 37 configuraciones y 84 variables de entorno (usa el campo `"env"` en tu `settings.json` para evitar scripts envoltorio), es muy probable que cualquier comportamiento que desees sea configurable.

<img src="assets/boris/12.webp" alt="Personaliza todo" width="50%" />

---

## Fuentes

- [Boris Cherny (@bcherny) en X — 12 de febrero de 2026](https://x.com/bcherny)
- [Documentación de Configuración de Terminal de Claude Code](https://code.claude.com/docs/en/terminal)
- [Documentación de Plugins y Descubrimiento de Claude Code](https://code.claude.com/docs/en/discover-plugins)
- [Documentación de Sub-agentes de Claude Code](https://code.claude.com/docs/en/sub-agents)
- [Documentación de Permisos de Claude Code](https://code.claude.com/docs/en/permissions)
- [Documentación de Sandbox de Claude Code](https://code.claude.com/docs/en/sandbox)
- [Documentación de Línea de Estado de Claude Code](https://code.claude.com/docs/en/statusline)
- [Documentación de Atajos de Teclado de Claude Code](https://code.claude.com/docs/en/keybindings)
- [Referencia de Hooks de Claude Code](https://code.claude.com/docs/en/hooks)
- [Documentación de Estilos de Salida de Claude Code](https://code.claude.com/docs/en/output-styles)
- [Documentación de Configuración de Claude Code](https://code.claude.com/docs/en/settings)
