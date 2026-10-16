Clasificación del malware 

● **Adware**: Software diseñado para mostrar publicidad no deseada en tu equipo, generalmente mediante ventanas emergentes 

● **Backdoor**: Método oculto para eludir la seguridad del sistema y permitir el acceso remoto no autorizado al equipo 

● **Bomba Fork**: Ataque donde un proceso se replica a sí mismo de forma infinita hasta agotar los recursos del sistema y bloquearlo 

● **Bots**: Programas automatizados que ejecutan tareas repetitivas. Cuando se agrupan en redes maliciosas, se usan para ataques masivos o envío de spam 

● **Bug**: Error o fallo en el código de un programa; no es un malware en sí, pero los atacantes los aprovechan para vulnerar el sistema 

● **Caballo de Troya**: Malware que se camufla como un programa legítimo y útil para engañar al usuario y conseguir acceso al sistema 

● **Cookies**: Archivos de texto legítimos usados por los navegadores web. Aunque no son virus, las "cookies de seguimiento" pueden usarse para rastrear la privacidad del usuario sin su permiso 

● **Crackers**: Personas que rompen los sistemas de seguridad informáticos con fines maliciosos, destructivos o de lucro ilícito 

● **Cryptovirus, Ransomware**: Malware que cifra los archivos o bloquea el dispositivo del usuario, exigiendo un rescate para devolver el acceso 

● **Exploits**: Fragmentos de código malicioso creados específicamente para aprovechar una vulnerabilidad particular de un sistema o software 

● **Hijacker**: Programa que altera la configuración del navegador web sin permiso, cambiando la página de inicio o redirigiendo el tráfico a sitios fraudulentos 

● **Hoax, Joke o Bulo**: Mensajes engañosos o cadenas falsas de correo que buscan crear alarma o desinformación, usando la ingeniería social en lugar de código malicioso 

● **Keystroke o Keylogger**: Software o hardware que registra en secreto todo lo que el usuario teclea, diseñado para robar contraseñas y datos bancarios 

● **Ladilla virtual**: Término coloquial para programas molestos y persistentes que se instalan sin permiso, alteran pequeñas configuraciones y son difíciles de erradicar 

● **Leapfrog**: Técnica de ataque donde un intruso utiliza un equipo previamente vulnerado como puente para atacar a otro, ocultando así su dirección y origen real 

● **Parásito Informático**: Término genérico para cualquier software malicioso que depende de un sistema o programa anfitrión para sobrevivir o consumir sus recursos.

● **Pharming**: Ataque que redirige el tráfico de un sitio web legítimo hacia uno falso, con el fin de robar credenciales 

● **Phishing**: Técnica de engaño donde el atacante se hace pasar por una entidad de confianza mediante correos o mensajes para que la víctima revele datos confidenciales 

● **Rabbit o conejos**: Malware diseñado para multiplicarse a gran velocidad y consumir los recursos del sistema hasta provocar una denegación de servicio 

● **Riskware**: Software completamente legítimo que, debido a sus funciones, supone un riesgo si es utilizado por un atacante 

● **Rootkit**: Conjunto de herramientas que ocultan la presencia de malware y otorgan al atacante privilegios máximos en el sistema 

● **Scumware o Escoria**: Término despectivo para agrupar programas indeseables que se instalan sin consentimiento claro y degradan el rendimiento del equipo 

● **Spam**: Correo basura o mensajería no solicitada enviada de forma masiva; a menudo actúa como vehículo para distribuir malware o phishing 

● **Spyware**: Software espía que se instala de forma oculta para recopilar información personal, historial de navegación y hábitos del usuario sin su conocimiento 

● **Pop_Ups o Ventanas emergentes**: Ventanas de navegador que se abren de repente. Aunque muchas son legítimas, son la herramienta principal del adware para mostrar publicidad intrusiva 

● **Worms o gusanos**: Malware independiente que se replica y propaga automáticamente a través de las redes infectando otros equipos, sin necesidad de un archivo anfitrión ni intervención humana 

| # | Incidente | Tipo de amenaza | ¿Se replica solo? | ¿Necesita al usuario? |
|---|---|---|---|---|
| 1 | Un socio abre factura_marzo.pdf.exe; poco después todo el disco compartido está cifrado y aparece un mensaje pidiendo 800 € | Ransomware | No | Si |
| 2 | Un "optimizador gratuito" de PC que, además, envía todo lo que se teclea a un servidor externo | Troyano con Keylogger | No | Si |
| 3 | Sin que nadie haga nada, un programa salta de portátil en portátil a través de la wifi del estudio | Gusano | Si | No |
| 4 | Al abrir ciertos ejecutables, éstos infectan a su vez a otros ficheros del equipo | Virus | Si | Si |
| 5 | Tras instalar un editor de PDF gratuito, aparecen ventanas de publicidad constantemente | Adware | No | Si |
| 6 | Un antiguo colaborador dejó un código que borrará los planos el 31 de diciembre | Bomba lógica | No | No |
| 7 | El ordenador del estudio está enviando spam sin que nadie lo sepa, bajo las órdenes de un servidor externo | Bot/zombi | No | No |
| 8 | Un conjunto de programas modificados que esconde procesos y ficheros del intruso dentro del propio sistema | Rootkit | No | Si |
| 9 | Un servicio escuchando en un puerto que permite entrar al equipo sin contraseña | Backdoor | No | No |
| 10 | Correo: "Tu banco necesita que confirmes tus datos en este enlace" | Phishing | No | Si |
| 11 | Mensaje: "Reenvía esto a 10 contactos o tu cuenta se cerrará" | Hoax (bulo) | No | Si |
| 12 | Una web idéntica a la del ERP a la que se llega porque el DNS ha sido manipulado | Pharming | No | No |
| 13 | Un programa que registra sin permiso los hábitos de navegación y los vende | Spyware | No | Si |

 ¿Cuáles de estos incidentes no son "malware" propiamente dicho, sino fraude o ingeniería social? ¿Por qué un antivirus no los detendría?

Los incidentes que no son malware propiamente dicho son el **phishing**, el **bulo** y el **pharming**. El **phishing** es una técnica de engaño donde el atacante se hace pasar por una entidad de confianza mediante correos o mensajes para que la víctima revele datos confidenciales , el **bulo** son mensajes engañosos o cadenas falsas de correo que buscan crear alarma o desinformación, usando la ingeniería social en lugar de código malicioso, y el **pharming** es un ataque que redirige el tráfico de un sitio web legítimo hacia uno falso, con el fin de robar credenciales. 

Un antivirus no siempre puede detener estas amenazas porque se basan en el engaño a la persona o usuario mediante la ingeniería social. Aunque puede bloquear enlaces peligrosos, no puede evitar todos los engaños ni impedir que una persona facilite voluntariamente sus datos.

