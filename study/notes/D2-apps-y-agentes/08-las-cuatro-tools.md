# Las tools del agente en detalle

> Docs de Azure — [Code Interpreter](https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/tools/code-interpreter) · [File Search](https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/tools/file-search) · [Function calling](https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/tools/function-calling)
> Cubre: **D2.1.c**, **D2.2.c** · Complementa [07 Construir y publicar agentes](07-construir-y-publicar-agentes.md), que da la visión de catálogo
> Peso: **alto** — D2 es el dominio más pesado (30–35%) y las tools son su parte más práctica
> Fecha: 2026-08-09

---

## En cristiano

Un modelo solo sabe generar texto. Las **tools** son lo que le permite *hacer cosas*.

Hay una diferencia crítica entre ellas y es lo que más se pregunta:

| Tool | ¿Quién ejecuta? |
| --- | --- |
| **Code Interpreter** | **Azure**, en un sandbox suyo |
| **File Search** | **Azure**, contra un vector store suyo |
| **Web / Bing Search** | **Azure**, saliendo a internet |
| **Function calling** | **⚠️ TU CÓDIGO** |

**Function calling es la excepción y por eso cae:** el modelo no ejecuta nada, solo te *pide* que ejecutes tú y le devuelvas el resultado.

---

## 1 · Code Interpreter — Python en un sandbox

El agente **escribe y ejecuta Python** en un entorno aislado gestionado por Microsoft.

**Para qué:** cálculos, análisis de datos, generar gráficos, procesar ficheros.

### Lo que hay que saber

| Aspecto | Detalle |
| --- | --- |
| **Dónde corre** | Sandbox de Azure Container Apps, aislado por **Hyper-V** |
| **Sesión** | Activa **1 hora**, timeout por inactividad de **30 minutos** |
| **Coste** | ⚠️ **Cobra aparte** de los tokens |
| **Concurrencia** | Dos conversaciones simultáneas = **dos sesiones** = doble coste |
| **Internet** | ❌ **No puede salir a la red**. Tampoco hereda tu subred |
| **Región** | La misma que tu proyecto |
| **Paquetes** | Set fijo de librerías de data science. Para otras → *Custom code interpreter* |

**Ficheros:** se suben con `purpose="assistants"` y se montan en `/mnt/data/`. Acepta CSV, XLSX, PDF, DOCX, PPTX, JSON, imágenes, ZIP y bastantes formatos de código.

Los ficheros que **genera** (un PNG, por ejemplo) vuelven como anotación `container_file_citation`, con `container_id` y `file_id` para descargarlos.

> ⚠️ **Confusión frecuente:** el sandbox **no tiene internet**. Si el agente necesita datos externos, es otra tool la que los trae; Code Interpreter solo procesa lo que ya tiene.

---

## 2 · File Search — RAG llave en mano

Búsqueda semántica sobre documentos **que tú subes**. Azure los trocea, los vectoriza y los guarda en un **vector store**.

**Es RAG sin montar la infraestructura.** Todo el pipeline del apunte [06 RAG y grounding](06-rag-grounding.md) — chunking, embeddings, búsqueda — lo hace el servicio por ti.

### File Search vs Azure AI Search

| | **File Search** | **Azure AI Search** |
| --- | --- | --- |
| **Los datos** | Los subes **al agente** | Ya están en **tu índice** |
| **Quién indexa** | Azure, automático | Tú lo tenías montado |
| **Escala** | Documentos de un agente | **Empresarial** |
| **Control** | Poco: es una caja negra | Total: chunking, ranking, filtros |
| **Aislamiento de red** | ❌ **No soportado** | ✅ Sí, con private endpoint |

**Regla:** *"ya tenemos los documentos indexados"* → **Azure AI Search**. *"sube estos PDF al agente"* → **File Search**.

> ⚠️ **Dato de examen:** con **aislamiento de red, File Search no está soportado**. Si el requisito es red privada, la respuesta es Azure AI Search.

---

## 3 · Web / Bing Search — internet en tiempo real

Da al agente información **posterior a su entrenamiento**, con **citas automáticas** de las fuentes.

> ⚠️ Sale por **endpoint público** aunque tu recurso esté aislado. Ver [Seguridad](../D1-plataforma/04-seguridad-identidad-y-red.md).

---

## 4 · Function calling — ⭐ tu código, no el de Azure

Aquí está la diferencia de fondo. **El agente no ejecuta tu función.** Te dice qué función quiere y con qué argumentos; tú la ejecutas y le devuelves el resultado.

### El ciclo, 5 pasos

| # | Paso | Quién |
| --- | --- | --- |
| 1 | **Definir** la función: nombre, descripción, parámetros (JSON Schema) | Tú |
| 2 | **Registrar** el agente con esa definición | Tú |
| 3 | El modelo decide que hace falta y **pide la llamada** | Azure |
| 4 | **Ejecutas** y devuelves la salida | **Tú** |
| 5 | El agente **redacta la respuesta final** con ese dato | Azure |

### ⚠️ El flujo cambió — esto importa

| API | Cómo se pide | Cómo se devuelve |
| --- | --- | --- |
| **Assistants** (antigua) | El run pasa a `requires_action` | `submit_tool_outputs` |
| **Responses** (actual) | Item de tipo **`function_call`** | Item **`function_call_output`** con el `call_id` |

> **Confusión frecuente:** `requires_action` y `submit_tool_outputs` son de la **Assistants API**. El material actual de Foundry usa **`function_call` / `function_call_output`**. Reconoce ambos: el concepto es idéntico (el servicio se detiene y espera tu resultado), cambia el nombre.

### La definición

```python
func_tool = FunctionTool(
    name="get_horoscope",
    description="Get today's horoscope for an astrological sign.",   # ← el modelo decide leyendo esto
    parameters={
        "type": "object",
        "properties": {
            "sign": {"type": "string", "description": "An astrological sign like Taurus"},
        },
        "required": ["sign"],
        "additionalProperties": False,
    },
    strict=True,      # validación estricta del esquema
)
```

**La `description` no es documentación: es lo que el modelo lee para decidir si llamarla.** Una descripción vaga = la tool no se usa o se usa mal.

### Devolver el resultado

```python
for item in response.output:
    if item.type == "function_call":                 # ← el modelo pide
        resultado = get_horoscope(**json.loads(item.arguments))
        input_list.append(FunctionCallOutput(
            type="function_call_output",
            call_id=item.call_id,                    # ← debe coincidir
            output=json.dumps({"horoscope": resultado}),
        ))

response = openai.responses.create(input=input_list, conversation=conversation.id, ...)
```

### ⚠️ Los runs expiran a los 10 minutos

Si tardas más en devolver la salida, se pierde. Para operaciones largas: **devuelve un estado inmediatamente y haz polling aparte**. Los 10 minutos son de tiempo total transcurrido, no de la ejecución de tu función.

### Seguridad

- **Los argumentos y salidas son entrada no confiable** — valida y sanea
- **Nunca devuelvas secretos** en la salida de una tool
- **Mínimo privilegio** en la identidad
- **Human-in-the-loop** para acciones con efectos: en el Agent Framework, `approval_mode="always_require"` en producción

---

## Tabla de decisión

| Necesito… | Tool |
| --- | --- |
| Calcular, analizar datos, hacer un gráfico | **Code Interpreter** |
| Responder sobre PDF que subo yo | **File Search** |
| Responder sobre un índice empresarial ya existente | **Azure AI Search** |
| Datos de actualidad, con citas | **Web / Bing Search** |
| Llamar a **mi** API o base de datos | **Function calling** |
| Conectar a una API externa que tiene spec | **OpenAPI tool** |
| Tools reutilizables entre agentes y proyectos | **MCP server** |

---

## Para el examen

**Alto valor:**
- **Function calling lo ejecuta TU código**; las demás las ejecuta Azure
- **`function_call` → `function_call_output`** (Responses API). El antiguo `requires_action` / `submit_tool_outputs` es de Assistants
- **File Search vs Azure AI Search**: tú los subes vs ya indexados
- **Con red privada, File Search no está soportado**
- **Code Interpreter no tiene internet** y **cobra aparte**
- La **`description`** de una función es lo que hace que el modelo la use bien

**Valor medio:** sesión de 1 h / 30 min de idle · runs expiran a los 10 min · `purpose="assistants"` y `/mnt/data/` · `strict: true` · anotaciones `container_file_citation`.

**Bajo valor:** la lista completa de MIME types · los nombres exactos de clases por SDK.

---

## Comprueba que lo tienes

1. Tu agente debe consultar el stock en tu ERP interno. ¿Qué tool y quién ejecuta la consulta?
2. Le pides a Code Interpreter que descargue un CSV de una URL pública y lo analice. ¿Funciona?
3. Tu empresa exige que todo el tráfico se quede en la red privada. Necesitas RAG sobre documentos internos. ¿File Search?
4. Tu función tarda 15 minutos en responder. ¿Qué pasa y cómo lo resuelves?
5. Definiste una función `procesar` con descripción *"procesa datos"* y el agente casi nunca la llama. ¿Por qué?
6. En la respuesta te llega un item `function_call`. ¿Qué campo necesitas conservar y para qué?
7. Tu agente atiende 50 conversaciones a la vez usando Code Interpreter. ¿Qué implicación tiene?

<details>
<summary>Respuestas</summary>

1. **Function calling**, y lo ejecuta **tu código**. Azure solo te dice qué función quiere y con qué argumentos; tú consultas el ERP y devuelves el resultado con `function_call_output`.
2. **No.** El sandbox de Code Interpreter **no puede hacer peticiones de red salientes**. Habría que traer el CSV con otra tool o subirlo como fichero.
3. **No.** File Search **no está soportado con aislamiento de red**. Usa **Azure AI Search** con private endpoint.
4. **El run expira a los 10 minutos** y pierdes la salida. Solución: devolver **un estado inmediatamente** e implementar polling por separado.
5. Porque **la `description` es lo que el modelo lee para decidir**. "Procesa datos" no dice cuándo usarla. Hay que describir qué hace y cuándo aplica, con parámetros documentados.
6. El **`call_id`**. Debe ir en el `function_call_output` para que el servicio empareje tu resultado con la llamada que lo pidió.
7. **50 sesiones de Code Interpreter simultáneas**, cada una facturada aparte de los tokens. Cada conversación crea su propia sesión: el coste se multiplica.

</details>
