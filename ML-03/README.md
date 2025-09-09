# README – Taller 1 (ML03: Model Inversion Attack)

## Resumen Ejecutivo
Se evaluó la vulnerabilidad **Model Inversion Attack (ML03)** en el sistema Intrusion.Aware. El análisis mostró que, aunque el modelo utilizado (árbol de decisión entrenado con UNSW-NB15) es interpretable y de bajo riesgo comparado con modelos más complejos, existe la posibilidad de que un adversario reconstruya **perfiles representativos** del dataset a partir de salidas del modelo o de rutas internas. Esto podría revelar patrones de tráfico malicioso y facilitar ataques de evasión.

## Análisis de las Vulnerabilidades
- **Riesgo principal:** filtración de reglas y combinaciones de atributos que reflejan el set de entrenamiento.  
- **Contexto Intrusion.Aware:** la transparencia de los árboles permite identificar umbrales críticos, lo que reduce la complejidad del ataque pero deja expuesta la estructura del dataset.  
- **Comparación OWASP vs ATLAS:** OWASP clasifica este escenario como una vulnerabilidad estructural (ML03), mientras que MITRE ATLAS lo define como la técnica *Invert ML Model* bajo la táctica de **Exfiltration**, es decir, un método de extracción de datos sensibles.  

## Recomendaciones de Mitigación
1. **Restringir salidas del modelo:** devolver solo la etiqueta en lugar de probabilidades detalladas.  
2. **Aplicar rate limiting y monitoreo** en la API de inferencia para detectar intentos de enumeración.  
3. **Introducir ruido o discretización** en las probabilidades para evitar inferencias precisas.  
4. **Auditar patrones de consulta** y establecer autenticación para acceso a predicciones.  
5. **Evaluar técnicas de privacidad diferencial** en escenarios con datos sensibles.  

## Conclusiones y Lecciones Aprendidas
El **impacto de ML03 en Intrusion.Aware es moderado**, pero ilustra la importancia de controlar lo que expone un modelo en producción. La lección clave es que **no siempre se necesita reconstruir ejemplos exactos para comprometer un sistema**; basta con filtrar patrones estructurales del dataset para que un atacante obtenga ventaja. Incorporar buenas prácticas de salida, auditoría y privacidad es esencial para mitigar este riesgo en cualquier sistema de ML.
