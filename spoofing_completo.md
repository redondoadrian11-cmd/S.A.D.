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
Es un ciberataque altamente engañoso en el que el atacante redirige el tráfico web hacia servidores web falsos y sitios web de phishing. Estos sitios falsos suelen parecerse al destino previsto por el usuario, lo que facilita a los atacantes engañar a los visitantes para que compartan información confidencial
### Cómo se lleva a cabo
Se lleva a cabo interceptando la petición de la víctima y enviándole una respuesta DNS falsa con una dirección IP maliciosa antes de que responda el servidor real.
### Qué categoría(s) de amenaza compromete
Autenticidad, Suplantación de identidad, Integridad, Disponibilidad 
### Ejemplo o caso real
En 2015, un grupo de atacantes conocido como Lizard Squad lanzó un ataque de envenenamiento DNS contra Malaysia Airlines en el que redirigían a los visitantes de la página a un sitio web falso que les animaba a iniciar sesión solo para ser recibidos por un mensaje 404 y la imagen de un lagarto.
En primer lugar, este ataque causó importantes estragos en la aerolínea, que ya venía de un año difícil en el que se perdieron dos vuelos. En segundo lugar, planteó serias dudas sobre si el grupo de hackers robó o no información personal de alguno de los usuarios que participaron en el ataque y se conectaron al sitio web falso.
### Medida de prevención
Los proveedores pueden usar DNSSEC (seguridad de DNS). Cuando el propietario de un dominio configura las entradas de DNS, DNSSEC añade una firma criptográfica a las entradas requeridas por los llamados de resolución (o “resolvers”) antes de que estos acepten las búsquedas de DNS como auténticas.
### Fuente
https://www.proofpoint.com/es/threat-reference/dns-spoofing

## 3. IP spoofing

### Qué es
Falsificas la ip de origen para hacer creer que vienen de una fuente fiable
### Cómo se lleva a cabo
Modificando la cabecera poniendo otra ip, y cuando la victima responde, la respuesta va al atacante
### Qué categoría(s) de amenaza compromete
Autenticidad, Integridad, Disponibilidad 
### Ejemplo o caso real
En 1994, Kevin Mitnick consiguió acceder a sistemas informáticos de The Well, una comunidad en línea, utilizando técnicas de ingeniería social y suplantación para obtener acceso a cuentas y sistemas. El incidente llamó especialmente la atención porque Mitnick aprovechó la confianza existente entre distintos equipos y usuarios para ocultar su identidad y acceder a información que no le pertenecía. La investigación sobre sus actividades se intensificó y terminó contribuyendo a su detención en 1995.
### Medida de prevención
Mediante el filtrado de paquetes, 
### Fuente
Grupo 1

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
