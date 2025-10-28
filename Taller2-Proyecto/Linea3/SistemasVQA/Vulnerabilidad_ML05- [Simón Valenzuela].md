# Vulnerabilidad – ML05: Model Theft (Model Extraction Attack)
**Línea 3** Sistemas VQA (MedVQA-AI)  
**Componente foco:** Multimodal Fusion + Output Layer  
**Autor:** Simón Valenzuela

---

## 1. Resumen / Definición
Un **Model Theft / Model Extraction** ocurre cuando un atacante **clona** el comportamiento del modelo consultando la API repetidamente y usando los pares **(imagen+pregunta → respuesta)** para entrenar un **modelo sustituto (proxy)**. En MedVQA-AI esto roba **propiedad intelectual**, expone **conocimiento clínico** y permite usos no autorizados.

---

## 2. Por qué es relevante para MedVQA-AI (contexto)
MedVQA-AI fusiona representaciones **visuales** (Rx tórax) y **lingüísticas** (preguntas clínicas). Una API sin controles finos habilita **scraping sistemático** de pares entrada-salida con los que se puede entrenar un proxy de **alta fidelidad**.

---

## 3. Equivalente / mapeo en MITRE ATLAS
- **Táctica ATLAS:** *Exfiltration*  
- **Técnica ATLAS:** **Steal Machine Learning Model**  
Objetivo común: **replicar** el modelo desde su interfaz mediante consultas y análisis de respuestas.

---

## 4. Ambiente de pruebas (diseño, no ejecución)
- **Sandbox controlado**: instancia limitada, dataset anónimo (subset **Medical-CXR-VQA**).  
- **API simulada:** `POST /vqa/infer` (imagen + pregunta → texto).  
- **Logs estructurados:** `user_key, ts, img_id, prompt_id, tokens_in/out, latency_ms`.  
- **Permisos:** autorización escrita para pruebas éticas.

---

## 5. Componentes afectados
- **Multimodal Fusion:** embeddings visual-textuales pueden filtrarse por la **estabilidad** de las respuestas.  
- **Output Layer (Generative):** el **texto** devuelto es materia prima para entrenar el proxy.  
- **API Interface:** sin cuotas ni detección de scraping facilita la **exfiltración** masiva.

---

## 6. Impacto (qué puede pasar) — con métricas operativas

> **Supuestos de referencia (baseline del servicio):**  
> - Longitud típica de pregunta: **≤ 120 tokens**; salida clínica: **≤ 300 tokens**.  
> - Calidad interna (QA validado): **BLEU ≥ 0.35**, **ROUGE-L ≥ 0.42**, **BERTScore F1 ≥ 0.84**.  
> - Tráfico legítimo por API-key: **≤ 3.000** requests/día; diversidad léxica (TTR) **≥ 0.60**.

**Indicadores de impacto por extracción:**
- **Fidelidad del proxy (Clonación):** **BERTScore F1 ≥ 0.90** y **ROUGE-L ≥ 0.50** vs. original en 200 ejemplos ciegos ⇒ **clonación efectiva**.  
- **Eficiencia de extracción:** alcanzar esa fidelidad con **≤ 50.000** consultas totales ⇒ **alto riesgo** (costo bajo de robo).  
- **Acuerdo de decisión clínica:** concordancia (exact match o sí/no) **≥ 85%** en preguntas diagnósticas ⇒ riesgo de **sustitución** del servicio.  
- **Leak de estilo/conocimiento:** divergencia de n-gramas **JSD ≤ 0.10** entre original y proxy ⇒ réplica de **estilo/contenido**.  
- **Exposición económica:** si el costo de consultar **N** veces al original < **20%** del costo de entrenar el proxy a esa fidelidad ⇒ **ventaja económica** para el atacante.

> **Criterio de “robo materializado”:** **Model Theft** si **(BERTScore ≥ 0.90 AND ROUGE-L ≥ 0.50 AND acuerdo clínico ≥ 85%)** con **≤ 50k** consultas.

---

## 7. Casos de prueba (mínimo 3) — con métricas esperadas

### Caso A — Enumeración sistemática (black-box)
**Objetivo:** medir **predecibilidad** y **estabilidad** ante variaciones ligeras.  
**Procedimiento:** 20 Rx base × 10 variantes de pregunta (sinónimos/orden) → registrar salidas → comparar intra-grupo.  
**Métricas/umbrales:**  
- **Consistencia semántica intra-grupo:** **BERTScore F1 ≥ 0.92** (media) → alto riesgo de extracción.  
- **Varianza léxica:** **TTR ≤ 0.45** → estilo fácilmente emulable.  
- **Redundancia n-gram:** **overlap ≥ 70%** entre variantes → baja entropía expresiva.

### Caso B — Entrenar proxy con pares recolectados
**Objetivo:** evaluar **fidelidad** del sustituto.  
**Procedimiento:** recolectar **30k–50k** pares (sandbox) → entrenar **Transformer pequeño** (110–150M) con **label distillation** → evaluar en 200 ejemplos ciegos.  
**Métricas/umbrales:**  
- **BERTScore F1 ≥ 0.90**, **ROUGE-L ≥ 0.50**, **Exact-match ≥ 60%** → **clonación efectiva**.  
- **Curva datos→fidelidad:** con **≤ 30k** pares, BERTScore **≥ 0.88** ⇒ riesgo **muy alto**.

### Caso C — Extracción de señales auxiliares
**Objetivo:** detectar exposición de **metadata** (logits, atenciones, razones).  
**Procedimiento:** consultas con `debug=true` o flags similares → inspección JSON.  
**Métricas/umbrales:**  
- Presencia de **logits/probabilidades/token-scores** o **vectores de atención** ⇒ **crítico** (acelera extracción en ≥ 30%).  
- **Correlación** de “importancias” con decisiones **r ≥ 0.6** ⇒ aporta señal entrenable.

> **Evidencia esperada:** tablas de similitud, curvas datos→fidelidad, histogramas n-gram, ejemplos cualitativos (original vs. proxy).

---

## 8. Herramientas / técnicas recomendadas
- **ART (Adversarial Robustness Toolbox)** – `model extraction`.  
- **TextAttack** – variantes semánticas de preguntas.  
- **Transformers/PEFT** – entrenamiento proxy eficiente.  
- **Pandas/Matplotlib** – análisis (BERTScore, ROUGE-L, n-gram overlap).  
- **Burp Suite / OWASP ZAP** – inspección de headers/respuestas para metadata.

---

## 9. Mitigaciones específicas y verificables (no genéricas)

**A. Cuotas por cómputo e información (budget híbrido)**  
- **Reglas:** `max_requests_per_api_key_per_day = 2.000`; `max_tokens_out_per_day = 300.000`; `max_pairs_per_week = 8.000`.  
- **KPI:** reducción **≥ 60%** de tasa de recolección sostenida sin afectar al **95%** de usuarios legítimos.

**B. Paráfrasis estocástica (shaping de salida) con plantillas múltiples**  
- **Mecánica:** mantener **contenido clínico** pero variar superficie (temperatura/top-p y **≥ 8** plantillas).  
- **KPI:** **n-gram overlap** proxy-vs-original **< 55%** a igual fidelidad; **JSD > 0.15**.

**C. Cerrar señales internas y discretizar confidencias**  
- **Prohibido:** logits, atenciones, razonamientos token-a-token.  
- **Si hay confidencias:** devolver **bandas** {baja, media, alta}.  
- **KPI:** necesidad de **≥ 2×** pares para lograr BERTScore 0.90.

**D. Detección de scraping (patrones de extracción)**  
- **Features:** TTR bajo, ritmo 24/7, sinónimos alternados, secuencias templadas.  
- **Acción:** **CAPTCHA** adaptativo, **cooldown** por API-key/IP.  
- **KPI:** caída **≥ 50%** del caudal sospechoso; **FPR ≤ 3%**.

**E. Watermarking semántico de salida**  
- Marcas ligeras en giros/plantillas para **detectar** uso en datasets externos.  
- **KPI:** detección con **recall ≥ 0.8** y **precision ≥ 0.9**.

**F. Canary prompts & honeypots**  
- Prompts canario con **respuestas traza** (sin PHI); si aparecen en datasets públicos ⇒ evidencia de extracción.  
- **KPI:** tiempo a detección **≤ 7 días**; rotación periódica de plantillas.

**G. Respuesta escalonada (graduated response)**  
- **Nivel 0:** salida completa. **Nivel 1 (sospecha):** **resumen ≤ 180 tokens** + paráfrasis fuerte. **Nivel 2 (alto):** **≤ 60 tokens** + aviso.  
- **KPI:** en Nivel 2, caída **≥ 0.05** en BERTScore alcanzable por el proxy con mismo presupuesto.

**H. Límite de cobertura visual**  
- **Regla:** solo **1 imagen** por request; rechazar barridos de múltiples Rx con misma pregunta.  
- **KPI:** reduce **≥ 40%** la ganancia marginal por consulta para extracción.

> **Nota ética:** nada de “poison responses” en producción; usar **degradación de estilo** y **cuotas**. Poison solo en **sandbox**.

---

## 10. Métricas de evaluación (criterios de aceptación de mitigaciones)
- **Con cuotas (A):** atacante no supera **15k** pares/semana sin bloqueo; usuarios legítimos (p95) no tocan límites.  
- **Con paráfrasis (B):** para BERTScore 0.90, el proxy necesita **≥ 1.7×** datos que sin paráfrasis.  
- **Sin señales (C):** pares necesarios para ROUGE-L 0.50 se **duplican**.  
- **Scraping detection (D):** reduce **≥ 50%** el caudal sospechoso (**FPR ≤ 3%**).  
- **Watermarking (E):** detección externa con **F1 ≥ 0.85**.  
- **Graduated response (G):** usuarios legítimos mantienen **acuerdo clínico ≥ 90%**, mientras cae **≥ 20%** el overlap n-gram del atacante.

---

## 11. Consideraciones éticas y regulatorias
- No retornar información sensible ni trazas internas.  
- Solo diseñar/documentar pruebas en **sandbox** con permisos.  
- No alterar contenido clínico crítico; priorizar **variación de estilo** y **cuotas**.  
- Reportar hallazgos y activar **plan de hardening** con evidencia de métricas.

---

> **Nota de alcance (anti-genérico):** Mitigaciones con **umbrales, KPIs y criterios de éxito**. Se descartan recomendaciones vagas sin **métrica verificable**.


**Diagrama:**
<img width="780" height="648" alt="Proceso de Extracción de Modelo — Vulnerabilidad ML05 (MedVQA-AI)" src="https://raw.githubusercontent.com/rttbot/TICS0866-TALLER1-OWASPML/main/Taller2-Proyecto/Linea3/SistemasVQA/image.png" />
