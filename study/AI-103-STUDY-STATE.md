# AI-103 — Estado del estudio

> **Qué contiene:** dónde estamos y qué toca hacer. Es un **índice**, no una copia — remite a la evidencia.
> **Cuándo leerlo:** al retomar el trabajo, o al actualizar progreso tras evidencia nueva.
> **Cuándo NO:** si ya sabes tu tarea concreta, ve directo al archivo que toca (ver `CLAUDE.md` § Qué leer).
>
> **Reglas de integridad al escribir aquí:** no subir un nivel sin evidencia registrada · no inventar progreso · fechas `YYYY-MM-DD` · nunca guardar claves ni tokens.
>
> Evidencia: `AI-103-ERROR-LOG.md` (errores y patrones) · `AI-103-PRACTICE.md` (quizzes y labs) · `AI-103-SYLLABUS.md` (niveles por objetivo) · `AI-103-LEARNING-PATH.md` (currículo) · `PROFILE.md` (perfil, fuentes, riesgos).

- **Última actualización:** 2026-08-05
- **Fase actual:** **LP1, módulo 4** (*"Develop generative AI apps that use tools"*). LP1 va **3 de 6 módulos (45%)**, verificado en Learn el 2026-08-05. El bloque B1 sigue definido pero **no se sigue linealmente**: avanza por el curso y por temas que le surgen
- **Examen reservado:** No
- **Fecha objetivo:** Sin definir (falta dato del usuario)

---

## Progreso global

| Métrica | Valor | Base |
| --- | --- | --- |
| Objetivos del temario | 44 | `AI-103-SYLLABUS.md` |
| Evaluados | 12 (27%) | DIAG-1 + D1.2.b |
| Competente o superior | **2** (5%) | D2.2.b · **D1.2.b** (3/3 casos, 2026-08-04) |
| Apuntes escritos | 4 | `notes/INDEX.md` |
| Simulaciones completas hechas | 0 | — |
| Mejor puntaje parcial | **42%** (DIAG-1, D1+D2) | `AI-103-PRACTICE.md` |
| Módulos del curso oficial | **3 / 30 (10%)** | Verificado en Learn 2026-08-05 |

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

Acierta lo **conceptual y de diseño**; falla lo **operativo y nombrado de Azure**. Brecha de superficie, no de fundamentos — pero es lo que el examen mide. Cero errores de lectura o descarte: dijo "no lo sé" tres veces en vez de inventar, lo que hace el diagnóstico fiable.

⚠️ **Confianza de esta conclusión: media.** n=12, respuesta abierta, corregidas por el mismo agente que las escribió. **Revisar tras DIAG-1B** y degradar si no se confirma.

Análisis completo, distribución por causa y pares de servicios que confunde: `AI-103-ERROR-LOG.md` § Patrones detectados.

---

**Riesgos R1–R8:** viven en `PROFILE.md` § Mapa de riesgos. Confirmados por DIAG-1: **R1, R2, R7** (y R5 parcial). Pendientes de DIAG-2: R3, R4, R6.

---

## Temas fuertes (con evidencia)

| Tema | Nivel | Evidencia |
| --- | --- | --- |
| Ciclo de vida de conversación de agentes (thread/run/message, persistencia en el servicio) | Competente | DIAG-1 p10 pleno |
| Foundry IQ y conexión de fuentes de datos empresariales (SharePoint, etc.) | En aprendizaje | DIAG-1 p1 pleno |
| Content filter vs blocklist | En aprendizaje | DIAG-1 p7 pleno |
| Patrón human-in-the-loop en agentes (a nivel de diseño) | En aprendizaje | DIAG-1 p11 pleno en concepto |

## Temas débiles

**9 errores abiertos, todos de DIAG-1.** Detalle, causa y respuesta correcta: `AI-103-ERROR-LOG.md`. Prioridad de estudio (peso × severidad):

- **Crítica:** E-003 managed identity · E-001 deployment vs model · E-002 cuotas TPM/PTU
- **Alta:** E-005 capas de retrieval · E-007 evaluadores · E-004 content filters
- **Media:** E-006 catálogo de tools · E-009 requires_action · E-008 LLM vs SLM

---

## Repaso programado (repetición espaciada)

Los 9 errores de DIAG-1 entran con el mismo calendario base.

**Estados:** ☐ pendiente · ☑ repasado OK · ✗ **fallido → reinicia el contador** (se reprograma desde 1d con fecha nueva y se anota el fallo en el error correspondiente).

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

**Si el usuario retoma sin decir qué quiere → continuar LP1 módulo 4** (*apps that use tools*, 9 unidades: `code_interpreter`, `web_search`, `file_search`, `function`). Cubre **D2.2.c** y su error **E-006**.

**Deudas acumuladas** (mencionarlas, no imponerlas):

1. **DIAG-2** — 8 preguntas, ~30 min. D3/D4/D5 llevan sin medir desde el inicio: **30–45% del examen** a ciegas. Es lo que más cambia el plan.
2. **Repaso de los 9 errores de DIAG-1.** La cola espaciada arrancaba el 2026-08-05 y **no se ejecutó**. Hay apuntes que cubren E-003 y E-007, pero **ninguno verificado con preguntas** — así que ningún error está cerrado.
3. **Preguntas planteadas sin responder:** 3 del lab 02 (model catalog) y 5 del apunte `LP1-M3-foundry-sdk.md` (estas cerrarían E-003 y E-001).

**Preguntas abiertas al usuario:** idioma del examen (recomendado inglés) · fecha objetivo · horas/semana.

---

### Bloque B1 — definido, no iniciado

Sigue siendo válido como plan, pero el usuario avanza por el curso a su ritmo. Mantener como referencia de qué falta cerrar en D1.

Ataca los 6 temas débiles de mayor prioridad de una sola vez, porque están encadenados: un *deployment* tiene una *cuota* (TPM), se protege con *content filters*, se accede con *Managed Identity* y se mide con *evaluadores*. Es una sola historia, no seis temas sueltos.

Contenido:

1. Model → deployment → endpoint. Qué se pasa en `model=` del SDK (E-001)
2. TPM vs PTU, 429 + `Retry-After`, backoff exponencial (E-002)
3. Managed Identity + RBAC + `DefaultAzureCredential` (E-003)
4. Content filters: 4 categorías × 4 severidades, prompt vs completion, Prompt Shields (E-004)
5. Evaluadores de Foundry: groundedness, relevance, coherence, fluency (E-007)
6. Capas de retrieval en AI Search: BM25 / vectorial / híbrida + RRF / semantic ranker (E-005)
7. **Deployment types** (D1.2.b): global/data zone/regional × standard/provisioned/batch, residencia. Apunte ya escrito, **falta evaluarlo**

Material: módulos LP1-1 y LP1-2 del curso + docs dirigidas para cuotas y seguridad (huecos R8, que el curso no cubre).

**Cierre del bloque:** DIAG-1B (repesca de las 8 preguntas falladas/parciales). Solo entonces suben los niveles.

Después: DIAG-2 (D3+D4+D5) para completar el mapa antes de planificar el resto.

**Orden tentativo del curso:** LP1 → LP2 → LP4 → LP3, con los huecos de D1 (R8) intercalados como estudio dirigido de docs.

---

## Información faltante

**Al resolver un dato, bórralo de esta tabla.** Si sigue aquí, es que nadie lo ha preguntado.

| # | Dato | Por qué importa |
| --- | --- | --- |
| 1 | Fecha objetivo o deadline del examen | Define ritmo y cuándo empezar simulaciones |
| 2 | Horas de estudio por semana | Determina el tamaño de los bloques |
| 3 | Idioma en que rendirá el examen | El inglés se actualiza primero; la localización va ~8 semanas atrás. Recomendado: inglés |

**Resueltos:** suscripción de Azure activa ✅ (2026-08-04, labs viables) · avanza por el curso oficial siguiendo LP1 en orden, no solo las brechas (observado 2026-08-05).

---

## Decisiones registradas

| Fecha | Decisión | Motivo |
| --- | --- | --- |
| 2026-08-04 | Diagnóstico dividido en dos partes: D1+D2 primero, luego D3+D4+D5 | Evita una sesión demasiado larga y prioriza el 55–65% del examen |
| 2026-08-04 | Todo el temario inicia como "No evaluado" | No hay progreso previo comprobado |
| 2026-08-04 | Adoptado el curso AI-103T00-A como ruta de contenido, en archivo separado (`AI-103-LEARNING-PATH.md`) | Aportado por el usuario. Se separa del temario porque cumplen funciones distintas: el curso es el camino, el temario es la vara de medir. Ante discrepancia manda el temario |
