# 04 · Planning Sheet — captura de presupuesto

La **Planning Sheet** es donde los responsables de cada región o área capturan los montos de presupuesto para 2026. Es la pieza central que reemplaza las hojas de Excel distribuidas.

---

## Crear la Planning Sheet

1. Dentro del ítem Plan, selecciona **+ New sheet** → **Planning sheet**
2. Nombra: `Presupuesto Operativo 2026`
3. Haz clic en **Create**

---

## Conectar al Semantic Model

1. En la Planning Sheet, selecciona **Add and Connect** (panel derecho o barra superior)
2. Elige la conexión `conn-zava-semantic-model`
3. Selecciona `zava_semantic_model` → **Add**

Una vez conectado, el panel **Data** mostrará las tablas y medidas del modelo.

---

## Agregar campos al modelo de planeación

Desde el panel **Data**, arrastra al área de **Fields** de la Planning Sheet:

- `region`, `estacion`, `categoria_gasto` → dimensiones de filas
- `Tiempo` (jerarquía año→mes) → dimensión de columnas/períodos
- `Total Real` → medida de referencia (actuals, solo lectura)

---

## Agregar campo de captura (input del usuario)

1. En la barra de la Planning Sheet, selecciona **+ Add measure** (o el ícono de nueva medida)
2. Elige tipo: **Data Input**
3. Nombra la medida: `Presupuesto 2026`
4. Tipo de dato: **Number** · Formato sugerido: moneda

La hoja ahora muestra, para cada combinación región/estación/categoría/mes:
- `Total Real` (dato histórico, de referencia — no editable)
- `Presupuesto 2026` (celda editable — el usuario captura aquí)

---

## Configurar un escenario de presupuesto

Para demostrar la capacidad de escenarios:

1. En la barra superior, selecciona **Model** → **Scenarios**
2. Crea dos escenarios:
   - `Base` — presupuesto conservador (+5% sobre 2025)
   - `Optimista` — presupuesto con inversión en región Sur (+12%)
3. Guarda

Los usuarios pueden alternar entre escenarios y ver cómo cambian los totales en tiempo real.

---

## Flujo de aprobación (opcional)

Plan soporta flujos de aprobación nativos:

1. Selecciona **Workflow** en la barra superior
2. Define etapas: `Captura` → `Revisión Gerencia` → `Aprobado`
3. Asigna aprobadores por etapa
4. Activa el flujo

Cuando un responsable termina de capturar, marca su hoja como lista y el sistema notifica al aprobador — sin correos ni versiones de Excel.

---

## Siguiente paso

→ [05 · PowerTable — vista consolidada](./05-powertable.md)
