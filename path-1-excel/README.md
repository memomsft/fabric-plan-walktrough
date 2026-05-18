# Path 1 — Plan desde Excel directo

> **El punto de partida:** Zava Environmental quiere modernizar su proceso de presupuesto
> operativo. Sus datos históricos de gasto viven en Excel y aún no tienen un Lakehouse
> ni un Semantic Model en Fabric.
>
> Este walkthrough muestra cómo un analista puede adoptar Fabric Plan **hoy**, cargando
> los actuals directamente desde Excel como referencia, y capturando el presupuesto 2026
> dentro de Plan — sin infraestructura de datos previa.

---

## Lo que vas a construir

Al final de este walkthrough tendrás:

- Un ítem **Plan** en Fabric con los datos históricos de Zava cargados como referencia
- Una **Planning Sheet** donde el analista captura el presupuesto 2026 viendo los actuals 2025
- Una **PowerTable** con vista consolidada y análisis de varianza
- Una **Intelligence Sheet** con dashboard comparativo plan vs real

---

## Prerequisitos

| Requisito | Detalle |
|---|---|
| Capacidad Fabric | F16 o superior |
| Rol en workspace | Contributor o superior |
| Tenant setting | `Users can create a Planning (preview) item(s)` → habilitado |
| Archivo de datos | `data/zava_actuals.xlsx` de este path |

> **Habilitar el tenant setting:** Admin portal → Tenant settings → Fabric IQ settings
> → `Users can create a Planning (preview) item(s)`

---

## El archivo de datos

`data/zava_actuals.xlsx` contiene los gastos operativos reales de Zava para 2024 y 2025
en dos hojas separadas. Es un archivo plano — una hoja por año, sin formato visual,
listo para ser ingerido por Plan.

| Columnas | Descripción |
|---|---|
| `region` | Norte, Centro, Sur, Bajío |
| `estacion` | 8 estaciones operativas |
| `categoria_gasto` | Recolección, Transporte, Tratamiento, Mantenimiento, Personal, Administrativo |
| `Ene` … `Dic` | Monto real mensual en MXN |

> **Formato requerido:** primera fila como encabezados, datos directamente abajo,
> sin celdas combinadas ni filas de totales. Si el archivo tiene otro formato,
> el Transform & Preview mostrará `Field1`, `Field2`... El archivo incluido ya
> cumple con el formato correcto.

---

## Paso 1 — Preparar el workspace

1. En `app.fabric.microsoft.com`, crea o usa un workspace con capacidad Fabric asignada
2. Nombre sugerido: `zava-planning`
3. Crea una carpeta: `plan-2026`

---

## Paso 2 — Crear el ítem Plan

1. Dentro de `plan-2026`, selecciona **+ New item** → **Plan (preview)**

   > Si no aparece, verifica que el tenant setting esté habilitado.

2. Nombra: `zava-plan-presupuesto-2026`
3. Haz clic en **Create**

Plan crea automáticamente un Fabric SQL Database interno (`__fabric_plan_sys`).
Verás la pantalla de inicio con las opciones **New Planning Sheet**,
**New PowerTable Sheet**, **New Intelligence Sheet**.

> ⚠️ La documentación oficial indica crear una SQL Database manualmente como
> prerequisito. En el comportamiento actual del preview, Plan la crea automáticamente
> sin solicitar selección de base de datos. Este punto puede cambiar en versiones futuras.

---

## Paso 3 — Configurar la conexión al SQL Database

Al abrir el ítem Plan aparece el banner:
*"Extra configuration recommended. Please set up a database connection to unlock
more features and allow your viewers to collaborate"*

1. Haz clic en **Set up connection**
2. Completa:
   - **Connection name:** `zava-conn-sql`
   - **Authentication kind:** `Organizational account`
3. Confirma que aparece tu cuenta bajo **You are currently signed in**
4. Haz clic en **Create**

> Este paso habilita la colaboración. No es obligatorio para crear sheets.

---

## Paso 4 — Cargar los actuals desde Excel

1. En la pantalla de inicio de Plan, selecciona **Get data from Excel/CSV**
2. Sube `zava_actuals.xlsx`
3. Selecciona la hoja `2025` como fuente de referencia
4. Revisa el esquema en el **Transform & Preview** y confirma

Los datos quedan disponibles en el panel **Data** bajo **Queries**.

> ⚠️ El flujo exacto del Transform & Preview puede variar. Valida el esquema
> antes de confirmar.

---

## Paso 5 — Crear la Planning Sheet

1. Selecciona **New Planning Sheet**
2. Nombra: `Presupuesto Operativo 2026`
3. En el panel **Data → Queries**, expande `Query 1 - 2025`
4. Marca los checkboxes de `region`, `estacion` y `categoria_gasto` — aparecen como filas con jerarquía en la hoja
5. Marca los checkboxes de `Ene` hasta `Dic` — aparecen como columnas con los actuals 2025 de referencia
6. En la barra superior ve a **Planning → Insert Column → Input → Number**
7. En el panel **Data Input** configura:
   - **Title:** `Presupuesto 2026`
   - **Insert as:** `Visual Measure`
   - **Input type:** `Number`
   - **Column aggregation type:** `Sum`
   - **Row aggregation type:** `Sum`
   - **Distribute parent value to children:** ✅
   - **Allow Input:** `In both read and edit mode`
   - Deja el resto en sus valores por defecto
8. Haz clic en **Create**

La columna **Presupuesto 2026** aparece al final de cada región — editable, con soporte
de fórmulas, lista para capturar el presupuesto 2026 viendo los actuals 2025 como
referencia al lado izquierdo.

---

## Paso 6 — Crear la PowerTable

1. Selecciona **New PowerTable Sheet**
2. Nombra: `Consolidado 2026`
3. Conecta a los datos cargados
4. Configura con `region`, `estacion`, `categoria_gasto` como dimensiones y los 12 meses
5. Agrega varianza: `([Presupuesto 2026] - [monto_real_mxn]) / [monto_real_mxn]`

> ⚠️ Sintaxis de fórmula y flujo de conexión pendientes de validación en UI.

---

## Paso 7 — Crear la Intelligence Sheet

1. Selecciona **New Intelligence Sheet**
2. Nombra: `Dashboard Plan vs Real`
3. Agrega un **Planning visual** → conecta a `Presupuesto Operativo 2026`
4. Los campos aparecen bajo **From Sheets** en el panel Data
5. Construye los visuales:
   - KPI cards: Presupuesto Total 2026 / Gasto Real 2025 / Varianza
   - Barras: Presupuesto vs Real por región
   - Línea: tendencia 2024 → 2025 → Presupuesto 2026

> ⚠️ Tipos de visual y configuración pendientes de validación en UI.

---

## Resultado

```
zava-planning (workspace)
│
├── plan-2026/
│   └── zava-plan-presupuesto-2026  (ítem Plan)
│       ├── Presupuesto Operativo 2026  ← Planning Sheet
│       ├── Consolidado 2026            ← PowerTable
│       └── Dashboard Plan vs Real      ← Intelligence Sheet
│
└── __fabric_plan_sys               (Fabric SQL Database — auto-creado por Plan)
```

---

## ¿Y después?

Una vez que Zava tenga sus datos operativos en Fabric, el paso natural es conectar
Plan directamente a un **Semantic Model** — eliminando la dependencia de subir
Excels manualmente y conectando planeación con el resto de la plataforma de datos.

Ese es el escenario del **[Path 2 — Semantic Model](../path-2-semantic-model/README.md)**.
