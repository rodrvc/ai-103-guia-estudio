# IA responsable: mapear, medir, mitigar y gestionar

> LP1 · Módulo 6 — [Implement a responsible generative AI solution in Microsoft Foundry](https://learn.microsoft.com/en-us/training/modules/responsible-ai-studio/) (9 unidades)
> Cubre: **D1.4.a**, **D1.4.b** · Cierra material para **E-004** (content filters)
> Peso: **alto** — D1 pesa 25–30% y es tu dominio **Débil**
> Fecha: 2026-08-08

---

## En cristiano

Microsoft no te pide "sé bueno". Te da un **proceso de 4 pasos**, en orden, y el examen pregunta por ese orden.

> **Map → Measure → Mitigate → Manage**

| Paso | En cristiano | Pregunta que responde |
| --- | --- | --- |
| **1. Map** | ¿Qué puede salir mal? | Listar y priorizar los daños posibles |
| **2. Measure** | ¿Cuánto sale mal hoy? | Medir la línea base |
| **3. Mitigate** | ¿Cómo lo reduzco? | Aplicar defensas en 4 capas |
| **4. Manage** | ¿Cómo lo saco a producción sin sustos? | Plan de despliegue y operación |

*Regla mnemotécnica:* las **4 M**. Y **no puedes mitigar lo que no has medido** — por eso Measure va antes que Mitigate.

> Estas 4 etapas se corresponden con las funciones del **NIST AI Risk Management Framework**. El examen puede nombrarlo.

---

## 1 · MAP — mapear los daños

Cuatro pasos, **en este orden**:

| # | Paso | Qué haces |
| --- | --- | --- |
| 1 | **Identify** | Listar daños posibles |
| 2 | **Prioritize** | Ordenar por **probabilidad × impacto** |
| 3 | **Test and verify** | Comprobar que ocurren de verdad → **red teaming** |
| 4 | **Document and share** | Documentar y comunicar a los stakeholders |

### Tipos comunes de daño

- Contenido ofensivo, peyorativo o discriminatorio
- **Inexactitudes factuales** (alucinaciones)
- Contenido que fomenta conductas ilegales o poco éticas

### Priorizar: probabilidad × impacto

El ejemplo del curso — un copiloto de cocina:

| Daño posible | Impacto | Probabilidad |
| --- | --- | --- |
| Da tiempos de cocción malos → comida cruda → enfermedad | Medio | **Alta** |
| Da la receta de un veneno letal con ingredientes caseros | **Muy alto** | Baja |

**No hay respuesta única.** La priorización es **subjetiva** y la decide el equipo, a veces con legal o de políticas. El examen puede preguntar precisamente eso: que es una decisión de equipo, no una fórmula.

### Red teaming

**Un equipo intenta romper tu solución a propósito** para que genere contenido dañino.

Viene de la ciberseguridad; aquí se extiende a buscar *daños*, no solo vulnerabilidades. Sus éxitos se documentan y sirven para estimar la probabilidad real.

### Dónde mirar las limitaciones conocidas

- **Transparency note** del servicio (Azure OpenAI tiene la suya)
- **System card** del modelo (p. ej. la de GPT-4 de OpenAI)
- **Responsible AI Impact Assessment Guide** y su plantilla

---

## 2 · MEASURE — medir

**Objetivo: una línea base (*baseline*)** que cuantifique cuánto daño produce hoy tu solución. Luego mides las mejoras contra ella.

Tres pasos:

1. **Preparar prompts diversos** que probablemente provoquen cada daño documentado
2. **Enviarlos** y recoger las salidas
3. **Evaluar con criterios predefinidos** y clasificar el nivel de daño

La clasificación puede ser tan simple como *harmful* / *not harmful*, o una escala. Lo importante son **criterios estrictos y definidos de antemano**.

### Manual vs automático — el orden importa

| Fase | Cómo | Por qué |
| --- | --- | --- |
| **Primero** | **Manual**, con pocos casos | Verificar que tus criterios son consistentes y están bien definidos |
| **Después** | **Automático**, gran volumen | Escalar. Puede usar un **modelo clasificador** |
| **Siempre** | Manual periódico | Validar escenarios nuevos y que el automático sigue funcionando |

⚠️ **Empiezas manual, no automático.** Si automatizas con criterios mal definidos, escalas el error.

---

## 3 · MITIGATE — las 4 capas ⭐

**Lo más examinable del módulo.** Defensa en profundidad: cada capa atrapa lo que la anterior deja pasar.

| # | Capa | Qué haces ahí |
| --- | --- | --- |
| 1 | **Model** | Elegir el modelo adecuado · **fine-tuning** |
| 2 | **Safety system** | **Content filters** · **prompt shields** |
| 3 | **System message and grounding** | System message · prompt engineering · **RAG** |
| 4 | **User experience** | UI que restringe entradas · validación · **transparencia en la documentación** |

### Capa 1 — Model

- **Elegir el modelo apropiado.** Si solo clasificas textos cortos, un modelo simple da lo mismo con **menos riesgo** que GPT-4. Más potente ≠ mejor.
- **Fine-tuning** con tus datos, para que las respuestas queden acotadas a tu escenario.

### Capa 2 — Safety system

Configuración de plataforma. En Foundry son los **guardrails**:

**Content filters** — clasifican en **4 niveles de severidad** × **5 categorías**:

| Severidad | Categorías |
| --- | --- |
| `safe` · `low` · `medium` · `high` | **hate and fairness** · **sexual** · **violence** · **self-harm** · **task-adherence** |

> ⚠️ **Ojo, esto cambió.** Antes eran 4 categorías. Este módulo (actualizado 2026-04-28) añade **task-adherence** — el modelo se sale de la tarea que le encargaste. **Son 5.**

**Prompt shields** — algoritmos de detección de abuso: detectan si alguien intenta **subvertir el system prompt** de forma sistemática. Es la defensa contra **prompt injection**.

### Capa 3 — System message and grounding

Todo lo que va en el prompt:

- **System message**: define los parámetros de comportamiento del modelo
- **Prompt engineering**: añadir datos de contexto
- **RAG**: recuperar datos de **fuentes de confianza** e incluirlos en el prompt ← tu apunte [06](../D2-apps-y-agentes/06-rag-grounding.md)

### Capa 4 — User experience

- **Restringir la UI** a temas o tipos de entrada concretos
- **Validar** entrada y salida
- **Transparencia**: documentar capacidades, limitaciones y **los daños que las mitigaciones NO cubren**

---

## 4 · MANAGE — desplegar y operar

### Antes de publicar: revisiones de cumplimiento

Cuatro, memorízalas: **Legal · Privacy · Security · Accessibility**.

### El plan de salida

| Elemento | Qué es |
| --- | --- |
| **Phased delivery plan** | Publicar primero a un grupo **restringido**, recoger feedback, luego ampliar |
| **Incident response plan** | Qué hacer ante un incidente, **con tiempos estimados** |
| **Rollback plan** | Cómo volver al estado anterior |
| **Bloqueo de respuestas** | Poder bloquear **al instante** una respuesta dañina detectada |
| **Bloqueo de abusadores** | Poder bloquear usuarios, aplicaciones o **IPs** |
| **Canal de feedback** | Que el usuario reporte contenido *inaccurate, incomplete, harmful, offensive* |
| **Telemetría** | Satisfacción y huecos funcionales — **cumpliendo las leyes de privacidad** |

---

## Para el examen

**Alto valor:**
- **Map → Measure → Mitigate → Manage**, en ese orden
- **Las 4 capas de mitigación** y qué vive en cada una
- **Content filters: 4 severidades × 5 categorías** (`safe/low/medium/high`) ← **E-004**
- **Prompt shields** = defensa contra prompt injection / subvertir el system prompt
- **Red teaming** = probar deliberadamente a romperlo, en la etapa **Map**
- **Manual primero, automático después**
- Los 4 pasos de Map: identify → prioritize → test → document

**Valor medio:** las 4 revisiones (legal/privacy/security/accessibility) · phased delivery, incident response, rollback · priorizar = probabilidad × impacto · NIST AI RMF.

**Bajo valor:** el ejemplo del copiloto de cocina · las URLs de las plantillas de impact assessment.

---

## Comprueba que lo tienes

1. Tu equipo ya listó y priorizó los daños posibles. ¿Cuál es el siguiente paso del proceso, y por qué no puedes saltar directamente a poner filtros?
2. Quieres impedir que un usuario manipule tu agente con instrucciones ocultas para que ignore su system prompt. ¿Qué mitigación aplicas y **en qué capa** está?
3. ¿Cuántas categorías y cuántos niveles de severidad tienen los content filters, y cómo se llaman los niveles?
4. Tu solución solo clasifica textos de dos líneas en tres etiquetas. ¿Por qué podría ser mejor un modelo pequeño que GPT-4, en términos de IA responsable?
5. Vas a medir cuánto daño genera tu app. ¿Empiezas con pruebas manuales o automatizadas? ¿Por qué?
6. Nombra las cuatro capas de mitigación en orden, de la más profunda a la más cercana al usuario.
7. Vas a publicar mañana. Nombra tres cosas que tu plan de salida debe incluir.

<details>
<summary>Respuestas</summary>

1. **Measure**: establecer una **línea base** que cuantifique los daños. Sin baseline no puedes demostrar que la mitigación funcionó — no sabrías contra qué comparar.
2. **Prompt shields**, en la capa **Safety system**. Detectan intentos sistemáticos de subvertir el system prompt.
3. **5 categorías** (hate and fairness, sexual, violence, self-harm, **task-adherence**) × **4 severidades**: `safe`, `low`, `medium`, `high`.
4. Porque un modelo más simple cumple igual la función **con menor riesgo de generar contenido dañino**. Mitigación de la capa **Model**: elegir el modelo apropiado al uso, no el más potente.
5. **Manual primero**, con pocos casos, para verificar que los criterios de evaluación son consistentes y están bien definidos. Después automatizas a volumen. Y aun así, sigues haciendo manual periódico.
6. **Model** → **Safety system** → **System message and grounding** → **User experience**.
7. Tres de: phased delivery plan · incident response plan (con tiempos) · rollback plan · bloqueo inmediato de respuestas dañinas · bloqueo de usuarios/apps/IPs · canal de feedback · telemetría respetando privacidad.

</details>
