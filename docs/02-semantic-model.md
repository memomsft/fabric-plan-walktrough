# 02 · Semantic Model

El Semantic Model es la capa de negocio que se construye sobre la tabla Delta del
Lakehouse. Es la fuente de verdad que Plan usa para leer los actuals — las mismas
definiciones, medidas y jerarquías que usaría Power BI.

> **Por qué el Semantic Model es clave:** Plan está diseñado para conectarse al
> Semantic Model, no directamente al Lakehouse. Esto garantiza que el presupuesto
> que captura el analista compare contra los mismos números que ve el equipo de BI
> en sus reportes — sin reconciliación manual.

---

## Crear el Semantic Model en Direct Lake

1. Desde el Lakehouse `zava_lakehouse`, haz clic en **New semantic model**
2. Nombra: `zava_semantic_model`
3. Selecciona la tabla `gastos_operativos` y haz clic en **Confirm**

Fabric crea el modelo en modo **Direct Lake** — los datos se leen directamente
desde OneLake sin duplicar ni mover nada.

> **En producción:** el Semantic Model puede incluir múltiples tablas con
> relaciones, medidas DAX complejas, Row-Level Security (RLS) por región o
> centro de costo, y jerarquías de tiempo, producto u organización. Aquí usamos
> el mínimo necesario para el ejercicio.

---

## Agregar la medida DAX

Con el modelo abierto en el editor:

1. Selecciona la tabla `gastos_operativos`
2. Haz clic en **New measure**
3. Escribe:

```dax
Total Real = SUM(gastos_operativos[monto_real_mxn])
```

4. Guarda: **File → Save**

Esta medida es la que Plan usará como referencia de actuals en la Planning Sheet.

---

## Configurar la conexión Direct Lake para Plan

> 📖 Fuente: [Create a semantic model connection — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-how-to-create-semantic-model-connection#connect-to-a-direct-lake-semantic-model)

Plan no soporta SSO para modelos Direct Lake. Debes crear una conexión con
fixed credentials directamente desde el Semantic Model.

> ⚠️ Si omites este paso e intentas conectar el Semantic Model desde Plan,
> recibirás un error de conexión.

1. En el workspace, selecciona **...** junto a `zava_semantic_model`
   → **Settings** → **Gateway & Cloud Connections**
2. La conexión por defecto aparece como **Single Sign On** — no la uses
3. Selecciona **Create a connection** desde la lista de conexiones
4. Completa:
   - **Connection name:** `zava-conn-semantic-model`
   - **Authentication method:** `OAuth 2.0`
5. Haz clic en **Create**
6. Selecciona la nueva conexión de la lista y haz clic en **Apply**

---

## Validar el modelo (opcional)

1. Desde el Semantic Model, selecciona **Explore this data** → **Auto-create report**
2. Arrastra `Total Real` al canvas y filtra por `region`
3. Verifica que los datos de 2024–2025 se muestren correctamente

---

## Siguiente paso

→ [03 · Ítem Plan](./03-plan-item.md)
