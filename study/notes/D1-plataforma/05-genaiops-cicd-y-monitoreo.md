# GenAIOps: CI/CD, monitoreo continuo y salud de índices

> Docs de Azure — [Observability in generative AI](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/observability) · [Monitor queries in AI Search](https://learn.microsoft.com/en-us/azure/search/search-monitor-queries)
> Cubre: **D1.2.d** (CI/CD), **D1.3.b**, **D1.3.c** (salud de índices y relevancia)
> Peso: **medio** — objetivos de menor frecuencia, pero D1 pesa 25–30% y el curso oficial no los cubre
> Fecha: 2026-08-09

---

## En cristiano

Con software normal el ciclo es: escribes código → tests → despliegas. Si los tests pasan, funciona.

Con IA generativa **no hay tests que pasen o fallen**: hay respuestas mejores o peores. Eso rompe el CI/CD clásico y obliga a añadir una pieza: **la evaluación como puerta de calidad**.

> **GenAIOps** = DevOps donde el test unitario se sustituye por un **evaluador**.

---

## Las 3 etapas del ciclo de vida ⭐

Lo más examinable: **cuándo se evalúa qué**.

| Etapa | Pregunta | Herramienta |
| --- | --- | --- |
| **1 · Base model selection** | ¿Qué modelo elijo? | **Benchmarks** y leaderboard, comparando modelos |
| **2 · Pre-production** | ¿Está listo para salir? | **Evaluación con tu dataset** + **AI red teaming agent** |
| **3 · Post-production** | ¿Sigue funcionando bien? | **Monitoreo continuo** en producción |

> ⚠️ **Confusión frecuente:** *benchmarks* ≠ *evaluación*. Los benchmarks comparan modelos sobre datasets públicos (etapa 1). La evaluación mide **tu** aplicación con **tus** datos (etapa 2). Un modelo que gana el leaderboard puede rendir mal en tu caso.

### Etapa 2 — antes de desplegar

- **Bring your own data**: evalúas con datos tuyos, con evaluadores integrados o **custom**
- **AI red teaming agent**: simula ataques con el framework **PyRIT** de Microsoft para encontrar fallos de seguridad **antes** del despliegue. Se recomienda con human-in-the-loop

### Etapa 3 — ya en producción

Aquí está la parte que casi nadie estudia:

| Mecanismo | Qué hace |
| --- | --- |
| **Operational metrics** | Latencia, tokens, tasa de error |
| **Continuous evaluation** | Evalúa **tráfico real, a un ritmo muestreado** |
| **Scheduled evaluation** | Evaluación periódica con un dataset fijo → detecta **drift** |
| **Scheduled red teaming** | Pruebas adversarias periódicas |
| **Azure Monitor alerts** | Avisan cuando la calidad baja del umbral o sale contenido dañino |

**Continuous vs scheduled — la distinción que cae:**

- **Continuous** → muestrea el **tráfico real**. Detecta qué está pasando *ahora*.
- **Scheduled** → **dataset fijo**, repetido en el tiempo. Detecta **drift**, porque la vara de medir no cambia.

---

## CI/CD para proyectos de IA

La idea central: **quality gates automatizados en el pipeline**.

| Paso del pipeline | En un proyecto de IA |
| --- | --- |
| Versionar | **El YAML del agente en Git**, junto al código |
| Build | — |
| **Test** | **Ejecutar evaluadores contra un dataset de referencia** |
| **Gate** | Si groundedness o safety bajan del umbral → **el pipeline falla** |
| Deploy | Publicar la nueva versión del agente |
| Post-deploy | Monitoreo continuo y alertas |

Piezas que lo hacen posible:

- **Evaluación remota en la nube** desde el SDK, para lanzarla desde el pipeline
- **Trazas a consola** (`ConsoleSpanExporter`) para que CI las capture — ver [Observabilidad](../D2-apps-y-agentes/10-observabilidad-y-tracing.md)
- **Versionado de agentes**: publicar una versión nueva **no cambia la URL** del Agent Application, y el tráfico se enruta al 100% a la nueva

> ⚠️ **Ojo con el coste:** las evaluaciones del **agents playground están activadas por defecto** en todos los proyectos y **se facturan por consumo**. Se desactivan quitando los evaluadores en el panel de métricas.

---

## Salud y relevancia de un índice de AI Search

El otro hueco de D1. Tres métricas, disponibles en la pestaña **Monitoring** del servicio:

| Métrica | Qué mide | Qué indica |
| --- | --- | --- |
| **Search latency** | Cuánto tarda una consulta | Cuello de botella de rendimiento |
| **QPS** (*Search Queries Per Second*) | Volumen de consultas | Si te acercas al límite de tu configuración |
| **Throttled queries %** | Consultas **descartadas**, no procesadas | Falta de capacidad |

**Datos que conviene retener:**

- Las métricas se guardan **30 días** por defecto. Para más tiempo hay que **activar diagnostic logging**
- El destino recomendado es un **Log Analytics workspace**, consultable con **Kusto**
- **El throttling no siempre es un fallo** — es parte normal del servicio. Se vuelve problema si sube

**El patrón clásico de diagnóstico:** si las consultas descartadas suben **durante la indexación**, el índice y las búsquedas compiten por capacidad. Solución: posponer la indexación, o añadir réplicas.

> **Réplicas vs particiones:** más **réplicas** = más capacidad de **consulta**. Más **particiones** = más capacidad de **almacenamiento**. Si el problema es throttling de queries, se añaden réplicas.

**Alertas:** lo habitual es crear metric alerts sobre **search latency** y **throttled queries**.

Para medir con precisión, Microsoft recomienda una **sola réplica y una sola partición**: con varias, las métricas se promedian entre nodos y pierden precisión.

---

## Para el examen

**Alto valor:**
- **Las 3 etapas**: selección de modelo → pre-producción → post-producción
- **Continuous evaluation (tráfico real muestreado) vs scheduled (dataset fijo, detecta drift)**
- **Evaluadores como quality gate en CI/CD**
- **Benchmarks ≠ evaluación**
- **Throttled queries + latency** son las métricas de salud de un índice

**Valor medio:** AI red teaming agent y PyRIT · YAML del agente en Git · 30 días de retención y diagnostic logging a Log Analytics · réplicas para consultas, particiones para almacenamiento · alertas de Azure Monitor.

**Bajo valor:** sintaxis Kusto · pasos del portal para crear una alerta.

---

## Comprueba que lo tienes

1. Tu pipeline despliega una versión nueva del agente cada semana. ¿Cómo evitas que una que responde peor llegue a producción?
2. Diferencia entre *continuous evaluation* y *scheduled evaluation*. ¿Cuál detecta drift y por qué?
3. Un modelo lidera el leaderboard de Foundry. ¿Basta para elegirlo?
4. Tu índice de AI Search descarta consultas justo cuando corre la indexación nocturna. ¿Qué pasa y qué opciones tienes?
5. Necesitas analizar las consultas de hace 3 meses en AI Search. ¿Puedes?
6. Quieres encontrar vulnerabilidades de seguridad de tu agente antes de publicarlo. ¿Qué herramienta y en qué etapa?
7. Tu factura de Foundry sube sin que nadie use la app en producción. ¿Qué revisas?

<details>
<summary>Respuestas</summary>

1. **Evaluadores como quality gate**: el pipeline ejecuta evaluación contra un dataset de referencia y **falla el build** si groundedness, safety o las métricas que definas bajan del umbral.
2. **Continuous** evalúa **tráfico real muestreado** — te dice qué pasa ahora. **Scheduled** usa un **dataset fijo** repetido en el tiempo, y por eso **detecta drift**: como la vara de medir no cambia, cualquier variación viene del sistema.
3. **No.** Los benchmarks comparan sobre **datasets públicos**. Hay que evaluar con **tus datos y tu caso** (etapa 2) antes de decidir.
4. La indexación y las consultas **compiten por la misma capacidad**. Opciones: mover la indexación a una ventana de menos tráfico, o **añadir réplicas** (que es lo que da capacidad de consulta).
5. **Por defecto no**: solo se retienen **30 días**. Habría que haber activado **diagnostic logging** hacia un Log Analytics workspace.
6. El **AI red teaming agent** (basado en PyRIT), en la etapa de **pre-producción**. También puede programarse de forma periódica ya en producción.
7. Las **evaluaciones del agents playground**: están **activadas por defecto** en todos los proyectos y se facturan por consumo. Se desactivan quitando los evaluadores del panel de métricas.

</details>
