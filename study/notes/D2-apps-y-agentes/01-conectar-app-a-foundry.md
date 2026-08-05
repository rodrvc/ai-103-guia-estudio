# Conectar tu app a Foundry: endpoints, SDKs, auth y APIs

> LP1 · Módulo 3 — [Develop a generative AI chat app with Microsoft Foundry](https://learn.microsoft.com/en-us/training/modules/foundry-sdk/) · 8 unidades
> Unidades cubiertas: 2 (playground), **3 (endpoints/SDK/auth)**, **4 (Responses API)**, **5 (ChatCompletions)**
> Cubre: **D2.1.e, D2.1.f** · Toca **D1.3.d** · **Cierra E-003** (auth sin claves) y aclara **E-001**
> Peso: **alto** — la unidad 3 es de las más rentables de LP1
> Fecha: 2026-08-04

---

## En cristiano

Para hablar con un modelo desde tu código necesitas decidir **cuatro cosas**:

1. **¿A qué dirección llamo?** → endpoint
2. **¿Con qué librería?** → SDK
3. **¿Cómo pruebo quién soy?** → autenticación
4. **¿Qué API de chat uso?** → Responses o ChatCompletions

---

## 1. Dos endpoints — todo proyecto tiene ambos

| Endpoint | Forma | Para qué |
| --- | --- | --- |
| **Project endpoint** | `https://{recurso}.services.ai.azure.com/api/projects/<proyecto>` | Cosas del **proyecto**: agentes, evaluaciones, tracing, conexiones, índices |
| **Azure OpenAI endpoint** | `https://{recurso}.openai.azure.com/openai/v1` | Solo **inferencia**: mandar prompts y recibir respuestas |

Ambos están en la página **Overview** del proyecto en el portal.

---

## 2. Dos SDKs

### Foundry SDK — `azure-ai-projects`

```bash
pip install azure-ai-projects azure-identity openai
```

```python
from azure.identity import DefaultAzureCredential
from azure.ai.projects import AIProjectClient

project_client = AIProjectClient(
    credential=DefaultAzureCredential(),
    endpoint=project_endpoint
)
# El cliente de chat sale de aquí:
openai_client = project_client.get_openai_client(api_version="2024-10-21")
```

⚠️ **Ojo:** aunque uses el Foundry SDK, **también instalas `openai`** — la funcionalidad de chat viene de ahí.

### OpenAI SDK — `openai`

```bash
pip install openai azure-identity
```

### Cuál elegir

| Usa **Foundry SDK** si necesitas | Usa **OpenAI SDK** si quieres |
| --- | --- |
| Agent Service | Compatibilidad total con OpenAI |
| Aprobación de tools | Portabilidad OpenAI ↔ Azure |
| **Cloud evaluations** | Solo inferencia, sin ataduras |
| **Tracing y observabilidad** | Que tu código OpenAI existente funcione casi sin tocar |
| Foundry direct models | |
| Conexiones y gobernanza del proyecto | |

**Se pueden combinar** en la misma app: Foundry SDK para lo del proyecto, OpenAI SDK para inferencia.

---

## 3. Autenticación — ✅ esto cierra tu E-003

**Regla:** producción → **Microsoft Entra ID**. Las API keys, solo para pruebas.

```python
from openai import OpenAI
from azure.identity import DefaultAzureCredential, get_bearer_token_provider

token_provider = get_bearer_token_provider(
    DefaultAzureCredential(), "https://ai.azure.com/.default"
)

openai_client = OpenAI(
    base_url="https://{recurso}.openai.azure.com/openai/v1/",
    api_key=token_provider,   # ← un proveedor de tokens, NO una clave
)
```

Las tres formas, de mejor a peor:

| Método | Cuándo |
| --- | --- |
| **Entra ID** (`DefaultAzureCredential`) | ✅ **Producción.** Cero secretos en config |
| **API key** | Pruebas. Si la usas: **Azure Key Vault**, nunca en el código |
| **Variables de entorno** | `OPENAI_BASE_URL` + `OPENAI_API_KEY` → el cliente las toma solo |

**Para correr en local:** `az login` antes de ejecutar. `DefaultAzureCredential` busca tu sesión de Azure CLI.

### El cliente `AzureOpenAI` (caso aparte)

Normalmente usas `OpenAI` con el endpoint v1. Solo usas `AzureOpenAI` si necesitas **una versión concreta** de la API de Azure OpenAI:

```python
openai_client = AzureOpenAI(
    azure_endpoint="https://{recurso}.openai.azure.com",
    api_key=os.getenv("AZURE_OPENAI_KEY"),
    api_version="2024-10-21",
)
```

---

## 4. Responses API vs ChatCompletions — la comparación que cae

| | **Responses** ⭐ recomendada | **ChatCompletions** |
| --- | --- | --- |
| Estado de la conversación | **El servicio lo guarda** | **Tú lo guardas** en tu código |
| Cómo encadenas | `previous_response_id` | Reenvías toda la lista de `messages` |
| Origen | Une ChatCompletions + Assistants | La clásica |
| Foundry direct models | ✅ Sí | Limitado |
| Cuándo usarla | Proyectos nuevos | Código existente, compatibilidad multiplataforma |

### Responses API

```python
response = openai_client.responses.create(
    model="gpt-4.1",                    # ← nombre del DEPLOYMENT
    instructions="Eres un asistente…",  # ← el system prompt
    input="¿Qué es machine learning?",
    temperature=0.8,
    max_output_tokens=200
)
print(response.output_text)
```

Del objeto sacas: `output_text` · `id` · `status` · `usage.total_tokens` · `model`.

**Multi-turno — la clave:**

```python
response2 = openai_client.responses.create(
    model="gpt-4.1",
    input="¿Me das un ejemplo?",
    previous_response_id=response1.id   # ← encadena con la anterior
)
```

También puedes recuperar una respuesta pasada: `openai_client.responses.retrieve(response_id)`.

### ChatCompletions

```python
completion = openai_client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "system", "content": "Eres un asistente útil."},
        {"role": "user", "content": "¿Cuándo se fundó Microsoft?"}
    ]
)
print(completion.choices[0].message.content)
```

**Aquí tú mantienes el historial:** tras cada respuesta, añades `{"role": "assistant", ...}` a la lista y reenvías **toda** la conversación en cada turno.

---

## 5. Detalles que suelen caer

**El estado no es gratis.** Aunque Responses guarde la conversación, en cada request se envía y tokeniza todo: instructions + prompt actual + historial + esquemas de tools + salidas de tools + documentos recuperados. *"El SDK gestiona el estado, no abarata los tokens."*

**Streaming** — para que la app no parezca congelada:

```python
stream = openai_client.responses.create(..., stream=True)
for event in stream:
    if event.type == "response.output_text.delta":
        print(event.delta, end="")
    elif event.type == "response.completed":
        response_id = event.response.id   # así recuperas el id al final
```

**Async** — `AsyncOpenAI` + `await`, para requests concurrentes sin bloquear.

**Encadenado manual** — puedes construir el historial a mano incluso con Responses. Sirve para podar contexto y controlar tokens, o para guardar/restaurar conversaciones desde tu base de datos.

---

## Conexión con tus errores

- **E-003 (auth sin claves):** aquí está la respuesta completa — `DefaultAzureCredential` + `get_bearer_token_provider`, del paquete `azure-identity`.
- **E-001 (deployment):** fíjate que en todos los ejemplos, el parámetro `model=` recibe **el nombre de tu deployment**, no el del modelo del catálogo. Eso era exactamente lo que fallaste.

---

## Para el examen

**Alto valor:**
- **Dos endpoints:** project (agentes, evaluaciones, tracing) vs Azure OpenAI (inferencia)
- **Entra ID en producción**, keys solo para pruebas → `DefaultAzureCredential`
- **Responses = stateful** (`previous_response_id`) · **ChatCompletions = tú gestionas el historial**
- Responses es la recomendada para proyectos nuevos
- Cuándo Foundry SDK vs OpenAI SDK

**Valor medio:**
- `AIProjectClient` + `get_openai_client()`
- Nombres de paquetes: `azure-ai-projects`, `azure-identity`, `openai`
- Cuándo `AzureOpenAI` en vez de `OpenAI` (fijar `api_version`)
- Streaming y async
- El estado no reduce el consumo de tokens

**Bajo valor:** las URLs exactas, los números de versión de API.

---

## Comprueba que lo tienes

1. Tu app corre en Azure y la política prohíbe secretos en configuración. ¿Cómo te autenticas? Nombra el mecanismo y la clase de Python.
2. Construyes un chat multi-turno con Responses API. ¿Cómo recuerda el modelo lo anterior? ¿Y si usaras ChatCompletions?
3. Tu app necesita agentes **y** evaluaciones en la nube. ¿Qué SDK y qué endpoint?
4. Verdadero o falso: usar Responses API con estado gasta menos tokens que reenviar el historial a mano.
5. En `responses.create(model="gpt-4.1", ...)`, ¿qué es exactamente `"gpt-4.1"`?

<details>
<summary>Respuestas</summary>

1. **Microsoft Entra ID** con **Managed Identity** y rol RBAC. En Python: `DefaultAzureCredential` (de `azure-identity`), pasada como credential o vía `get_bearer_token_provider`.
2. **Responses:** el servicio guarda el estado; encadenas con **`previous_response_id`**. **ChatCompletions:** no hay estado — tú mantienes la lista de `messages` y reenvías toda la conversación en cada turno.
3. **Foundry SDK** (`azure-ai-projects`, `AIProjectClient`) con el **project endpoint**. Agentes, evaluaciones y tracing son funciones del proyecto, no del endpoint de inferencia.
4. **Falso.** El SDK gestiona el estado, pero el historial se envía y tokeniza igual en cada request.
5. El **nombre de tu deployment** — no el del modelo del catálogo. (Tu E-001.)

</details>
