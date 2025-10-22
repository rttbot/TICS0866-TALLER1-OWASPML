LLM03 — Cadena de suministro (Línea 2: Generación de reportes clínicos, MedVQA-AI)

Basado en: OWASP Top 10 LLM 2025 (LLM03) y tu proyecto MedVQA-AI (Línea 2)

proyecto (1)

.

1) Descripción (simple y directa)

La vulnerabilidad LLM03 – Cadena de suministro ocurre cuando modelos, datasets o dependencias de terceros (p. ej., pesos descargados, librerías o APIs) se comprometen o alteran antes de integrarse. En generación de reportes clínicos, esto puede producir informes incorrectos, sesgados o maliciosamente manipulados que afectan decisiones médicas.

2) Ambiente de pruebas

Herramientas

Python 3.10; PyTorch; transformers (Hugging Face)

pip-audit / safety (dependencias)

Docker (entorno aislado), Git (versionado), MLflow (registry)

MITRE ATLAS Evaluator / OWASP AI Testing Toolkit

Burp Suite / mitmproxy (tráfico a repos/repositorios)

Datasets

Medical-CXR-VQA (base) + snapshot verificado (hash)

“Dataset control” (golden) y “dataset contaminado” (inyectado para test)

Configuración

Red sin salida a internet en runtime

Verificación de SHA256 de pesos/modelos y datasets antes de cargar

Logging de integridad + Modo auditoría activado

3) Identificación del sistema (componente afectado)

Language Branch + Generative Language Model (módulo que redacta reportes clínicos a partir de la fusión multimodal). 

proyecto (1)

4) Arquitectura (relación con MedVQA-AI)

El Language Branch (LLM) recibe señales del Fusion Layer (hallazgos de imagen + contexto) y genera el reporte clínico.

Si el modelo/dataset o la lib usada en el LLM está comprometida, el OUTPUT (reporte clínico) queda afectado directamente. 

proyecto (1)

5) Impacto

Clínico: falsos positivos/negativos, recomendaciones erróneas.

Legal/compliance: riesgo HIPAA/GDPR por datos/artefactos no confiables.

Operacional: pérdida de confianza, auditorías y retrasos.

Reputacional: descrédito institucional por informes fallidos.

6) Mitigaciones (concretas)

Verificación de integridad: firmar y verificar hashes/firma de modelos, datasets y contenedores antes de usarlos.

Model registry interno (MLflow/Artifactory): only-from-registry en CI/CD; prohibido descargar pesos en producción.

SBOM y auditoría de dependencias: pip-audit, safety, SCA en pipeline; pinning de versiones.

Entornos reproducibles: Docker inmutable; no internet en runtime; builder separate.

Re-entrenamiento/fine-tuning local controlado: datasets validados y curados.

Monitoreo de drift y outputs: detección de desviaciones semánticas en reportes; circuit breakers.

Política de orígenes confiables: lista blanca de repositorios; revisión manual de nuevos modelos.

7) Mapeo a MITRE ATLAS (equivalentes)
ATLAS ID	Técnica	Aplicación en este caso
AML.T0024.002	Extract ML Model	Sustitución/robo y reempaque del modelo en la cadena.
AML.T0024.001	Invert ML Model	Inversión para insertar backdoors/behaviors.
AML.T0024.000	Infer Training Data Membership	Fugas al usar artefactos contaminados.
(rel.) Supply-chain/Artifact Tampering	Compromiso de repos/artefactos	Envenenamiento de pesos/checkpoints.
8) Casos de prueba (≥3)

TST-001 — Integridad de modelo

Paso: calcular SHA256 de model.safetensors y comparar con hash esperado.

Criterio: Mismatch ⇒ bloquear carga y alertar.

TST-002 — Auditoría de dependencias

Paso: ejecutar pip-audit/safety sobre requirements.txt.

Criterio: CVEs críticas ⇒ fallar build en CI.

TST-003 — Detección de comportamiento anómalo

Paso: generar N=100 reportes con prompts clínicos estándar (corpus control).

Métrica: distancia semántica/consistencia estructural vs golden outputs.

Criterio: desviación > umbral ⇒ marcar posible contaminación.

TST-004 — Dataset contaminado (controlado)

Paso: introducir 5% de QA adversaria y re-entrenar LoRA/PEFT.

Criterio: incremento de falsas negativas o sesgos clínicos ⇒ confirmación de riesgo.

TST-005 — Política de orígenes

Paso: intentar cargar pesos desde una URL no permitida.

Criterio: denegar por política (allow-list only).

9) Herramientas de automatización

CI/CD (GitHub Actions/GitLab CI): jobs de hash-check, SBOM y SCA.

MLflow Model Registry / Artifactory: gates de promoción (staging → prod).

Pre-commit hooks: bloqueo de cambios no versionados en models/ y requirements.txt.

Prometheus + reglas de calidad de salida: alertas por drift/anomalías.

10) Datos de ejemplo (I/O)

Input (ejemplo)

{
  "imagen": "CXR_1023.dcm",
  "consulta": "¿El paciente presenta signos de neumonía?"
}


Salida esperada (modelo íntegro)

{
  "diagnostico": "Opacidades bilaterales compatibles con neumonía.",
  "nivel_confianza": 0.92,
  "recomendacion": "Confirmar con TC y evaluación clínica."
}


Salida alterada (modelo contaminado)

{
  "diagnostico": "Radiografía normal, sin hallazgos.",
  "nivel_confianza": 0.95,
  "recomendacion": "Ninguna acción necesaria."
}
