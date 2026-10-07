# Informe de auditoría de vulnerabilidades 

## 1. Contexto y alcance
Se ha realizado una auditoría externa para Estudio Técnico Torrent S.L. con el objetivo de evaluar el estado real de seguridad de sus sistemas antes de firmar un contrato de mantenimiento.
## 2. Metodología
Para esta auditoría se ha utilizado el escáner de vulnerabilidades Nessus (versión Nessus Essentials Plus). Se ha ejecutado un "Basic Network Scan" contra el objetivo simulado (Metasploitable2). La dirección IP escaneada es 192.168.56.101
## 3. Resumen de resultados
El escaneo ha identificado un total de 70 vulnerabilidades agrupadas. Destacan múltiples fallos de riesgo bastante alto

- Críticas (CVSS 10): Múltiples riesgos que permiten obtener acceso directo al sistema, incluyendo puertas traseras y obtención de shells remotos.   

- Altas (CVSS 7 – 8,9): Un riesgo de un protocolo de acceso remoto, el "rlogin".   

- Medias (CVSS 4 – 6,9): Diversos riesgos que implican riesgo moderado, relacionadas con recursos Samb, configuraciones de TLS y servicios NFS.   

- Bajas (CVSS 0,1 – 3,9): Riesgos menores principalmente sobre detección de servicios SSL/TLS y el servidor X.   

- Informativas(CVSS ...): Gran cantidad de registros sobre descubrimiento de configuraciones básicas.
   
## 4. Vulnerabilidades clasificadas
| Vulnerabilidad | Severidad / CVSS | Origen (diseño / implementación / uso) | Breve descripción |
| :--- | :--- | :--- | :--- |
| **Bind** (Puerta trasera) | Crítica / 10 | Implementación | Se ha detectado una puerta trasera en este servicio que permite acceder al sistema. |
| **TLS** (Detección de servicios) | Media / 6,1 | Diseño | El servidor permite conexiones utilizando versiones antiguas y obsoletas del protocolo TLS.|
| **SSL** (Detección de servicios) | Baja / 2,6 | Uso | El servicio está configurado para aceptar de cifrado "anónimas" en SSL. |

## 5. Análisis en profundidad
La vulnerabilidad crítica detectada "Gain a shell remotely"
 - Qué es —  El servidor VNC utiliza una contraseña débil.
 - Cómo se podría explotar — Permite a un atacante conectarse directamente a la máquina con un cliente estándar y tomar el control total del sistema.
 - Cómo se mitigaría — Para solucionar este riesgo, se debe cambiar inmediatamente las contraseñas por unas mas robustas, además de bloquear la exposición pública del puerto mediante un cortafuegos o tener conexión a través de un túnel cifrado.
 - Referencia — Plugin ID 26925.

## 6. Recomendaciones
Para asegurar el sistema, es fundamental reemplazar inmediatamente cualquier contraseña débil o por defecto por credenciales robustas. Asimismo, se deben eliminar los protocolos obsoletos que transmiten en texto claro (como rlogin o Telnet) a favor de alternativas cifradas como SSH, y minimizar la exposición de la red cerrando puertos innecesarios y aplicando políticas de firewall estrictas.

## 7. Conclusión
El sistema auditado presenta un estado crítico de vulnerabilidad que facilita su compromiso inminente, por lo que no firmaria un contrato de mantenimiento.
