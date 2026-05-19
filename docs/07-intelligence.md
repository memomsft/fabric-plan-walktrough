# 07 · Intelligence Sheet — Dashboard plan vs real

La Intelligence Sheet combina los datos del presupuesto capturado en la Planning
Sheet con los actuals del Semantic Model para mostrar el comparativo plan vs real
en tiempo real — sin exportar nada, sin cruzar datos manualmente.

> **Propósito en Zava:** reemplaza el reporte mensual que hoy requiere exportar
> datos del ERP, pegarlos junto al presupuesto en Excel y construir una tabla
> dinámica. Aquí dirección ve el comparativo siempre actualizado.

---

## Crear la Intelligence Sheet

1. En la pantalla de inicio del ítem Plan, selecciona **New Intelligence Sheet**
2. Nombra: `Dashboard Plan vs Real`
3. Haz clic en **Create**
4. Selecciona **Blank Canvas**

---

## Fuentes de datos disponibles

El panel **Data** muestra automáticamente dos secciones:

- **Semantic Model** → campos de `gastos_operativos`: `año`, `categoria_gasto`,
  `estacion`, `region`, `Total Real`
- **From Sheets** → medidas de la Planning Sheet: `Presupuesto 2026 Total`
  (calculado) y `Presupuesto 2026` (input manual)

> No es necesario configurar las fuentes manualmente — Plan las detecta
> automáticamente desde el ítem Plan.

---

## Agregar visuales

El panel **Visualizations** tiene las categorías:
**Charts 100+, Planning, PowerTable, Matrix, Table, Gantt, Filter, KPIs**

### KPI Cards — totales ejecutivos

1. Selecciona **KPIs** del panel Visualizations y arrástralo al canvas
2. Asigna **Value:** `Total Real` (from Semantic Model)
3. Repite para un segundo KPI con `Presupuesto 2026 Total` (from Sheets)

> Para renombrar el KPI haz doble clic sobre el título del visual y edítalo
> directamente en el canvas.

### Gráfica comparativa por región

1. Selecciona **Charts 100+** → **Bar/Column**
2. Asigna:
   - **Category:** `region` (from Semantic Model)
   - **Actual(s):** `Total Real` (from Semantic Model)
   - **Comparison 1 (vs Actuals):** `Presupuesto 2026 Total` (from Sheets)

Este visual muestra el comparativo plan vs real por región en una sola vista.

### Tabla de actuals por categoría y año

1. Selecciona **Matrix** del panel Visualizations
2. Asigna:
   - **Rows:** `region` y `categoria_gasto` (from Semantic Model)
   - **Columns:** `año` (from Semantic Model)
   - **Values (AC):** `Total Real` (from Semantic Model)

> ⚠️ La Matrix solo acepta campos de la misma fuente de datos. No es posible
> combinar medidas del Semantic Model con medidas de From Sheets en el mismo
> visual Matrix — el sistema lanza un error de dimensiones incompatibles.
> El comparativo plan vs real se muestra en el Bar/Column chart.

---

## Resultado final

El dashboard muestra en una sola vista:
- **KPI Total Real:** gasto real histórico acumulado de Zava
- **KPI Presupuesto Total 2026:** presupuesto calculado automáticamente
- **Bar chart:** comparativo plan vs real por región
- **Matrix:** detalle de actuals por categoría y año

Cuando el analista ajusta valores en la Planning Sheet y ejecuta Writeback,
el dashboard se actualiza — sin exportar, sin reconciliar, sin versiones
de archivos.

> **En producción:** Intelligence Sheet soporta más de 100 tipos de visual
> incluyendo charts IBCS para reportes financieros estándar, Gantt para
> seguimiento de proyectos, y exportación a Excel/PDF con formato preservado.

---

## Estado del workspace al terminar el ejercicio

```
zava-planning (workspace)
│
├── zava_lakehouse              ← datos históricos (actuals 2024–2025)
│   └── Tables/gastos_operativos
│
├── zava_semantic_model         ← capa semántica Direct Lake
│   └── Medida: Total Real
│
├── zava-plan-db                ← presupuestos capturados via Writeback
│   └── dbo.presupuesto_2026
│
├── __fabric_plan_sys           ← sistema interno de Plan (no modificar)
│
└── plan-2026/
    └── zava-plan-budget-2026   (ítem Plan)
        ├── Presupuesto Operativo 2026  ← Planning Sheet
        ├── Consolidado Presupuesto 2026 ← PowerTable
        └── Dashboard Plan vs Real       ← Intelligence Sheet
```
