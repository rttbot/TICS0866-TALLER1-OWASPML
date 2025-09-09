# Informe Ejecutivo Final – Taller IA Security Village

## 1. Resumen Ejecutivo de Hallazgos
Durante el desarrollo del taller se analizaron vulnerabilidades asociadas a la cadena de suministro en proyectos de IA (ML06:2023 Supply Chain Attacks).  
Se documentaron **casos de prueba de pentesting** y se identificaron **riesgos críticos** en dependencias externas, datasets abiertos y modelos pre-entrenados.  
Los principales hallazgos son:
- La falta de control de integridad en dependencias permite ataques de tipo *dependency confusion*.  
- La ausencia de validación de datasets abre la puerta a *data poisoning*, que degrada el desempeño del modelo.  
- La descarga de modelos sin firmas digitales expone a *model tampering* y backdoors ocultos.  

---

## 2. Análisis de Vulnerabilidades Encontradas
Las vulnerabilidades analizadas se clasifican en tres grandes categorías:
1. **Dependencias externas inseguras**: riesgo de instalar paquetes alterados desde registros públicos sin validación.  
2. **Datasets contaminados**: envenenamiento de datos que introduce sesgos y errores en los modelos.  
3. **Modelos pre-entrenados adulterados**: inclusión de modelos con backdoors desde repositorios no verificados.  

**Impacto potencial:**
- Pérdida de integridad del pipeline de IA.  
- Incremento de *false negatives* en sistemas de detección.  
- Riesgo de ejecución de código malicioso en ambientes críticos.  

**Controles sugeridos:**
- Uso de **SBOM** y bloqueo de versiones.  
- Validación de datasets mediante **hashes y QA estadístico**.  
- Firmado digital de modelos y dependencias.  
- Monitoreo continuo y auditoría en pipelines de CI/CD.  

---

## 3. Comparación con el Estudio de México
El **estudio de México sobre ciberseguridad en IA** resalta desafíos similares:
- Dependencia de datasets abiertos sin validación.  
- Falta de mecanismos regulatorios claros sobre modelos importados.  
- Ausencia de cultura de auditoría en la cadena de suministro digital.  

**Similitudes con nuestros hallazgos:**
- Ambos reconocen la vulnerabilidad de los modelos a ataques de *data poisoning* y *model tampering*.  
- Se destaca la carencia de controles de integridad en los procesos de desarrollo y despliegue.  

**Diferencias:**
- El estudio de México enfatiza el contexto **regulatorio y de políticas públicas**, mientras nuestro taller se centró en un enfoque **práctico y técnico**.  
- En México se observa mayor preocupación por la falta de capacidades locales en ciberseguridad para IA, mientras que nuestro análisis se enfocó en riesgos específicos en el pipeline técnico.  

---

## 4. Conclusiones y Lecciones Aprendidas
- La seguridad en IA no depende solo del modelo, sino de toda la **cadena de suministro digital** (datos, dependencias, infraestructura).  
- Los **ataques a la cadena de suministro (ML06/ATLAS)** son altamente factibles y pueden ejecutarse con bajo esfuerzo técnico si no existen controles de verificación.  
- La **integración de buenas prácticas de seguridad desde el inicio** (DevSecOps, MLOps seguro) es clave para reducir riesgos.  
- La comparación internacional muestra que, más allá del plano técnico, es necesario fortalecer la **gobernanza y regulación** para proteger ecosistemas de IA.  

**Lecciones aprendidas para proyectos futuros:**
1. Siempre validar y versionar datasets, dependencias y modelos.  
2. Incluir auditorías automáticas en pipelines (CI/CD con *fail the build* ante anomalías).  
3. Adoptar SBOM y firmas digitales como estándar mínimo.  
4. Promover la capacitación continua en ciberseguridad aplicada a IA.  

---

