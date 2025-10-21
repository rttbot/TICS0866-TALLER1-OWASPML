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

