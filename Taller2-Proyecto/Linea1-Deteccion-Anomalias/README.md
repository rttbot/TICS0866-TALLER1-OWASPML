# Auditoría de Seguridad - Línea 1: Detección de Anomalías con Deep Learning

## 📌 Descripción de la Línea Asignada
La Línea 1 del proyecto MedVQA-AI se enfoca en la auditoría de sistemas de detección automática de anomalías en imágenes médicas, específicamente radiografías de tórax. Utiliza técnicas de deep learning para identificar patologías, lesiones y hallazgos clínicos relevantes.

## 🧩 Componentes Arquitectónicos Relevantes
- **Vision Branch:** Red neuronal convolucional (CNN) especializada en imágenes médicas (ResNet).
- **Multimodal Fusion:** Mecanismo de atención para combinar características visuales y textuales.
- **Input Layer:** Imagen médica en formato DICOM/JPEG.
- **Output Layer:** Diagnóstico médico estructurado con nivel de confianza.

## 🚨 Vulnerabilidades a Analizar
- **ML01 - Manipulación de Entrada (Adversarial Examples):** Alteración de imágenes para evadir la detección de anomalías.
- **ML02 - Envenenamiento de Datos (Data Poisoning):** Inclusión de datos mal etiquetados en el entrenamiento para sesgar el modelo.
- **ML08 - Sesgos Algorítmicos:** Discriminación o errores sistemáticos en la clasificación por características demográficas o clínicas.

## 🧪 Metodología de Testing
La auditoría se basa en la metodología de hacking ético adaptada a sistemas de IA médica:

1. **Reconocimiento:** Mapeo de superficie de ataque y componentes vulnerables.
2. **Análisis de Vulnerabilidades:** Evaluación estática y dinámica del sistema, incluyendo revisión de arquitectura y comportamiento.
3. **Explotación:** Desarrollo de casos de prueba específicos para cada vulnerabilidad (ML01, ML02, ML08).
4. **Post-Explotación:** Análisis de impacto clínico, propuesta de mitigaciones y documentación de hallazgos.

Se utilizarán herramientas como Adversarial Robustness Toolbox (ART), SHAP/LIME para explicabilidad, y MITRE ATLAS para mapeo de amenazas.

## Sesgos del AA
El Aprendizaje Automático sufre de varios sesgos que pueden afectar la eficiencia de sus predicciones:

**El sesgo de reportaje** se produce cuando la frecuencia de los eventos, las propiedades o los resultados contenidos en un conjunto de datos no refleja exactitud su frecuencia en el mundo real. Una posible causa es que las personas tienden a enfocarse en el registro de circunstancias inusuales o especialmente memorables, ya que se supone que lo común no se necesita registrador.

**El sesgo histórico** se produce cuando los datos históricos reflejan desigualdades que existían en el mundo en ese momento.

**El sesgo de automatización** es la tendencia a favorecer los resultados que se generan mediante sistemas automatizados sobre los que se generan a través de aquellos que no lo son, sin importar la tasa de error de cada uno.

**El sesgo de cobertura** se produce si los datos no se seleccionan de forma representativa.

**El sesgo de no respuesta** (también conocido como sesgo de participación) ocurre si los datos no resultan representativos debido a interrupciones en la participación en el proceso de recopilación de datos.

**El sesgo muestral** ocurre si no se usa una aleatorización adecuada durante la recopilación de datos.

**El sesgo endogrupal** es una preferencia por los miembros de tu propio grupo al que también perteneces o por características que también compartes.

**El sesgo de homogeneidad de los demás** es una tendencia a estereotipar a los miembros individuales de un grupo al que no perteneces o a ver sus características como más uniformes.

**El sesgo implícita** tiene lugar cuando se realizan supposiciones en función de modelos mentales propios y experiencias personales que no aplican necesariamente un nivel más general.

**El sesgo de confirmación** se produce cuando los creadores de modelos procesan inconscientemente los datos de maneras que afirman sus hipótesis y creencias preexistentes.

**El sesgo del experimentador** ocurre cuando quien crea el modelo sigue entrenándolo hasta que produce un resultado que se alinea con su hipótesis original.

