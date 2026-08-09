# Labs oficiales: lo que se aprende haciéndolos

> Labs de [mslearn-ai-studio](https://microsoftlearning.github.io/mslearn-ai-studio/) · 02 (model catalog), 03 (foundry SDK), 04a (use own data)
> Cubre: **D2.1.a**, **D2.1.d**, **D2.1.e**
> Peso: **medio** — complemento práctico. Los conceptos que rescata sí caen; los pasos de los labs, no
> Fecha: 2026-08-05

---

## Lab 02 — Explorar y comparar modelos

### El leaderboard: cómo se elige un modelo

Compara con **trade-off charts** en cuatro ejes:

| Eje | Mide |
| --- | --- |
| **Quality** | Rendimiento en benchmarks |
| **Cost** | Precio por token |
| **Throughput** | Velocidad |
| **Safety** | Comportamiento en pruebas de seguridad |

Más, en cada model card: **context window** y **training date**.

**No existe "el mejor modelo".** Existe el mejor para tu restricción. Por eso el lab compara `gpt-5.2` contra `gpt-5-mini`.

### ⚠️ Benchmarks ≠ tu evaluación

| | Qué te dice | Para qué |
| --- | --- | --- |
| **Benchmarks** | Rendimiento en pruebas públicas estándar | **Preseleccionar** |
| **Evaluación** | Rendimiento con **tus datos y tu prompt** | **Decidir** |

Un modelo puede liderar el leaderboard y rendir mal en tu caso. Por eso el lab hace las dos, en ese orden.

### Los 3 orígenes de datos de prueba

| Opción | Cuándo |
| --- | --- |
| **Upload new dataset** | Ya tienes casos en CSV o JSONL |
| **Use existing dataset** | Ya subiste uno antes |
| **Generate synthetic dataset** | **No tienes datos** ← la del lab |

**Generación sintética** — le das tres cosas:

1. **El recurso que genera** — un deployment que escribe los casos. **No es el que evalúas**
2. **Número de filas** — el lab pide 45
3. **El prompt de generación** — qué quieres

Opcionalmente subes archivos para que los casos sean relevantes a tu dominio.

### 🎯 Lo más importante del lab

El prompt de generación del lab dice:

> *"Create various travel related questions, **and include some content safety and security tests**"*

Pide **deliberadamente casos hostiles**: jailbreaks, contenido inapropiado. No solo "¿qué visito en Roma?".

**Por qué:** un dataset amable te da un score alto y cero información. Solo descubres dónde se rompe tu app si el set *intenta* romperla. Igual que un test suite: los casos felices no encuentran bugs.

### Dos prompts que se confunden

| Prompt | Para qué |
| --- | --- |
| **De generación** | Fabrica los casos de prueba |
| **Developer prompt** (system) | Instruye al modelo **que evalúas** |

El primero construye el examen; el segundo define al alumno. Confundirlos = evaluar otra cosa.

### Leer resultados

El lab pide **analizar los fallos agrupados por categoría**, no mirar el promedio. Un 85% global puede esconder que el 100% de los fallos son de groundedness en preguntas de precios.

Y **revisa el dataset generado**: si las 45 filas son variaciones de "¿qué hago en París?", la evaluación no vale nada aunque el score salga bonito.

---

## Lab 03 — Chat app con el SDK

### Cuatro detalles prácticos

**1. Usa el Azure OpenAI endpoint**, no el project endpoint. La app solo chatea: no necesita agentes ni evaluaciones.

**2. El `.env` no lleva API key:**

```
AZURE_OPENAI_ENDPOINT=...
MODEL_DEPLOYMENT=...
```

Con Entra ID no hay secreto que guardar. Y ⚠️ `MODEL_DEPLOYMENT` es **el nombre de tu deployment**, no el del modelo del catálogo — aquí se ve en la práctica esa distinción.

**3. `az login` antes de correr.** Tu PC no está en Azure; `DefaultAzureCredential` usa tu sesión del CLI.

**4. Python 3.13, no 3.14** — el lab avisa que 3.14 rompe la compilación de dependencias.

### El orden es pedagógico

Escribes lo mismo cuatro veces, cada vez mejor:

1. **ChatCompletions** → funciona, sin memoria
2. **Responses** → más limpio
3. **`previous_response_id`** → ahora recuerda
4. **Streaming** → ya no parece congelado

Se **siente** la diferencia entre las dos APIs, no solo se lee.

### La versión async (`chat-async.py`)

```python
from azure.identity.aio import DefaultAzureCredential   # ← .aio
from openai import AsyncOpenAI

# al terminar:
await credential.close()
```

Dos detalles fáciles de olvidar: el import `.aio` y **cerrar la credencial**.

---

## Regla de oro de los labs

**Borra el resource group al terminar.** Dejas deployments corriendo y cuestan.

---

## Para el examen

**Alto valor:**
- Leaderboard: **quality / cost / throughput / safety** como ejes de decisión
- **Benchmarks ≠ evaluación con tus datos**
- **Dataset sintético** cuando no hay datos de prueba
- Incluir **casos de seguridad** en el set de prueba
- Analizar fallos **por categoría**, no el promedio

**Valor medio:** comparación lado a lado en playground · Model vs Agent vs Dataset como base de evaluación · Azure OpenAI endpoint para inferencia pura.

**Bajo valor:** clics del portal, nombres de versiones, el repo del lab.

---

## Comprueba que lo tienes

1. `gpt-5.2` lidera en quality. Tu app clasifica tickets: alto volumen, respuestas de una palabra. ¿Lo eliges?
2. ¿Por qué el prompt de generación pide casos de seguridad?
3. Tu evaluación da 85% de relevance promedio. ¿Por qué no basta?
4. ¿Qué diferencia hay entre el *prompt de generación* y el *developer prompt*?
5. Tu `.env` tiene endpoint y deployment, pero ninguna key. ¿Está mal configurado?

<details>
<summary>Respuestas</summary>

1. **No.** Quality lidera, pero tu restricción es **costo y latencia** con tareas triviales. Un modelo pequeño rinde igual por mucho menos. Ese es el trade-off del leaderboard.
2. Porque un dataset de preguntas amables da un score alto y cero información. Solo descubres dónde se rompe la app si el set intenta romperla.
3. Porque el **promedio esconde los clusters**. Puede que todos los fallos estén concentrados en un tipo de pregunta. Hay que mirar los fallos por categoría.
4. El **de generación** fabrica los casos de prueba. El **developer prompt** es el system prompt del modelo que evalúas. Uno construye el examen, el otro define al alumno.
5. **Está bien.** Con Entra ID (`DefaultAzureCredential`) no hay key que guardar — ese es justamente el objetivo.

</details>
