# Informe de Vulnerabilidad: ML-04 / AML.T0024

Tras la evaluación realizada, se determinó que la aplicación de monitoreo de tráfico es vulnerable a ataques de inferencia de pertenencia (Membership Inference Attacks).

## OWASP ML-04: Membership Inference Attack
Según OWASP, este ataque ocurre cuando un atacante logra descubrir si un dato específico formó parte del conjunto de entrenamiento de un modelo de Machine Learning.  
El atacante envía múltiples consultas al modelo con distintos datos de prueba, explotando que el modelo suele comportarse de manera diferente frente a datos nuevos en comparación con los que ya conoce.  
Al analizar los niveles de confianza de las predicciones, es posible inferir si un dato proviene del conjunto de entrenamiento original, lo que permite identificar usuarios y/o exponer información sensible.  
Fuente: [OWASP ML-04](https://owasp.org/www-project-machine-learning-security-top-10/docs/ML04_2023-Membership_Inference_Attack.html)

## MITRE ATLAS AML.T0024: Exfiltration via AI Inference API
De manera equivalente, MITRE ATLAS clasifica esta amenaza bajo la técnica **AML.T0024 – Exfiltration via AI inference API**.  
Este ataque ocurre cuando un adversario utiliza la API de inferencia de un modelo para determinar si un dato específico formó parte del entrenamiento.  
El modelo suele responder con mayor confianza ante datos previamente vistos, lo que permite inferir información privada del dataset original y exponer datos sensibles o identificar usuarios.  
Fuente: [MITRE ATLAS AML.T0024](https://atlas.mitre.org/techniques/AML.T0024)

## Justificación
Ambas clasificaciones —**OWASP ML-04** y **MITRE ATLAS AML.T0024**— describen la misma vulnerabilidad fundamental:  
la capacidad de un atacante de inferir si un dato perteneció al conjunto de entrenamiento de un modelo de ML.  

La diferencia está en la nomenclatura, pero en esencia ambos señalan que el modelo revela, a través de sus niveles de confianza en las predicciones, información que debería permanecer privada.  
Esto representa un riesgo conocido y documentado en la seguridad de la IA, compromete la privacidad de los datos y puede ser el punto de partida para ataques más avanzados que conduzcan a la reconstrucción de información sensible.

## Conclusión
La aplicación es vulnerable a ataques de inferencia de pertenencia.  
La falta de protección contra esta amenaza implica un riesgo crítico para la privacidad de los datos de clientes y la seguridad general del sistema.

