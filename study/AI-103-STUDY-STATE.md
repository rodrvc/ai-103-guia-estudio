# AI-103 — Estado del estudio

> **Qué contiene:** dónde estamos y qué toca hacer. Es un **índice**, no una copia — remite a la evidencia.
> **Cuándo leerlo:** al retomar el trabajo, o al actualizar progreso tras evidencia nueva.
> **Cuándo NO:** si ya sabes tu tarea concreta, ve directo al archivo que toca (ver `CLAUDE.md` § Qué leer).
>
> **Reglas de integridad al escribir aquí:** no subir un nivel sin evidencia registrada · no inventar progreso · fechas `YYYY-MM-DD` · nunca guardar claves ni tokens.
>
> Evidencia: `AI-103-ERROR-LOG.md` (errores y patrones) · `AI-103-PRACTICE.md` (quizzes y labs) · `AI-103-SYLLABUS.md` (niveles por objetivo) · `AI-103-LEARNING-PATH.md` (currículo) · `PROFILE.md` (perfil, fuentes, riesgos).

- **Última actualización:** 2026-08-05
- **Fase actual:** **LP1, módulo 5** (*"Optimize generative AI model performance"*, 8 unidades) — va por la **unidad 6** (ejercicio). **Terminó el módulo 4** (tools). LP1 va **4 de 6 módulos (67%)**, verificado en Learn el 2026-08-05 (1800/3699 XP, Level 3). Avanza por el curso y por temas que le surgen, no linealmente
- **Examen reservado:** No
- **Fecha objetivo:** Sin definir (falta dato del usuario)

---

## Progreso global

| Métrica | Valor | Base |
| --- | --- | --- |
| Objetivos del temario | 44 | `AI-103-SYLLABUS.md` |
| Evaluados | 12 (27%) | DIAG-1 + D1.2.b |
| Competente o superior | **2** (5%) | D2.2.b · **D1.2.b** (3/3 casos, 2026-08-04) |
| Apuntes escritos | **7** | `notes/INDEX.md` |
| **Cobertura de material** | **~8 / 44 objetivos (18%)** | D3, D4 y D5 sin una línea |
| Simulaciones completas hechas | 0 | — |
| Mejor puntaje parcial | **42%** (DIAG-1, D1+D2) | `AI-103-PRACTICE.md` |
| Módulos del curso oficial | **4 / 30 (13%)** | Verificado en Learn 2026-08-05 |

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

**Si el usuario dice "sigamos" sin más → antes de avanzar, mira § Repaso programado.** Si hay hitos vencidos (los hay desde 2026-08-05), **ofrece el repaso primero**: *"Tienes N repasos vencidos y 4 apuntes sin verificar. ¿Repasamos 10 minutos o seguimos avanzando?"* No lo impongas — pero tampoco lo omitas.

**Por qué:** el sistema tiende a premiar avanzar (siempre hay un módulo nuevo) y a no premiar repasar (la cola no protesta sola). Con 42% en DIAG-1 y 9 errores abiertos, avanzar sin consolidar es exactamente el fallo a evitar.

**Si elige avanzar → LP1 módulo 5, donde está ahora** (unidad 6, el ejercicio). Anotada la u3 (RAG); quedan sin apunte **u2 prompt engineering**, **u4 fine-tuning** y **u5 comparar estrategias** — cubren D2.3.a, hoy en cero.

⚠️ **Deuda de material del módulo 4** (tools, ya completado en Learn el 2026-08-05): **no dejó apunte**. E-006 sigue abierto y sin material pese a haber pasado por el contenido. Escribir ese apunte es barato ahora que lo vio, y caro más adelante.

**El cuello de botella real es la cobertura de material.** Preguntó (2026-08-05) si con los apuntes actuales en NotebookLM tendría suficiente: **no**. 7 apuntes ≈ 18% de los objetivos; D3+D4+D5 (30–45% del examen) sin nada. Escribir apuntes de los dominios vacíos es lo que más rinde después de DIAG-2.

**Deudas acumuladas** (mencionarlas, no imponerlas):

1. **DIAG-2** — 8 preguntas, ~30 min. D3/D4/D5 llevan sin medir desde el inicio: **30–45% del examen** a ciegas. Es lo que más cambia el plan.
2. **Repaso de los 9 errores de DIAG-1.** La cola espaciada arrancaba el 2026-08-05 y **no se ejecutó**. Hay apuntes que cubren E-003 y E-007, pero **ninguno verificado con preguntas** — así que ningún error está cerrado.
3. **Preguntas planteadas sin responder:** 3 del lab 02 (model catalog) y 5 del apunte `notes/D2-apps-y-agentes/01-conectar-app-a-foundry.md` (estas cerrarían E-003 y E-001).

**Preguntas abiertas al usuario:** idioma del examen (recomendado inglés) · fecha objetivo · horas/semana.

---

### Fundamentos operativos de D1 — estado por tema

Están encadenados: un *deployment* tiene un *type* y una *cuota*, se protege con *content filters*, se accede con *Managed Identity* y se mide con *evaluadores*. Una sola historia.

| Tema | Error | Material | Evaluado |
| --- | --- | --- | --- |
| Deployment types | — | ✅ `notes/D1-plataforma/01-deployment-types.md` | ✅ **Competente** 3/3 |
| Model → deployment → endpoint | E-001 | ✅ dentro de D2/01 y D2/03 | ❌ |
| Managed Identity + RBAC | E-003 | ✅ D2/01 § Autenticación | ❌ |
| TPM vs PTU, 429, backoff | E-002 | 🟡 parcial en D1/01 | ❌ |
| Evaluadores (groundedness…) | E-007 | ✅ D2/02 | ❌ |
| Content filters: 4×4, Prompt Shields | E-004 | ❌ sin material | ❌ |
| Capas de retrieval en AI Search | E-005 | 🟡 parcial en D2/06 — **falta BM25, RRF y semantic ranker** (el curso no los trata) | ❌ |

**Lectura:** hay material para 6 de 7 y **ninguno se ha verificado con preguntas**. El cuello de botella no es escribir apuntes, es **repasar** (skill `repasar`).

**Cierre:** DIAG-1B, repesca de las 8 preguntas falladas/parciales de DIAG-1.

**Orden tentativo del curso:** LP1 → LP2 → LP4 → LP3, con los huecos de D1 (R8) intercalados como estudio dirigido de docs.

---

## Información faltante

**Al resolver un dato, bórralo de esta tabla.** Si sigue aquí, es que nadie lo ha preguntado.

*(Ninguno pendiente ahora mismo.)*

**Resueltos (2026-08-04/05):** suscripción de Azure activa ✅ · examen en **inglés** ✅ · **10 h/semana** incluido fin de semana ✅ · avanza por el curso siguiendo LP1 en orden, no solo las brechas.

---

## Calendario

- **Disponibilidad:** 10 h/semana (con fin de semana).
- **Objetivo que planteó el usuario (2026-08-05):** rendir en ~2 semanas.
- **Estimación realista: 6–8 semanas** → **mediados de septiembre de 2026**.

**Por qué no llegan 2 semanas** — se le comunicó explícitamente:

| Dato | Valor |
| --- | --- |
| Horas en 2 semanas | ~20 h |
| Estimación oficial del curso | 96 h |
| Objetivos con material | 4 de 44 |
| Dominios sin medir | D3, D4, D5 (30–45% del examen) |
| DIAG-1 | 42% (se aprueba con ~70%) |
| Errores abiertos sin repasar | 9 |

**Regla de reserva vigente:** ≥2 simulaciones completas con ≥80%, ningún dominio <70%, sin dominios de peso alto en Débil. Hoy no se cumple ninguna condición. **No recomendar reservar.**

Si el usuario insiste en la fecha corta, ofrecer un plan de emergencia priorizado por peso (D2 → D1 → resto) advirtiendo que iría con ~50% de probabilidad.

---

## Decisiones registradas

| Fecha | Decisión | Motivo |
| --- | --- | --- |
| 2026-08-04 | Diagnóstico dividido en dos partes: D1+D2 primero, luego D3+D4+D5 | Evita una sesión demasiado larga y prioriza el 55–65% del examen |
| 2026-08-04 | Todo el temario inicia como "No evaluado" | No hay progreso previo comprobado |
| 2026-08-04 | Adoptado el curso AI-103T00-A como ruta de contenido, en archivo separado (`AI-103-LEARNING-PATH.md`) | Aportado por el usuario. Se separa del temario porque cumplen funciones distintas: el curso es el camino, el temario es la vara de medir. Ante discrepancia manda el temario |
