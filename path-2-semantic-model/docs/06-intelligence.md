# 06 · Intelligence Sheet — dashboard plan vs real

La **Intelligence Sheet** es el dashboard analítico embebido dentro de Plan. Permite comparar visualmente el presupuesto capturado contra los gastos reales históricos, con drill-down interactivo y seguimiento de KPIs — sin necesidad de abrir Power BI por separado.

---

## Crear la Intelligence Sheet

1. Dentro del ítem Plan, selecciona **+ New sheet** → **Intelligence sheet**
2. Nombra: `Dashboard Plan vs Real`
3. Haz clic en **Create**

---

## Importar datos desde la Planning Sheet

El punto de entrada de la Intelligence Sheet es el **Planning visual** — no se conecta directamente al Semantic Model, sino que consume los datos desde la Planning Sheet ya configurada.

1. En la Intelligence Sheet, selecciona **+ Add visual** → **Planning visual**
2. En el panel de configuración, elige `Presupuesto Operativo 2026` (la Planning Sheet del paso anterior)
3. Confirma — los datos se importan al lienzo

Una vez importados, el panel **Data** mostrará dos secciones:
- **From Sheets** → medidas de la Planning Sheet (`Total Real`, `Presupuesto 2026`)
- **From Model** → dimensiones del Semantic Model (`region`, `estacion`, `categoria_gasto`, `Tiempo`)

---

## Agregar visualizaciones

Con los campos disponibles en el panel Data, agrega los siguientes visuales:

### KPI cards

Agrega 3 cards usando medidas de **From Sheets**:
- `SUM(Presupuesto 2026)` → "Presupuesto Total 2026"
- `SUM(Total Real)` filtrado año=2025 → "Gasto Real 2025"
- Diferencia entre ambos → "Varianza"

### Barras comparativas por región

- **+ Add visual** → **Bar chart**
- Eje: `region` (From Model)
- Valores: `Total Real` y `Presupuesto 2026` (From Sheets)

### Línea de tendencia mensual

- **+ Add visual** → **Line chart**
- Eje: `mes` (From Model → jerarquía Tiempo)
- Valores: `Total Real 2024`, `Total Real 2025`, `Presupuesto 2026`
- Muestra la tendencia histórica y cómo el presupuesto nuevo se posiciona respecto a ella

---

## Comportamiento en tiempo real

Cuando alguien modifica un valor en la **Planning Sheet**, los gráficos de la Intelligence Sheet **se actualizan en tiempo real** — sin necesidad de refrescar manualmente.

---

## Resultado final

Al completar este walkthrough, el workspace `zava-planning` contiene:

```
zava-planning (workspace)
├── zava_lakehouse              ← datos históricos (actuals)
├── zava_semantic_model         ← capa semántica compartida
├── zava-plan-budget-2026  ← ítem Plan
│   ├── Presupuesto Operativo 2026  (Planning Sheet)
│   ├── Consolidado Presupuesto 2026 (PowerTable Sheet)
│   └── Dashboard Plan vs Real      (Intelligence Sheet)
└── zava-plan-budget-2026 (DB)  ← Fabric SQL Database (auto-creado)
```

Los presupuestos capturados en la Planning Sheet se almacenan en el SQL Database y son inmediatamente visibles en el PowerTable y el Intelligence Sheet — sin exportaciones, sin correos con archivos adjuntos, sin consolidaciones manuales.
