# Análisis de Vulnerabilidades – OWASP vs MITRE ATLAS
**Vulnerabilidad asignada:** ML03 – Model Inversion Attack  
**Autor:** [julian contreras] 

---

## 1. OWASP ML03: Model Inversion Attack
Según OWASP ML Top 10, un ataque de inversión de modelo ocurre cuando un adversario utiliza las salidas de un modelo de ML (probabilidades, scores o etiquetas) para **inferir y hasta reconstruir datos sensibles del conjunto de entrenamiento**.  
- **Ejemplo:** en reconocimiento facial, un atacante puede aproximar imágenes de rostros usados en el entrenamiento.  
- **En Intrusion.Aware (árboles de decisión sobre UNSW-NB15):** el riesgo es que se filtren **rutas y umbrales** que caracterizan patrones de tráfico del dataset. Aunque no se recuperen ejemplos exactos, se pueden deducir **“vectores arquetipo”** que describen perfiles representativos de tráfico malicioso o normal.

---

## 2. Técnica asociada en MITRE ATLAS
En la matriz ATLAS, la técnica relacionada es **Invert ML Model** (táctica: *Exfiltration*).  
- **Objetivo:** reconstruir o estimar información del dataset de entrenamiento explotando la salida del modelo.  
- **Mecanismo:** consultas repetidas a la API con variaciones de entrada para extraer reglas, probabilidades o regiones densas en el espacio de entrenamiento.

---

## 3. Similitudes y diferencias OWASP ↔ ATLAS
- **Similitudes:**  
  - Ambos describen la **extracción de conocimiento del dataset** mediante acceso a salidas del modelo.  
  - El atacante no busca robar el modelo completo, sino **obtener datos o patrones sensibles**.  

- **Diferencias:**  
  - OWASP clasifica esto como una **vulnerabilidad del modelo ML** (ML03).  
  - ATLAS lo cataloga como una **técnica adversaria** dentro de la táctica de *Exfiltration*, es decir, un paso concreto en el ciclo de ataque.  

---

## 4. Aplicación a Intrusion.Aware
- Los **árboles de decisión** hacen más fácil mapear rutas y reglas, pero el impacto es **menor** que en modelos black-box complejos.  
- El riesgo real es la **exposición de perfiles de tráfico** usados en el entrenamiento, que podría ayudar a un atacante a **evadir la detección** o a entender mejor el modelo.  

---

## 5. Mitigaciones recomendadas
- Limitar lo que devuelve el modelo (por ejemplo, solo etiquetas en vez de scores).  
- Implementar **rate-limiting y monitoreo** de consultas para detectar barridos.  
- Usar **regularización o privacidad diferencial** para reducir memorization.  
- Control de acceso a la API de inferencia y auditoría de logs.  

---

## 6. Conclusión
OWASP ML03 y la técnica de ATLAS *Invert ML Model* describen la misma amenaza desde marcos distintos: OWASP como **vulnerabilidad estructural** en sistemas ML, y ATLAS como **técnica concreta de exfiltración** en un ataque. En el caso de Intrusion.Aware, aunque el impacto sea moderado por la naturaleza interpretable del modelo, sigue existiendo el riesgo de fuga de información del dataset, lo que justifica aplicar controles de salida y monitoreo continuo.
