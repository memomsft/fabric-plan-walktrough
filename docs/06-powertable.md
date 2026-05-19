# 06 · PowerTable — Gestión y gobernanza del presupuesto

La PowerTable Sheet conecta el `zava-plan-db` a una aplicación tabular gobernada
donde el controller o CFO puede revisar los presupuestos capturados, hacer ajustes
con trazabilidad completa, y aprobar el plan final.

> **Propósito en Zava:** reemplaza el proceso de "mandame el Excel para revisarlo
> y te digo qué cambiar". Aquí el controller ve los datos en tiempo real, ajusta
> directamente, y cada cambio queda registrado con quién lo hizo y cuándo.

---

## Crear la PowerTable Sheet

1. En la pantalla de inicio del ítem Plan, selecciona **New PowerTable Sheet**
2. Nombra: `Consolidado Presupuesto 2026`
3. Haz clic en **Create**
4. Selecciona **Create a New App**

---

## Conectar al SQL Database

1. Selecciona la conexión `zava-conn-sql` → **Connect**
2. Selecciona la base de datos `zava-plan-db` → **Add**

---

## Conectar a la tabla del presupuesto

1. En **Select Table**, selecciona **Existing Table**
2. Selecciona el schema `dbo`
3. Selecciona la tabla `presupuesto_2026`
4. Haz clic en **Next**

---

## Configurar las columnas

PowerTable detecta automáticamente el esquema de la tabla. Revisa la configuración:

1. Verifica que `IrId` tenga marcado **Primary Key** e **Identity Column**
2. En la columna `LastUpdatedAt`, selecciona **Time Zone Adjustment: User Local**
3. Deja **SCD (Slowly Changing Dimensions)** desactivado
4. Haz clic en **Finish**

---

## Explorar las capacidades de gobernanza

Con la PowerTable configurada, tienes acceso a:

**Audit trail:**
- Selecciona **PowerTable → Audit**
- Cada cambio muestra: Row ID, tipo de acción, columna modificada, valor anterior,
  valor nuevo, usuario y timestamp

**Edición directa:**
- Haz doble clic en cualquier celda para editarla
- Selecciona **Preview Changes** para revisar antes de guardar
- Selecciona **Save to Database** para persistir los cambios

> **En producción:** PowerTable soporta flujos de aprobación multi-etapa
> (Captura → Revisión → Aprobado), notificaciones por email y Teams, y
> permisos a nivel de fila y columna para controlar quién puede ver o editar qué.

---

> **Flujo controller → analista:** Los ajustes que el controller hace en PowerTable
> se persisten en `zava-plan-db`. Plan soporta **Connected Planning** — la capacidad
> de vincular tablas de PowerTable como inputs de una Planning Sheet — lo que
> permitiría que los ajustes del controller se reflejen automáticamente en la vista
> del analista. Esta configuración avanzada está fuera del alcance de este ejercicio.
>
> 📖 Fuente: [PowerTable — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/powertable-how-to-create-table-app)

## Siguiente paso

→ [07 · Intelligence Sheet](./07-intelligence.md)
