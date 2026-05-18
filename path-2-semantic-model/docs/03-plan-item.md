# 03 · Crear el ítem Plan

El ítem **Plan** es el contenedor principal del ejercicio. Requiere dos conexiones previas: una al **Fabric SQL Database** (donde se guardan los presupuestos capturados) y otra al **Semantic Model** (donde están los actuals).

---

## Paso 1 — Crear el Fabric SQL Database

Cuando se crea un ítem Plan, Fabric crea automáticamente un Fabric SQL Database en el workspace. Sin embargo, **necesitas tener una conexión configurada** antes de poder usarlo.

1. En el workspace `zava-planning`, selecciona **+ New item** → **SQL database**
2. Nombra: `zava-plan-db`
3. Haz clic en **Create** y espera a que termine

---

## Paso 2 — Crear las conexiones

Las conexiones son credenciales reutilizables que Plan usa para acceder al SQL Database y al Semantic Model.

### Conexión al SQL Database

1. Ve a ⚙️ **Settings** → **Manage connections and gateways**
2. Selecciona **+ New** → **Cloud**
3. Completa:
   - **Connection name:** `conn-zava-sql`
   - **Connection type:** `SQL database in Fabric`
   - **Authentication method:** `OAuth 2.0`
4. Selecciona **Edit credentials** → inicia sesión con tu cuenta Microsoft
5. Haz clic en **Create**

### Conexión al Semantic Model

1. En la misma pantalla, selecciona **+ New** → **Cloud**
2. Completa:
   - **Connection name:** `conn-zava-semantic-model`
   - **Connection type:** `Power BI Semantic Model`
   - **Authentication method:** `OAuth 2.0`
3. Selecciona **Edit credentials** → inicia sesión
4. Haz clic en **Create**

---

## Paso 3 — Crear el ítem Plan

1. En el workspace, selecciona **+ New item** → busca **Plan (preview)**

   > Si no aparece, verifica que el tenant setting del paso [00-prereqs](./00-prereqs.md) esté habilitado.

2. Nombra: `zava-plan-budget-2026`
3. Crea una carpeta nueva para mantener el workspace ordenado y coloca el ítem ahí
4. Haz clic en **Create**

Al abrir el ítem, Plan pedirá que selecciones una conexión:

5. Selecciona `conn-zava-sql` → **Connect**
6. Selecciona la base de datos `zava-plan-db` → **Add**

Verás la pantalla de inicio con las opciones: **Planning**, **PowerTable** e **Intelligence**.

---

## Qué existe en el workspace ahora

| Ítem | Tipo | Propósito |
|---|---|---|
| `zava_lakehouse` | Lakehouse | Datos históricos (actuals) |
| `zava_semantic_model` | Semantic Model | Capa semántica compartida |
| `zava-plan-db` | Fabric SQL Database | Almacén de writeback para presupuestos |
| `zava-plan-budget-2026` | Plan | Contenedor principal del ejercicio |

---

## Siguiente paso

→ [04 · Planning Sheet — captura de presupuesto](./04-planning-sheet.md)
