# AI-103 — Estado del estudio

> **Resumen ejecutivo.** La evidencia detallada vive en `AI-103-SYLLABUS.md`, `AI-103-ERROR-LOG.md`, `AI-103-PRACTICE.md` y `AI-103-SESSION-LOG.md`. El currículo de contenido está en `AI-103-LEARNING-PATH.md`.

- **Última actualización:** 2026-08-04
- **Fase actual:** DIAG-1 completado (42%) → **estudio del bloque B1 (D1 operativo)**
- **Examen reservado:** No
- **Fecha objetivo:** Sin definir (falta dato del usuario)

---

## Progreso global

| Métrica | Valor | Base |
| --- | --- | --- |
| Objetivos del temario | 44 | `AI-103-SYLLABUS.md` |
| Evaluados | 11 (25%) | DIAG-1 |
| Competente o superior | 1 (2%) | D2.2.b |
| Simulaciones completas hechas | 0 | — |
| Mejor puntaje parcial | **42%** (DIAG-1, D1+D2) | `AI-103-PRACTICE.md` |
| Módulos del curso oficial | 0 / 30 (0%) | `AI-103-LEARNING-PATH.md` |

**Nivel estimado actual: En aprendizaje, por debajo del umbral de aprobación.** DIAG-1 dio 42% sobre los dominios que pesan 55–65% del examen; se aprueba con 700/1000 (~70%). La brecha es real pero de **superficie, no de fundamentos** (ver § Lectura del diagnóstico).

---

## Nivel por dominio

| Dominio | Peso | Nivel | Evidencia |
| --- | --- | --- | --- |
| D1 Plan and manage an Azure AI solution | 25–30% | **Débil** | DIAG-1: 3 fallos de 6 objetivos evaluados. Prioridad #1 |
| D2 Implement generative AI and agentic solutions | 30–35% | **En aprendizaje** | DIAG-1: conceptos sólidos, mecánica Azure floja |
| D3 Implement computer vision solutions | 10–15% | No evaluado | Pendiente DIAG-2 |
| D4 Implement text analysis solutions | 10–15% | No evaluado | Pendiente DIAG-2 |
| D5 Implement information extraction solutions | 10–15% | No evaluado | Pendiente DIAG-2. Ojo: E-005 sugiere brecha en AI Search |

---

## Lectura del diagnóstico (2026-08-04)

**El patrón es nítido y se repite en las 12 preguntas.**

Lo que acertó — Foundry IQ con fuentes externas, filter vs blocklist, el modelo thread/run/message con persistencia del lado del servicio, el patrón de aprobación humana — es todo **conceptual y de diseño de sistemas**.

Lo que falló — deployments, TPM/PTU, Managed Identity, categorías de content filter, las capas de retrieval de AI Search, el catálogo de tools, los evaluadores — es todo **operativo, nombrado y específico de Azure**.

**Conclusión:** piensa como arquitecto de sistemas de IA; le falta el vocabulario de operación de la plataforma. Eso es una brecha de **superficie**, no de fundamentos: se cierra memorizando nombres, límites y mecánicas, no reaprendiendo conceptos. Pero es **exactamente lo que el examen mide**.

**Señal positiva:** cero errores de lectura o de descarte en DIAG-1. No hay problema de técnica de examen ni de sobreconfianza — dijo "no lo sé" tres veces en vez de inventar. Eso hace el diagnóstico fiable y acelera la recuperación.

**Dos errores especialmente reveladores:**

- **E-005** (invierte vectorial/híbrida): usa RAG en producción pero no maneja la terminología de Azure AI Search. Es el arquetipo de R7.
- **E-007** (confunde evaluación con observabilidad): usa LangSmith a diario. El concepto le es familiar; el encuadre y los nombres de Azure no.

---

## Hipótesis inicial (NO es evidencia)

Derivada del perfil declarado por el usuario el 2026-08-04. Debe confirmarse o refutarse con el diagnóstico. **No usar para asignar niveles.**

### Probables fortalezas transferibles

- Conceptos de RAG, chunking, embeddings, búsqueda vectorial → **D2.1.b, D5.1.b**
- Diseño de agentes: herramientas, memoria, orquestación multi-agente (LangGraph) → **D2.2.a–e**
- Evaluación y observabilidad de LLMs (LangSmith) → **D2.1.d, D2.3.c, D2.2.f**
- Prompt engineering y tuning de parámetros → **D2.3.a**
- Consumo de APIs de modelos, Python, integración en backend → **D2.1.a, D2.1.e**

### Probables riesgos principales

| Riesgo | Dominios | Por qué |
| --- | --- | --- |
| **R1. Vocabulario y arquitectura Microsoft Foundry** | D1, D2 (55–65% del examen) | Toda la experiencia es fuera de Azure. El examen pregunta por nombres, límites y decisiones de servicios Foundry concretos, no por conceptos genéricos de IA. Riesgo más alto del proyecto. **Confirmado al revisar el curso oficial (2026-08-04):** el temario asume Responses API, Foundry IQ, Microsoft Agent Framework, A2A, MCP servers de Azure, Work IQ, Sora 2, Content Understanding — nomenclatura reciente y propia de Microsoft, sin equivalente 1:1 en LangChain/LangGraph. |
| **R2. Seguridad e identidad de Azure** | D1.3.d | Managed identity, keyless credentials, RBAC, private endpoints. Sin experiencia declarada en Azure. |
| **R3. Visión y generación de imagen/video** | D3 (10–15%) | Sin experiencia declarada. Dominio completo en riesgo. |
| **R4. Voz (Speech)** | D4.2 | Sin experiencia declarada. |
| **R5. Azure AI Search específico** | D5.1 | Sabe búsqueda vectorial en general, pero no la implementación de AI Search: skillsets, indexers, semantic ranker, híbrida + RRF. |
| **R6. Content Understanding** | D3.2.e/g, D5.2 | Servicio Azure sin equivalente directo en su stack. Aparece en 3 dominios distintos. |
| **R7. Falsa confianza por transferencia** | D2 | El mayor riesgo silencioso: asumir que "sé RAG y agentes" equivale a saber cómo AI-103 lo pregunta en Foundry. Requiere verificación explícita, no autoevaluación. |
| **R8. El curso oficial no cubre todo D1** | D1.2.d, D1.3.a, D1.3.c, D1.3.d | Detectado el 2026-08-04 al mapear los 30 módulos contra los 44 objetivos. CI/CD, cuotas/costos, salud de índices y seguridad de red quedan fuera o de pasada. Completar solo el curso deja descubierto un trozo del segundo dominio más pesado. Requiere estudio dirigido de docs. Ver `AI-103-LEARNING-PATH.md` § Cobertura y huecos. |

---

## Temas fuertes (con evidencia)

| Tema | Nivel | Evidencia |
| --- | --- | --- |
| Ciclo de vida de conversación de agentes (thread/run/message, persistencia en el servicio) | Competente | DIAG-1 p10 pleno |
| Foundry IQ y conexión de fuentes de datos empresariales (SharePoint, etc.) | En aprendizaje | DIAG-1 p1 pleno |
| Content filter vs blocklist | En aprendizaje | DIAG-1 p7 pleno |
| Patrón human-in-the-loop en agentes (a nivel de diseño) | En aprendizaje | DIAG-1 p11 pleno en concepto |

## Temas débiles (con evidencia)

Ordenados por prioridad = peso en el examen × severidad de la brecha.

| # | Tema | Objetivo | Evidencia | Prioridad |
| --- | --- | --- | --- | --- |
| 1 | Managed Identity, RBAC, keyless auth | D1.3.d | E-003 | **Crítica** — muy preguntado, cero base |
| 2 | Deployment vs model; qué se pasa a la API | D1.2.c | E-001 | **Crítica** — concepto de base de toda la plataforma |
| 3 | Cuotas: TPM, PTU, 429, backoff | D1.3.a | E-002 | **Crítica** — cero base |
| 4 | Capas de retrieval en AI Search: BM25 / vector / híbrida+RRF / semantic ranker | D2.1.b, D5.1.b | E-005 | **Alta** — concepto invertido, afecta 2 dominios |
| 5 | Evaluadores de Foundry, groundedness | D2.1.d | E-007 | **Alta** |
| 6 | Categorías y severidades de content filter | D1.4.a | E-004 | **Alta** — memorización pura, barata de cerrar |
| 7 | Catálogo de tools del Agent Service | D2.2.c | E-006 | Media |
| 8 | Mecánica `requires_action` / `submit_tool_outputs` | D2.2.e | E-009 | Media |
| 9 | Criterios LLM vs SLM (costo, latencia, edge) | D1.1.a | E-008 | Media |

---

## Repaso programado (repetición espaciada)

Los 9 errores de DIAG-1 entran en la cola con el mismo calendario base. Un repaso fallido reinicia el contador.

| Error | Tema | Nivel | 1d | 3d | 7d | 14d | 30d |
| --- | --- | --- | --- | --- | --- | --- | --- |
| E-001 | Deployment vs model | Débil | 08-05 ☐ | 08-07 ☐ | 08-11 ☐ | 08-18 ☐ | 09-03 ☐ |
| E-002 | TPM / PTU / 429 | Débil | 08-05 ☐ | 08-07 ☐ | 08-11 ☐ | 08-18 ☐ | 09-03 ☐ |
| E-003 | Managed Identity / RBAC | Débil | 08-05 ☐ | 08-07 ☐ | 08-11 ☐ | 08-18 ☐ | 09-03 ☐ |
| E-004 | Categorías content filter | Débil | 08-05 ☐ | 08-07 ☐ | 08-11 ☐ | 08-18 ☐ | 09-03 ☐ |
| E-005 | Capas de retrieval AI Search | En aprendizaje | 08-05 ☐ | 08-07 ☐ | 08-11 ☐ | 08-18 ☐ | 09-03 ☐ |
| E-006 | Catálogo de tools | En aprendizaje | 08-05 ☐ | 08-07 ☐ | 08-11 ☐ | 08-18 ☐ | 09-03 ☐ |
| E-007 | Evaluadores / groundedness | Débil | 08-05 ☐ | 08-07 ☐ | 08-11 ☐ | 08-18 ☐ | 09-03 ☐ |
| E-008 | LLM vs SLM | En aprendizaje | 08-05 ☐ | 08-07 ☐ | 08-11 ☐ | 08-18 ☐ | 09-03 ☐ |
| E-009 | requires_action / submit_tool_outputs | En aprendizaje | 08-05 ☐ | 08-07 ☐ | 08-11 ☐ | 08-18 ☐ | 09-03 ☐ |

**Prioritarios para el repaso de 1 día (08-05):** E-001, E-003, E-005, E-007 — los cuatro de mayor impacto en el examen.

---

## Siguiente acción recomendada

**Bloque B1 — "Fundamentos operativos de la plataforma Azure AI"**

Ataca los 6 temas débiles de mayor prioridad de una sola vez, porque están encadenados: un *deployment* tiene una *cuota* (TPM), se protege con *content filters*, se accede con *Managed Identity* y se mide con *evaluadores*. Es una sola historia, no seis temas sueltos.

Contenido:

1. Model → deployment → endpoint. Qué se pasa en `model=` del SDK (E-001)
2. TPM vs PTU, 429 + `Retry-After`, backoff exponencial (E-002)
3. Managed Identity + RBAC + `DefaultAzureCredential` (E-003)
4. Content filters: 4 categorías × 4 severidades, prompt vs completion, Prompt Shields (E-004)
5. Evaluadores de Foundry: groundedness, relevance, coherence, fluency (E-007)
6. Capas de retrieval en AI Search: BM25 / vectorial / híbrida + RRF / semantic ranker (E-005)

Material: módulos LP1-1 y LP1-2 del curso + docs dirigidas para cuotas y seguridad (huecos R8, que el curso no cubre).

**Cierre del bloque:** DIAG-1B (repesca de las 8 preguntas falladas/parciales). Solo entonces suben los niveles.

Después: DIAG-2 (D3+D4+D5) para completar el mapa antes de planificar el resto.

**Orden tentativo del curso:** LP1 → LP2 → LP4 → LP3, con los huecos de D1 (R8) intercalados como estudio dirigido de docs.

---

## Información faltante

| # | Dato | Por qué importa |
| --- | --- | --- |
| 1 | Fecha objetivo o deadline del examen | Define ritmo, profundidad y cuándo empezar simulaciones |
| 2 | Horas de estudio disponibles por semana | Determina tamaño de los bloques |
| 3 | ¿Tiene suscripción de Azure activa con acceso a Foundry? ¿Créditos o límites? | Decide si los labs son reales o solo teóricos. Es un examen de implementación |
| 4 | ¿Ha usado Azure antes en cualquier medida? (portal, CLI, App Service…) | Ajusta el punto de partida de D1 |
| 5 | ¿Región de Azure disponible? | Disponibilidad de modelos y features varía por región; a veces se pregunta |
| 6 | Idioma en que rendirá el examen | El inglés se actualiza primero; la localización va ~8 semanas atrás |
| 7 | Preferencia de formato: ¿más quiz rápido o más lab práctico? | Ajusta el balance del plan |
| 8 | ¿Harás el curso oficial completo (96 h) o solo los módulos que el diagnóstico marque como brecha? | Cambia radicalmente el calendario. Con tu perfil, hacer los 30 módulos linealmente probablemente desperdicia tiempo en LP1/LP2 |

---

## Decisiones registradas

| Fecha | Decisión | Motivo |
| --- | --- | --- |
| 2026-08-04 | Diagnóstico dividido en dos partes: D1+D2 primero, luego D3+D4+D5 | Evita una sesión demasiado larga y prioriza el 55–65% del examen |
| 2026-08-04 | Todo el temario inicia como "No evaluado" | No hay progreso previo comprobado |
| 2026-08-04 | Adoptado el curso AI-103T00-A como ruta de contenido, en archivo separado (`AI-103-LEARNING-PATH.md`) | Aportado por el usuario. Se separa del temario porque cumplen funciones distintas: el curso es el camino, el temario es la vara de medir. Ante discrepancia manda el temario |
