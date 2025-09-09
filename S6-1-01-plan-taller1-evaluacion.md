# 🧬 TICS00866 - Taller 1: IA Security Village
## Plan de Evaluación

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

## 📋 **¿Qué es el Taller?**

**Objetivo:** Diseñar casos de prueba de pentesting ético para evaluar vulnerabilidades de IA en el sistema **Intrusion.Aware** (proyecto FONDEF-ANID).

**Inspiración:** Biohacking Village de DEFCON (empresas como Medtronic, Siemens llevan dispositivos médicos para evaluación ética).

**Duración:** 2 bloques de 70 minutos (140 min total)

---

## 🎯 **Estructura del Taller**

### **BLOQUE 1: INTRODUCCIÓN Y DISEÑO (70 min)**
- **Presentación del Sistema (15 min)** - Intrusion.Aware
- **Asignación de Vulnerabilidades (10 min)** - ML01-ML10 individual
- **Diseño de Casos de Prueba (45 min)** - Mínimo 2 casos por vulnerabilidad

### **BLOQUE 2: REFINAMIENTO Y ENTREGA (70 min)**
- **Refinamiento de Casos (30 min)** - Mejorar procedimientos
- **Preparación de Entregables (25 min)** - Completar análisis y reporte
- **Entrega y Evaluación (15 min)** - Subir archivos al GitHub

---

## 📝 **Entregables Obligatorios**

### **Repositorio GitHub:**
**URL:** https://github.com/rttbot/TICS0866-TALLER1-OWASPML

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

### **3 Archivos por Carpeta:**

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

#### **3. Reporte Final del Taller**
**Archivo:** `reporte-final-taller1-[NOMBRE].md`
- Resumen ejecutivo de hallazgos
- Análisis de vulnerabilidades encontradas
- Recomendaciones de mitigación
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
  - **Evaluación de OWASP ML Top 10 vs Informe México:** 5% - ¿Evalúa lo que nosotros consideramos como seguridad en IA?
  - **Comparación con ATLAS ML:** 5% - Análisis de similitudes y diferencias
- **Contexto Regional:** 10%
  - **Brecha Identificada:** 5% - Comprensión de la brecha en informes regionales
  - **Relevancia Social:** 5% - Explicación del impacto social de la vulnerabilidad

### **Participación (5%)**
- **Colaboración:** 3%
- **Cumplimiento de normas:** 2%

---

## 🎯 **Preparación Previa**

### **PASO 1: Acuerdo de Hacking Ético (OBLIGATORIO)**
- ✅ **Descargar** `S6-2-01-acuerdo-hacking-etico.md`
- ✅ **Completar** todos los campos (nombre, email, carrera, **usuario GitHub**)
- ✅ **NO firmar** (solo completar con tu nombre)
- ✅ **Subir al aula virtual** en casilla especial "Acuerdo Hacking Ético"
- ✅ **Nombre:** `acuerdo-hacking-etico-[TU-NOMBRE].pdf`

### **PASO 2: Acceso al Repositorio GitHub**
- ✅ **Repositorio:** https://github.com/rttbot/TICS0866-TALLER1-OWASPML
- ✅ **Ser agregado como colaborador** por la profesora
- ✅ **Crear carpeta específica** según vulnerabilidad asignada (ML01-ML10)

---

## ⚠️ **Normas del Taller**

- ✅ **Solo diseño** de casos de prueba (NO implementación)
- ✅ **Trabajo individual** (ENTREGA individual)
- ✅ **Entrega puntual** de todos los materiales
- ✅ **Formato correcto** de archivos
- ✅ **Participación activa** durante el taller

---

## 🔧 **Metodología de Pentesting en IA**

### **Fases del Pentesting:**
1. **Reconocimiento** - Identificar arquitectura y componentes de IA
2. **Evaluación** - Probar vulnerabilidades específicas (ML01-ML10)
3. **Explotación** - Desarrollar casos de prueba específicos
4. **Documentación** - Reportar hallazgos y recomendaciones


## 📝 **Template para Casos de Prueba**

### **Formato Obligatorio:**
```markdown
## 🔴 CASO DE PRUEBA - [TIPO DE VULNERABILIDAD]

**ID:** [IDENTIFICADOR ÚNICO]
**Tipo:** [ATAQUE/VÁLIDO/INVÁLIDO]
**Vulnerabilidad:** [ML01/ML02/ML03/ML04/ML05]
**Descripción:** [DESCRIPCIÓN CLARA DEL CASO]

**Entrada:** [DATOS DE ENTRADA ESPECÍFICOS]
**Salida Esperada:** [RESULTADO ESPERADO]
**Salida Real:** [RESULTADO OBTENIDO]
**Estado:** [PENDIENTE/COMPLETADO/FALLIDO]
**Severidad:** [BAJA/MEDIA/ALTA/CRÍTICA]

**Precondiciones:** [CONDICIONES NECESARIAS]
**Postcondiciones:** [ESTADO DESPUÉS DE LA PRUEBA]
**Observaciones:** [NOTAS ADICIONALES]

**🔧 Procedimiento paso a paso:**
1. [PASO 1]
2. [PASO 2]
3. [PASO 3]
...
```
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

## 📞 **Contacto**

**Profesora:** Romina Torres  
**Asignatura:** TICS00866 - Auditoría y Defensa en Sistemas de Inteligencia Artificial  
**Institución:** Universidad Adolfo Ibáñez

---

**¡Éxito en el taller!** 🎯