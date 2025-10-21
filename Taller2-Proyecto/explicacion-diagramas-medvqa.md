# 📋 MedVQA-AI - Explicación del Diagrama de Componentes y Casos de Uso

## 🏗️ **Explicación del Diagrama de Componentes UML**

### **Descripción General del Sistema MedVQA-AI**

El diagrama de componentes UML representa la arquitectura completa del sistema **MedVQA-AI**, un sistema de Visual Question Answering médico que combina procesamiento de imágenes médicas con generación de lenguaje natural para responder preguntas clínicas sobre radiografías de tórax.

### **Arquitectura por Capas**

#### **1. Input Layer (Capa de Entrada)**
- **Chest X-Ray Images**: Componente que maneja imágenes médicas en formato DICOM/JPEG
- **Natural Language Questions**: Componente que procesa preguntas médicas en lenguaje natural
- **Interfaces**: IImageInput e ITextInput para la comunicación entre capas

#### **2. Vision Branch (Rama de Visión)**
- **Image Preprocessing**: Normalización, redimensionamiento y aumento de imágenes
- **CNN Backbone**: Modelos pre-entrenados (ResNet-152, ViT-Base, DenseNet)
- **Feature Extractor**: Extracción de características visuales
- **Global Average Pooling**: Reducción dimensional
- **Visual Features**: Representación final de 2048 dimensiones

#### **3. Language Branch (Rama de Lenguaje)**
- **Tokenization**: Procesamiento de texto (WordPiece, BPE, SentencePiece)
- **Token Embedding**: Conversión a embeddings
- **Positional Encoding**: Codificación posicional
- **Text Features**: Representación final de 768 dimensiones

#### **4. Multimodal Fusion Layer (Capa de Fusión Multimodal)**
- **Linear Projection**: Proyección a espacio común (512 dims)
- **Cross-Modal Attention**: Mecanismo de atención cruzada
- **Feature Alignment**: Alineación de características
- **Fusion Network**: Red de fusión
- **Fused Features**: Representación fusionada de 512 dimensiones

#### **5. Generative Language Model (Modelo Generativo)**
- **Transformer Encoder**: Codificador con 12 capas y 8 cabezas
- **Decoder Layers**: Capas decodificadoras (12 capas, causal, enmascarado)
- **Medical Knowledge Base**: Base de conocimiento médico
- **Response Generator**: Generador de respuestas

#### **6. Output Layer (Capa de Salida)**
- **Structured Response**: Respuesta estructurada
- **Confidence Scores**: Puntuaciones de confianza
- **Medical References**: Referencias médicas
- **Follow-up Recommendations**: Recomendaciones de seguimiento

#### **7. Componentes Externos**
- **Medical-CXR-VQA Dataset**: Dataset con 780,014 pares pregunta-respuesta
- **External Medical APIs**: APIs médicas externas

---

## 🔄 **Diagramas de Secuencia para Casos de Uso**

### **Caso de Uso 1: Consulta Médica Básica**

```plantuml
@startuml MedVQA-AI Basic Medical Query
!theme plain
title MedVQA-AI - Consulta Médica Básica

actor "Radiólogo" as User
participant "Input Layer" as Input
participant "Vision Branch" as Vision
participant "Language Branch" as Language
participant "Multimodal Fusion" as Fusion
participant "Generative Model" as GenModel
participant "Output Layer" as Output
participant "Medical KB" as KB

User -> Input : Sube radiografía de tórax
User -> Input : Pregunta: "¿Hay evidencia de neumonía?"

Input -> Vision : Envía imagen (DICOM/JPEG)
Input -> Language : Envía pregunta en texto

Vision -> Vision : Preprocessing (normalizar, resize)
Vision -> Vision : CNN Backbone (ResNet-152)
Vision -> Vision : Feature Extractor
Vision -> Vision : Global Average Pooling
Vision -> Vision : Visual Features (2048 dims)

Language -> Language : Tokenization (WordPiece)
Language -> Language : Token Embedding
Language -> Language : Positional Encoding
Language -> Language : Text Features (768 dims)

Vision -> Fusion : Visual Features
Language -> Fusion : Text Features

Fusion -> Fusion : Linear Projection (2048→512, 768→512)
Fusion -> Fusion : Cross-Modal Attention
Fusion -> Fusion : Feature Alignment
Fusion -> Fusion : Fusion Network
Fusion -> Fusion : Fused Features (512 dims)

Fusion -> GenModel : Fused Features
GenModel -> KB : Consulta base de conocimiento médico
KB -> GenModel : Información médica relevante

GenModel -> GenModel : Transformer Encoder (12 layers)
GenModel -> GenModel : Decoder Layers (12 layers)
GenModel -> GenModel : Response Generation

GenModel -> Output : Respuesta generada
Output -> Output : Structured Response
Output -> Output : Confidence Scores
Output -> Output : Medical References
Output -> Output : Follow-up Recommendations

Output -> User : "Sí, hay evidencia de neumonía en el lóbulo superior derecho. Confianza: 0.87. Recomendación: Seguimiento en 48 horas."

@enduml
```

### **Caso de Uso 2: Consulta Compleja con Múltiples Hallazgos**

```plantuml
@startuml MedVQA-AI Complex Medical Query
!theme plain
title MedVQA-AI - Consulta Compleja con Múltiples Hallazgos

actor "Radiólogo" as User
participant "Input Layer" as Input
participant "Vision Branch" as Vision
participant "Language Branch" as Language
participant "Multimodal Fusion" as Fusion
participant "Generative Model" as GenModel
participant "Output Layer" as Output
participant "Medical KB" as KB
participant "External APIs" as APIs

User -> Input : Sube radiografía compleja
User -> Input : Pregunta: "¿Qué anomalías se observan y cuál es la severidad?"

Input -> Vision : Imagen compleja con múltiples hallazgos
Input -> Language : Pregunta compleja

Vision -> Vision : Preprocessing avanzado
Vision -> Vision : CNN Backbone (ViT-Base)
Vision -> Vision : Feature Extractor
Vision -> Vision : Visual Features (2048 dims)

Language -> Language : Tokenization compleja
Language -> Language : Text Features (768 dims)

Vision -> Fusion : Visual Features
Language -> Fusion : Text Features

Fusion -> Fusion : Cross-Modal Attention (múltiples regiones)
Fusion -> Fusion : Fused Features (512 dims)

Fusion -> GenModel : Fused Features
GenModel -> KB : Consulta múltiples condiciones
KB -> GenModel : Información sobre neumonía, edema, etc.

GenModel -> GenModel : Procesamiento multimodal complejo
GenModel -> GenModel : Generación de respuesta estructurada

GenModel -> Output : Respuesta compleja
Output -> Output : Structured Response (múltiples hallazgos)
Output -> Output : Confidence Scores (por cada hallazgo)
Output -> Output : Medical References (regiones específicas)
Output -> Output : Follow-up Recommendations (priorizadas)

Output -> APIs : Consulta referencias médicas externas
APIs -> Output : Información adicional de guidelines

Output -> User : "Hallazgos: 1) Neumonía en LSR (confianza: 0.92), 2) Edema pulmonar leve (confianza: 0.78), 3) Cardiomegalia (confianza: 0.65). Recomendaciones: Tratamiento inmediato para neumonía, seguimiento cardiológico."

@enduml
```

### **Caso de Uso 3: Consulta de Seguimiento**

```plantuml
@startuml MedVQA-AI Follow-up Query
!theme plain
title MedVQA-AI - Consulta de Seguimiento

actor "Radiólogo" as User
participant "Input Layer" as Input
participant "Vision Branch" as Vision
participant "Language Branch" as Language
participant "Multimodal Fusion" as Fusion
participant "Generative Model" as GenModel
participant "Output Layer" as Output
participant "Medical KB" as KB

User -> Input : Sube radiografía de seguimiento
User -> Input : Pregunta: "¿Ha mejorado la neumonía comparado con la radiografía anterior?"

Input -> Vision : Imagen de seguimiento
Input -> Language : Pregunta comparativa

Vision -> Vision : Preprocessing
Vision -> Vision : CNN Backbone
Vision -> Vision : Visual Features (2048 dims)

Language -> Language : Tokenization
Language -> Language : Text Features (768 dims)

Vision -> Fusion : Visual Features
Language -> Fusion : Text Features

Fusion -> Fusion : Cross-Modal Attention
Fusion -> Fusion : Fused Features (512 dims)

Fusion -> GenModel : Fused Features
GenModel -> KB : Consulta evolución de neumonía
KB -> GenModel : Criterios de mejoría

GenModel -> GenModel : Análisis comparativo
GenModel -> GenModel : Generación de respuesta

GenModel -> Output : Respuesta comparativa
Output -> Output : Structured Response (comparación)
Output -> Output : Confidence Scores (mejoría)
Output -> Output : Medical References (criterios)
Output -> Output : Follow-up Recommendations (próximos pasos)

Output -> User : "La neumonía muestra mejoría significativa. Reducción del 60% en la opacidad del LSR. Confianza: 0.89. Recomendación: Continuar tratamiento actual, nueva radiografía en 1 semana."

@enduml
```

### **Caso de Uso 4: Consulta de Emergencia**

```plantuml
@startuml MedVQA-AI Emergency Query
!theme plain
title MedVQA-AI - Consulta de Emergencia

actor "Médico de Emergencia" as User
participant "Input Layer" as Input
participant "Vision Branch" as Vision
participant "Language Branch" as Language
participant "Multimodal Fusion" as Fusion
participant "Generative Model" as GenModel
participant "Output Layer" as Output
participant "Medical KB" as KB
participant "Alert System" as Alert

User -> Input : Sube radiografía de emergencia
User -> Input : Pregunta: "¿Hay signos de neumotórax o neumoperitoneo?"

Input -> Vision : Imagen de emergencia
Input -> Language : Pregunta crítica

Vision -> Vision : Preprocessing rápido
Vision -> Vision : CNN Backbone (procesamiento acelerado)
Vision -> Vision : Visual Features (2048 dims)

Language -> Language : Tokenization
Language -> Language : Text Features (768 dims)

Vision -> Fusion : Visual Features
Language -> Fusion : Text Features

Fusion -> Fusion : Cross-Modal Attention (modo emergencia)
Fusion -> Fusion : Fused Features (512 dims)

Fusion -> GenModel : Fused Features
GenModel -> KB : Consulta condiciones críticas
KB -> GenModel : Criterios de emergencia

GenModel -> GenModel : Procesamiento prioritario
GenModel -> GenModel : Generación de respuesta urgente

GenModel -> Output : Respuesta crítica
Output -> Output : Structured Response (urgente)
Output -> Output : Confidence Scores (alta confianza)
Output -> Output : Medical References (protocolos de emergencia)
Output -> Output : Follow-up Recommendations (inmediatas)

Output -> Alert : Alerta si condición crítica
Alert -> User : Notificación de emergencia

Output -> User : "URGENTE: Signos de neumotórax en hemitórax derecho. Confianza: 0.95. Recomendación: Toracostomía inmediata. Contactar cirujano torácico."

@enduml
```

---

## 🔍 **Análisis de Flujos de Datos**

### **Flujo Principal de Datos**
1. **Entrada**: Imagen + Pregunta → Input Layer
2. **Procesamiento Paralelo**: Vision Branch + Language Branch
3. **Fusión**: Multimodal Fusion Layer
4. **Generación**: Generative Language Model
5. **Salida**: Output Layer → Respuesta estructurada

### **Puntos de Integración**
- **Cross-Modal Attention**: Punto crítico donde se combinan características visuales y textuales
- **Medical Knowledge Base**: Integración de conocimiento médico especializado
- **External APIs**: Conexión con sistemas médicos externos

### **Consideraciones de Rendimiento**
- **Procesamiento Paralelo**: Vision y Language Branch operan simultáneamente
- **Optimización**: Diferentes modelos CNN según complejidad de la consulta
- **Modo Emergencia**: Procesamiento acelerado para casos críticos

---

## 🛡️ **Puntos de Vulnerabilidad Identificados**

### **En el Flujo de Datos**
1. **Input Layer**: Manipulación de imágenes o preguntas
2. **Vision Branch**: Adversarial examples en CNN
3. **Language Branch**: Prompt injection en tokenization
4. **Multimodal Fusion**: Cross-modal attacks
5. **Generative Model**: Manipulación de respuestas
6. **Output Layer**: Interceptación de resultados

### **En las Interfaces**
- **IImageInput**: Validación de imágenes médicas
- **ITextInput**: Sanitización de preguntas
- **IExternalAPIs**: Autenticación y autorización
- **IDataset**: Integridad del dataset médico

---

**¡Estos diagramas proporcionan una visión completa del sistema MedVQA-AI!** 🎯🏥
