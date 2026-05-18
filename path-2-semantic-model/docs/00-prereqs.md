# 00 · Prerequisitos

## Capacidad y licenciamiento

| Requisito | Detalle |
|---|---|
| Capacidad Fabric | F16 o superior (Fabric IQ requiere capacidad habilitada) |
| Licencia usuario | Microsoft Fabric (incluida en M365 E3/E5 o como SKU independiente) |
| Rol en workspace | Contributor o superior |
| Tenant setting | `Users can create a Planning (preview) item(s)` → **habilitado** |

> **Preview:** Plan está en Public Preview desde marzo 2026. Los meters de consumo (CUs) existen pero **no se cobran aún**; billing esperado para junio 2026.

---

## Habilitar el tenant setting

1. Ir al portal de administración de Fabric: `app.fabric.microsoft.com` → ⚙️ **Admin portal**
2. Buscar **Tenant settings** → sección **Fabric IQ settings**
3. Habilitar: `Users can create a Planning (preview) item(s)`
4. Aplicar a toda la organización o a un grupo de seguridad específico

Sin este setting, la creación del ítem Plan fallará con un error de permisos.

---

## Workspace

Crea o usa un workspace existente con capacidad Fabric asignada.

- Nombre sugerido para este ejercicio: `zava-planning` (Zava Environmental × Planeación)
- El ítem Plan creará automáticamente un **Fabric SQL Database** dentro del mismo workspace — no requiere acción adicional

---

## Siguiente paso

→ [01 · Lakehouse y carga de datos históricos](./01-lakehouse.md)
