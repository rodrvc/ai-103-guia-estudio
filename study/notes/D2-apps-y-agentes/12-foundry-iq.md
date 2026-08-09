# Foundry IQ: la capa de conocimiento compartida

> LP2 · Módulo 4 — [Build knowledge-enhanced AI agents with Foundry IQ](https://learn.microsoft.com/en-us/training/modules/introduction-foundry-iq/) (8 unidades)
> Cubre: **D2.1.b**, **D2.2.b**, **D5.1.e** · Amplía [06 RAG y grounding](06-rag-grounding.md)
> Peso: **alto** — cruza D2 (30–35%) y D5 (10–15%)
> Fecha: 2026-08-09

---

## En cristiano

Ya sabes montar RAG a mano: trocear documentos, generar embeddings, indexar, buscar. Funciona.

**El problema aparece con el segundo agente.** Y con el tercero.

| | Sin Foundry IQ | Con Foundry IQ |
| --- | --- | --- |
| 3 agentes que necesitan los mismos documentos | **3 sistemas RAG** que mantener | **1 knowledge base** que los tres usan |
| Añades un documento nuevo | Lo reindexas en los 3 | Lo añades una vez, **los 3 lo ven** |
| Tu tiempo se va en | Infraestructura de búsqueda | Diseñar qué hace cada agente |

> **Foundry IQ = RAG como servicio gestionado, construido sobre Azure AI Search.**

La analogía: en vez de que cada departamento monte su propio archivo, hay **una biblioteca central** y todos sacan libros de ahí.

---

## Knowledge bases: se organizan por tema, no por dónde están

Este es el cambio de mentalidad que el módulo subraya.

Los agentes **no buscan en "SharePoint Sitio A"** ni en "Contenedor B". Buscan en **"Documentación de producto"** o **"Políticas de RRHH"**.

Una sola knowledge base puede reunir:

- Especificaciones técnicas de **SharePoint**
- Documentación de API en **Blob Storage**
- Analíticas de uso en **OneLake**
- Tickets de soporte de un **índice de AI Search** que ya tenías

Para el agente, todo eso es **una sola fuente**. Tú conectas orígenes de datos a temas, no gestionas sistemas de búsqueda separados.

---

## ⭐ Las 6 fuentes de datos — la tabla que cae

| Fuente | Tipo de acceso | Cuándo |
| --- | --- | --- |
| **Azure AI Search Index** | Indexado | Ya tienes índices montados y quieres semantic ranking, filtros, scoring propio |
| **Azure Blob Storage** | Directo | Ficheros sueltos en Azure (PDF, .docx, .txt, .md, HTML) |
| **Web** | **Tiempo real** (vía Bing) | Información pública y actual |
| **SharePoint (Remote)** | **Tiempo real** | Contenido vivo, con la gobernanza de Microsoft 365 |
| **SharePoint (Indexed)** | Indexado | Búsqueda avanzada sobre SharePoint, pipelines propios |
| **OneLake** | Directo | Datos no estructurados en Microsoft Fabric |

**Se pueden combinar en una misma knowledge base.** Lo típico: SharePoint interno como fuente principal + Web como apoyo para actualidad.

### Los dos SharePoint — la distinción examinable

| | **Remote** | **Indexed** |
| --- | --- | --- |
| **Cómo accede** | Consulta en tiempo real | Índice preprocesado |
| **Velocidad** | Depende de SharePoint | **Más rápido** |
| **Mantenimiento** | **Ninguno** | Hay que actualizar el índice |
| **Búsqueda avanzada** | Limitada | **Todo Azure AI Search** |
| **Frescura** | **Siempre actual** | Según el calendario de indexado |
| **Permisos** | **Respeta los de SharePoint automáticamente** | Se configuran al indexar |

**Regla:** *"lo más simple y siempre actualizado"* → **Remote**. *"búsqueda avanzada, combinar con otras fuentes, analizadores propios"* → **Indexed**.

> ⚠️ **Cuidado con Web:** depende de los resultados de Bing, así que **pierdes control sobre las fuentes**. Si el requisito es exactitud y verificabilidad, usa fuentes indexadas y controladas.

### Guía de decisión

| Si tus datos están… | Y necesitas… | Eliges |
| --- | --- | --- |
| En SharePoint | Simple, siempre actual | **SharePoint Remote** |
| En SharePoint | Búsqueda avanzada, pipelines | **SharePoint Indexed** |
| Ficheros en Azure | Acceso directo | **Blob Storage** |
| En Microsoft Fabric | Contenido del lakehouse | **OneLake** |
| Ya indexados | Aprovechar tu inversión en AI Search | **Azure AI Search Index** |
| Públicos y actuales | Contenido web al día | **Web** |

---

## Qué hace Foundry IQ por ti

Al añadir una fuente de datos, el ciclo es automático:

| Paso | Qué pasa |
| --- | --- |
| **Discovery** | Escanea tu almacenamiento buscando documentos |
| **Processing** | **Chunking + embeddings** |
| **Indexing** | El contenido queda buscable |
| **Monitoring** | Si cambian los documentos, **reindexa solo** |

Y en cada consulta, la **inteligencia de retrieval** también es automática:

- **Analiza la pregunta** — *"¿cuál es la política de devoluciones?"* necesita otra estrategia que *"lista todas las políticas"*
- **Elige la estrategia**: preguntas factuales simples → keyword; complejas → semántica + expansión de consulta
- **Rankea por relevancia**, lo que además **reduce los tokens** que consume la respuesta
- **Devuelve citas** de los documentos fuente

**Todo eso sin escribir código.** Tú defines *qué* contiene cada knowledge base; Foundry IQ decide *cómo* recuperar.

---

## Cómo se conecta un agente

> **Foundry IQ se conecta vía MCP** (Model Context Protocol). Ese es el mecanismo — enlaza con [11 Tools personalizadas](11-tools-personalizadas.md).

```python
from azure.ai.projects.models import PromptAgentDefinition, MCPTool

knowledge_tool = MCPTool(
    server_label="product-docs",
    server_url=f"{search_endpoint}/knowledgebases/product-documentation/mcp",
)

agent = project_client.agents.create_version(
    agent_name="product-support-agent",
    definition=PromptAgentDefinition(
        model="gpt-4o-mini",
        instructions="Answer product questions using the knowledge base. Always cite your sources.",
        tools=[knowledge_tool],
    ),
)
```

La knowledge base es **una tool más** para el agente.

---

## ⭐ Configurar el retrieval — donde fallan las implementaciones

El módulo lo dice claro: puedes tener el índice perfecto y aun así fallar, porque **el agente no sabe cuándo usarlo**.

Preguntas *"¿cuál es la política de vacaciones?"* y pueden pasar tres cosas:

| Comportamiento | Respuesta | Problema |
| --- | --- | --- |
| Responde de su entrenamiento | *"La mayoría de empresas dan 2-3 semanas"* | ❌ Genérico, no es tu política |
| Busca pero **no cita** | *"Tienes 15 días de PTO"* | ⚠️ Correcto pero **no verificable** |
| **Busca, cita y se apoya en la fuente** | *"Tienes 15 días【doc_id:1†Employee Handbook 2024】"* | ✅ Lo que quieres |

**Solo el tercero sirve en una empresa.** Y lo que decide cuál ocurre son **las instructions**.

### Las 3 cosas que unas instructions buenas especifican

| # | Qué | Por qué |
| --- | --- | --- |
| 1 | **Cuándo recuperar** | *"Siempre busca antes de responder. Nunca uses tu conocimiento propio"* |
| 2 | **Cómo citar** | El formato exacto: `【doc_id:search_id†source_name】` |
| 3 | **Qué hacer si no lo encuentra** | El fallback: *"No tengo esa información, contacta con RRHH"* |

```python
retrieval_instructions = """You are a helpful HR assistant.

CRITICAL RULES:
- You must ALWAYS search the knowledge base before answering any question
- You must NEVER answer from your own knowledge or training data
- Every answer must include citations in this format: 【doc_id:search_id†source_name】
- If the knowledge base doesn't contain the answer, respond with "I don't have that
  information in our current documentation. Please contact HR directly."
"""
```

> ⚠️ **Confusión frecuente:** *"Answer HR questions using the knowledge base"* **no basta**. No dice *cuándo* buscar ni *cómo* presentar el resultado, así que el agente unas veces busca y otras no.

---

## Probar: los 4 tipos de consulta

| Tipo | Ejemplo | Qué debe pasar |
| --- | --- | --- |
| **Factual directa** | *"¿Cuál es la política de vacaciones?"* | Recupera y cita |
| **Requiere síntesis** | *"¿Qué diferencias hay entre los tipos de permiso?"* | **Varios documentos**, respuesta sintetizada con **varias citas** |
| **Fuera de la knowledge base** | *"¿Qué tiempo hace hoy?"* | **Fallback elegante**, no inventarse nada |
| **Ambigua** | *"¿Y de los beneficios?"* | Pide aclaración o busca lo más relevante |

### Los 4 criterios de una buena respuesta

**Grounding** (viene de la fuente, no del entrenamiento) · **Citation** (cada afirmación con su origen) · **Relevance** (lo recuperado responde de verdad) · **Completeness** (no fragmentos sueltos).

---

## Ya en producción: qué monitorizar

Los usuarios preguntan distinto a como probaste tú.

| Métrica | Qué te dice |
| --- | --- |
| **Citation frequency** | ¿Está citando de forma consistente? |
| **Fallback frequency** | ¿Cuánto dice "no lo sé"? Si es mucho → **falta contenido** |
| **Query types** | Qué preguntan de verdad |
| **Retrieval accuracy** | ¿Los documentos recuperados contienen la respuesta? |

Enlaza con [10 Observabilidad](10-observabilidad-y-tracing.md).

---

## Para el examen

**Alto valor:**
- **Foundry IQ = capa de conocimiento compartida** sobre Azure AI Search; **varios agentes, una knowledge base**
- **Las 6 fuentes** y su tipo de acceso (indexado / directo / tiempo real)
- **SharePoint Remote vs Indexed** — frescura y permisos automáticos vs velocidad y búsqueda avanzada
- Las instructions deben decir **cuándo buscar, cómo citar y qué hacer si no encuentra**
- Se conecta **vía MCP**

**Valor medio:** knowledge bases organizadas por dominio de negocio · discovery → processing → indexing → monitoring · los 4 tipos de consulta de prueba · los 4 criterios de calidad · métricas de producción · Web pierde control sobre las fuentes.

**Bajo valor:** el formato exacto del string de cita · los ejemplos de instructions por tipo de agente.

---

## Comprueba que lo tienes

1. Tu organización va a montar tres agentes que consultan la misma documentación. ¿Qué ganas usando Foundry IQ en vez de RAG a medida?
2. Tienes documentación en SharePoint que cambia a diario y necesitas que se respeten los permisos existentes sin configurar nada. ¿Remote o Indexed?
3. Misma documentación en SharePoint, pero necesitas analizadores personalizados para terminología médica y combinarla con otras fuentes. ¿Cuál ahora?
4. Tu agente responde bien pero nunca incluye fuentes. Las instructions dicen *"responde usando la knowledge base"*. ¿Qué falta?
5. En producción, tu agente contesta *"no tengo esa información"* el 40% de las veces. ¿Qué te dice ese dato?
6. Requisito: exactitud y poder verificar cada fuente. ¿Añadirías la fuente Web?
7. ¿Qué protocolo usa Foundry IQ para conectar agentes con knowledge bases?

<details>
<summary>Respuestas</summary>

1. **Una sola knowledge base compartida** en vez de tres sistemas RAG que mantener. Al añadir o mejorar contenido, **los tres agentes se benefician a la vez**. Y no gestionas indexado, embeddings ni infraestructura de búsqueda.
2. **SharePoint Remote.** Consulta en tiempo real (siempre actual), sin índice que mantener, y **respeta automáticamente los permisos de SharePoint**.
3. **SharePoint Indexed.** El contenido se indexa en Azure AI Search, lo que permite analizadores propios, pipelines de enriquecimiento y combinar con otras fuentes.
4. Falta especificar **cómo citar** — el formato exacto — y **cuándo buscar** ("siempre, nunca desde tu conocimiento propio"). Una instrucción vaga produce comportamiento inconsistente.
5. Que probablemente **falta contenido en la knowledge base**: los usuarios preguntan cosas que no cubriste. Es señal de añadir fuentes o documentos, no de tocar las instructions.
6. **No, o solo como apoyo.** Web depende de los resultados de Bing, así que **pierdes control sobre qué fuentes referencia**. Para exactitud y verificación, fuentes indexadas y controladas.
7. **MCP** (Model Context Protocol).

</details>
