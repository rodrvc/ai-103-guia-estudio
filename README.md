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

Los apuntes son Markdown plano, sin dependencias. Puedes:

- Arrastrar `study/notes/` completo a **NotebookLM** (o solo la carpeta de un dominio)
- Abrir la carpeta como vault de **Obsidian**
- Imprimir o exportar a PDF cualquier apunte

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
- **9 errores** abiertos, ninguno repasado todavía

**Lo que más rinde ahora:** hacer DIAG-2 (30 min) y empezar a repasar. Tienes 4 apuntes escritos y ninguno verificado con preguntas.

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
