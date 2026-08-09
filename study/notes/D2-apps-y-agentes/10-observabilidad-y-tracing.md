# Observabilidad y tracing

> Docs de Azure — [Trace AI applications](https://learn.microsoft.com/en-us/azure/ai-foundry/how-to/develop/trace-application)
> Cubre: **D1.3.b**, **D2.3.b** · complementa [02 Evaluación de modelos](02-evaluacion-de-modelos.md)
> Peso: **medio-alto** — cierra el ciclo de vida y aparece en preguntas de operación
> Fecha: 2026-08-09

---

## En cristiano

Tu agente responde mal. ¿Por qué? Sin trazas solo tienes la respuesta final — como un ticket de compra sin saber qué pasó en la tienda.

El **tracing** te da la película completa: qué prompt entró, qué tools se llamaron, con qué argumentos, cuánto tardó cada paso y cuántos tokens costó.

---

## ⚠️ Evaluación vs observabilidad — la confusión clásica

Son cosas distintas y el examen las cruza:

| | **Evaluación** | **Observabilidad / tracing** |
| --- | --- | --- |
| **Pregunta que responde** | ¿Es **buena** la respuesta? | ¿**Qué pasó** por dentro? |
| **Cuándo** | Antes de desplegar, con un dataset | **En producción**, con tráfico real |
| **Qué mide** | Groundedness, relevancia, coherencia | Latencia, tokens, llamadas a tools, errores |
| **Herramienta** | Evaluators | **Traces en Application Insights** |

**Regla:** *"¿el modelo alucina?"* → **evaluación**. *"¿por qué tarda 8 segundos?"* o *"¿llamó a la tool correcta?"* → **tracing**.

Se complementan: una evaluación baja te dice que algo falla; la traza te dice **dónde**.

---

## Cómo funciona

Foundry usa **OpenTelemetry** — el estándar abierto — y guarda las trazas en **Azure Application Insights**.

> ⚠️ **Application Insights NO se crea solo.** Hay que asociarlo una vez por recurso de Foundry. Sin eso no hay trazas.

### Los conceptos de OpenTelemetry

| Concepto | Qué es |
| --- | --- |
| **Trace** | La operación completa, de principio a fin |
| **Span** | Un tramo dentro de la traza: una llamada al modelo, una tool, un bloque tuyo |
| **Attributes** | Metadatos que añades a un span (`operation.claims_count`, por ejemplo) |

Los spans **anidan**, y de ahí sale la línea de tiempo que ves en el portal.

---

## Activarlo

**1 · Asociar Application Insights** — Portal → tu proyecto → **Tracing**. Reutilizas uno o creas uno nuevo. Hace falta rol **Contributor** sobre el recurso.

**2 · Instrumentar el código:**

```bash
pip install azure-ai-projects azure-monitor-opentelemetry opentelemetry-instrumentation-openai-v2
```

```python
from azure.monitor.opentelemetry import configure_azure_monitor
from opentelemetry.instrumentation.openai_v2 import OpenAIInstrumentor

# La cadena de conexión se pide al proyecto, no se hardcodea
connection_string = project_client.telemetry.get_application_insights_connection_string()

configure_azure_monitor(connection_string=connection_string)
OpenAIInstrumentor().instrument()      # ← a partir de aquí, cada llamada se traza sola
```

Con esas dos líneas ya se trazan **todas** las llamadas al SDK.

**3 · (Opcional) Capturar el contenido de los mensajes:**

```bash
export OTEL_INSTRUMENTATION_GENAI_CAPTURE_MESSAGE_CONTENT=true
```

> ⚠️ **Está desactivado por defecto a propósito.** Activarlo guarda **prompts y respuestas completos** en Application Insights. Depurar es más fácil, pero puedes estar almacenando datos personales — decisión de privacidad, no técnica.

### Spans propios

Para instrumentar tu lógica de negocio, no solo las llamadas al modelo:

```python
from opentelemetry import trace
tracer = trace.get_tracer(__name__)

@tracer.start_as_current_span("assess_claims_with_context")
def assess_claims(claims, contexts):
    trace.get_current_span().set_attribute("operation.claims_count", len(claims))
    ...
```

Todas las llamadas dentro del método quedan agrupadas bajo ese span.

---

## Qué ves en el portal

Portal → proyecto → **Tracing**:

| Campo | Qué te dice |
| --- | --- |
| **Trace ID** | Identificador único |
| **Duration** | Cuánto tardó — para cazar cuellos de botella |
| **Status** | Éxito o fallo |
| **Operations** | Número de spans |

Y al abrir una traza: línea de tiempo completa, entradas y salidas de cada operación, tokens consumidos, errores y atributos propios.

### Los atributos GenAI estandarizados

OpenTelemetry define un vocabulario común, útil de reconocer:

`gen_ai.operation.name` · `gen_ai.system` · `gen_ai.request.model` · `gen_ai.response.finish_reasons` · `gen_ai.usage.input_tokens` · `gen_ai.usage.output_tokens`

> `finish_reasons` es donde ves si una respuesta se cortó por `content_filter` — enlaza con [IA responsable](../D1-plataforma/02-ia-responsable.md).

---

## Otros dos destinos

| Destino | Para qué |
| --- | --- |
| **Consola** (`ConsoleSpanExporter`) | Tests y CI/CD: las trazas salen por stdout y las captura el pipeline |
| **Foundry Toolkit en VS Code** | Desarrollo local con un colector OTLP. **Sin necesidad de nube** |

---

## Permisos

Para **ver** las trazas hace falta el rol **Log Analytics Reader** sobre el Application Insights. Si las tablas están protegidas, además **Privileged Monitoring Data Reader**.

> Otra vez el patrón: **el recurso conectado necesita su propia asignación de rol**. Ver [Seguridad](../D1-plataforma/04-seguridad-identidad-y-red.md).

---

## Para el examen

**Alto valor:**
- **Evaluación (¿es buena?) vs tracing (¿qué pasó?)**
- **OpenTelemetry + Application Insights**, que **hay que asociar manualmente**
- **Trace / span / attribute**
- La captura del contenido de mensajes está **desactivada por defecto** por privacidad

**Valor medio:** `OpenAIInstrumentor().instrument()` · spans propios con decorador · rol **Log Analytics Reader** para leer · atributos `gen_ai.*` · exportar a consola en CI/CD.

**Bajo valor:** nombres exactos de los paquetes pip · pasos del portal.

---

## Comprueba que lo tienes

1. Tu agente da respuestas correctas pero tarda 12 segundos. ¿Evaluación o tracing? ¿Qué buscas?
2. Activas el tracing en el código y en el portal no aparece nada. ¿Qué se te olvidó?
3. Quieres ver el prompt exacto que se envió. ¿Sale por defecto? ¿Qué implica activarlo?
4. Un compañero puede entrar al proyecto pero no ve las trazas. ¿Qué le falta?
5. Necesitas trazas en tu pipeline de CI, sin depender de Azure. ¿Cómo?
6. ¿Qué es un *span* y en qué se diferencia de un *trace*?

<details>
<summary>Respuestas</summary>

1. **Tracing.** La calidad es correcta, el problema es operativo. En la traza buscas el **span más largo**: qué paso concreto consume esos segundos — una tool lenta, un retrieval pesado, el propio modelo.
2. **Asociar un recurso de Application Insights** al recurso de Foundry. No se crea automáticamente y sin él las trazas no tienen dónde guardarse.
3. **No sale por defecto.** Se activa con `OTEL_INSTRUMENTATION_GENAI_CAPTURE_MESSAGE_CONTENT=true`. Implica **almacenar prompts y respuestas completos** en Application Insights: posible dato personal, así que es una decisión de privacidad y cumplimiento.
4. El rol **Log Analytics Reader** sobre el Application Insights (y *Privileged Monitoring Data Reader* si las tablas están protegidas). El acceso al proyecto no incluye el del recurso conectado.
5. **`ConsoleSpanExporter`**: las trazas se emiten por consola y el pipeline las captura. También existe el Foundry Toolkit de VS Code para desarrollo local.
6. Un **trace** es la operación completa de principio a fin; un **span** es un tramo dentro de ella (una llamada al modelo, una tool, un bloque de tu código). Los spans se anidan y forman la línea de tiempo.

</details>
