# 01 · Lakehouse y carga de datos históricos

El Lakehouse almacena los **datos reales (actuals)** — los gastos operativos históricos por estación, región y categoría. Estos son los datos contra los cuales se compararán los presupuestos que se capturen en Plan.

> En producción esta capa vendría de un proceso de ingesta desde los sistemas fuente (ERP, SAP, etc.). En este ejercicio cargamos el CSV directamente.

---

## Crear el Lakehouse

1. En tu workspace `zava-planning`, selecciona **+ New item**
2. Busca y selecciona **Lakehouse**
3. Nombra: `zava_lakehouse`
4. Haz clic en **Create**

---

## Cargar el archivo de datos

1. Descarga el archivo [`zava_actuals.xlsx`](../data/zava_actuals.xlsx) de este repositorio
2. En el Lakehouse, selecciona la sección **Files** (panel izquierdo)
3. Haz clic en **Upload** → **Upload files**
4. Sube `zava_actuals.xlsx`

---

## Crear la tabla Delta a partir del CSV

Una vez subido el archivo, conviértelo en una tabla Delta que el Semantic Model pueda consumir.

1. Haz clic derecho sobre `zava_actuals.xlsx` en la sección Files
2. Selecciona **Load to Tables** → **New table**
3. Nombra la tabla: `gastos_operativos`
4. Confirma el esquema — debe detectar automáticamente:

| Columna | Tipo |
|---|---|
| año | int64 |
| mes | int64 |
| region | string |
| estacion | string |
| categoria_gasto | string |
| monto_real_mxn | int64 |

5. Haz clic en **Load** y espera a que termine

Verifica que la tabla aparezca bajo **Tables → gastos_operativos** en el Lakehouse Explorer.

---

## Explorar los datos (opcional)

Puedes hacer una validación rápida desde la SQL analytics endpoint del Lakehouse:

```sql
SELECT
    año,
    region,
    categoria_gasto,
    SUM(monto_real_mxn) AS total_gasto
FROM gastos_operativos
GROUP BY año, region, categoria_gasto
ORDER BY año, region, categoria_gasto;
```

Esperado: 4 regiones × 6 categorías × 2 años = 48 filas de agregado.

---

## Siguiente paso

→ [02 · Semantic Model (capa analítica)](./02-semantic-model.md)
