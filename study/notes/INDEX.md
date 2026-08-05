# Material de estudio AI-103

Apuntes por tema. Cada uno es autocontenido: explicación → **Para el examen** → **Comprueba que lo tienes** con respuestas.

**Cómo usarlo:**
- Estudiar / repasar → abre el archivo del tema.
- Pedirle a un agente *"repasemos deployment types"* → toma el apunte y te pregunta.
- NotebookLM / Obsidian / imprimir → arrastra la carpeta `notes/` entera o la de un dominio.

---

## D1 — Plataforma: planificar, desplegar, asegurar
*Peso en el examen: 25–30%*

| Apunte | De qué va | Estado |
| --- | --- | --- |
| [01 Deployment types](D1-plataforma/01-deployment-types.md) | Global / Data Zone / Regional × Standard / Provisioned / Batch. Residencia de datos, SLA, cuándo elegir cada uno | ✅ **Competente** (3/3 casos) |

**Pendiente en D1:** cuotas (TPM/PTU, 429) · Managed Identity y RBAC · content filters (4 categorías × 4 severidades) · CI/CD · redes privadas.

---

## D2 — Apps generativas y agentes
*Peso en el examen: 30–35% — el dominio más pesado*

| Apunte | De qué va | Estado |
| --- | --- | --- |
| [01 Conectar tu app a Foundry](D2-apps-y-agentes/01-conectar-app-a-foundry.md) | Endpoints, SDKs, **autenticación**, Responses vs ChatCompletions. **Peso alto** | 📖 Leído, sin evaluar |
| [02 Evaluación de modelos](D2-apps-y-agentes/02-evaluacion-de-modelos.md) | Manual / AI-assisted / NLP metrics. **Groundedness** = anti-alucinación. Evaluator library | 📖 Leído, sin evaluar |
| [03 El playground](D2-apps-y-agentes/03-playground.md) | Probar sin código, botón Code. **Peso bajo** | 📖 Leído, sin evaluar |

**Pendiente en D2:** las 4 tools (`code_interpreter`, `web_search`, `file_search`, `function`) ← **estás aquí** · agentes en Foundry · multi-agente · RAG y capas de retrieval · observabilidad.

---

## D3 — Visión · D4 — Texto y voz · D5 — Extracción documental
*Peso combinado: 30–45%*

**Sin material todavía.** Tampoco están evaluados (DIAG-2 pendiente).

---

## Leyenda de estado

| | Significado |
| --- | --- |
| 📖 **Leído, sin evaluar** | Tienes el apunte, pero no has respondido preguntas. **No cuenta como aprendido** |
| 🟡 **En aprendizaje** | Respondiste, con fallos en detalles |
| ✅ **Competente** | Aciertas consistente y explicas por qué |
| ⭐ **Dominado** | + casos de arquitectura con restricciones, en ≥2 sesiones |

Detalle por objetivo del temario: `../AI-103-SYLLABUS.md`

---

## Repaso pendiente

9 errores abiertos de DIAG-1 (`../AI-103-ERROR-LOG.md`). Los que ya tienen apunte:

| Error | Tema | Apunte que lo cubre |
| --- | --- | --- |
| E-003 | Autenticación sin claves | D2/01 § Autenticación |
| E-007 | Evaluadores vs observabilidad | D2/02 |
| E-001 | Deployment vs modelo | D2/01 y D2/03 |
| E-002 | TPM vs PTU | D1/01 |

Los demás (E-004 content filters, E-005 retrieval, E-006 tools, E-008 SLM, E-009 `requires_action`) **aún no tienen material**.

---

## Para agentes

Lee **este índice**, no el directorio. Abre un apunte solo si el usuario lo nombra o vas a actualizarlo. Plantilla: `_TEMPLATE.md`. Un apunte cubre un **tema**, no una unidad suelta del curso.
