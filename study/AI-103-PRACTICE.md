# AI-103 — Práctica: ejercicios, labs y simulaciones

> **Qué contiene:** quizzes, diagnósticos, labs y simulaciones — pendientes y hechos, con sus resultados. **Fuente canónica de los puntajes.**
> **Cuándo leerlo:** al corregir o preparar una prueba (solo la que toca) · al planificar simulaciones.
> **Cuándo NO:** entero. Ve al ítem concreto.
>
> **Regla de reserva del examen:** ≥2 simulaciones completas con ≥80%, ningún dominio <70%, sin dominios de peso alto en Débil, y resultados consistentes (no un pico aislado). **No recomendar reservar solo por haber cubierto el contenido.**

## Estado general

| Tipo | Pendientes | Completados |
| --- | --- | --- |
| Quiz / diagnóstico | 2 | 0 |
| Laboratorio | 0 | 0 |
| Caso de arquitectura | 0 | 0 |
| Simulación completa | 0 | 0 |

---

## Diagnóstico inicial

### DIAG-1 — Dominios D1 + D2 (55–65% del examen) · 12 preguntas

- **Estado:** ✅ Completado 2026-08-04
- **Publicado:** 2026-08-04
- **Resultado: 4 plenos / 4 parciales / 4 fallos ≈ 42%** (parciales cuentan 0.5)
- **Objetivo:** verificar si la experiencia en RAG/agentes fuera de Azure transfiere a la terminología y decisiones de Foundry (riesgos R1 y R7).
- **Formato:** respuesta breve. No hay opciones múltiples a propósito — adivinar entre 4 opciones esconde las brechas.
- **Instrucción para el usuario:** responde con lo que sepas. **"No sé" es una respuesta válida y útil**; una respuesta inventada contamina el diagnóstico y te hará estudiar lo equivocado.

**Preguntas:**

1. **(D1.1.b)** Necesitas que un agente responda preguntas sobre 50.000 PDFs internos, con citas a la fuente. ¿Qué servicios de Azure combinarías y cuál es el rol de cada uno?
2. **(D1.1.a)** ¿Cuándo elegirías un *small language model* en lugar de un LLM en Azure? Da dos criterios concretos.
3. **(D1.2.c)** En Azure OpenAI / Foundry, ¿qué es un *deployment* y en qué se diferencia del *model*? ¿Qué nombre usas al llamar a la API?
4. **(D1.3.a)** ¿En qué unidad se mide y limita la capacidad de un deployment de Azure OpenAI? ¿Qué pasa exactamente cuando la superas y cómo debe reaccionar tu cliente?
5. **(D1.3.d)** Tu API en Python corre en Azure y llama a Azure OpenAI. Política de seguridad: **cero claves en configuración**. ¿Cómo te autenticas? Nombra el mecanismo de Azure y la clase del SDK de Python que usarías.
6. **(D1.4.a)** ¿Qué son las cuatro categorías de daño de los filtros de contenido de Azure OpenAI y qué niveles de severidad puedes configurar? Si no sabes los nombres exactos, di qué crees que cubren.
7. **(D1.4.a)** ¿Qué diferencia hay entre un *content filter* y un *blocklist* en Azure OpenAI? ¿Cuándo usarías cada uno?
8. **(D2.1.b)** En una app RAG, ¿cuál es la diferencia entre búsqueda vectorial pura, búsqueda híbrida y *semantic ranking*? ¿Cuál añade más latencia y costo?
9. **(D2.2.c)** En el Agent Service de Foundry, ¿qué tipos de herramientas puedes darle a un agente? Nombra las que recuerdes.
10. **(D2.2.b)** ¿Qué son un *thread*, un *run* y un *message* en el modelo de agentes de Azure? ¿Quién persiste el historial de conversación: tu app o el servicio?
11. **(D2.2.e)** Un agente puede emitir reembolsos. Debe pedir aprobación humana antes de ejecutar. ¿Cómo lo implementas en el ciclo de vida del agente? ¿En qué momento se interrumpe?
12. **(D2.1.d)** Nombra tres métricas de evaluación que Azure AI Foundry ofrece de fábrica para apps generativas. ¿Cuál detecta alucinaciones?

**Resultado detallado (2026-08-04):**

| # | Obj. | Veredicto | Nota |
| --- | --- | --- | --- |
| 1 | D1.1.b | ✅ Pleno | Nombró Foundry IQ + SharePoint/fuentes externas. Respuesta actualizada |
| 2 | D1.1.a | ⚠️ Parcial | Criterio válido (decisiones simples) pero faltan los dos canónicos: costo/latencia y edge/on-device |
| 3 | D1.2.c | ❌ Fallo | Confunde *deployment* con el playground. No sabe que la API se llama con el **nombre del deployment** |
| 4 | D1.3.a | ❌ Fallo | "No lo sé". Desconoce TPM, 429/Retry-After, backoff y PTU |
| 5 | D1.3.d | ⚠️ Parcial | Intuye Azure pero no nombra Managed Identity, RBAC ni `DefaultAzureCredential` |
| 6 | D1.4.a | ❌ Fallo | "No lo sé". Desconoce las 4 categorías y los 4 niveles de severidad |
| 7 | D1.4.a | ✅ Pleno | Distinción filter (clasificador ML) vs blocklist (match literal) correcta |
| 8 | D2.1.b | ⚠️ Parcial | **Invierte el concepto**: atribuye lo semántico a la híbrida en vez de al vector. Omite BM25, RRF y semantic ranker |
| 9 | D2.2.c | ⚠️ Parcial | Menciona tools y preconfiguraciones; incluye voz que no es tool. Faltan Code Interpreter, OpenAPI, MCP, Bing grounding |
| 10 | D2.2.b | ✅ Pleno | Acierta lo clave: **el servicio persiste el thread**, no la app |
| 11 | D2.2.e | ✅ Pleno (concepto) | Idea correcta de human-in-the-loop. Falta el mecanismo: `requires_action` → `submit_tool_outputs` |
| 12 | D2.1.d | ❌ Fallo | Confunde observabilidad con evaluación. No conoce groundedness |

**Lectura:** el patrón es nítido. Todo lo **conceptual** (agentes, threads, aprobaciones, filtros) lo tiene; todo lo **operativo y nombrado de Azure** (deployments, cuotas, identidad, evaluadores, mecánica de retrieval) no. Confirma R1 y R7.

---

### DIAG-1B — Repesca de los 4 fallos + 4 parciales

- **Estado:** 📋 Pendiente
- **Cuándo:** tras estudiar el bloque B1
- **Alcance:** preguntas 2, 3, 4, 5, 6, 8, 9, 12 reformuladas. Verifica si el estudio cerró las brechas o solo dio familiaridad.

---

### DIAG-2 — Dominios D3 + D4 + D5 (30–45% del examen) · 8 preguntas

- **Estado:** 🔒 Bloqueado hasta completar DIAG-1
- **Objetivo:** medir la brecha real en visión, voz y extracción documental (riesgos R3, R4, R5, R6).

---

## Laboratorios pendientes

*Se definen tras el diagnóstico. Requieren confirmar si hay suscripción de Azure activa (ver `AI-103-STUDY-STATE.md` § Información faltante, punto 3).*

---

## Casos de arquitectura pendientes

*Se definen tras el diagnóstico.*

---

## Simulaciones

| ID | Alcance | Fecha | Total | D1 | D2 | D3 | D4 | D5 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| *(ninguna todavía)* | | | | | | | | |

Regla de reserva del examen: ver `CLAUDE.md` § Regla para reservar el examen.

---

## Completados

*Ninguno todavía.*
