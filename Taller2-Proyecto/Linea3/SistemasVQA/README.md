# Auditoría de Seguridad - Línea 3: Sistemas de Pregunta-Respuesta (VQA)

## Descripción de la Línea Asignada

La **Línea 3** se centra en auditar la seguridad de la **capa de interacción multimodal** (Visión + Lenguaje) y la **interfaz de inferencia (API)** del sistema MedVQA-AI.

A diferencia de otras líneas que se enfocan en la manipulación de la entrada visual, nuestro objetivo es evaluar los riesgos asociados a la **exposición del modelo** como un servicio. El análisis se enfoca en cómo un atacante puede explotar el *endpoint* de VQA para comprometer:
* **La Propiedad Intelectual** (Robo del modelo).
* **La Confidencialidad** (Fuga de datos de entrenamiento).
* **La Disponibilidad** (Denegación de servicio clínico).

---

## Componente de la arquitectura relevantes para la linea

El foco de esta auditoría son los componentes que gestionan la inferencia y la respuesta al usuario:

* **Language Branch (LLM) y API Interface:** Es el principal vector de ataque para la sobrecarga de recursos (DoS) y el punto de recolección de datos para los ataques de exfiltración.
* **Multimodal Fusion:** El componente clave donde se combina la lógica visual y textual. Su comportamiento es el objetivo a replicar en un ataque de *Model Theft* (ML05).
* **Output Layer (Generative):** La fuente de las respuestas (texto y probabilidades) que el atacante utiliza para inferir datos de entrenamiento (ML03) o entrenar modelos sustitutos (ML05).

---

## Amenazas y Vulnerabilidades Analizadas

Nuestro equipo diseñó casos de prueba para las siguientes tres amenazas críticas, adaptadas al contexto VQA de MedVQA-AI:

1.  **ML05 - Model Theft (Robo de Modelo):**
    * **Riesgo:** Clonación de la lógica de razonamiento clínico del sistema a través de consultas masivas a la API para entrenar un modelo sustituto.
    * **Impacto:** Pérdida de propiedad intelectual.

2.  **ML03 - Model Inversion (Inversión de Modelo):**
    * **Riesgo:** Reconstrucción de información sensible o arquetipos de imágenes del *dataset* de entrenamiento (potencialmente PHI) explotando las salidas y *scores* del modelo.
    * **Impacto:** Fuga de datos confidenciales.

3.  **LLM04 - Denial of Service (DoS):**
    * **Riesgo:** Saturación del *Language Branch* (LLM) mediante *prompts* excesivamente largos o consultas concurrentes masivas.
    * **Impacto:** Paralización del servicio de diagnóstico, afectando la disponibilidad clínica.

---

## Enfoque de la Auditoría (Diseño de Pruebas)

Nuestra metodología se centró en el diseño conceptual de pruebas de *testing* en la capa de inferencia (API), simulando un atacante *Black-Box* o *Gray-Box*:

* **Pruebas de Exfiltración (ML03, ML05):** Diseño de estrategias de consulta (Query-based attacks) para medir la facilidad de replicación del modelo (Robo) y la fuga de información estadística (Inversión).
* **Pruebas de Disponibilidad (LLM04):** Diseño de pruebas de carga y estrés (usando Locust/JMeter conceptualmente) para medir el impacto de *prompts* largos y alta concurrencia en la latencia de la API.
* **Análisis de Impacto:** Evaluación del riesgo clínico (fuga de PHI, indisponibilidad del servicio) y mapeo de amenazas con **MITRE ATLAS**.
