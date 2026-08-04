# Proyecto: Preparación certificación Microsoft AI-103

---

## 🚦 EMPIEZA AQUÍ · actualizado 2026-08-04

> Si hoy es **>7 días** después de esa fecha, este bloque puede estar obsoleto: lee `study/AI-103-STUDY-STATE.md` antes de confiar en él, y actualízalo.

**Qué es esto:** sistema de estudio para que Rodrigo apruebe el examen **AI-103**. No es un repo de código.

**Fase:** DIAG-1 hecho (**42%** en D1+D2, se aprueba con ~70%) · estudiando bloque **B1** · **DIAG-2 pendiente** → D3/D4/D5 (30–45% del examen) sin medir.

**Hallazgo clave (n=12, revisar tras DIAG-1B):** acierta lo **conceptual**, falla lo **operativo de Azure** (deployments, cuotas, managed identity, evaluadores, retrieval). Brecha de superficie, no de fundamentos — pero es lo que el examen mide. **No confundir su fluidez conceptual con dominio** → riesgo R7.

**Primera acción:** `study/AI-103-STUDY-STATE.md` § Siguiente acción. Luego, según tarea, ver § Qué leer.

**Preguntas abiertas** (bloquean el calendario, sin responder desde 2026-08-04): ¿suscripción de Azure activa? · ¿DIAG-2 antes de B1? (recomendado sí) · ¿examen en inglés o español? (recomendado inglés).

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
- **SESSION-LOG:** solo las **últimas 5 sesiones** en detalle (≤8 líneas cada una). Las anteriores colapsan a una fila de tabla y se archivan por mes.
- **ERROR-LOG:** un error **cerrado** pasa de bloque a **una línea** de tabla. Los abiertos conservan el detalle — son material de repaso. Si hay >15 abiertos, el problema es el plan de estudio, no la memoria.
- **`notes/`** no se compacta (es producto humano), pero se accede solo por `INDEX.md`.

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

## Convenciones

- Nada de progreso inventado. Lo no comprobado queda **No evaluado**.
- El % de avance sube **solo con evidencia registrada**.
- Un commit por sesión de estudio, mensaje: `study(YYYY-MM-DD): <resumen corto>`.
