---
name: mercadopublico-directo
description: Consulta licitaciones y descarga anexos de mercadopublico.cl vía API y navegación web
user-invocable: true
allowed-tools:
  - Bash
  - TaskCreate
  - TaskUpdate
  - mcp__chrome-devtools__navigate_page
  - mcp__chrome-devtools__take_snapshot
  - mcp__chrome-devtools__fill
  - mcp__chrome-devtools__click
  - mcp__chrome-devtools__evaluate_script
  - mcp__chrome-devtools__wait_for
  - mcp__chrome-devtools__list_network_requests
  - mcp__chrome-devtools__get_network_request
---

# Skill: Mercado Público Directo

Consulta licitaciones, órdenes de compra y proveedores directamente desde `api.mercadopublico.cl`. Solo requiere un ticket de Mercado Público.

## Cuándo usar esta skill

- Buscar licitaciones activas o por fecha/organismo
- Consultar órdenes de compra por fecha o código
- Buscar información de proveedores por RUT

---

## Setup

### 1. Obtener el ticket

1. Ir a [https://www.mercadopublico.cl/](https://www.mercadopublico.cl/) → iniciar sesión
2. Ir a **Mi Cuenta** → **Datos de la API** (o similar según el tipo de cuenta)
3. Copiar el ticket (formato: UUID, ej. `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`)

### 2. Configurar el ticket

Pedir al usuario su ticket antes de ejecutar cualquier consulta. Guardarlo en una variable para usarlo en todos los curls:

```bash
TICKET="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  # <-- reemplazar con el ticket del usuario
```

Si el proyecto tiene `.env`, también puede configurarse ahí:

```bash
MERCADO_PUBLICO_TICKET=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Y leerlo con:

```bash
TICKET="${MERCADO_PUBLICO_TICKET}"
```

---

## Base URL

```
https://api.mercadopublico.cl/servicios/v1/publico/
```

Todos los endpoints requieren el parámetro `ticket` en el query string.

---

## Formato de fechas

**IMPORTANTE:** La API de Mercado Público usa el formato `ddmmyyyy` (sin separadores).

```bash
# Ejemplos
hoy: $(date +%d%m%Y)           # → 19032026
ayer: $(date -d yesterday +%d%m%Y)  # → 18032026
fecha fija: 01012026            # → 1 de enero de 2026
```

---

## Rate Limiting

- **Límite:** 30 requests/minuto
- **Recomendación:** `sleep 2` entre llamadas consecutivas
- **Timeout:** usar `--max-time 30` en curl
- Si recibes HTTP 429, esperar al menos 60 segundos antes de reintentar

---

## Endpoints: Licitaciones

### Licitaciones activas (estado 5 = publicada)

```bash
TICKET="$MERCADO_PUBLICO_TICKET"

curl -s --max-time 30 \
  "https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?estado=5&ticket=${TICKET}" \
  | python3 -c "
import json, sys
data = json.load(sys.stdin)
items = data.get('Listado', [])
print(f'Total: {len(items)}')
for item in items[:5]:
    print(f\"  {item.get('CodigoExterno','?')} | {item.get('Nombre','?')[:60]}\")
    print(f\"    Organismo: {item.get('Comprador',{}).get('NombreOrganismo','?')}\")
"
```

**Estados de licitación:**
- `5` = Publicada (activa)
- `6` = Cerrada
- `7` = Desierta
- `8` = Adjudicada
- `18` = Revocada

### Licitaciones por fecha

```bash
FECHA=$(date +%d%m%Y)  # Hoy en formato ddmmyyyy

curl -s --max-time 30 \
  "https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?fecha=${FECHA}&ticket=${TICKET}" \
  | python3 -c "
import json, sys
data = json.load(sys.stdin)
items = data.get('Listado', [])
print(f'Licitaciones del día: {len(items)}')
for item in items:
    print(f\"  {item.get('CodigoExterno','?')} | {item.get('Nombre','?')[:70]}\")
"
```

### Licitación por código

```bash
CODIGO="1234567-10-LE25"  # Código de licitación específica

curl -s --max-time 30 \
  "https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?codigo=${CODIGO}&ticket=${TICKET}" \
  | python3 -c "
import json, sys
data = json.load(sys.stdin)
item = data.get('Listado', [{}])[0]
print(json.dumps(item, ensure_ascii=False, indent=2))
"
```

### Licitaciones por organismo (RUT)

```bash
RUT_ORGANISMO="61979440-0"  # RUT del organismo comprador

curl -s --max-time 30 \
  "https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?rutUnidadCompra=${RUT_ORGANISMO}&ticket=${TICKET}" \
  | python3 -c "
import json, sys
data = json.load(sys.stdin)
items = data.get('Listado', [])
print(f'Licitaciones del organismo: {len(items)}')
for item in items[:10]:
    print(f\"  {item.get('CodigoExterno','?')} | {item.get('Nombre','?')[:60]}\")
"
```

### Búsqueda por keyword (filtrado local)

La API no tiene búsqueda de texto libre. Filtrar localmente después de obtener los resultados:

```bash
KEYWORD="software"
FECHA=$(date +%d%m%Y)

curl -s --max-time 30 \
  "https://api.mercadopublico.cl/servicios/v1/publico/licitaciones.json?fecha=${FECHA}&ticket=${TICKET}" \
  | python3 -c "
import json, sys
keyword = '${KEYWORD}'.lower()
data = json.load(sys.stdin)
items = data.get('Listado', [])
matches = [
    item for item in items
    if keyword in item.get('Nombre', '').lower()
    or keyword in item.get('Descripcion', '').lower()
]
print(f'Matches para \"{keyword}\": {len(matches)} de {len(items)} total')
for item in matches:
    print(f\"  {item.get('CodigoExterno','?')} | {item.get('Nombre','?')[:70]}\")
    print(f\"    Cierre: {item.get('FechaCierre','?')}\")
"
```

---

## Endpoints: Órdenes de Compra (OC)

### OCs por fecha

```bash
FECHA=$(date +%d%m%Y)

curl -s --max-time 30 \
  "https://api.mercadopublico.cl/servicios/v1/publico/ordenesdecompra.json?fecha=${FECHA}&ticket=${TICKET}" \
  | python3 -c "
import json, sys
data = json.load(sys.stdin)
items = data.get('Listado', [])
print(f'OCs del día: {len(items)}')
for item in items[:10]:
    print(f\"  {item.get('CodigoExterno','?')} | {item.get('Nombre','?')[:60]}\")
    print(f\"    Monto: \${item.get('TotalNeto', item.get('Total','?')):,}\")
"
```

### OC por código

```bash
CODIGO_OC="750-234-SE25"

curl -s --max-time 30 \
  "https://api.mercadopublico.cl/servicios/v1/publico/ordenesdecompra.json?codigo=${CODIGO_OC}&ticket=${TICKET}" \
  | python3 -c "
import json, sys
data = json.load(sys.stdin)
item = data.get('Listado', [{}])[0]
print(json.dumps(item, ensure_ascii=False, indent=2))
"
```

---

## Endpoints: Proveedores

### Proveedor por RUT

```bash
RUT_PROVEEDOR="76543210-9"

curl -s --max-time 30 \
  "https://api.mercadopublico.cl/servicios/v1/publico/empresas/buscarempresa.json?rutempresa=${RUT_PROVEEDOR}&ticket=${TICKET}" \
  | python3 -c "
import json, sys
data = json.load(sys.stdin)
print(json.dumps(data, ensure_ascii=False, indent=2))
"
```

---

## Estructura JSON de respuesta

### Licitaciones (`Listado[]`)

```json
{
  "Cantidad": 42,
  "FechaCreacion": "19-03-2026 10:30:00",
  "Listado": [
    {
      "CodigoExterno": "1234567-10-LE25",
      "Nombre": "Nombre de la licitación",
      "Descripcion": "Descripción detallada...",
      "FechaCreacion": "2026-03-19T10:00:00",
      "FechaCierre": "2026-03-30T15:00:00",
      "FechaPublicacion": "2026-03-19T10:00:00",
      "Estado": 5,
      "CodigoEstado": "Publicada",
      "Tipo": "LE",
      "Comprador": {
        "CodigoOrganismo": "61979440-0",
        "NombreOrganismo": "Nombre del Organismo",
        "CodigoUnidad": "12345",
        "NombreUnidad": "Unidad Compradora",
        "RutUnidad": "61979440-0",
        "DireccionUnidad": "Dirección 123",
        "ComunaUnidad": "Santiago",
        "RegionUnidad": "Región Metropolitana"
      },
      "MontoEstimado": 5000000,
      "Moneda": "CLP",
      "Items": {
        "Cantidad": 3,
        "Listado": [
          {
            "CodigoProducto": "43211507",
            "CodigoCategoria": "43",
            "Nombre": "Software",
            "Descripcion": "Descripción del ítem",
            "Cantidad": 1,
            "UnidadMedida": "unidad"
          }
        ]
      }
    }
  ]
}
```

### Órdenes de Compra (`Listado[]`)

```json
{
  "Cantidad": 15,
  "Listado": [
    {
      "CodigoExterno": "750-234-SE25",
      "Nombre": "Nombre de la OC",
      "FechaCreacion": "2026-03-19T10:00:00",
      "FechaEnvio": "2026-03-19T10:30:00",
      "Estado": 5,
      "CodigoEstado": "Aceptada",
      "TotalNeto": 1190476,
      "TotalImpuesto": 226190,
      "TotalCargo": 1416666,
      "Moneda": "CLP",
      "Proveedor": {
        "CodigoProveedor": "76543210-9",
        "NombreProveedor": "Nombre Empresa SPA",
        "RutProveedor": "76543210-9"
      },
      "Comprador": {
        "CodigoOrganismo": "61979440-0",
        "NombreOrganismo": "Nombre del Organismo"
      }
    }
  ]
}
```

---

## Script completo: resumen del día

```bash
#!/bin/bash
# resumen-mp.sh - Resumen diario de Mercado Público
# Uso: MERCADO_PUBLICO_TICKET=xxx bash resumen-mp.sh

TICKET="${MERCADO_PUBLICO_TICKET}"
FECHA=$(date +%d%m%Y)
BASE="https://api.mercadopublico.cl/servicios/v1/publico"

if [ -z "$TICKET" ]; then
  echo "Error: MERCADO_PUBLICO_TICKET no está configurado"
  exit 1
fi

echo "=== Resumen Mercado Público - $(date +%d/%m/%Y) ==="
echo ""

# Licitaciones activas
echo "--- Licitaciones publicadas hoy ---"
curl -s --max-time 30 \
  "${BASE}/licitaciones.json?fecha=${FECHA}&ticket=${TICKET}" \
  | python3 -c "
import json, sys
data = json.load(sys.stdin)
items = data.get('Listado', [])
print(f'Total: {len(items)} licitaciones')
for item in items[:5]:
    print(f\"  [{item.get('CodigoExterno','?')}] {item.get('Nombre','?')[:60]}\")
"

sleep 2

# OCs del día
echo ""
echo "--- Órdenes de Compra emitidas hoy ---"
curl -s --max-time 30 \
  "${BASE}/ordenesdecompra.json?fecha=${FECHA}&ticket=${TICKET}" \
  | python3 -c "
import json, sys
data = json.load(sys.stdin)
items = data.get('Listado', [])
total = sum(item.get('TotalCargo', 0) for item in items if isinstance(item.get('TotalCargo'), (int, float)))
print(f'Total: {len(items)} OCs | Monto total: \${total:,.0f} CLP')
for item in items[:5]:
    print(f\"  [{item.get('CodigoExterno','?')}] {item.get('Nombre','?')[:50]} | \${item.get('TotalCargo',0):,.0f}\")
"
```

---

## Navegación web: descarga de anexos

La API no expone archivos adjuntos. Para descargar anexos/adjuntos de licitaciones, usar Chrome DevTools MCP para navegar el sitio web.

### Consideraciones clave del sitio

- **ASP.NET con frames** — usar URLs directas, no clicks en menú del frame principal
- **Postbacks** — si `click` no funciona, usar `evaluate_script` con `__doPostBack()` directo
- **URL de búsqueda:** `https://www.mercadopublico.cl/BID/Modules/RFB/NEwSearchProcurement.aspx`
- **URL de ficha:** `https://www.mercadopublico.cl/Procurement/Modules/RFB/DetailsAcquisition.aspx?qs={TOKEN}`
  - El `qs` es un token **encriptado**, no el código de licitación. Se obtiene del `onclick` del botón "Ver ficha"
- **Nunca hardcodear UIDs** — siempre `take_snapshot` para descubrir la UI
- **Rate limiting** — `sleep` 2-3 segundos entre navegaciones
- **Directorio de descarga:** `./descargas-mp/{CodigoExterno}/` por cada licitación

### Fase 1: Login del usuario

```
1. navigate_page → https://www.mercadopublico.cl
2. take_snapshot → buscar "Cerrar sesión" en el snapshot
3. Si no está logueado:
   - Pedir al usuario: "Inicia sesión en mercadopublico.cl para que pueda descargar los anexos"
   - wait_for con texto ["Cerrar sesión"] timeout 120000
4. Confirmado: snapshot muestra "Hola, NOMBRE" y "Cerrar sesión"
```

**Detección de sesión:**
- Logueado: snapshot contiene `"Cerrar sesión"` + `"Hola,"`
- No logueado: snapshot contiene `"Iniciar Sesión"`

### Fase 2: Planificación

```
1. Con los CodigoExterno obtenidos de la API, crear lista de licitaciones a procesar
2. Presentar al usuario: cuántas licitaciones y qué se descargará de cada una
3. TaskCreate por cada licitación
```

### Fase 3: Por cada licitación

```
1. navigate_page → URL de búsqueda
2. fill el campo "ID de licitación" con el CodigoExterno (buscar uid con take_snapshot)
3. evaluate_script para ejecutar búsqueda:
   __doPostBack('rptAcquisition$ctl01$hlkNumAcquisition','')
4. wait_for con el CodigoExterno para confirmar que cargó
5. evaluate_script para extraer URL de la ficha:
   () => {
     const btn = document.querySelector('input[src*="verficha"]');
     return btn?.getAttribute('onclick'); // → "OpenGlobalPopup('/Procurement/...?qs=TOKEN')"
   }
6. navigate_page → URL de ficha directa (https://www.mercadopublico.cl + ruta extraída)
7. take_snapshot → encontrar sección "Antecedentes para incluir en la oferta"
8. Listar todos los botones descargar.png con sus nombres de documento
9. Por cada anexo:
   a. click en el botón descargar.png (uid del botón, obtenido del snapshot)
   b. list_network_requests o get_network_request con responseFilePath → guardar en ./descargas-mp/{CodigoExterno}/
   c. sleep 2 entre descargas
10. TaskUpdate → completada
```

### Estructura de la ficha de licitación

La sección **"4. Antecedentes para incluir en la oferta"** lista TODOS los documentos descargables:
- Documentos Administrativos (formularios, declaraciones)
- Documentos Técnicos (experiencia, equipo, propuesta)
- Documentos Económicos (oferta económica)

Cada documento tiene 2 botones:
- `bt_bajar_si.png` — subir documento (para ofertar) — **NO es este**
- `descargar.png` — **DESCARGAR el template/anexo** ← este es el que queremos

### Alternativa: botón "Ver adjuntos"

Si la sección 4 no tiene los documentos necesarios, el botón "Ver adjuntos" (`ic-21.png`) abre los archivos adjuntos subidos por el organismo (bases, resoluciones, etc.):

```
1. take_snapshot → buscar input con src que contiene "ic-21"
2. click en ese botón
3. take_snapshot → ver lista de archivos adjuntos
4. Descargar cada uno con click + get_network_request
```

### Verificar sesión activa periódicamente

Durante descargas múltiples, verificar que la sesión no expiró:
```
take_snapshot → si no aparece "Cerrar sesión", pedir re-login al usuario
wait_for ["Cerrar sesión"] timeout 120000
```

---

## Troubleshooting

### Encoding Latin-1

Algunos responses vienen en Latin-1. Si ves caracteres extraños:

```bash
curl -s --max-time 30 "URL" | iconv -f latin1 -t utf8 | python3 -c "..."
```

### Timeouts

Si el endpoint demora más de 30 segundos:
1. Reducir el rango de fechas consultado
2. Usar filtros adicionales (estado, organismo)
3. Reintentar con `--retry 2 --retry-delay 5`

### Ticket expirado o inválido

- Error típico: HTTP 401 o body `{"statusCode": 401, "message": "Unauthorized"}`
- Solución: Renovar el ticket en [mercadopublico.cl](https://www.mercadopublico.cl) → Mi Cuenta → Datos de la API

### Rate limit alcanzado

- Error típico: HTTP 429
- Solución: `sleep 60` y reintentar. Aumentar el `sleep` entre requests a 3-5 segundos.

### Sin resultados (`Cantidad: 0`)

- Verificar que el formato de fecha sea `ddmmyyyy` (no `yyyy-mm-dd`)
- Verificar que el ticket sea válido
- Algunos días (feriados, fines de semana) pueden tener pocas publicaciones
