# 🏥 TICS00866 - Taller 2: Auditoría de Seguridad MedVQA-AI
## Diseño de Casos de Prueba para Sistemas de IA Médica

---

## 🎯 **¿Qué es MedVQA-AI?**

**MedVQA-AI** es un sistema de Visual Question Answering médico que:
- Analiza radiografías de tórax automáticamente
- Responde preguntas médicas en lenguaje natural
- Genera reportes clínicos estructurados
- Proporciona asistencia diagnóstica a radiólogos

### **🏗️ Arquitectura del Sistema**
- **Vision Branch**: CNN para procesamiento de imágenes médicas
- **Language Branch**: LLM para consultas médicas
- **Multimodal Fusion**: Mecanismo de atención
- **Generative Model**: Generación de respuestas médicas

---

## 📋 **¿Qué Debes Hacer para Obtener la Nota?**

### **PASO 1: Acceso al Repositorio GitHub**
- ✅ **Repositorio:** https://github.com/rttbot/TICS0866-TALLER1-OWASPML (mismo del Taller 1)
- ✅ **Ya tienes acceso** como colaborador
- ✅ **Crear carpeta específica** según línea de trabajo asignada
- ✅ **Subir archivos de entrega** en la carpeta correspondiente

### **PASO 2: Estructura de Carpetas Obligatoria**
```
TICS0866-TALLER1-OWASPML/
├── ML01/                    # Taller 1 (existente)
├── ML02/                    # Taller 1 (existente)
├── ...                      # Taller 1 (existente)
├── ML10/                    # Taller 1 (existente)
└── Taller2-Proyecto/        # NUEVO - Taller 2
    ├── README.md                    # Descripción del proyecto
    ├── Linea1-Deteccion-Anomalias/  # Track 1
    │   ├── README.md               # Descripción de la línea
    │   ├── vulnerabilidad-1.md     # Por cada vulnerabilidad
    │   ├── vulnerabilidad-2.md
    │   └── vulnerabilidad-3.md
    ├── Linea2-Generacion-Reportes/  # Track 2
    │   ├── README.md
    │   ├── vulnerabilidad-1.md
    │   ├── vulnerabilidad-2.md
    │   └── vulnerabilidad-3.md
    └── Linea3-Sistemas-VQA/        # Track 3
        ├── README.md
        ├── vulnerabilidad-1.md
        ├── vulnerabilidad-2.md
        └── vulnerabilidad-3.md
```

---

## 📝 **Entregables Obligatorios (100% del NPE)**

### **1. README Principal del Proyecto**
**Archivo:** `Taller2-Proyecto/README.md` (compartido - recuerde usar su cuenta para dejar evidencia de su trabajo)
- Descripción general de MedVQA-AI
- Explicación de las 3 líneas de trabajo
- Arquitectura del sistema 
- Objetivos de la auditoría

### **2. README por Línea de Trabajo**
**Archivo:** `Linea[X]/README.md`
- Descripción específica de la línea asignada
- Componentes arquitectónicos relevantes
- Vulnerabilidades a analizar
- Metodología de testing

### **3. Archivo por Vulnerabilidad**
**Archivo:** `vulnerabilidad-[NOMBRE].md`

#### **Contenido Obligatorio:**
- **Imagen explicativa simple** de cómo la vulnerabilidad afecta al sistema - considere napkin.ai
- **Ambiente de pruebas** (herramientas, datasets, configuración)
- **Identificación del sistema** (componente específico afectado)
- **Arquitectura** (cómo se relaciona con MedVQA-AI)
- **Impacto de la vulnerabilidad** (qué puede pasar)
- **Mitigaciones** (cómo prevenirla)
- **Equivalente en MITRE ATLAS ML** (mapeo de técnicas)
- **Casos de prueba** (mínimo 3 por vulnerabilidad)
- **Herramientas de automatización** disponibles
- **Datos de ejemplo** (entrada y salida esperada)
- **Formato de reporte** (campos y estructura)

### **4. Bonus: Videos con NotebookLM**
- Video explicativo de cada vulnerabilidad
- Cómo se explotaría en MedVQA-AI
- Cómo mitigar su ocurrencia
- Duración: 2-3 minutos por vulnerabilidad

---

## 🎯 **Las 3 Líneas de Trabajo**

### **🔍 LÍNEA 1: DETECCIÓN DE ANOMALÍAS CON DEEP LEARNING**
**Componente:** Vision Branch + Multimodal Fusion
**Vulnerabilidades:** ML01-ML10 (OWASP ML Top 10)

**Ejemplos de vulnerabilidades:**
- **ML01 - Manipulación de Entrada**: Adversarial patches en radiografías
- **ML02 - Envenenamiento de Datos**: Datasets médicos contaminados
- **ML08 - Sesgo del Modelo**: Discriminación en diagnósticos

### **📝 LÍNEA 2: GENERACIÓN DE REPORTES CLÍNICOS**
**Componente:** Language Branch + Generative Language Model
**Vulnerabilidades:** LLM01-LLM10 (OWASP LLM Top 10)

**Ejemplos de vulnerabilidades:**
- **LLM01 - Prompt Injection**: Manipulación de reportes médicos
- **LLM09 - Desinformación**: Reportes clínicos falsos
- **LLM06 - Agencia Excesiva**: Recomendaciones médicas no autorizadas

### **💬 LÍNEA 3: SISTEMAS DE PREGUNTA-RESPUESTA (VQA)**
**Componente:** Multimodal Fusion + Output Layer
**Vulnerabilidades:** Cross-modal attacks, Query manipulation

**Ejemplos de vulnerabilidades:**
- **Cross-modal attacks**: Ataques que explotan la fusión de modalidades
- **Query manipulation**: Manipulación de consultas médicas
- **System prompt leakage**: Filtración de prompts internos

---

## 📊 **Criterios de Evaluación**

### **Diseño de Casos de Prueba (40%)**
- **Calidad técnica**: 15%
- **Completitud**: 10%
- **Creatividad**: 10%
- **Aplicabilidad**: 5%

### **Análisis de Vulnerabilidades (30%)**
- **Identificación correcta**: 10%
- **Análisis de impacto**: 10%
- **Mitigaciones propuestas**: 10%

### **Documentación y Presentación (20%)**
- **Claridad en explicaciones**: 10%
- **Estructura del documento**: 5%
- **Imágenes explicativas**: 5%

### **Trabajo en Equipo (10%)**
- **Colaboración**: 5%
- **Distribución de tareas**: 5%

---

## 🛠️ **Herramientas Recomendadas**

### **Para Análisis de Modelos ML:**
- **Adversarial Robustness Toolbox (ART)**
- **CleverHans**
- **Foolbox**
- **SHAP/LIME**

### **Para Análisis de LLM:**
- **TextAttack**
- **PromptInject**
- **LM-Eval**
- **OpenAI Evals**

### **Para Testing de Seguridad:**
- **OWASP ZAP**
- **Burp Suite**
- **Nmap**
- **Wireshark**

### **Para Análisis de Datos:**
- **Pandas**
- **NumPy**
- **Matplotlib/Seaborn**
- **Scikit-learn**

---

## 📚 **Recursos de Referencia**

### **Documentación del Proyecto:**
- `01_proyecto_principal.md` - Descripción general
- `02_metodologia_hacking_etico.md` - Metodología paso a paso
- `03_vulnerabilidades_owasp.md` - Lista completa ML/LLM Top 10
- `04_arquitectura_tecnica.md` - Arquitectura detallada
- `09_mitre_atlas.md` - MITRE ATLAS ML

### **Recursos Externos:**
- [OWASP Machine Learning Security Top 10](https://mltop10.info/)
- [OWASP LLM Security Top 10](https://owasp.org/www-project-llm-top-10/)
- [MITRE ATLAS ML](https://atlas.mitre.org/)
- [Medical-CXR-VQA Dataset](https://physionet.org/content/medical-cxr-vqa-dataset/)

---

## ⚠️ **Normas del Taller**

- ✅ **Solo diseño** de casos de prueba (NO implementación)
- ✅ **Trabajo en equipo** (máximo 3 personas)
- ✅ **Entrega puntual** de todos los materiales
- ✅ **Formato correcto** de archivos
- ✅ **Participación activa** durante el taller

---

## 📞 **Contacto**

**Profesora:** Romina Torres  
**Asignatura:** TICS00866 - Auditoría y Defensa en Sistemas de Inteligencia Artificial  
**Institución:** Universidad Adolfo Ibáñez

---

## 🎯 **Entrega Final**

**Fecha límite:** Al final de la clase  
**Formato:** Repositorio GitHub completo  
**Presentación:** 5 minutos por equipo explicando su línea de trabajo

**Deben indicar:**
- Quiénes trabajaron en qué parte
- Vulnerabilidades analizadas
- Casos de prueba diseñados
- Herramientas identificadas
- Mitigaciones propuestas

---

**¡Éxito en el taller!** 🎯🏥
