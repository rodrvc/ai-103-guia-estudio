# AI-103 — Bitácora de sesiones

> **Qué contiene:** qué pasó en cada sesión, en orden cronológico inverso.
> **Cuándo leerlo:** para reconstruir historia o entender por qué se decidió algo.
> **Cuándo NO:** en trabajo normal. El estado vive en STUDY-STATE; la evidencia en ERROR-LOG y PRACTICE. Este archivo casi nunca es necesario.

**Política de tamaño:** solo las **últimas 5 sesiones** en detalle (**≤8 líneas** cada una: hecho / resultado / errores / cambios de nivel / próxima acción). Las anteriores colapsan a una fila de § Historial comprimido y se archivan por mes en `study/archive/SESSION-LOG-<AAAA-MM>.md`. No re-narrar aquí lo que ya está en ERROR-LOG o PRACTICE — enlazar.

---

## 2026-08-09 — Sesión 015: cierre de material de D1 y D2 (6 apuntes) + publicación

- **El usuario pidió cerrar el contenido de D1 y D2** para poder estudiarlos completos. Escritos **6 apuntes** leyendo docs de Azure (5 de los 6 temas **no están en el curso oficial**): `D1/03-cuotas-y-rate-limits` · `D1/04-seguridad-identidad-y-red` · `D1/05-genaiops-cicd-y-monitoreo` · `D2/08-las-cuatro-tools` · `D2/09-multi-agente-y-orquestacion` · `D2/10-observabilidad-y-tracing`.
- **Cobertura: 27% → 59%.** D1 y D2 (**55–65% del examen**) quedan cubiertos.
- ⚠️ **Tres correcciones tras verificar la fuente** — dos afectaban a apuntes ya publicados:
  1. **Content filters: son 4 categorías graduadas**, no 5. Task Adherence, Prompt Shields, Groundedness, PII y Protected Material son **filtros binarios sin severidad**. La sesión 014 lo había apuntado mal por leer solo el módulo de Learn. Corregido en E-004.
  2. **Los roles RBAC se renombraron:** *Azure AI User* → **Foundry User**. Corregido en 3 apuntes.
  3. **`requires_action` / `submit_tool_outputs` es de la Assistants API.** La Responses API usa **`function_call` → `function_call_output`**. Anotado en E-009 con ambos nombres.
- **Revisión externa aplicada** (agente `alvaro-tech-architect`) antes de publicar: despersonalizadas **23 referencias** al historial personal (E-NNN, DIAG, R7) → "Confusión frecuente: X no es Y" · apunte nuevo `D1/00-fundamentos` (el vocabulario que todo lo demás daba por sabido) · chunking añadido a RAG · INDEX con ruta de lectura y tabla de confusiones · README reescrito · LICENSE MIT.
- **Publicado** en https://github.com/rodrvc/ai-103-guia-estudio (público, rama `main`).
- **Sin cambios de nivel.** El cuello de botella se movió: ya no es escribir material, es **verificarlo** (~40 preguntas, 2 respondidas).
- **Próxima acción:** DIAG-2 (D3/D4/D5 sin medir) o evaluar D1/D2, que ya tienen material completo.

---

## 2026-08-08 — Sesión 014: IA responsable (LP1-M6) + primer repaso real

- **Volvió a LP1-M6** tras empezar LP2-M1 en la misma sesión. **Salta de módulo con frecuencia** — no asumir continuidad, verificar en Learn o preguntar.
- **Apunte nuevo:** `notes/D1-plataforma/02-ia-responsable.md` — **Map → Measure → Mitigate → Manage** · los 4 pasos de Map (identify/prioritize/test/document) y **red teaming** · baseline y **manual antes que automático** · **las 4 capas de mitigación** (model / safety system / system message+grounding / UX) · **prompt shields** · plan de salida (phased delivery, incident response, rollback, telemetría).
- ⚠️ **Contradicción detectada y comunicada:** el módulo (actualizado 2026-04-28) lista **5 categorías** de content filter — añade **task-adherence** a hate/sexual/violence/self-harm. E-004 decía 4. **Corregido**; queda por verificar en docs de Content Safety.
- **Primer repaso real del proyecto** (LP2-M1, interrumpido a las 2 preguntas): **P1 Deploy vs Publish ✅ acertó** · **P2 RBAC al publicar ⚠️ parcial** — vio que era de permisos, no que la identidad de Entra es nueva y no hereda. Preguntó **qué es RBAC** (toca E-003, explicado).
- **Cobertura: 23% → 27%** (8 → 9 apuntes). D1.4.b pasa a *Estudiado, sin evaluar*; E-004 y E-006 ya tienen material.
- **Sin cambios de nivel** — 2 respuestas no bastan y una fue parcial.
- **Feedback explícito del usuario sobre el método:** *"como me lo explicaste suena super simple… has mejorado en explicaciones"*, tras desarrollar el proceso de 4 pasos sobre un **caso concreto** (chatbot médico en una clínica) en vez de definir cada etapa. Pidió guardar el ejemplo **y** el método.
- **Skill nuevo:** `.claude/skills/explicar/SKILL.md` — un caso concreto atravesando todos los pasos · hacer visible la fricción contraintuitiva · analogías solo para lo arbitrario, lógica para lo lógico · 5 señales de que la explicación va mal. El ejemplo del chatbot queda **dentro del apunte**, no solo en la conversación.
- **Próxima acción:** u7 de LP1-M6 (**ejercicio de content filters**, cierra E-004) o las 7 preguntas del apunte nuevo. Retomar el repaso de LP2-M1 desde la P3.

---

## 2026-08-08 — Sesión 013: LP2-M1 completo (agentes con Foundry y VS Code)

- **Cambio de fase verificado en Learn:** el usuario **saltó de LP1-M5 a LP2-M1**. LP1 = 4/6 (87%), LP2 = 0/9. LP1 queda incompleto: módulo 5 pausado en la u6 y **módulo 6 sin empezar (D1.4 entero)**.
- **Corrección de proceso en la misma sesión:** el agente resumió solo la unidad 4 (la URL que pasó el usuario). **Él lo detectó** — *"te pedí del módulo, no del video"*. Releídas las 11 unidades y rehecho el resumen. Es la trampa del skill § "lee el módulo entero", que ya había fallado el 2026-08-04.
- **Apunte nuevo:** `notes/D2-apps-y-agentes/07-construir-y-publicar-agentes.md` — portal vs VS Code · YAML del agente (temperature/top_p) · catálogo de tools (3 categorías) y MCP (3 tipos) · **Deploy vs Publish** · Agent Application, endpoint estable, Entra ID y la trampa de RBAC · stateless en producción.
- **Hallazgo de alto valor:** *deploy* (guarda en el proyecto) ≠ *publish* (crea Agent Application con endpoint). Al publicar, el agente recibe **identidad de Entra propia y los permisos NO se heredan** — hay que reasignar RBAC o las tools fallan. Cae seguro.
- **Cobertura: 18% → 23%** (7 → 8 apuntes). D2.2.a pasa a *Estudiado, sin evaluar*; D2.2.c y D1.3.d ganan material.
- **Sin cambios de nivel. Sin evaluación** — ya van **~20 preguntas planteadas y nunca respondidas** en 4 apuntes. Ese es el patrón a romper.
- **Próxima acción:** las 7 preguntas del apunte 07 (10 min) → luego u9 (ejercicio) y u10 (knowledge check) de LP2-M1, que sí dan evidencia.

---

## 2026-08-05 — Sesión 012: Apunte de RAG (LP1-M5, unidad 3)

- **El usuario pasó una URL en `es-es`** de *Optimize generative AI model performance* u3. Leída la fuente en **`en-us`** + índice del módulo (8 unidades).
- **Apunte nuevo:** `notes/D2-apps-y-agentes/06-rag-grounding.md` — retrieve/augment/generate · embeddings y cosine similarity · Azure AI Search (3 pasos, 4 búsquedas, **hybrid recomendado**) · SDK con Responses API · RAG vs prompt eng. vs fine-tuning.
- **Fase corregida:** saltó del **módulo 4** (tools) al **módulo 5**. El 4 queda ⏭️ saltado, no abandonado — sigue cubriendo E-006.
- **Hallazgo:** el apunte **no cierra E-005**. El curso no menciona **BM25, RRF ni semantic ranker**, que es justo lo que falló en DIAG-1 p8. Esa capa exige docs de AI Search. Anotado en el error.
- **Sin evaluación: el usuario la rechazó explícitamente** ("ya no más… por ahora me interesa saber lo más importante"). Se le dio el resumen de alto valor en su lugar.
- **Corrección (mismo día):** el agente escribió que había "saltado" el módulo 4 **sin verificarlo** — lo dedujo de que estuviera en el 5. **El usuario lo desmintió.** Verificado en Learn: **4/6 módulos (67%)**, 1800/3699 XP, va por la u6 del módulo 5. Corregido en 4 archivos.
- **Lección:** no inferir progreso del curso a partir de la unidad que el usuario menciona. Learn es la única fuente — se lee con el navegador y su sesión.
- **Deuda nueva:** el módulo 4 (tools) está **completo sin apunte**. E-006 sigue abierto y sin material.
- **Sin cambios de nivel.** Cobertura: 6 → **7 apuntes**. **Próxima acción:** sin cambio — DIAG-2 o repaso.

---

## 2026-08-05 — Sesión 011: Rescate de contenido explicado sin anotar

- **El usuario preguntó si los apuntes cubrían todo lo conversado.** Auditadas las secciones de los 4 apuntes contra la conversación: **5 huecos**.
- **Rescatado a `notes/`:** Service Principal vs Managed Identity vs `az login` (+ tabla API key vs SP, cadena de `DefaultAzureCredential`) · tabla comparativa de código ChatCompletions/Responses · imports `.aio` y `await credential.close()`.
- **2 apuntes nuevos:** `04-agentes-en-foundry.md` (declarativo vs hosted, automatic tool calling, 8 riesgos + mitigaciones) y `05-labs-oficiales.md` (leaderboard, benchmarks ≠ evaluación, dataset sintético, `.env` sin key).
- **Cobertura: 9% → 16%** (4 → 6 apuntes, ~7 de 44 objetivos).
- **Lección de proceso:** explicar en conversación **no** deja rastro. Si vale la pena explicarlo, va a `notes/` en la misma sesión.
- **Sin cambios de nivel.** **Próxima acción:** sin cambio — DIAG-2 o repaso.

---

## 2026-08-05 — Sesión 010: Datos de calendario + auditoría de onboarding

- **Datos nuevos:** examen en **inglés** · **10 h/semana** (con fin de semana) · objetivo del usuario ~2 semanas. Los 3 datos faltantes quedan resueltos.
- **Se le comunicó que 2 semanas no es realista:** ~20 h contra 96 h estimadas del curso, 4/44 objetivos con material, 3 dominios sin medir, 42% en DIAG-1. **Estimación: 6–8 semanas** (mediados de septiembre).
- **Preguntó por NotebookLM:** con los apuntes actuales **no** tendría material suficiente — 9% de cobertura, D3/D4/D5 vacíos.
- **Test de onboarding:** un agente sin contexto se orientó en **2 archivos**, pero reportó 4 desfases (rutas rotas tras la reorganización, bloque B1 fosilizado, ancla inexistente en el skill, contador desfasado). Todos corregidos.
- **Hallazgo estructural:** *"el sistema premia avanzar y no premia repasar"*. La siguiente acción ahora obliga a ofrecer los repasos vencidos antes de proponer módulo nuevo.
- **Historial de git limpiado** de firmas de coautoría.
- **Sin cambios de nivel.** **Próxima acción:** DIAG-2, o repaso (9 hitos vencidos).

---

## 2026-08-05 — Sesión 009: Progreso real verificado + handoff

- **Verificado en Learn con la sesión del usuario** (navegador, no WebFetch): **LP1 = 3/6 módulos (45%)** · LP2 = 0/9 (1%, solo un vistazo) · perfil Level 3, 200/3699 XP.
- **Fase corregida:** sigue en **LP1, módulo 4** (*apps that use tools*). El 1% de LP2 no fue cambio de rumbo.
- **Conceptos explicados** (endpoints, SDK, auth, Managed Identity vs Service Principal, memoria, Responses vs ChatCompletions): **sin evaluar** — el usuario pidió entender antes de que le pregunten.
- **Lección de método registrada en `CLAUDE.md`:** un concepto por mensaje, analogía antes que jerga, no examinar sin haber explicado. El usuario rechazó explícitamente los muros de texto.
- **Sin cambios de nivel.** Ningún error cerrado: los apuntes cubren E-003 y E-007 pero no se verificaron con preguntas.
- **Próxima acción:** continuar LP1-M4. Deudas: DIAG-2, repaso de los 9 errores, 8 preguntas sin responder.

---

## 2026-08-04 — Sesión 008: Módulo foundry-sdk completo (corrección)

- **El usuario detectó un fallo del agente:** el apunte de LP1-M3 cubría solo la **unidad 2 de 8** (playground, peso bajo). Faltaban las unidades 3–5: endpoints, SDKs, autenticación y Responses vs ChatCompletions — **peso alto**.
- **Corregido:** leídas las unidades 3, 4 y 5. Escrito `notes/LP1-M3-foundry-sdk.md` cubriendo el módulo.
- **Contenido clave:** dos endpoints (project vs Azure OpenAI) · Foundry SDK vs OpenAI SDK y cuándo cada uno · **Entra ID + `DefaultAzureCredential`** · Responses (stateful, `previous_response_id`) vs ChatCompletions (historial manual).
- **Cierra E-003** (auth sin claves) y aclara **E-001** (`model=` recibe el nombre del deployment) — **pendiente de verificar con preguntas**.
- **Cambios de nivel:** ninguno. D2.1.e y D2.1.f → `Estudiado, sin evaluar`.
- **Lección de proceso registrada en el skill:** leer el **índice del módulo** antes de escribir un apunte, no solo la unidad que pasa el usuario. El apunte cubre el módulo, no la unidad.
- **Próxima acción:** 5 preguntas del apunte nuevo (verifican E-003 y E-001) · luego DIAG-2.

---

## Historial comprimido

| # | Fecha | Qué pasó |
| --- | --- | --- |
| 007 | 2026-08-04 | Creado el skill `study-tracker`: registro automático de progreso, hogar canónico por dato, 7 trampas |
| 006 | 2026-08-04 | **D1.2.b → Competente** (3/3 casos). Hallazgo pedagógico: analogías → 100% de acierto donde la jerga fallaba. Método registrado en `CLAUDE.md` |
| 005 | 2026-08-04 | Lab 02 (model catalog) + dataset sintético. Suscripción Azure confirmada. Apunte de **deployment types** (hueco R8) |
| 004 | 2026-08-04 | Auditoría de memoria (memory-engineer): routing por tarea, presupuesto de contexto, políticas de archivado |
| 003 | 2026-08-04 | **DIAG-1: 42%.** 9 errores registrados. D1→Débil, D2→En aprendizaje. Primera evidencia real |
| 002 | 2026-08-04 | Curso oficial AI-103T00-A mapeado: 4 paths, 30 módulos. Detectados 7 huecos (R8), casi todos en D1 |
| 001 | 2026-08-04 | Setup: memoria creada, temario oficial verificado (vigente 2026-04-16), 44 objetivos |

Detalle en `study/archive/SESSION-LOG-2026-08.md`.
