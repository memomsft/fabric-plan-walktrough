# 04 · Planning Sheet — Captura del presupuesto 2026

La Planning Sheet es donde el analista define y captura el presupuesto operativo
de Zava para 2026 viendo los actuals 2025 como referencia. Reemplaza el Excel que
cada área llenaba y mandaba por correo.

---

## Crear la Planning Sheet

1. En la pantalla de inicio del ítem Plan, selecciona **New Planning Sheet**
2. Nombra: `Presupuesto Operativo 2026`
3. Haz clic en **Create**

---

## Conectar al Semantic Model

1. En la Planning Sheet, selecciona **Add and Connect** (barra superior o panel Data)
2. Elige la conexión `zava-conn-semantic-model`
3. Selecciona `zava_semantic_model` → **Add**

---

## Configurar el modelo de planeación

Una vez conectado, el panel **Data** muestra los campos disponibles del Semantic Model.

1. Marca los checkboxes de `region`, `estacion` y `categoria_gasto`
   — aparecen como filas con jerarquía en la hoja
2. Marca los checkboxes de `año` y `mes`
   — aparecen como dimensiones de columna
3. Marca `Total Real`
   — aparece como medida de referencia (actuals, solo lectura)

La hoja muestra la estructura jerárquica: región → categoría de gasto → estación,
con los gastos reales mensuales de 2025 como referencia.

---

## Agregar la medida de presupuesto

Para capturar el presupuesto 2026 necesitas agregar una columna de tipo calculado
que tome los actuals como base.

1. En la barra superior, selecciona **Planning → Insert Column → Input → Number**
2. En el panel **Data Input** configura:
   - **Title:** `Presupuesto 2026 Total`
   - **Insert as:** `Visual Measure`
   - **Input type:** `Number`
   - **Column aggregation type:** `Sum`
   - **Row aggregation type:** `Formula`
3. En el campo **Formula** escribe:

```
([Sum of Ene]+[Sum of Feb]+[Sum of Mar]+[Sum of Abr]+[Sum of May]+[Sum of Jun]+
[Sum of Jul]+[Sum of Ago]+[Sum of Sep]+[Sum of Oct]+[Sum of Nov]+[Sum of Dic])*1.08
```

4. Cambia el formato de display a **Millions**
5. Activa **Distribute parent value to children** ✅
6. **Allow Input:** `In both read and edit mode`
7. Haz clic en **Create**

La columna `Presupuesto 2026 Total` muestra automáticamente un presupuesto base
calculado como el total anual de actuals 2025 con un incremento del 8%.

> **Nota:** El 8% es un incremento de ejemplo. En producción el analista puede
> usar cualquier fórmula — porcentaje por categoría, drivers de negocio,
> proyecciones por estación, etc.
>
> **En producción:** Plan soporta escenarios (Base, Optimista, Pesimista),
> forecasts rolling, y simulaciones what-if que permiten modelar múltiples
> versiones del presupuesto sin duplicar datos.

---

## Siguiente paso

→ [05 · Writeback](./05-writeback.md)
