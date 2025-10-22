# LLM06 – Agencia Excesiva 
Maximiliano Saenz

Esta ocurre cuando un modelo de lenguaje o agente conectado al sistema tiene demasiada autonomía para ejecutar acciones sin la supervisión o validación humana necesaria.
En el contexto de MedVQA-AI, esto se traduce en que el modelo podría emitir reportes clínicos, guardar resultados o enviar recomendaciones médicas automáticas sin confirmación del profesional tratante, lo que introduce riesgos éticos, legales y clínicos.

<img width="721" height="745" alt="MedVQ - visual selection" src="https://github.com/user-attachments/assets/be6f8555-c7a4-4a66-afeb-7ec5fa245960" />

# Ambiente de pruebas

Entorno: entorno de validación hospitalaria (Python, PyTorch, Docker, API REST).

Herramientas: Hugging Face Transformers, FastAPI, SQLite local para auditoría.

Dataset: CXR-VQA (radiografías de tórax con preguntas/respuestas clínicas).

Configuración: el modelo responde preguntas clínicas y genera reportes automáticos vía API interna.

Permisos simulados: ejecución de acciones (guardar reporte, enviar correo, marcar alta médica).
