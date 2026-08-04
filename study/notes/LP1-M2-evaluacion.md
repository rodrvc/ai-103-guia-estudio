# Evaluación de modelos en Foundry

> LP1 · Módulo 2 · Unidad 5 — [Evaluate model performance](https://learn.microsoft.com/en-us/training/modules/model-catalog-evaluate/5-evaluate-performance?pivots=text)
> Cubre: **D2.1.d** · Cierra tu error **E-007**
> Fecha: 2026-08-04

---

## La idea que te faltaba

**Evaluación ≠ observabilidad.**

- **Observabilidad** = qué pasó (trazas, tokens, latencia).
- **Evaluación** = qué tan buena fue la respuesta.

En DIAG-1 respondiste "observabilidad" a la pregunta de métricas. Son cosas distintas.

---

## Tres formas de evaluar

| Forma | Qué es | Cuándo |
| --- | --- | --- |
| **Manual** | Humanos revisan respuestas | Captura lo subjetivo: tono, marca, contexto |
| **AI-assisted** | Un modelo GPT juzga a otro modelo | Escala. No necesita respuesta correcta previa |
| **NLP metrics** | Fórmula matemática | Necesita **ground truth**. Barata y determinista |

**Manual complementa, no reemplaza.** Lo automático no ve satisfacción del usuario ni alineación de marca.

---

## Las 4 métricas de calidad — memorízalas

> **Groundedness · Relevance · Coherence · Fluency**

| Métrica | Pregunta que responde |
| --- | --- |
| **Groundedness** | ¿Se apoya en el contexto dado, o se lo inventó? |
| **Relevance** | ¿Responde lo que se le preguntó? |
| **Coherence** | ¿Fluye lógicamente, sin contradecirse? |
| **Fluency** | ¿Está bien escrito? |

**Groundedness = anti-alucinación.** Es la respuesta a "¿cuál detecta fabricaciones?".

**Groundedness Pro** = variante binaria (grounded / not grounded). Para cuando necesitas un sí/no tajante sobre exactitud factual.

*Truco:* Groundedness mira **hacia el contexto**. Relevance mira **hacia la pregunta**.

---

## Métricas de riesgo y seguridad

Cuatro de daño — **las mismas 4 categorías de los content filters**:

- Self-harm · Hateful and unfair · Violent · Sexual

Más dos que no son categorías de daño:

- **Protected material** — ¿reprodujo contenido con copyright?
- **Indirect attack (jailbreak)** — ¿es vulnerable a manipulación?

### Defect rate

Cómo se agregan los resultados de seguridad:

- **Daño:** % de respuestas que superan un umbral de severidad (típicamente **Medium**).
- **Protected material e indirect attack:** `(instancias verdaderas / total) × 100`.

---

## Métricas NLP — necesitan ground truth

| Métrica | Compara | Fuerte en |
| --- | --- | --- |
| **F1-score** | Palabras compartidas (precision + recall) | Clasificación, information retrieval |
| **BLEU** | N-gramas contra referencia | **Traducción** |
| **METEOR** | BLEU + sinónimos, stemming, paráfrasis | Traducción, más flexible |
| **ROUGE** | Prioriza **recall** | **Resumen** (importa cubrir, no ser breve) |
| **GLEU** | Variante de BLEU | Nivel de **oración** |

**Regla de decisión:**
- ¿Hay una respuesta correcta única? → NLP metrics.
- ¿Generación abierta con muchas respuestas válidas? → AI-assisted.

*Trucos de memoria:* **R**OUGE → **R**ecall → resumen. BLEU → traducción. METEOR extiende BLEU.

---

## Crear una evaluación

**Tres bases posibles:**

| Base | Qué evalúa |
| --- | --- |
| **Model** | Un deployment. Genera las salidas durante la evaluación |
| **Agent** | Las respuestas de un agente |
| **Dataset** | Salidas **ya generadas** que están en el dataset |

**Tres orígenes de datos:** subir nuevo (CSV/JSONL) · usar uno existente · **generar dataset sintético** (describes el tema, el sistema crea filas).

El job corre **asíncrono**, fila por fila.

---

## Evaluator library

Ubicación central de evaluadores. Pestaña dentro de **Evaluation**.

- Evaluadores curados por Microsoft (calidad, seguridad, rendimiento)
- Los **annotation prompts** son visibles → puedes ver *cómo* se calcula cada métrica
- Evaluadores custom propios
- Versionado: comparar, restaurar, colaborar

---

## Qué hacer con los resultados

**Si la calidad es baja** — en orden de costo creciente:

1. **Prompt engineering** — lo más barato, empieza aquí
2. **Cambiar de modelo**
3. **RAG** — anclar en tus datos
4. **Fine-tuning** — lo más caro

**Si la seguridad preocupa:**

- Content filters (Azure AI Content Safety)
- Prompt hardening (instrucciones de seguridad en el system message)
- Validación de salida antes de mostrar

**Regla operativa:** establece el benchmark **temprano** y re-evalúa tras cada cambio. Sin línea base no sabes si mejoraste o rompiste algo.

---

## Para el examen

**Alto valor (cae casi seguro):**
- Groundedness detecta alucinaciones
- Las 4 de calidad: groundedness, relevance, coherence, fluency
- NLP metrics necesitan ground truth; AI-assisted no
- La escalera de mejora: prompt → modelo → RAG → fine-tuning

**Valor medio:**
- ROUGE → resumen, BLEU → traducción
- Defect rate y umbral Medium
- Model / Agent / Dataset como bases de evaluación

**Bajo valor:** detalles de la UI del portal.

---

## Comprueba que lo tienes

1. Tu RAG responde con datos que no están en los documentos recuperados. ¿Qué métrica lo detecta?
2. Evalúas un traductor y tienes las traducciones correctas de referencia. ¿AI-assisted o NLP? ¿Cuál?
3. ¿Por qué ROUGE encaja mejor que BLEU para resumen?
4. Los scores salen bajos. ¿Cuál es el primer arreglo a probar y por qué?

<details>
<summary>Respuestas</summary>

1. **Groundedness** (o Groundedness Pro si quieres binario).
2. **NLP** — tienes ground truth. **BLEU**, o METEOR si quieres tolerar sinónimos.
3. ROUGE prioriza **recall**: en un resumen importa cubrir los puntos clave, no evitar palabras de más.
4. **Prompt engineering** — el más barato. La escalera sube en costo: prompt → modelo → RAG → fine-tuning.

</details>
