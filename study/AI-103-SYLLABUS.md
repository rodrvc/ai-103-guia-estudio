# AI-103 — Matriz del temario oficial

> **Qué contiene:** los 44 objetivos oficiales con peso, nivel y evidencia. **Fuente canónica de los niveles.**
> **Cuándo leerlo:** al actualizar un nivel (solo el dominio tocado) · al planificar (solo § Resumen de pesos) · al verificar vigencia (solo las cabeceras de fecha).
> **Cuándo NO:** entero. Son 188 líneas y casi nunca necesitas todas.
>
> **Integridad al escribir:** un nivel solo sube con **evidencia registrada** (quiz, ejercicio, lab). Leer una lección no cuenta. Citar siempre la evidencia en la celda. Escala en `CLAUDE.md`.

- **Fuente:** https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103
- **Skills measured as of:** 2026-04-16
- **Última verificación por un agente:** 2026-08-04
- **Nota oficial:** la mayoría de preguntas cubren features GA; puede haber Preview si son de uso común.
- **Aprobación:** 700/1000.

Escala de niveles: ver `CLAUDE.md` § Escala de niveles.

**Progreso de evaluación:** 11 de 44 objetivos evaluados (25%) tras DIAG-1 el 2026-08-04. El resto sigue **No evaluado**.

---

## Resumen de pesos

| # | Dominio | Peso | Nivel agregado | Riesgo estimado |
| --- | --- | --- | --- | --- |
| D1 | Plan and manage an Azure AI solution | 25–30% | **Débil** (6/16 evaluados) | **Crítico** — confirmado por DIAG-1 |
| D2 | Implement generative AI and agentic solutions | 30–35% | **En aprendizaje** (5/16 evaluados) | **Alto** — confirmado: conceptos sí, operación Azure no |
| D3 | Implement computer vision solutions | 10–15% | No evaluado | **Alto** (sin experiencia declarada) |
| D4 | Implement text analysis solutions | 10–15% | No evaluado | **Alto** (voz sin experiencia declarada) |
| D5 | Implement information extraction solutions | 10–15% | No evaluado | Medio-alto (RAG fuerte, AI Search/CU a validar) |

> **D1 y D2 ya no son hipótesis:** los niveles y riesgos vienen de DIAG-1 (2026-08-04, 42%). D3/D4/D5 siguen siendo hipótesis del perfil, pendientes de DIAG-2.

---

## D1 — Plan and manage an Azure AI solution (25–30%)

### D1.1 Choose the appropriate Foundry services for generative AI and agents

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D1.1.a | Choose an appropriate model for each task, including LLMs, small language models, multimodal models, and Foundry Tools | Evaluado | **En aprendizaje** | DIAG-1 p2 parcial: criterio válido pero sin eje costo/latencia/edge. E-008 |
| D1.1.b | Choose the appropriate Foundry services for generative tasks, grounding, vector search, agent workflows, or multimodal processing | Evaluado | **En aprendizaje** | DIAG-1 p1 pleno (Foundry IQ + SharePoint). Nivel limitado por E-005 (retrieval) y E-006 (tools): acierta el servicio, no sus capas |
| D1.1.c | Choose an appropriate method for retrieval and indexing | Pendiente | No evaluado | — |
| D1.1.d | Choose appropriate memory, tool, and knowledge integration services for agent solutions | Pendiente | No evaluado | — |

### D1.2 Set up AI solutions in Foundry

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D1.2.a | Design Azure infrastructure for AI apps and agent-based solutions | Pendiente | No evaluado | — |
| D1.2.b | Choose appropriate deployment options | Estudiado, **sin evaluar** | No evaluado | Apunte `notes/D1-deployment-types.md` (2026-08-04, docs oficiales). Cubre los 10 deployment types, residencia de datos y SLA. **Leer no es evidencia** — pendiente quiz |
| D1.2.c | Configure model and agent deployments | Evaluado | **Débil** | DIAG-1 p3 fallo: confunde deployment con playground. E-001 |
| D1.2.d | Integrate Foundry projects with CI/CD pipelines | Pendiente | No evaluado | — |

### D1.3 Manage, monitor, and secure AI systems

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D1.3.a | Manage quotas, scaling, rate limits, and cost footprints for model and agent workloads | Evaluado | **Débil** | DIAG-1 p4: "no lo sé". Sin TPM/PTU/429. E-002 |
| D1.3.b | Monitor model performance, drift, safety events, and grounding quality | Pendiente | No evaluado | — |
| D1.3.c | Monitor data ingestion quality, search index health, and relevance performance | Pendiente | No evaluado | — |
| D1.3.d | Configure security: managed identity, private networking, keyless credentials, role policies | Evaluado | **Débil** | DIAG-1 p5 parcial-bajo: intuye Azure, no nombra Managed Identity / RBAC / DefaultAzureCredential. E-003 |

### D1.4 Implement responsible AI across generative AI and agentic systems

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D1.4.a | Configure safety filters, guardrails, risk detection, and content moderation | Evaluado | **En aprendizaje** | DIAG-1 p7 pleno (filter vs blocklist) pero p6 fallo: no conoce las 4 categorías ni las 4 severidades. E-004 |
| D1.4.b | Apply responsible AI instrumentation: evaluators, safety evaluations, explanation tooling | Pendiente | No evaluado | — |
| D1.4.c | Implement auditing through trace logging, provenance metadata, approval workflows | Pendiente | No evaluado | — |
| D1.4.d | Govern agent behavior with oversight modes, constraints, and tool-access controls | Pendiente | No evaluado | — |

---

## D2 — Implement generative AI and agentic solutions (30–35%)

### D2.1 Build generative applications by using Foundry

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D2.1.a | Deploy and consume LLMs, small models, code models, and multimodal models | Pendiente | No evaluado | — |
| D2.1.b | Implement retrieval-augmented generation (RAG) in an application | Evaluado | **En aprendizaje** | DIAG-1 p8 parcial: **invierte** vectorial/híbrida, omite BM25, RRF y semantic ranker. Sabe RAG en la práctica, no la terminología AI Search. E-005 |
| D2.1.c | Design workflows, tool-augmented flows, and multistep reasoning pipelines | Pendiente | No evaluado | — |
| D2.1.d | Evaluate models and apps: fabrications, relevance, quality, safety | Evaluado | **Débil** | DIAG-1 p12 fallo: confunde evaluación con observabilidad, no conoce groundedness. Llamativo por su experiencia en LangSmith. E-007 |
| D2.1.e | Integrate generative workflows into applications by using Foundry SDKs and connectors | Pendiente | No evaluado | — |
| D2.1.f | Configure an application to connect to a Foundry project | Pendiente | No evaluado | — |

### D2.2 Build agents by using Foundry

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D2.2.a | Define agent roles, goals, conversation-tracking approach, and tool schemas | Pendiente | No evaluado | — |
| D2.2.b | Build agents that integrate retrieval, function-calling, and conversation memory | Evaluado | **Competente** | DIAG-1 p10 pleno: thread/run/message correctos y acierta lo clave — **el servicio persiste el thread**, no la app. Único objetivo en Competente |
| D2.2.c | Integrate agent tools: APIs, knowledge stores, search, content understanding, custom functions | Evaluado | **En aprendizaje** | DIAG-1 p9 parcial: catálogo incompleto, confunde modalidad (voz) con tool. E-006 |
| D2.2.d | Implement orchestrated multi-agent solutions | Pendiente | No evaluado | — |
| D2.2.e | Build autonomous or semiautonomous workflows with safeguards and approval flow controls | Evaluado | **En aprendizaje** | DIAG-1 p11: patrón human-in-the-loop correcto, falta la mecánica `requires_action` → `submit_tool_outputs`. E-009 |
| D2.2.f | Integrate monitoring into deployed agents, evaluate agent behavior, perform error analysis | Pendiente | No evaluado | — |

### D2.3 Optimize and operationalize generative AI systems

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D2.3.a | Tune generation behavior: prompt engineering, model parameters | Pendiente | No evaluado | — |
| D2.3.b | Implement model reflection, chain-of-thought evaluations, self-critique loops | Pendiente | No evaluado | — |
| D2.3.c | Set up observability: tracing, token analytics, safety signals, latency breakdowns | Pendiente | No evaluado | — |
| D2.3.d | Orchestrate multiple models, flows, or hybrid LLM and rules engines | Pendiente | No evaluado | — |

---

## D3 — Implement computer vision solutions (10–15%)

### D3.1 Design and implement image- and video-generation solutions

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D3.1.a | Generate images from text prompts and reference media | Pendiente | No evaluado | — |
| D3.1.b | Generate videos from text prompts and reference media | Pendiente | No evaluado | — |
| D3.1.c | Configure image-editing workflows: inpainting, mask-based edits, prompt-driven modifications | Pendiente | No evaluado | — |
| D3.1.d | Implement workflows to edit generated videos | Pendiente | No evaluado | — |
| D3.1.e | Select and apply appropriate generation and editing controls provided by the platform | Pendiente | No evaluado | — |

### D3.2 Design and implement multimodal understanding workflows

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D3.2.a | Analyze visual context by using multimodal models | Pendiente | No evaluado | — |
| D3.2.b | Produce concise or detailed captions for single or multiple images | Pendiente | No evaluado | — |
| D3.2.c | Question-answering grounded in visual evidence | Pendiente | No evaluado | — |
| D3.2.d | Generate alt-text and extended image descriptions per accessibility guidelines | Pendiente | No evaluado | — |
| D3.2.e | Visual understanding via Azure Content Understanding in Foundry Tools (extract visual characteristics) | Pendiente | No evaluado | — |
| D3.2.f | Video analysis workflows to process and interpret video segments | Pendiente | No evaluado | — |
| D3.2.g | Configure single-task and pro-mode Content Understanding pipelines | Pendiente | No evaluado | — |
| D3.2.h | Identify objects, components, or regions within images or video | Pendiente | No evaluado | — |

### D3.3 Implement responsible AI for multimodal content

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D3.3.a | Filters to classify unsafe or disallowed visual content | Pendiente | No evaluado | — |
| D3.3.b | Detect and mitigate indirect prompt injection via embedded text in images | Pendiente | No evaluado | — |
| D3.3.c | Enforce visual policy rules: watermarks, prohibited symbols, brand usage, inappropriate content | Pendiente | No evaluado | — |

---

## D4 — Implement text analysis solutions (10–15%)

### D4.1 Apply language model text analysis

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D4.1.a | Extract entities, topics, summaries, structured JSON via generative prompting and Foundry Tools | Pendiente | No evaluado | — |
| D4.1.b | Configure detection of sentiment, tone, safety issues, sensitive content | Pendiente | No evaluado | — |
| D4.1.c | Translate text via Azure Translator in Foundry Tools or LLM-powered translation flows | Pendiente | No evaluado | — |
| D4.1.d | Customize outputs for domain tasks: compliance summarization, domain extraction | Pendiente | No evaluado | — |

### D4.2 Implement speech solutions

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D4.2.a | Speech-to-text and text-to-speech workflows for agentic interactions | Pendiente | No evaluado | — |
| D4.2.b | Integrate speech as an agent modality, including custom speech models | Pendiente | No evaluado | — |
| D4.2.c | Enable multimodal reasoning from audio inputs | Pendiente | No evaluado | — |
| D4.2.d | Translate speech into other languages using language models and Foundry Tools | Pendiente | No evaluado | — |

---

## D5 — Implement information extraction solutions (10–15%)

### D5.1 Build retrieval and grounding pipelines

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D5.1.a | Ingest and index content: documents, images, audio, video | Pendiente | No evaluado | — |
| D5.1.b | Configure semantic search, hybrid search, and vector search for grounding | Pendiente | No evaluado | — |
| D5.1.c | Implement enrichment using custom or built-in skills for text, images, layout | Pendiente | No evaluado | — |
| D5.1.d | Configure RAG ingestion flow, including documents and OCR | Pendiente | No evaluado | — |
| D5.1.e | Connect retrieval pipelines directly to workflows and agent tools | Pendiente | No evaluado | — |

### D5.2 Extract content from documents

| ID | Objetivo oficial | Estado | Nivel | Evidencia |
| --- | --- | --- | --- | --- |
| D5.2.a | Extract info via multimodal pipelines combining OCR, layout analysis, field extraction | Pendiente | No evaluado | — |
| D5.2.b | Produce clean, grounded representations for agents and RAG using Content Understanding | Pendiente | No evaluado | — |
| D5.2.c | Implement analyzers for structured or markdown outputs using Content Understanding | Pendiente | No evaluado | — |

---

## Historial de verificación del temario

| Fecha | Agente | Resultado |
| --- | --- | --- |
| 2026-08-04 | Claude | Temario leído de la fuente oficial. "Skills measured as of April 16, 2026". Sin aviso de actualización pendiente en la página. Matriz creada con 5 dominios y 44 objetivos. |
| 2026-08-04 | Claude | El usuario aportó la URL `es-es`. Verificada: **misma página, mismo `git_commit_id` 8e58c0d8, misma fecha, mismos pesos**. Pero es traducción automática (`ms.translationtype: MT`) con errores de terminología relevantes — el peor: "modelos multiplataforma" por *multimodal*. Se mantiene `en-us` como fuente única. Regla de terminología en inglés añadida a `CLAUDE.md`. |
