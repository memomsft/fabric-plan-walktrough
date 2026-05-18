# 05 · PowerTable — vista consolidada

La **PowerTable Sheet** permite ver todos los presupuestos consolidados en una vista tabular estructurada, con cálculos de varianza, drill-down por jerarquía y comparación período a período. Es la vista que usaría un director financiero o de operaciones para revisar el presupuesto total.

---

## Crear la PowerTable Sheet

1. Dentro del ítem Plan, selecciona **+ New sheet** → **PowerTable sheet**
2. Nombra: `Consolidado Presupuesto 2026`
3. Haz clic en **Create**

---

## Configurar la vista

1. Conecta al mismo Semantic Model `zava_semantic_model`
2. Configura las dimensiones:

   | Área | Campo |
   |---|---|
   | Rows | `region` → `estacion` (jerarquía) |
   | Columns | `Tiempo` → trimestres y meses |
   | Values | `Total Real` (2025) · `Presupuesto 2026` |

3. Agrega la columna calculada **Varianza %**:
   - Haz clic en **+ Add column** → **Calculated**
   - Fórmula: `([Presupuesto 2026] - [Total Real]) / [Total Real]`
   - Formato: **Porcentaje** · Colores condicionales (verde si < 10%, ámbar 10–20%, rojo > 20%)

---

## Análisis de varianza

La columna de varianza muestra inmediatamente qué regiones o categorías tienen el mayor incremento de presupuesto respecto al gasto real del año anterior. Esto es lo que actualmente Zava Environmental haría manualmente en Excel — consolidar todas las hojas y calcular las varianzas.

Con PowerTable:
- Un clic en cualquier región expande a nivel estación
- Un clic en estación expande a nivel categoría de gasto
- Los subtotales se calculan automáticamente

---

## Habilitar edición directa (opcional)

PowerTable también permite hacer ajustes directos desde la vista consolidada:

1. En la barra superior, activa **Edit mode**
2. Haz clic en cualquier celda de `Presupuesto 2026`
3. Modifica el valor — el cambio se propaga inmediatamente al Fabric SQL Database y se refleja en la Planning Sheet del responsable correspondiente

---

## Siguiente paso

→ [06 · Intelligence Sheet — dashboard plan vs real](./06-intelligence.md)
