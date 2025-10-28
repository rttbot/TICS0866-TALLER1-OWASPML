# 💣 Vulnerabilidad – LLM04: Denial of Service (DoS)

**Línea 3: Sistemas VQA (MedVQA-AI)**
**Componente foco:** Language Branch + API Interface

***

## 1. Resumen / Definición

Un ataque **LLM04 - Denial of Service (DoS)** consume una cantidad excesiva de recursos de cómputo (tiempo de GPU/TPU, memoria) al sobrecargar el **Language Branch (LLM)** de **MedVQA-AI** con entradas que son costosas de procesar. El objetivo es paralizar o ralentizar el servicio de inferencia de la API, impidiendo que usuarios legítimos (radiólogos) reciban respuestas diagnósticas oportunas. Los ataques típicos incluyen el envío de *prompts* excesivamente largos o forzar la generación de respuestas extensas.

## 2. Por qué es relevante para MedVQA-AI (Contexto)

En el ámbito clínico, la disponibilidad y la baja latencia del sistema VQA son críticas para la asistencia diagnóstica. La **Language Branch** y el **Generative Model** son los componentes con mayor consumo de recursos. Un DoS exitoso, ya sea por una sola consulta muy larga o por un alto volumen de consultas concurrentes, puede paralizar el servicio de MedVQA-AI.

## 3. Equivalente / mapeo en MITRE ATLAS

* **Táctica ATLAS:** **Inhibit Response or Capability** (Inhibir Respuesta o Capacidad).
* **Técnica ATLAS:** **Resource Exhaustion** (T1230).

**Justificación:** El LLM04 y la técnica ATLAS de Explotación de Recursos buscan específicamente agotar los recursos de cómputo (GPU/TPU) para interrumpir la disponibilidad del servicio de inferencia de la IA.

***

## 4. Ambiente de Pruebas (Diseño, No Ejecución)

* **Sandbox Controlado:** Instancia de MedVQA-AI con recursos limitados para simular un cuello de botella.
* **Interfaz API simulada:** Endpoint POST `/vqa/infer`.
* **Métricas Clave:** La herramienta de prueba debe poder medir la **latencia** (tiempo de procesamiento), el **consumo de GPU/TPU** y el **número de *tokens* generados**.
* **Herramientas:** **Locust** o **JMeter** para simulación de carga y concurrencia.

## 5. Componentes Afectados

* **Language Branch (LLM):** Consumidor principal de recursos (GPU/TPU) al realizar la inferencia y la generación de texto.
* **API Interface:** Punto de entrada que debe gestionar la cola de solicitudes y aplicar límites de tasa o longitud de *prompt*.
* **Generative Model (Output Layer):** Puede ser forzado a generar respuestas y reportes médicos excesivamente largos, agotando el tiempo de cómputo.

## 6. Impacto (Qué puede pasar)

* **Riesgo Clínico Alto:** **Denegación de Disponibilidad** del servicio. Imposibilidad de recibir diagnósticos o reportes en tiempo real, lo que retrasa la toma de decisiones críticas para el paciente.
* **Pérdida Económica:** Altos costos operativos debido al consumo excesivo de GPU/TPU por parte de las consultas maliciosas.
* **Degradación del Servicio:** El tiempo de respuesta aumenta a niveles inaceptables (alta latencia), volviendo el sistema inoperativo.

***

## 7. Casos de Prueba (Mínimo 3) — Diseño Conceptual

| Caso | Objetivo | Procedimiento (Resumido) | Evidencia Esperada | Criterio de Riesgo |
| :--- | :--- | :--- | :--- | :--- |
| **A. Ataque de Inferencia Larga (Prompt Length)** | Determinar si el LLM colapsa al procesar *prompts* de longitud excesiva. | 1. **Baseline:** Medir la latencia con un *prompt* estándar (100 *tokens*). 2. **Ataque:** Crear un *prompt* sintético de **10,000 *tokens*** (texto médico repetido + pregunta). 3. **Consultar:** Enviar la imagen y el *prompt* largo. 4. **Analizar:** Registrar la latencia y el consumo de memoria. | La latencia de respuesta aumenta exponencialmente (p. ej., de 1s a 30s+), o la solicitud falla por *timeout*. | **Alto:** Si la latencia se vuelve inaceptable para el uso clínico. |
| **B. Ataque de Generación Larga (Forzar Output)** | Determinar si el LLM puede ser forzado a generar un reporte excesivamente extenso. | 1. **Ataque:** Utilizar un *prompt* diseñado para forzar la máxima generación de *tokens* (p. ej., "¿Escribe un reporte clínico detallado de 500 párrafos sobre todos los posibles diagnósticos..."). 2. **Consultar:** Enviar una imagen y el *prompt* de generación larga. 3. **Analizar:** Registrar el número de *tokens* generados y el tiempo de respuesta. | El sistema genera una respuesta que supera el límite de longitud de *tokens* establecido (p. ej., 4096 *tokens*), o el tiempo de respuesta se dispara. | **Crítico:** Si la generación de una única respuesta consume todos los recursos de la GPU/TPU. |
| **C. Ataque Concurrente (Saturación de API)** | Simular múltiples ataques simultáneos para saturar el *pipeline* de inferencia. | 1. **Baseline:** Medir la capacidad de la API para manejar 10 consultas legítimas/s. 2. **Ataque:** Utilizar **Locust** para inyectar 100 consultas por segundo, cada una con un *prompt* de longitud media-alta. 3. **Analizar:** Registrar el *throughput* y la tasa de error. | El *throughput* cae drásticamente y la tasa de error (errores 5xx) de las solicitudes legítimas aumenta por encima del 5% debido a la saturación. | **Grave:** Demuestra que la infraestructura es vulnerable a una Denegación de Servicio distribuida (DDoS) a nivel de aplicación (Capa 7). |

***

## 8. Herramientas / Técnicas Recomendadas

* **Generación de Carga:** **Locust** o **JMeter** para simular la concurrencia y la alta tasa de solicitudes.
* **Análisis:** Scripts en **Python** para crear *prompts* largos automáticamente y registrar las métricas de latencia.
* **Monitoreo:** Herramientas de monitoreo de infraestructura (**Prometheus/Grafana**) para observar el **uso de GPU/TPU** y la **saturación de memoria** durante el ataque.

## 9. Mitigaciones Propuestas (En Prioridad)

1.  **Rate Limiting y Throttling de API:** Implementar límites estrictos en el **número de solicitudes por unidad de tiempo** y por usuario/clave de API.
2.  **Límites de Longitud de *Token*:** Implementar límites de longitud fijos tanto para la **entrada (*prompt*)** como para la **salida (*generation length*)** en el LLM.
3.  **Filtrado de *Prompts* Repetitivos:** Detectar y descartar *prompts* que contengan patrones de repetición excesiva (*padding* malicioso).
4.  **Aprovisionamiento de Recursos (Autoscaling):** Utilizar *autoscaling* basado en la longitud de la cola de inferencia para intentar absorber picos legítimos, sin dejar de aplicar *rate limiting* a patrones maliciosos.

## 10. Métricas de Evaluación (Para el Informe)

* **Latencia Promedio:** Tiempo de respuesta del sistema bajo carga (debe ser estable y bajo).
* **Número Máximo de *Tokens* procesables:** El límite de *tokens* donde el sistema se vuelve inaceptablemente lento.
* **Tasa de Error (5xx):** Porcentaje de solicitudes fallidas durante el ataque concurrente.

## 11. Consideraciones Éticas y Regulatorias

* **Solo diseñar y documentar las pruebas** en un sandbox aislado y con autorización escrita. La ejecución de ataques DoS es ilegal sin consentimiento.
* El objetivo es proteger la **disponibilidad** del servicio clínico, que es un requisito de seguridad fundamental en entornos de salud (HIPAA/normativa local).

**Diagrama de un Ataque DoS contra la API de MedVQA-AI**
![Diagrama de un ataque de Denegación de Servicio (DoS) contra el LLM de MedVQA-AI.](grafico3.png)
