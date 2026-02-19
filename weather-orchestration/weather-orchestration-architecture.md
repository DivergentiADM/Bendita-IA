# Flujo del Sistema Climático de Karachi

Este documento describe el flujo completo del sistema de obtención y transformación de datos climáticos.

<table width="100%">
<tr>
<td><a href="../">← Volver a las Mejores Prácticas de Claude Code</a></td>
<td align="right"><img src="../!/claude-jumping.svg" alt="Claude" width="60" /></td>
</tr>
</table>

## Descripción General del Sistema

El sistema climático demuestra el patrón de arquitectura **Comando → Agente → Habilidades**, donde:
- Un comando orquesta el flujo de trabajo
- Un agente ejecuta tareas usando habilidades precargadas
- Las habilidades proporcionan conocimiento e instrucciones específicas del dominio

## Diagrama de Flujo

```
┌─────────────────────────────────────────────────────────────────┐
│                        Interacción del Usuario                   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
                    ┌──────────────────────┐
                    │  /weather-orchestrator│
                    │  Comando              │
                    │  (Punto de entrada)   │
                    └──────────────────────┘
                              │
                              │ Invocación de la herramienta Task
                              ▼
                    ┌──────────────────────┐
                    │  weather             │
                    │  Agente              │
                    │  (Orquesta el flujo) │
                    │                      │
                    │  habilidades:        │
                    │  - weather-fetcher   │
                    │  - weather-transformer│
                    └──────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              │                               │
              ▼                               ▼
┌─────────────────────────┐     ┌─────────────────────────┐
│  weather-fetcher        │     │  weather-transformer    │
│  Habilidad              │     │  Habilidad              │
│  (Conocimiento precargado)│   │  (Conocimiento precargado)│
└─────────────────────────┘     └─────────────────────────┘
              │                               │
              ▼                               ▼
┌─────────────────────────┐     ┌─────────────────────────┐
│  API wttr.in            │     │  weather-orchestration/ │
│  Obtener Temperatura    │     │  Leer Reglas de         │
│  para Karachi           │     │  Transformación         │
└─────────────────────────┘     └─────────────────────────┘
              │                               │
              │ Devuelve: 26°C               ▼
              │                     ┌─────────────────────────┐
              │                     │  Aplicar Transformación │
              └─────────────────────│  26 + 10 = 36°C         │
                                    └─────────────────────────┘
                                              │
                                              ▼
                                    ┌─────────────────────────┐
                                    │  weather-orchestration/output.md       │
                                    │  Escribir Resultados    │
                                    └─────────────────────────┘
                                              │
                                              ▼
                                    ┌─────────────────────────┐
                                    │  Mostrar Resumen        │
                                    │  al Usuario             │
                                    └─────────────────────────┘
```

## Detalles de los Componentes

### 1. Comando

#### `/weather-orchestrator` (Comando)
- **Ubicación**: `.claude/commands/weather-orchestrator.md`
- **Propósito**: Punto de entrada para las operaciones climáticas
- **Acción**: Invoca el agente del clima mediante la herramienta Task
- **Modelo**: haiku

### 2. Agente con Habilidades

#### `weather` (Agente)
- **Ubicación**: `.claude/agents/weather.md`
- **Propósito**: Ejecutar el flujo de trabajo climático usando habilidades precargadas
- **Habilidades**: `weather-fetcher`, `weather-transformer`
- **Herramientas Disponibles**: WebFetch, Read, Write
- **Modelo**: haiku
- **Color**: verde

El agente tiene las habilidades precargadas en su contexto al inicio. Sigue las instrucciones de cada habilidad secuencialmente.

### 3. Habilidades

#### `weather-fetcher` (Habilidad)
- **Ubicación**: `.claude/skills/weather-fetcher/SKILL.md`
- **Propósito**: Instrucciones para obtener datos de temperatura en tiempo real
- **Fuente de Datos**: API wttr.in para Karachi, Pakistán
- **Salida**: Temperatura en Celsius (valor numérico)

#### `weather-transformer` (Habilidad)
- **Ubicación**: `.claude/skills/weather-transformer/SKILL.md`
- **Propósito**: Instrucciones para aplicar transformaciones matemáticas
- **Fuente de Entrada**: `weather-orchestration/input.md` (reglas de transformación)
- **Destino de Salida**: `weather-orchestration/output.md` (resultados formateados)

### 4. Archivos de Datos

#### `weather-orchestration/input.md`
- **Propósito**: Almacena las reglas de transformación
- **Formato**: Instrucciones en lenguaje natural (por ejemplo, "suma +10 al resultado")
- **Acceso**: Leído por el agente del clima siguiendo la habilidad weather-transformer

#### `weather-orchestration/output.md`
- **Propósito**: Almacena los resultados formateados de la transformación
- **Formato**: Markdown estructurado con secciones:
  - Temperatura Original
  - Transformación Aplicada
  - Resultado Final
  - Detalles del Cálculo

## Flujo de Ejecución

1. **Invocación del Usuario**: El usuario ejecuta el comando `/weather-orchestrator`
2. **Prompt del Usuario**: El comando pregunta al usuario la unidad de temperatura preferida (Celsius/Fahrenheit)
3. **Invocación del Agente**: El comando invoca al agente del clima mediante la herramienta Task
4. **Ejecución de Habilidades** (dentro del contexto del agente):
   - **Paso 1**: El agente sigue las instrucciones de la habilidad `weather-fetcher` para obtener la temperatura de wttr.in
   - **Paso 2**: El agente sigue las instrucciones de la habilidad `weather-transformer` para:
     - Leer las reglas de transformación desde `weather-orchestration/input.md`
     - Aplicar las reglas a la temperatura obtenida
     - Escribir los resultados formateados en `weather-orchestration/output.md`
5. **Visualización del Resultado**: Se muestra un resumen al usuario con:
   - Unidad de temperatura solicitada
   - Temperatura original
   - Regla de transformación aplicada
   - Resultado final transformado

## Ejemplo de Ejecución

```
Entrada: /weather-orchestrator
├─ Pregunta: ¿Celsius o Fahrenheit?
├─ Usuario: Celsius
├─ Tarea: agente del clima (mediante la herramienta Task)
│  ├─ Habilidades Precargadas:
│  │  ├─ weather-fetcher (conocimiento)
│  │  └─ weather-transformer (conocimiento)
│  ├─ Paso 1 (habilidad weather-fetcher):
│  │  └─ Obtiene de wttr.in → 26°C
│  ├─ Paso 2 (habilidad weather-transformer):
│  │  ├─ Lee: weather-orchestration/input.md ("suma +10")
│  │  ├─ Calcula: 26 + 10 = 36°C
│  │  └─ Escribe: weather-orchestration/output.md
│  └─ Devuelve: Informe completo
└─ Salida:
   ├─ Unidad: Celsius
   ├─ Original: 26°C
   ├─ Transformación: Suma +10
   └─ Resultado: 36°C
```

## Principios de Diseño Clave

1. **Comando → Agente → Habilidades**: Arquitectura de tres niveles para una separación limpia
2. **Habilidades como Conocimiento**: Las habilidades proporcionan conocimiento del dominio precargado en el contexto del agente
3. **Agente Único**: Un agente maneja múltiples tareas relacionadas usando sus habilidades
4. **Ejecución Secuencial**: El agente sigue las instrucciones de las habilidades en orden
5. **Transformaciones Configurables**: Las reglas se almacenan externamente en archivos de entrada
6. **Salida Estructurada**: Los resultados se formatean de manera consistente en los archivos de salida

## Patrón de Arquitectura: Agente-Habilidades

Este sistema demuestra el **patrón agente-habilidades** donde:

```yaml
# En la definición del agente (.claude/agents/weather.md)
---
name: weather
skills:
  - weather-fetcher
  - weather-transformer
---
```

- **Las habilidades están precargadas**: El contenido completo de la habilidad se inyecta en el contexto del agente al inicio
- **El agente usa el conocimiento de las habilidades**: El agente sigue las instrucciones de las habilidades precargadas
- **Sin invocación dinámica**: Las habilidades no se invocan por separado; son material de referencia
- **Contexto de ejecución único**: Todo el trabajo ocurre dentro del contexto de un agente
