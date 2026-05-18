# Microsoft Fabric Plan — Zava Environmental

> ⚠️ **Preview:** Fabric Plan está en Public Preview desde marzo 2026. Los meters de
> consumo (CUs) existen pero no se cobran aún. Billing estimado para junio 2026.

---

## El problema: planeación desconectada de los datos

Zava Environmental opera estaciones de gestión de residuos en cuatro regiones de
México. Cada año, el proceso de presupuesto operativo sigue el mismo patrón:

```
Finanzas manda plantilla Excel
        │
        ├──▶ Región Norte  →  norte_2026_v1.xlsx  →  correo
        ├──▶ Región Centro →  centro_FINAL.xlsx   →  correo
        ├──▶ Región Sur    →  sur_ok_este.xlsx    →  correo
        └──▶ Región Bajío  →  bajio_2026.xlsx     →  correo
                                    │
                        Consolidación manual
                        (días de trabajo, errores)
                                    │
                        presupuesto_2026_DEFINITIVO_v8.xlsx
                        (¿es el correcto? ¿quién lo cambió?)
```

El resultado: datos desactualizados, sin trazabilidad, y cuando finanzas quiere
comparar presupuesto vs gasto real tiene que cruzar dos sistemas distintos a mano.

---

## La solución: Microsoft Fabric Plan

**Fabric Plan** es una solución EPM (Enterprise Performance Management) construida
nativamente dentro de Microsoft Fabric. Permite a las organizaciones crear, gestionar
y analizar planes — como presupuestos, forecasts y escenarios — dentro de la misma
plataforma gobernada donde viven sus datos históricos y su analítica.

Para Zava, esto significa: **un solo lugar donde el analista captura el presupuesto
viendo los actuals reales como referencia, finanzas lo revisa con audit trail completo,
y dirección analiza el comparativo plan vs real en tiempo real — sin exportar nada,
sin consolidar manualmente, sin versiones de archivos.**

---

## Los tres componentes de Plan

### 📋 Planning Sheet — Captura de presupuesto

La Planning Sheet es el espacio de trabajo colaborativo donde el analista define y
captura el presupuesto operativo directamente sobre los datos del Semantic Model.

**Para Zava:** reemplaza el Excel individual que cada área llena y manda por correo.
El analista ve los gastos reales de 2025 como referencia y captura el presupuesto
2026 en el mismo ambiente — sin distribuir plantillas, sin recolectar archivos.

**Lo que puede hacer:**
- Capturar valores de presupuesto por región, estación y categoría de gasto
- Crear medidas calculadas (ej. presupuesto base = actuals × 1.08)
- Definir escenarios y hacer simulaciones what-if
- Colaborar con comentarios y @menciones por celda
- Hacer Writeback al SQL Database para persistir los datos

> 📖 Fuente: [Planning sheets — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-overview)

---

### 📊 PowerTable Sheet — Gestión y gobernanza de datos

La PowerTable Sheet es una aplicación de gestión de datos tabular construida sobre
el Fabric SQL Database. Proporciona una experiencia similar a Excel pero con
gobernanza empresarial — cada cambio se registra y puede requerir aprobación.

**Para Zava:** es la vista que usaría el controller o CFO para revisar los
presupuestos capturados, hacer ajustes directos con trazabilidad completa, y
aprobar el plan final antes de que sea oficial.

**Lo que puede hacer:**
- Ver y editar los datos del presupuesto en una tabla gobernada
- Audit trail completo: quién cambió qué celda, cuándo y de qué valor a cuál
- Flujos de aprobación: definir etapas de revisión y aprobadores
- Sincronización bidireccional con el Fabric SQL Database

> ⚠️ **Importante:** PowerTable requiere el `zava-plan-db` (SQL Database creado
> manualmente) como destino. **No** puede usar `__fabric_plan_sys` (base interna
> de Plan) como destino de datos de negocio.

> 📖 Fuente: [PowerTable sheets — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/powertable-overview)

---

### 📈 Intelligence Sheet — Dashboard plan vs real

La Intelligence Sheet es el componente de reporting y visualización integrado en
Plan. Se conecta en tiempo real a la Planning Sheet y al Semantic Model para
mostrar el comparativo plan vs actuals en el mismo ambiente donde se planea.

**Para Zava:** elimina el reporte manual que hoy requiere exportar datos del ERP,
cruzarlos con el presupuesto en Excel y construir una tabla dinámica. Aquí el
dashboard siempre está actualizado.

**Lo que puede hacer:**
- Más de 100 tipos de visual: charts, KPI cards, matrices, tablas
- Conectarse simultáneamente a la Planning Sheet (presupuesto) y al Semantic
  Model (actuals) para mostrar plan vs real en una sola vista
- Exportar a Excel, PDF y PNG con formato preservado
- Comentarios y anotaciones a nivel de celda o punto de datos

> 📖 Fuente: [Intelligence sheets — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/intelligence-overview)

---

## Las dos bases de datos del workspace

Al trabajar con Plan aparecen dos Fabric SQL Databases en el workspace — es
importante entender el rol de cada una:

| Base de datos | Creada por | Propósito | ¿Se puede usar para writeback? |
|---|---|---|---|
| `__fabric_plan_sys` | Plan automáticamente | Metadata interna del sistema — configuración, estado interno de Plan | ❌ No — es una base de aplicación reservada |
| `zava-plan-db` | El usuario (prerequisito) | Destino del Writeback — almacena los presupuestos capturados en la Planning Sheet | ✅ Sí |

> `__fabric_plan_sys` se crea sola al crear el ítem Plan. No la modifiques ni
> la uses como destino de datos.

---

## Arquitectura del ejercicio

```
                    DATOS HISTÓRICOS
                         │
              zava_actuals.csv (dataset sintético)
                         │
                         ▼
              Lakehouse: zava_lakehouse
              tabla Delta: gastos_operativos
                         │
                    Direct Lake
                         │
                         ▼
              Semantic Model: zava_semantic_model
              medida DAX: [Total Real]
                         │
                    ┌────┴────────────────────┐
                    │                         │
                    ▼                         ▼
           PLAN: zava-plan-budget-2026    zava-plan-db
                    │                   (Fabric SQL DB)
         ┌──────────┼──────────┐              ▲
         │          │          │              │
         ▼          ▼          ▼         Writeback
    Planning    PowerTable  Intelligence
     Sheet       Sheet       Sheet
   (captura)  (gobernanza)  (dashboard)
```

---

## Flujo del ejercicio

| Paso | Descripción |
|---|---|
| [00 · Prerequisitos](./docs/00-prereqs.md) | Workspace, SQL Database, conexiones |
| [01 · Lakehouse](./docs/01-lakehouse.md) | Cargar CSV y crear tabla Delta |
| [02 · Semantic Model](./docs/02-semantic-model.md) | Crear modelo Direct Lake con medida DAX |
| [03 · Ítem Plan](./docs/03-plan-item.md) | Crear Plan conectado a SQL DB y Semantic Model |
| [04 · Planning Sheet](./docs/04-planning-sheet.md) | Configurar vista y capturar presupuesto 2026 |
| [05 · Writeback](./docs/05-writeback.md) | Persistir el presupuesto en zava-plan-db |
| [06 · PowerTable](./docs/06-powertable.md) | Vista de gestión con audit trail |
| [07 · Intelligence Sheet](./docs/07-intelligence.md) | Dashboard plan vs real |

---

## Dataset

`data/zava_actuals.csv` es un dataset sintético que simula los gastos operativos
reales de Zava para 2024 y 2025.

| Columna | Descripción |
|---|---|
| `año` | 2024 o 2025 |
| `mes` | 1–12 |
| `region` | Norte, Centro, Sur, Bajío |
| `estacion` | 8 estaciones operativas |
| `categoria_gasto` | Recolección, Transporte, Tratamiento, Mantenimiento, Personal, Administrativo |
| `monto_real_mxn` | Gasto real mensual en MXN |

**Total:** 1,152 registros (2 años × 12 meses × 8 estaciones × 6 categorías)

> **Nota:** En producción, estos datos llegarían al Lakehouse desde los sistemas
> fuente de Zava (ERP, sistemas operacionales) vía Mirroring, Shortcuts o Pipelines
> de Data Factory — sin cargar archivos manualmente. El proceso de Fabric Plan es
> idéntico en ambos casos: Plan siempre lee desde el Semantic Model,
> independientemente de cómo llegaron los datos al Lakehouse.

---

## Referencias

- [What is Plan (preview)? — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/overview)
- [Planning sheets — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-overview)
- [PowerTable sheets — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/powertable-overview)
- [Intelligence sheets — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/intelligence-overview)
- [Get started with Planning sheets — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-how-to-get-started)
- [Writeback — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-writeback/planning-how-to-persist-data)
- [Announcing Planning in Fabric IQ — FabCon Atlanta 2026](https://blog.fabric.microsoft.com/en-us/blog/introducing-planning-in-microsoft-fabric-iq-from-historical-data-to-forecasting-the-future/)
- [Lumel × Microsoft — Strategic Alliance](https://lumel.com/newsroom/lumel-microsoft-strategic-first-party-alliance-fabric-planning/)
