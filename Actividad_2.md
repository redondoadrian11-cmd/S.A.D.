**Paso 1.** Leed las seis fuentes con ojos de atacante y encontrad al menos 10 datos que os servirían para atacar a la empresa. Anotadlos en esta tabla:
| Dato encontrado | Fuente (1-6) | ¿Para qué le sirve al atacante? | Contramedida propuesta | 
| --- | --- | --- | --- | 
| Uso de Ubuntu 8.04 y Windows 7 | 2 | Sistemas operativos desactualizados y sin soporte que permiten explotar vulnerabilidades conocidas | Actualizar los sistemas operativos a Windows 11 o Ubuntu 22 | 
| Registro SPF configurado como +all | 4 | Es permitido que cualquier IP de internet a enviar correos en nombre del dominio. Permite suplantación de identidad (spoofing) | Corregir el registro SPF usando -all o con las IPs legítimas | 
| Ausencia de DKIM y DMARC | 4 | No hay firma criptográfica ni política de rechazo para correos no verificados | Implementar y configurar correctamente registros DKIM y DMARC | 
| Mensaje de "Fuera de la oficina" detallado | 6 | Revela el contacto de quien maneja los pagos y una urgencia. Ideal para un ataque de fraude de la CEO | Redactar mensajes automáticos genéricos sin revelar roles internos ni procesos de pago | 
| Subdominios internos públicos | 4 | Expone infraestructura crítica directamente a internet, facilitando ataques directos | Ocultar estos recursos detrás de una VPN; eliminar registros DNS públicos de servicios internos | 
| Nota adhesiva en el monitor | 5 | Un atacante puede ampliar la imagen y obtener credenciales | Aplicar política de mesas limpias y usar gestores de contraseñas | 
| Teletrabajo en red WiFi pública | 5 | Al usar una WiFi de cafetería, el tráfico puede ser interceptado (Man-in-the-Middle) para robar credenciales. | Obligar al uso de VPN corporativa siempre que se trabaje fuera de la oficina | 
| CMS desactualizado | 3 | Un software web sin actualizar es altamente vulnerable a inyecciones SQL | Mantener el CMS y sus plugins actualizados a la última versión | 
| Datos personales en el WHOIS | 3 | Expone el móvil y correo personal de Marta, siendo objetivo de phishing personal o ingeniería social | Activar la protección de privacidad en el registrador de dominios | 
| Información de roles en la web | 2 | Permite perfilar a los empleados para correos maliciosos | Formación en concienciación de seguridad para los empleados | 

**Paso 2.** Con lo que habéis encontrado, escribid en **5 líneas como máximo** el plan de ataque que montaríais contra Estudio Torrent.

Aprovechando la configuración permisiva del SPF y el estado de vacaciones de Marta, falsificaré su correo para ordenar a Lucía un pago fraudulento *(Fraude del CEO)*. Simultáneamente, explotaré las vulnerabilidades del servidor Ubuntu 8.04 y el NAS expuestos al público para penetrar en la infraestructura. Como vector de acceso alternativo, interceptaré el tráfico WiFi en la cafetería mediante un ataque *Man-in-the-Middle* para robar credenciales, logrando finalmente el control total para desplegar *ransomware* en los equipos Windows 7 obsoletos de la oficina.

**Paso 3.** De todas las contramedidas de vuestra tabla, **elegid las 3 más importantes** y justificad por qué esas y no otras.

Para proteger la infraestructura, lo mas importante es *actualizar los sistemas operativos antiguos (Windows 7 y Ubuntu 8.04)* para evitar la ejecución remota de exploits públicos; *corregir el registro SPF y configurar DMARC/DKIM* para frenar drásticamente los ataques de suplantación de identidad (spoofing) y fraude financiero; y *retirar los servicios internos de internet* (como el NAS) exigiendo el uso de una VPN, lo que blinda las copias de seguridad de ataques directos y cifra las conexiones vulnerables durante el teletrabajo.
