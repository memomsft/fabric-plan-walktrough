# 05 · Writeback — Persistir el presupuesto

> **¿Por qué necesitamos `zava-plan-db`?** La conexión a una Fabric SQL Database
> es opcional en Plan — solo se requiere para colaboración y writeback. En este
> ejercicio configuramos writeback para persistir el presupuesto capturado, por
> lo que creamos `zava-plan-db` en el paso de prerequisitos.
>
> 📖 Fuente: [Prerequisites for Plan (preview) — MicrosoftDocs/fabric-docs](https://github.com/MicrosoftDocs/fabric-docs/blob/main/docs/iq/plan/overview-prerequisites.md)

El Writeback escribe los valores del presupuesto capturado en la Planning Sheet
hacia el `zava-plan-db` SQL Database. Este paso es el puente entre la captura
del analista y la vista de gestión del controller en PowerTable.

---

## Configurar el destino de Writeback

1. En la Planning Sheet, selecciona **Writeback → Add Destination**
2. En el modal **Create Destination**:
   - **Connection:** selecciona `zava-conn-sql`
   - **Database Name:** selecciona `zava-plan-db`

   > ⚠️ Si aparece `__fabric_plan_sys` en la lista, **no la selecciones** —
   > es la base interna del sistema y no puede usarse como destino.

   - **Schema:** `dbo`
   - **Table Name:** `presupuesto_2026`
   - **Decimal Precision:** `2`
   - **Text Length:** `512`
3. Haz clic en **Add**

---

## Configurar el tipo de Writeback

1. Selecciona **Writeback → Settings**
2. En **Writeback Type** selecciona **Long**
   — almacena los valores como pares clave-valor por fila, un registro por
   combinación región/estación/categoría/mes

> **En producción:** también está disponible el formato **Wide** (columnas por
> medida) y **Long with Changes** (solo registra los valores modificados).
> El formato Long es el recomendado para integraciones con warehouses.

---

## Ejecutar el Writeback

1. Selecciona **Writeback → Writeback**
2. Espera la confirmación — aparece un mensaje de éxito
3. Abre `zava-plan-db` en una nueva pestaña para verificar que la tabla
   `presupuesto_2026` contiene los datos

> Los datos en `zava-plan-db` están disponibles para PowerTable inmediatamente
> después del Writeback. Cualquier cambio posterior en la Planning Sheet requiere
> ejecutar Writeback nuevamente para sincronizar.

> **Sobre el formato de la tabla `presupuesto_2026`:** Los datos se almacenan
> en formato **Long** — una fila por cada combinación región/estación/categoría/medida.
> Verás columnas internas de Plan como `ValueColumnName`, `IrScenario`, `Source`
> que son metadata del sistema. Esto es comportamiento esperado — la tabla no
> está diseñada para ser leída directamente sino para ser consumida por PowerTable
> e Intelligence Sheet.

---

## Siguiente paso

→ [06 · PowerTable](./06-powertable.md)
