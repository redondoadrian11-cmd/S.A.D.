# Laboratorio fases 1 y 2

## B. Antes y después

**Inventario de servicios:**
En la medición Antes con Nmap, se detectaron múltiples puertos abiertos que no eran necesarios para el propósito del servidor (solo web 80 y SSH 22). Entre ellos se encontraban puertos como el 21 (FTP), 23 (Telnet), 139/445 (Samba), 5432 (PostgreSQL) y varios servicios de los "porteros" (inetd/xinetd).

**Contramedidas aplicadas (Apagado de servicios):**

| Puerto | Servicio | Dueño | Cómo se arranca | Decisión | Lo que hice |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 23 | Telnet | xinetd | inetd.conf | Apagar | Comentar la línea con # en /etc/inetd.conf + sudo /etc/init.d/xinetd reload |
| 5432 | PostgreSQL | postgres | /etc/init.d/postgresql-8.3 | Apagar | sudo /etc/init.d/postgresql-8.3 stop + sudo update-rc.d -f postgresql-8.3 remove |
| 139/445 | Samba | smbd | /etc/init.d/samba | Apagar | sudo /etc/init.d/samba stop + sudo update-rc.d -f samba remove |
| 53 | DNS | bind9 | /etc/init.d/bind9 | Apagar | sudo /etc/init.d/bind9 stop + sudo update-rc.d -f bind9 remove |

**Contramedidas aplicadas (Firewall):**
Se bloqueó la respuesta al ping (ICMP) mediante iptables:
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP

**Tabla comparativa:**

| Aspecto | Antes | Contramedida aplicada | Después |
| :--- | :--- | :--- | :--- |
| **Puertos abiertos** | Muchos puertos abiertos | Detener procesos e inutilizar el arranque en init.d/inetd | Solo quedan abiertos el 22 y el 80 |
| **Servicios/versiones** | Visibles múltiples servicios | Desactivar servicios que sobran | Solo Apache y OpenSSH |
| **Respuesta al ping** | Sí respondía a las peticiones ICMP | Regla DROP en iptables | No responde (paquetes perdidos 100%) |

## Reflexión

**1. ¿Qué contramedida ha reducido más lo que ve el atacante? ¿Por qué?**
Apagar los servicios innecesarios. Al detener el programa que escucha detrás del puerto, Nmap ya no lo detecta como abierto en su escaneo. Esto elimina por completo las puertas de entrada, reduciendo drásticamente el ataque, mucho más que simplemente poner un cortafuegos.

**2. Si solo ocultáis un banner pero el servicio sigue activo, ¿la vulnerabilidad sigue ahí? Razonad la respuesta.**
Sí, la vulnerabilidad sigue ahi. Ocultar el banner lo único que hace es que el atacante no sabrá a simple vista qué versión estamos usando, pero el programa vulnerable sigue corriendo.

**3. Torrent-Vulnerable usa un sistema operativo sin soporte desde hace años. ¿Puede una contramedida de estas sustituir a actualizarlo? ¿Qué haríais en una empresa real?**
No, las contramedidas reduden el riesgo temporalmente, pero no solucionan los agujeros de seguridad del sistema operativo. En una empresa real, habría que migrar el servicio web y SSH a un servidor moderno con soporte y actualizaciones al día lo antes posible. Si no se puede actualizar por razones de software heredado, habría que aislarlo por completo en una red interna sin exposición directa.

**4. De todas las contramedidas de las partes A y B, ¿cuáles evitan que el atacante encuentre información y cuáles solo ayudan a detectar que lo están intentando?**
- **Evitan que el atacante encuentre información:** Apagar los servicios, bloquear el ping con DROP y cambiar los banners.
- **Ayudan a detectar que lo intentan:** Registrar los escaneos mediante la regla LOG de iptables.
