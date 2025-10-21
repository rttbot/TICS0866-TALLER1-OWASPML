Imagen explicativa simple de cómo la vulnerabilidad afecta al sistema - considere
napkin.ai


Ambiente de pruebas (herramientas, datasets, configuración)

1. Identificación del sistema

Componente afectado: Multimodal Fusion y Generative Model
Estos módulos dependen de datos de entrenamiento y pruebas bien estructurados. Un ambiente de pruebas mal configurado puede afectar la calidad de las respuestas médicas generadas.


2. Arquitectura y relación con MedVQA-AI

Vision Branch (CNN) procesa imágenes médicas.
Language Branch (LLM) interpreta preguntas médicas.
Multimodal Fusion combina ambas entradas.
Generative Model produce respuestas clínicas.
El ambiente de pruebas afecta directamente la validación de la interacción entre estos componentes, especialmente en la fase de fine-tuning y evaluación.


3. Impacto de la vulnerabilidad

Resultados clínicos incorrectos: Diagnósticos erróneos por falta de validación adecuada.
Sobreajuste o subajuste: Modelos que no generalizan bien.
Falsos positivos/negativos: Riesgo para la salud del paciente.
Pérdida de confianza: Por parte de radiólogos y personal médico.


4. Mitigaciones

Uso de datasets médicos validados como MIMIC-CXR, CheXpert, NIH ChestX-ray14.
Separación clara entre datos de entrenamiento, validación y prueba.
Simulación de escenarios clínicos reales.
Auditoría del entorno de pruebas (versiones, configuraciones, dependencias).
Pruebas automatizadas con cobertura de casos extremos.


5. Equivalente en MITRE ATLAS

ATLAS Technique: MLTA-8: Inadequate Testing and Evaluation
Relacionado con fallas en la validación de modelos ML en ambientes simulados o reales.


6. Casos de prueba (mínimo 3)

Radiografía con neumonía: Entrada de imagen + pregunta “¿Hay signos de neumonía?” → Verificar respuesta con diagnóstico real.
Radiografía normal: Entrada + pregunta “¿Hay alguna anomalía?” → Esperar respuesta negativa.
Imagen corrupta o incompleta: Evaluar cómo responde el sistema ante datos defectuosos.


7. Herramientas de automatización disponibles

pytest, unittest para pruebas unitarias.
Great Expectations para validación de datos.
MLflow para seguimiento de experimentos.
Docker para replicar ambientes de prueba.
TensorBoard para visualización de métricas.


8. Datos de ejemplo

Entrada:

Imagen: Radiografía de tórax (formato DICOM o PNG)
Pregunta: “¿Hay signos de edema pulmonar?”


Salida esperada:

Texto: “Sí, se observan signos compatibles con edema pulmonar en el lóbulo inferior derecho.”

9. Formato de reporte:

    Vulnerabilidad: ML-08 - Ambiente de pruebas
Sistema afectado: MedVQA-AI
Componente: Multimodal Fusion, Generative Model
Impacto: Diagnóstico erróneo, pérdida de confianza
Mitigaciones:
  - Uso de datasets validados
  - Separación de datos
  - Simulación clínica
Casos de prueba:
  - Neumonía detectada
  - Imagen normal
  - Imagen corrupta
Herramientas:
  - pytest, MLflow, Docker
Datos de ejemplo:
  - Entrada: Radiografía + pregunta
  - Salida: Respuesta médica estructurada
Equivalente MITRE ATLAS: MLTA-8
