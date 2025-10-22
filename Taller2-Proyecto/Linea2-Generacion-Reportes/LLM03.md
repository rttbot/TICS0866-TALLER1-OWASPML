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
