# TICS0866-OWASPML
# Resumen Ejecutivo — Intrusion Aware & International AI Safety Report 2025

Como parte del procedimiento de **hacking ético en la aplicación Intrusion Aware**, hemos explorado las vulnerabilidades **ML-01,ML-04, ML-05, ML-06, ML-09, ML-10** del estándar **OWASP Machine Learning Top 10**.  
De este análisis identificamos que las vulnerabilidades relacionadas con la manipulación de entradas, el envenenamiento de datos y, especialmente, los ataques de inferencia (**ML-06, ML-05, ML-09**) representan los riesgos más relevantes para la seguridad del sistema.

En relacion al **ML-05** podemos ver que a través de ingenieria a la reversa se puede ir entendiendo y construyendo de cierta forma como funciona el codigo sin la necesidad de verlo lo cual permite así hacer ataques en funcion al codigo reconstruido para que sean lo más efectivos posible. Por otra parte el **ML-06** ataca a la cadena de suministro de para así lograr introducir alguna especie de malware escondido en las bilbiotecas de entrenamiento del modelo, finalmente el **ML-09** tambien presenta un gran riesgo ya que al estar dentro de la red se pueden modificar los putputs del sistema para hacer que un ataque pase desapercibido, o se pueden coordinar ataques con pruebas de seguridad para que los inputs que normalmentes podrian parecer de ataque tambien pasen desapercibidos.

Este trabajo se sitúa en la puntera de la innovación en ciberseguridad aplicada a IA, ya que incluso documentos avanzados y de alcance global, como el *International AI Safety Report 2025*, no abordan en detalle este tipo de vulnerabilidades técnicas específicas.  
Nuestra evaluación evidencia que, más allá de los riesgos estratégicos descritos en informes internacionales, la práctica concreta de seguridad en IA requiere atender vulnerabilidades puntuales y verificables que pueden ser explotadas por adversarios en escenarios reales.
---

## Reflexión

Si bien el *International AI Safety Report 2025* ofrece una visión amplia de los riesgos estratégicos y sociales de la IA, **no aborda en detalle las vulnerabilidades técnicas específicas** que ya han sido documentadas en marcos como **OWASP ML Top 10** o **MITRE ATLAS**.  

Esta omisión refleja la distancia que existe entre el análisis de alto nivel y la práctica concreta de seguridad en sistemas de Machine Learning. Vulnerabilidades como la manipulación de entradas, el envenenamiento de datos, la extracción de modelos o los ataques de inferencia (OWASP ML-04 / MITRE AML.T0024) son ejemplos claros de cómo un modelo puede ser explotado en escenarios reales.  

La importancia de reconocer estas vulnerabilidades específicas radica en que constituyen el **puente entre la teoría y la práctica**: transforman la noción abstracta de “riesgo” en escenarios concretos, reproducibles y con impactos verificables sobre la privacidad, la integridad y la disponibilidad de los sistemas.  

En definitiva, para que la seguridad de la IA sea efectiva, los debates estratégicos deben incorporar estas vulnerabilidades técnicas. Solo así será posible traducir las recomendaciones generales en **planes de acción concretos**, que permitan proteger de forma real a los sistemas que procesan información crítica en todo el mundo.  
