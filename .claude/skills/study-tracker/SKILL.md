---
name: study-tracker
description: Registra el progreso de estudio de AI-103 en la memoria compartida del repo. Úsalo SIEMPRE al terminar un bloque de estudio - tras escribir un apunte, corregir respuestas del usuario, completar un lab o un diagnóstico, o cerrar una sesión. También cuando el usuario pregunte "¿dónde quedamos?", "¿qué sabe el próximo agente?", "¿cómo voy?" o pida guardar/actualizar el progreso. Mantiene sincronizados CLAUDE.md, STUDY-STATE, SYLLABUS, ERROR-LOG, PRACTICE, SESSION-LOG y notes/INDEX.md.
---

# study-tracker

Mantiene la memoria de estudio AI-103 sincronizada para el siguiente agente, sin sobrecargar su contexto.

## Cuándo dispararte

**Sin que te lo pidan**, al terminar cualquiera de estos:

- Escribir un apunte en `notes/`
- Corregir respuestas del usuario a preguntas de comprobación
- Completar un lab, quiz, diagnóstico o simulación
- Descubrir un tema del temario que no estaba cubierto
- Cerrar la sesión o cambiar de tema

**Cuando te lo piden:** "¿dónde quedamos?", "¿qué sabe el próximo agente?", "guarda el progreso", "¿cómo voy?".

## Principio rector

> **Un agente nuevo debe poder retomar leyendo ~110 líneas, no 1.300.**

Actualiza **solo las secciones afectadas**. No regeneres documentos. Diffs pequeños y legibles.

---

## Regla de oro: leer ≠ dominar

**Nunca subas un nivel porque el usuario leyó una explicación o completó un módulo.**

Un nivel solo sube con evidencia: **preguntas respondidas, ejercicios resueltos, labs completados o simulaciones**. Si escribiste un apunte pero el usuario no respondió nada, el objetivo queda `Estudiado, sin evaluar` — que **no** es un nivel.

Escala completa en `CLAUDE.md` § Escala de niveles.

---

## Dónde va cada cosa

Cada dato tiene **un solo hogar canónico**. Los demás archivos llevan un puntero, nunca una copia.

| Qué registras | Archivo canónico | Qué NO tocar |
| --- | --- | --- |
| Nivel de un objetivo + su evidencia | `AI-103-SYLLABUS.md` (solo la fila del objetivo) | No dupliques el análisis en STUDY-STATE |
| Error nuevo, causa, respuesta correcta | `AI-103-ERROR-LOG.md` § Errores abiertos | — |
| Patrones entre errores | `AI-103-ERROR-LOG.md` § Patrones (reescribir, no acumular) | — |
| Resultado de quiz / lab / simulación | `AI-103-PRACTICE.md` | — |
| Apunte nuevo | `notes/<archivo>.md` + fila en `notes/INDEX.md` | — |
| Módulo del curso completado | `AI-103-LEARNING-PATH.md` (marcar ☑) | — |
| Qué pasó en la sesión | `AI-103-SESSION-LOG.md` (**≤8 líneas**) | No re-narres lo que ya está en ERROR-LOG o PRACTICE — enlaza |
| Fase, métricas, siguiente acción | `AI-103-STUDY-STATE.md` | — |
| Fase + hallazgo + preguntas abiertas | `CLAUDE.md` § 🚦 Estado | — |

---

## Procedimiento

### 1. ¿Hubo evidencia nueva?

Si el usuario **respondió preguntas**: evalúa, y actualiza la fila del objetivo en `SYLLABUS.md` con nivel + evidencia concreta (qué acertó, qué falló, fecha).

Si **falló algo**: añade el error a `ERROR-LOG.md` con el formato de su cabecera (objetivo, origen, su respuesta, la correcta, causa, concepto a reforzar, calendario de repaso 1/3/7/14/30 días).

Si **solo se leyó contenido**: marca `Estudiado, sin evaluar` y **deja el nivel como está**.

### 2. ¿Se escribió un apunte?

Añade la fila a `notes/INDEX.md`: archivo, tema, objetivo, error que cierra, fecha. Actualiza el contador de cobertura.

### 3. Bitácora

Entrada en `SESSION-LOG.md`, **≤8 líneas**, arriba del todo: hecho / resultado / errores / cambios de nivel / próxima acción.

### 4. Sincroniza el resumen

En `STUDY-STATE.md`: métricas (evaluados, competentes, apuntes), temas fuertes/débiles, **siguiente acción**.

En `CLAUDE.md` § 🚦 Estado: fase real, progreso, preguntas abiertas, fecha de actualización.

> ⚠️ **Verifica que la fase declarada sea la real.** El fallo más común es dejar escrito un plan que se abandonó. Si el usuario lleva sesiones haciendo otra cosa, **corrige la fase** — un arranque desactualizado hace más daño que ausente.

### 4b. ¿Aprendiste algo que merezca quedar escrito? Decide DÓNDE

`CLAUDE.md` se carga **entero en cada sesión**; todo lo demás es bajo demanda. Por eso crece: cada hallazgo valioso tiende a escribirse en el sitio más visible. Aplica este criterio:

| ¿Se puede romper **antes** de que un agente cargue un skill o abra un archivo? | Dónde va |
| --- | --- |
| **Sí** — se rompe en el primer mensaje (cómo explicar, fase actual, `en-us`, no subir nivel sin evidencia) | `CLAUDE.md`, en la mínima expresión |
| **No** — solo aplica al registrar, planificar o escribir apuntes | **Este skill** o el archivo de `study/` que corresponda |

**La regla va en `CLAUDE.md`; la anécdota que la originó, en `PROFILE.md` o aquí.** Ejemplo: *"usa `en-us`"* está en CLAUDE.md; la tabla de errores de traducción que lo justifica, en PROFILE.

Antes de añadir a `CLAUDE.md`, comprueba que no esté ya dicho — la duplicación interna es el modo de fallo más común.

### 4c. Depura lo resuelto

Revisa `STUDY-STATE.md` § Información faltante: **borra los datos que ya se respondieron**. Un dato resuelto que sigue listado hace que el siguiente agente vuelva a preguntarlo. Pasó con la suscripción de Azure (resuelta el 2026-08-04, seguía en la tabla el 2026-08-05).

### 5. Commit

```
study(YYYY-MM-DD): <resumen corto>
```

Cuerpo: qué cambió y por qué. Termina con:
`Co-Authored-By: Claude Opus 5 (1M context) <noreply@anthropic.com>`

---

## Presupuesto de contexto

- Ningún archivo de `study/` supera **200 líneas**. Al superarlo: compacta o archiva en `study/archive/`.
- `SESSION-LOG`: solo **5 sesiones** en detalle; el resto colapsa a una fila de § Historial comprimido y se archiva por mes.
- `ERROR-LOG`: un error **cerrado** pasa de bloque a **una línea** de tabla. Los abiertos conservan el detalle — son material de repaso.
- `notes/`: no se compacta (es producto humano). Se accede solo por `INDEX.md`.

Si un archivo se acerca a 200 líneas, dilo y propón qué compactar.

---

## Modo consulta

Si preguntan "¿dónde quedamos?" o "¿qué sabe el próximo agente?", **no actualices nada**: lee `CLAUDE.md` § Estado + `STUDY-STATE.md` § Siguiente acción y responde en 5 líneas. Si detectas desfase entre lo escrito y lo real, avísalo y ofrece corregirlo.

---

## Antes de escribir un apunte: lee el módulo entero

Cuando el usuario pasa la URL de **una unidad**, lee primero el **índice del módulo** (quita la unidad de la URL: `.../modules/<modulo>/`). Verás cuántas unidades tiene y qué cubre cada una.

**Por qué:** las unidades individuales son fragmentos. Un apunte hecho desde una sola unidad omite lo que el examen realmente pregunta. Ocurrió el 2026-08-04 con `foundry-sdk`: se cubrió solo la unidad 2 (playground, peso bajo) y se dejaron fuera las unidades 3–5 — endpoints, autenticación y Responses vs ChatCompletions, que son **peso alto** y cierran E-003. Lo detectó el usuario, no el agente.

**Regla:** el apunte cubre el **módulo**, no la unidad. Si el módulo es muy grande, divídelo — pero deliberadamente, y dilo en el índice.

---

## Trampas conocidas

1. **Fase desactualizada** — el fallo más caro. Verifícala siempre.
2. **Duplicar en vez de enlazar** — STUDY-STATE es un índice, no una copia.
3. **Subir un nivel sin evidencia** — leer no cuenta. Nunca.
4. **Re-narrar el diagnóstico** en cada archivo. Vive en ERROR-LOG § Patrones; los demás apuntan ahí.
5. **Olvidar el commit** — sin commit, el trabajo no existe para el siguiente agente.
6. **Marcar un error como cerrado sin repaso verificado.**
7. **Nunca guardar** claves, tokens, endpoints privados ni datos sensibles. Usar `<AZURE_ENDPOINT>`.
