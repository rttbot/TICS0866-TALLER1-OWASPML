1) Imagen explicativa simple
   
3) Ambiente de pruebas (herramientas, datasets, configuración):
   
4) Identificación del sistema (componente específico afectado):
   
6) Arquitectura (cómo se relaciona con MedVQA-AI):
   
7) Impacto de la vulnerabilidad (qué puede pasar):
   Lo que puede pasar principalmente con la vulnerabilidad ML 01 (manipulación de datos de entrada) es que puede dar respuestas clínicas incorrectas, dando como resultados patologías inexistentes o ignorar casos reales, haciendo que gente que no necesita recibir un tratamiento lo reciba y que personas que si lo necesitan lo sean tratadas de forma correcta y a tiempo. Además, esto conllevaría a una pérdida de la confianza clínica, pues respuestas erróneas pueden generar desconfianza entre profesionales médicos como pacientes, lo que puede perjudicar la reputación del centro de salud que incorpore este sistema.
   
8) Mitigaciones (cómo prevenirla):
   Implementar hashing para verificar que la imagen no ha sido modificada desde su origen
   Aplicar diversas técnicas de visión computacional para detectar alteraciones en la imagen (ruido, inserciones, distorsiones)
   Incluir en el dataset de entrenamiento ejemplos de imágenes manipuladas para que el modelo las aprenda a detectar.
   
10) Equivalente en MITRE ATLAS ML (mapeo de técnicas):
    Un equivalente a ML 01 OWASP Top 10 en MITRE ATLAS sería ATLAS-1002: Adversarial Input Manipulation, en el cual el atacante modifica la entrada para provocar errores, omisiones o respuestas incorrectas del modelo. En este caso, introducir como datos de entrada imágenes médicas alteradas.
    
12) Casos de prueba (mínimo 3 por vulnerabilidad):
    
    
14) Herramientas de automatización disponibles:
    Dentro de las herramientas de automatización disponibles, se recomienda utilizar Adversarial Robustness Toolbox (ART) y Foolbox. La primera se dice que es la más completa, ya que puede generar, detectar y defender contra ataques adversariales en imágenes médicas, mientras que el segundo es muy útil para generar ataques, pero es menos orientada a la defensa del modelo.

