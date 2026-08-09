# Seguridad: identidad, RBAC y aislamiento de red

> Docs de Azure — [RBAC for Microsoft Foundry](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/rbac-azure-ai-foundry) · [Network isolation](https://learn.microsoft.com/en-us/azure/ai-foundry/how-to/configure-private-link)
> Cubre: **D1.3.d** *Configure security: managed identity, private networking, keyless credentials, role policies*
> Peso: **alto** — objetivo directo de D1 (25–30%). El curso oficial lo toca de pasada
> Fecha: 2026-08-09

---

## En cristiano

Proteger una solución de IA en Azure son **dos preguntas separadas**, y confundirlas es el error clásico:

| Pregunta | Se responde con | Analogía |
| --- | --- | --- |
| **¿Quién eres y qué puedes hacer?** | Identidad + RBAC | La credencial y qué puertas abre |
| **¿Por dónde puedes entrar?** | Redes privadas | Si el edificio da a la calle o solo al patio interior |

Una no sustituye a la otra. Puedes tener RBAC perfecto y el recurso expuesto a internet, o una red blindada donde cualquiera de dentro lo puede todo.

---

## 1 · Identidad: quién eres

Tres formas de autenticarse, según **dónde corre tu código**:

| Dónde corre | Qué usas | Por qué |
| --- | --- | --- |
| **Dentro de Azure** (App Service, Functions, VM, AKS) | **Managed Identity** | Azure gestiona la credencial. **Nunca hay secreto que guardar** |
| **Fuera de Azure** (on-prem, otra nube, CI/CD) | **Service Principal** | Hay secreto o certificado, pero es una identidad de Entra con roles |
| **Tu portátil, desarrollando** | **`az login`** | Usa tu propio usuario |

`DefaultAzureCredential` prueba varias en cadena, así que **el mismo código funciona en los tres sitios** sin cambios.

### ⚠️ La regla que cae: API key vs Entra ID

> **Si usas una API key, RBAC no se aplica.** La clave da **acceso completo**, sin restricción de rol.

Por eso Microsoft recomienda **keyless** (Entra ID) siempre. Una key es todo o nada; un rol es granular y revocable.

Código y cadena de fallback: [Conectar tu app a Foundry](../D2-apps-y-agentes/01-conectar-app-a-foundry.md).

---

## 2 · RBAC: qué puedes hacer

Tres piezas, siempre en el mismo orden:

> **A una identidad** → **le doy un rol** → **sobre un scope**

### Los scopes, de más amplio a más estrecho

| Scope | Qué abarca |
| --- | --- |
| Suscripción / resource group | Todo lo de dentro |
| **Foundry resource** | Frontera administrativa y de seguridad |
| **Foundry project** | Un proyecto concreto |
| **Agent** | **Un agente individual** — solo para acceso a su endpoint |

> El scope de **agente** es útil para dar acceso a *un* agente sin abrir todos los del proyecto. Ojo: solo se evalúa para acceso al endpoint, no da permisos de gestión.

### ⚠️ Los roles se renombraron

Este es un cambio reciente que puede aparecer con cualquiera de los dos nombres:

| Nombre nuevo | Nombre antiguo |
| --- | --- |
| **Foundry User** | Azure AI User |
| **Foundry Owner** | Azure AI Owner |
| **Foundry Account Owner** | Azure AI Account Owner |
| **Foundry Project Manager** | Azure AI Project Manager |

Los IDs y los permisos **no cambiaron**, solo el nombre.

### Los 5 roles y qué permite cada uno

| Rol | Para quién | Puede… |
| --- | --- | --- |
| **Foundry Agent Consumer** | Quien **solo llama** a un agente | Interactuar con endpoints. **Nada más** |
| **Foundry User** | Desarrollador que construye y prueba | Build en el proyecto + lectura. **No crea proyectos ni publica** |
| **Foundry Project Manager** | Team lead | Todo lo anterior + **publicar agentes** + asignar el rol Foundry User |
| **Foundry Account Owner** | Manager / plataforma | Crear cuentas y proyectos, gestionar modelos. **No hace build** |
| **Foundry Owner** | Rol completo | Todo: crear, construir, publicar y asignar roles |

**Los dos de mínimo privilegio:**
- ¿Solo consume el agente? → **Foundry Agent Consumer**
- ¿Desarrolla agentes? → **Foundry User**

> ⚠️ **Cae seguro:** para **publicar** un agente hace falta como mínimo **Foundry Project Manager** sobre el scope del recurso. `Foundry User` **no puede publicar**.

> ⚠️ **Confusión frecuente:** no uses roles que empiecen por **Cognitive Services** ni el rol **Azure AI Developer** para trabajar con Foundry. Pese al nombre, *Azure AI Developer* está pensado para workspaces de Azure Machine Learning y hubs, no para proyectos de Foundry.

### Lo mínimo para arrancar

Dos asignaciones de **Foundry User** sobre el recurso:
1. A **tu usuario**
2. A la **managed identity del proyecto**

Si quien crea el proyecto puede asignar roles (es Owner), ambas se hacen solas.

### Recursos externos no heredan nada

Si conectas un Storage o un AI Search creados fuera de Foundry, hay que **darle el rol a la managed identity de Foundry** sobre ese recurso. Ejemplo: *Storage Blob Data Reader* sobre la cuenta de Storage.

Es la misma trampa que al **publicar un agente**: identidad nueva, permisos que no se heredan → ver [Construir y publicar agentes](../D2-apps-y-agentes/07-construir-y-publicar-agentes.md).

---

## 3 · Red: por dónde se entra

### Entrada (inbound) — tres niveles

Se controla con el flag **Public Network Access (PNA)**:

| Configuración | Quién puede llegar |
| --- | --- |
| **Enabled** | Cualquiera desde internet (con credencial válida) |
| **Enabled from selected IP addresses** | Solo las IPs que listes |
| **Disabled** + **private endpoint** | **Solo desde tu red virtual** |

**Private endpoint** = a tu recurso se le asigna una **IP privada dentro de tu VNet**. El tráfico deja de salir a internet.

**Cómo funciona el DNS** — la parte elegante:

> La **cadena de conexión no cambia**. Azure crea una zona DNS privada `privatelink`. Desde dentro de la VNet, el nombre resuelve a la **IP privada**; desde fuera, a la pública. Mismo código, distinto camino.

Si usas DNS propio, tienes que delegarle el subdominio `privatelink` — si no, resolverá a la IP pública y no entrarás.

### Salida (outbound) — VNet injection

Para que **el agente** solo hable con lo que tú permitas, se inyecta en una **subred tuya** (delegada a `Microsoft.App/environments`, mínimo **/27**).

Con eso puedes poner un **firewall de egreso** delante y controlar a dónde sale.

### ⚠️ No todas las tools funcionan aisladas

Esto es muy examinable porque rompe suposiciones:

| Por dónde va | Tools |
| --- | --- |
| **Tu VNet** (privado) | MCP privado · Azure AI Search · OpenAPI · Azure Functions · Agent-to-Agent |
| **Backbone de Microsoft** | Code Interpreter (parcial) · Function Calling |
| **Endpoint público** ⚠️ | **Bing Grounding · Web Search · SharePoint Grounding** |
| **No soportadas** ❌ | **File Search** · Logic Apps · Browser Automation · Computer Use · Image Generation |

> **La trampa:** Bing y Web Search *funcionan* en un entorno aislado, pero **salen a internet**. Si el requisito es "todo el tráfico dentro de la red privada", **no cumplen**. Se bloquean con Azure Policy.
>
> Y **File Search directamente no está soportado** con aislamiento de red — hay que usar Azure AI Search en su lugar.

### Trusted Azure services

Con la red restringida, puedes hacer una excepción para servicios de confianza (Foundry Tools, Azure AI Search, Azure Machine Learning). Se autentican con **managed identity**, no abriendo la red.

---

## Para el examen

**Alto valor:**
- **API key → RBAC no aplica**, acceso total. Por eso keyless
- **Managed Identity dentro de Azure · Service Principal fuera**
- **Publicar un agente exige Foundry Project Manager**; Foundry User no basta
- **Foundry Agent Consumer** = mínimo privilegio para quien solo llama
- **PNA: Enabled / selected IPs / Disabled + private endpoint**
- **Bing y Web Search salen por endpoint público** aunque el recurso esté aislado
- Los recursos externos **no heredan permisos**: hay que asignar el rol a la managed identity

**Valor medio:** los 4 scopes (incluido el de agente) · el renombrado de roles · DNS `privatelink` y que la cadena de conexión no cambia · VNet injection con subred /27 · trusted services.

**Bajo valor:** GUIDs de los roles · pasos exactos del portal · lista completa de FQDNs del firewall.

---

## Comprueba que lo tienes

1. Tu app corre en Azure App Service y necesita llamar a un modelo de Foundry. ¿Qué usas para autenticarte y por qué no una API key?
2. Un desarrollador tiene el rol **Foundry User** y no consigue publicar su agente. ¿Qué le falta?
3. Un cliente externo solo debe poder invocar un agente concreto, sin ver ni tocar los demás. ¿Qué rol y a qué scope?
4. Requisito: *"ningún dato puede viajar por internet público"*. Tu agente usa Bing Web Search. ¿Cumples?
5. Desactivas el acceso público y creas un private endpoint. ¿Hay que cambiar la cadena de conexión de tus apps?
6. Conectas una cuenta de Storage que ya existía a tu proyecto de Foundry y el agente no puede leerla. ¿Por qué?
7. Alguien propone dar el rol **Azure AI Developer** al equipo que construye agentes. ¿Es correcto?

<details>
<summary>Respuestas</summary>

1. **Managed Identity** (con `DefaultAzureCredential`). Corre dentro de Azure, así que no hay secreto que guardar. Con API key **RBAC no se aplica**: la clave da acceso completo sin restricción de rol.
2. El rol **Foundry Project Manager** (mínimo) sobre el scope del recurso. Foundry User puede construir y probar, pero **no publicar**.
3. **Foundry Agent Consumer** sobre el **scope del agente** concreto. Es el mínimo privilegio: solo interactúa con ese endpoint, sin permisos de gestión ni acceso a los demás agentes.
4. **No.** Bing Grounding y Web Search funcionan en entornos aislados, pero **se comunican por endpoint público**. Si el requisito es tráfico privado extremo a extremo, hay que bloquearlas con Azure Policy y usar otra fuente.
5. **No.** Azure crea la zona DNS privada `privatelink`: desde dentro de la VNet el mismo nombre resuelve a la IP privada. Solo hay que ajustarlo si usas un servidor DNS propio.
6. Porque **los recursos creados fuera de Foundry no heredan permisos**. Hay que asignar a la **managed identity** de Foundry el rol correspondiente (p. ej. *Storage Blob Data Reader*) sobre esa cuenta.
7. **No.** Pese al nombre, *Azure AI Developer* está pensado para workspaces de Azure Machine Learning y hubs, no para proyectos de Foundry. Lo correcto es **Foundry User** (o Foundry Owner).

</details>
