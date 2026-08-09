# Multi-agente y orquestación

> Docs de Azure — [Agent Framework Workflows](https://learn.microsoft.com/en-us/agent-framework/workflows/) · complementa [04 Agentes en Foundry](04-agentes-en-foundry.md)
> Cubre: **D2.2.d**, **D2.2.e** · toca **D1.4.d** (gobierno de agentes)
> Peso: **alto** — LP2 dedica 9 módulos a esto y D2 pesa 30–35%
> Fecha: 2026-08-09

---

## En cristiano

Un agente resuelve una tarea. Cuando el problema es grande, hay dos maneras de repartirlo — y la diferencia es **quién decide el orden**.

| | **Agente** | **Workflow** |
| --- | --- | --- |
| **Quién decide los pasos** | **El modelo**, sobre la marcha | **Tú**, de antemano |
| **Analogía** | Un empleado listo al que le das objetivos | Una cadena de montaje con estaciones fijas |
| **Ventaja** | Se adapta a lo inesperado | **Predecible, auditable, repetible** |
| **Riesgo** | Puede irse por donde no quieres | No maneja lo que no previste |

**Regla de decisión:** ¿el enunciado dice *"proceso de negocio"*, *"pasos definidos"*, *"auditable"* o *"con aprobación humana"*? → **workflow**. ¿Dice *"se adapta"*, *"según la consulta del usuario"*? → **agente**.

---

## Los 4 patrones de orquestación ⭐

Esto es lo más examinable. Cada uno resuelve una forma distinta de repartir trabajo.

| Patrón | Cómo funciona | Cuándo |
| --- | --- | --- |
| **Sequential** | A → B → C, en cadena. La salida de uno es la entrada del siguiente | Un proceso con etapas: redactar → revisar → traducir |
| **Concurrent** | Varios trabajan **a la vez** sobre lo mismo, y se combinan los resultados | Varias perspectivas del mismo problema: legal + técnico + comercial |
| **Handoff** | Uno **cede el control** a otro más adecuado y se retira | Triaje: soporte general detecta que es de facturación y transfiere |
| **Magentic** | Un **orquestador dinámico** decide en cada momento quién actúa | Problemas abiertos donde el plan no se sabe de antemano |

**Cómo distinguirlos en una pregunta:**

| Si el enunciado dice… | Es |
| --- | --- |
| "primero… después… finalmente" | **Sequential** |
| "en paralelo", "distintos puntos de vista", "y luego agregar" | **Concurrent** |
| "derivar", "transferir al especialista" | **Handoff** |
| "el sistema decide qué agente usar según el problema" | **Magentic** |

---

## Cómo se construye un workflow

Es un **grafo dirigido**:

| Pieza | Qué es |
| --- | --- |
| **Executors** | Los nodos. Un agente de IA **o** código tuyo |
| **Edges** | Las conexiones. Pueden llevar **condiciones** → enrutado según el contenido |
| **Events** | La observabilidad: qué pasó en cada nodo |

Lo importante: **un executor no tiene por qué ser un agente**. Puedes mezclar pasos de IA con validaciones, llamadas a API o transformaciones deterministas. Es lo que hace que un workflow sea auditable.

### Dos APIs

| | **Functional** (`@workflow`, `@step`) | **Graph** (`WorkflowBuilder`) |
| --- | --- | --- |
| **Control de flujo** | Python normal: `if`, bucles, `asyncio.gather` | Edges con condiciones |
| **Mejor para** | Pipelines secuenciales, bucles a medida | Grafos fijos, fan-out/fan-in, tipado estricto |

Empieza con la functional; pasa a la de grafo cuando necesites enrutado con tipos validados.

---

## Las 3 características que resuelven problemas reales

### Type safety

Los mensajes entre executors están **fuertemente tipados**. Si un nodo devuelve algo que el siguiente no acepta, falla al construir el workflow, no en producción.

### Checkpointing

El estado se guarda en **checkpoints**: un proceso largo puede **recuperarse y continuar** desde donde se cayó, en vez de reempezar.

En la API de grafo los checkpoints van en el **límite de cada superstep**; en la functional, se cachea el resultado de cada `@step`.

### Human-in-the-loop

Patrón de request/response integrado para **pedir aprobación humana** a mitad del proceso.

| API | Cómo |
| --- | --- |
| Functional | `ctx.request_info()` |
| Grafo | `RequestInfoExecutor` |

> Esta es la mitigación clave contra el riesgo de **exceso de autonomía** — ver los 8 riesgos en [04 Agentes en Foundry](04-agentes-en-foundry.md).

---

## Agent-to-Agent (A2A)

Un agente puede **delegar en otro** como si fuera una tool más (la tool *Agent-to-Agent* del catálogo). Es la vía descentralizada: no hay orquestador, los agentes se llaman entre sí.

**Diferencia con un workflow:** en A2A la decisión de delegar la toma el modelo; en un workflow el camino lo definiste tú.

Con aislamiento de red, A2A **sí está soportado** y va por tu VNet.

---

## Un workflow también puede ser un agente

`.as_agent()` envuelve un workflow entero y lo expone **como si fuera un solo agente**.

Así compones: un workflow complejo se convierte en un componente reutilizable dentro de otro sistema. Por fuera parece un agente; por dentro son diez.

---

## ⚠️ Límite de red que cae

> Los **workflow agents** solo soportan aislamiento de red **parcialmente**: la entrada sí, pero la **inyección de VNet para salida no está soportada**.

Si el requisito es aislamiento extremo a extremo con workflow agents, hoy **no se cumple**.

---

## Para el examen

**Alto valor:**
- **Los 4 patrones** y cómo reconocerlos por el enunciado: sequential / concurrent / handoff / magentic
- **Agente = el modelo decide · Workflow = tú defines el camino**
- **Executors y edges**; un executor puede ser código, no solo un agente
- **Human-in-the-loop** integrado en workflows
- **Checkpointing** para procesos largos recuperables

**Valor medio:** type safety entre executors · `.as_agent()` para componer · A2A como tool · Functional vs Graph API · los workflow agents no soportan VNet injection de salida.

**Bajo valor:** nombres exactos de clases por lenguaje · detalles de los supersteps.

---

## Comprueba que lo tienes

1. Un proceso: extraer datos de una factura → validarlos → registrarlos en el ERP. Los pasos son siempre los mismos y auditoría exige trazabilidad. ¿Agente o workflow, y qué patrón?
2. Quieres que tres agentes analicen el mismo contrato desde lo legal, lo técnico y lo económico, y luego combinar sus conclusiones. ¿Qué patrón?
3. Tu agente de soporte detecta que la consulta es de facturación y debe pasarla al agente especializado, desentendiéndose. ¿Qué patrón?
4. Un workflow procesa 10.000 documentos y falla en el 7.000. ¿Qué característica evita reempezar?
5. Un paso del workflow ejecuta un reembolso. ¿Qué añades y cómo se llama en cada API?
6. ¿Puede un executor ser algo que no sea un agente de IA? ¿Por qué importa?
7. Requisito: workflow agents con todo el tráfico de salida dentro de la red privada. ¿Se puede hoy?

<details>
<summary>Respuestas</summary>

1. **Workflow**, patrón **sequential**. Los pasos son fijos y hace falta trazabilidad: no quieres que un modelo decida el orden.
2. **Concurrent**. Trabajan en paralelo sobre la misma entrada y los resultados se agregan.
3. **Handoff**: cede el control al agente adecuado y se retira. Distinto de concurrent, donde ambos seguirían trabajando.
4. **Checkpointing**. El estado se guarda y el proceso se reanuda desde el último checkpoint.
5. **Human-in-the-loop**: aprobación antes de ejecutar. `ctx.request_info()` en la API functional, `RequestInfoExecutor` en la de grafo.
6. **Sí**: puede ser código propio — validaciones, llamadas a API, transformaciones. Importa porque permite mezclar pasos deterministas con pasos de IA, y eso es lo que hace auditable el proceso.
7. **No.** Los workflow agents soportan aislamiento **parcial**: entrada sí, **inyección de VNet para salida no**.

</details>
