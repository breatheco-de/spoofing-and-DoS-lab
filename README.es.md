<!-- hide -->
# Suplantación y DoS en un sitio web
<!-- endhide -->
Esta práctica te permitirá comprender mejor las técnicas de suplantación y DoS, así como sus efectos en un entorno web.
<!-- hide -->
> Por [@rosinni](https://github.com/rosinni) y [otros colaboradores](https://github.com/4GeeksAcademy/deploying-wordpress-debian/graphs/contributors) en [4Geeks Academy](https://4geeksacademy.co/)

[![build by developers](https://img.shields.io/badge/build_by-Developers-blue)](https://4geeks.com)
[![build by developers](https://img.shields.io/twitter/follow/4geeksacademy?style=social&logo=twitter)](https://twitter.com/4geeksacademy)

*Estas instrucciones están [disponibles en inglés](https://github.com/breatheco-de/spoofing-and-DoS-lab/blob/main/README.md)*

### Antes de comenzar...

> ¡Te necesitamos! Estos ejercicios son construidos y mantenidos en colaboración con contribuyentes como tú. Si encuentras algún error o falta de ortografía, por favor contribuye y/o repórtalo.
<!-- endhide -->

## 🌱 ¿Cómo comenzar un proyecto?

No clones este ni ningún repositorio, sigue las instrucciones a continuación:

### Requisitos

Para esta práctica específica de Suplantación y DoS, es mejor configurar la red como una Red Interna. Esto se debe a varias razones:

- **Aislamiento Completo:** Asegura que las actividades de ataque y prueba no interfieran con la red de producción ni con otras redes externas. Esto previene cualquier posible impacto no deseado en otros sistemas.

- **Entorno Controlado:** Permite un control completo sobre el entorno de la red, facilitando el monitoreo y análisis del tráfico de red generado durante las pruebas.

- **Simulación Realista:** Aunque está aislada, una red interna puede simular efectivamente un entorno de red real para las prácticas de Suplantación y DoS.

### Herramientas y Máquinas Virtuales

* Oracle VirtualBox
* Máquina virtual con Kali Linux (Atacante)
* Máquina virtual con Debian (Servidor Web): Donde tenemos el servidor Apache y el sitio de WordPress alojado.
* Herramientas de Suplantación y DoS instaladas en las máquinas virtuales.
* Un entorno de red aislado.
* Herramientas necesarias: **arpspoof, hping3, wireshark**

## 📝 Instrucciones

### Paso 1: Configurar la Red en VirtualBox

#### Configurar la Red de la Máquina Debian (Servidor Web):
- [ ] Abre VirtualBox.
* [ ] Selecciona tu máquina virtual Debian y haz clic en "Configuración".
* [ ] Ve a la sección "Red".
* [ ] Configura el "Adaptador 1" como "Red Interna".
* [ ] En el campo "Nombre", ingresa un nombre para la red interna, por ejemplo, "LabNetwork".
* [ ] Inicia la máquina y verifica la interfaz de red disponible y su configuración actual usando el siguiente comando en la terminal:

````bash
ip addr show
````

> *Generalmente encontrarás líneas etiquetadas como eth0, enp0s3, wlan0, etc. La que esté activa y tenga una dirección IP asignada será la interfaz que estás utilizando.*

* [ ] Configuración manual de IP para usar la red interna en el archivo `/etc/network/interfaces` con el siguiente comando:

```bash
sudo nano /etc/network/interfaces
```

* [ ] Agrega lo siguiente al archivo que se abre:

```plaintext:
auto enp0s3
iface enp0s3 inet static
    			address 192.168.1.10
    			netmask 255.255.255.0
    			gateway 192.168.1.1
```

* [ ] Guarda los cambios y cierra la ventana de configuración.

![Configuración manual de IPs](https://github.com/breatheco-de/spoofing-and-DoS-lab/blob/dc2d83a7772cb56ce82de4793f60e704be11995d/assets/ip-config.png?raw=true)

> *Ejemplo de configuración para ambos archivos (Kali y Debian), puede contener más comandos de los que se muestran, pero agrega cualquier comando faltante de la configuración proporcionada previamente, ya que serán necesarios.*

* [ ] Reinicia el servicio de red:

```bash
sudo systemctl restart networking
```

* [ ] Usa nuevamente el comando **ip addr** show y verifica que tu dirección IP sea la que configuraste.

#### Configurar la Red de la Máquina Kali Linux (Atacante)

* [ ] Selecciona tu máquina virtual Kali Linux y haz clic en "Configuración".
* [ ] Ve a la sección "Red".
* [ ] Configura el "Adaptador 1" como "Red Interna".
* [ ] En el campo "Nombre", selecciona el mismo nombre de red interna que usaste para la máquina Debian ("LabNetwork").
* [ ] Inicia la máquina y verifica la interfaz de red disponible y su configuración actual usando el siguiente comando en la terminal:

````bash
ip addr show
````

> *Generalmente encontrarás líneas etiquetadas como eth0, enp0s3, wlan0, etc. La que esté activa y tenga una dirección IP asignada será la interfaz que estás utilizando.*

* [ ] Configuración manual de IP para usar la red interna en el archivo `/etc/network/interfaces` con el siguiente comando:

```bash
sudo nano /etc/network/interfaces
```

* [ ] Agrega lo siguiente al archivo que se abre:

```plaintext:
auto eth0
iface eth0 inet static
    			address 192.168.1.11
    			netmask 255.255.255.0
    			gateway 192.168.1.1
```

* [ ] Guarda los cambios y cierra la ventana de configuración.
* [ ] Reinicia el servicio de red:

```bash
sudo systemctl restart networking
```

* [ ] Usa nuevamente el comando **ip addr** show y verifica que tu dirección IP sea la que configuraste.

### Paso 2: Verificar la Conexión Entre las Máquinas

#### Desde la Máquina Kali Linux (Atacante):

* [ ] Abre una terminal y haz un ping a la máquina Debian para verificar la conexión:

```bash
$ ping <IP_debian>
```

> Reemplaza <IP_debian> con la dirección IP que obtuviste para la máquina Debian.

#### Desde la Máquina Debian (Servidor Web):

* [ ] Abre una terminal y haz un ping a la máquina Kali Linux para verificar la conexión:

```bash
$ ping <IP_kali>
```

> Reemplaza <IP_kali> con la dirección IP que obtuviste para la máquina Kali.

*Ejemplo gráfico de cómo se ven los pings cuando están conectados*

<!-- ![verificación de conexión entre las maquinas virtuales](assets/ping-view.png) -->

### Paso 3: Práctica de ARP Spoofing

Para realizar esta práctica, utilizaremos arpspoof. Esta herramienta se usa para enviar paquetes ARP falsificados a la red, haciendo que un dispositivo (como la máquina Debian) crea que la dirección MAC del atacante (Kali Linux) es la dirección MAC del gateway (router). Esto se puede verificar observando las tablas ARP en la máquina Debian antes y después de ejecutar arpspoof.

#### En la Máquina Kali Linux (Atacante):

* [ ] Instalar arpspoof:

```bash
sudo apt update
sudo apt install dsniff
```

> *Nota: arpspoof es parte del paquete dsniff..*

 * [ ] Verifica si arpspoof está instalado:

 ```bash
sudo arpspoof -h
```

* [ ] Ejecuta arpspoof para envenenar las tablas ARP de la máquina Debian y el gateway:

```bash
sudo arpspoof -i <interfaz_kali> -t <IP_debian> <gateway>
```

* -i <interfaz_kali>: Especifica la interfaz de red desde la cual se enviarán los paquetes ARP, por ejemplo, eth0.
* -t <IP_debian>: Especifica la dirección IP de la víctima (la máquina Debian en este caso).
* <gateway>: Especifica la dirección IP del gateway. (inicialmente configurado en el archivo /etc/network/interfaces, es el mismo para ambas máquinas)

#### Monitorear con Wireshark en la Máquina Debian (Servidor Web):

Wireshark es ideal para analizar el tráfico de red, identificar posibles ataques y monitorear la seguridad en un entorno de red.

* [ ] Instalar Wireshark:

```bash
sudo apt update
sudo apt install wireshark
```

Durante la instalación, es posible que te pregunten si los usuarios no root deberían poder capturar paquetes. Selecciona "Sí". Si completaste la instalación sin esta configuración, puedes configurarlo más tarde con:

```bash
sudo dpkg-reconfigure wireshark-common
```

* [ ] Agrega tu usuario al grupo de Wireshark:

```bash
sudo usermod -aG wireshark $USER
```

> ***NOTA:*** $USER es el nombre de usuario que usas en la máquina virtual Debian.

Una vez completada la instalación, cierra sesión y vuelve a iniciarla para aplicar los cambios de grupo. Puedes iniciar Wireshark ejecutando:

```bash
sudo wireshark
```
 
### Monitorización y Análisis

* [ ] Con Wireshark abierto, pulse el botón «play» para empezar a capturar paquetes.
* [ ] Aplica filtros para centrarte en tipos específicos de tráfico, como ARP, TCP, UDP, etc.

![Monitereo y analisis con wireshark](https://github.com/breatheco-de/spoofing-and-DoS-lab/raw/dc2d83a7772cb56ce82de4793f60e704be11995d/assets/monitoring-spoof.png)

### Paso 4: Práctica de DoS - Inundación ICMP

Para realizar un ataque de inundación ICMP (ping flood) desde Kali a Debian, puede utilizar el siguiente comando:

```bash
sudo hping3 -1 <IP_debian> -I eth0
```

* hping3: Una herramienta de línea de comandos para generar paquetes TCP/IP que pueden usarse para varias pruebas de red, incluyendo escaneo de puertos, pruebas de cortafuegos y pruebas de rendimiento de red.
-1: Indica que se deben enviar paquetes ICMP tipo 1 (ICMP Echo Request), que son los paquetes utilizados por el comando ping.
* -I eth0: Especifica la interfaz de red a utilizar para enviar los paquetes. En este caso, eth0 es la interfaz de red de la máquina atacante.

### Monitorización y Análisis

* [ ] Con Wireshark abierto, haz clic en el botón «play» para empezar a capturar paquetes.
* [ ] Mientras hping3 se está ejecutando, puedes aplicar un filtro para ver sólo el tráfico ICMP. El filtro es icmp.

![Monitoreo con DoS](https://github.com/breatheco-de/spoofing-and-DoS-lab/raw/dc2d83a7772cb56ce82de4793f60e704be11995d/assets/monitoring-DoS.png)

## Discusión sobre Estrategias de Mitigación

* [ ] Guiar a los estudiantes sobre las herramientas de monitorización disponibles en Kali Linux, como `htop`, para observar el impacto del ataque DoS en el servidor WordPress.
* [ ] Los estudiantes deben monitorizar la capacidad de respuesta del servidor WordPress, la tasa de errores y el uso de recursos del sistema durante el ataque.
* [ ] Debate sobre las estrategias de mitigación (10 minutos):
* [ ] Cubrir posibles medidas defensivas, como el uso de firewalls (cortafuegos).
* [ ] Concluir con las mejores prácticas para proteger un sitio WordPress contra ataques DoS y spoofing en el mundo real.

<!-- hide -->

## Colaboradores

Gracias a estas personas maravillosas ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

1. [Rosinni Rodríguez (rosinni)](https://github.com/rosinni) contribución: (build-tutorial) ✅, (documentación) 📖
  
2. [Alejandro Sanchez (alesanchezr)](https://github.com/alesanchezr),  contribución: (detector bugs) 🐛

3. [Lorena Gubaira (lorenagubaira)](https://github.com/lorenagubaira), contribution: (detector bugs) 🐛, contribution: (editor), (tranducción) 🌎

Este proyecto sigue la especificación [all-contributors](https://github.com/kentcdodds/all-contributors). ¡Todas las contribuciones son bienvenidas!

Este y otros ejercicios son usados para [aprender a programar](https://4geeksacademy.com/es/aprender-a-programar/aprender-a-programar-desde-cero) por parte de los alumnos de 4Geeks Academy [Coding Bootcamp](https://4geeksacademy.com/us/coding-bootcamp) realizado por [Alejandro Sánchez](https://twitter.com/alesanchezr) y muchos otros contribuyentes. Conoce más sobre nuestros [Cursos de Programación](https://4geeksacademy.com/es/curso-de-programacion-desde-cero?lang=es) para convertirte en [Full Stack Developer](https://4geeksacademy.com/es/coding-bootcamps/desarrollador-full-stack/?lang=es), o nuestro [Data Science Bootcamp](https://4geeksacademy.com/es/coding-bootcamps/curso-datascience-machine-learning).Tambien puedes adentrarte al mundo de ciberseguridad con nuestro [Bootcamp de ciberseguridad](https://4geeksacademy.com/es/coding-bootcamps/curso-ciberseguridad).
<!-- endhide -->