
# Vulnerabilidad – ML03: Model Inversion Attack
**Línea 3** Sistemas VQA (MedVQA-AI)  
Componente foco: Multimodal Fusion + Output Layer  
Autor: Julian Contreras

---

## 1. Resumen / Definición
Un **Model Inversion Attack** ocurre cuando un adversario, explotando las salidas del modelo (etiquetas, probabilidades, embeddings o texto generado), logra inferir o reconstruir información sensible presente en el conjunto de entrenamiento. En un sistema VQA médico esto puede traducirse en la recuperación parcial de imágenes de entrenamiento, diagnósticos frecuentes o rasgos clínicos que permitan identificar pacientes o condiciones clínicamente sensibles.  
Este documento aplica el concepto al sistema MedVQA-AI, centrado en la fusión multimodal (imagen + pregunta) y la capa de salida textual. 

---

## 2. Por qué es relevante para MedVQA-AI (contexto)
- MedVQA-AI toma radiografías de tórax y preguntas clínicas, fusiona ambas representaciones y genera respuestas en lenguaje natural. La salida puede incluir texto diagnóstico, métricas y explicaciones. Un atacante que interactúe con el endpoint de inferencia podría, mediante consultas diseñadas, extraer información estadística o reconstrucciones aproximadas de imágenes del dataset (PHI). El alcance y la metodología del taller recomiendan únicamente diseñar casos de prueba (no ejecutar explotación en sistemas reales). 

---

## 3. Equivalente / mapeo en MITRE ATLAS
**Técnica ATLAS:** *Invert ML Model* (táctica: *Exfiltration*).  
La justificación se basa en que ambos marcos describen la extracción de información del dataset de entrenamiento a partir de las respuestas del modelo; ATLAS la modela como técnica dentro de un flujo de ataque estructurado (reconocimiento → acceso → extracción). 
---

## 4. Ambiente de pruebas (diseño, no ejecución)
- **Sandbox controlado** con una instancia de MedVQA-AI (o un modelo proxy entrenado con un subset anónimo del Medical-CXR-VQA dataset).  
- **Inputs aceptados:** DICOM/JPEG para imágenes y texto libre para preguntas.  
- **Permisos:** autorización escrita para testing (hacking ético).  
- **Registros:** logging de consultas, respuestas y metadata (IPs, timestamps).  
- **Datasets de soporte:** Medical-CXR-VQA (solo ejemplo/anonimizado) para construir hipótesis. 

---

## 5. Componentes afectados
- **Multimodal Fusion:** donde se combinan embeddings visuales y textuales — posible punto de fuga si embeddings son expuestos o si la fusión codifica información identificable.  
- **Output Layer (generative / textual):** devuelve explicaciones o reportes; si las respuestas incluyen ejemplos o descripciones que reflejen registros concretos, facilitan la inversión.

---

## 6. Impacto (qué puede pasar)
- **Confidencialidad:** exposición de PHI (p. ej., reconstrucción parcial de radiografías o patrones clínicos asociados a pacientes).  
- **Integridad / Seguridad Clínica:** un atacante que conozca perfiles de casos puede diseñar inputs para evadir detección o manipular diagnósticos.  
- **Reputacional / Legal:** incumplimiento de regulaciones (como por ejemplo GDPR/HIPAA / normativa local) por fuga de datos. 
---

## 7. Casos de prueba (mínimo 3) — diseño paso a paso (NO ejecutar)
> Nota: todos los casos son conceptuales; la entrega exige diseño claro y metodológico, junto con evidencia esperada (logs, tablas, análisis). No realizar ataques en entornos sin permiso.

### Caso A — Reconstrucción por optimización de entradas (Black-box queries)
**Objetivo:** aproximar un “arquetipo” de imagen o vector de features que maximice la probabilidad de una etiqueta/diagnóstico concreto (p. ej., “consolidación pulmonar”).

**Precondiciones:** endpoint público o con acceso a inferencia que devuelva scores/probabilidades por etiqueta (no solo etiqueta final).

**Procedimiento:**
1. Seleccionar diagnóstico objetivo `D` (ej.: “consolidación”).
2. Partir de una imagen base neutral (imagen en blanco/ruido o imagen genérica).
3. Aplicar una rutina iterativa de generación de variantes de imagen (pequeñas modificaciones o parametros del ruido/patch) y enviar cada variante junto a la misma pregunta: “¿Presenta el paciente consolidación pulmonar?”.
4. Registrar la probabilidad/score devuelto para `D` por cada variante.
5. Usar un algoritmo de búsqueda/optimización (grid search, hill-climbing) sobre el espacio de parámetros para maximizar el score de `D`.
6. Sintetizar la variante que produzca mayor score: **vector arquetipo** → analizar si refleja regiones/rasgos clínicos que coinciden con ejemplos del dataset.

**Evidencia esperada:**
- Tabla con iteraciones (imagen_param_set → score_D).  
- Imagen final (variante) y discusión sobre similitudes con casos conocidos.  
- Métrica: aumento relativo del score (%) respecto al baseline.

**Criterio de riesgo:** si la variante produce scores elevados de forma estable, indica que el modelo contiene regiones de memorization explotables.

---

### Caso B — Barridos sobre prompts + preguntas condicionadas (Query-based inversion)
**Objetivo:** explotar la salida textual para inferir atributos del set de entrenamiento (por ejemplo, edad promedio, comorbilidades frecuentes asociadas a cierta etiqueta).

**Precondiciones:** endpoint que devuelve respuestas detalladas (texto explicativo) o probabilidades por sub-clase.

**Procedimiento:**
1. Fijar un set de preguntas tipo (Qbase) diseñadas para sondear información latente:  
   - “¿Cuál es la probabilidad de que un paciente con esta radiografía tenga X?”  
   - “¿Qué factores son más comunes en los casos de Y?”  
2. Enviar Qbase acompañadas de múltiples imágenes (o variantes) que exploren rangos demográficos (si se permite): p. ej., imágenes con metadatos sintéticos (edad simulada) o imágenes alteradas levemente.  
3. Analizar respuestas textuales buscando mención de factores repetidos (p. ej., “frecuentemente asociada con enfermedad Z en adultos mayores”).  
4. Cruce estadístico: mapear respuestas vs. inputs para inferir correlaciones (edad ↔ diagnóstico) que sugieran patrones de entrenamiento.

**Evidencia esperada:**
- Matriz (input_variant × respuesta_texto) con anotaciones de términos detectados.  
- Análisis de frecuencia de términos y su asociación con atributos simulados.  
- Métrica: consistencia (p-valor aproximado o score de asociación).

**Criterio de riesgo:** respuestas que consistentemente reflejan atributos demográficos/clinicos indican fuga de información estadística.

---

### Caso C — Inversión usando embeddings expuestos (White-box / metadata leakage)
**Objetivo:** si el servicio retorna embeddings o si hay endpoints de debug, usarlos para aproximar entradas de entrenamiento.

**Precondiciones:** acceso a embeddings o vectores intermedios (por diseño o por endpoint de debug).

**Procedimiento:**
1. Solicitar/recuperar embeddings para un conjunto de imágenes de prueba (si el endpoint lo permite).  
2. Construir un conjunto de embeddings objetivo que correspondan a la clase `D`.  
3. Aplicar métodos de reconstrucción inversa (ej.: optimización sobre un generador que, partiendo de ruido, produce una imagen cuya embedding se aproxima al target).  
4. Evaluar similitud (cosine similarity) entre embedding reconstruido y embeddings reales.

**Evidencia esperada:**
- Logs de embeddings (hashes/anotaciones), similitud final (cosine) y ejemplos de imágenes reconstruidas (si procede).  
- Métrica: similitud promedio y número de dimensiones explicativas con alta correlación.

**Criterio de riesgo:** alta similitud y reconstrucciones reconocibles implican fuga grave (posible reconstrucción de PHI).

---

## 8. Herramientas / técnicas recomendadas para testing conceptual
- **Para diseño/automatización (no ejecución en producción):** ART (Adversarial Robustness Toolbox), Foolbox, CleverHans (referencia para ataques adversariales y optimización).  
- **Análisis de texto:** tokenización + conteo/TF-IDF para detectar patrones en respuestas.  
- **Simulación de búsquedas:** scripts controlados (Python) para grid search y registro estructurado (CSV/JSON).  
- **Visualización:** matrices de calor, curvas de score vs. iteración.  
  En este caso para el taller, se sugeriría usar datasets como Medical-CXR-VQA para pruebas en sandbox). 

---

## 9. Mitigaciones propuestas (en prioridad)
1. **Restringir salidas:** no devolver probabilidades finas ni embeddings; exponer solo etiquetas o respuestas discretizadas/ruidosas.  
2. **Rate limiting y detección de patrones de consulta:** bloquear enumeraciones y barridos de entradas.  
3. **Ruido/Randomización y Diferential Privacy:** añadir ruido calibrado a outputs o aplicar privacidad diferencial en el entrenamiento.  
4. **No exponer embeddings ni endpoints de debug** en producción.  
5. **Auditoría y registros avanzados:** trazabilidad de consultas sospechosas y revisión humana en outputs con confianza baja.  
6. **Evaluación de memorization antes del despliegue:** tests de inversion/membership para medir riesgo. 

---

## 10. Métricas de evaluación (para el informe)
- **Aumento de score objetivo (%)** tras optimización (Caso A).  
- **Consistencia de términos/atributos** en respuestas (Caso B) — frecuencia relativa.  
- **Similitud de embeddings (cosine)** en Caso C.  
- **Número de queries necesarias** para alcanzar umbrales (indica coste del ataque).  
- **Severidad estimada** (impacto × probabilidad) para priorizar mitigaciones.

---

## 11. Consideraciones éticas y regulatorias
- Solo diseñar y documentar las pruebas; no ejecutar sin autorización.  
- Mantener anonimización de cualquier dato clínico y pedir permisos por escrito.  
- Relacionar hallazgos con obligaciones regulatorias (HIPAA/GDPR / normativa local) y notificar riesgos de divulgación de PHI al equipo de gobernanza. 

---

## Imagen explicativa simple
**Este documento ilustra un ataque de inversión de modelo dirigido a un sistema MedVQA-AI, una IA médica de respuesta visual a preguntas. El diagrama destaca el flujo de información, la estrategia del atacante y la posible fuga de información sanitaria protegida (PHI). Se describe cómo un atacante puede explotar las salidas del modelo para reconstruir datos de entrada sensibles, como imágenes de pacientes, comprometiendo así la privacidad.**

"Fuga de información en modelos de IA médica.
Fuga de información
La información del paciente se filtra del modelo.
Entradas del usuario
Las entradas del usuario alimentan el modelo de IA.
Núcleo del modelo
El modelo de IA procesa las entradas para generar una respuesta.
Ataque del atacante
El atacante explota el modelo para reconstruir datos" (texto de napkin.ia) .<img width="1080" height="792" alt="image" src="https://github.com/user-attachments/assets/259aca61-6d16-4e8e-8a85-4813c0cacf52" />

