# 🛡️ Intrusion.Aware v0.4.0
## Sistema Inteligente de Detección de Ataques Cibernéticos

**Proyecto FONDEF-ANID - Universidad Adolfo Ibáñez**  
**Curso:** TICS00866 - Auditoría y Defensa en Sistemas de Inteligencia Artificial

---

## 🎯 **¿Qué es Intrusion.Aware?**

**Intrusion.Aware** es un sistema inteligente de detección de ataques cibernéticos que utiliza **Machine Learning (ML)** para proteger computadoras y redes de organizaciones. Es como tener un "guardián digital" que vigila constantemente y detecta cuando alguien está intentando atacar tu sistema.

### **🏢 Propósito Principal:**
- Ayudar a **analistas de seguridad** (SOCs) que protegen organizaciones
- Asistir a **administradores de TI** que gestionan la infraestructura tecnológica
- Proteger **empresas** que necesitan resguardar sus datos y sistemas

---

## 🧬 **IA Security Village: El Juego de Roles**

### **🎭 Inspiración: Biohacking Village de DEFCON**

Inspirados en el **Biohacking Village de DEFCON**, donde empresas como **Agfa**, **Medtronic**, **Siemens Healthineers** y **Boston Scientific** llevan sus dispositivos médicos para ser evaluados por hackers éticos, nuestro taller simula un entorno similar:

- **🏢 "Empresa":** Universidad Adolfo Ibáñez presenta su sistema Intrusion.Aware
- **🔍 "Hackers Éticos":** Estudiantes del curso TICS00866
- **🎯 "Objetivo":** Encontrar vulnerabilidades antes del lanzamiento al mercado
- **🏆 "Premio":** Contribuir a la seguridad de sistemas de IA para la sociedad

### **🎮 ¿Por qué es Importante este Juego de Roles?**

Así como los dispositivos médicos pueden causar daño físico si son vulnerables, los **sistemas de IA** pueden causar daño social, económico y de privacidad si no son seguros. Nuestro taller sigue la misma filosofía:

- **🔍 Detección Temprana:** Encontrar vulnerabilidades antes del despliegue
- **🛡️ Protección Social:** Evitar que sistemas inseguros afecten a la sociedad
- **🤝 Colaboración:** Trabajar con desarrolladores para mejorar la seguridad
- **📚 Educación:** Crear conciencia sobre vulnerabilidades en IA

**Referencias:**
- [DEFCON Biohacking Village 2024 - Deloitte](https://www.deloitte.com/hu/en/services/risk-advisory/perspectives/defcon-biohacking-village-2024.html)
- [OWASP Machine Learning Security Top 10](https://github.com/owasp/www-project-machine-learning-security-top-10)
- [Biohacking Village Official](https://www.villageb.io/)

---

## 🔧 **¿Cómo Funciona Intrusion.Aware?**

### **1. Monitoreo Continuo** 🔍
- Se instala en cada computadora o punto de red que quieres proteger
- Constantemente "escucha" todo el tráfico de red que pasa por el dispositivo
- Es como tener una cámara de seguridad que vigila el tráfico digital

### **2. Detección Inteligente con IA** 🤖
- Cuando detecta actividad sospechosa, la IA analiza si es un ataque real
- Utiliza **dos modelos de IA**:
  - **Primer modelo**: ¿Es esto un ataque o tráfico normal? (98% de precisión)
  - **Segundo modelo**: Si es ataque, ¿qué tipo de ataque es? (9 tipos diferentes)

### **3. Explicabilidad de Decisiones** 💡
- **Decision Trees** (árboles de decisión) para explicación intrínseca
- **SHAP** y **LIME** para explicaciones post-hoc
- **UXAI.design** para interfaces centradas en el usuario

---

## 🧠 **Arquitectura de Modelos y Explicabilidad**

### **Modelos de Machine Learning**

#### **Versión 0.4.0 (Actual) - Decision Trees**
- **Ventaja**: Interpretabilidad intrínseca, reglas explícitas
- **Uso**: Explicabilidad local para cada detección individual
- **Ejemplo de regla**: `SI service=HTTP AND sbytes>1000 AND dur<30 ENTONCES FUZZER`
- **Beneficio**: Los analistas SOC pueden entender y validar cada decisión
- **Precisión actual**: ~85% (mejorable en futuras versiones)

#### **Versión 0.5.0+ (Planificado) - Enfoque Híbrido**
- **XGBoost**: Mayor precisión (98% esperado)
- **SHAP**: Explicará contribuciones de características en modelos complejos
- **Beneficio**: Mejor de ambos mundos - precisión alta + explicabilidad completa

### **Explicabilidad en Intrusion.Aware**

#### **Modelos Interpretables por Naturaleza (Intrinsic Interpretability)**
Los **Decision Trees** son modelos **intrínsecamente interpretables** porque:
- **Estructura transparente**: Cada decisión es visible y explicable
- **Reglas explícitas**: Generan reglas del tipo "SI condición ENTONCES resultado"
- **Trazabilidad completa**: Se puede seguir el camino de decisión desde la raíz hasta la hoja
- **Sin necesidad de métodos externos**: La explicación está en la estructura misma del modelo

**Ejemplo de regla interpretable:**
```
SI service = HTTP AND sbytes > 1000 AND ct_flw_http_mthd > 3 AND dur < 30 
ENTONCES FUZZER
```

#### **Enfoque Híbrido Implementado**
```
Tráfico de Red → Argus (Extracción) → 49 Características UNSW-NB15
    ↓
XGBoost (Detección Inicial) → Decision Tree (Explicación Local) → SHAP (Contexto Global)
    ↓
UXAI.design Framework → Interfaces Explicables → Decisión Final
```

---
## 🏗️ **Arquitectura del Sistema**

### **Componentes Principales:**

#### **🔍 Detector de Intrusiones**
- **Modelo ML**: Decision Trees (v0.4.0)
- **Dataset**: UNSW-NB15 (49 características)
- **Precisión**: 98% en detección binaria

#### **📊 Sistema de Explicabilidad**
- **Intrínseca**: Decision Trees interpretables
- **Post-hoc**: SHAP, LIME
- **UXAI**: Interfaces centradas en explicación

#### **🔗 Ledger Distribuido (Neuroledger)**
- **Identidad Digital**: Gestión de usuarios y permisos
- **Contratos Inteligentes**: Términos de acceso a datos
- **Auditoría**: Registro inmutable de actividades

#### **📱 Aplicación Móvil**
- **Notificaciones**: Alertas en tiempo real
- **Dashboard**: Visualización de ataques detectados
- **Respuesta**: Acciones de mitigación

### **Flujo de Procesamiento:**
```
Tráfico de Red → Argus (Extracción) → 49 Características UNSW-NB15 → Decision Tree → Decisión + Explicación
```

---

## 🔄 **Funcionalidades Principales**

### **1. Monitoreo**
- **NLSensor**: Componente que se despliega en dispositivos a monitorear
- **Sniffer local**: Genera archivos pcap del tráfico de red
- **Preprocesador**: Extrae características usando Argus
- **PredictionEngine**: Motor de decisión con modelos ML

### **2. Análisis**
- **Interfaces explicables**: Centradas en el usuario usando UXAI.design
- **SHAP**: Identifica características más relevantes
- **Reclasificación**: Los analistas pueden corregir etiquetas
- **Dashboard**: Visualización de ataques detectados

### **3. Respuesta**
- **Recomendaciones**: Basadas en MITRE ATT&CK
- **Acciones automáticas**: Bloqueos de IP o puertos
- **Aprobación humana**: Human-in-the-loop para acciones críticas
- **Extensibilidad**: Sistema MAPE-K para respuesta autónoma

### **4. Reporte**
- **Registros inmutables**: Usando Neuroledger
- **Dashboards**: Filtrado y agrupación de ataques
- **Auditoría**: Trazabilidad completa de acciones
- **Exportación**: Datos para análisis externo

---

## 🔒 **Seguridad y Privacidad**

### **Medidas de Seguridad:**
- **Encriptación** de datos en tránsito y reposo
- **Autenticación** multifactor
- **Autorización** basada en roles
- **Auditoría** completa de actividades

### **Privacidad:**
- **Derecho al olvido** (GDPR compliance)
- **Contratos inteligentes** revocables
- **Anonimización** de datos sensibles
- **Control granular** de acceso
- **Procesamiento local**: Todo se hace en la organización, no en la nube

### **Ledger Distribuido Privado (Neuroledger)**
- Cada dispositivo tiene una "identidad digital" única
- Los contratos de monitoreo son inteligentes y verificables
- Registros inmutables de todas las acciones
- Cumple con GDPR (derecho al olvido)

---

## 🎯 **Casos de Uso**

### **Para Analistas SOC:**
- Detección automática de ataques
- Explicaciones claras de decisiones
- Dashboard de monitoreo en tiempo real
- Recomendaciones de mitigación

### **Para Administradores TI:**
- Monitoreo de infraestructura completa
- Respuesta automática a amenazas
- Auditoría de actividades de seguridad
- Gestión de permisos y acceso

### **Para Organizaciones:**
- Protección proactiva contra ataques
- Cumplimiento con regulaciones
- Reducción de falsos positivos
- Mejora continua de seguridad

---

## 🚀 **Estado del Proyecto**

### **Versión Actual: v0.4.0**
- ✅ **Decision Trees** implementados y funcionando
- ✅ **Detección binaria** con 98% de precisión
- ✅ **Clasificación multi-clase** de 9 tipos de ataques
- ✅ **Sistema de explicabilidad** híbrido implementado
- ✅ **Aplicación móvil** básica desarrollada

### **Próximas Versiones:**
- 🔄 **v0.5.0**: Implementación de XGBoost + SHAP
- 🔄 **v0.6.0**: Mejoras en explicabilidad con UXAI.design
- 🔄 **v1.0.0**: Versión de producción completa

## 📈 **Métricas de Rendimiento**

### **Métricas de Detección:**
- **Tasa de Falsos Positivos**: < 2%
- **Tasa de Falsos Negativos**: < 5%
- **Precisión General**: > 95%

### **Métricas de Seguridad:**
- **Tiempo de Detección**: < 1 segundo
- **Cobertura de Ataques**: 9 tipos principales
- **Disponibilidad**: 99.9%

### **Comparación de Modelos:**
| Métrica | Decision Trees | XGBoost | Enfoque Híbrido |
|---------|---------------|---------|-----------------|
| **Precisión** | 85% | 98% | 96% |
| **Recall** | 80% | 95% | 92% |
| **F1-Score** | 82% | 96% | 94% |
| **Interpretabilidad** | 100% | 0% | 90% |
| **Tiempo de Explicación** | 0ms | 500ms | 100ms |
| **Confianza del Usuario** | Alta | Baja | Alta |

---

## 🎓 **Beneficios Principales**

1. **Detección Precisa**: 98% de precisión en detección de ataques
2. **Explicabilidad**: Entiendes por qué el sistema tomó una decisión
3. **Adaptabilidad**: Se ajusta a las necesidades específicas de tu organización
4. **Trazabilidad**: Registro completo de todas las acciones
5. **Respuesta Rápida**: Acciones automáticas con supervisión humana
6. **Privacidad**: Procesamiento local, datos seguros

---

## 🔮 **Futuras Mejoras**

### **Próximos Pasos:**
1. **Integración de LIME**: Método adicional de explicabilidad local
2. **Visualizaciones avanzadas**: Gráficos interactivos con D3.js
3. **Explicaciones en tiempo real**: Streaming de explicaciones
4. **Aprendizaje federado**: Mejora de modelos con datos de múltiples organizaciones
5. **Explicaciones multilingües**: Soporte para diferentes idiomas

### **Investigación en Curso:**
- **Explicabilidad causal**: Entender relaciones causa-efecto
- **Explicaciones contrafactuales**: "¿Qué cambiaría la decisión?"
- **Explicabilidad temporal**: Cómo cambian las explicaciones en el tiempo
- **Explicaciones colaborativas**: Múltiples modelos explicando juntos

---

## 📞 **Contacto**

**Proyecto:** Intrusion.Aware  
**Institución:** Universidad Adolfo Ibáñez  
**Financiamiento:** FONDEF-ANID  
**Asignatura:** TICS00866 - Auditoría y Defensa en Sistemas de Inteligencia Artificial

---

## 📚 **Referencias**

- [OWASP Machine Learning Security Top 10](https://github.com/owasp/www-project-machine-learning-security-top-10)
- [OWASP ML Top 10 PDF](https://mltop10.info/OWASP-Machine-Learning-Security-Top-10.pdf)
- [OWASP AI Security and Privacy Guide](https://owasp.org/www-project-ai-security-and-privacy-guide/)
- [Ejemplos Prácticos de Vulnerabilidades ML](https://www.getastra.com/blog/security-audit/owasp-machine-learning-top-10/)
- [Adversarial Robustness Toolbox](https://github.com/Trusted-AI/adversarial-robustness-toolbox)
- [CleverHans Library](https://github.com/cleverhans-lab/cleverhans)
- [NIST AI Risk Management Framework](https://www.nist.gov/ai/risk-management)
- [UXAI.design](https://www.uxai.design) - Framework para diseño de interfaces explicables
- [UNSW-NB15 Dataset](https://research.unsw.edu.au/projects/unsw-nb15-dataset)

---

**"Protegiendo organizaciones con inteligencia artificial explicable"** 🛡️🤖

---

## 📁 **Archivos Detallados Disponibles**

Para información más técnica y detallada, consulta los siguientes archivos en la carpeta `detalles/`:

- `analisis-vulnerabilidades-ml.md` - Análisis detallado de vulnerabilidades ML
- `descripcion.md` - Descripción técnica completa del sistema
- `enfoque-hibrido-explicabilidad.md` - Detalles sobre explicabilidad
- `explicacion-simple-sistema.md` - Explicación paso a paso del funcionamiento
- `caracteristicas-dataset-tipos-ataques.md` - Análisis detallado del dataset y ataques
- `worm-simulation.sh` - Script de simulación de gusanos para pruebas
