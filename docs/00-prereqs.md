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

## Conexiones a Fabric SQL Database (opcional)

> 📖 Fuente: [Prerequisites for Plan (preview) — MicrosoftDocs/fabric-docs](https://github.com/MicrosoftDocs/fabric-docs/blob/main/docs/iq/plan/overview-prerequisites.md)

Al crear el ítem Plan, Fabric crea automáticamente un Fabric SQL Database
interno (`__fabric_plan_sys`) que almacena la metadata del reporte de Plan.
Esta base es de sistema — no la modifiques ni la uses para datos de negocio.

Las conexiones a una Fabric SQL Database adicional son **opcionales** y solo
se requieren si necesitas alguno de estos escenarios:

| Escenario | ¿Necesitas una SQL Database adicional? |
|---|---|
| Solo capturar y analizar presupuesto (un usuario) | ❌ No |
| Colaboración — múltiples usuarios editando el plan | ✅ Sí |
| Writeback — persistir el presupuesto capturado | ✅ Sí |

En este ejercicio usamos Writeback para persistir el presupuesto de Zava,
por lo que necesitamos crear `zava-plan-db` manualmente.

### Crear la Fabric SQL Database (para Writeback)

1. En el workspace `zava-planning`, selecciona **+ New item** → **SQL database**
2. Nombra: `zava-plan-db`
3. Haz clic en **Create**

### Crear la conexión al SQL Database

1. Ve a ⚙️ **Settings** → **Manage connections and gateways** → **+ New** → **Cloud**
2. Completa:
   - **Connection name:** `zava-conn-sql`
   - **Connection type:** `SQL database in Fabric`
   - **Authentication method:** `OAuth 2.0`
3. Selecciona **Edit credentials** → inicia sesión con tu cuenta Microsoft
4. Haz clic en **Create**

---

## Conexión al Semantic Model (Direct Lake)

> 📖 Fuente: [Create a semantic model connection — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-how-to-create-semantic-model-connection#connect-to-a-direct-lake-semantic-model)

Plan requiere una conexión al Semantic Model para leer los actuals. Esta
conexión es **obligatoria**.

> ⚠️ **Direct Lake — configuración especial requerida:**
> Los Semantic Models en modo Direct Lake usan SSO por defecto — y Plan
> **no soporta SSO para Direct Lake**. Debes crear la conexión directamente
> desde el Semantic Model con fixed credentials, no desde Manage connections
> and gateways.

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

## Siguiente paso

→ [01 · Lakehouse y carga de datos históricos](./01-lakehouse.md)
