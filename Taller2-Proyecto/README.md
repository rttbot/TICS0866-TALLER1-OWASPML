# Proyecto de Auditoría de Seguridad - MedVQA-AI

## Descripción General de MedVQA-AI
MedVQA-AI es un sistema avanzado de Visual Question Answering (VQA) médico que combina procesamiento de imágenes radiológicas con modelos de lenguaje natural para asistir en el diagnóstico clínico. El sistema está diseñado para analizar radiografías de tórax y responder preguntas médicas complejas en lenguaje natural, generando reportes estructurados y recomendaciones clínicas.

## Líneas de Trabajo del Proyecto

### Línea 1: Detección de Anomalías con Deep Learning
- **Descripción:** Esta línea se enfoca en auditar los componentes encargados de detectar automáticamente anomalías en radiografías médicas.
- **Componentes involucrados:** Vision Branch (CNN) + Multimodal Fusion
- **Vulnerabilidades a analizar:**
  - ML01: Manipulación de Entrada (Adversarial Examples)
  - ML02: Envenenamiento de Datos (Data Poisoning)
  - ML08: Sesgos Algorítmicos
- **Objetivo:** Evaluar la robustez del sistema ante entradas maliciosas, sesgos en el entrenamiento y errores de clasificación que puedan comprometer el diagnóstico.

### Línea 2: 
### Línea 3: 

## Arquitectura del Sistema
MedVQA-AI utiliza una arquitectura multimodal compuesta por:
- **Vision Branch:** CNN especializada en imágenes médicas (ResNet)
- **Language Branch:** LLM para procesamiento de texto médico (BERT)
- **Multimodal Fusion:** Mecanismos de atención para combinar información visual y textual
- **Generative Model:** Generación de respuestas y reportes médicos

## Objetivos de la Auditoría
### Linea 1
- Identificar vulnerabilidades específicas en los modelos de clasificación de imágenes médicas
- Evaluar el impacto clínico de ataques adversarios de entradas, datos de entrenamiento y sesgos algorítmicos
- Proponer mitigaciones técnicas y éticas para mejorar la seguridad del sistema
