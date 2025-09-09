# Informe de Vulnerabilidad: ML-04 / AML.T0024

Tras la evaluación realizada, se determinó que la aplicación de monitoreo de tráfico es **vulnerable a ataques de inferencia de pertenencia (Membership Inference Attacks)**.  
En este contexto, el riesgo principal no está en revelar datos sensibles de clientes —ya que el modelo fue entrenado con el dataset UNSW-NB15, público y anonimizado—, sino en permitir que **adversarios organizados utilicen el sistema para probar si sus técnicas de ataque ya forman parte del entrenamiento**.  
Esto convierte al modelo en un **laboratorio de validación de TTPs**, facilitando que los atacantes descarten métodos obsoletos y prioricen aquellos no vistos por el detector.

---

## OWASP ML-04: Membership Inference Attack
Según OWASP, este ataque ocurre cuando un atacante logra descubrir si un dato específico formó parte del conjunto de entrenamiento de un modelo de Machine Learning.  
El atacante envía múltiples consultas al modelo con distintos datos de prueba, explotando que el modelo suele comportarse de manera diferente frente a datos nuevos en comparación con los que ya conoce.  
Al analizar los niveles de confianza de las predicciones, es posible inferir si un dato proviene del conjunto de entrenamiento original, lo que permite identificar usuarios y/o exponer información sensible.  
Fuente: [OWASP ML-04](https://owasp.org/www-project-machine-learning-security-top-10/docs/ML04_2023-Membership_Inference_Attack.html)

---

## MITRE ATLAS AML.T0024: Exfiltration via AI Inference API
De manera equivalente, MITRE ATLAS clasifica esta amenaza bajo la técnica **AML.T0024 – Exfiltration via AI inference API**.  
Este ataque ocurre cuando un adversario utiliza la API de inferencia de un modelo para determinar si un dato específico formó parte del entrenamiento.  
El modelo suele responder con mayor confianza ante datos previamente vistos, lo que permite inferir información privada del dataset original y exponer datos sensibles o identificar usuarios.  
Fuente: [MITRE ATLAS AML.T0024](https://atlas.mitre.org/techniques/AML.T0024)

---

## Justificación
Ambas clasificaciones —**OWASP ML-04** y **MITRE ATLAS AML.T0024**— describen la misma vulnerabilidad fundamental:  
la capacidad de un atacante de inferir si un dato perteneció al conjunto de entrenamiento de un modelo de ML.  

En este caso, la diferencia crítica radica en el **tipo de dataset usado**:  
- Dado que el entrenamiento se hizo con **UNSW-NB15 (público y anonimizado)**, el riesgo de exponer información privada de clientes es **bajo**.  
- Sin embargo, la vulnerabilidad sigue siendo relevante porque permite a los adversarios **inferir qué técnicas de ataque ya están contempladas por el detector** y cuáles no.  
- Esto otorga a los atacantes la capacidad de **acelerar la innovación de sus TTPs** y optimizar su efectividad contra el sistema, debilitando la capacidad de defensa.

---

## Conclusión
La aplicación es **vulnerable a ataques de inferencia de pertenencia (ML-04 / AML.T0024)**.  

- **Impacto en privacidad:** bajo, ya que el dataset de entrenamiento es público y anonimizado.  
- **Impacto en seguridad operativa:** alto, porque la app puede ser explotada como un entorno de **“novelty testing”** que ayuda a adversarios a identificar qué técnicas de ataque son detectadas y cuáles permanecen invisibles.  

En consecuencia, aunque la vulnerabilidad no expone datos sensibles de clientes, sí compromete la **eficacia del sistema de detección** y aumenta el riesgo de evasión frente a actores maliciosos sofisticados.
