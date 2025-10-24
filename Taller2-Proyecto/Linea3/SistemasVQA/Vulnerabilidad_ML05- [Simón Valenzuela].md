# Vulnerabilidad – ML05: Model Theft (Model Extraction Attack)

**Línea 3** Sistemas VQA (MedVQA-AI)
Componente foco: Multimodal Fusion + Output Layer
Autor: Simón Valenzuela

---

## 1. Resumen / Definición

Un **Model Theft Attack** o **Model Extraction Attack** ocurre cuando un atacante intenta **replicar o clonar el comportamiento del modelo original** realizando múltiples consultas a su API o endpoint de inferencia. A partir de las salidas obtenidas (predicciones, probabilidades o texto generado), el adversario **entrena un modelo sustituto** que reproduce la lógica interna del modelo víctima.
En el contexto de **MedVQA-AI**, este ataque permitiría que un agente malicioso reconstruya un modelo que imite el razonamiento clínico del sistema, obteniendo su conocimiento diagnóstico o incluso replicando las correlaciones aprendidas del dataset médico.

---

## 2. Por qué es relevante para MedVQA-AI (contexto)

MedVQA-AI combina representaciones visuales (radiografías de tórax) y lingüísticas (preguntas médicas) para generar respuestas en lenguaje natural.
Si el modelo se expone a través de una API clínica o servicio web, un atacante podría automatizar consultas masivas —variando imágenes y prompts médicos— para recolectar pares entrada-salida y entrenar un modelo sustituto. Esto permitiría **robar la propiedad intelectual del modelo**, exponer pesos entrenados en datasets sensibles y comprometer la integridad de la plataforma.

---

## 3. Equivalente / mapeo en MITRE ATLAS

**Técnica ATLAS:** *Steal Machine Learning Model* (táctica: *Exfiltration*).
Ambos marcos describen el mismo objetivo: **replicar el modelo a través de su interfaz pública** mediante consultas repetidas y análisis de respuestas. MITRE ATLAS documenta esta técnica como una forma de extraer propiedad intelectual y lograr persistencia en ataques de cadena de suministro.

---

## 4. Ambiente de pruebas (diseño, no ejecución)

* **Sandbox controlado** con instancia limitada de MedVQA-AI (entrenada con subset anónimo de Medical-CXR-VQA).
* **Interfaz API simulada:** endpoint POST `/vqa/infer` que recibe imagen + pregunta y devuelve respuesta textual.
* **Dataset de entrada:** radiografías anónimas de baja resolución + preguntas clínicas genéricas.
* **Registro de consultas:** logging de queries, timestamps, frecuencia y tokens devueltos.
* **Permisos:** autorización de pruebas éticas en sandbox institucional.

---

## 5. Componentes afectados

* **Multimodal Fusion:** sus salidas intermedias (embeddings visual-textuales) podrían ser inferencia indirecta del conocimiento entrenado.
* **Output Layer (Generative):** las respuestas de texto sirven como dataset para entrenar el modelo sustituto.
* **API Interface:** si no existen límites de consulta ni protecciones anti-scraping, facilita la exfiltración de miles de pares entrada-salida.

---

## 6. Impacto (qué puede pasar)

* **Pérdida de propiedad intelectual:** clonación del modelo diagnóstico y competencia desleal.
* **Divulgación de información entrenada:** al replicar el modelo, podrían emergir patrones o vocablos de datos médicos originales.
* **Compromiso de seguridad clínica:** un modelo robado podría ser modificado para emitir diagnósticos erróneos o no regulados.
* **Daño reputacional y legal:** exposición de propiedad intelectual y violación de normas de protección de datos (GDPR/HIPAA).

---

## 7. Casos de prueba (mínimo 3) — diseño conceptual

### Caso A — Enumeración de consultas masivas (Black-Box Query Attack)

**Objetivo:** medir si el modelo responde consistentemente a múltiples consultas ligeramente variadas, facilitando su replicación.
**Procedimiento:**

1. Definir un conjunto de imágenes base (10–20 radiografías anónimas).
2. Enviar múltiples consultas variando las preguntas (p. ej. sinónimos médicos).
3. Registrar las respuestas textuales y tokens devueltos.
4. Evaluar consistencia léxica y semántica entre respuestas.
   **Evidencia esperada:** tabla de pares (input → output), similaridad coseno > 0.85.
   **Criterio de riesgo:** si el modelo es predecible y determinista ante inputs ligeramente distintos, es fácilmente replicable.

---

### Caso B — Entrenamiento de modelo sustituto a partir de outputs capturados

**Objetivo:** evaluar la facilidad de entrenar un modelo proxy con pares entrada-salida obtenidos.
**Procedimiento:**

1. Recolectar 1000 consultas controladas (en sandbox).
2. Preprocesar las respuestas como etiquetas textuales de diagnóstico.
3. Entrenar un modelo simple (LSTM o Transformer pequeño) para replicar las respuestas.
4. Comparar las salidas del modelo proxy con el original en 50 casos nuevos.
   **Evidencia esperada:** exactitud ≥ 80 % en respuestas semánticamente similares.
   **Criterio de riesgo:** alto si el modelo sustituto reproduce las respuestas con poca data.

---

### Caso C — Inyección de consultas dirigidas para extraer tokens de atención

**Objetivo:** detectar si la API devuelve metadata (o logits) que puedan revelar parámetros internos.
**Procedimiento:**

1. Enviar consultas válidas pero con flag de debug (“show-attention=true”).
2. Analizar la respuesta JSON para detectar vectores de atención o scores internos.
3. Comparar los vectores entre preguntas similares para inferir representaciones del modelo.
   **Evidencia esperada:** extracción de arrays numéricos correlacionados entre consultas.
   **Criterio de riesgo:** grave si los vectores se mantienen estables y repetibles por clase.

---

## 8. Herramientas / técnicas recomendadas (para testing conceptual)

* **Adversarial Robustness Toolbox (ART)** – módulo de model extraction.
* **TextAttack** – para generar variaciones semánticas de preguntas médicas.
* **Scikit-learn / Transformers** – para entrenar modelo proxy controlado.
* **Burp Suite / OWASP ZAP** – monitoreo de tráfico y detección de fugas de tokens.
* **Pandas + Matplotlib** – registro y visualización de correlaciones input-output.

---

## 9. Mitigaciones propuestas (en prioridad)

1. **Autenticación fuerte y control de API keys** para limitar acceso a la inferencias.
2. **Rate limiting y monitorización de patrones de consulta** (p. ej. > X requests/min).
3. **Respuestas no deterministas / ruido controlado** en outputs para dificultar clonación.
4. **Privacidad diferencial** en el entrenamiento para proteger conocimiento sensible.
5. **No exponer scores intermedios ni logits** en la API pública.
6. **Monitoreo de uso anómalo:** alertas por consultas automatizadas o con poca diversidad visual.
7. **Evaluación de riesgo de extraction attack** antes del despliegue.

---

## 10. Métricas de evaluación (para el informe)

* **Tasa de reproducción del modelo proxy (%)** respecto al original.
* **Número de consultas necesarias** para entrenar modelo sustituto (eficiencia del ataque).
* **Exactitud semántica entre respuestas (model vs proxy)** medida con BLEU o cosine similarity.
* **Volumen de data exfiltrada** (logs y tamaño del dataset derivado).
* **Severidad = Impacto × Probabilidad.**

---

## 11. Consideraciones éticas y regulatorias

* Solo diseñar y documentar los ataques de forma controlada sin ejecutar en producción.
* Asegurar el uso de datasets anónimos y autorizados.
* Respetar la confidencialidad de información médica y las normas institucionales.
* Relatar hallazgos al equipo de seguridad y cumplimiento para implementar controles.

---

## Imagen explicativa simple

**Diagrama:**
<img width="780" height="648" alt="Proceso de Extracción de Modelo — Vulnerabilidad ML05 (MedVQA-AI)" src="https://raw.githubusercontent.com/rttbot/TICS0866-TALLER1-OWASPML/main/Taller2-Proyecto/Linea3-SistemasVQA/image.png" />

