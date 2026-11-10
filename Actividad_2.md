#Contraseñas, MFA y privilegios justos

##1. Instalación del módulo PAM para Google Authenticator

Se instala el paquete libpam-google-authenticator en el sistema operativo mediante el gestor de paquetes de APT, el cual incorpora las librerías necesarias para integrar la validación de códigos de 6 tiempos en la pila de autenticación de Linux, con el comando:

sudo apt install libpam-google-authenticator

##2. Generación del secreto TOTP por usuario

Se ejecuta el asistente interactivo de configuración para el usuario, encargado de generar el secreto criptográfico único en base32 y el código QR asociado para su sincronización con la aplicación cliente (Google Authenticator), con el comando:

sudo apt install libpam-google-authenticator
google-authenticator

¿Do you want authentication tokens to be time-based (y/n)? respondes y 

¿Do you want me to update your "~/.google_authenticator" file (y/n)? respondes y

¿Do you want to disallow multiple uses of the same authentication token...? respondes y 

¿Do you want to fast-forward time to deal with time skew...? respondes n 

¿Do you want to enable rate-limiting...? respondes y 

##3. Integración del módulo en la pila PAM de SSH

Para asegurar que el servicio de SSH procese la comprobación del segundo factor, se modifica el gestor de módulos de autenticación enchufables (PAM).
Se editó el archivo de configuración:

sudo nano /etc/pam.d/sshd

después de @include common-auth le das enter y pones:

auth required pam_google_authenticator.so

Le damos a ctrl + o para guardar y a ctrl + x para volver

Revisamos la configuración del servidor SSH en:

sudo nano /etc/ssh/sshd_config

Y le ponemos esto en yes (si no es que ya lo está):

KbdInteractiveAuthentication yes

UsePAM yes

PasswordAuthentication yes

Le damos a ctrl + o para guardar y a ctrl + x para volver

Y reiniciamos: 

sudo systemctl restart ssh
