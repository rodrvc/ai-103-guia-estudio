# AI-103 — Bitácora de sesiones

> **Qué contiene:** qué pasó en cada sesión, en orden cronológico inverso.
> **Cuándo leerlo:** para reconstruir historia o entender por qué se decidió algo.
> **Cuándo NO:** en trabajo normal. El estado vive en STUDY-STATE; la evidencia en ERROR-LOG y PRACTICE. Este archivo casi nunca es necesario.

**Política de tamaño:** solo las **últimas 5 sesiones** en detalle (**≤8 líneas** cada una: hecho / resultado / errores / cambios de nivel / próxima acción). Las anteriores colapsan a una fila de § Historial comprimido y se archivan por mes en `study/archive/SESSION-LOG-<AAAA-MM>.md`. No re-narrar aquí lo que ya está en ERROR-LOG o PRACTICE — enlazar.

---

## Historial comprimido

| # | Fecha | Qué pasó |
| --- | --- | --- |
| *(vacío — se llena al rotar la sexta sesión)* | | |

---

---

## 2026-08-04 — Sesión 005: Lab 02 + deployment types

- **Hecho:** repasado el lab oficial 02 (model catalog & evaluation) en modo tutor. Aclarada la duda del usuario sobre **generación de dataset sintético** (los 3 orígenes de datos; distinción entre *prompt de generación* y *developer prompt*).
- **Dato nuevo del usuario:** ✅ **tiene suscripción de Azure activa** → los labs son viables. Cierra una de las 3 preguntas abiertas.
- **Hallazgo del usuario:** le preguntaron por **deployment types**. Verificado en docs oficiales (actualizado 2026-05-18) y escrito `notes/D1-deployment-types.md`. Cubre **D1.2.b**, que era un **hueco R8** — el curso oficial no lo trata.
- **Cambios de nivel:** ninguno. D1.2.b marcado "Estudiado, sin evaluar" — leer no es evidencia.
- **Errores:** ninguno nuevo. El apunte de deployment types apoya E-001 (qué define un deployment) y E-002 (standard=TPM vs provisioned=PTU).
- **Próxima acción:** sin cambio. Pendiente: 3 preguntas del lab 02 + 5 del apunte de deployment types + DIAG-2.

---

## 2026-08-04 — Sesión 004: Auditoría de memoria (memory-engineer)

- **Hecho:** auditoría de arquitectura de contexto por el agente `memory-engineer`. Aplicado lo urgente.
- **Podas:** `CLAUDE.md` 173 → ~110 líneas. Perfil, fuentes y riesgos R1–R8 → `study/PROFILE.md`. Formato de apuntes → `notes/_TEMPLATE.md`. Regla de reserva → PRACTICE. Duplicados eliminados en STUDY-STATE (§Hipótesis, §Temas débiles, §Lectura comprimida).
- **Routing:** tabla de archivos sustituida por **tabla por tarea con columna "NO leas"**. Cabecera de 3 líneas (qué / cuándo sí / cuándo no) en los 5 archivos de `study/`.
- **Anti-crecimiento:** techo de 200 líneas por archivo · SESSION-LOG solo 5 sesiones en detalle, resto a `study/archive/` · error cerrado = 1 línea · `notes/INDEX.md` para que los agentes no lean el directorio (proyección: 30 módulos ≈ 4800 líneas).
- **Robustez:** auto-chequeo de frescura en EMPIEZA AQUÍ · tercer estado ✗ en repaso espaciado (antes no se podía registrar un fallo) · reglas de integridad repetidas en SYLLABUS/STUDY-STATE por si un agente no carga `CLAUDE.md`.
- **Sesgo marcado:** la conclusión "brecha de superficie" ahora lleva `n=12, fuente única, revisar tras DIAG-1B`. El sistema aplicaba R7 al usuario pero no a sí mismo.
- **Sin cambios** de nivel, errores ni progreso. **Próxima acción:** sin cambio — DIAG-2 o bloque B1.

---

## 2026-08-04 — Sesión 003: DIAG-1 completado — primera evidencia real

- **Hecho:** el usuario respondió las 12 preguntas de DIAG-1 (D1+D2). Corregidas una a una con la respuesta esperada.
- **Resultado: 4 plenos / 4 parciales / 4 fallos ≈ 42%.** Por debajo del umbral de aprobación (~70%), sobre los dominios que pesan 55–65% del examen.
- **Aciertos plenos:** p1 Foundry IQ + fuentes externas · p7 filter vs blocklist · p10 thread/run/message con persistencia en el servicio · p11 patrón human-in-the-loop.
- **Fallos:** p3 deployment (lo confunde con el playground) · p4 cuotas ("no lo sé") · p6 content filters ("no lo sé") · p12 evaluadores (los confunde con observabilidad).
- **Parciales:** p2 SLM · p5 auth sin claves · p8 retrieval · p9 catálogo de tools.
- **Patrón identificado:** acierta todo lo **conceptual y de diseño**; falla todo lo **operativo y nombrado de Azure**. Brecha de superficie, no de fundamentos. Confirma R1 y R7 con evidencia.
- **Señal positiva:** cero errores de lectura o descarte. Respondió "no lo sé" tres veces en vez de inventar → diagnóstico fiable, sin sobreconfianza que corregir.
- **Errores registrados:** 9 (E-001 … E-009), todos con calendario de repaso 1/3/7/14/30 d desde 2026-08-05.
- **Cambios de nivel:** 11 de 44 objetivos pasan de "No evaluado" a evaluado.
  - **Competente:** D2.2.b (único).
  - **En aprendizaje:** D1.1.a, D1.1.b, D1.4.a, D2.1.b, D2.2.c, D2.2.e.
  - **Débil:** D1.2.c, D1.3.a, D1.3.d, D2.1.d.
  - **Por dominio:** D1 → Débil · D2 → En aprendizaje.
- **Próxima acción:** estudiar el bloque **B1 "Fundamentos operativos de la plataforma"** (deployments, cuotas, identidad, filtros, evaluadores, capas de retrieval), cerrar con DIAG-1B, luego DIAG-2.

---

## 2026-08-04 — Sesión 002: Ruta oficial de aprendizaje incorporada

- **Aportado por el usuario:** el curso oficial AI-103T00-A como ruta de la certificación.
- **Hecho:**
  - Verificado el curso **AI-103T00-A** *Develop AI apps and agents on Azure* (4 días / 96 h, actualizado 2026-04-17). El outline se carga por JS, así que los learning paths se extrajeron del metadata `learn_item` y se verificó cada uno por separado.
  - Confirmados **4 learning paths / 30 módulos**: LP1 generative AI (6), LP2 agentes (9), LP3 lenguaje y voz (7), LP4 visual data (8).
  - Creado `AI-103-LEARNING-PATH.md` con los 30 módulos, checkboxes de avance y mapeo módulo → objetivo del temario.
  - **Análisis de cobertura:** el curso NO cubre 1:1 el temario. Detectados 7 huecos, concentrados en D1 (25–30%): CI/CD con Foundry, cuotas/TPM/costos, salud de índices, seguridad de red e identidad, gobierno de agentes; y en D3.3: prompt injection en imágenes y reglas de política visual.
  - Registrado **R8** (huecos del curso en D1) en el mapa de riesgos.
  - **R1 reforzado con evidencia:** el temario asume nomenclatura Microsoft muy reciente — Responses API, Foundry IQ, Microsoft Agent Framework, A2A, MCP servers de Azure, Work IQ, Sora 2, Content Understanding. Ninguno tiene equivalente 1:1 en LangChain/LangGraph.
  - `CLAUDE.md` actualizado: dos fuentes oficiales con roles distintos; ante discrepancia manda el temario.
- **Evaluado:** nada. Sigue sin haber evidencia de nivel.
- **Errores registrados:** 0.
- **Cambios de nivel:** ninguno.
- **Próxima acción:** sin cambio — el usuario responde DIAG-1.

---

## 2026-08-04 — Sesión 001: Setup del sistema de estudio

- **Duración:** setup (sin estudio efectivo)
- **Hecho:**
  - Creada la estructura de memoria persistente: `CLAUDE.md` + 5 archivos en `study/`.
  - Verificado el temario oficial de AI-103 en la fuente de Microsoft Learn. Vigente: **"Skills measured as of April 16, 2026"** (página actualizada 2026-07-07). Sin aviso de actualización pendiente.
  - Matriz del temario creada: 5 dominios, 44 objetivos, todos en estado **No evaluado**.
  - Registrado el perfil inicial del usuario (Full-Stack, +6 años, fuerte en IA generativa fuera de Azure).
  - Redactado DIAG-1 (12 preguntas sobre D1+D2) y publicado en `AI-103-PRACTICE.md`.
- **Evaluado:** nada. Sin evidencia de nivel todavía.
- **Errores registrados:** 0.
- **Cambios de nivel:** ninguno.
- **Próxima acción:** el usuario responde DIAG-1.

---
