# Flujo de Trabajo RPI

**RPI** = **I**nvestigación (**R**esearch) → **P**lanificación (**P**lan) → **I**mplementación (**I**mplement)

Un flujo de trabajo de desarrollo sistemático con puertas de validación en cada fase. Previene el esfuerzo desperdiciado en características no viables y garantiza una documentación completa.

<table width="100%">
<tr>
<td><a href="../../">← Volver a las Mejores Prácticas de Claude Code</a></td>
<td align="right"><img src="../../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

---

## Descripción General

![Flujo de Trabajo RPI](rpi-workflow.svg)

---

## Instalación

Copia la carpeta `.claude` (que contiene `agents/` y `commands/rpi/`) a la raíz de tu repositorio, luego crea el directorio `rpi/plans`.

---

## Ejemplo de Flujo de Trabajo

### Característica: Autenticación de Usuario

**Paso 1: Describir**
```
Usuario: "Añadir autenticación OAuth2 con proveedores de Google y GitHub"

1. Claude genera el plan
   → Salida: rpi/plans/oauth2-authentication.md
2. Crear carpeta de característica: rpi/oauth2-authentication/
3. Copiar el plan en la carpeta de la característica
4. Renombrar el plan a REQUEST.md
   → Final: rpi/oauth2-authentication/REQUEST.md
```

**Paso 2: Investigar**
```bash
/rpi:research rpi/oauth2-authentication/REQUEST.md
```
Salida:
- `research/RESEARCH.md` con el análisis
- Veredicto: **GO** (factible, alineado con la estrategia)

**Paso 3: Planificar**
```bash
/rpi:plan oauth2-authentication
```
Salida:
- `plan/pm.md` - Historias de usuario y criterios de aceptación
- `plan/ux.md` - Flujos de la interfaz de inicio de sesión
- `plan/eng.md` - Arquitectura técnica
- `plan/PLAN.md` - 3 fases, 15 tareas

**Paso 4: Implementar**
```bash
/rpi:implement oauth2-authentication
```
Progreso:
- Fase 1: Fundamentos del Backend → APROBADO
- Fase 2: Integración del Frontend → APROBADO
- Fase 3: Pruebas y Pulido → APROBADO

Resultado: Característica completa, lista para PR.

---

## Estructura de la Carpeta de Características

Todo el trabajo de características vive en `rpi/{feature-slug}/`:

```
rpi/{feature-slug}/
├── REQUEST.md              # Paso 1: Descripción inicial de la característica
├── research/
│   └── RESEARCH.md         # Paso 2: Análisis GO/NO-GO
├── plan/
│   ├── PLAN.md             # Paso 3: Hoja de ruta de implementación
│   ├── pm.md               # Requisitos del producto
│   ├── ux.md               # Diseño UX
│   └── eng.md              # Especificación técnica
└── implement/
    └── IMPLEMENT.md        # Paso 4: Registro de implementación
```

---

## Agentes y Comandos

| Comando | Agentes Utilizados |
|---------|-------------------|
| `/rpi:research` | requirement-parser, product-manager, Explore, senior-software-engineer, technical-cto-advisor, documentation-analyst-writer |
| `/rpi:plan` | senior-software-engineer, product-manager, ux-designer, documentation-analyst-writer |
| `/rpi:implement` | Explore, senior-software-engineer, code-reviewer |
