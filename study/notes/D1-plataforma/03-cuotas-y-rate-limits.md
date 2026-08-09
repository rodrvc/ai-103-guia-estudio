# Cuotas, TPM/PTU y rate limits

> Docs de Azure — [Quotas and limits](https://learn.microsoft.com/en-us/azure/ai-foundry/openai/quotas-limits) · actualizado 2026-07-29
> Cubre: **D1.3.a** *Manage costs, quotas and rate limits*
> Peso: **alto** — objetivo directo de D1 (25–30%) y aparece disfrazado en preguntas de arquitectura
> ⚠️ El curso oficial de Learn **no cubre** este tema. Fuente: documentación de Azure.
> Fecha: 2026-08-09

---

## En cristiano

Tu deployment no puede procesar infinito. Azure te pone **dos contadores en paralelo** y basta con reventar uno para que te corte.

La analogía: **una autopista de peaje.**

| | En la autopista | En Azure |
| --- | --- | --- |
| **RPM** | Cuántos **coches** pasan por minuto | *Requests Per Minute* — cuántas llamadas |
| **TPM** | Cuánta **carga total** transportan | *Tokens Per Minute* — cuánto texto procesas |

Puedes quedarte fuera por mandar **muchas llamadas pequeñas** (revientas RPM) o **pocas llamadas enormes** (revientas TPM). Se miden por separado.

---

## TPM vs PTU — la decisión que cae en el examen

Esto es lo que de verdad preguntan: **qué modelo de capacidad eliges.**

| | **TPM** (pay-as-you-go) | **PTU** (Provisioned Throughput Units) |
| --- | --- | --- |
| **Qué compras** | Un límite de consumo por minuto | **Capacidad dedicada y reservada** |
| **Analogía** | Taxi: pagas por trayecto | Coche propio con chofer: pagas esté o no en marcha |
| **Pagas** | Por token consumido | Por la capacidad, la uses o no |
| **Latencia** | Variable; empeora en horas punta | **Predecible y garantizada** |
| **Riesgo de 429** | Sí, si te pasas | Prácticamente no: la capacidad es tuya |
| **Para quién** | Desarrollo, tráfico bajo o irregular | **Producción crítica**, alto volumen sostenido |

**Regla de decisión:** ¿el enunciado dice *"latencia predecible"*, *"misión crítica"* o *"volumen alto sostenido"*? → **PTU**. ¿Dice *"empezar barato"*, *"tráfico variable"* o *"prototipo"*? → **standard con TPM**.

> Esta es la misma decisión del apunte [01 Deployment types](01-deployment-types.md): standard = TPM, provisioned = PTU. Aquí se ve el lado del consumo; allí, el del despliegue.

---

## El error 429 — cómo se maneja

Cuando te pasas del límite recibes **HTTP 429 (Too Many Requests)**.

**La respuesta trae la cabecera `Retry-After`** con los segundos que debes esperar. **Respétala.**

### Qué se espera que hagas

| Práctica | Por qué |
| --- | --- |
| **Retry con backoff exponencial** | Reintentar de inmediato empeora la congestión. Espera 1s, 2s, 4s, 8s… |
| **Respetar `Retry-After`** | Azure te está diciendo exactamente cuánto esperar |
| **Subir la carga gradualmente** | Los picos bruscos disparan 429 aunque la media esté bajo el límite |
| **Mover cuota entre deployments** | La cuota es un pool: puedes reasignarla sin pedir más |
| **Pasar a PTU** | Si el 429 es crónico, el problema es de modelo de capacidad, no de reintentos |

> ⚠️ **Confusión frecuente:** *"tengo 429 pero mis métricas de tokens están por debajo de la cuota"*. Es normal: el límite también se evalúa en **ventanas cortas** (algunos modelos miden por 10 segundos, no por minuto). Un pico de un segundo puede dispararlo con la media baja.

```python
# El patrón que se espera saber describir
import time
from openai import RateLimitError

for intento in range(5):
    try:
        return client.responses.create(model="mi-deployment", input=prompt)
    except RateLimitError as e:
        espera = 2 ** intento          # backoff exponencial: 1, 2, 4, 8, 16
        time.sleep(espera)
raise RuntimeError("agotados los reintentos")
```

---

## Dónde vive la cuota — el alcance

Esto cambió y es importante:

| Nivel | ¿Se aplica cuota? |
| --- | --- |
| **Tenant** | ❌ No |
| **Suscripción** | ✅ **Sí — es el nivel más alto** |
| Región / recurso | Antes sí; **desde mayo 2026 se consolida en la suscripción** |

**Consecuencia:** todos los recursos y regiones de una suscripción **comparten el mismo pool**. Dos deployments del mismo modelo en regiones distintas ya no tienen cuotas separadas.

- **Global Standard**: un pool para todas las regiones de la suscripción
- **Data Zone Standard**: un pool **por zona de datos** (US, EU…)

### Quota tiers

Existen **7 niveles**: Free Tier y Tiers 1 a 6. Suben **automáticamente** con tu consumo — antes había que pedirlo a mano.

También puedes **pedir más cuota** con un formulario sin cambiar de tier. Y puedes **desactivar el auto-upgrade** (`NoAutoUpgrade`) si usas la cuota como tope de gasto — aunque Azure dice que esa no es la práctica recomendada para controlar costes.

---

## Límites concretos que conviene reconocer

No memorices la tabla entera. Estos son los que se repiten:

| Límite | Valor |
| --- | --- |
| Recursos de Azure OpenAI por suscripción | **30** |
| Deployments standard por recurso | **32** |
| Deployments de modelos fine-tuned | **10** |
| PTU máximas por deployment | 100.000 |
| Tools por llamada a `/chat/completions` | **128** |
| Inputs en un array de `/embeddings` | 2.048 |

> Los valores de RPM/TPM por modelo **cambian constantemente** y dependen del tier. No los memorices: memoriza **que existen dos contadores y que la cuota vive en la suscripción**.

---

## Controlar el coste

| Palanca | Cómo |
| --- | --- |
| **Elegir modelo más pequeño** | Un `mini` o `nano` cuesta una fracción y suele bastar |
| **Limitar `max_tokens`** | Pones techo a lo que puede costar cada respuesta |
| **Batch** | Procesamiento por lotes, más barato, sin garantía de inmediatez |
| **Caché de prompts** | Reutiliza la parte fija del prompt |
| **Rate limiting propio** | Evita que un bug o un abuso dispare la factura |

---

## Para el examen

**Alto valor:**
- **TPM vs PTU** y cuándo elegir cada uno (latencia predecible / misión crítica → **PTU**)
- **429 → backoff exponencial + respetar `Retry-After`**
- **La cuota se aplica a nivel de suscripción**, no de tenant ni (ya) de región
- **RPM y TPM son dos contadores independientes**

**Valor medio:** quota tiers y auto-upgrade · pools compartidos por Global Standard vs Data Zone · mover cuota entre deployments · límites de 30 recursos / 32 deployments.

**Bajo valor:** los números exactos de RPM/TPM por modelo y tier — cambian cada pocos meses.

---

## Comprueba que lo tienes

1. Tu app de producción atiende picos de tráfico y el equipo se queja de que la latencia varía mucho entre las 9 y las 11 de la mañana. ¿Qué cambias?
2. Recibes 429 en ráfagas, pero el panel dice que consumes el 60% de tu cuota de tokens. ¿Cómo se explica?
3. Tienes dos deployments del mismo modelo, uno en East US y otro en West Europe, en la misma suscripción. ¿Tienen cuotas separadas?
4. Escribe (o describe) qué debe hacer tu código al recibir un 429.
5. Un proceso nocturno procesa 200.000 documentos y no importa que tarde horas. ¿Qué configuración de capacidad eliges y por qué **no** PTU?

<details>
<summary>Respuestas</summary>

1. **Pasar a PTU** (provisioned). Compras capacidad dedicada, así que la latencia es predecible aunque haya picos. Con standard/TPM compites por capacidad compartida.
2. Porque el límite también se evalúa en **ventanas cortas** (algunos modelos por cada 10 segundos) y porque **RPM y TPM son contadores separados**: puedes estar reventando el de peticiones con el de tokens bajo. Un pico de un segundo no se ve en la media.
3. **No.** Desde mayo de 2026 la cuota se consolida a nivel de **suscripción**: en Global Standard comparten un único pool para todas las regiones.
4. **Reintentar con backoff exponencial** (1s, 2s, 4s, 8s…) **respetando la cabecera `Retry-After`**. Nunca reintentar de inmediato ni en bucle cerrado.
5. **Batch.** Es el más barato y la latencia no importa. **PTU sería tirar el dinero**: pagas capacidad dedicada reservada 24/7 para un proceso que corre unas horas de noche.

</details>
