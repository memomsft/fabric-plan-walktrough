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
- **Columnas:** estaciones expandidas por región
- **Values:** `Total Real` en Millones — el gasto real acumulado de Zava
  como referencia para capturar el presupuesto 2026

---

## Agregar la medida de presupuesto

Plan puede calcular automáticamente el presupuesto 2026 tomando los actuals
históricos como base. En lugar de que el analista capture valores manualmente,
Plan aplica la lógica de negocio directamente sobre los datos del Semantic Model.

1. En la barra superior, selecciona **Planning → Insert Column → Calculated → Formula**
2. En el panel **Formula Measure** configura:
   - **Title:** `Presupuesto 2026 Total`
   - **Data type:** `Number`
   - **Formula:** `[Total Real]*1.08`
   - **Row aggregation type:** `Formula`
3. Haz clic en **Create**

La columna `Presupuesto 2026 Total` se popula automáticamente para cada
región, categoría y estación — mostrando el presupuesto base calculado como
un incremento del 8% sobre el gasto real histórico de Zava.

> **Nota:** El 8% es un incremento de ejemplo. En producción el analista
> puede usar cualquier fórmula basada en los campos disponibles: `Total Real`,
> `categoria_gasto`, `estacion`, `region`, `COLUMN` y `ROW` — permitiendo
> lógica diferenciada por categoría, región o estación.
>
> **En producción:** Plan también soporta columnas de tipo **Input** para
> captura manual cuando el analista necesita sobreescribir el valor calculado
> para una fila específica — combinando así el presupuesto base automático
> con ajustes discrecionales del negocio.

---

## Siguiente paso

→ [05 · Writeback](./05-writeback.md)
