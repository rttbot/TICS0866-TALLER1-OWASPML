## 🔴 CASO DE PRUEBA 01- ML09 Output Integrity Attack

**ID:** ML09-01
**Tipo:** Valido
**Vulnerabilidad:** ML09
**Descripción:** Cambio de modelo en uso para el dispositivo en un momento puntual para ejecutar un ataque.

**Entrada:** Patrones de ataque similares a los practicados en pruebas de seguridad.
**Salida Esperada:** Sin alertas de ataques posibles. Aviso de Cambio del modelo en uso del dispositivo.
**Salida Real:** [RESULTADO OBTENIDO]
**Estado:** [PENDIENTE/COMPLETADO/FALLIDO]
**Severidad:** ALTA

**Precondiciones:** Tener acceso para administrar los modelos en uso para los dispositivos, acceso a modelos modificados para laboratorios de seguridad como el de la universidad, donde se ejecuten ataques de manera educativa.
**Postcondiciones:** Alerta de cambio de modelo en el sistema.
**Observaciones:** Posibilidad de hacer más estrictos los modelos por un tiempo luego de que se cambien dentro de un dispositivo.

**🔧 Procedimiento paso a paso:**
1. Persona con acceso edita el modelo de ML que se esta ejecutando en el dispositivo a uno ajustado para laboratorios de seguridad que puedan omitir ciertos tipos de ataques.
2. Coordinar el ataque para el día y hora habituales donde se utilizan los dispositivos para pruebas de seguridad.
3. Realizar el ataque pensado para que el sistema piense que es un comportamiento habitual del laboratorio.
4. tener acceso al sistema durante ese periodo de tiempo y reestablecer el modelo anterior.

## 🔴 CASO DE PRUEBA 02- ML09 Output Integrity Attack

**ID:** ML09-02
**Tipo:** Valido
**Vulnerabilidad:** ML09
**Descripción:** Intercepción de paquete de alerta por comportamniento sospechoso/ataque.

**Entrada:** Patron de ataque al sistema.
**Salida Esperada:** Ningúna alerta de comportamiento sospechoso.
**Salida Real:** [RESULTADO OBTENIDO]
**Estado:** [PENDIENTE/COMPLETADO/FALLIDO]
**Severidad:** ALTA

**Precondiciones:** Tener acceso a la ruta que siguen las alertas del servicio
**Observaciones:** Prevencion con uso de hash en paquetes y verificacion, para que se genere una alerta de hash alterado al recibir los paquetes.

**🔧 Procedimiento paso a paso:**
1. Entrar en la red donde se estan enviando los paquetes y alertas del sistema.
2. Atrapar los paquetes de alerta para cambiar su estado antes de que se genere el reporte de ataque.
3. Una vez alterada la salida no se guarda el evento ni aviso con el arbol de decicion por lo que el ataque pasa desapercibido.
