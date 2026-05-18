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

## Configurar las fuentes de datos

En el panel **Data** (lado derecho del canvas) tienes tres secciones:

- **Semantic Model** → conecta a `zava_semantic_model` para los actuals
- **From Sheets** → consume datos de la Planning Sheet (presupuesto calculado)
- **Queries** → permite cargar archivos adicionales si fuera necesario

### Conectar el Semantic Model

1. Bajo **Semantic Model**, haz clic en **+ Add**
2. Selecciona la conexión `zava-conn-semantic-model`
3. Selecciona `zava_semantic_model` → **Add**

Las dimensiones (`region`, `estacion`, `categoria_gasto`, `año`, `mes`) y la
medida `Total Real` quedan disponibles para los visuales.

### Activar From Sheets

Expande **From Sheets** — aparece `Presupuesto Operativo 2026` con la medida
`Presupuesto 2026 Total` lista para usar.

---

## Agregar visuales

### Abrir el panel de Visualizaciones

Haz clic en el ícono de **Visualizations** en el lado derecho del canvas para
abrir el panel de tipos de visual disponibles.

### KPI Cards — totales ejecutivos

1. Selecciona el visual **KPI** del panel de Visualizaciones
2. Arrástralo al canvas
3. Asigna:
   - **Value:** `Total Real` (from Semantic Model) — actuals 2025
4. Repite para crear un segundo KPI con `Presupuesto 2026 Total` (from Sheets)

### Gráfica comparativa por región

1. Selecciona **Charts 100+** del panel de Visualizaciones
2. Elige un **Bar chart** o **Column chart**
3. Asigna:
   - **Rows/Axis:** `region` (from Semantic Model)
   - **Values:** `Total Real` y `Presupuesto 2026 Total`

### Tabla de detalle por categoría

1. Selecciona el visual **Matrix**
2. Asigna:
   - **Rows:** `region` → `categoria_gasto`
   - **Columns:** `año`
   - **Values:** `Total Real` y `Presupuesto 2026 Total`

---

## Resultado final

El dashboard muestra en una sola vista:
- El gasto real histórico de Zava (actuals del Semantic Model)
- El presupuesto 2026 calculado (desde la Planning Sheet)
- La comparativa por región, estación y categoría

Cuando el analista ajusta valores en la Planning Sheet y ejecuta Writeback, el
dashboard se actualiza automáticamente — sin exportar, sin reconciliar, sin
versiones de archivos.

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
