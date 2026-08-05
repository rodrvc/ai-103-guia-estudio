# AI-103 — Bitácora de sesiones

> **Qué contiene:** qué pasó en cada sesión, en orden cronológico inverso.
> **Cuándo leerlo:** para reconstruir historia o entender por qué se decidió algo.
> **Cuándo NO:** en trabajo normal. El estado vive en STUDY-STATE; la evidencia en ERROR-LOG y PRACTICE. Este archivo casi nunca es necesario.

**Política de tamaño:** solo las **últimas 5 sesiones** en detalle (**≤8 líneas** cada una: hecho / resultado / errores / cambios de nivel / próxima acción). Las anteriores colapsan a una fila de § Historial comprimido y se archivan por mes en `study/archive/SESSION-LOG-<AAAA-MM>.md`. No re-narrar aquí lo que ya está en ERROR-LOG o PRACTICE — enlazar.

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

## 2026-08-04 — Sesión 007: Skill `study-tracker`

- **Hecho:** creado `.claude/skills/study-tracker/SKILL.md` — automatiza el registro de progreso para que no dependa de que el usuario lo pida.
- **Se dispara solo** al: escribir un apunte, corregir respuestas, completar lab/quiz/diagnóstico, descubrir tema no cubierto, o cerrar sesión. También responde "¿dónde quedamos?" en modo consulta (sin escribir).
- **Contiene:** tabla de hogar canónico por dato · procedimiento de 5 pasos · presupuesto de contexto · 7 trampas conocidas (la #1: dejar escrita una fase abandonada).
- **`CLAUDE.md` comprimido** (–9 líneas): el procedimiento detallado vive ahora en el skill; aquí queda el puntero.
- **Nota:** el skill se indexa al iniciar sesión. En **esta** sesión aún no es invocable; a partir de la próxima, sí.
- **Sin cambios** de nivel ni errores nuevos.
- **Próxima acción:** sin cambio — DIAG-2 (prioridad 1) y repaso de los 9 errores.

---

## 2026-08-04 — Sesión 006: Deployment types evaluado + hallazgo pedagógico

- **Hallazgo importante:** el usuario pidió "bájalo a algo entendible". Reexplicado con analogías (taxi / auto propio / encomienda · casa vs. lavandería) → pasó de no entender a **3/3 casos correctos**, incluido el que había fallado en jerga.
- **Método registrado en `CLAUDE.md`:** analogía → decisión en cristiano → nombre técnico **al final**. No es simplificar contenido, es cambiar la puerta de entrada.
- **Evidencia:** GlobalStandard (sin restricciones) · GlobalBatch (lotes sin prisa) · DataZoneProvisioned (UE + latencia crítica). Domina los dos ejes y el criterio de **no sobre-restringir** ("la UE"→zona, "Alemania"→región).
- **Cambio de nivel:** **D1.2.b → Competente** (primer objetivo que sube desde el diagnóstico; el segundo Competente del proyecto).
- **Falta para Dominado:** Developer tier, SLA por tipo, at-rest vs procesamiento, y evidencia en 2ª sesión separada.
- **Próxima acción:** sin cambio. Pendientes: 3 preguntas del lab 02 · DIAG-2 · bloque B1.

---

## Historial comprimido

| # | Fecha | Qué pasó |
| --- | --- | --- |
| 005 | 2026-08-04 | Lab 02 (model catalog) + dataset sintético. Suscripción Azure confirmada. Apunte de **deployment types** (hueco R8) |
| 004 | 2026-08-04 | Auditoría de memoria (memory-engineer): routing por tarea, presupuesto de contexto, políticas de archivado |
| 003 | 2026-08-04 | **DIAG-1: 42%.** 9 errores registrados. D1→Débil, D2→En aprendizaje. Primera evidencia real |
| 002 | 2026-08-04 | Curso oficial AI-103T00-A mapeado: 4 paths, 30 módulos. Detectados 7 huecos (R8), casi todos en D1 |
| 001 | 2026-08-04 | Setup: memoria creada, temario oficial verificado (vigente 2026-04-16), 44 objetivos |

Detalle en `study/archive/SESSION-LOG-2026-08.md`.
