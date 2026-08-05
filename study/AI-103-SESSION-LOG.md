# AI-103 — Bitácora de sesiones

> **Qué contiene:** qué pasó en cada sesión, en orden cronológico inverso.
> **Cuándo leerlo:** para reconstruir historia o entender por qué se decidió algo.
> **Cuándo NO:** en trabajo normal. El estado vive en STUDY-STATE; la evidencia en ERROR-LOG y PRACTICE. Este archivo casi nunca es necesario.

**Política de tamaño:** solo las **últimas 5 sesiones** en detalle (**≤8 líneas** cada una: hecho / resultado / errores / cambios de nivel / próxima acción). Las anteriores colapsan a una fila de § Historial comprimido y se archivan por mes en `study/archive/SESSION-LOG-<AAAA-MM>.md`. No re-narrar aquí lo que ya está en ERROR-LOG o PRACTICE — enlazar.

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

## 2026-08-04 — Sesión 005: Lab 02 + deployment types

- **Hecho:** repasado el lab oficial 02 (model catalog & evaluation) en modo tutor. Aclarada la duda del usuario sobre **generación de dataset sintético** (los 3 orígenes de datos; distinción entre *prompt de generación* y *developer prompt*).
- **Dato nuevo del usuario:** ✅ **tiene suscripción de Azure activa** → los labs son viables. Cierra una de las 3 preguntas abiertas.
- **Hallazgo del usuario:** le preguntaron por **deployment types**. Verificado en docs oficiales (actualizado 2026-05-18) y escrito `notes/D1-deployment-types.md`. Cubre **D1.2.b**, que era un **hueco R8** — el curso oficial no lo trata.
- **Cambios de nivel:** ninguno. D1.2.b marcado "Estudiado, sin evaluar" — leer no es evidencia.
- **Errores:** ninguno nuevo. El apunte de deployment types apoya E-001 (qué define un deployment) y E-002 (standard=TPM vs provisioned=PTU).
- **Próxima acción:** sin cambio. Pendiente: 3 preguntas del lab 02 + 5 del apunte de deployment types + DIAG-2.

---

## Historial comprimido

| # | Fecha | Qué pasó |
| --- | --- | --- |
| 004 | 2026-08-04 | Auditoría de memoria (memory-engineer): routing por tarea, presupuesto de contexto, políticas de archivado |
| 003 | 2026-08-04 | **DIAG-1: 42%.** 9 errores registrados. D1→Débil, D2→En aprendizaje. Primera evidencia real |
| 002 | 2026-08-04 | Curso oficial AI-103T00-A mapeado: 4 paths, 30 módulos. Detectados 7 huecos (R8), casi todos en D1 |
| 001 | 2026-08-04 | Setup: memoria creada, temario oficial verificado (vigente 2026-04-16), 44 objetivos |

Detalle en `study/archive/SESSION-LOG-2026-08.md`.
