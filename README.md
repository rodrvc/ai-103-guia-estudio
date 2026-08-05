# Preparación AI-103

Sistema de estudio para aprobar el examen **Microsoft AI-103: Developing AI Apps and Agents on Azure**.

> **Este archivo es para ti (Rodrigo).** Los agentes leen `CLAUDE.md`.

---

## 📚 Tu material de estudio

Todo está en **[`study/notes/`](study/notes/INDEX.md)**, organizado por tema.

Cada apunte tiene la misma estructura:

1. **En cristiano** — la idea con analogías, sin jerga
2. **El contenido** — tablas, comparaciones, reglas de decisión
3. **Para el examen** — qué es alto / medio / bajo valor
4. **Comprueba que lo tienes** — preguntas con respuestas plegadas

Empieza por **[el índice](study/notes/INDEX.md)**: ahí ves qué hay, qué falta y en qué nivel estás de cada tema.

### Llevarlo a otra herramienta

Markdown plano, sin dependencias. Arrastra `study/notes/` a **NotebookLM**, ábrelo como vault de **Obsidian**, o exporta a PDF.

> ⚠️ **Cobertura actual: ~16% del temario** (6 apuntes ≈ 7 de 44 objetivos).
> **D3 (visión), D4 (texto y voz) y D5 (extracción) no tienen material** — y son el 30–45% del examen.
> Llevarlo hoy a NotebookLM te da una base sólida de deployment types y de cómo conectar una app a Foundry. Nada más. Ver la cobertura real en el [índice](study/notes/INDEX.md).

---

## 🗣️ Cómo usar los agentes

Escribe lo que quieras hacer. Los skills se activan solos.

| Le dices... | Qué pasa |
| --- | --- |
| *"repasemos deployment types"* | Te hace preguntas del apunte, corrige y **sube tu nivel si aciertas** |
| *"pregúntame lo del día"* | Toma los errores con repaso vencido |
| *"estoy en este módulo: `<url>`"* | Lee la fuente, te explica y escribe el apunte |
| *"¿dónde quedamos?"* | Resumen en 5 líneas |
| *"¿cómo voy?"* | Progreso con evidencia |

**Regla del sistema:** leer un apunte **no** sube tu nivel. Solo lo suben preguntas respondidas, ejercicios y labs. Por eso *"repasemos X"* es el comando que más importa.

---

## 📊 Dónde ves tu progreso

| Archivo | Qué te dice |
| --- | --- |
| [`study/notes/INDEX.md`](study/notes/INDEX.md) | Material por tema y tu nivel en cada uno |
| [`study/AI-103-STUDY-STATE.md`](study/AI-103-STUDY-STATE.md) | Resumen: fase, métricas, siguiente acción |
| [`study/AI-103-SYLLABUS.md`](study/AI-103-SYLLABUS.md) | Los 44 objetivos oficiales con tu nivel en cada uno |
| [`study/AI-103-ERROR-LOG.md`](study/AI-103-ERROR-LOG.md) | Lo que fallaste, por qué, y cuándo toca repasarlo |
| [`study/AI-103-LEARNING-PATH.md`](study/AI-103-LEARNING-PATH.md) | Los 30 módulos del curso oficial y cuáles llevas |

---

## 🎯 Estado actual · 2026-08-05

- **LP1:** 3 de 6 módulos (45%). Vas por el módulo 4, *apps that use tools*
- **DIAG-1:** 42% en D1+D2 (se aprueba con ~70%)
- **DIAG-2:** sin hacer → D3, D4 y D5 (**30–45% del examen**) sin medir
- **Material:** ~16% del temario. **9 errores** abiertos, ninguno repasado
- **Examen:** en inglés · 10 h/semana disponibles

### Calendario

Planteaste rendir en ~2 semanas. **Con los datos actuales no llegas:** son ~20 h contra 96 h que estima el curso oficial, con 3 dominios sin medir y 9% de material escrito.

**Estimación realista: 6–8 semanas** → mediados de septiembre.

No se recomienda reservar el examen hasta tener ≥2 simulaciones con ≥80% y ningún dominio por debajo de 70%.

### Lo que más rinde ahora

1. **DIAG-2** (30 min) — deja de tener el 30–45% del examen a ciegas
2. **Repasar** — tienes 4 apuntes y ninguno verificado con preguntas
3. **Escribir material de D3/D4/D5** — están vacíos

---

## 📁 Estructura

```
README.md              ← estás aquí (para ti)
CLAUDE.md              ← reglas para los agentes
study/
  notes/               ← 📚 TU MATERIAL DE ESTUDIO
    INDEX.md           ← empieza aquí
    D1-plataforma/
    D2-apps-y-agentes/
  AI-103-*.md          ← seguimiento (progreso, errores, temario)
  archive/             ← histórico rotado
.claude/skills/        ← automatizaciones
```

---

## ⚙️ Fuentes oficiales

- [Temario del examen](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-103) — vigente 2026-04-16, aprobación 700/1000
- [Curso AI-103T00-A](https://learn.microsoft.com/en-us/training/courses/ai-103t00) — 4 learning paths, 30 módulos

Usa siempre la versión **`en-us`**: la traducción al español es automática y tiene errores que inducen a confusión (traduce *multimodal* como "multiplataforma").
