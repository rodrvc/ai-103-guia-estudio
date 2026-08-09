# Construir, configurar y publicar agentes

> LP2 · Módulo 1 — [Develop AI agents with Foundry and VS Code](https://learn.microsoft.com/en-us/training/modules/develop-ai-agents-azure-vs-code/) (11 unidades)
> Cubre las unidades **3, 4, 5, 6, 7 y 8**. La unidad 2 (qué es un agente, tipos, riesgos) está en [04 Agentes en Foundry](04-agentes-en-foundry.md).
> Cubre: **D2.2.a**, **D2.2.c**, toca **D1.3.d** (identidad y RBAC)
> Peso: **alto** — D2 es el dominio más pesado (30–35%)
> Fecha: 2026-08-08

---

## En cristiano

Este módulo es el ciclo de vida completo de un agente: **lo creas → lo configuras → le das herramientas → lo pruebas → lo sacas a producción**.

Se puede hacer por dos puertas: el **portal** (con el ratón) o **VS Code** (con una extensión). Hacen lo mismo; cambia el estilo, no la capacidad.

---

## 1. Portal vs VS Code — cuál eliges

| | Foundry portal | VS Code (extensión Microsoft Foundry) |
| --- | --- | --- |
| **Para qué** | Prototipar rápido, gestión centralizada | Trabajar junto a tu código |
| **Ventaja clave** | Sin instalar nada, formularios visuales | **YAML en Git**, versionado, code review |
| **Público** | También no-técnicos | Desarrolladores |

*Regla:* ¿quiero **control de versiones** de la configuración? → **VS Code**. ¿Quiero enseñárselo a alguien mañana? → **portal**.

Muchos equipos usan los dos: portal para explorar, VS Code para producción.

### El workflow, 8 pasos (igual en ambos)

`Connect` → `Create` → `Configure` → `Add tools` → `Test` → `Iterate` → `Deploy` → `Integrate`

### Recursos Azure que necesitas

**Obligatorios — solo dos:**
- **Microsoft Foundry project**
- **Model deployments** (el modelo desplegado que dará vida al agente)

**Opcionales según lo que haga el agente:** Azure AI Search (retrieval avanzado) · Azure Storage (archivos) · Azure Key Vault (secretos) · Azure Functions (tools propias).

> Al crear el proyecto, la infraestructura base se aprovisiona sola. Lo demás se integra "por detrás" a medida que añades capacidades.

---

## 2. Crear un agente en el portal

`ai.azure.com` → tu proyecto → **Build > Agents** → **Create**

| Campo | Qué es |
| --- | --- |
| **Name** | Nombre descriptivo |
| **Description** | Para qué sirve — lo leen tus compañeros |
| **Model** | ⚠️ Un **deployment tuyo ya desplegado**, no "gpt-4o" a secas |
| **Instructions** | Cómo se comporta. **Lo que de verdad importa** |

> ⚠️ **Confusión frecuente — deployment ≠ modelo:** el desplegable de Model muestra *solo modelos que ya has desplegado*, no el catálogo entero.

Se prueba en el **Playground** integrado, que mantiene el historial durante la sesión (así verificas multi-turno).

---

## 3. Configurar: el YAML del agente

La extensión de VS Code ofrece dos vistas **sincronizadas**: el **Agent Designer** (visual) y el **YAML** (texto). Cambias en una y se refleja en la otra.

> El YAML de esta unidad es para **agentes declarativos prompt-based**. Los *hosted* se configuran en código; los *workflow* usan otro esquema.

```yaml
# yaml-language-server: $schema=https://aka.ms/ai-foundry-vsc/agent/1.0.0
version: 1.0.0
name: healthcare-assistant
description: Assists healthcare staff with appointment scheduling
id: 'agent-abc123xyz'          # ← lo genera la extensión, se usa en las APIs
metadata:
  authors: [developer-name]
  tags: [healthcare, scheduling]
model:
  id: 'gpt-4.1'
  options:
    temperature: 0.5
    top_p: 1
instructions: |
  You're a healthcare assistant...
  - Never access or share patient medical information
tools: []
```

**Cuatro secciones:** `metadata` · `model` · `instructions` · `tools`.

### Los dos parámetros del modelo

| Parámetro | Controla | Valores |
| --- | --- | --- |
| **Temperature** | Creatividad / aleatoriedad | **0.1–0.3** consistente · **0.7–1.0** creativo · **0.3–0.7** para agentes de negocio |
| **Top P** | Diversidad de vocabulario | Por defecto **1.0**. Bájalo para salidas más predecibles |

### Por qué el YAML y no el diseñador visual

Version control en Git · cambios masivos · plantillas reutilizables · code review · automatización por script.

---

## 4. Tools — el catálogo

El **tool catalog** (`Build > Tools`) tiene **tres categorías**:

| Categoría | Qué contiene |
| --- | --- |
| **Configured** | Built-in, listas para usar: Code Interpreter, File Search |
| **Catalog** | Añadibles desde registro: Bing Web Search, Azure AI Search, SharePoint, **MCP servers** |
| **Custom** | Las tuyas, vía **OpenAPI** o implementación propia |

### Las tools que hay que conocer

| Tool | Qué hace | Cuándo |
| --- | --- | --- |
| **Code Interpreter** | Escribe y ejecuta **Python en sandbox** | Cálculos, análisis de datos, gráficos |
| **File Search** | RAG sobre documentos que **tú subes**, indexados en un **vector store** | PDF, .docx, .txt, .md |
| **Bing Web Search** | Internet en tiempo real, **con citas automáticas** | Actualidad, más allá del entrenamiento |
| **Azure AI Search** | Retrieval sobre **índices empresariales que ya existen** | Escala enterprise |
| **OpenAPI tools** | Llama a APIs externas vía spec **OpenAPI 3.0** | Sistemas propios |

> **File Search vs Azure AI Search — cae seguro:**
> File Search = documentos que subes **al agente**. Azure AI Search = índices **que ya tienes** en tu organización.

Otras del catálogo: Browser Automation · Computer Use · Image Generation · SharePoint · Microsoft Fabric · Deep Research · **Agent-to-Agent** · Custom Code Interpreter.

### Tools en YAML

```yaml
tools:
  - type: code_interpreter
  - type: bing_grounding
    bing_grounding:
      connection_id: "your-connection-id"
  - type: file_search
    file_search:
      vector_store_ids:
        - "vectorstore-123"
```

Algunas tools necesitan parámetros extra: **connection ID** o referencia al **vector store**.

### MCP servers

**Model Context Protocol** = forma estandarizada de añadir tools propias. Viven en la categoría **Catalog**.

**Tres tipos:** **Remote** (hospedado fuera, el habitual en producción) · **Local** (tu máquina, para desarrollo) · **Custom** (implementación propia).

Ventajas: protocolo estándar · componentes reutilizables entre agentes · tools de la comunidad · integración simplificada.

### Buenas prácticas

- Built-in **antes** que custom.
- **Cada tool añade latencia** — no añadas tools sin propósito claro.
- Dile en las instructions **cuándo usar cada tool y cuándo no**.
- File Search: mantén los documentos al día.

---

## 5. Probar — 5 tipos de test

| Test | Qué verifica |
| --- | --- |
| **Happy path** | Lo esperado funciona |
| **Edge case** | Entradas ambiguas o incompletas |
| **Boundary** | Respeta los límites de sus instructions (peticiones fuera de alcance) |
| **Multi-turn** | Mantiene contexto entre mensajes |
| **Tool invocation** | Llama a la tool correcta en el momento correcto |

---

## 6. ⭐ Deploy vs Publish — lo más examinable del módulo

Son **dos cosas distintas** y es la trampa de esta unidad.

| | **Deploy** | **Publish** |
| --- | --- | --- |
| **Qué hace** | Guarda la configuración en tu **proyecto** | Crea un **recurso Azure** aparte |
| **Alcance** | Interno: tu equipo | Externo: cualquiera con permiso |
| **Resultado** | El agente vive en el workspace | **Agent Application** con endpoint estable |
| **Acción** | `Save` / `Save to Foundry` | `Publish` |

### Qué crea Publish

- **Agent Application** — recurso Azure con su **URL de invocación**, su política de auth y su **identidad de Entra propia**
- **Deployment** — instancia en ejecución de una versión concreta, con ciclo start/stop

### El endpoint

```
https://<recurso>.services.ai.azure.com/api/projects/<proyecto>/applications/<app>/protocols/openai/responses
```

**La URL no cambia entre versiones.** Publicas una versión nueva, el tráfico se enruta al 100% a ella, y los consumidores no se enteran.

### ⚠️ La trampa de permisos — memorízala

> Al publicar, el agente recibe una **identidad de Entra propia**, separada de la del proyecto. **Los permisos NO se heredan.**
> Hay que **reasignar los roles RBAC** a la nueva identidad para cada recurso que use el agente.
> Si te lo saltas: las tools que funcionaban en desarrollo **fallan con error de autorización** en producción.

### Autenticación

- **Microsoft Entra ID**, obligatorio
- El llamante necesita el rol **Foundry User** (antes *Azure AI User*) sobre el Agent Application. Si solo consume el agente, basta **Foundry Agent Consumer**
- **API keys NO soportadas** para Agent Applications
- `403 Forbidden` → falta el rol. Detalle de roles: [Seguridad](../D1-plataforma/04-seguridad-identidad-y-red.md)

Verificar el endpoint:

```bash
az account get-access-token --resource https://ai.azure.com
# luego POST con Authorization: Bearer <token>
```

### Estado de la conversación ⚠️

> Los endpoints de Agent Application soportan **solo la Responses API en modo stateless**.
> **El historial lo guardas tú en el cliente.**

Cuidado: dentro del proyecto el servicio persiste la conversación (nota 04), pero **el agente publicado no**. Es la excepción.

---

## 7. Producción

| Área | Qué hacer |
| --- | --- |
| **Monitoreo** | **Application Insights**: latencia, éxito de tools, errores, tokens |
| **Seguridad** | Managed identities, mínimo privilegio, retención de datos |
| **Costo** | Vigilar tokens, limitar longitud de respuesta, rate limiting |
| **Errores** | **Reintentos con backoff exponencial** (la respuesta correcta ante throttling / límites de cuota) |

---

## Para el examen

**Alto valor:**
- **Deploy (proyecto) vs Publish (Agent Application con endpoint)**
- **Identidad de Entra nueva al publicar → reasignar RBAC**
- **Foundry User** (antes *Azure AI User*) + Entra ID; **API key no vale**
- **File Search (tus subidas) vs Azure AI Search (índices existentes)**
- Las 3 categorías del catálogo: **Configured / Catalog / Custom**
- El agente publicado es **stateless** — el historial lo guarda el cliente
- Recursos mínimos: **proyecto + model deployment**

**Valor medio:** estructura del YAML · temperature vs top_p · los 3 tipos de MCP server · los 5 tipos de test · Application Insights.

**Bajo valor:** pasos de instalación de la extensión · navegación exacta del portal · la lista completa de tools del catálogo.

---

## Comprueba que lo tienes

1. Guardas tu agente en el proyecto y un compañero lo prueba. Un sistema externo necesita llamarlo por HTTP. ¿Qué te falta hacer?
2. Publicas el agente. En desarrollo leía un blob de Storage; en producción falla con error de autorización. ¿Por qué?
3. Un cliente quiere autenticarse contra tu agente publicado con una API key. ¿Qué le dices?
4. Tienes 50.000 documentos ya indexados en un índice de tu empresa. ¿File Search o Azure AI Search?
5. Tu agente publicado responde bien, pero olvida lo que dijiste hace dos mensajes. ¿Qué pasa y cómo lo arreglas?
6. Necesitas que la configuración del agente pase por code review antes de producción. ¿Portal o VS Code, y por qué?
7. ¿Qué dos recursos Azure son imprescindibles para empezar?

<details>
<summary>Respuestas</summary>

1. **Publicar** (Publish), no solo desplegar. Eso crea el **Agent Application** con su endpoint estable e invocable desde fuera del proyecto.
2. Al publicar recibió una **identidad de Entra propia**, distinta de la del proyecto. Los permisos no se heredan: hay que **reasignar el rol RBAC** sobre el Storage a la nueva identidad.
3. Que no se puede. Los Agent Applications usan **Entra ID exclusivamente**; necesita el rol **Foundry User** (antes *Azure AI User*), o **Foundry Agent Consumer** si solo consume. Sin rol, `403 Forbidden`.
4. **Azure AI Search** — conecta con índices empresariales que ya existen. File Search es para documentos que subes al agente y se indexan en un vector store.
5. Los endpoints de Agent Application son **stateless** (Responses API). El **historial lo mantiene tu cliente** y lo envía en cada llamada.
6. **VS Code**: el YAML se versiona en Git junto a tu código y entra en el flujo normal de code review.
7. **Microsoft Foundry project** y **model deployments**. El resto (AI Search, Storage, Key Vault, Functions) es opcional.

</details>
