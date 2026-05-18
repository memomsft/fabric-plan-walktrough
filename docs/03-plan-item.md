# 03 · Ítem Plan

El ítem Plan es el contenedor principal que agrupa las tres sheets del ejercicio:
Planning, PowerTable e Intelligence. Al crearlo se conecta al SQL Database que
creaste como prerequisito y al Semantic Model recién configurado.

---

## Crear el ítem Plan

1. En el workspace `zava-planning`, crea una carpeta: `plan-2026`
2. Dentro de la carpeta, selecciona **+ New item** → **Plan (preview)**

   > Si no aparece, verifica que el tenant setting del paso
   > [00-prereqs](./00-prereqs.md) esté habilitado.

3. Nombra: `zava-plan-budget-2026`
4. Haz clic en **Create**

Al abrir el ítem, Plan muestra el banner:
*"Extra configuration recommended. Please set up a database connection to unlock
more features and allow your viewers to collaborate"*

5. Haz clic en **Set up connection** — se abre el modal **Create New Connection**
6. Completa:
   - **Connection name:** `zava-conn-sql`
   - **Authentication kind:** `Organizational account`
7. Confirma que aparece tu cuenta bajo **You are currently signed in**
8. Haz clic en **Create**

> **Nota:** Al crear el ítem Plan, Fabric crea automáticamente `__fabric_plan_sys`
> — una base de datos interna del sistema que almacena la metadata de Plan.
> Esta base **no es** la `zava-plan-db` que creaste como prerequisito, y **no
> puede usarse** como destino de Writeback. Verás ambas en el workspace — esto
> es comportamiento esperado.

Verás la pantalla de inicio con las opciones:
**New Planning Sheet | New PowerTable Sheet | New Intelligence Sheet**

---

## Qué existe en el workspace ahora

| Ítem | Tipo | Propósito |
|---|---|---|
| `zava_lakehouse` | Lakehouse | Datos históricos (actuals) |
| `zava_semantic_model` | Semantic Model | Capa semántica — fuente de actuals para Plan |
| `zava-plan-db` | Fabric SQL Database | Destino del Writeback — presupuestos capturados |
| `__fabric_plan_sys` | Fabric SQL Database | Sistema interno de Plan — no modificar |
| `zava-plan-budget-2026` | Plan | Contenedor principal del ejercicio |

---

## Siguiente paso

→ [04 · Planning Sheet](./04-planning-sheet.md)
