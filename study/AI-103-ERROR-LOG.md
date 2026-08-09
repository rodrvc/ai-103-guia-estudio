# AI-103 — Registro de errores

> **Qué contiene:** cada fallo con su causa, la respuesta correcta y el calendario de repaso. Es la materia prima de la repetición espaciada y la **fuente canónica** del análisis de patrones.
> **Cuándo leerlo:** al corregir un quiz (solo § Patrones) · al registrar un error nuevo (solo la cabecera de formato) · en el repaso del día (solo los E-NNN de hoy).
> **Cuándo NO:** entero, casi nunca. Ve a la sección que necesitas.
>
> **Integridad:** no borrar errores abiertos — su detalle es el material de repaso. Al cerrar uno, comprimir a una línea en § Errores cerrados.

## Cómo registrar

Una entrada por error. Formato:

```
### E-NNN — <título corto> [YYYY-MM-DD]

- **Objetivo:** D2.1.b
- **Origen:** quiz | simulación | lab | conversación
- **Pregunta / situación:** ...
- **Mi respuesta:** ...
- **Respuesta correcta:** ...
- **Causa:** conceptual | lectura | descarte | falta de práctica | confusión entre servicios
- **Concepto a reforzar:** ...
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto | cerrado (YYYY-MM-DD)
```

### Tipos de causa

| Causa | Definición | Remedio |
| --- | --- | --- |
| **Conceptual** | No entiende el mecanismo subyacente | Volver a la teoría + ejercicio |
| **Lectura** | Sabía el tema, leyó mal el enunciado o el requisito | Técnica de examen: subrayar restricciones |
| **Descarte** | Dudó entre 2 opciones y eligió mal | Tabla comparativa entre esos servicios |
| **Falta de práctica** | Conoce la teoría, no ha tocado el servicio | Lab |
| **Confusión entre servicios** | Mezcla dos servicios Azure parecidos | Ficha de decisión "cuándo cada uno" |

---

## Errores abiertos

Todos provienen de **DIAG-1 (2026-08-04)**. Repasos programados: 1d = 2026-08-05, 3d = 2026-08-07, 7d = 2026-08-11, 14d = 2026-08-18, 30d = 2026-09-03.

### E-001 — Deployment vs Model en Foundry [2026-08-04]

- **Objetivo:** D1.2.c · **Origen:** DIAG-1 p3
- **Mi respuesta:** "un deployment pongo ya un modelo en línea, es la face donde Microsoft me presenta el modelo y puedo interactuar con él, como un ambiente cerrado; una vez escojo el modelo me entrega su api key"
- **Correcta:** *Model* = el modelo del catálogo (`gpt-4o`). *Deployment* = instancia con **nombre propio** que tú creas, con su **cuota (TPM)**, versión de modelo, política de actualización y **content filter** asignado. La API se invoca con el **nombre del deployment**, no el del modelo. Puede haber N deployments del mismo modelo con cuotas distintas. Lo descrito por el usuario es el **playground** (UI), no el deployment.
- **Causa:** conceptual + confusión entre servicios (mezcla deployment con playground)
- **Concepto a reforzar:** ciclo model → deployment → endpoint; qué se pasa en el parámetro `model=` del SDK
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto

### E-002 — Cuotas, rate limits y PTU [2026-08-04]

- **Objetivo:** D1.3.a · **Origen:** DIAG-1 p4
- **Mi respuesta:** "no lo sé"
- **Correcta:** La capacidad se asigna en **TPM (tokens per minute)**; los RPM se derivan de los TPM. Al exceder: **HTTP 429** con header `Retry-After`; el cliente debe implementar **retry con backoff exponencial**. Alternativa: **PTU (Provisioned Throughput Units)** = capacidad reservada, latencia predecible, costo fijo, para cargas de producción constantes. Pay-as-you-go/TPM para cargas variables o desarrollo.
- **Causa:** falta de conocimiento (área nunca tocada)
- **Concepto a reforzar:** TPM vs PTU y cuándo elegir cada uno; manejo de 429
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto

### E-003 — Autenticación sin claves: Managed Identity [2026-08-04]

- **Objetivo:** D1.3.d · **Origen:** DIAG-1 p5
- **Mi respuesta:** "quizás usando el toolkit de azure"
- **Correcta:** **Managed Identity** (system-assigned o user-assigned) asignada al recurso de cómputo, más un **rol RBAC** (`Cognitive Services OpenAI User` para inferencia). En Python: `DefaultAzureCredential` del paquete `azure-identity`, pasada al cliente. Cero secretos en configuración. Contrasta con API key (secreto que rota y se filtra).
- **Causa:** falta de conocimiento
- **Concepto a reforzar:** Managed Identity vs Service Principal vs API key; `DefaultAzureCredential` y su cadena de fallback; roles RBAC de Azure AI
- **Riesgo asociado:** R2. Muy probable en el examen
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto

### E-004 — Categorías y severidades de content filters [2026-08-04]

- **Objetivo:** D1.4.a · **Origen:** DIAG-1 p6
- **Mi respuesta:** "no lo sé"
- **Correcta:** **Cuatro** categorías **con niveles de severidad**: hate and fairness, sexual, violence, self-harm. Cuatro severidades: safe, low, medium, high (`safe` se anota pero no se filtra ni es configurable). Se configuran por separado para **prompt (input)** y **completion (output)**. **Aparte, filtros binarios sin severidad:** Prompt Shields (user prompt attacks + indirect attacks), Groundedness, PII, Protected Material (text/code) y **Task Adherence** (desvío del agente).
- **Causa:** falta de conocimiento (memorización requerida)
- **Concepto a reforzar:** 4 categorías graduadas × 4 severidades vs. filtros binarios; prompt filtrado → **HTTP 400**, completion filtrada → `finish_reason: content_filter`
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto — **con material desde 2026-08-08:** `notes/D1-plataforma/02-ia-responsable.md`
- ⚠️ **Verificado contra la fuente el 2026-08-08** (docs de Content Safety). Hubo una fase intermedia en la que esta ficha dijo "5 categorías" tras leer el módulo `responsible-ai-studio`, que las lista juntas. **Es incorrecto:** Task Adherence es un filtro, pero no una de las 4 categorías graduadas. Confusión resuelta.

### E-005 — Vectorial vs híbrida vs semantic ranker [2026-08-04]

- **Objetivo:** D2.1.b, D5.1.b · **Origen:** DIAG-1 p8
- **Mi respuesta:** "si es pura solo buscará por coincidencias; en cambio híbrida buscará por coincidencias y también semántica"
- **Correcta:** **Concepto invertido.** La búsqueda **vectorial** es la semántica (embeddings, similitud de significado). La **keyword/BM25** es la de coincidencias léxicas. **Híbrida = BM25 + vectorial**, fusionadas con **RRF (Reciprocal Rank Fusion)**. Y hay una **tercera capa**: el **semantic ranker**, que re-rankea el top-N con un modelo de lenguaje — es el que **más latencia y costo añade** (era el núcleo de la pregunta y quedó sin responder).
- **Causa:** conceptual — invierte la atribución de lo semántico
- **Concepto a reforzar:** las 3 capas de retrieval en AI Search y su costo/latencia relativos; qué es RRF
- **Nota:** error revelador. El usuario domina RAG en la práctica pero no la terminología de AI Search
- **Material (2026-08-05):** `notes/D2-apps-y-agentes/06-rag-grounding.md` — cubre el marco (retrieve/augment/generate, embeddings, los 4 tipos de búsqueda, hybrid como recomendado). ⚠️ **No cierra el error:** el curso de LP1 no menciona **BM25**, **RRF** ni **semantic ranker**, que es exactamente lo que falló. Esa capa hay que sacarla de las docs de Azure AI Search, no del training path
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto

### E-006 — Catálogo de tools de agentes en Foundry [2026-08-04]

- **Objetivo:** D2.2.c · **Origen:** DIAG-1 p9
- **Mi respuesta:** "puede usar voz, también consultar herramientas, también preconfiguraciones para los casos más comunes"
- **Correcta:** Tools del Agent Service: **File Search / Knowledge**, **Azure AI Search**, **Code Interpreter**, **Function calling** (funciones propias), **OpenAPI tools** (APIs REST por spec), **MCP**, **Bing grounding**, **Content Understanding**. La **voz no es una tool** — es una modalidad de interacción (Speech / Voice Live).
- **Causa:** falta de conocimiento del catálogo + confusión tool vs modalidad
- **Concepto a reforzar:** inventario de tools y cuándo usar cada una
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto

### E-007 — Evaluadores de Foundry vs observabilidad [2026-08-04]

- **Objetivo:** D2.1.d · **Origen:** DIAG-1 p12
- **Mi respuesta:** "en la observabilidad"
- **Correcta:** **Confunde dos cosas distintas.** *Observabilidad* = trazas, tokens, latencia (qué pasó). *Evaluación* = métricas de calidad de la salida. Evaluadores integrados: **groundedness** (← detecta alucinaciones: mide si la respuesta se sustenta en el contexto recuperado), **relevance**, **coherence**, **fluency**, **similarity**, **retrieval**, más los evaluadores de seguridad (violence, hate, sexual, self-harm) y de material protegido.
- **Causa:** conceptual — colapsa evaluación y observabilidad en un concepto
- **Concepto a reforzar:** groundedness como métrica anti-alucinación; separar evaluación de observabilidad
- **Nota:** llamativo porque el usuario usa LangSmith. El concepto le es familiar, el nombre Azure no
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto

### E-008 — Criterios para elegir SLM sobre LLM [2026-08-04]

- **Objetivo:** D1.1.a · **Origen:** DIAG-1 p2
- **Mi respuesta:** "cuando necesito decisiones que no necesiten tanto texto, como decidir si volver a iterar en una extracción"
- **Correcta:** El criterio es válido pero secundario. Los dos que el examen busca: **(1) costo y latencia** — un SLM es órdenes de magnitud más barato y rápido para tareas acotadas (clasificación, routing, extracción simple); **(2) edge / on-device / soberanía de datos** — modelos como **Phi** corren local o en el borde, sin salida a la nube. Añadir: fine-tuning más barato sobre SLM para dominio específico.
- **Causa:** conceptual parcial — le falta el eje costo/latencia/edge
- **Concepto a reforzar:** matriz de decisión LLM vs SLM vs multimodal; familia Phi
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto

### E-009 — Mecánica de human-in-the-loop en un run [2026-08-04]

- **Objetivo:** D2.2.e · **Origen:** DIAG-1 p11
- **Mi respuesta:** "cuando se va a tomar la decisión y se tienen todas las evidencias habría que agregar un paso de humano en el medio para aprobar"
- **Correcta:** El **concepto es correcto** (contó como acierto). Falta el **mecanismo exacto**, que es lo que se pregunta: el run entra en estado **`requires_action`** cuando el modelo solicita invocar la tool; la ejecución se **pausa antes de ejecutar la función**; tu app inserta la aprobación humana y luego llama a **`submit_tool_outputs`** para reanudar el run.
- **Causa:** falta de práctica (conoce el patrón, no la API)
- **Concepto a reforzar:** estados del run (`queued`, `in_progress`, `requires_action`, `completed`, `failed`) y el ciclo submit_tool_outputs
- **Repasos:** 1d ☐ | 3d ☐ | 7d ☐ | 14d ☐ | 30d ☐
- **Estado:** abierto

---

## Errores cerrados

**Un error cerrado ocupa UNA línea, no un bloque.** Al cerrarse, se borra su detalle y se añade aquí. El valor de un error cerrado es saber que se cerró y con qué evidencia; si el concepto sigue siendo útil, ya vive en `notes/`. Cuando esta tabla supere 40 filas, mover a `study/archive/ERROR-LOG-cerrados-<AAAA>Q<n>.md`.

| ID | Tema | Objetivo | Cerrado | Evidencia |
| --- | --- | --- | --- | --- |
| *(ninguno todavía)* | | | | |

---

## Patrones detectados

**Actualizado 2026-08-04 tras DIAG-1 (9 errores).**

### Patrón dominante: brecha conceptual vs. operativa

Lo que el usuario **sí tiene** (aciertos plenos): Foundry IQ y fuentes de datos, filter vs blocklist, modelo thread/run/message con persistencia del lado del servicio, patrón human-in-the-loop. Todo **conceptual y de diseño**.

Lo que **no tiene** (fallos): deployments, cuotas/TPM/PTU, Managed Identity, categorías de content filter, capas de retrieval en AI Search, catálogo de tools, evaluadores. Todo **operativo, nombrado y específico de Azure**.

**Diagnóstico:** el usuario piensa como arquitecto de sistemas de IA y le falta el vocabulario de operación de la plataforma. Confirma R1 y R7 de `AI-103-STUDY-STATE.md`. La buena noticia: es una brecha de **superficie**, no de fundamentos — se cierra estudiando nombres y límites, no reaprendiendo conceptos. La mala: **es exactamente lo que el examen pregunta**.

### Distribución por causa

| Causa | N | Errores |
| --- | --- | --- |
| Falta de conocimiento (nunca tocado) | 3 | E-002, E-003, E-004 |
| Conceptual | 3 | E-001, E-005, E-007 |
| Conceptual parcial | 2 | E-006, E-008 |
| Falta de práctica (sabe el patrón, no la API) | 1 | E-009 |
| Lectura / descarte | 0 | — |

**Nota:** cero errores de lectura o descarte. Esto es significativo — no hay problema de técnica de examen. Las respuestas fueron honestas y bien razonadas dentro de lo que sabe. El problema es cobertura, no estrategia. Es el tipo de brecha que se cierra rápido.

### Pares que se confunden — fichas de decisión pendientes

| Par | Error |
| --- | --- |
| Deployment ↔ Playground | E-001 |
| Búsqueda vectorial ↔ híbrida ↔ semantic ranker | E-005 |
| Evaluación ↔ Observabilidad | E-007 |
| Tool ↔ Modalidad (voz) | E-006 |
| TPM ↔ PTU | E-002 |

### Concentración por dominio

7 de 9 errores caen en **D1** (D1.1.a, D1.2.c, D1.3.a, D1.3.d, D1.4.a ×2) y en la mecánica operativa de **D2**. Coincide con el hueco R8 del curso oficial: **el curso tampoco cubre bien D1**. Doble motivo para atacarlo con estudio dirigido de documentación.
