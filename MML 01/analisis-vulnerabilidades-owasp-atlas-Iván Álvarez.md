Comparación OWASP ML Top 10 vs ATLAS ML
Análisis de similitudes y diferencias
Casos de aplicación específicos a Intrusion.Aware


Vulnerabilidad ML 01. ATT&CK

Consiste en alterar los datos de entrada de manera estratégica para que el modelo cometa errores al clasificar objetos, textos o imagenes.El atacante puede eludir el sistema de detección de intrusiones del modelo manipulando el tráfico de red mediante paquetes que alteran las características del tráfico de red y escondiendo su dirección IP mediante un servidor proxy. Para robustecer el modelo se puede utilizar entrenamiento adversarial, que consiste en utilizar datos cuidadosamente alterados para poner a prueba la capacidad del modelo en detectar este tipo de ataques. Por otra parte el atacante puede elaborar datos adversarios para contraarrestar el efecto de esta medida mediante técnicas algorítmicas como optimización de caja negra en el cual tiene acceso a la API de interferencia de modelos de IA del modelo objetivo, estos ataques suelen ser menos efectivos, pero requieren mucho menos acceso.  

Similitudes entre OWASP ML top 10 vs ATLAS ML

Enfoque en amenazas reales:	OWASP.Describe ataques donde entradas maliciosas manipulan el comportamiento del modelo ML.	  ATLAS.Basado en observaciones reales y red teaming de ataques adversariales.

Objetivo:	OWASP.Aumentar la conciencia sobre vulnerabilidades comunes en sistemas ML.	ATLAS.Proveer una base de conocimiento de tácticas y técnicas adversarias contra sistemas AI.

Cobertura de ataques adversariales:	OWASP.ML01 cubre ataques como adversarial examples, perturbaciones intencionales, etc.	ATLAS.Técnicas como Adversarial Input Crafting y Evasion están documentadas en ATLAS.

Audiencia:	OWASP.Ingenieros ML, DevSecOps, profesionales de seguridad.	ATLAS.Investigadores, red teams, desarrolladores de seguridad AI.

Casos de uso:	OWASP.Evaluación de riesgos, auditorías de seguridad ML.	ATLAS.Simulación de ataques, entrenamiento de defensas, desarrollo de herramientas como Arsenal.

Diferencias entre OWASP ML top 10 vs ATLAS ML

Formato:	OWASP.Lista curada de los 10 principales riesgos de seguridad en ML.	ATLAS.Matriz de tácticas y técnicas (similar a MITRE ATT&CK).

Profundidad: OWASP.técnica	Más accesible y resumido, ideal para concienciación y evaluación inicial.	ATLAS.Más detallado y técnico, útil para modelado de amenazas y simulación de ataques.

ML01 Input Manipulation:	OWASP.Se refiere a entradas manipuladas para alterar la salida del modelo (ej. adversarial examples, perturbaciones invisibles).	ATLAS.Técnicas como Adversarial Input Crafting permiten simular este tipo de ataques con herramientas como Counterfit.

Herramientas asociadas: OWASP.No incluye herramientas específicas, pero sugiere buenas prácticas.	ATLAS.Integrado con herramientas como Microsoft Counterfit y CALDERA a través del plugin Arsenal.

Cobertura:	OWASP.Incluye también riesgos no adversariales (ej. higiene operativa).	ATLAS.Se enfoca exclusivamente en amenazas adversariales y técnicas ofensivas.


