# Spoofing y DDoS a un sitio web

Esta práctica te permitirá entender mejor las técnicas de spoofing y DDoS, así como sus efectos en un entorno web. 


<!-- hide -->

> By [@rosinni](https://github.com/rosinni) and [other contributors](https://github.com/4GeeksAcademy/deploying-wordpress-debian/graphs/contributors) at [4Geeks Academy](https://4geeksacademy.co/)

![last commit](https://img.shields.io/github/last-commit/4geeksacademy/installing-kali-linux-on-virtual-machine)
[![build by developers](https://img.shields.io/badge/build_by-Developers-blue)](https://4geeks.com)
[![build by developers](https://img.shields.io/twitter/follow/4geeksacademy?style=social&logo=twitter)](https://twitter.com/4geeksacademy)

*These instructions are [available in english](https://github.com/breatheco-de/spoofing-and-DDoS-lab/blob/main/README.md)*
<!-- endhide -->


<!-- hide -->


### Antes de empezar...

> ¡Te necesitamos! Estos ejercicios se crean y mantienen en colaboración con personas como tú. Si encuentras algún error o falta de ortografía, contribuye y/o repórtalo.

<!-- endhide -->

## 🌱 ¿Cómo empezar este proyecto?

### Instalación local:

Clona el repositorio en tu ambiente local [repositorio](https://github.com/breatheco-de/spoofing-and-DDoS-lab) y sigue las instrucciones en el archivo readme.



### Requisitos

Para llevar a cabo este proyecto vamos a necesitar 2 maquinas virtuales. Una de ellas será la maquina virtual de debian donde construimos el sitio web de wordpress anteriormente y la otra la maquina atacante con kali.

* Oracle VirtualBox
* Máquina virtual con Kali Linux (Atacante)
* Máquina virtual con Debian (Servidor Web): Donde tenemos alojado el servidor Apache y el sitio WordPress.
* Herramientas de spoofing y DDoS instaladas en máquinas virtuales.
* Un entorno de red aislado.
* Herramientas Necesarias?

## 📝 Instrucciones

### Paso 1: Configurar la Red en VirtualBox

#### Configurar la Red de la Máquina Debian (Servidor Web):
* Abre VirtualBox.
* Selecciona tu máquina virtual con Debian y haz clic en "Configuración".
* Ve a la sección "Red".
* Configura el "Adaptador 1" como "Red Interna" (Internal Network).
* En el campo "Nombre", escribe un nombre para la red interna, por ejemplo, "LabNetwork".
* Guarda los cambios y cierra la ventana de configuración.


#### Configurar la Red de la Máquina Kali Linux (Atacante):
* Selecciona tu máquina virtual con Kali Linux y haz clic en "Configuración".
* Ve a la sección "Red".
* Configura el "Adaptador 1" como "Red Interna" (Internal Network).
* En el campo "Nombre", selecciona el mismo nombre de red interna que utilizaste para la máquina Debian ("LabNetwork").
* Guarda los cambios y cierra la ventana de configuración.

![Configurar maquina virtual](assets/config-virtual-machine.png)

### Paso 2: Obtener la Dirección IP de las Máquinas para poderlas conectar entre sí.

#### En la Máquina Debian (Servidor Web):
* Inicia la máquina virtual Debian.
* Abre una terminal y ejecuta el siguiente comando para ver la dirección IP asignada:

```bash
$ ip addr show
```
> ***Busca la sección correspondiente a tu interfaz de red (usualmente `eth0` o `enp0s3`) y encuentra la línea que dice inet. Ahí verás la dirección IP asignada, algo como `192.168.1.x`.***

#### En la Máquina Kali Linux (Atacante):
* Inicia la máquina virtual Kali Linux.
* Abre una terminal y ejecuta el siguiente comando para ver la dirección IP asignada:

```bash
$ ip addr show
```

> ***Busca la sección correspondiente a tu interfaz de red (usualmente `eth0` o `enp0s3`) y encuentra la línea que dice inet. Ahí verás la dirección IP asignada, algo como `192.168.1.x`.***


### PASO 3: Verificar la Conexión Entre las Máquinas

#### Desde la Máquina Kali Linux (Atacante):
* Abre una terminal y haz ping a la máquina Debian para verificar la conexión:

```bash
$ ping <IP_debian>
```

> Reemplaza <IP_debian> con la dirección IP que obtuviste para la máquina Debian.

#### Desde la Máquina Debian (Servidor Web):
* Abre una terminal y haz ping a la máquina Kali Linux para verificar la conexión:

```bash
$ ping <IP_kali>
```

> Reemplaza <IP_kali> con la dirección IP que obtuviste para la máquina Kali.

Ejemplo gráfico de cómo se ven los ping al estar conectados
![verificación de conexión entre las maquinas virtuales](assets/ping-view.png)


### PASO 4: 
## Entrega de proyecto

En el repositorio que se ha clonado debes entregar 2 informes.






## Colaboradores

Gracias a estas personas maravillosas ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

1. [Rosinni Rodríguez (rosinni)](https://github.com/rosinni) contribución: (build-tutorial) ✅, (documentación) 📖
  
2. [Alejandro Sanchez (alesanchezr)](https://github.com/alesanchezr),  contribución: (detector bugs) 🐛

3. [Lorena Gubaira (lorenagubaira)](https://github.com/lorenagubaira), contribution: (detector bugs) 🐛, contribution: (editor), (tranducción) 🌎

Este proyecto sigue la especificación [all-contributors](https://github.com/kentcdodds/all-contributors). ¡Todas las contribuciones son bienvenidas!

Este y otros ejercicios son usados para [aprender a programar](https://4geeksacademy.com/es/aprender-a-programar/aprender-a-programar-desde-cero) por parte de los alumnos de 4Geeks Academy [Coding Bootcamp](https://4geeksacademy.com/us/coding-bootcamp) realizado por [Alejandro Sánchez](https://twitter.com/alesanchezr) y muchos otros contribuyentes. Conoce más sobre nuestros [Cursos de Programación](https://4geeksacademy.com/es/curso-de-programacion-desde-cero?lang=es) para convertirte en [Full Stack Developer](https://4geeksacademy.com/es/coding-bootcamps/desarrollador-full-stack/?lang=es), o nuestro [Data Science Bootcamp](https://4geeksacademy.com/es/coding-bootcamps/curso-datascience-machine-learning).Tambien puedes adentrarte al mundo de ciberseguridad con nuestro [Bootcamp de ciberseguridad](https://4geeksacademy.com/es/coding-bootcamps/curso-ciberseguridad).
<!-- endhide -->
