
# Línea 2: Generación de Reportes Clínicos

## Descripción específica de la línea asignada

Esta línea se enfoca en auditar la seguridad del componente de generación automática de reportes médicos del sistema **MedVQA-AI**, el cual utiliza modelos de lenguaje generativo (LLMs) para producir reportes clínicos a partir de imágenes y preguntas médicas. Dado que estos reportes pueden influir directamente en diagnósticos y tratamientos, su integridad y confiabilidad son críticas para la seguridad del paciente y la calidad de la atención clínica.

##  Componentes arquitectónicos relevantes

- **Language Branch**: Procesa las consultas médicas en lenguaje natural.
- **Generative Language Model**: Genera el texto del reporte clínico final.
- Estos componentes son los más expuestos a entradas no controladas y, por tanto, representan puntos críticos para la auditoría.

##  Vulnerabilidades a analizar

Basado en el OWASP LLM Top 10, se identificaron las siguientes vulnerabilidades críticas:

- **LLM01 – Prompt Injection**: Manipulación de entradas para alterar diagnósticos.
