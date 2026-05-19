# 04 · Planning Sheet — Captura del presupuesto 2026

La Planning Sheet es donde el analista define y captura el presupuesto operativo
de Zava para 2026 viendo los actuals históricos como referencia. Reemplaza el Excel que
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

Una vez conectado, el panel **Data** muestra los campos disponibles del
Semantic Model bajo `gastos_operativos`.

### Configurar las filas

En el panel **Fields → Rows**, agrega en este orden:
1. `region`
2. `categoria_gasto`
3. `estacion`

Esto crea la jerarquía región → categoría → estación en las filas de la hoja.

### Configurar los valores

En el panel **Fields → Values**, agrega únicamente:
- `Total Real`

> ⚠️ No agregues `año`, `mes` ni `monto_real_mxn` a Values — Plan los
> trataría como medidas numéricas y los sumaría, lo cual no tiene sentido
> en este contexto.

### Resultado esperado

La hoja muestra:
- **Filas:** región → categoría de gasto → estación (con blancos donde la
  combinación no existe — esto es comportamiento correcto)
- **Values:** `Total Real` en Millones — el gasto real acumulado de Zava
  como referencia para capturar el presupuesto 2026

---

## Agregar el presupuesto base calculado

Plan calcula automáticamente el presupuesto 2026 tomando los actuals
históricos como base — el analista no empieza desde cero, tiene un punto
de partida inteligente.

1. En la barra superior, selecciona **Planning → Insert Column → Calculated → Formula**
2. En el panel **Formula Measure** configura:
   - **Title:** `Presupuesto 2026 Total`
   - **Data type:** `Number`
   - **Formula:** `[Total Real]*1.08`
   - **Row aggregation type:** `Formula`
3. Haz clic en **Create**

La columna `Presupuesto 2026 Total` se popula automáticamente para cada
región, categoría y estación — mostrando el presupuesto base como un
incremento del 8% sobre el gasto real histórico de Zava.

> **Nota:** El 8% es un incremento de ejemplo. En producción el analista
> puede usar cualquier fórmula basada en los campos disponibles: `Total Real`,
> `categoria_gasto`, `estacion`, `region`, `COLUMN` y `ROW`.

---

## Agregar columna de ajustes manuales

El analista puede sobreescribir el presupuesto base cuando el negocio lo
requiere — por ejemplo, una inversión especial en una estación específica.

1. En la barra superior, selecciona **Planning → Insert Column → Input → Number**
2. Selecciona **Insert a new empty series**
3. En el panel **Data Input** configura:
   - **Title:** `Presupuesto 2026`
   - **Input type:** `Number`
   - **Row aggregation type:** `Sum`
   - **Distribute parent value to children:** ✅
   - **Allow Input:** `In both read and edit mode`
4. Haz clic en **Create**


> **¿Cuándo usar cada columna?**
> - `Presupuesto 2026 Total` (calculado) → es el presupuesto base automático.
>   Úsalo como referencia — no es editable.
> - `Presupuesto 2026` (input manual) → úsalo **solo cuando necesitas ajustar
>   una estación específica** que difiere del base calculado. Por ejemplo, si
>   Recolección en CDMX tendrá una inversión especial en 2026, capturas ese
>   valor aquí. El resto de las celdas puede quedar vacío.

---

## Capturar ajustes de presupuesto

1. Expande la jerarquía hasta nivel estación
2. Haz clic en la celda de la estación que quieres ajustar
3. En la barra de fórmula superior escribe el valor en Millones
   (ej. `55` equivale a 55 millones MXN) y presiona **Enter**

Plan propaga automáticamente el valor hacia los niveles padre —
categoría, región y total general se actualizan en tiempo real.

> **Distribute parent value to children** está activo — cuando capturas
> un valor a nivel estación, los subtotales de categoría y región
> se recalculan automáticamente.

Captura al menos 4 valores en diferentes regiones y categorías antes
de continuar al paso de Writeback.

---

## Siguiente paso

→ [05 · Writeback](./05-writeback.md)
