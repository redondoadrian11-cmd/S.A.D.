Parte A · ¿Está actualizado el sistema?

A1. 

linux@SAD-Ubuntu:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 24.04.3 LTS
Release:        24.04
Codename:       noble

linux@SAD-Ubuntu:~$ uname -r
7.0.0-31-generic

A2. 

linux@SAD-Ubuntu:~$ apt list --upgradable
...

A3. 

linux@SAD-Ubuntu:~$ sudo apt-get -s upgrade | head -30
[sudo] password for linux: 
Reading package lists...
Building dependency tree...
Reading state information...
Calculating upgrade...
The following packages have been kept back:
  fwupd linux-generic-hwe-24.04 linux-headers-generic
  linux-headers-generic-hwe-24.04 linux-image-generic-hwe-24.04
The following packages will be upgraded:
  alsa-ucm-conf apparmor apport apport-core-dump-handler apport-gtk base-files
  bind9-dnsutils bind9-host bind9-libs bluez bluez-cups bluez-obexd bubblewrap
  ca-certificates cloud-init console-setup console-setup-linux dconf-cli
  dconf-gsettings-backend dconf-service dhcpcd-base distro-info-data dmidecode
  dnsmasq-base dracut-install gdb gdm3 ghostscript gir1.2-gdm-1.0
  gir1.2-glib-2.0 gir1.2-gtk-4.0 gir1.2-mutter-14 gir1.2-nm-1.0
  gir1.2-rsvg-2.0 gir1.2-soup-3.0 gnome-control-center
  gnome-control-center-data gnome-control-center-faces gnome-keyring
  gnome-keyring-pkcs11 gnome-remote-desktop gnome-settings-daemon
  gnome-settings-daemon-common gnome-shell gnome-shell-common
  gnome-shell-extension-desktop-icons-ng gstreamer1.0-plugins-good gvfs
  gvfs-backends gvfs-common gvfs-daemons gvfs-fuse gvfs-libs heif-gdk-pixbuf
  heif-thumbnailer initramfs-tools initramfs-tools-bin initramfs-tools-core
  iproute2 keyboard-configuration krb5-locales language-pack-en
  language-pack-en-base language-pack-gnome-en language-pack-gnome-en-base
  ldap-utils libaom3 libapparmor1 libarchive13t64 libaudit-common libaudit1
  libauthen-sasl-perl libbluetooth3 libcares2 libcurl3t64-gnutls libcurl4t64
  libdconf1 libde265-0 libegl-mesa0 libexpat1 libfprint-2-2 libfprint-2-tod1
  libfreerdp-server3-3 libfreerdp3-3 libfreetype6 libfwupd2 libgbm1 libgdm1
  libgl1-mesa-dri libglib2.0-0t64 libglib2.0-bin libglib2.0-data libglx-mesa0
  libgs-common libgs10 libgs10-common libgssapi-krb5-2
  libgstreamer-plugins-good1.0-0 libgtk-4-1 libgtk-4-bin libgtk-4-common

A4. 

| Sistema | Fecha de fin de soporte | ¿Recibe parches hoy? | 
|---|---|---|
| Torrent-Auditor (Ubuntu 24.04) | 31 de mayo de 2029 | si |
| Torrent-Vulnerable (Ubuntu 8.04) | 9 de mayo de 2013 | no |

Pregunta A: En la oferta de empleo de Torrent (dossier de la UD02) decían que trabajaban con "Ubuntu 8.04". Aunque pusieran todas las contraseñas bien, ¿qué problema sigue sin tener solución?

El problema es que el sistema es tan antiguo que hace más de diez años que no recibe actualizaciones de seguridad. Esto significa que cualquier fallo que tenga se quedará sin arreglar, permitiendo que un atacante pueda entrar fácilmente.

Parte B · ¿De dónde vienen los programas instalados?

B1. 

linux@SAD-Ubuntu:~$ cat /etc/apt/sources.list
# Ubuntu sources have moved to /etc/apt/sources.list.d/ubuntu.sources

linux@SAD-Ubuntu:~$ ls /etc/apt/sources.list.d/
ubuntu.sources  ubuntu.sources.curtin.orig

El archivo sources.list (y los de la carpeta sources.list.d) contiene las direcciones web (URL) de los servidores desde los cuales el gestor de paquetes apt descarga el software y las actualizaciones

B2. ¿Qué pasaría si un atacante introduce una clave falsa en /usr/share/keyrings/? ¿y si intercepta o reemplaza la clave oficial de Ubuntu ubuntu-archive-keyrings.gpg?

El sistema apt pasaría a confiar ciegamente en cualquier paquete malicioso firmado con esa clave falsa y si intercepta o reemplaza la clave oficial de Ubuntu podría suplantar los repositorios oficiales por completo. Al actualizar el sistema, este descargaría e instalaría paquetes controlados por el atacante

B3.

linux@SAD-Ubuntu:~$ apt policy curl
curl:
  Installed: (none)
  Candidate: 8.5.0-2ubuntu10.16
  Version table:
     8.5.0-2ubuntu10.16 500
        500 http://es.archive.ubuntu.com/ubuntu noble-updates/main amd64 Packages
        500 http://security.ubuntu.com/ubuntu noble-security/main amd64 Packages
     8.5.0-2ubuntu10 500
        500 http://es.archive.ubuntu.com/ubuntu noble/main amd64 Packages

Versión instalada: (none). Esto significa que el paquete no está desplegado ni operativo actualmente en el sistema.

Versión candidata: 8.5.0-2ubuntu10.16. Es la versión sugerida que el gestor APT propone instalar en caso de lanzar una orden de instalación.

Repositorio de origen: Proviene principalmente de [http://es.archive.ubuntu.com/ubuntu](http://es.archive.ubuntu.com/ubuntu) (rama de actualizaciones noble-updates) y [http://security.ubuntu.com/ubuntu](http://security.ubuntu.com/ubuntu) (rama de seguridad noble-security).

Significado de la tabla de versiones: Muestra todas las versiones de curl que están disponibles en los repositorios configurados y la prioridad (500) asignada a cada una, indicando al sistema de dónde se descargaría la versión candidata por defecto.

b4.

linux@SAD-Ubuntu:~$ apt list --installed 2>/dev/null | grep -i local
krb5-locales/noble-security,now 1.20.1-6ubuntu2.7 all [installed,upgradable to: 1.20.1-
6ubuntu2.10]
libencode-locale-perl/noble,now 1.05-3 all [installed,automatic]
liblocale-gettext-perl/noble,now 1.07-6ubuntu5 amd64 [installed,automatic]
locales/noble-updates,noble-security,now 2.39-0ubuntu8.9 all [installed,automatic]
nessus/now 10.12.4 amd64 [installed,local]
python-babel-localedata/noble,now 2.10.3-3build1 all [installed,automatic]

¿Quién lo instaló? 

Lo instalamos nosotros manualmente como administradores (usando el comando dpkg -i).

¿De dónde? 

No proviene de los repositorios seguros de Ubuntu, sino que el archivo .deb se descargó directamente de forma externa desde la página web de su fabricante (Tenable).

¿Quién lo verificó? 

Al instalarse de forma local y ajena a los repositorios, el sistema APT no comprobó ninguna clave GPG ni firma digital automática. La responsabilidad de la verificación recayó exclusivamente en nosotros (la confianza la pusimos nosotros al descargarlo de la web).

B5. 

Plaintext
??5?????? /opt/nessus/etc/nessus/fipsmodule.cnf
missing   /opt/nessus/var/nessus/tmp/nessusd
missing   /opt/nessus/var/nessus/tmp/nessusd.service
??5?????? /opt/nessus/lib/nessus/plugins/known_CA.inc
??5?????? /usr/lib/systemd/system/cloud-init.service
Análisis de la salida:

missing: Indica que el archivo ya no se encuentra en la ruta original (en este caso, archivos temporales de Nessus que probablemente se limpiaron automáticamente).

??5??????: El número 5 advierte que el hash MD5 actual es distinto al original; es decir, el contenido del archivo ha cambiado.

Conclusión: Todas las alteraciones mostradas pertenecen a archivos de configuración o plugins de Nessus y al servicio cloud-init, lo cual es un comportamiento completamente normal derivado del uso y configuración de estas aplicaciones. Al no haber binarios críticos del sistema operativo modificados, no hay signos de alarma o intrusión.

Pregunta B: dpkg -V ¿detectaría un rootkit que además hubiera modificado la base de datos de dpkg? ¿Por qué conviene comparar con una copia guardada fuera del sistema?

No lo detectaría. Si el atacante tiene suficientes privilegios en el sistema para modificar el archivo original y además alterar la base de datos interna de dpkg, dpkg -V comparará el archivo malicioso con un hash también falsificado, dando un resultado de "todo correcto". Por este motivo, siempre se deben comparar los hashes de los ficheros críticos contra una copia segura guardada fuera del propio servidor, donde el atacante no pueda manipular la evidencia.

Parte C · Verificar una descarga: hash y firma

C1.

linux@SAD-Ubuntu:~$ mkdir -p ~/verif && cd ~/verif
linux@SAD-Ubuntu:~/verif$ wget https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz
--2026-10-23 11:26:11--  https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz
Resolving ftp.gnu.org (ftp.gnu.org)... 209.51.188.20
Connecting to ftp.gnu.org (ftp.gnu.org)|209.51.188.20|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1033297 (1009K) [application/x-gzip]
Saving to: ‘hello-2.12.1.tar.gz’

hello-2.12.1.tar.gz   100%[===================>]   1009K   873KB/s    in 1.2s

2026-10-23 11:26:13 (873 KB/s) - ‘hello-2.12.1.tar.gz’ saved [1033297/1033297]

linux@SAD-Ubuntu:~/verif$ wget https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz.sig
--2026-10-23 11:26:34--  https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz.sig
Resolving ftp.gnu.org (ftp.gnu.org)... 209.51.188.20
Connecting to ftp.gnu.org (ftp.gnu.org)|209.51.188.20|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 488 [application/pgp-signature]
Saving to: ‘hello-2.12.1.tar.gz.sig’

hello-2.12.1.tar.gz.s 100%[===================>]     488  --.-KB/s    in 0s

2026-10-23 11:26:34 (45.3 MB/s) - ‘hello-2.12.1.tar.gz.sig’ saved [488/488]

linux@SAD-Ubuntu:~/verif$ wget https://ftp.gnu.org/gnu/gnu-keyring.gpg
--2026-10-23 11:27:13--  https://ftp.gnu.org/gnu/gnu-keyring.gpg
Resolving ftp.gnu.org (ftp.gnu.org)... 209.51.188.20
Connecting to ftp.gnu.org (ftp.gnu.org)|209.51.188.20|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3698125 (3.5M)
Saving to: ‘gnu-keyring.gpg’

gnu-keyring.gpg       100%[===================>]   3.53M   751KB/s    in 5.5s

2026-10-23 11:27:20 (662 KB/s) - ‘gnu-keyring.gpg’ saved [3698125/3698125]

C2.

linux@SAD-Ubuntu:~/verif$ sha256sum hello-2.12.1.tar.gz
8d99142afd92576f30b0cd7cb42a8dc6809998bc5d607d88761f512e26c7db20  hello-2.12.1.tar.gz

C4.

| Situación | ¿Basta con el hash? | ¿Hace falta la firma? ¿Por qué? |
| :--- | :--- | :--- |
| **El fichero se corrompió al descargarlo** | Sí | No es estrictamente necesaria. Un error de transmisión altera los bits del archivo, provocando que el hash local calculado no coincida con el hash publicado originalmente en la web. |
| **Un atacante cambió el fichero y el hash publicado en la misma web** | No | Sí. Si el atacante ha logrado comprometer el servidor web, puede modificar tanto el archivo malicioso como el hash asociado. Solo la firma digital (cifrada con la clave privada del autor original) garantiza la autenticidad, ya que el atacante no posee dicha clave. |
| **Descargas desde un espejo (mirror) que no es de confianza** | No | Sí. Un servidor espejo no oficial podría servir un archivo troyanizado junto a un hash falso inventado por él mismo. La firma digital del autor original asegura que el software procede legítimamente de su creador, independientemente del servidor intermedio utilizado. |

Parte D · Política de actualizaciones para Torrent

Política de mantenimiento y actualización:
Las actualizaciones serán gestionadas semanalmente por el administrador de sistemas designado. Los parches de seguridad críticos del sistema operativo se aplicarán automáticamente (unattended-upgrades) para minimizar la ventana de exposición. Sin embargo, las actualizaciones de aplicaciones críticas de negocio, como el ERP y el servidor de impresión, se desplegarán de forma manual tras ser probadas previamente en un entorno de preproducción. Todo el software se instalará exclusivamente desde repositorios oficiales o fuentes firmadas y autorizadas. Cualquier equipo o software que alcance su estado EOL (fin de soporte) deberá ser aislado de la red principal inmediatamente y programado para su migración a una versión soportada.

Debate (Automático vs Manual):

Automático: Ganas inmediatez ante vulnerabilidades críticas y reduces la carga de trabajo administrativo, pero pierdes control, asumiendo el riesgo de que una actualización incompatible rompa un servicio vital (como el ERP) en un momento crítico.

Manual/Probado: Ganas estabilidad y control absoluto sobre los servicios de producción, asegurando que nada se rompe, pero pierdes agilidad; durante el tiempo que tardas en probar el parche, el sistema es vulnerable a ataques de "día cero".
