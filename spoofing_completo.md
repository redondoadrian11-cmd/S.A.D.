# Ataques de suplantación de identidad

## 1. SMTP spoofing

### Qué es
Es una técnica maliciosa en la que un atacante altera el encabezado de un correo electrónico para hacer creer al destinatario que el mensaje proviene de una fuente legítima o de una persona de confianza.
### Cómo se lleva a cabo
Mediante el SMTP, ya que no tiene protocolos de autenticidad, modificando las cabeceras de los correos. 
### Qué categoría(s) de amenaza compromete
la autenticidad y la integridad
### Ejemplo o caso real
Fraude de 100 Millones a Google, uso este metodo suplantando a un proveedor 
### Medida de prevención
SPF, DKIM y DMARC
### Fuente
Grupo 4


## 2. DNS spoofing

### Qué es
Es un ciberataque altamente engañoso en el que los hackers redirigen el tráfico web hacia servidores web falsos y sitios web de phishing. Estos sitios falsos suelen parecerse al destino previsto por el usuario, lo que facilita a los hackers engañar a los visitantes para que compartan información confidencial
### Cómo se lleva a cabo
Se lleva a cabo interceptando la petición de la víctima y enviándole una respuesta DNS falsa con una dirección IP maliciosa antes de que responda el servidor real.
### Qué categoría(s) de amenaza compromete
Spoofing (Suplantación): El atacante se hace pasar por el servidor DNS legítimo y por el sitio web de destino.
Tampering (Manipulación): Se modifican maliciosamente los paquetes de datos que viajan por la red (las respuestas DNS).
Information Disclosure (Divulgación de información): Como consecuencia del desvío, se exponen credenciales y datos privados de las víctimas en los sitios clonados.
### Ejemplo o caso real
En 2015, un grupo de hackers conocido como Lizard Squad lanzó un ataque de envenenamiento DNS contra Malaysia Airlines en el que redirigían a los visitantes de la página a un sitio web falso que les animaba a iniciar sesión solo para ser recibidos por un mensaje 404 y la imagen de un lagarto.
En primer lugar, este ataque causó importantes estragos en la aerolínea, que ya venía de un año difícil en el que se perdieron dos vuelos. En segundo lugar, planteó serias dudas sobre si el grupo de hackers robó o no información personal de alguno de los usuarios que participaron en el ataque y se conectaron al sitio web falso.
### Medida de prevención
Los proveedores pueden usar DNSSEC (seguridad de DNS). Cuando el propietario de un dominio configura las entradas de DNS, DNSSEC añade una firma criptográfica a las entradas requeridas por los llamados de resolución (o “resolvers”) antes de que estos acepten las búsquedas de DNS como auténticas.
### Fuente
https://www.proofpoint.com/es/threat-reference/dns-spoofing

## 3. IP spoofing

### Qué es

### Cómo se lleva a cabo

### Qué categoría(s) de amenaza compromete

### Ejemplo o caso real

### Medida de prevención

### Fuente


## 4. Captura de cuentas de usuario y contraseñas

### Qué es
Es una vulnerabilidad  activa, que lo que hace es obtener credenciales aprovechando vulnerabilidades 
### Cómo se lleva a cabo
mediante las herramientas, como los snifer, que monitorean el trafico de red
### Qué categoría(s) de amenaza compromete
Intercepción, Suplantación de identidad 
### Ejemplo o caso real
Mayor caso de roo de datos, afectados netflix paypal
### Medida de prevención
Utilizar contraseñas diferentes y seguras, verificacion en 2 pasos
### Fuente
Grupo 3

## Aplicado a Estudio Torrent

De los cuatro, ¿cuál creéis que sería el más plausible contra Estudio Torrent 
(wifi de oficina, ERP online, disco compartido con clientes)? Razonad la respuesta 
en 3-4 líneas, usando lo que habéis aprendido de los cuatro ataques, no solo del vuestro.
