# AI-103 — Ruta oficial de aprendizaje (curso AI-103T00-A)

- **Curso:** AI-103T00-A — *Develop AI apps and agents on Azure*
- **URL:** https://learn.microsoft.com/en-us/training/courses/ai-103t00
- **Duración oficial:** 4 días presenciales / **96 h** self-paced (`hoursToComplete: 96`)
- **Nivel:** Intermediate · **Rol:** AI Engineer / Developer
- **Página actualizada:** 2026-04-17
- **Verificado por un agente:** 2026-08-04

> Este archivo es el **currículo de contenido**. `AI-103-SYLLABUS.md` es el **checklist de evaluación** (temario oficial del examen). Se estudia por aquí; se mide por allá.

El curso se compone de **4 learning paths / 30 módulos**. Todos accesibles gratis en self-paced.

**Advertencia:** el curso NO cubre 1:1 el temario del examen. Ver § Cobertura y huecos al final.

---

## Estado global

| Path | Módulos | Completados | Dominios AI-103 |
| --- | --- | --- | --- |
| LP1 Develop generative AI apps in Azure | 6 | 0 | D1, D2 |
| LP2 Develop AI agents on Azure | 9 | 0 | D2, D1.4 |
| LP3 Develop natural language solutions in Azure | 7 | 0 | D4 |
| LP4 Extract insights from visual data on Azure | 8 | 0 | D3, D5 |
| **Total** | **30** | **0 (0%)** | |

Marcar ☑ solo cuando el módulo esté **completado con su lab**, no solo leído.

---

## LP1 — Develop generative AI apps in Azure (6 módulos)

`https://learn.microsoft.com/en-us/training/paths/develop-generative-ai-apps/` · código AI-3016 · actualizado 2026-05-11

| ☐ | Módulo | Mapea a |
| --- | --- | --- |
| ☐ | Plan and prepare to develop AI solutions on Azure | D1.1, D1.2 |
| ☐ | Select, deploy, and evaluate Microsoft Foundry models | D1.1.a, D1.2.c, D2.1.a, D2.1.d |
| ☐ | Develop a generative AI chat app with Microsoft Foundry (**Responses API**) | D2.1.e, D2.1.f |
| ☐ | Develop generative AI apps that use tools | D2.1.c, D2.2.c |
| ☐ | Optimize generative AI model performance with Microsoft Foundry (prompt eng. + RAG + fine-tuning) | D2.3.a, D2.1.b |
| ☐ | Implement a responsible generative AI solution in Microsoft Foundry | D1.4.a, D1.4.b |

## LP2 — Develop AI agents on Azure (9 módulos)

`https://learn.microsoft.com/en-us/training/paths/develop-ai-agents-azure/` · código AI-3026 · actualizado 2026-03-16

| ☐ | Módulo | Mapea a |
| --- | --- | --- |
| ☐ | Develop AI agents with Microsoft Foundry and Visual Studio Code | D2.2.a |
| ☐ | Integrate custom tools into your agent | D2.2.c |
| ☐ | Integrate MCP Tools with Azure AI Agents | D2.2.c |
| ☐ | Build knowledge-enhanced AI agents with **Foundry IQ** | D2.1.b, D2.2.b, D5.1.e |
| ☐ | Integrate your agent with Microsoft 365 (Teams, Copilot, **Work IQ**) | D2.2.c |
| ☐ | Build agent-driven workflows using Microsoft Foundry | D2.2.d, D2.2.e |
| ☐ | Develop an AI agent with **Microsoft Agent Framework** | D2.2.a, D2.2.b |
| ☐ | Orchestrate a multi-agent solution using the Microsoft Agent Framework | D2.2.d |
| ☐ | Discover Azure AI Agents with **A2A** | D2.2.d |

## LP3 — Develop natural language solutions in Azure (7 módulos)

`https://learn.microsoft.com/en-us/training/paths/develop-language-solutions-azure-ai/` · actualizado 2026-03-18

| ☐ | Módulo | Mapea a |
| --- | --- | --- |
| ☐ | Analyze text with Azure Language in Foundry Tools | D4.1.a, D4.1.b |
| ☐ | Develop a text analysis agent with the **Azure Language MCP server** | D4.1.a, D2.2.c |
| ☐ | Develop a speech-capable generative AI application | D4.2.a, D4.2.c |
| ☐ | Create speech-enabled apps with Azure Speech in Microsoft Foundry Tools | D4.2.a, D4.2.b |
| ☐ | Develop a speech agent with the **Azure Speech MCP server** | D4.2.b |
| ☐ | Develop an Azure Speech **Voice Live** Agent in Microsoft Foundry | D4.2.b, D4.2.c |
| ☐ | Translate text and speech with Microsoft Foundry Tools | D4.1.c, D4.2.d |

## LP4 — Extract insights from visual data on Azure (8 módulos)

`https://learn.microsoft.com/en-us/training/paths/insight-visual-data/` · actualizado 2026-03-24

| ☐ | Módulo | Mapea a |
| --- | --- | --- |
| ☐ | Develop a vision-enabled generative AI application | D3.2.a, D3.2.b, D3.2.c |
| ☐ | Generate images with AI | D3.1.a, D3.1.c |
| ☐ | Generate videos with Microsoft Foundry (**Sora 2**) | D3.1.b, D3.1.d |
| ☐ | Analyze images with Content Understanding | D3.2.e, D3.2.h |
| ☐ | Create a multimodal analysis solution with Azure Content Understanding | D3.2.f, D3.2.g, D5.2.b |
| ☐ | Create an Azure Content Understanding client application | D5.2.c |
| ☐ | Extract data with **Azure Document Intelligence** | D5.2.a |
| ☐ | Create a knowledge mining solution with **Azure AI Search** | D5.1.a–d |

---

## Cobertura y huecos

El curso oficial es el mejor punto de partida, pero **no es suficiente por sí solo**. Objetivos del examen con cobertura débil o nula en los 30 módulos:

| Hueco | Objetivo | Cómo cubrirlo |
| --- | --- | --- |
| **CI/CD con proyectos Foundry** | D1.2.d | Docs de Azure + lab propio. Ningún módulo lo trata a fondo |
| **Cuotas, TPM/PTU, rate limits, costos** | D1.3.a | Docs de Azure OpenAI quotas & limits. Alto valor por peso de D1 |
| **Seguridad: managed identity, private networking, keyless, RBAC** | D1.3.d | Docs + lab. Riesgo R2 del perfil; los módulos lo tocan de pasada |
| **Monitoreo de salud de índices y relevancia** | D1.3.c | Docs de AI Search |
| **Gobierno de agentes: oversight modes, tool-access controls** | D1.4.d | Parcial en LP2 (workflows) |
| **Prompt injection indirecto vía texto en imágenes** | D3.3.b | Docs de Content Safety (Prompt Shields) |
| **Reglas de política visual: watermarks, símbolos prohibidos** | D3.3.c | Docs de Content Safety |

**Conclusión:** LP1–LP4 cubren bien D2, D3, D4 y D5. **D1 (25–30%, el segundo dominio más pesado) queda parcialmente descubierto** — sobre todo operaciones y seguridad. Requiere estudio dirigido de documentación además del curso.

---

## Cómo usar esta ruta

- **Orden sugerido por defecto:** LP1 → LP2 → LP4 → LP3, con D1 (huecos) intercalado. LP1 primero porque construye el vocabulario Foundry que todo lo demás asume. LP3 (voz) al final: es autocontenido y su peso es menor.
- **El orden final se decide tras el diagnóstico**, no antes. Si DIAG-1 muestra que D2 ya está sólido, se salta directo a los huecos de D1.
- Cada módulo completado con su lab → registrar en `AI-103-PRACTICE.md` y actualizar la evidencia del objetivo en `AI-103-SYLLABUS.md`.
- Un módulo leído sin lab **no** sube el nivel (ver `CLAUDE.md` § Escala de niveles).

---

## Historial de verificación

| Fecha | Agente | Resultado |
| --- | --- | --- |
| 2026-08-04 | Claude | Curso AI-103T00-A verificado. El outline se carga por JS; los 4 learning paths se extrajeron del metadata `learn_item` de la página y se verificó cada uno por separado: 6+9+7+8 = 30 módulos. Mapeo a objetivos y análisis de huecos hechos por el agente (no oficial). |
