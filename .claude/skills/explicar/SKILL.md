---
name: explicar
description: Explica un concepto de AI-103 a Rodrigo de forma que se entienda a la primera. Úsalo cuando diga "explícame X", "de qué va esto", "en simple", "no entiendo", "dame un ejemplo", "resúmeme esta lección", o cuando pase una URL de Microsoft Learn pidiendo entender el contenido. Método: un caso concreto que se sigue de principio a fin, no una lista de definiciones.
---

# explicar

Explicar de forma que **se entienda a la primera**. Validado por el usuario el 2026-08-08: *"como me lo explicaste suena super simple"*.

Este skill existe porque hubo dos fallos de comunicación graves — muros de texto con jerga que casi hacen abandonar el proyecto (*"o te bajas un poco y me explicas bien o lo hago con gpt"*). Lo que funciona está abajo.

---

## Lo que funciona: un caso concreto que se sigue entero

**No expliques el proceso. Ejecuta el proceso delante de él sobre un caso inventado.**

La diferencia:

| ❌ No hagas esto | ✅ Haz esto |
| --- | --- |
| "Map identifica daños potenciales" | "Un chatbot médico. Listas: dosis equivocada, diagnóstico inventado, fuga de datos…" |
| "Se prioriza por impacto y probabilidad" | "Gana el diagnóstico inventado: no es lo más grave, pero pasa **todos los días**" |
| "Measure establece una línea base" | "200 prompts, los lanzas, los clasificas. **18% inventa diagnósticos.** Ese es tu número" |

**El mismo caso atraviesa todos los pasos.** No un ejemplo por sección: un solo hilo del principio al final. Así el usuario lleva el contexto acumulado y cada paso se apoya en el anterior.

### Elegir el caso

- **Concreto y con consecuencias reales.** Un chatbot médico funciona porque equivocarse importa. Un "sistema de ejemplo" no.
- **Fuera del dominio del usuario** cuando quieras que vea la estructura, no discuta la implementación.
- **Con números inventados pero verosímiles.** "18% → 2%" enseña más que "se reduce el daño".

---

## La estructura

1. **Frase de encuadre**, una línea: *"Un chatbot que da información médica a pacientes en una clínica."*
2. **Un bloque por paso**, con el caso avanzando. Tabla si hay comparación; texto corto si es secuencia.
3. **La idea de fondo** al final: la regla que hace que el orden importe. *"Mitigar sin medir = no sabes si funcionó."*
4. **Una pregunta de comprobación.** Siempre.

---

## Reglas que no se negocian

- **Nombres técnicos en inglés, al final de la explicación.** Primero se entiende la decisión, después se le pone la etiqueta que usa el examen.
- **Tablas antes que prosa.** Tres columnas máximo.
- **Sin jerga de entrada.** Si una palabra necesita explicación, no puede aparecer en la primera frase.
- **Un concepto por mensaje** cuando el tema es nuevo. Cierra con "¿sigo?" y **espera**.
  - Excepción: si pide *"resumido"* o *"de qué va"*, dáselo entero pero corto — quiere el mapa, no la caminata.
- **No lo examines sobre lo que no le explicaste.**
- **Misma profundidad, distinta puerta de entrada.** Adaptar no es rebajar. El ejemplo del chatbot contiene las 4 capas completas, no una versión reducida.

---

## Hacer visible la fricción

Cuando algo es contraintuitivo, **dilo**. No lo dejes implícito.

- *"Funcionaba, publicas, deja de funcionar."*
- *"No es capricho: mitigar sin medir = no sabes si funcionó."*
- *"Más potente ≠ mejor."*

Eso es lo que se recuerda en el examen. Lo obvio se olvida.

### Analogías: para lo arbitrario, no para lo lógico

Funcionan cuando la distinción **no se deduce** y hay que memorizarla:

- Deployment types → taxi / auto propio con chofer / encomienda
- Residencia de datos → tu casa vs. la lavandería
- RBAC tras publicar → ascendieron a alguien y le dieron **credencial nueva**: misma persona, mismo edificio, la tarjeta vieja no abre

Si el concepto tiene lógica interna (como el orden Map→Measure→Mitigate→Manage), **explica la lógica** — es más sólido que una metáfora.

---

## Después de explicar

- **Si valió la pena explicarlo, va a `notes/` en la misma sesión.** Explicar en conversación no deja rastro.
- **Guarda el ejemplo completo, no solo las conclusiones.** El ejemplo es lo que hizo que se entendiera; sin él el apunte vuelve a ser una lista de definiciones.
- Invoca **`study-tracker`** para sincronizar.

---

## Señales de que lo estás haciendo mal

| Señal | Qué pasó |
| --- | --- |
| El mensaje no cabe en una pantalla | Muro de texto. Corta y pregunta "¿sigo?" |
| Empiezas con "X es un servicio que…" | Definición antes que caso. Dale la vuelta |
| Un ejemplo distinto por sección | No hay hilo. Un solo caso atraviesa todo |
| Términos en inglés en la primera frase | Jerga de entrada |
| Pregunta "¿qué significa X?" a mitad | Usaste una sigla sin explicarla — pasó con RBAC |
