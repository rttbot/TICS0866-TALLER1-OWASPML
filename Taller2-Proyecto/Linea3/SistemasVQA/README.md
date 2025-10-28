# Auditoría de Seguridad - Línea 3: Sistemas de Pregunta-Respuesta (VQA)

## Descripción de la Línea Asignada

La **Línea 3** se centra en auditar la seguridad de la **capa de interacción multimodal** (Visión + Lenguaje) y la **interfaz de inferencia (API)** del sistema MedVQA-AI.

A diferencia de otras líneas que se enfocan en la manipulación de la entrada visual, nuestro objetivo es evaluar los riesgos asociados a la **exposición del modelo** como un servicio. El análisis se enfoca en cómo un atacante puede explotar el *endpoint* de VQA para comprometer:
* **La Propiedad Intelectual** (Robo del modelo).
* **La Confidencialidad** (Fuga de datos de entrenamiento).
* **La Disponibilidad** (Denegación de servicio clínico).

---

## Componente de la arquitectura relevantes para la linea

* **API Interface / Endpoint de Inferencia:**
    * **LLM04 (Denial of Service):** Es el vector de ataque principal para la saturación de recursos (consultas concurrentes masivas).
    * **ML05 (Model Theft):** Es el punto de recolección de datos (consultas masivas) para entrenar el modelo sustituto.
    * **ML03 (Model Inversion):** Es el punto de consulta para la exfiltración de información estadística.

* **Language Branch (LLM):**
    * **LLM04 (Denial of Service):** Es el componente que se satura, consumiendo GPU/TPU al procesar *prompts* largos.

* **Multimodal Fusion:**
    * **ML05 (Model Theft):** Es el "cerebro" del razonamiento clínico. Su comportamiento (la lógica de fusión) es el objetivo a replicar (robar).

* **Output Layer (Generative):**
    * **ML03 (Model Inversion):** Es la fuente de la fuga de datos. Las respuestas y *scores* que genera se utilizan para inferir los datos de entrenamiento.

## Amenazas y Vulnerabilidades Analizadas

Nuestro equipo diseñó casos de prueba para las siguientes tres amenazas críticas, adaptadas al contexto VQA de MedVQA-AI:

1.  **ML05 - Model Theft (Robo de Modelo):**
    * **Riesgo:** Clonación de la lógica de razonamiento clínico del sistema a través de consultas masivas a la API para entrenar un modelo sustituto.
    * **Impacto:** Pérdida de propiedad intelectual.

2.  **ML03 - Model Inversion (Inversión de Modelo):**
    * **Riesgo:** Reconstrucción de información sensible o arquetipos de imágenes del *dataset* de entrenamiento (potencialmente PHI) explotando las salidas y *scores* del modelo.
    * **Impacto:** Fuga de datos confidenciales.

3.  **LLM04 - Denial of Service (DoS):**
    * **Riesgo:** Saturación del *Language Branch* (LLM) mediante *prompts* excesivamente largos o consultas concurrentes masivas.
    * **Impacto:** Paralización del servicio de diagnóstico, afectando la disponibilidad clínica.

---

## Enfoque de la Auditoría (Diseño de Pruebas)

Nuestra metodología se centró en el diseño conceptual de pruebas de *testing* en la capa de inferencia (API), simulando un atacante *Black-Box* o *Gray-Box*:

* **Pruebas de Exfiltración (ML03, ML05):** Diseño de estrategias de consulta (Query-based attacks) para medir la facilidad de replicación del modelo (Robo) y la fuga de información estadística (Inversión).
* **Pruebas de Disponibilidad (LLM04):** Diseño de pruebas de carga y estrés (usando Locust/JMeter conceptualmente) para medir el impacto de *prompts* largos y alta concurrencia en la latencia de la API.
* **Análisis de Impacto:** Evaluación del riesgo clínico (fuga de PHI, indisponibilidad del servicio) y mapeo de amenazas con **MITRE ATLAS**.
```mermaid
graph TD;
    
    %% --- NODOS PRINCIPALES ---
    A[Atacante Externo]
    
    subgraph Sistema MedVQA-AI
        direction LR
        API[API Interface]
        LB["Language Branch (LLM)"]
        VB[Vision Branch]
        MF[Multimodal Fusion]
        OL["Output Layer (Generative)"]
        
        API --> VB
        API --> LB
        VB --> MF
        LB --> MF
        MF --> OL
    end

    %% --- NODOS DE ATAQUE (AMARILLO) ---
    LLM04["<b>LLM04: Denial of Service</b><br/>(Saturación de API)"]
    ML03["<b>ML03: Model Inversion</b><br/>(Fuga de datos de entrenamiento)"]
    ML05["<b>ML05: Model Theft</b><br/>(Robo de propiedad intelectual)"]
    
    %% --- FLECHAS DE ATAQUE ---
    A --> LLM04
    LLM04 --> API
    LLM04 --> LB
    
    A --> ML03
    ML03 --> OL
    
    A --> ML05
    ML05 -. "(Colecta Respuestas)" .-> OL
    ML05 --> API

    %% --- DEFINICIÓN DE ESTILOS (CON LETRA NEGRA) ---
    
    %% Estilo Sistema (Azul claro, letra negra)
    classDef default fill:#e6f7ff,stroke:#0056b3,stroke-width:2px,color:#000000;
    
    %% Estilo Atacante (Rojo claro, letra negra)
    classDef estiloAtacante fill:#ffe5e5,stroke:#d4380d,stroke-width:3px,color:#000000;
    class A estiloAtacante;

    %% Estilo Ataques (Amarillo/Alerta, letra negra)
    classDef estiloAtaque fill:#fffbe6,stroke:#d48806,stroke-width:2px,color:#000000;
    class LLM04,ML03,ML05 estiloAtaque;
