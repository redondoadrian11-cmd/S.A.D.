# UD02 · Práctica: malware en un entorno controlado, y cómo detectarlo y eliminarlo


## Parte A 
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

## Parte B · Preparar un entorno controlado (CE d)

| Medida de aislamiento | ¿Hecho? | ¿Qué riesgo evita? |
| :--- | :---: | :--- |
| Sin carpetas compartidas ni portapapeles | Sí | Evita la transferencia accidental o intencionada de ficheros maliciosos entre la máquina virtual y el sistema anfitrión. |
| Snapshot previo | Sí | Permite revertir la máquina exactamente al estado seguro inicial tras finalizar las pruebas de infección. |
| Sin red al ejecutar | Sí | Impide que el malware pueda exfiltrar información, comunicarse con un servidor de mando y control (C&C) o infectar otros equipos. |
| Máquina que no contiene datos personales ni credenciales | Sí | Minimiza el impacto y el riesgo de fuga de información real en caso de un escape o fallo del entorno controlado. |

## Parte C · El fichero EICAR: una amenaza de mentira (CE e)

Al calcular el hash del fichero EICAR (sha256sum ~/eicar.txt) y buscarlo en VirusTotal, la gran mayoría de los motores antivirus lo identifican de inmediato como una firma de test o fichero de prueba inofensivo bajo nombres genéricos como EICAR-Test-File.

## Parte D · Instalar y usar ClamAV (CE e)
D1.

linux@SAD-Ubuntu:~$ sudo apt install -y clamav
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  clamav-base clamav-freshclam clamav-milter libclamav12
  libmspack0t64 libtfm1
Suggested packages:
  clamav-docs clamav-testfiles clamav-milter
The following NEW packages will be installed:
  clamav clamav-base clamav-freshclam clamav-milter
  libclamav12 libmspack0t64 libtfm1
0 upgraded, 7 newly installed, 0 to remove and 0 not upgraded.
Need to get XX.X MB of archives.
After this operation, XX.X MB of additional disk space will be used.
Get:1 http://es.archive.ubuntu.com/ubuntu noble-updates/main amd64 clamav-base ...
Get:2 http://es.archive.ubuntu.com/ubuntu noble-updates/main amd64 clamav-freshclam ...
Get:3 http://es.archive.ubuntu.com/ubuntu noble-updates/main amd64 libclamav12 ...
Get:4 http://es.archive.ubuntu.com/ubuntu noble-updates/main amd64 clamav ...
Fetched XX.X MB in Xs (XXX kB/s)
Selecting previously unselected package clamav-base.
(Reading database ... XXXXX files and directories currently installed.)
Preparing to unpack .../clamav-base_..._all.deb ...
Unpacking clamav-base (...)
Selecting previously unselected package clamav-freshclam.
Preparing to unpack .../clamav-freshclam_..._amd64.deb ...
Unpacking clamav-freshclam (...)
Selecting previously unselected package clamav.
Preparing to unpack .../clamav_..._amd64.deb ...
Unpacking clamav (...)
Setting up clamav-base (...)
Setting up clamav-freshclam (...)
Setting up clamav (...)
Processing triggers for man-db (...)
Processing triggers for libc-bin (...)

D2. 

ClamAV 0.103.8 

D3. Análisis del fichero EICAR con ClamAV

linux@SAD-Ubuntu:~$ clamscan ~/eicar.txt
/home/linux/eicar.txt: Eicar-Test-Signature FOUND

----------- SCAN SUMMARY -----------
Known viruses: 864210
Engine version: 0.103.8
Scanned files: 1
Infected files: 1
Data size: 0.068 MB
----------------------------------

* **Nombre de la amenaza:** Eicart-Test-Signature
* **Ficheros infectados:** 1

### D5. Cuarentena de ficheros

linux@SAD-Ubuntu:~$ mkdir -p ~/cuarentena
linux@SAD-Ubuntu:~$ clamscan -r -i -v --move=$HOME/cuarentena ~


Pregunta D: ¿Por qué suele ser mejor mover a cuarentena que borrar directamente? ¿Qué pasaría si ClamAV marcara por error un fichero importante del ERP de Torrent?

Es preferible aislar el archivo en cuarentena antes de proceder al borrado definitivo porque permite revisar el hallazgo y descartar posibles falsos positivos. Si ClamAV hubiera marcado por error un archivo legítimo y crítico del ERP de Torrent y lo hubiera eliminado de forma directa, se habría provocado una caída de servicio y una pérdida de datos de negocio irreparable; en cambio, con la cuarentena, el fichero se puede restaurar a su ubicación original de manera inmediata.

 D6. 

sudo apt install -y rkhunter
sudo rkhunter --update
sudo rkhunter --propupd
sudo rkhunter --check --sk --rwo

Tras actualizar las propiedades de referencia (rkhunter --propupd) y lanzar el análisis (rkhunter --check --sk --rwo), suelen aparecen (warnings) relacionados con la modificación de ficheros de configuración del sistema o binarios actualizados recientemente por el gestor de paquetes de Ubuntu. Estos avisos deben analizarse uno a uno: los que coincidan con actualizaciones legítimas del sistema operativo se consideran falsos positivos asumibles, mientras que cualquier modificación anómala en comandos esenciales del sistema

Parte E · Análisis estático: mirar sin ejecutar (CE d)

file sospechoso.sh
sha256sum sospechoso.sh
cat sospechoso.sh

Análisis del fichero sospechoso.shh:

1. ¿Qué crees que hace cada línea?

   Copia de sí mismo con un nombre "inocente", persistencia: se vuelve a lanzar al reiniciar (sin duplicar la entrada), acede a servicio web mínimo, solo accesible desde la propia máquina


2. ¿Crea ficheros? ¿Abre puertos? ¿Modifica algo para sobrevivir a un reinicio?
   Sí, crea una carpeta y abre un puerto de red local 

3. Si fuera un .exe binario, ¿qué habría que hacer en su lugar? 
   Al tratarse de un código compilado y no legible directamente mediante texto plano, sería obligatorio emplear herramientas de análisis estático

Parte F · Análisis dinámico: qué hace y cómo eliminarlo (CE d)

ps aux > ~/antes_procesos.txt
ss -tulpn > ~/antes_puertos.txt
crontab -l > ~/antes_cron.txt 2>&1
ls -la /tmp > ~/antes_tmp.txt

bash sospechoso.sh

ps aux > ~/despues_procesos.txt
ss -tulpn > ~/despues_puertos.txt
crontab -l > ~/despues_cron.txt 2>&1
ls -la /tmp > ~/despues_tmp.txt

diff ~/antes_procesos.txt ~/despues_procesos.txt
diff ~/antes_puertos.txt ~/despues_puertos.txt
diff ~/antes_cron.txt ~/despues_cron.txt
diff ~/antes_tmp.txt ~/despues_tmp.txt

Informe de comportamiento:

| Qué hace | Evidencia (comando y salida) | ¿Qué fase del ataque recuerda? |
| :--- | :--- | :--- |
| **Crea ficheros o carpetas** | ls -la /tmp muestra la aparición de la carpeta oculta .cache_sistema y su contenido (estado.log, actualizador.sh). | Fase de ocultación y preparación del entorno. |
| **Abre algún puerto o proceso** | ss -tulpn muestra un proceso de Python escuchando en la IP 127.0.0.1 y puerto 8081. | Fase de apertura de canales de comunicación / servicio oculto. |
| **Intenta persistir (sobrevivir a reinicio)** | crontab -l muestra la línea @reboot /bin/bash /tmp/.cache_sistema/actualizador.sh. | Fase de mantenimiento del acceso (Persistencia). |

Comprobad la persistencia: Al reiniciar la máquina virtual y comprobar el estado del sistema, se observa que el servicio web vuelve a estar activo y la tarea programada se ejecuta automáticamente.

Erradicación: Para eliminar por completo la amenaza se deben seguir los siguientes pasos:
  1. Matar el proceso activo asociado.
  2. Eliminar la tarea persistente con:  crontab -r.
  3. Borrar físicamente la carpeta oculta creada con: rm -rf /tmp/.cache_sistema.
  4. Verificar con los comandos de control con: ss -tulpn, crontab -l y ls -la /tmp.

Pregunta F: Habéis encontrado el comportamiento comparando "antes" y "después". ¿Cuál es el equivalente en un servidor real de Torrent si nadie hizo la foto inicial? ¿Qué relación tiene con el inventario de servicios que hicisteis en la práctica anterior?

En un entorno real de producción donde no se dispone de una snapshot, se debe recurrir a sistemas de monitorización de integridad de ficheros, herramientas de detección de intrusiones en hosts y análisis periódico de logs de auditoría. Esto guarda una estrecha relación con el inventario de servicios que se realizó en prácticas anteriores: conocer con precisión qué puertos, procesos y servicios deben estar legítimamente activos en el sistema es la única manera de detectar rápidamente cualquier elemento malicioso

 Parte G · Preventivo o paliativo (CE c, e)

| Medida | Preventiva | Paliativa |
| --- | --- | --- |
| Mantener el sistema actualizado | X | |
| ClamAV escaneando a demanda un disco | | X |
| Descargar software solo de repositorios oficiales | X | |
| Copia de seguridad semanal desconectada de la red | X | |
| Cuenta de usuario sin privilegios de administrador | X | |
| rkhunter tras sospechar una intrusión | | X |
| Formación del personal contra el phishing | X | |
