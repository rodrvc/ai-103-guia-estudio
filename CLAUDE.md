# Proyecto: Preparación certificación Microsoft AI-103

---

## 🚦 EMPIEZA AQUÍ · actualizado 2026-08-04

> Si hoy es **>7 días** después de esa fecha, este bloque puede estar obsoleto: lee `study/AI-103-STUDY-STATE.md` antes de confiar en él, y actualízalo.

**Qué es esto:** sistema de estudio para que Rodrigo apruebe el examen **AI-103**. No es un repo de código.

**Fase:** DIAG-1 hecho (**42%** en D1+D2, se aprueba con ~70%). **Ahora: estudio activo del curso oficial**, guiado por el usuario — trae URLs de módulos y se le escribe apunte en `notes/` + preguntas de comprobación. **DIAG-2 pendiente** → D3/D4/D5 (30–45% del examen) sin medir.

**Cómo trabaja el usuario (validado 2026-08-04):** trae un link de Microsoft Learn o docs → se lee la fuente (nunca de memoria) → apunte en `notes/` con analogías primero y el nombre técnico al final → preguntas de comprobación → si responde, se evalúa y se actualiza SYLLABUS. Si pasa un link en `es-es`, usar la versión `en-us`.

**Progreso real:** 2 objetivos en Competente (D2.2.b, **D1.2.b**) · 4 apuntes escritos · 9 errores abiertos de DIAG-1 **sin repasar aún**.

⚠️ **Antes de escribir un apunte, lee el índice del módulo** (`.../modules/<modulo>/`), no solo la unidad que pasa el usuario. El apunte cubre el **módulo**. Detalle en el skill.

**Hallazgo clave (n=12, revisar tras DIAG-1B):** acierta lo **conceptual**, falla lo **operativo de Azure** (deployments, cuotas, managed identity, evaluadores, retrieval). Brecha de superficie, no de fundamentos — pero es lo que el examen mide. **No confundir su fluidez conceptual con dominio** → riesgo R7.

**Primera acción:** `study/AI-103-STUDY-STATE.md` § Siguiente acción. Luego, según tarea, ver § Qué leer.

**Al terminar cualquier bloque de estudio** (apunte escrito, respuestas corregidas, lab hecho, tema nuevo, fin de sesión): invoca el skill **`study-tracker`**. Registra el progreso y sincroniza la memoria sin que haya que pedirlo. También responde "¿dónde quedamos?".

**Azure:** ✅ suscripción activa (2026-08-04) — los labs son viables.

**Preguntas abiertas** (bloquean el calendario): ¿DIAG-2 antes de B1? (recomendado sí) · ¿examen en inglés o español? (recomendado inglés) · fecha objetivo y horas/semana.

**Reglas que se rompen fácil:**

- Leer una lección **no** sube el nivel. Solo lo suben preguntas, ejercicios o labs con evidencia.
- Terminología técnica **en inglés** (deployment, groundedness, semantic ranker); explicación en español.
- `study/` = memoria de agentes · `study/notes/` = apuntes del humano. No mezclar.

---

## Qué leer según la tarea

**La columna "NO leas" es obligatoria, no una sugerencia.** Leer de más es el fallo por defecto.

| Vas a... | Lee (en orden) | NO leas |
| --- | --- | --- |
| Retomar / no sé qué toca | STUDY-STATE § Siguiente acción | nada más hasta decidir |
| Corregir un quiz o diagnóstico | ERROR-LOG § Patrones + PRACTICE (solo ese quiz) | SYLLABUS entero |
| Registrar un error nuevo | ERROR-LOG (cabecera de formato) | — |
| Planificar el siguiente bloque | STUDY-STATE + SYLLABUS § Resumen de pesos + LEARNING-PATH § Cobertura | ERROR-LOG detallado |
| Escribir apuntes de un módulo | `notes/_TEMPLATE.md` + la lección fuente | `study/` entero |
| Actualizar niveles tras evidencia | SYLLABUS (solo el dominio tocado) + STUDY-STATE § Progreso | — |
| Preparar simulación | PRACTICE + SYLLABUS § Resumen de pesos | `notes/` |
| Repaso espaciado del día | STUDY-STATE § Repaso programado + los E-NNN de hoy | resto de ERROR-LOG |
| Verificar vigencia del temario | SYLLABUS + LEARNING-PATH (cabeceras de fecha) | — |

Rutas: todo bajo `study/` con prefijo `AI-103-`. Detalle de perfil, fuentes y riesgos: `study/PROFILE.md`.

### `study/` vs `study/notes/`

- `study/*.md` = **memoria operativa para agentes**.
- `study/notes/*.md` = **apuntes para que Rodrigo estudie**. Un agente lee `notes/INDEX.md`, **nunca el directorio entero**; abre una nota concreta solo si el usuario la nombra o va a actualizarla. Plantilla y formato: `notes/_TEMPLATE.md`.

`AI-103-STUDY-STATE.md` es el **resumen**; los demás son la **evidencia detallada**. Ante contradicción, gana la evidencia y se corrige el resumen.

## Presupuesto de contexto

- **Ningún archivo de `study/` supera 200 líneas.** Al superarlo: compactar o archivar a `study/archive/`.
- **SESSION-LOG:** solo las **últimas 5 sesiones** en detalle (≤8 líneas). El resto se archiva por mes.
- **ERROR-LOG:** error **cerrado** = una línea de tabla. Los abiertos conservan el detalle (son material de repaso).
- **`notes/`** no se compacta; se accede solo por `INDEX.md`.

Detalle del procedimiento de registro: skill **`study-tracker`**.

## Reglas para otros agentes

1. **Leer la memoria antes** de responder o modificar el plan.
2. **No sobrescribir progreso sin evidencia.** Un tema no sube de nivel porque el usuario leyó algo.
3. **Registrar al terminar** cualquier bloque de estudio → skill **`study-tracker`**.
4. **Registrar fuentes y fechas** cuando el temario o un servicio pueda haber cambiado.
5. **Comunicar contradicciones** en vez de resolverlas en silencio.
6. **Diffs pequeños.** Markdown plano, editar solo lo afectado, no regenerar documentos.
7. Fechas en `YYYY-MM-DD`.
8. **Nunca guardar** claves, tokens, connection strings, endpoints privados ni datos sensibles. Usar `<AZURE_ENDPOINT>`.

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

### Analogía primero, nombre después (validado 2026-08-04)

Rodrigo pidió explícitamente "bájalo a algo entendible". Al reexplicar deployment types con analogías cotidianas (**taxi** = pay-per-token · **auto propio con chofer** = provisioned · **encomienda** = batch; **tu casa vs. la lavandería** = datos guardados vs. procesados) pasó de no entender a resolver 3/3 casos de decisión, incluido el que había fallado en jerga.

**Método:** (1) analogía concreta → (2) la decisión en cristiano → (3) recién ahí el nombre técnico en inglés. El nombre se aprende **después** de que el concepto está firme, nunca antes ni en lugar de él.

No confundir esto con simplificar el contenido: la profundidad es la misma, cambia la puerta de entrada. Los términos siguen siendo obligatorios en inglés (ver § arranque).

## Repetición espaciada

Los temas clasificados **Débil** o **En aprendizaje** se reprograman aproximadamente a **1, 3, 7, 14 y 30 días**. La cola vive en `AI-103-STUDY-STATE.md` § Repaso programado. Un repaso fallido reinicia el contador.

## Convenciones

- Nada de progreso inventado. Lo no comprobado queda **No evaluado**.
- El % de avance sube **solo con evidencia registrada**.
- Un commit por sesión de estudio, mensaje: `study(YYYY-MM-DD): <resumen corto>`.
