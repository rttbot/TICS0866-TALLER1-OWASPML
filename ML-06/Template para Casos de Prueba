# Casos de Prueba – ML06:2023 Supply Chain Attacks

---

## CASO DE PRUEBA - DEPENDENCY CONFUSION

**ID:** ML06-DC-001  
**Tipo:** ATAQUE  
**Vulnerabilidad:** ML06 (Supply Chain Attacks)  
**Descripción:** Un pipeline de CI/CD instala una dependencia con el mismo nombre que una librería interna, pero tomada desde un registro público por tener versión más alta y sin pinning de hashes/firmas, permitiendo ejecución de código no autorizado durante la instalación.

**Entrada:**  
- `requirements.txt` sin hashes/lockfile, p.ej.: `acme-utils>=1.0`  
- Config de CI con `PIP_EXTRA_INDEX_URL` apuntando también a registro público  
- Paquete externo `acme-utils==99.0.0` disponible en el registro público

**Salida Esperada (sistema seguro):**  
- El pipeline prioriza el repo privado o verifica hashes/firmas → rechaza el paquete externo y falla la build.

**Salida Real:**  
- No ejecutado (documentado). En un entorno vulnerable se observaría:  
  - Logs de instalación mostrando descarga desde el registro público  
  - Ejecución de “post-install” no autorizada (ejemplo: lectura de variables de entorno del CI)

**Estado:** COMPLETADO (documentado, sin ejecución)  
**Severidad:** CRÍTICA

**Precondiciones:**  
- Ausencia de pinning con hash/lockfile  
- Registro público accesible en resolución de dependencias  
- Falta de política de CI que bloquee paquetes no firmados

**Postcondiciones:**  
- Si se ejecutara: Integridad del pipeline comprometida; artefactos potencialmente contaminados

**Observaciones:**  
- Escenario simulado en laboratorio, sin publicar paquetes reales  
- Alternativa defensiva: SBOM + verificación de firmas antes de build

**🔧 Procedimiento paso a paso:**  
1. Preparar `requirements.txt` sin hashes y permitir acceso a registro público.  
2. Resolver dependencias en CI → observar la selección de la versión más alta.  
3. Registrar logs de instalación (deberían evidenciar el origen público).  
4. Verificar árbol de dependencias (`pipdeptree` o `npm list`).  
5. Activar control defensivo (hashes/lockfile/firmas) y repetir → la build debe fallar.

---

