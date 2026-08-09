# Material de estudio AI-103

Apuntes para preparar la certificación **Microsoft AI-103** (*Developing AI Apps and Agents on Azure*).

Cada apunte es autocontenido y sigue la misma estructura:

> **En cristiano** (analogía y ejemplo concreto) → tablas de contenido → **Para el examen** (alto / medio / bajo valor) → **Comprueba que lo tienes** (preguntas con respuestas desplegables)

**Cómo usarlo:** ábrelo y estudia · pídele a un agente *"repasemos deployment types"* y te pregunta · arrastra la carpeta a NotebookLM, Obsidian o imprímela.

> ⚠️ Material de estudio no oficial, escrito a partir de la documentación pública de Microsoft Learn. La fuente de verdad es siempre [la guía oficial del examen](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103).

---

## Por dónde empezar

La numeración es por dominio, no por orden de lectura. Esta es la ruta recomendada:

| # | Apunte | Por qué aquí |
| --- | --- | --- |
| 1 | [D1/00 Fundamentos](D1-plataforma/00-fundamentos-foundry.md) | El vocabulario que todo lo demás da por sabido |
| 2 | [D1/01 Deployment types](D1-plataforma/01-deployment-types.md) | Qué despliegas y con qué restricciones |
| 3 | [D2/01 Conectar tu app](D2-apps-y-agentes/01-conectar-app-a-foundry.md) | Endpoints, SDKs y autenticación |
| 4 | [D2/03 Playground](D2-apps-y-agentes/03-playground.md) | Probar sin código (corto) |
| 5 | [D2/02 Evaluación](D2-apps-y-agentes/02-evaluacion-de-modelos.md) | Cómo se mide si un modelo sirve |
| 6 | [D2/06 RAG y grounding](D2-apps-y-agentes/06-rag-grounding.md) | Darle tus datos al modelo |
| 7 | [D2/04 Agentes](D2-apps-y-agentes/04-agentes-en-foundry.md) | Qué es un agente y sus tipos |
| 8 | [D2/07 Construir y publicar](D2-apps-y-agentes/07-construir-y-publicar-agentes.md) | Ciclo de vida completo — **depende del 04** |
| 9 | [D1/02 IA responsable](D1-plataforma/02-ia-responsable.md) | Filtros, riesgos y salida a producción |
| — | [D2/05 Labs oficiales](D2-apps-y-agentes/05-labs-oficiales.md) | Complemento práctico, en cualquier momento |

---

## D1 — Plataforma: planificar, desplegar, asegurar
*Peso en el examen: 25–30%*

| Apunte | De qué va |
| --- | --- |
| [00 Fundamentos](D1-plataforma/00-fundamentos-foundry.md) | Recurso → proyecto → **deployment** → endpoint · **modelo ≠ deployment** · los dos endpoints · qué es RBAC |
| [01 Deployment types](D1-plataforma/01-deployment-types.md) | Global / Data Zone / Regional × Standard / Provisioned / Batch · residencia de datos · SLA · cuándo elegir cada uno |
| [02 IA responsable](D1-plataforma/02-ia-responsable.md) | **Map → Measure → Mitigate → Manage** · las **4 capas de mitigación** · content filters y prompt shields · red teaming · plan de salida |

**Sin cubrir en D1:** cuotas y rate limits (TPM/PTU, 429) · CI/CD con proyectos Foundry · redes privadas · monitoreo de índices.

---

## D2 — Apps generativas y agentes
*Peso en el examen: 30–35% — el dominio más pesado*

| Apunte | De qué va |
| --- | --- |
| [01 Conectar tu app a Foundry](D2-apps-y-agentes/01-conectar-app-a-foundry.md) | Endpoints · Foundry SDK vs OpenAI SDK · **autenticación** (Managed Identity / Service Principal / `az login`) · Responses vs ChatCompletions · async |
| [02 Evaluación de modelos](D2-apps-y-agentes/02-evaluacion-de-modelos.md) | Manual / AI-assisted / métricas NLP · **groundedness** = anti-alucinación · evaluator library |
| [03 El playground](D2-apps-y-agentes/03-playground.md) | Probar sin código · el botón Code · **playground ≠ deployment** |
| [04 Agentes en Foundry](D2-apps-y-agentes/04-agentes-en-foundry.md) | Declarativo (prompt-based / workflow YAML) vs hosted · **automatic tool calling** · 8 riesgos de seguridad y mitigaciones |
| [05 Labs oficiales](D2-apps-y-agentes/05-labs-oficiales.md) | Leaderboard y trade-offs · benchmarks ≠ evaluación · **dataset sintético** · el `.env` sin key |
| [06 RAG y grounding](D2-apps-y-agentes/06-rag-grounding.md) | Retrieve → Augment → Generate · embeddings y cosine similarity · Azure AI Search · **hybrid search** · RAG vs fine-tuning |
| [07 Construir y publicar agentes](D2-apps-y-agentes/07-construir-y-publicar-agentes.md) | Portal vs VS Code · **YAML del agente** · catálogo de tools y MCP · **Deploy vs Publish** · Agent Application, Entra y RBAC |

**Sin cubrir en D2:** las 4 tools en detalle (`code_interpreter`, `web_search`, `file_search`, `function`) · multi-agente y orquestación · observabilidad y tracing.

---

## D3 — Visión · D4 — Texto y voz · D5 — Extracción documental
*Peso combinado: 30–45%*

**Sin material todavía.** Es el hueco principal de esta guía.

---

## Confusiones frecuentes — dónde se resuelven

Los pares que más se confunden en el examen, y el apunte que los distingue:

| Se confunde | Con | Apunte |
| --- | --- | --- |
| Modelo | Deployment | [D1/00](D1-plataforma/00-fundamentos-foundry.md) |
| Playground | Deployment | [D2/03](D2-apps-y-agentes/03-playground.md) |
| Project endpoint | Azure OpenAI endpoint | [D1/00](D1-plataforma/00-fundamentos-foundry.md) · [D2/01](D2-apps-y-agentes/01-conectar-app-a-foundry.md) |
| API key | Managed Identity / Service Principal | [D2/01](D2-apps-y-agentes/01-conectar-app-a-foundry.md) |
| TPM (standard) | PTU (provisioned) | [D1/01](D1-plataforma/01-deployment-types.md) |
| Content filter | Blocklist | [D1/02](D1-plataforma/02-ia-responsable.md) |
| Categorías graduadas | Filtros binarios (Prompt Shields, PII…) | [D1/02](D1-plataforma/02-ia-responsable.md) |
| File Search | Azure AI Search | [D2/07](D2-apps-y-agentes/07-construir-y-publicar-agentes.md) |
| Búsqueda vectorial | Híbrida y semantic ranker | [D2/06](D2-apps-y-agentes/06-rag-grounding.md) |
| Evaluadores | Observabilidad / tracing | [D2/02](D2-apps-y-agentes/02-evaluacion-de-modelos.md) |
| Deploy | Publish | [D2/07](D2-apps-y-agentes/07-construir-y-publicar-agentes.md) |
| Declarative agent | Hosted agent | [D2/04](D2-apps-y-agentes/04-agentes-en-foundry.md) |

---

## Cobertura

**9 apuntes ≈ 13 de los 44 objetivos del temario (~30%).** D3, D4 y D5 sin material.

Detalle objetivo por objetivo: [`../AI-103-SYLLABUS.md`](../AI-103-SYLLABUS.md).

---

## Para agentes

Lee **este índice**, no el directorio. Abre un apunte solo si el usuario lo nombra o vas a actualizarlo. Plantilla: `_TEMPLATE.md`. Un apunte cubre un **tema**, no una unidad suelta del curso.
