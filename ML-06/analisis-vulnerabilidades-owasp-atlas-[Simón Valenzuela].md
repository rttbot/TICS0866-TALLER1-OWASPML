# Parte 2 – Análisis de Vulnerabilidades

## 1. Caso similar en ATLAS: AI Supply Chain Compromise
En el marco ATLAS (AI Threat Landscape Assessment System), existe la categoría **AI Supply Chain Compromise**, que describe ataques donde un adversario manipula los componentes de la cadena de suministro de un sistema de IA.  
Esto incluye:
- Dependencias externas (bibliotecas, paquetes).
- Modelos pre-entrenados de repositorios públicos.
- Datasets abiertos o compartidos.
- Herramientas y pipelines de CI/CD que no cuentan con controles de integridad.

La similitud con **OWASP ML06:2023 Supply Chain Attacks** es directa: ambas taxonomías reconocen que la cadena de suministro es un eslabón débil y crítico en proyectos de IA.

---

## 2. Comparación OWASP ML Top 10 vs ATLAS ML

| Aspecto | OWASP ML Top 10 (ML06) | ATLAS (AI Supply Chain Compromise) | Similitudes | Diferencias |
|---------|-------------------------|-----------------------------------|-------------|-------------|
| **Enfoque** | Resalta ataques a librerías, datasets y modelos externos sin validación. | Define compromiso de toda la cadena de suministro de IA, incluyendo herramientas de despliegue y CI/CD. | Ambos reconocen riesgos en dependencias externas y modelos pre-entrenados. | OWASP se enfoca más en los **vectores técnicos** (datasets, librerías, modelos). ATLAS expande el alcance a **infraestructura y operaciones**. |
| **Mitigación** | Uso de checksums, firmas digitales, verificación de integridad y auditoría de dependencias. | Políticas de adquisición seguras, SBOM (Software Bill of Materials), control de versiones y monitoreo de terceros. | Ambos proponen **validación de integridad** y **versionado estricto**. | ATLAS enfatiza la **gestión organizacional** y **gobernanza de proveedores**. |
| **Ejemplos de ataque** | Dependency confusion, data poisoning en dataset público, modelo trojanizado. | Inserción de componentes alterados en CI/CD, manipulación de repositorios de modelos, adulteración de anotaciones humanas. | Coinciden en dependency hijacking y modelo adulterado. | ATLAS incluye más escenarios **organizacionales** (compromiso de proveedores, insider threats). |

---

## 3. Análisis de similitudes y diferencias

- **Similitudes**  
  - Ambos frameworks reconocen que la IA depende de un ecosistema externo (datos, librerías, modelos).  
  - Señalan el riesgo de **incluir dependencias sin verificación**.  
  - Consideran que la manipulación puede tener efectos silenciosos y devastadores en producción.  

- **Diferencias**  
  - **OWASP ML06**: se centra en los **vectores técnicos** (ej.: instalar un paquete alterado, dataset contaminado, modelo trojanizado).  
  - **ATLAS AI Supply Chain Compromise**: incluye también **factores humanos y organizacionales** (ej.: compromiso del proveedor, falta de auditoría de terceros, manipulación en anotaciones).  
  - ATLAS ofrece un marco más amplio para la gestión del riesgo; OWASP es más directo para pruebas técnicas de pentesting.  

---

## 4. Casos de aplicación específicos a Intrusion.Aware

**Intrusion.Aware** (hipotético sistema de detección de intrusiones basado en IA) puede verse afectado de la siguiente forma:

1. **Paquetes de librerías (dependency hijacking)**  
   - Si Intrusion.Aware usa librerías de Python para procesamiento de logs (ej. `numpy`, `pandas`), un atacante podría publicar una versión maliciosa sin pinning.  
   - **Impacto esperado:** ejecución de código malicioso en el entorno de detección.  

2. **Dataset de entrenamiento envenenado (data poisoning)**  
   - Intrusion.Aware depende de datasets de tráfico de red. Si un atacante manipula etiquetas (ej. tráfico malicioso etiquetado como benigno), el sistema no detectará ciertos ataques.  
   - **Impacto esperado:** tasa elevada de *false negatives*.  

3. **Modelos pre-entrenados adulterados**  
   - Si el equipo integra un modelo de clasificación de intrusiones descargado de un repositorio público, este podría contener un backdoor.  
   - **Impacto esperado:** el IDS funciona correctamente en general, pero ignora ataques con un trigger específico.  

**Medidas recomendadas para Intrusion.Aware**:  
- Implementar **SBOM** (lista completa de dependencias y versiones).  
- Validar datasets con **hashes y QA estadístico**.  
- Firmar modelos y verificarlos antes de usarlos.  
- Integrar auditorías automáticas de dependencias en el pipeline de CI/CD.  

---

# Resumen
- **ATLAS y OWASP** coinciden en la importancia de la cadena de suministro.  
- **OWASP ML06** → visión técnica y puntual.  
- **ATLAS Supply Chain Compromise** → visión más amplia e incluye gobernanza.  
- Para **Intrusion.Aware**, los riesgos principales son: *dependency hijacking*, *data poisoning* y *model tampering*, todos mitigables con controles de integridad, auditoría y firmas digitales.
