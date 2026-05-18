# Path 2 — Plan sobre Semantic Model

> **El contexto:** Zava Environmental ya lleva varios meses usando Microsoft Fabric. Sus datos operativos — rutas, estaciones, gastos — viven en un Lakehouse. El equipo de BI tiene reportes en Power BI conectados a un Semantic Model. Hay gobernanza, hay un proceso de ingesta.
>
> El problema sigue siendo el presupuesto: ese proceso todavía vive fuera de Fabric, en Excels que no hablan con los datos que ya están en la plataforma. Cuando finanzas quiere comparar presupuesto contra gasto real, tiene que exportar datos del reporte de Power BI, pegarlos en Excel, y hacer el cálculo a mano.
>
> Este walkthrough muestra cómo conectar Plan directamente al Semantic Model existente — para que el presupuesto y los actuals vivan en el mismo ambiente, con las mismas definiciones, sin duplicación ni reconciliación manual.

---

## Lo que vas a construir

Al final de este walkthrough, Zava tendrá:

- Los datos históricos de gasto (2024–2025) en un **Lakehouse** como tabla Delta
- Un **Semantic Model** en Direct Lake sobre esa tabla — la misma fuente que Power BI
- Un ítem **Plan** conectado a ese modelo, donde los actuals se actualizan solos cada vez que el Lakehouse recibe datos nuevos
- Una **Planning Sheet** donde el equipo captura el presupuesto 2026 viendo los actuals en tiempo real, con las mismas métricas que el reporte de Power BI
- Una **Intelligence Sheet** que compara plan vs real sin salir de Fabric

La diferencia con el Path 1: los datos no se suben manualmente. Viven en OneLake, gobernados, y Plan los consume igual que cualquier otro ítem de Fabric.

---

## Prerequisitos

| Requisito | Detalle |
|---|---|
| Capacidad Fabric | F16 o superior |
| Rol en workspace | Contributor o superior |
| Tenant setting | `Users can create a Planning (preview) item(s)` → habilitado en Admin portal |
| Archivo de datos | `data/zava_actuals.xlsx` de este path |

> **Habilitar el tenant setting:** Admin portal → Tenant settings → Fabric IQ settings → `Users can create a Planning (preview) item(s)` → habilitar para la organización o grupo de seguridad.

---

## Arquitectura del ejercicio

```
                         OneLake
┌────────────────────────────────────────────────────────┐
│                                                        │
│  ┌─────────────────┐    ┌──────────────────────┐       │
│  │  zava_lakehouse  │───▶│  zava_semantic_model  │      │
│  │                 │    │                      │       │
│  │  gastos_        │    │  Total Real (DAX)    │       │
│  │  operativos     │    │  Direct Lake         │       │
│  │  (Delta table)  │    │                      │       │
│  └─────────────────┘    └──────────┬───────────┘       │
│                                   │                   │
│                        ┌──────────▼───────────┐        │
│                        │  zava-plan-budget-    │        │
│                        │  2026 (Plan item)     │        │
│                        │                      │        │
│                        │  Planning Sheet       │        │
│                        │  PowerTable           │        │
│                        │  Intelligence Sheet   │        │
│                        └──────────┬───────────┘        │
│                                   │                   │
│                        ┌──────────▼───────────┐        │
│                        │   zava-plan-db        │        │
│                        │  (Fabric SQL DB)      │        │
│                        │  presupuestos 2026    │        │
│                        └──────────────────────┘        │
└────────────────────────────────────────────────────────┘
```

---

## Dataset

El archivo `data/zava_actuals.xlsx` contiene los gastos operativos reales de Zava para 2024–2025.

| Dimensión | Valores |
|---|---|
| Regiones | Norte, Centro, Sur, Bajío |
| Estaciones | Monterrey, Saltillo, CDMX, Toluca, Oaxaca, Veracruz, Guadalajara, Querétaro |
| Categorías | Recolección, Transporte, Tratamiento, Mantenimiento, Personal, Administrativo |
| Períodos | Enero–Diciembre 2024 y 2025 (1,152 registros) |

Incluye estacionalidad (pico en Jul–Sep) y ~10% de crecimiento interanual — representativo de una operación de gestión de residuos en crecimiento.

---

## Walkthrough paso a paso

| Paso | Descripción |
|---|---|
| [00 · Prerequisitos](./docs/00-prereqs.md) | Capacidad, licencias, tenant setting |
| [01 · Lakehouse](./docs/01-lakehouse.md) | Crear Lakehouse y cargar zava_actuals.xlsx como tabla Delta |
| [02 · Semantic Model](./docs/02-semantic-model.md) | Capa semántica Direct Lake + medida DAX |
| [03 · Ítem Plan](./docs/03-plan-item.md) | Crear SQL DB, conexiones y el ítem Plan |
| [04 · Planning Sheet](./docs/04-planning-sheet.md) | Captura de presupuesto 2026 conectada al Semantic Model |
| [05 · PowerTable](./docs/05-powertable.md) | Vista consolidada con análisis de varianza |
| [06 · Intelligence Sheet](./docs/06-intelligence.md) | Dashboard Plan vs Real en tiempo real |

---

## Estado de validación de flujos

| Paso | Fuente | Estado |
|---|---|---|
| Tenant setting | [Microsoft Learn — Overview](https://learn.microsoft.com/en-us/fabric/iq/plan/overview) | ✅ Validado |
| Fabric SQL DB + conexiones | [Microsoft Learn — Get started](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-how-to-get-started) | ✅ Validado |
| Crear ítem Plan | [Microsoft Learn — Get started](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-how-to-get-started) | ✅ Validado |
| Planning Sheet — Add and Connect | [Microsoft Learn — Get started](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-how-to-get-started) | ✅ Validado |
| PowerTable — Create app + write-back | [Microsoft Learn — PowerTable](https://learn.microsoft.com/en-us/fabric/iq/plan/powertable-how-to-create-table-app) | ✅ Validado |
| Intelligence Sheet — Planning visual | [Microsoft Learn — Visualize](https://learn.microsoft.com/en-us/fabric/iq/plan/intelligence-how-to-visualize-planning-data) | ✅ Validado |
| Data Input measure, escenarios, aprobación | Documentación de overview | ⚠️ Pendiente validación en UI |

> **Preview:** Fabric Plan evoluciona con frecuencia. Si algún paso difiere en la UI, la [documentación oficial](https://learn.microsoft.com/en-us/fabric/iq/plan/overview) es la fuente de verdad.
