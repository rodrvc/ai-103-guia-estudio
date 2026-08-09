# El Model playground

> LP1 · Módulo 3 (Foundry SDK) · Unidad 2 — [Explore with the model playground](https://learn.microsoft.com/en-us/training/modules/foundry-sdk/02-chat-playground)
> Cubre: **D2.1.f** (conectar app a proyecto Foundry), roza **D2.3.a** (tuning)
> Peso en el examen: **bajo** (7 min de lección, es introductoria)
> Fecha: 2026-08-04

---

## En cristiano

El playground es **el probador de ropa**. Te pruebas el modelo antes de comprarlo: escribes prompts, mueves perillas, ves qué sale. Sin escribir una línea de código.

Y cuando encuentras lo que funciona, aprieta un botón y **te da el código ya escrito** con tu configuración dentro.

---

## ⚠️ Confusión frecuente: playground ≠ deployment

Es habitual describir un *deployment* como "la pantalla donde Microsoft te presenta el modelo y puedes interactuar con él".

**Eso que se describe ahí es el playground.** Son cosas distintas:

| | Qué es |
| --- | --- |
| **Deployment** | Tu instancia del modelo, con nombre, cuota y content filter. **Es lo que llamas desde código** |
| **Playground** | Una **pantalla** para probar ese deployment a mano, sin código |

El playground **usa** un deployment. No lo es. Puedes tener un deployment y no abrir nunca el playground.

---

## Qué te deja hacer

- Mandar prompts y ver respuestas al momento
- Ajustar **temperature** y **max tokens**
- Poner un **system message** para cambiar el comportamiento
- Probar **varios modelos** y configuraciones

---

## Lo único que de verdad hay que recordar: el botón **Code**

En el panel de chat hay un botón **Code**. Te genera el código que reproduce esa misma sesión, y te deja elegir:

| Eliges | Opciones |
| --- | --- |
| **API** | **Responses API**, ChatCompletions, u otra |
| **Lenguaje** | Python, C#, etc. |
| **SDK** | Cuál quieres ver |

Y viene **pre-rellenado** con tu endpoint del proyecto, el nombre de tu deployment y los ajustes actuales.

**Por qué importa:** es el puente entre "probé algo que funciona" y "lo tengo en mi app". Copias, pegas, adaptas.

---

## El flujo de trabajo (esto sí puede caer)

1. **Explorar** en el playground — probar prompts y ajustes
2. **Generar código** con el botón Code
3. **Desarrollar** tu app a partir de esa base
4. **Iterar** — vuelves al playground a probar ideas nuevas, actualizas el código

La idea: **validar barato antes de invertir tiempo en desarrollo**.

---

## Para el examen

**Valor medio:**
- El botón **Code** genera SDK samples con tu endpoint y deployment ya puestos
- Puedes elegir entre **Responses API** y ChatCompletions al generar
- El flujo playground → código → iterar

**Bajo valor:**
- Dónde está el botón en el menú
- La lista de lenguajes disponibles

**Lo que sí vale de esta unidad:** que el playground **no es** el deployment, y que existe la **Responses API** como opción — es la API moderna de Foundry y aparece en el resto del módulo.

---

## Comprueba que lo tienes

1. ¿Cuál es la diferencia entre el playground y un deployment?
2. Encontraste el prompt perfecto en el playground. ¿Cómo lo llevas a tu app en Python sin reescribirlo?
3. ¿Por qué conviene probar en el playground antes de programar?

<details>
<summary>Respuestas</summary>

1. El **deployment** es tu instancia del modelo (con nombre, cuota y filtros) — es lo que se llama desde código. El **playground** es una pantalla para probarlo a mano. El playground usa el deployment; no lo sustituye.
2. Botón **Code** → eliges API (Responses API), lenguaje y SDK → te da el código con tu endpoint y deployment ya rellenos.
3. Validas barato. Descubres qué prompt y qué ajustes funcionan **antes** de invertir tiempo escribiendo código sobre una idea que quizá no sirve.

</details>
