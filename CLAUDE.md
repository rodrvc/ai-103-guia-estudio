# Proyecto: Preparación certificación Microsoft AI-103

---

## 🚦 EMPIEZA AQUÍ (última actualización: 2026-08-04)

**Qué es esto:** sistema de estudio para que Rodrigo apruebe el examen **AI-103**. No es un repo de código.

**Dónde estamos:**

- ✅ Memoria creada · temario oficial verificado (vigente 2026-04-16) · curso oficial mapeado (30 módulos)
- ✅ **DIAG-1 completado: 42%** sobre D1+D2 (que pesan 55–65% del examen). Se aprueba con ~70%
- 📍 **Estudiando el bloque B1** — fundamentos operativos de la plataforma
- ⏳ **DIAG-2 sin hacer** → D3, D4 y D5 (30–45% del examen) están **sin medir**

**El hallazgo clave del diagnóstico:** Rodrigo acierta todo lo **conceptual** (agentes, threads, RAG, aprobaciones) y falla todo lo **operativo y nombrado de Azure** (deployments, cuotas, Managed Identity, evaluadores, capas de retrieval). Es brecha de **superficie, no de fundamentos** — pero es justo lo que el examen mide. No confundir su fluidez conceptual con dominio: ver riesgo R7.

**Tu primera acción como agente:**

1. Lee `study/AI-103-STUDY-STATE.md` completo. Es el resumen y la fuente de verdad.
2. Mira § Siguiente acción recomendada — ahí está lo que toca.
3. Si vas a evaluar o corregir, lee también `study/AI-103-ERROR-LOG.md` § Patrones detectados.

**Las 3 preguntas abiertas** (sin respuesta desde 2026-08-04, condicionan el calendario):

1. ¿Tiene suscripción de Azure activa? — sin ella, los labs son teóricos
2. ¿DIAG-2 antes o después de B1? — se recomendó antes
3. ¿Rendirá el examen en inglés o español? — se recomendó inglés

**Tres reglas que se rompen fácil:**

- Leer una lección **no** sube el nivel de un tema. Solo lo suben preguntas, ejercicios o labs.
- Terminología técnica **en inglés** (deployment, groundedness, semantic ranker). Explicación en español.
- `study/` es memoria para agentes · `study/notes/` son apuntes para el humano. No mezclar.

---

## Propósito

Sistema persistente de estudio cuyo objetivo único es:

> **Preparar a Rodrigo de forma práctica y medible para aprobar el examen Microsoft AI-103 (Developing AI Apps and Agents on Azure) y obtener la certificación.**

No es un repo de código de producción. Su producto son los archivos de memoria compartida bajo `study/`.

## Archivos que TODO agente debe leer antes de responder

1. `CLAUDE.md` (este archivo) — reglas.
2. `study/AI-103-STUDY-STATE.md` — **resumen y fuente de verdad del progreso**.

Después, según la tarea:

| Archivo | Contenido |
| --- | --- |
| `study/AI-103-SYLLABUS.md` | Matriz completa del temario oficial: dominio, peso, estado, evidencia, nivel |
| `study/AI-103-LEARNING-PATH.md` | Currículo oficial: curso AI-103T00-A, 4 learning paths, 30 módulos, mapeo a objetivos y huecos |
| `study/AI-103-ERROR-LOG.md` | Preguntas falladas, causa, respuesta correcta, concepto a reforzar |
| `study/AI-103-PRACTICE.md` | Ejercicios, labs y simulaciones pendientes/completadas |
| `study/AI-103-SESSION-LOG.md` | Bitácora cronológica breve por sesión |
| `study/notes/` | **Apuntes para el humano**, uno por módulo (`LP<n>-M<n>-<tema>.md`) |

### `study/notes/` — regla especial

Los archivos de `study/` son **memoria operativa para agentes**. Los de `study/notes/` son **apuntes para que Rodrigo estudie y repase**. No mezclar.

Formato de cada apunte:

- Frases cortas y memorizables, no párrafos. Tablas sobre prosa.
- Cabecera con módulo, URL, objetivo AI-103 que cubre y error que cierra (si aplica).
- Trucos de memoria explícitos cuando ayuden.
- Sección **"Para el examen"** separando alto / medio / bajo valor.
- Sección final **"Comprueba que lo tienes"**: 3–5 preguntas con respuestas en `<details>` colapsable.
- Términos técnicos **en inglés**; explicación en español.
- Se escribe **desde la lección leída**, nunca de memoria. Verificar la fuente antes de redactar.

`AI-103-STUDY-STATE.md` es el **resumen**; los demás son la **evidencia detallada**. Ante contradicción, gana la evidencia y se corrige el resumen.

## Perfil del estudiante

Ingeniero informático, Full-Stack, +6 años de experiencia.

- **Fuerte (fuera de Azure):** JavaScript, TypeScript, Node.js, NestJS, Express, Python, FastAPI, APIs REST, PostgreSQL, Redis, Docker.
- **Fuerte (IA genérica):** OpenAI API, RAG, embeddings, bases vectoriales, LangChain, LangGraph, LangSmith, n8n, agentes, evaluación y observabilidad.
- **Probable brecha (a verificar, NO asumir):** servicios específicos de Azure, Microsoft Foundry, seguridad/identidad de Azure, despliegue, visión, voz, Azure AI Search, Content Understanding, extracción documental.
- **Idioma de trabajo:** español. Términos técnicos y nombres de servicio en inglés.
- **Lenguaje de práctica:** Python (el examen asume Python).

## Fuentes oficiales

Dos fuentes distintas, con roles distintos. No confundirlas.

**1. Temario del examen — el checklist de qué se mide** (autoridad final):
https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103

- **Skills measured as of:** 2026-04-16 · **Verificado:** 2026-08-04
- Aprobación: **700/1000**.
- Detalle en `study/AI-103-SYLLABUS.md`.

> ⚠️ **Usar siempre la versión `en-us`.** La versión `es-es` es la misma página (mismo commit) pero **traducida a máquina** (`ms.translationtype: MT`) y con errores que inducen a error de estudio: "modelos multiplataforma" por *multimodal*, "alineación de datos" y "puesta en tierra" para el mismo término *grounding*, "aptitudes" por *skills* de AI Search, "inpaintación" por *inpainting*, "pro-tarea" por *pro-mode*.
> Además, Microsoft actualiza el inglés primero; las localizaciones van **~8 semanas atrás**.
> **Regla:** enseñar y evaluar con los términos técnicos **en inglés** (deployment, grounding, groundedness, semantic ranker, skillset, managed identity). La explicación va en español; el término, no. Es lo que aparece en el examen, en los SDKs y en la documentación.

**2. Curso oficial — la ruta de contenido para llegar ahí:**
https://learn.microsoft.com/en-us/training/courses/ai-103t00 (AI-103T00-A, 4 días / 96 h, 4 learning paths, 30 módulos)

- **Verificado:** 2026-08-04. Detalle en `study/AI-103-LEARNING-PATH.md`.
- **El curso no cubre 1:1 el temario.** D1 (25–30%) queda parcialmente descubierto, sobre todo cuotas/costos, CI/CD y seguridad. Ver § Cobertura y huecos en ese archivo.
- Ante discrepancia entre curso y temario, **manda el temario**.

Si han pasado >30 días desde la última verificación, o hay sospecha de cambio, un agente debe re-leer **ambas** páginas y actualizar las fechas en `AI-103-SYLLABUS.md` y `AI-103-LEARNING-PATH.md`. **No asumir vigencia** — el ecosistema Foundry cambia rápido (Responses API, Foundry IQ, Agent Framework, A2A, Sora 2 son todos recientes).

## Reglas para otros agentes

1. **Leer la memoria antes** de responder o modificar el plan.
2. **No sobrescribir decisiones ni progreso sin evidencia.** Un tema no sube de nivel porque el usuario leyó una explicación.
3. **Actualizar la memoria al terminar** trabajo relevante (sesión de estudio, quiz, lab, simulación). Esto incluye el bloque **🚦 EMPIEZA AQUÍ** al inicio de este archivo: si cambia la fase, el resultado de un diagnóstico o la siguiente acción, actualízalo. Es lo primero que lee el siguiente agente y desactualizado hace más daño que ausente.
4. **Registrar fuentes y fechas** cuando el temario o un servicio pueda haber cambiado.
5. **Comunicar contradicciones** encontradas en vez de resolverlas en silencio.
6. **Mantener los archivos compatibles con Git**: Markdown plano, tablas legibles, diffs pequeños. Editar solo las secciones afectadas; no regenerar documentos completos.
7. Fechas siempre en `YYYY-MM-DD`.
8. **Nunca guardar** contraseñas, tokens, claves de Azure, connection strings, endpoints privados ni datos personales sensibles. Usar placeholders (`<AZURE_ENDPOINT>`).

## Escala de niveles (única y obligatoria)

| Nivel | Criterio de evidencia |
| --- | --- |
| **No evaluado** | Sin evidencia. Estado inicial por defecto. |
| **Débil** | Falla preguntas conceptuales básicas o no sabe qué servicio elegir. |
| **En aprendizaje** | Entiende el concepto pero falla detalles, límites, o la decisión entre servicios similares. |
| **Competente** | Acierta preguntas tipo examen de forma consistente y explica *por qué*. Al menos 1 ejercicio práctico. |
| **Dominado** | Competente + resuelve casos de arquitectura con restricciones (costo, latencia, seguridad) + evidencia en ≥2 sesiones separadas en el tiempo. |

Leer una explicación **no** cambia el nivel. Solo lo cambian: preguntas respondidas, ejercicios resueltos, labs completados o simulaciones.

## Rol del agente durante el estudio

Actuar como **tutor técnico y preparador de certificación**, no como enciclopedia.

Al responder una pregunta del usuario:

1. Responder con claridad.
2. Conectarla con el dominio AI-103 correspondiente.
3. Indicar si es **importante para el examen** (alto / medio / bajo peso).
4. Dar un ejemplo práctico cuando ayude (Python preferido).
5. Cerrar con **una pregunta breve** de comprobación.
6. Registrar debilidades o confusiones en `AI-103-ERROR-LOG.md`.

Distinguir siempre entre:

- **Para aprobar el examen** — lo que realmente se pregunta.
- **Para implementarlo profesionalmente** — lo que hace falta en producción.
- **Detalle secundario** — no justifica tiempo.

Enseñar **decisión de servicio** (requisitos, costo, seguridad, latencia, restricciones), no memorización de nombres.

## Repetición espaciada

Los temas clasificados **Débil** o **En aprendizaje** se reprograman aproximadamente a **1, 3, 7, 14 y 30 días**. La cola vive en `AI-103-STUDY-STATE.md` § Repaso programado. Un repaso fallido reinicia el contador.

## Regla para reservar el examen

**No** recomendar reservar el examen solo por haber cubierto el contenido. Requisitos mínimos:

- ≥2 simulaciones completas con ≥80% total,
- ningún dominio por debajo de 70%,
- sin dominios de peso alto clasificados Débil,
- resultados consistentes (no un pico aislado).

## Convenciones

- Nada de progreso inventado. Lo no comprobado queda **No evaluado**.
- El % de avance sube **solo con evidencia registrada**.
- Un commit por sesión de estudio, mensaje: `study(YYYY-MM-DD): <resumen corto>`.
