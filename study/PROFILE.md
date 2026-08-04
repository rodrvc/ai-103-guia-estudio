# Perfil, fuentes y riesgos

> **Qué contiene:** perfil del estudiante, fuentes oficiales verificadas y mapa de riesgos.
> **Cuándo leerlo:** al planificar un diagnóstico o un bloque de estudio; al verificar vigencia del temario.
> **Cuándo NO:** en sesiones de tutoría o corrección de quiz — no lo necesitas.

---

## Perfil del estudiante

Rodrigo. Ingeniero informático, Full-Stack, +6 años de experiencia.

- **Fuerte (fuera de Azure):** JavaScript, TypeScript, Node.js, NestJS, Express, Python, FastAPI, APIs REST, PostgreSQL, Redis, Docker.
- **Fuerte (IA genérica):** OpenAI API, RAG, embeddings, bases vectoriales, LangChain, LangGraph, LangSmith, n8n, agentes, evaluación y observabilidad.
- **Idioma de trabajo:** español. Términos técnicos y nombres de servicio en inglés.
- **Lenguaje de práctica:** Python (el examen lo asume).

**Estado del perfil como fuente:** para **D1 y D2 ya no aplica** — hay evidencia real de DIAG-1 que lo sustituye. Para **D3, D4 y D5 sigue siendo lo único que hay** hasta que se haga DIAG-2. Usarlo solo como hipótesis, nunca como nivel.

---

## Fuentes oficiales

Dos fuentes con roles distintos. **Ante discrepancia, manda el temario.**

**1. Temario del examen — qué se mide** (autoridad final):
https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103

- Skills measured as of **2026-04-16** · Verificado **2026-08-04**
- Aprobación: **700/1000**. Detalle en `AI-103-SYLLABUS.md`.

**2. Curso oficial — la ruta de contenido:**
https://learn.microsoft.com/en-us/training/courses/ai-103t00 — AI-103T00-A, 4 días / 96 h, 4 learning paths, **30 módulos**.

- Verificado **2026-08-04**. Detalle en `AI-103-LEARNING-PATH.md`.
- **No cubre 1:1 el temario.** D1 queda parcialmente descubierto (cuotas, CI/CD, seguridad). Ver § Cobertura y huecos allí.

### Por qué solo `en-us`

La versión `es-es` es la misma página (mismo commit) pero **traducida a máquina** (`ms.translationtype: MT`), con errores que inducen a error de estudio:

| Traducción `es-es` | Original | Problema |
| --- | --- | --- |
| "modelos multiplataforma" | *multimodal models* | Error grave: multiplataforma ≠ multimodal |
| "alineación de datos" / "puesta en tierra" | *grounding* | El mismo término, dos traducciones distintas |
| "aptitudes" | *skills* (AI Search) | Pierde el término técnico |
| "inpaintación" | *inpainting* | No existe en español |
| "modo pro-tarea" | *pro-mode* | Invierte el sentido |

Además, Microsoft actualiza el inglés primero: las localizaciones van **~8 semanas atrás**.

### Verificación periódica

Si han pasado **>30 días** desde la última verificación, re-leer ambas páginas y actualizar fechas en `AI-103-SYLLABUS.md` y `AI-103-LEARNING-PATH.md`. **No asumir vigencia** — el ecosistema Foundry cambia rápido (Responses API, Foundry IQ, Agent Framework, A2A, Sora 2 son todos recientes).

**Próxima verificación: 2026-09-03.**

---

## Mapa de riesgos

| # | Riesgo | Dominios | Estado |
| --- | --- | --- | --- |
| **R1** | Vocabulario y arquitectura Foundry: el examen pregunta por nombres, límites y decisiones concretas, no conceptos genéricos de IA | D1, D2 (55–65%) | **Confirmado** por DIAG-1 |
| **R2** | Seguridad e identidad Azure: managed identity, keyless, RBAC, private endpoints | D1.3.d | **Confirmado** (E-003) |
| **R3** | Visión y generación de imagen/video: sin experiencia declarada | D3 (10–15%) | Hipótesis — pendiente DIAG-2 |
| **R4** | Voz (Speech, Voice Live): sin experiencia declarada | D4.2 | Hipótesis — pendiente DIAG-2 |
| **R5** | Azure AI Search específico: skillsets, indexers, semantic ranker, híbrida + RRF | D5.1 | **Parcialmente confirmado** (E-005) |
| **R6** | Content Understanding: servicio sin equivalente en su stack, aparece en 3 dominios | D3.2, D5.2 | Hipótesis — pendiente DIAG-2 |
| **R7** | **Falsa confianza por transferencia:** asumir que "sé RAG y agentes" equivale a saber cómo AI-103 lo pregunta en Foundry | D2 | **Confirmado** por DIAG-1 |
| **R8** | El curso oficial no cubre todo D1 (CI/CD, cuotas, salud de índices, seguridad de red) | D1 | Confirmado al mapear los 30 módulos |

**R7 es el riesgo estructural del proyecto.** Rodrigo habla con fluidez de RAG, agentes y evaluación; eso induce a tratarlo como experto y enseñarle al nivel equivocado. Su brecha está en la capa operativa de Azure, no en los conceptos.
