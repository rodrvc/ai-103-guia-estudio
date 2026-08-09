# Fundamentos: recurso, proyecto, modelo, deployment, endpoint

> **Empieza por aquí.** El resto de apuntes da por sabido este vocabulario.
> Cubre: base de **D1.1**, **D1.2** · imprescindible para entender D2 entero
> Peso: **alto** de forma indirecta — no suele preguntarse solo, pero sin esto no se entiende ninguna otra pregunta

---

## En cristiano

Microsoft usa cinco palabras que parecen sinónimos y no lo son. Confundirlas es el error más común al empezar — y el examen lo explota.

La analogía: **montar un restaurante.**

| Palabra de Azure | En el restaurante | Qué es de verdad |
| --- | --- | --- |
| **Resource** (recurso) | El local que alquilas | La unidad que Azure factura y ubica en una región |
| **Project** (proyecto) | Un restaurante concreto dentro del local | Donde agrupas tus modelos, agentes, datos y conexiones |
| **Model** (modelo) | Una receta del libro de cocina | GPT-4.1, Sora… existe en el catálogo, no es tuyo todavía |
| **Deployment** (despliegue) | El plato en **tu** carta, con **tu** nombre | Una instancia del modelo, tuya, con nombre y capacidad propios |
| **Endpoint** | La dirección del restaurante | La URL a la que tu código manda las peticiones |

---

## La cadena, en orden

```
Resource → Project → Deployment (de un Model) → Endpoint
```

**Se lee así:** creas un recurso, dentro pones un proyecto, dentro del proyecto despliegas un modelo del catálogo, y eso te da un endpoint al que llamar.

---

## ⚠️ La confusión que más cuesta: modelo ≠ deployment

Esta es la número uno. Aparece en preguntas de examen disfrazada de mil formas.

| | Modelo | Deployment |
| --- | --- | --- |
| **Qué es** | La receta del catálogo | **Tu** instancia de esa receta |
| **Nombre** | `gpt-4.1` (lo pone Microsoft) | `mi-chat-produccion` (lo pones tú) |
| **Existe** | Para todo el mundo | Solo en tu proyecto |
| **Tiene cuota** | No | **Sí** — TPM o PTU |

**La consecuencia práctica:** cuando en tu código escribes `model="..."`, **no pones el nombre del modelo: pones el nombre de tu deployment.**

```python
# ❌ No es el nombre del catálogo
response = client.responses.create(model="gpt-4.1", input="Hola")

# ✅ Es el nombre que TÚ le diste al desplegarlo
response = client.responses.create(model="mi-chat-produccion", input="Hola")
```

> Puedes tener **tres deployments del mismo modelo** con nombres, cuotas y content filters distintos. Por eso el nombre del modelo no bastaría para identificar a cuál llamas.

---

## Los dos endpoints que se confunden

Un proyecto de Foundry expone **dos URLs distintas**, y no sirven para lo mismo:

| Endpoint | Forma | Cuándo se usa |
| --- | --- | --- |
| **Project endpoint** | `https://<recurso>.services.ai.azure.com/api/projects/<proyecto>` | Con el **Foundry SDK** (`azure-ai-projects`): agentes, conexiones, evaluaciones |
| **Azure OpenAI endpoint** | `https://<recurso>.openai.azure.com/openai/v1` | Con el **OpenAI SDK** directamente, solo para inferencia |

*Regla:* si necesitas cosas de la plataforma (agentes, conexiones, evaluación) → **project endpoint**. Si solo quieres hablar con un modelo → cualquiera de los dos, pero el de OpenAI es más directo.

Detalle completo: [Conectar tu app a Foundry](../D2-apps-y-agentes/01-conectar-app-a-foundry.md).

---

## Cómo se autentica: sin claves

Azure recomienda **no usar API keys**. En su lugar, tres piezas que van juntas:

| Pieza | Pregunta que responde | Ejemplo |
| --- | --- | --- |
| **Identity** | ¿Quién eres? | Managed Identity, Service Principal, tu usuario |
| **RBAC** | ¿Qué puedes hacer? | Rol *Azure AI User* sobre el recurso |
| **Least privilege** | ¿Cuánto de eso necesitas? | Solo el rol mínimo, solo donde haga falta |

**RBAC** = *Role-Based Access Control*. Se lee siempre igual:

> *A esta **identidad*** → *le doy este **rol*** → *sobre este **recurso***

No das permisos sueltos: das un rol, que es un paquete de permisos con nombre.

Detalle y código: [Conectar tu app a Foundry § Autenticación](../D2-apps-y-agentes/01-conectar-app-a-foundry.md).

---

## Para el examen

**Alto valor:**
- **`model=` recibe el nombre del deployment**, no el del modelo
- **Project endpoint vs Azure OpenAI endpoint** y qué SDK usa cada uno
- La cadena **recurso → proyecto → deployment → endpoint**
- **RBAC** = identidad + rol + recurso; base de la autenticación sin claves

**Valor medio:** un mismo modelo puede tener varios deployments con cuotas y filtros distintos.

**Bajo valor:** la jerarquía exacta de facturación de recursos.

---

## Comprueba que lo tienes

1. Despliegas `gpt-4.1` y lo llamas `asistente-legal`. ¿Qué cadena escribes en el parámetro `model=` de tu código?
2. ¿Por qué querrías dos deployments del mismo modelo en un mismo proyecto?
3. Tu app solo necesita mandar prompts y recibir respuestas. ¿Qué endpoint y qué SDK son suficientes?
4. Explica RBAC en una frase, con sus tres piezas.

<details>
<summary>Respuestas</summary>

1. **`asistente-legal`** — el nombre del deployment, el que tú elegiste. Nunca `gpt-4.1`.
2. Para tener **cuotas separadas** (que un equipo no consuma la del otro), **content filters distintos**, o **tipos de deployment distintos** (uno standard para producción, otro batch para procesos nocturnos).
3. El **Azure OpenAI endpoint** (`https://<recurso>.openai.azure.com/openai/v1`) con el **OpenAI SDK**. El project endpoint y el Foundry SDK hacen falta cuando necesitas agentes, conexiones o evaluaciones.
4. **A una identidad le asignas un rol sobre un recurso** — así Azure decide quién puede hacer qué, sin repartir claves.

</details>
