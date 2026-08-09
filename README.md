# Guía de estudio AI-103

Material en **español** para preparar la certificación **Microsoft AI-103: Developing AI Apps and Agents on Azure**.

Apuntes escritos leyendo la documentación oficial, pensados para entender y aprobar — no para memorizar nombres.

> ⚠️ **Material no oficial.** Escrito a partir de la documentación pública de Microsoft Learn. La fuente de verdad es siempre [la guía oficial del examen](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103). Los servicios de Azure cambian; verifica antes de examinarte.

---

## Empieza aquí

**→ [`study/notes/INDEX.md`](study/notes/INDEX.md)**

Ahí está la ruta de lectura recomendada, los apuntes por dominio y una tabla de **confusiones frecuentes** con el apunte que resuelve cada una.

---

## Cómo está escrito cada apunte

| Sección | Qué contiene |
| --- | --- |
| **En cristiano** | La idea con una analogía y un caso concreto, sin jerga |
| **El contenido** | Tablas, comparaciones y reglas de decisión |
| **Para el examen** | Qué es de **alto / medio / bajo** valor. No todo pesa igual |
| **Comprueba que lo tienes** | Preguntas de **aplicación** con respuestas plegadas |

Dos principios detrás del material:

- **Analogía → decisión en cristiano → nombre técnico en inglés al final.** Los nombres de servicio se mantienen en inglés porque así aparecen en el examen; la explicación va en español.
- **Enseñar decisión de servicio, no memorización.** *"¿Cuál eliges si necesitas latencia baja y los datos no pueden salir de la UE?"* rinde más que *"¿qué es X?"*.

---

## Cobertura

**15 apuntes ≈ 26 de los 44 objetivos del temario (~59%).**

| Dominio | Peso en el examen | Estado |
| --- | --- | --- |
| **D1** Plan and manage an Azure AI solution | 25–30% | ✅ **Cubierto** |
| **D2** Implement generative AI and agentic solutions | 30–35% | ✅ **Cubierto** |
| **D3** Computer vision | 10–15% | ❌ Sin material |
| **D4** Text analysis | 10–15% | ❌ Sin material |
| **D5** Information extraction | 10–15% | ❌ Sin material |

**No es una guía completa.** D1 y D2 (el 55–65% del examen) están cubiertos; **D3, D4 y D5 son el hueco principal**. Detalle objetivo por objetivo en [`study/AI-103-SYLLABUS.md`](study/AI-103-SYLLABUS.md).

---

## Llevarlo a otra herramienta

Markdown plano, sin dependencias ni frontmatter raro. Arrastra `study/notes/` a **NotebookLM**, ábrelo como vault de **Obsidian**, o expórtalo a PDF.

---

## Si usas Claude Code

El repo incluye tres skills que automatizan el estudio:

| Escribes... | Qué hace |
| --- | --- |
| *"explícame X"* / *"estoy en este módulo: `<url>`"* | Lee la fuente oficial, explica con un caso concreto y escribe el apunte |
| *"repasemos deployment types"* | Pregunta, corrige y registra el resultado como evidencia |
| *"¿dónde quedamos?"* | Resumen del estado en 5 líneas |

**Regla del sistema:** leer un apunte **no** sube tu nivel. Solo lo suben preguntas respondidas, ejercicios y labs. Los archivos `study/AI-103-*.md` son la memoria de seguimiento que mantienen los agentes.

---

## Estructura

```
study/
  notes/                 ← 📚 EL MATERIAL DE ESTUDIO
    INDEX.md             ← empieza aquí
    D1-plataforma/
    D2-apps-y-agentes/
  AI-103-SYLLABUS.md     ← los 44 objetivos oficiales
  AI-103-*.md            ← seguimiento de progreso
.claude/skills/          ← automatizaciones para Claude Code
CLAUDE.md                ← instrucciones para los agentes
```

---

## Fuentes

- [Temario oficial del examen](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103) — vigente 2026-04-16 · se aprueba con 700/1000
- [Curso oficial AI-103T00-A](https://learn.microsoft.com/en-us/training/courses/ai-103t00) — 4 learning paths, 30 módulos, ~96 h

**Usa siempre la versión `en-us`** de Microsoft Learn: la traducción al español es automática y tiene errores de término que inducen a confusión.

---

## Licencia

[MIT](LICENSE) para el material propio. Los nombres de productos y servicios de Microsoft pertenecen a sus titulares; este proyecto no está afiliado a Microsoft.
