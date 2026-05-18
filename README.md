# Microsoft Fabric Plan — Walkthrough de Planeación Operativa

> **Escenario:** Zava Environmental opera estaciones de gestión de residuos en cuatro regiones de México. Cada ciclo de presupuesto anual es una pesadilla de coordinación: docenas de archivos Excel circulando por correo, consolidaciones manuales que toman días, y al final del proceso nadie está seguro de estar mirando los mismos números.
>
> Este repositorio demuestra cómo **Microsoft Fabric Plan** resuelve ese problema — y lo hace en dos caminos distintos, dependiendo de en qué punto del viaje tecnológico se encuentre la organización.

> ⚠️ **Preview:** Fabric Plan está en Public Preview desde marzo 2026. Los meters de consumo (CUs) existen pero no se cobran aún; billing estimado para junio 2026.

---

## El pain: el ciclo de presupuesto hoy

Cada año, el equipo de finanzas de Zava inicia el proceso de planeación enviando una plantilla Excel a los responsables de cada estación. Lo que sigue es predecible:

```
Finanzas                Región Norte          Región Centro
    │                        │                     │
    │── plantilla.xlsx ──▶   │                     │
    │                   llena, guarda              │
    │                   manda por correo           │
    │◀── norte_v1.xlsx ──────│                     │
    │                                              │
    │── plantilla.xlsx ──────────────────────────▶ │
    │                                         llena, guarda
    │◀── centro_FINAL.xlsx ────────────────────────│
    │
    │  ... mismo proceso con Sur y Bajío ...
    │
    │  [3 días después]
    │
    │  Consolida manualmente en presupuesto_2026_v8.xlsx
    │  Descubre inconsistencias entre regiones
    │  Vuelve a pedir correcciones
    │
    │  [1 semana después]
    │
    │  presupuesto_2026_DEFINITIVO_este_si.xlsx ✓ (quizás)
```

**El resultado:** datos desactualizados, errores de consolidación, sin trazabilidad de quién cambió qué, y un equipo de finanzas que pasó una semana pegando celdas en lugar de analizando el negocio.

---

## La solución: Microsoft Fabric Plan

Fabric Plan es una solución EPM (Enterprise Performance Management) construida directamente dentro de Microsoft Fabric. Permite a las organizaciones crear, gestionar y analizar planes — como presupuestos, forecasts y escenarios — dentro de la misma plataforma gobernada que usan para datos, analítica e IA.

En términos prácticos para Zava: **un solo lugar donde cada región captura su presupuesto, finanzas lo revisa en tiempo real, y el comparativo contra gastos reales está disponible de inmediato** — sin exportaciones, sin consolidaciones manuales, sin versiones de archivos.

---

## Dos caminos de adopción

No todas las organizaciones llegan a Fabric Plan desde el mismo punto. Este repositorio cubre ambos escenarios:

### 📂 Path 1 — Excel directo (adopción inicial)

**Cuándo usar este camino:**
- La organización no tiene datos históricos en Fabric todavía
- Se quiere demostrar valor rápido sin construir infraestructura previa
- El cliente está en evaluación y necesita ver Plan funcionar en horas, no días

**Cómo funciona:**  
Los archivos Excel que hoy circulan por correo se suben directamente a Plan. Plan los ingiere, los almacena en un Fabric SQL Database interno, y de inmediato habilita captura de presupuesto, consolidación y análisis — todo sin mover datos a un Lakehouse ni construir un modelo semántico.

**Trade-off:** los datos viven dentro de Plan (Fabric SQL Database) y no están conectados al resto de la plataforma Fabric. Es el punto de partida correcto, no el destino final.

→ **[Ir al walkthrough: Path 1 — Excel directo](./path-1-excel/README.md)**

---

### 🏗️ Path 2 — Semantic Model (adopción madura)

**Cuándo usar este camino:**
- La organización ya tiene datos en Fabric (Lakehouse, Warehouse, o Eventstream)
- Se quiere que el presupuesto se compare contra datos históricos gobernados y actualizados automáticamente
- El equipo de BI ya tiene un Semantic Model que el resto de la organización usa en Power BI

**Cómo funciona:**  
Los datos históricos de gastos reales viven en un Lakehouse con una tabla Delta. Sobre eso se construye un Semantic Model (Direct Lake). Plan se conecta a ese modelo — los actuals que ve el equipo de planeación son exactamente los mismos que ve el equipo de BI en Power BI, con las mismas definiciones y la misma gobernanza.

**Trade-off:** requiere más pasos de setup, pero el resultado es una fuente única de verdad que conecta planeación, reporteo y analítica en el mismo ambiente.

→ **[Ir al walkthrough: Path 2 — Semantic Model](./path-2-semantic-model/README.md)**

---

## Comparativa rápida

| | Path 1 — Excel directo | Path 2 — Semantic Model |
|---|---|---|
| **Tiempo de setup** | ~30 minutos | ~2 horas |
| **Prerequisitos Fabric** | Workspace con capacidad | Lakehouse + Semantic Model |
| **Fuente de actuals** | Excel subido manualmente | Delta table en OneLake (viva) |
| **Gobernanza de datos** | Dentro de Plan (SQL DB) | OneLake + Unity de Fabric |
| **Sincronización** | Manual (re-subir Excel) | Automática |
| **Ideal para** | Piloto, evaluación rápida | Producción, adopción consolidada |

---

## Dataset

Ambos caminos usan los mismos datos de Zava Environmental — gastos operativos 2024–2025 por región, estación y categoría de gasto. El formato difiere por path:

- **Path 1:** `zava_actuals.xlsx` — subido directamente a Plan como fuente de referencia
- **Path 2:** `zava_actuals.xlsx` — mismo archivo, cargado al Lakehouse como tabla Delta

---

## Referencias

- [What is Plan (preview)? — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/overview)
- [Get started with Planning sheets — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-how-to-get-started)
- [Build forecasts — Microsoft Learn](https://learn.microsoft.com/en-us/fabric/iq/plan/planning-how-to-build-forecasts)
- [Announcing Planning in Fabric IQ — FabCon Atlanta 2026](https://blog.fabric.microsoft.com/en-us/blog/introducing-planning-in-microsoft-fabric-iq-from-historical-data-to-forecasting-the-future/)
