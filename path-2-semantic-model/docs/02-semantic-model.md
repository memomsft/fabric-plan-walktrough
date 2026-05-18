# 02 · Semantic Model

El Semantic Model es la capa que Plan usa para leer los **datos reales (actuals)**. Plan se conecta directamente al Semantic Model — no al Lakehouse — porque es la forma en que Fabric garantiza una semántica de negocio consistente entre planeación y reporteo.

> Piénsalo como el "diccionario de negocio" compartido entre Power BI y Plan.

---

## Crear el Semantic Model en Direct Lake

1. Desde el Lakehouse `zava_lakehouse`, haz clic en el botón **New semantic model** (barra superior)
2. Nombra: `zava_semantic_model`
3. Selecciona la tabla `gastos_operativos` y haz clic en **Confirm**

Fabric crea el modelo en modo **Direct Lake** — los datos se leen directamente desde OneLake sin mover ni duplicar nada.

---

## Definir la jerarquía temporal

Una vez creado el modelo, ábrelo en el editor de Semantic Model:

1. Selecciona la tabla `gastos_operativos`
2. Haz clic en **New measure** y agrega:

```dax
Total Real = SUM(gastos_operativos[monto_real_mxn])
```

3. Crea una jerarquía de tiempo:
   - Haz clic derecho en la columna `año` → **Create hierarchy**
   - Nombra la jerarquía: `Tiempo`
   - Agrega `mes` como segundo nivel

4. Guarda el modelo: **File → Save**

---

## Validar en Power BI (opcional)

Para confirmar que el modelo funciona antes de conectarlo a Plan:

1. Desde el Semantic Model, selecciona **Explore this data** → **Auto-create report**
2. Arrastra `Total Real` al canvas y filtra por `region`
3. Verifica que los datos de 2024–2025 se muestren correctamente

---

## Siguiente paso

→ [03 · Crear el ítem Plan](./03-plan-item.md)
