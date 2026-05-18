# 01 · Lakehouse y carga de datos históricos

El Lakehouse es la capa de almacenamiento donde viven los datos históricos de Zava.
En este ejercicio cargamos el dataset sintético directamente — en producción estos
datos llegarían desde los sistemas fuente vía pipelines o Mirroring.

---

## Crear el Lakehouse

1. En el workspace `zava-planning`, selecciona **+ New item** → **Lakehouse**
2. Nombra: `zava_lakehouse`
3. Haz clic en **Create**

---

## Cargar el archivo de datos

1. Descarga `data/zava_actuals.csv` de este repositorio
2. En el Lakehouse, selecciona la sección **Files** (panel izquierdo)
3. Haz clic en **Upload** → **Upload files**
4. Sube `zava_actuals.csv`

> **Formato soportado:** Load to Tables del Lakehouse soporta únicamente archivos
> **CSV y Parquet** — no Excel. El archivo incluido ya está en el formato correcto.
>
> **En producción:** los datos no se cargarían manualmente. Llegarían al Lakehouse
> desde los sistemas fuente de Zava (ERP, sistemas operacionales) vía:
> - **Mirroring** — replicación continua desde Azure SQL, Snowflake, Databricks, SAP
> - **Shortcuts** — acceso directo a datos en ADLS, S3 u otros storage sin mover datos
> - **Pipelines de Data Factory** — ingesta programada con transformaciones

---

## Crear la tabla Delta

Una vez subido el archivo, conviértelo en una tabla Delta que el Semantic Model
pueda consumir.

1. Haz clic derecho sobre `zava_actuals.csv` en la sección Files
2. Selecciona **Load to Tables** → **New table**
3. Nombra la tabla: `gastos_operativos`
4. Confirma el esquema detectado automáticamente:

| Columna | Tipo | Descripción |
|---|---|---|
| `año` | int64 | Año del registro (2024 o 2025) |
| `mes` | int64 | Mes del registro (1–12) |
| `region` | string | Región operativa |
| `estacion` | string | Estación dentro de la región |
| `categoria_gasto` | string | Categoría del gasto operativo |
| `monto_real_mxn` | int64 | Monto real en pesos mexicanos |

5. Haz clic en **Load** y espera a que termine

Verifica que `gastos_operativos` aparezca bajo **Tables** en el Lakehouse Explorer.

---

## Validar los datos (opcional)

Desde el SQL analytics endpoint del Lakehouse:

```sql
SELECT
    año,
    region,
    COUNT(*) AS registros,
    SUM(monto_real_mxn) AS total_gasto_mxn
FROM gastos_operativos
GROUP BY año, region
ORDER BY año, region;
```

Resultado esperado: 4 regiones × 2 años = 8 filas.

---

## Siguiente paso

→ [02 · Semantic Model](./02-semantic-model.md)
