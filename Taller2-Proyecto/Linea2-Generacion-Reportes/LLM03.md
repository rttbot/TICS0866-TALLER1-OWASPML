# LLM03 Cadena de suministros
Maximiliano Saenz

La vulnerabilidad LLM03 – Cadena de suministro ocurre cuando modelos, datasets o dependencias de terceros (p. ej., pesos descargados, librerías o APIs) se comprometen o alteran antes de integrarse. En generación de reportes clínicos, esto puede producir informes incorrectos, sesgados o maliciosamente manipulados que afectan decisiones médicas.
<img width="888" height="576" alt="MedQVA - visual selection" src="https://github.com/user-attachments/assets/f89b7ce0-8d9e-4f77-9a1f-2c2285fcf775" />

# Ambiente de pruebas
**Herramientas:**
Python 3.10; PyTorch; transformers (Hugging Face)

pip-audit / safety (dependencias) 

Docker (entorno aislado), Git (versionado), MLflow (registry)

MITRE ATLAS Evaluator / OWASP AI Testing Toolkit

Burp Suite / mitmproxy (tráfico a repos/repositorios)
 
 **Datasets:**

 Medical-CXR-VQA (base) + snapshot verificado (hash)
 
 “Dataset control” (golden) y “dataset contaminado” (inyectado para test)
 
 **Configuración:**

 Red sin salida a internet en runtime
 
 Verificación de SHA256 de pesos/modelos y datasets antes de cargar
 
 Logging de integridad + Modo auditoría activado

# Identificación del sistema (componente afectada)
 Language Branch + Generative Language Model (módulo que redacta reportes clínicos a partir de la fusión multimodal).

# Arquitectura (relación con MedVQA-AI)
El Language Branch (LLM) recibe señales del Fusion Layer (hallazgos de imagen + contexto) y genera el reporte clínico.

Si el modelo/dataset o la lib usada en el LLM está comprometida, el OUTPUT (reporte clínico) queda afectado directamente.

# Impacto de la vulnerabilidad
Clínico: falsos positivos/negativos, recomendaciones erróneas.

Legal/compliance: riesgo HIPAA/GDPR por datos/artefactos no confiables.

Operacional: pérdida de confianza, auditorías y retrasos.

Reputacional: descrédito institucional por informes fallidos.

# Mitigaciones 

Verificación de integridad: firmar y verificar hashes/firma de modelos, datasets y contenedores antes de usarlos.

Model registry interno (MLflow/Artifactory): only-from-registry en CI/CD; prohibido descargar pesos en producción.

SBOM y auditoría de dependencias: pip-audit, safety, SCA en pipeline; pinning de versiones.

Entornos reproducibles: Docker inmutable; no internet en runtime; builder separate.

Re-entrenamiento/fine-tuning local controlado: datasets validados y curados.

Monitoreo de drift y outputs: detección de desviaciones semánticas en reportes; circuit breakers.

Política de orígenes confiables: lista blanca de repositorios; revisión manual de nuevos modelos.

# Equivalente en MITRE ATLAS
Esta vulnerabilidad se relaciona con las técnicas de MITRE ATLAS que describen ataques a la cadena de suministro de modelos de IA.
Corresponde principalmente a AML.T0024.002 (Extract ML Model), que implica el robo o reemplazo del modelo por una versión alterada; AML.T0024.001 (Invert ML Model), que permite modificar el comportamiento interno del modelo introduciendo sesgos o backdoors; y AML.T0024.000 (Infer Training Data Membership), que facilita descubrir datos usados en el entrenamiento, exponiendo información sensible.
En conjunto, estas técnicas reflejan cómo un atacante puede comprometer la integridad y confiabilidad del sistema MedVQA-AI manipulando modelos o datasets durante su distribución o integración.
