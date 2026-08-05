# RAG y grounding

> LP1 · Módulo *Optimize generative AI model performance* · Unidad 3 — [Ground your model with Retrieval Augmented Generation](https://learn.microsoft.com/en-us/training/modules/optimize-generative-ai-model-performance/3-retrieval-augmented-generation)
> Cubre: **grounding con RAG · Azure AI Search como retrieval** · Cierra tu error **E-005**
> Fecha: 2026-08-05

---

## La idea que te faltaba

El modelo **no sabe nada de tus datos**. Su entrenamiento tiene fecha de corte y no incluye tu catálogo, tus políticas ni tu inventario. Cuando le falta contexto no dice "no sé": **inventa algo que suena bien**.

**Grounding** = pasarle el dato real junto con la pregunta. **RAG** es la forma más común de hacerlo.

---

## La analogía

Un recepcionista de hotel nuevo, sin acceso al sistema:

| | Qué pasa |
| --- | --- |
| **Sin grounding** | Le preguntas "¿qué hoteles tienen en París?" y **se inventa nombres**. Suena profesional, es falso |
| **Con grounding** | Antes de responder, **abre el catálogo** y lee. Responde con hoteles, precios y disponibilidad reales |

El modelo no se volvió más listo. Le pusiste el catálogo delante.

---

## El patrón RAG — 3 pasos

| Paso | En cristiano | Nombre técnico |
| --- | --- | --- |
| 1 | Busca en tu fuente de datos lo relevante a la pregunta | **Retrieve** |
| 2 | Pega ese resultado dentro del prompt, como contexto | **Augment** |
| 3 | Manda el prompt ya enriquecido al modelo | **Generate** |

**Truco:** **R-A-G** = **R**ecuperar → **A**ñadir → **G**enerar. El nombre *es* el procedimiento.

Nada de esto reentrena el modelo. Es todo en tiempo de consulta (*at query time*).

---

## Embeddings — cómo se busca "por significado"

Un **embedding** es el texto convertido en un **vector** (una lista de números decimales) que captura su significado. Lo genera un **embedding model** (ej. un modelo de embeddings de Azure OpenAI en Microsoft Foundry).

Las dos frases del curso:

- *"The children played joyfully in the park."*
- *"Kids happily ran around the playground."*

**Cero palabras en común.** Pero sus vectores quedan **cerca** en el espacio multidimensional, porque significan casi lo mismo.

**Cosine similarity** = mide el **ángulo** entre dos vectores. Valor **cerca de 1 → muy parecidos**.

> Por eso el vector search encuentra el documento aunque el usuario no haya escrito ni una de sus palabras.

---

## Azure AI Search — el componente de retrieval

Es el servicio que hace el paso *Retrieve* en Foundry.

| Paso | Qué haces |
| --- | --- |
| 1. **Add your data** | Desde Azure Blob Storage, Azure Data Lake Storage Gen2 o Microsoft OneLake. También subir archivos directamente |
| 2. **Create an index** | Un embedding model vectoriza tu contenido. **El índice vive en Azure AI Search** |
| 3. **Query the index** | La pregunta se convierte a embedding, se busca lo más similar y se devuelven los resultados |

### Los 4 tipos de búsqueda — esto cae

| Tipo | Qué compara | Cuándo |
| --- | --- | --- |
| **Keyword search** | Términos exactos contra el texto del índice | Coincidencia literal |
| **Semantic search** | El **significado** de la consulta, con modelos semánticos | Reformulaciones |
| **Vector search** | **Embeddings** — similitud semántica por vectores | Sinónimos, otro idioma |
| **Hybrid search** | **Combina keyword + semantic + vector** | ⭐ **Recomendado para apps de IA generativa** |

**Regla de decisión:** ¿pregunta de examen sobre qué búsqueda usar en una app generativa? → **hybrid**. Es la respuesta por defecto y el curso lo dice explícito.

---

## RAG con el SDK de Azure AI Foundry

Se conecta el índice al modelo vía el proyecto de Foundry. `azure-ai-projects` te da un cliente OpenAI autenticado y usas la **Responses API**.

```python
from azure.ai.projects import AIProjectClient
from azure.identity import DefaultAzureCredential

project = AIProjectClient(
    endpoint=os.environ["PROJECT_ENDPOINT"],
    credential=DefaultAzureCredential(),
)

client = project.get_openai_client()

response = client.responses.create(
    model="gpt-4o",
    input=[
        {"role": "system", "content": "You are a helpful travel advisor. "
         "Use the following hotel data to answer: " + retrieved_context},
        {"role": "user", "content": "Which hotels do you offer in Paris?"},
    ],
)

print(response.output_text)
```

**Lo que importa del código:** `retrieved_context` son los documentos que devolvió tu índice de Azure AI Search, **inyectados en el system message**. Ahí está el *augment*. Nota que sigue el patrón de tu apunte [01](01-conectar-app-a-foundry.md): `DefaultAzureCredential`, sin keys.

---

## Cuándo usar RAG

| Situación | Por qué RAG |
| --- | --- |
| **Conocimiento de dominio privado** | Catálogo, políticas, base de conocimiento interna — el modelo nunca lo vio |
| **La información cambia seguido** | Inventario, precios, noticias. RAG trae el dato actual **sin reentrenar** |
| **La exactitud factual es crítica** | Respuestas ancladas en datos reales, no en conocimiento general |
| **Hay eventos posteriores al training cutoff** | El corte de entrenamiento no los incluye |

**Frontera con prompt engineering:** el prompt engineering **guía cómo responde**; no puede darle conocimiento que no tiene. Si el problema es *falta de datos* → RAG. Si es *formato o tono* → prompt engineering.

**Foundry IQ**: mencionado como alternativa gestionada — un *knowledge store* administrado para dar grounding a agentes sin montar tu propia infraestructura de búsqueda. Solo reconoce el nombre.

---

## Para el examen

**Alto valor (cae casi seguro):**
- Los **3 pasos de RAG**: retrieve → augment → generate.
- **Hybrid search es el recomendado** para apps de IA generativa.
- **Azure AI Search** = el servicio de retrieval; **ahí vive el índice**.
- **RAG vs fine-tuning**: RAG = falta *conocimiento*. Fine-tuning = falta *consistencia de comportamiento*.
- RAG **no reentrena nada** — todo ocurre en tiempo de consulta.

**Valor medio:**
- **Embedding** = texto → vector; **cosine similarity** cerca de 1 = parecidos.
- Las 3 fuentes de datos: **Blob Storage, ADLS Gen2, OneLake** (+ subida directa).
- Los 4 tipos de búsqueda y qué compara cada uno.
- Se necesita un **embedding model** desplegado, aparte del modelo de chat.

**Bajo valor:** los nombres de los diagramas del curso, el escenario de la agencia de viajes, y el detalle matemático del cálculo del coseno. **Foundry IQ**: solo reconocer el nombre.

---

## Comprueba que lo tienes

1. Tu app responde con nombres de productos que **no existen** en el catálogo de la empresa. Prompt engineering ya no ayuda. ¿Qué aplicas y por qué?
2. Un usuario busca *"calzado para correr"* pero tus documentos dicen *"zapatillas deportivas de running"*. ¿Qué tipo de búsqueda garantiza que lo encuentre, y qué tipo fallaría?
3. Vas a montar RAG para una app generativa en producción y el examen te da a elegir entre keyword, semantic, vector e hybrid. ¿Cuál eliges?
4. Tienes desplegado `gpt-4o` en tu proyecto de Foundry. ¿Basta para crear el índice de Azure AI Search? Si no, ¿qué falta?
5. Los precios de tu inventario cambian cada día. ¿RAG o fine-tuning? Explica en una línea.

<details>
<summary>Respuestas</summary>

1. **RAG (grounding)**. El prompt engineering guía *cómo* responde, pero no le puede dar conocimiento que no tiene. El modelo inventa porque le falta el dato; hay que recuperarlo de una fuente confiable e inyectarlo en el prompt.
2. **Vector search** (o **hybrid**, que lo incluye) lo encuentra: los embeddings capturan el significado aunque no coincida ninguna palabra. **Keyword search fallaría** — no hay término exacto en común.
3. **Hybrid search**. Combina keyword + semantic + vector y es el recomendado explícitamente para aplicaciones de IA generativa.
4. **No basta.** `gpt-4o` genera las respuestas, pero para crear el índice necesitas además un **embedding model** desplegado, que es el que vectoriza tu contenido.
5. **RAG.** Recupera el dato actual en tiempo de consulta; el fine-tuning congelaría los precios del día en que entrenaste y exigiría reentrenar en cada cambio.

</details>
