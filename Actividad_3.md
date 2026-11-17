# Memoria de Práctica: Certificados y HTTPS 

## 0.1 Antes de empezar: qué necesitas
* **Configuración de red elegida:** Dos adaptadores: Adaptador 1 en NAT para salida a Internet y adaptador 2 en Solo anfitrión.

---

## PARTE 1: Nuestra propia autoridad de certificación (CA)

### 1.2 Paso a paso

**Paso 1 - Instalar Apache y comprobar que funciona**
* **¿Qué indica el navegador sobre la seguridad de la conexión y por qué?:** El navegador indica "No seguro" porque la web se está sirviendo por HTTP en texto plano, sin cifrar. Cualquiera conectado a la red podría interceptar o modificar los paquetes en tránsito.

**Paso 2 - Generar la clave privada de la CA**
* **¿Por qué conviene proteger con contraseña la clave privada de una CA aunque esté en un directorio restrictivo?:** Porque es el elemento más crítico de la infraestructura. Si alguien lograra evadir los permisos del sistema (por ejemplo, mediante una escalada de privilegios o un robo físico del disco) y extrajera el archivo ca.key, la contraseña cifrada (con AES-256) actuaría como una barrera adicional. Sin ella, el atacante no podría usar la clave para emitir certificados falsos.

**Paso 3 - Inspeccionar el certificado**
* **¿Por qué coinciden Subject e Issuer?:** Porque se trata de un certificado de CA raíz autofirmado. Como no hay ninguna entidad jerárquicamente superior que avale a esta CA raíz, es ella misma (el Issuer) quien emite y firma el certificado de su propia identidad (el Subject).
* **¿Hasta qué fecha es válida tu CA?:** Es válida durante 10 años (3650 días) desde el momento de su creación.

**Paso 4 - La CA firma el certificado del servidor**
* **¿Por qué el certificado del servidor dura 365 días y el de la CA 10 años?:** Los certificados raíz duran mucho tiempo porque renovarlos implica un coste administrativo muy alto (hay que re-distribuir la CA pública a todos los clientes del sistema para que sigan confiando). Los certificados de servidor duran poco (365 días) por seguridad: si la clave del servidor web se ve comprometida, el certificado fraudulento dejará de ser válido en poco tiempo.

**Paso 5 - Verificar la cadena de confianza**

  certs/srv.crt: OK

  subject=C = ES, O = Torrent S.L., CN = hmi.torrent.local
  
  issuer=C = ES, O = Torrent S.L., CN = Torrent S.L. CA Raiz
  
  X509v3 Subject Alternative Name: DNS:hmi.torrent.local, IP:IP_SERVIDOR


**Paso 6 - Probar HTTPS desde la VM antes de confiar**
* **Primer comando (curl -I https://IP_SERVIDOR):** Falla mostrando un error de validación (SSL certificate problem: unable to get local issuer certificate) porque curl revisa el almacén de entidades de confianza del sistema, y la CA que acabamos de inventar (Torrent S.L.) no se encuentra en él.
* **Segundo comando (con --cacert):** Funciona y devuelve HTTP/1.1 200 OK porque al ejecutar el comando le pasamos explícitamente nuestro archivo ca.crt, indicándole a curl que debe confiar en esa CA concreta para esta conexión y así logra verificar la cadena de confianza.

**Paso 7 - El navegador (advertencia y candado)**
* **Emisor:** Quien aparece como emisor del certificado es "Torrent S.L. CA Raiz".

**Paso 8 - Mirar la conexión TLS por dentro**
* **TLS y Cifrado:** Se ha negociado la versión TLS 1.3 (o TLS 1.2 según tu SO) con un conjunto de cifrado avanzado (como por ejemplo TLS_AES_256_GCM_SHA384).
* **Investigación (sin -CAfile):** Si omitimos la opción -CAfile ~/ca/certs/ca.crt, OpenSSL intentaría verificar el certificado contra las CAs públicas por defecto de Linux. Como no encontraría nuestra CA local, el "Verify return code" pasaría a ser un código de error de OpenSSL, habitualmente 21 (unable to verify the first certificate) o 19 (self signed certificate in certificate chain), indicando que no puede trazar la cadena de confianza.

### 1.3 Preguntas de la Parte 1
1. **Diferencia de claves:**
   * ca.key: Clave privada de la CA. Es el secreto más crítico de todos.
   * ca.crt: Certificado público de la CA. Es público y debe estar en los navegadores de los clientes.
   * srv.key: Clave privada del servidor web (Apache). Es un secreto y solo debe tenerlo el servidor.
   * srv.crt: Certificado público del servidor. Es público y el servidor lo entrega a cada cliente que se conecta.
2. **Robo de ca.key (sin contraseña):** El atacante podría empezar a generar certificados completamente válidos (y firmados criptográficamente) a nombre de cualquier servidor interno de Torrent S.L., lo que le permitiría montar un ataque de suplantación *Man-in-the-Middle*. Para recuperar la confianza, Torrent S.L. tendría que anular/desechar esa infraestructura, generar una nueva CA desde cero, volver a firmar todos los certificados de servidores, y pedir a todos sus clientes o empleados que instalen manualmente el nuevo ca.crt y desconfíen del anterior.
3. **Riesgo de descarga por HTTP:** Como la red interna no está cifrada mediante HTTPS, un atacante en la misma red Wi-Fi podría interceptar la petición HTTP, modificar la descarga por el camino, e inyectar su propio certificado ca.crt falso en tu equipo. Si el usuario no verifica la huella SHA-256, el navegador terminará confiando plenamente en la CA del atacante en lugar de la oficial.
4. **CA y servidor web en la misma máquina:** No es una buena idea. Si un atacante logra explotar una vulnerabilidad en el servidor web (por ejemplo, en Apache), obtendría acceso de consola al sistema y podría robar la carpeta con las claves de la CA. En un entorno empresarial real, la CA se aloja en servidores ultraseguros, sin conexión a Internet e incluso muchas veces apagados cuando no se están firmando nuevos certificados.

---

## PARTE 2: Un certificado real y gratuito con Let's Encrypt

### 2.2 Paso a paso

**Paso 1 - Crear tu subdominio**
* **Pasos
  1. Entrar en [https://www.duckdns.org](https://www.duckdns.org) e iniciar sesión.   
  2. Crear un subdominio con un nombre único, en minúsculas y sin espacios.   
  3. Apuntar el subdominio a tu servidor escribiendo tu IP_SERVIDOR en el campo "current ip" y pulsando "update ip".   
  4. Copiar el token que aparece en la parte superior de la página.

**Paso 2 - Instalar acme.sh**
* **Lectura del script:** Tras inspeccionar el script con less, se observa que la rutina automatizada crea los directorios ocultos (como ~/.acme.sh) e instala un trabajo/tarea automática utilizando cron (crontab) en el sistema para permitir la autogestión de los certificados.

**Paso 3 - Pedir el certificado (desafío DNS-01)**
* **Pasos de acme.sh y su proceso:** Al lanzarlo, la terminal va mostrando cómo:
  1. Registra nuestra cuenta y contacta a la API de Let's Encrypt.
  2. Solicita el desafío y recibe la orden de inyectar un token TXT en nuestro DNS.
  3. Ejecuta el hook mediante la API de DuckDNS (usando la variable almacenada DuckDNS_Token) para crear automáticamente un registro _acme-challenge.
  4. Espera activamente unos segundos y Let's Encrypt valida leyendo los DNS públicos.
  5. Una vez verificado el control del dominio, Let's Encrypt emite, firma y nos entrega el certificado fullchain y la clave.

**Paso 4 - La renovación automática**
* **Caducidad y renovación:** El certificado en uso caduca a los 90 días exactos, pero la utilidad de cron intentará renovarlo automáticamente con un margen prudencial (habitualmente unos 30 días antes de su vencimiento) para prevenir incidencias.

---

### 2.3 Comparamos las dos situaciones

| | **CA propia de Torrent S.L.** | **Let's Encrypt** |
| :--- | :--- | :--- |
| **¿Quién firma el certificado?** |  con la CA raíz autogestionada | Let's Encrypt Authority (CA pública de confianza). |
| **¿El navegador lo acepta sin hacer nada?** | No. Da error SEC_ERROR_UNKNOWN_ISSUER. | Sí, lo valida transparentemente (candado verde). |
| **¿Qué tuviste que hacer para que confiara?** | Importar el archivo ca.crt manualmente en las Autoridades de Firefox tras verificar el HASH. | Absolutamente nada. Su CA raíz ya viene en el sistema de fábrica. |
| **Validez del certificado del servidor** | 365 días. | 90 días |
| **Coste y esfuerzo de renovarlo** | Es gratis, pero costoso en esfuerzo humano (proceso de terminal y redistribución de CA). | Es gratis y de esfuerzo cero gracias a la automatización de herramientas como acme.sh y el servicio Cron. |
| **¿Qué se comprueba antes de firmar?** | No se exige validación pública. | Verifican tu propiedad y control absoluto sobre el nombre del dominio mediante desafíos HTTP-01 o DNS-01. |
| **¿Valdría para un servidor sin Internet?** | Sí, la infraestructura puede aislarse 100% (ideal redes LAN e Intranets cerradas). | No. El servidor, incluso bajo desafío DNS-01, requiere salida a Internet para hacer la validación y descargar el certificado. |

---

### 3. Preguntas finales
1. **Navegadores y CAs:**
El navegador confía en Let's Encrypt de fábrica porque esta figura como una CA mundial autorizada, avalada por los consorcios de seguridad y respaldada oficialmente. Quien decide el almacén de entidades de confianza es o el Sistema Operativo o el navegador 
2. **Duración (90 días):**
Un plazo corto de 90 días obliga a la automatización forzosa en la infraestructura web y minimiza muchísimo el riesgo. Si una clave privada de servidor sufre una fuga y es vulnerada, el atacante apenas tiene unas semanas para explotar un certificado falsificado antes de que pierda por completo su validez.
3. **Escenario Intranet vs Internet:**
Para la intranet de la planta de Torrent S.L., que no tiene salida a Internet, la única opción viable y segura es emplear la CA propia, la interna. Para la web corporativa pública de un cliente utilizaríamos Let's Encrypt, para garantizar que cualquier dispositivo externo la valide de serie sin advertencias molestas.
4. **Límites de HTTPS:**
HTTPS encripta el túnel de transporte, pero no impide ataques a nivel de aplicación, como inyección de SQL o XSS contra la lógica del portal web, ni previene ataques de Denegación de Servicio (DDoS) contra el servidor, ni evita que un administrador del servidor extraiga o haga un volcado de la base de datos de los clientes desde la propia máquina.
5. **CA Propia en la realidad y VPN:**
Para una micropyme de 3 personas no tendría sentido desplegar una infraestructura completa de CA solo para tener candados web locales; es un sobreesfuerzo de mantenimiento. Pero cuando el despliegue de la VPN de una CA propia actúa vigilando en las VPN basadas en certificados, puesto que nos va a permitir generar y revocar certificados digitales, Client TL para autenticar a cada trabajador, impidiendo accesos no autorizados ajenos a la empresa corporativa.
