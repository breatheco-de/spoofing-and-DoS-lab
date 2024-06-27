# Spoofing and DDoS on web site

This practice will allow you to better understand spoofing and DDoS techniques, as well as their effects in a web environment.

<!-- hide -->
<a href="https://www.4geeksacademy.co"><img height="280" align="right" src="https://github.com/4GeeksAcademy/deploying-wordpress-debian/blob/master/js-bg-badge.png"></a>


> By [@rosinni](https://github.com/rosinni) and [other contributors](https://github.com/4GeeksAcademy/deploying-wordpress-debian/graphs/contributors) at [4Geeks Academy](https://4geeksacademy.co/)

![last commit](https://img.shields.io/github/last-commit/4geeksacademy/installing-kali-linux-on-virtual-machine)
[![build by developers](https://img.shields.io/badge/build_by-Developers-blue)](https://4geeks.com)
[![build by developers](https://img.shields.io/twitter/follow/4geeksacademy?style=social&logo=twitter)](https://twitter.com/4geeksacademy)

*These instructions are [available in english](https://github.com/4GeeksAcademy/deploying-wordpress-debian/blob/main/README.md)*
<!-- endhide -->

<!-- hide -->
### Before you start...

> We need you! These exercises are built and maintained in collaboration with contributors such as yourself. If you find any bugs or misspellings please contribute and/or report them.
<!-- endhide -->

<!-- hide -->


## 🌱 How to start a project?


### Local Installation:
Clone the repository to your local environment [repository](https://github.com/breatheco-de/spoofing-and-DDoS-lab) and follow the instructions in the README file.


### Requirements

To carry out this project, we will need 2 virtual machines. One will be the Debian virtual machine where we previously built the WordPress website, and the other will be the Kali virtual machine as the attacker.

* Oracle VirtualBox
* Virtual machine with Kali Linux (Attacker).
* Virtual machine with Debian (Web Server): Where we have the Apache server and the WordPress site hosted.
* Spoofing and DDoS tools installed on virtual machines.
* An isolated network environment or a lab network.
* Tools needed?

## 📝 Instructions

### Step 1: Configure the Network in VirtualBox

#### Configure the Network of the Debian Machine (Web Server):
* Open VirtualBox.
* Select your Debian virtual machine and click on "Settings".
* Go to the "Network" section.
* Configure "Adapter 1" as "Internal Network".
* In the "Name" field, enter a name for the internal network, for example, "LabNetwork".
* Save the changes and close the settings window.


#### Configure the Network of the Kali Linux Machine (Attacker):
* Select your Kali Linux virtual machine and click on "Settings".
* Go to the "Network" section.
* Configure "Adapter 1" as "Internal Network".
* In the "Name" field, select the same internal network name you used for the Debian machine ("LabNetwork").
* Save the changes and close the settings window.


![Configurar maquina virtual](assets/config-virtual-machine.png)



### Step 2: Obtain the IP Address of the Machines to Connect Them.

#### On the Debian Machine (Web Server):
* Start the Debian virtual machine.
* Open a terminal and execute the following command to view the assigned IP address:
```bash
$ ip addr show
```
> ***Look for the section corresponding to your network interface (usually `eth0` o `enp0s3`) and find the line that starts with inet. There you will see the assigned IP address, something like `192.168.1.x`.***

#### On the Kali Linux Machine (Attacker):
* Open a terminal and execute the following command to view the assigned IP address:
* Abre una terminal y ejecuta el siguiente comando para ver la dirección IP asignada:

```bash
$ ip addr show
```

> ***Look for the section corresponding to your network interface (usually `eth0` o `enp0s3`) and find the line that starts with inet. There you will see the assigned IP address, something like `192.168.1.x`.***


### Step 3: Verify the Connection Between the Machines

#### From the Kali Linux Machine (Attacker):
* Open a terminal and ping the Debian machine to verify the connection:

```bash
$ ping <IP_debian>
```

> Replace <IP_debian> with the IP address you obtained for the Debian machine.

#### From the Debian Machine (Web Server): 
* Open a terminal and ping the Kali Linux machine to verify the connection:

```bash
$ ping <IP_kali>
```
> Replace <IP_kali> with the IP address you obtained for the Kali machine.

Graphical example of how the pings look when connected:
![Graphical example of how the pings look when connected:](assets/ping-view.png)


### Step 4: 


## Project Delivery






<!-- hide -->
## Contributors

Thanks goes to these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

1. [Rosinni Rodríguez (rosinni)](https://github.com/rosinni) contribution: (build-tutorial) ✅, (documentation) 📖
  
2. [Alejandro Sanchez (alesanchezr)](https://github.com/alesanchezr),  contribution: (bug reports) 🐛

3. [Lorena Gubaira (lorenagubaira)](https://github.com/lorenagubaira), contribution: (bug reports) 🐛, contribution: (editor), (translation) 🌎

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind are welcome!

This and many other exercises are built by students as part of the 4Geeks Academy [Coding Bootcamp](https://4geeksacademy.com/us/coding-bootcamp) by [Alejandro Sánchez](https://twitter.com/alesanchezr) and many other contributors. Find out more about our [Full Stack Developer Course](https://4geeksacademy.com/us/coding-bootcamps/part-time-full-stack-developer), and  [Data Science Bootcamp](https://4geeksacademy.com/us/coding-bootcamps/datascience-machine-learning).You can alse deepdive in the world of cybersecurity with our [Cybersecurity Bootcamp](https://4geeksacademy.com/us/coding-bootcamps/cybersecurity)
<!-- endhide -->