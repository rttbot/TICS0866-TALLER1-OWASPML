1) Imagen explicativa simple
2) Ambiente de pruebas (herramientas, datasets, configuración):
   
4) Identificación del sistema (componente específico afectado):
5) Arquitectura (cómo se relaciona con MedVQA-AI):
6) Impacto de la vulnerabilidad (qué puede pasar):
   Lo que puede pasar principalmente con la vulnerabilidad ML 01 (manipulación de datos de entrada) es que puede dar respuestas clínicas incorrectas, dando como resultados patologías inexistentes o ignorar casos reales, haciendo que gente que no necesita recibir un tratamiento lo reciba y que personas que si lo necesitan lo sean tratadas de forma correcta y a tiempo. Además, esto conllevaría a una pérdida de la confianza clínica, pues respuestas erróneas pueden generar desconfianza entre profesionales médicos como pacientes, lo que puede perjudicar la reputación del centro de salud que incorpore este sistema.
   
8) Mitigaciones (cómo prevenirla):
   Implementar hashing para verificar que la imagen no ha sido modificada desde su origen
   Aplicar diversas técnicas de visión computacional para detectar alteraciones en la imagen (ruido, inserciones, distorsiones)
   Incluir en el dataset de entrenamiento ejemplos de imágenes manipuladas para que el modelo las aprenda a detectar.
   
10) Equivalente en MITRE ATLAS ML (mapeo de técnicas):
11) Casos de prueba (mínimo 3 por vulnerabilidad):
12) Herramientas de automatización disponibles:
13) Datos de ejemplo (entrada y salida esperada):
14) Formato de reporte (campos y estructura):
