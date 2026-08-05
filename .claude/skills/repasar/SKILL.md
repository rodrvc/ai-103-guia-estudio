---
name: repasar
description: Repasa un tema de AI-103 con Rodrigo mediante preguntas activas, y registra el resultado como evidencia. Úsalo cuando diga "repasemos X", "pregúntame de X", "hazme un quiz", "quiero repasar", "vamos con el repaso del día", "ponme a prueba" o cuando toque un repaso espaciado programado. Toma el apunte de study/notes/, pregunta, corrige y actualiza el nivel del objetivo.
---

# repasar

Repaso activo: **preguntar, no re-explicar**. Es lo único que convierte "leído" en evidencia.

## 1. Elige el material

- **Nombra un tema** → busca el apunte en `study/notes/INDEX.md`.
- **Dice "el repaso del día"** → `AI-103-STUDY-STATE.md` § Repaso programado: los E-NNN que tocan hoy.
- **No especifica** → propón en este orden: (1) errores con repaso vencido, (2) apuntes 📖 *Leído, sin evaluar*, (3) lo último estudiado.

Si **no hay apunte** del tema: dilo y ofrece escribirlo primero, leyendo la fuente. No improvises preguntas de memoria.

## 2. Pregunta

Usa la sección **"Comprueba que lo tienes"** del apunte; añade variantes si hace falta.

- **De aplicación, no de definición.** "Tu app necesita X con restricción Y, ¿qué usas?" en vez de "¿qué es X?".
- **De una en una.** Espera la respuesta antes de seguir. Nunca sueltes las 5 juntas.
- Empieza por lo de **mayor peso en el examen**.
- 3–5 preguntas por sesión. Más, cansa.
- **Sin pistas dentro de la pregunta.** Si no sabe, esa es la información valiosa.

## 3. Corrige

- Di **claro si acertó o no**. Nada de "casi" cuando está mal.
- Si falla: respuesta correcta, **por qué**, y el concepto de fondo.
- Si acierta a medias: reconoce lo bueno y nombra lo que faltó.
- Si dice "no sé": es honesto y útil. Explica sin reproche.

## 4. Registra

**Siempre**, aunque el repaso sea corto:

- **Acertó todo** → sube el nivel en `AI-103-SYLLABUS.md` con evidencia (qué preguntas, qué fecha). **Competente** pide consistencia + explicar el porqué; **Dominado** pide además ≥2 sesiones separadas.
- **Falló** → registra o reabre el error en `AI-103-ERROR-LOG.md` y **reinicia su contador** de repaso espaciado.
- **Repasó un E-NNN con éxito** → marca ☑ el hito. **Cerrar un error exige acertar en un repaso**, no basta con haber leído el apunte.
- Actualiza el estado en `notes/INDEX.md` (📖 → 🟡 → ✅ → ⭐).

Luego invoca **`study-tracker`** para sincronizar el resto y commitear.

## 5. Cierra

Dos líneas: qué acertó, qué falló, y cuándo toca el próximo repaso de lo fallado.

---

## Reglas

- **Explicar primero, preguntar después.** Si el tema no se ha explicado, explícalo (un concepto por mensaje) y repasa en otra sesión.
- **Nunca subas un nivel sin respuestas.** Leer el apunte no cuenta.
- Términos técnicos en inglés; explicación en español. Si responde "credencial de empleado" en vez de "Entra ID", **cuenta como acierto** — el nombre viene después del concepto.
- Ver `CLAUDE.md` § Cómo trabajar con Rodrigo.
