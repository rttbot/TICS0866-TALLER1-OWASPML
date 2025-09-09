# 🧬 TICS00866 - Taller 1: IA Security Village
## Instrucciones para Obtener la Nota

---

## 🎭 **¿Qué es el IA Security Village?**

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

## 📋 **¿Qué Debes Hacer para Obtener la Nota?**

### **PASO 1: Acuerdo de Hacking Ético (OBLIGATORIO)**
- ✅ **Descargar** el documento `S6-2-01-acuerdo-hacking-etico.md`
- ✅ **Completar** todos los campos (nombre, email, carrera, usuario GitHub)
- ✅ **Subir al aula virtual** en la casilla especial "Acuerdo Hacking Ético"
- ✅ **Nombre del archivo:** `acuerdo-hacking-etico-[TU-NOMBRE].pdf`

### **PASO 2: Acceso al Repositorio GitHub**
- ✅ **Repositorio:** https://github.com/rttbot/TICS0866-TALLER1-OWASPML
- ✅ **Ser agregado como colaborador** por la profesora
- ✅ **Crear carpeta específica** según vulnerabilidad asignada (ML01-ML10)
- ✅ **Subir archivos de entrega** en la carpeta correspondiente

---

## 📝 **Entregables Obligatorios (70% del NPE)**

### **Estructura de Carpetas:**
```
TICS0866-TALLER1-OWASPML/
├── ML01/          # Manipulación de Entrada
├── ML02/          # Envenenamiento de Datos
├── ML03/          # Inversión de Modelos
├── ML04/          # Inferencia de Pertenencia
├── ML05/          # Robo de Modelos
├── ML06/          # Cadena de Suministro
├── ML07/          # Transfer Learning
├── ML08/          # Sesgo del Modelo
├── ML09/          # Integridad de Salida
└── ML10/          # Envenenamiento de Modelos
```

### **Archivos por Carpeta (3 archivos obligatorios):**

#### **1. Casos de Prueba de Pentesting**
**Archivo:** `casos-prueba-pentesting-[NOMBRE].md`
- Mínimo **2 casos** para la vulnerabilidad asignada
- Procedimientos paso a paso detallados
- Evidencia esperada

#### **2. Análisis de Vulnerabilidades**
**Archivo:** `analisis-vulnerabilidades-owasp-atlas-[NOMBRE].md`
- Comparación OWASP ML Top 10 vs ATLAS ML
- Análisis de similitudes y diferencias
- Casos de aplicación específicos a Intrusion.Aware

#### **3. Informe Ejecutivo Final GRUPAL del Taller**
**Archivo:** `README.md`
- Resumen ejecutivo de hallazgos
- Análisis de vulnerabilidades encontradas
- Comparación con el estudio de México
- Conclusiones y lecciones aprendidas

---

## 📊 **Criterios de Evaluación**

### **Evaluación Técnica (50%)**
- **Calidad de casos de prueba:** 20%
- **Identificación de vulnerabilidades:** 15%
- **Análisis técnico:** 15%

### **Explicación Simple de Vulnerabilidades (25%)**
- **Claridad en la explicación:** 15%
  - **Simplicidad:** 8% - Explicación accesible para audiencia no técnica
  - **Ejemplos Prácticos:** 7% - Ejemplos concretos aplicados a Intrusion.Aware
- **Contexto Regional:** 10%
  - **Brecha Identificada:** 5% - Comprensión de la brecha en informes regionales
  - **Relevancia Social:** 5% - Explicación del impacto social de la vulnerabilidad

### **Reflexión Grupal sobre Seguridad en IA (20%)**
- **Análisis Crítico:** 10%
  - **Evaluación de OWASP ML Top 10:** 5% - ¿Evalúa lo que nosotros consideramos como seguridad en IA?
  - **Comparación con ATLAS ML:** 5% - Análisis de similitudes y diferencias
- **Contexto Regional:** 10%
  - **Brecha Identificada:** 5% - Comprensión de la brecha en informes regionales
  - **Relevancia Social:** 5% - Explicación del impacto social de la vulnerabilidad

### **Participación (5%)**
- **Colaboración:** 3%
- **Cumplimiento de normas:** 2%

---

## 🎯 **Las 10 Vulnerabilidades que Analizaremos**

Basándonos en el [OWASP Machine Learning Security Top 10](https://mltop10.info/OWASP-Machine-Learning-Security-Top-10.pdf), cada estudiante será asignado a **UNA** de las siguientes vulnerabilidades:

### **🎯 ML01 - Manipulación de Entrada** 
**¿Qué es?** Es como si alguien pudiera cambiar las señales de tráfico para que un auto autónomo tome la dirección equivocada.

**En Intrusion.Aware:** Atacantes alteran datos de entrada para evadir detección.
- **Ejemplo**: Enviar exactamente 999 bytes para evadir regla `sbytes > 1000`
- **Herramientas Kali**: `wfuzz`, `ffuf`, `dirb`

### **🎯 ML02 - Envenenamiento de Datos**
**¿Qué es?** Es como si alguien contaminara la comida de un chef para que aprenda a cocinar mal.

**En Intrusion.Aware:** Atacantes inyectan datos maliciosos en el dataset de entrenamiento.
- **Ejemplo**: Analistas corruptos marcan ataques como normales
- **Impacto**: Decision Trees aprenden reglas incorrectas

### **🎯 ML03 - Inversión de Modelos**
**¿Qué es?** Es como si alguien pudiera "hacer ingeniería inversa" de cómo piensa un experto.

**En Intrusion.Aware:** Atacantes intentan extraer información del modelo Decision Tree.
- **Limitación**: Decision Trees son intrínsecamente interpretables
- **Menor impacto** que en modelos black-box

### **🎯 ML04 - Inferencia de Pertenencia**
**¿Qué es?** Es como si alguien pudiera adivinar si una foto específica fue usada para entrenar un modelo.

**En Intrusion.Aware:** Atacantes determinan si datos específicos estuvieron en entrenamiento.
- **Protección**: Dataset UNSW-NB15 es público y anonimizado

### **🎯 ML05 - Robo de Modelos**
**¿Qué es?** Es como si alguien robara la receta secreta de un chef.

**En Intrusion.Aware:** Atacantes roban el modelo completo para uso malicioso.
- **Bajo riesgo**: Decision Trees son modelos simples y públicos

### **🎯 ML06 - Cadena de Suministro**
**¿Qué es?** Es como si alguien reemplazara las herramientas de un mecánico con herramientas defectuosas.

**En Intrusion.Aware:** Ataques a componentes externos del sistema.
- **Ejemplo**: Compromiso de librerías de ML o datasets

### **🎯 ML07 - Transfer Learning**
**¿Qué es?** Es como si alguien enseñara mal a un estudiante que luego enseña a otros.

**En Intrusion.Aware:** Ataques a modelos pre-entrenados.
- **Ejemplo**: Modelo base comprometido afecta modelos derivados

### **🎯 ML08 - Sesgo del Modelo**
**¿Qué es?** Es como si un juez fuera parcial hacia ciertos grupos de personas.

**En Intrusion.Aware:** Modelos que discriminan ciertos tipos de tráfico.
- **Ejemplo**: Falsos positivos para ciertos protocolos

### **🎯 ML09 - Integridad de Salida**
**¿Qué es?** Es como si alguien interceptara y cambiara el diagnóstico de un médico.

**En Intrusion.Aware:** Atacantes modifican las predicciones del modelo.
- **Ejemplo**: Cambiar "ATAQUE" por "NORMAL" en las alertas

### **🎯 ML10 - Envenenamiento de Modelos**
**¿Qué es?** Es como si alguien modificara directamente el cerebro de un experto.

**En Intrusion.Aware:** Atacantes manipulan los parámetros del modelo.
- **Ejemplo**: Alterar pesos de Decision Trees para clasificaciones incorrectas

---

## 📚 **Referencias Requeridas**

### **Recursos Principales:**
- [OWASP Machine Learning Security Top 10](https://github.com/owasp/www-project-machine-learning-security-top-10)
- [OWASP ML Top 10 PDF](https://mltop10.info/OWASP-Machine-Learning-Security-Top-10.pdf)
- [OWASP AI Security and Privacy Guide](https://owasp.org/www-project-ai-security-and-privacy-guide/)
- [Ejemplos Prácticos de Vulnerabilidades ML](https://www.getastra.com/blog/security-audit/owasp-machine-learning-top-10/)

### **Contexto Regional:**
- [Informe de México - ANCI](https://anci.gob.cl/noticias/mexico-reunion-ciberseguridad-ia/)
- [DEFCON Biohacking Village 2024 - Deloitte](https://www.deloitte.com/hu/en/services/risk-advisory/perspectives/defcon-biohacking-village-2024.html)

### **Recursos Adicionales:**
- [Adversarial Robustness Toolbox](https://github.com/Trusted-AI/adversarial-robustness-toolbox)
- [CleverHans Library](https://github.com/cleverhans-lab/cleverhans)
- [NIST AI Risk Management Framework](https://www.nist.gov/ai/risk-management)

---

## ⚠️ **Normas del Taller**

- ✅ **Solo diseño** de casos de prueba (NO implementación)
- ✅ **Trabajo individual** (ENTREGA individual)
- ✅ **Entrega puntual** de todos los materiales
- ✅ **Formato correcto** de archivos
- ✅ **Participación activa** durante el taller

---

## 📞 **Contacto**

**Profesora:** Romina Torres  
**Asignatura:** TICS00866 - Auditoría y Defensa en Sistemas de Inteligencia Artificial  
**Institución:** Universidad Adolfo Ibáñez

---

**¡Éxito en el taller!** 🎯
