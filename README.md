<!-- hide -->
# Spoofing and DoS on web site
<!-- endhide -->

This practice will allow you to better understand spoofing and DoS techniques, as well as their effects in a web environment.

<!-- hide -->
> By [@rosinni](https://github.com/rosinni) and [other contributors](https://github.com/4GeeksAcademy/deploying-wordpress-debian/graphs/contributors) at [4Geeks Academy](https://4geeksacademy.co/)

[![build by developers](https://img.shields.io/badge/build_by-Developers-blue)](https://4geeks.com)
[![build by developers](https://img.shields.io/twitter/follow/4geeksacademy?style=social&logo=twitter)](https://twitter.com/4geeksacademy)

*These instructions are [available in spanish](https://github.com/breatheco-de/spoofing-and-DoS-lab/blob/main/README.es.md)*

### Before you start...

> We need you! These exercises are built and maintained in collaboration with contributors such as yourself. If you find any bugs or misspellings please contribute and/or report them.
<!-- endhide -->

<onlyfor saas="true" withBanner="false">
    
## 🌱 How to start a project?

Do not clone this or any repository, follow the instructions below:

### Requirements

For this specific practice of Spoofing and DoS, it is best to set up the network as an Internal Network. This is due to several reasons:

- **Complete Isolation:** Ensures that attack and test activities do not interfere with the production network or other external networks. This prevents any potential unwanted impact on other systems.

- **Controlled Environment:** Allows complete control over the network environment, facilitating the monitoring and analysis of network traffic generated during the tests.

- **Realistic Simulation:** Although isolated, an internal network can effectively simulate a real network environment for Spoofing and DoS practices.

### Tools and Virtual Machines

* Oracle VirtualBox
* Virtual machine with Kali Linux (Attacker)
* Virtual machine with Debian (Web Server): Where we have the Apache server and the WordPress site hosted.
* Spoofing and DoS tools installed on virtual machines.
* An isolated network environment.
* Necessary tools: **arpspoof, hping3, wireshark**

</onlyfor>

## 📝 Instructions

### Step 1: Configure the Network in VirtualBox

#### Configure the Network of the Debian Machine (Web Server):
* [ ] Open VirtualBox.
* [ ] Select your Debian virtual machine and click "Settings".
* [ ] Go to the "Network" section.
* [ ] Set "Adapter 1" to "Internal Network".
* [ ] In the "Name" field, enter a name for the internal network, for example, "LabNetwork".
* [ ] Start the machine and check the available network interface and its current settings using the following command in the terminal:

````bash
ip addr show
````

> *You will generally find lines labeled as eth0, enp0s3, wlan0, etc. The one that is active and has an assigned IP address will be the interface you are using.*

* [ ] Manual IP Configuration to use the internal network in the `/etc/network/interfaces` file with the following command:

```bash
sudo nano /etc/network/interfaces
```

* [ ] Add the following to the file that opens:

```plaintext:
auto enp0s3
iface enp0s3 inet static
    			address 192.168.1.10
    			netmask 255.255.255.0
    			gateway 192.168.1.1
```

* [ ] Save the changes and close the configuration window.

![Manual setting dof IPs](https://github.com/breatheco-de/spoofing-and-DoS-lab/blob/dc2d83a7772cb56ce82de4793f60e704be11995d/assets/ip-config.png?raw=true)

> *Example configuration for both files (Kali and Debian), may contain more commands than shown, but add any missing commands from the previously provided configuration, as they will be necessary.*

* [ ] Restart the network service:

```bash
sudo systemctl restart networking
```

* [ ] Use the **ip addr** show command again and verify that your IP address is the one you configured in the address.

#### Configure the Network of the Kali Linux Machine (Attacker)

* [ ] Select your Kali Linux virtual machine and click "Settings".
* [ ] Go to the "Network" section.
* [ ] Set "Adapter 1" to "Internal Network".
* [ ] In the "Name" field, select the same internal network name you used for the Debian machine ("LabNetwork").
* [ ] Start the machine and check the available network interface and its current settings using the following command in the terminal:

````bash
ip addr show
````

> *You will generally find lines labeled as eth0, enp0s3, wlan0, etc. The one that is active and has an assigned IP address will be the interface you are using.*

* [ ] Manual IP Configuration to use the internal network in the `/etc/network/interfaces` file with the following command:

```bash
sudo nano /etc/network/interfaces
```

* [ ] Add the following to the file that opens:

```plaintext:
auto eth0
iface eth0 inet static
    			address 192.168.1.11
    			netmask 255.255.255.0
    			gateway 192.168.1.1
```

* [ ] Save the changes and close the configuration window.
* [ ] Restart the network service:

```bash
sudo systemctl restart networking
```

* [ ] Use the **ip addr** show command again and verify that your IP address is the one you configured in the address.

### Step 2: Verify the Connection Between the Machines

#### From the Kali Linux Machine (Attacker):

* [ ] Open a terminal and ping the Debian machine to verify the connection:

```bash
$ ping <IP_debian>
```

> Replace <IP_debian> with the IP address you obtained for the Debian machine.

#### From the Debian Machine (Web Server):

* [ ] Open a terminal and ping the Kali Linux machine to verify the connection:

```bash
$ ping <IP_kali>
```

> Replace <IP_kali> with the IP address you obtained for the Kali machine.

*Graphical example of how pings look when connected*

<!-- ![verificación de conexión entre las maquinas virtuales](assets/ping-view.png) -->

### Step 3: ARP Spoofing Practice

To carry out this practice, we will use arpspoof. This tool is used to send spoofed ARP packets to the network, making a device (like the Debian machine) believe that the attacker's MAC address (Kali Linux) is the MAC address of the gateway (router). This can be verified by observing the ARP tables on the Debian machine before and after running arpspoof.

#### On the Kali Linux Machine (Attacker):

* [ ] Install arpspoof:

```bash
sudo apt update
sudo apt install dsniff
```

> *Note: arpspoof is part of the dsniff package..*

 * [ ] Verify if arpspoof is installed:

 ```bash
sudo arpspoof -h
```

* [ ] Run arpspoof to poison the ARP tables of the Debian machine and the gateway:

```bash
sudo arpspoof -i <interfaz_kali> -t <IP_debian> <gateway>
```

* -i <kali_interface>: Specifies the network interface from which ARP packets will be sent, e.g., eth0.
* -t <IP_debian>: Specifies the victim's IP address (the Debian machine in this case).
* <gateway>: Specifies the gateway's IP address. (initially configured in the /etc/network/interfaces file, it is the same for both machines)

#### Monitor with Wireshark on the Debian Machine (Web Server):

Wireshark is ideal for analyzing network traffic, identifying possible attacks, and monitoring security in a network environment.

* [ ] Install Wireshark:

```bash
sudo apt update
sudo apt install wireshark
```

During the installation, you might be asked if non-root users should be able to capture packets. Select "Yes". If you completed the installation without this setting, you can configure it later with:

```bash
sudo dpkg-reconfigure wireshark-common
```

* [ ] Add your user to the Wireshark group:

```bash
sudo usermod -aG wireshark $USER
```

> ***NOTE:*** $USER is the username you use on the Debian virtual machine.

Once the installation is complete, log out and back in to apply the group changes. You can start Wireshark by running:

```bash
sudo wireshark
```

### Monitoring and Analysis

* [ ] With Wireshark open, click the "play" button to start capturing packets.
* [ ] Apply filters to focus on specific types of traffic, such as ARP, TCP, UDP, etc.

![Monitoring and analysis with WireShark](https://github.com/breatheco-de/spoofing-and-DoS-lab/blob/dc2d83a7772cb56ce82de4793f60e704be11995d/assets/monitoring-spoof.png)

### Step 4: DoS - ICMP Flood Practice

To perform an ICMP flooding attack (ping flood) from Kali to Debian, you can use the following command:

```bash
sudo hping3 -1 <IP_debian> -I eth0
```

* hping3: A command-line tool for generating TCP/IP packets that can be used for various network tests, including port scanning, firewall testing, and network performance testing.
* -1: Indicates that ICMP type 1 (ICMP Echo Request) packets should be sent, which are the packets used by the ping command.
* -I eth0: Specifies the network interface to use for sending the packets. In this case, eth0 is the network interface of the attacking machine.

### Monitoring and Analysis

* [ ] With Wireshark open, click the "play" button to start capturing packets.
* [ ] While hping3 is running, you can apply a filter to see only the ICMP traffic. The filter is icmp.

![Monitoring DoS](https://github.com/breatheco-de/spoofing-and-DoS-lab/blob/dc2d83a7772cb56ce82de4793f60e704be11995d/assets/monitoring-DoS.png)
 
## Discussion on Mitigation Strategies

* [ ] Guide students on the monitoring tools available in Kali Linux, such as `htop`, to observe the impact of the DoS attack on the WordPress server.
* [ ] Students should monitor the WordPress server's responsiveness, error rate, and system resource usage during the attack.
* [ ] Discussion on mitigation strategies (10 minutes):
* [ ] Cover possible defensive measures, such as using firewalls.
* [ ] Conclude with best practices for protecting a WordPress site against real-world DoS and spoofing attacks.

<!-- hide -->
## Contributors

Thanks goes to these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

1. [Rosinni Rodríguez (rosinni)](https://github.com/rosinni) contribution: (build-tutorial) ✅, (documentation) 📖
  
2. [Alejandro Sanchez (alesanchezr)](https://github.com/alesanchezr),  contribution: (bug reports) 🐛

3. [Lorena Gubaira (lorenagubaira)](https://github.com/lorenagubaira), contribution: (bug reports) 🐛, contribution: (editor), (translation) 🌎

4. [Tomas Gonzalez (tommygonzaleza)](https://github.com/tommygonzaleza), contribution: (editor)

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind are welcome!

This and many other exercises are built by students as part of the 4Geeks Academy [Coding Bootcamp](https://4geeksacademy.com/us/coding-bootcamp) by [Alejandro Sánchez](https://twitter.com/alesanchezr) and many other contributors. Find out more about our [Full Stack Developer Course](https://4geeksacademy.com/us/coding-bootcamps/part-time-full-stack-developer), and  [Data Science Bootcamp](https://4geeksacademy.com/us/coding-bootcamps/datascience-machine-learning).You can alse deepdive in the world of cybersecurity with our [Cybersecurity Bootcamp](https://4geeksacademy.com/us/coding-bootcamps/cybersecurity)
<!-- endhide -->
