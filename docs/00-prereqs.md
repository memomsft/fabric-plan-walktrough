# 00 · Prerequisitos

Antes de iniciar el ejercicio, asegúrate de tener lo siguiente en orden.

---

## Capacidad y licenciamiento

| Requisito | Detalle |
|---|---|
| Capacidad Fabric | F16 o superior |
| Licencia de usuario | Microsoft Fabric (incluida en M365 E3/E5 o SKU independiente) |
| Rol en workspace | Contributor o superior |
| Tenant setting | `Users can create a Planning (preview) item(s)` → habilitado |

### Habilitar el tenant setting

1. Ve a `app.fabric.microsoft.com` → ⚙️ **Admin portal**
2. Selecciona **Tenant settings** → sección **Fabric IQ settings**
3. Habilita: `Users can create a Planning (preview) item(s)`
4. Aplica a toda la organización o a un grupo de seguridad específico

> Sin este setting, la creación del ítem Plan fallará.

---

## Workspace

Crea un workspace con capacidad Fabric asignada. Nombre sugerido: `zava-planning`

---

## Fabric SQL Database

Plan requiere una Fabric SQL Database como destino de Writeback — donde se
almacenarán los presupuestos capturados. Esta base **debe crearse manualmente**
antes de crear el ítem Plan.

1. En el workspace `zava-planning`, selecciona **+ New item** → **SQL database**
2. Nombra: `zava-plan-db`
3. Haz clic en **Create** y espera a que termine

> **¿Por qué dos bases de datos?** Al crear el ítem Plan, Fabric crea
> automáticamente `__fabric_plan_sys` — una base de sistema interna para metadata
> de Plan que **no puede usarse** como destino de writeback ni debe modificarse.
> La `zava-plan-db` es la base de negocio donde viven los presupuestos capturados.

---

## Conexiones

Plan necesita dos conexiones configuradas en Settings para acceder al SQL Database
y al Semantic Model.

### Conexión al SQL Database

1. Ve a ⚙️ **Settings** → **Manage connections and gateways** → **+ New** → **Cloud**
2. Completa:
   - **Connection name:** `zava-conn-sql`
   - **Connection type:** `SQL database in Fabric`
   - **Authentication method:** `OAuth 2.0`
3. Selecciona **Edit credentials** → inicia sesión con tu cuenta Microsoft
4. Haz clic en **Create**

### Conexión al Semantic Model

1. En la misma pantalla, selecciona **+ New** → **Cloud**
2. Completa:
   - **Connection name:** `zava-conn-semantic-model`
   - **Connection type:** `Power BI Semantic Model`
   - **Authentication method:** `OAuth 2.0`
3. Selecciona **Edit credentials** → inicia sesión
4. Haz clic en **Create**

---

## Siguiente paso

→ [01 · Lakehouse y carga de datos históricos](./01-lakehouse.md)
