# Deployment types en Foundry

> Docs de Azure — [Understanding deployment types](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types) · actualizado 2026-05-18
> Cubre: **D1.2.b** *Choose appropriate deployment options*
> Peso: **alto** — D1 pesa 25–30% y este es uno de sus objetivos más concretos
> ⚠️ El curso oficial de Learn **no cubre** este tema. Fuente: la documentación de Azure.
> Fecha: 2026-08-04

---

## En cristiano primero

Solo tomas **dos decisiones**. Todo lo demás son combinaciones.

**1. ¿Cómo pagas?**

| Analogía | Tipo | Qué significa |
| --- | --- | --- |
| 🚕 **Taxi** | Standard | Pagas por lo que usas. Barato si andas poco. En hora punta, esperas |
| 🚗 **Auto propio con chofer** | Provisioned | Pagas fijo, lo uses o no. Siempre disponible, sin esperas |
| 📦 **Encomienda** | Batch | Lo dejas y vuelves mañana. **Mitad de precio**, sin apuro |

**2. ¿Dónde se procesa?**

| Analogía | Tipo |
| --- | --- |
| "Me da igual, el que esté libre" | **Global** |
| "Dentro de Europa" (o EE.UU., o Asia) | **Data Zone** |
| "Solo en este datacenter" | **Regional** |

**La regla de oro:** elige el nivel de restricción **que pide el requisito, ni más ni menos**. Si el enunciado dice "la UE" → Data Zone. Si dice "Alemania" → Regional. Restringir de más no te hace más legal, solo te limita (menos cuota, menos modelos, más riesgo de saturación).

**Dónde viven tus datos:** tu ropa vive en tu casa (*at rest* → siempre en la geografía designada, en todos los tipos), pero la lavandería puede estar en otro barrio (*procesamiento* → esto es lo que cambia). **Cuando preguntan por GDPR o residencia, hablan de la lavandería.**

---

## Las 3 decisiones que tomas al desplegar

Un deployment type define tres cosas a la vez:

1. **Dónde se procesan tus datos** — global / data zone / una región
2. **Cómo pagas** — por token (*standard*) o capacidad reservada (*provisioned*)
3. **Rendimiento** — variabilidad de latencia y límites de throughput

---

## Los dos ejes — la clave de todo

Casi todos los tipos salen de cruzar dos ejes:

| | **Standard** (pay-per-token) | **Provisioned** (PTU reservado) | **Batch** (50% dcto, 24 h) |
| --- | --- | --- | --- |
| **Global** | `GlobalStandard` | `GlobalProvisionedManaged` | `GlobalBatch` |
| **Data Zone** | `DataZoneStandard` | `DataZoneProvisionedManaged` | `DataZoneBatch` |
| **Regional** | `Standard` | `ProvisionedManaged` | — |

Más dos casos aparte:

- **Instant (preview)** — sin deployment. Llamas al modelo por nombre. Para prototipar.
- **Developer** (`DeveloperTier`) — solo para evaluar modelos fine-tuned. **Sin SLA, sin garantías de residencia, vida fija de 24 h** y se borra solo.

*Ojo:* provisioned **no** tiene variante regional-batch, y batch no existe en regional.

---

## Residencia de datos — lo que más se pregunta

**Los datos en reposo (*at rest*) siempre se quedan en la geografía de Azure designada.** Lo que cambia es dónde se **procesa** la inferencia:

| Tipo | Procesa en |
| --- | --- |
| **Global** | Cualquier región de Azure |
| **DataZone** | Solo dentro de la data zone: **US, EU o APAC** |
| **Standard / Regional** | Solo la región del deployment |

### Las tres data zones

| Zona | Cubre |
| --- | --- |
| **United States** | Cualquier región de EE.UU. |
| **European Union** | **EU Data Boundary** — Francia, Alemania, Italia, Países Bajos, Noruega, Polonia, España, Suecia, Suiza *(a mayo 2026; pueden añadirse más sin aviso)* |
| **Asia Pacific** | Australia, Japón, Corea, Singapur, India |

**Data Zone es el punto medio:** más cuota que regional, más control que global.

---

## Cómo elegir — las tres preguntas

### 1. ¿Requisito de residencia?

- Sin restricción → **Global**
- Cumplimiento por zona (GDPR, etc.) → **Data Zone**
- Una sola región obligatoria → **Standard / Regional Provisioned**

### 2. ¿Patrón de carga?

| Carga | Tipo |
| --- | --- |
| Prototipo / probar un modelo | **Instant** (sin deployment) |
| Tráfico variable, a ráfagas | **Standard** o **Global Standard** |
| Alto volumen constante | **Provisioned** (PTU) |
| Lotes grandes, no urgentes | **Batch** — 50% más barato, 24 h |
| Evaluar un modelo fine-tuned | **Developer** |

### 3. ¿Latencia?

- Necesitas **baja variabilidad** → **Provisioned**
- Toleras variabilidad → **Standard**

---

## Trade-offs que hay que tener claros

**Global Standard** — punto de partida habitual. Mayor cuota por defecto, no necesitas balancear entre recursos, y **recibe los modelos nuevos primero**. A cambio: con volumen alto y constante, **más variabilidad de latencia**.

**Provisioned** — compras PTUs fijos. Throughput garantizado, latencia baja y consistente. Caro si tu tráfico es irregular: pagas la capacidad, la uses o no.

**Batch** — 50% de descuento y **cuota de tokens separada**, así que no interfiere con tu tráfico online. A cambio: **sin SLA de tiempo real**, objetivo 24 h y puede tardar más. Para análisis masivo, generación de contenido, resúmenes de documentos.

**SLA:** provisioned garantiza throughput · standard es *best-effort* · **Developer no tiene SLA**.

---

## Dos detalles que suelen caer

**Alta disponibilidad:** con Global Standard y Data Zone Standard, si la **región primaria** cae, *todo* el tráfico enrutado allí se ve afectado. El enrutamiento dinámico no es failover automático.

**Azure Policy** puede **prohibir** deployment types concretos en tu organización. Se filtra por `sku.name` (ej. bloquear `GlobalStandard` para forzar cumplimiento de residencia). Esto es gobernanza, no solo una decisión técnica.

---

## Dos ideas que conviene fijar

- ⚠️ **Deployment ≠ modelo.** El deployment type es parte de lo que define un *deployment* — junto al modelo, la versión, la cuota y el content filter. El deployment es **tu instancia configurada**, no el modelo del catálogo.
- **TPM vs PTU.** Aquí aterriza la distinción: **Standard = pay-per-token (TPM)**, **Provisioned = capacidad reservada (PTU)**. Es la misma decisión, vista desde el despliegue.

---

## Para el examen

**Alto valor:**
- Los dos ejes: **global / data zone / regional** × **standard / provisioned**
- Residencia: at rest siempre en la geografía; el **procesamiento** es lo que cambia
- Las 3 data zones: **US, EU, APAC**
- Batch = 50% dcto + 24 h + cuota separada, **sin SLA de tiempo real**
- Provisioned = latencia predecible; Standard = variable

**Valor medio:**
- Nombres de SKU (`GlobalStandard`, `DataZoneProvisionedManaged`…)
- Developer tier: solo fine-tuned, 24 h, sin SLA
- Global recibe modelos nuevos primero
- Azure Policy para restringir tipos

**Bajo valor:** la lista exacta de países de cada data zone, el JSON de la policy.

---

## Comprueba que lo tienes

1. Un banco alemán exige que los datos no salgan de la UE, con tráfico alto y constante, y latencia predecible. ¿Qué deployment type?
2. Necesitas resumir 500.000 documentos este mes. No corre prisa. ¿Qué eliges y cuánto ahorras?
3. ¿Qué diferencia hay entre *datos en reposo* y *procesamiento de inferencia* en cuanto a residencia?
4. Tu app usa Global Standard y la región primaria cae. ¿El tráfico se reenruta solo?
5. ¿Por qué Developer tier no sirve para producción, aunque sea el más barato?

<details>
<summary>Respuestas</summary>

1. **Data Zone Provisioned** (`DataZoneProvisionedManaged`) en región EU. Data zone → cumplimiento; provisioned → throughput y latencia predecibles.
2. **Global Batch** o **Data Zone Batch** si hay requisito de residencia. **50% de descuento**, objetivo 24 h, con cuota separada que no toca tu tráfico online.
3. **At rest** se queda siempre en la geografía designada, en todos los tipos. Lo que varía es **dónde se procesa la inferencia**: global (cualquier región), data zone (US/EU/APAC) o la región del deployment.
4. **No.** Si la región primaria cae, todo el tráfico enrutado a ella se ve afectado. El enrutamiento dinámico reparte carga, no hace failover.
5. **Sin SLA, sin garantías de residencia, y vida fija de 24 h** (se borra automáticamente). Es solo para evaluar modelos fine-tuned.

</details>
