# Tools personalizadas: las 4 formas de conectar lo tuyo

> LP2 · Módulo 2 — [Integrate custom tools into your agent](https://learn.microsoft.com/en-us/training/modules/build-agent-with-custom-tools/) (7 unidades)
> Cubre: **D2.2.c** · Profundiza la categoría **Custom** del catálogo visto en [07](07-construir-y-publicar-agentes.md) y [08](08-las-cuatro-tools.md)
> Peso: **alto** — D2 es el dominio más pesado (30–35%)
> Fecha: 2026-08-09

---

## En cristiano

Las tools del catálogo (Code Interpreter, File Search, Bing) resuelven lo genérico. Pero **nadie de fuera conoce tu ERP, tu CRM ni tu base de datos**.

Las **custom tools** son cómo le enseñas al agente a hablar con tus sistemas.

Hay **4 formas**, y elegir la correcta es la pregunta de examen:

| Forma | Qué conectas | Analogía |
| --- | --- | --- |
| **Function calling** | Código tuyo, ejecutándose en tu app | Le enseñas a tu empleado a usar una herramienta que tienes en la mano |
| **Azure Functions** | Código serverless en Azure | Contratas a alguien externo que hace esa tarea concreta |
| **OpenAPI** | Una API que ya existe y está documentada | Le das el manual de un servicio ya montado |
| **Logic Apps** | Un flujo low-code con conectores | Le das acceso a un proceso ya hecho, sin programar |

---

## ⭐ La tabla de decisión

Esto es lo que se pregunta: **te describen una situación y eliges**.

| Si el enunciado dice… | Eliges | Por qué |
| --- | --- | --- |
| "lógica propia en el código de mi app" | **Function calling** | Tu app ejecuta y devuelve |
| "sin servidores", "por evento", "cuando llegue a una cola" | **Azure Functions** | Serverless, con triggers y bindings |
| "la API ya existe", "tenemos su especificación" | **OpenAPI** | Integración estándar, sin escribir código |
| "sin código", "conectar apps y servicios", "flujo de trabajo" | **Logic Apps** | Low-code / no-code |

---

## 1 · Function calling

Ya visto en detalle en [08 Las tools](08-las-cuatro-tools.md). El resumen: **tu código ejecuta**, el agente solo pide.

```python
function_tool = FunctionTool(
    name="recent_snowfall",
    description="Get recent snowfall totals for a given location.",
    parameters={
        "type": "object",
        "properties": {
            "location": {"type": "string", "description": "The city name to check snowfall for."},
        },
        "required": ["location"],
        "additionalProperties": False,
    },
    strict=True,
)

agent = project_client.agents.create_version(
    name="snowfall-agent",
    definition=PromptAgentDefinition(model="gpt-4.1", instructions="...", tools=[function_tool]),
)
```

Tu función puede a su vez **llamar a otras APIs** o arrancar un programa. No tiene que resolverlo todo ella.

---

## 2 · Azure Functions — lo nuevo de este módulo

Computación **serverless**: código que corre en Azure sin que gestiones servidores.

Dos conceptos propios de Azure Functions:

| Concepto | Qué es |
| --- | --- |
| **Trigger** | **Cuándo** se ejecuta la función (una petición HTTP, un mensaje en una cola, un temporizador) |
| **Binding** | **Cómo** se conecta a los datos de entrada o salida, sin escribir código de conexión |

En el agente se configuran una **cola de entrada** y una **cola de salida**:

```python
tool = AzureFunctionTool(
    azure_function=AzureFunctionDefinition(
        input_binding=AzureFunctionBinding(
            storage_queue=AzureFunctionStorageQueue(
                queue_name="STORAGE_INPUT_QUEUE_NAME",
                queue_service_endpoint="STORAGE_QUEUE_SERVICE_ENDPOINT",
            )
        ),
        output_binding=AzureFunctionBinding(...),   # cola de salida
        function=AzureFunctionDefinitionFunction(
            name="queue_trigger",
            description="Get weather for a given location",
            parameters={...},
        ),
    )
)
```

**El flujo:** el agente deja un mensaje en la cola de entrada → la función se dispara → escribe el resultado en la cola de salida → el agente lo recoge.

> ⚠️ **Confusión frecuente — Function calling vs Azure Functions.** Los nombres se parecen y son cosas distintas:
>
> | | Function **calling** | Azure **Functions** |
> | --- | --- | --- |
> | **Dónde corre** | En **tu app** | En **Azure**, serverless |
> | **Quién ejecuta** | Tú, en tu bucle | Azure, por trigger |
> | **Comunicación** | Devuelves el resultado en la respuesta | A través de **colas de storage** |

---

## 3 · OpenAPI — conectar una API que ya existe

Le das la **especificación OpenAPI 3.0** en JSON y Foundry se encarga del resto: mapea los parámetros y parsea la respuesta.

```json
{
  "openapi": "3.1.0",
  "info": { "title": "get weather data", "version": "v1.0.0" },
  "servers": [{ "url": "https://wttr.in" }],
  "paths": {
    "/{location}": {
      "get": {
        "operationId": "GetCurrentWeather",
        "parameters": [ { "name": "location", "in": "path", "required": true, ... } ]
      }
    }
  }
}
```

```python
tool = OpenApiTool(
    openapi=OpenApiFunctionDefinition(
        name="get_weather",
        spec=openapi_weather,
        description="Retrieve weather information for a location.",
        auth=OpenApiAnonymousAuthDetails(),
    )
)
```

### ⚠️ Los 3 tipos de autenticación — dato de examen

OpenAPI 3.0 soporta **exactamente tres**:

| Tipo | Cuándo |
| --- | --- |
| **Anonymous** | API pública sin credenciales |
| **API key** | El servicio pide una clave |
| **Managed identity** | Recurso de Azure — **la opción recomendada** |

Solo esas tres. Si una pregunta ofrece OAuth o certificados como opción de OpenAPI tool, es distractor.

---

## 4 · Logic Apps

**Low-code / no-code.** Flujos de trabajo que conectan apps, datos y servicios mediante conectores ya hechos, sin programar.

> ⚠️ **Logic Apps no está soportado con aislamiento de red** (ver [Seguridad](../D1-plataforma/04-seguridad-identidad-y-red.md)).

---

## ⭐ Lo declarativo — el concepto que el módulo marca como difícil

El propio material avisa de que esto cuesta entenderlo:

> **Tú nunca escribes código que llame a la tool.**

No hay un `if el usuario pregunta por el tiempo: llamar_a_get_weather()`. Tú **registras** la tool con un nombre y una descripción, y **el agente decide solo** cuándo usarla leyendo el prompt.

**La consecuencia práctica:** la calidad de tus **nombres y descripciones** determina si el agente acierta. No son documentación — son las instrucciones con las que decide.

| Mal | Bien |
| --- | --- |
| `procesar(datos)` — *"procesa datos"* | `recent_snowfall(location)` — *"Get recent snowfall totals for a given location"* |

---

## Dónde se usan las custom tools

Los casos del módulo, todos con el mismo patrón — **conectar el agente a un sistema interno**:

| Sector | Sistema | Qué hace el agente |
| --- | --- | --- |
| Atención al cliente | **CRM** | Historial de pedidos, reembolsos, estado de envío |
| Fabricación | **Inventario** | Stock, predicción de reposición, pedidos automáticos |
| Salud | **Agenda + historiales** | Huecos disponibles, recordatorios |
| IT | **Ticketing** | Diagnóstico, escalado, seguimiento |
| Educación | **LMS** | Recomendar cursos, seguir progreso |

---

## Para el examen

**Alto valor:**
- **Las 4 opciones y cuándo cada una** (function calling / Azure Functions / OpenAPI / Logic Apps)
- **Function calling ≠ Azure Functions** — tu app vs serverless con colas
- **OpenAPI: solo 3 tipos de auth** — anonymous, API key, managed identity
- **Es declarativo:** tú no llamas a la tool, el agente decide. Los **nombres y descripciones** son lo que lo hace posible

**Valor medio:** triggers y bindings de Azure Functions · colas de entrada/salida · `strict: true` · Logic Apps como low-code y su límite de red.

**Bajo valor:** los casos de uso por sector · la sintaxis completa de una spec OpenAPI.

---

## Comprueba que lo tienes

1. Tu empresa ya tiene una API REST documentada con OpenAPI 3.0 para consultar pedidos. ¿Qué opción eliges y cuánto código escribes?
2. Necesitas que el agente procese algo pesado cuando llega un mensaje a una cola, sin mantener servidores. ¿Cuál?
3. Diferencia entre *function calling* y *Azure Functions*.
4. Tu OpenAPI tool debe autenticarse contra un servicio de Azure. ¿Qué tipo de auth eliges de los disponibles?
5. Registraste una tool y el agente nunca la usa. El código está bien. ¿Qué revisas?
6. Un compañero pregunta dónde está el `if` que decide llamar a la tool. ¿Qué le respondes?
7. Requisito: todo el tráfico en red privada, y quieres usar Logic Apps. ¿Se puede?

<details>
<summary>Respuestas</summary>

1. **OpenAPI tool.** Le pasas la spec JSON y **no escribes código de integración**: Foundry mapea los parámetros y parsea la respuesta.
2. **Azure Functions**, con un **trigger de cola**. Es serverless y está pensado para flujos por eventos.
3. **Function calling** = el modelo pide y **tu app ejecuta** la función en tu proceso. **Azure Functions** = código serverless **en Azure**, que se dispara por trigger y se comunica por **colas de storage**.
4. **Managed identity** — es la recomendada para recursos de Azure y evita gestionar claves. Las otras dos opciones posibles son anonymous y API key.
5. **El nombre y la descripción de la tool.** Es declarativo: el agente decide leyendo esos campos. Una descripción vaga hace que nunca la elija.
6. Que **no existe**. El agente decide por sí mismo en función del prompt y de las descripciones de las tools. Es un sistema **declarativo**: tú registras capacidades, no invocaciones.
7. **No.** Logic Apps **no está soportado** con aislamiento de red.

</details>
