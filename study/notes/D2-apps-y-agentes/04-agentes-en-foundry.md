# Agentes en Foundry

> LP2 · Módulo 1 · Unidad 2 — [Understand AI agents and Foundry Agent Service](https://learn.microsoft.com/en-us/training/modules/develop-ai-agents-azure-vs-code/2-understand-ai-agents-foundry)
> Cubre: **D2.2.a**, toca **D1.4.d** (gobierno) · Relacionado con **E-006**, **E-009**
> Peso: **alto** — D2 es el dominio más pesado (30–35%)
> Fecha: 2026-08-05

---

## En cristiano

Un **chat** responde. Un **agente** hace cosas.

> Agente = modelo + instrucciones + herramientas

Puede buscar en internet, leer tus archivos, ejecutar código o llamar a tu API.

---

## Qué hace el Agent Service por ti

**Sin él**, tú escribirías este bucle a mano:

> el modelo quiere usar una tool → yo la llamo → le devuelvo el resultado → sigue

**Con él, eso pasa solo.** Se llama **automatic tool calling**.

Y **guarda la conversación** por ti (vía la Responses API). No mantienes historial.

---

## Los dos tipos de agente — esto es lo que cae

### Declarativos — se configuran, no se programan

| Tipo | Qué es |
| --- | --- |
| **Prompt-based** | Un agente: modelo + instrucciones + tools + prompts. **El común** |
| **Workflow** | Varios agentes colaborando, definidos en **YAML** |

### Hosted — se programan

Agentes en **contenedor**, creados en código y hospedados por Foundry. Control total de la lógica; la infraestructura la gestiona la plataforma.

*Regla:* ¿configuración? → declarativo. ¿código propio en contenedor? → hosted.

---

## Features del servicio

| Feature | Qué significa |
| --- | --- |
| **Automatic tool calling** | Gestiona el ciclo completo de invocar tools |
| **Estado gestionado** | El servicio persiste la conversación, no tu app |
| **Catálogo de tools** | Built-in + comunidad: código, file search, web search, servicios Azure, APIs |
| **Seguridad enterprise** | **Keyless auth**, content safety filters, privacidad |
| **Storage** | Gestionado por la plataforma **o tu propio Azure Blob** |
| **Observabilidad** | Tracing y monitoreo integrados |

Menos de **50 líneas de código** para un agente básico, contra el esfuerzo de hacerlo con la API directa.

---

## Seguridad de agentes — 8 riesgos

Un agente accede a datos sensibles, decide y actúa solo. Riesgos que el módulo enumera:

| Riesgo | Ejemplo de impacto |
| --- | --- |
| **Fuga de datos** | Resume archivos internos e incluye datos privados en una respuesta al cliente |
| **Prompt injection** | Instrucciones ocultas le hacen filtrar credenciales |
| **Escalada de privilegios** | Un agente conectado a un CRM borra o exporta registros |
| **Data poisoning** | Datos corruptos → recomienda contenido dañino |
| **Cadena de suministro** | Un plugin de terceros comprometido inyecta código |
| **Exceso de autonomía** | Envía pagos o publica contenido sin verificar |
| **Falta de auditoría** | Sin logs, nadie detecta el mal uso |
| **Model inversion** | Consultas repetidas extraen datos del fine-tuning |

### Mitigaciones — **esto es lo que se pregunta**

- **RBAC + mínimo privilegio**
- **Filtrado y validación de prompts** (contra injection)
- **Human-in-the-loop** para operaciones sensibles ← tu E-009
- **Logging y trazabilidad** completos
- Auditar dependencias de terceros
- Reentrenar y validar contra drift o poisoning

---

## ⚠️ Tu riesgo R7 aquí

Con LangGraph esto te suena obvio. **Lo que no es obvio son los nombres de Microsoft:** *declarative* vs *hosted*, *workflow agents en YAML*, *automatic tool calling*.

Entiendes el concepto; te falta la etiqueta. Y la etiqueta es lo que pregunta el examen.

---

## Para el examen

**Alto valor:**
- **Declarativo (prompt-based / workflow YAML) vs hosted**
- **Automatic tool calling** — el servicio gestiona el ciclo
- **El servicio persiste la conversación**, no tu app
- Mitigaciones: RBAC, mínimo privilegio, human-in-the-loop, logging

**Valor medio:** keyless auth · storage propio o gestionado · observabilidad integrada.

**Bajo valor:** los casos de uso de ejemplo (Cineplex, GitHub Copilot).

---

## Comprueba que lo tienes

1. Defines un agente con modelo, instrucciones y tools, sin escribir código. ¿Qué tipo es?
2. Necesitas varios agentes colaborando. ¿Qué tipo y en qué formato se define?
3. ¿Qué es *automatic tool calling* y qué te ahorra?
4. Tu agente emite reembolsos. ¿Qué mitigación de seguridad aplicas?
5. ¿Quién guarda el historial de conversación de un agente: tu app o el servicio?

<details>
<summary>Respuestas</summary>

1. **Declarativo prompt-based** — el tipo más común.
2. **Workflow agent**, definido en **YAML**. Es multi-agente y también declarativo.
3. El servicio gestiona el ciclo completo: ejecuta el modelo, invoca la tool y devuelve el resultado. Te ahorra escribir ese bucle a mano.
4. **Human-in-the-loop**: aprobación humana antes de ejecutar la acción sensible. Más RBAC con mínimo privilegio y logging completo.
5. **El servicio** (vía Responses API). Lo acertaste en DIAG-1 p10.

</details>
