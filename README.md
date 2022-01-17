<div id="top"></div>



<!-- PROJECT LOGO -->
<br />
<div align="center">
    <img src="images/logo.png" alt="Logo" width="700" height="400">
  <h2 align="center">TP 2</h2>
  <h3 align="center">Attaques actives</h3>
</div>



<!-- TABLE OF CONTENTS -->

  <summary>Table of Contents</summary>
  <ol>
   <li><a href="#Objectifs-de-ce-TP">Objectifs de ce TP</a></li>
   <li><a href="#Outils-logiciels">Outils logiciels</a></li>
      <li>
        <a href="#Implémentation-attaques">Implémentation d’attaques</a>
           <ul>
              <li><a href="#Exercice-1">Exercice 1 : my_ping.c</a></li>
              <li><a href="#Exercice-2">Exercise 2 : pingsur2frag.c</a></li>
              <li><a href="#Exercice-3">Exercice 3 : pingfragments.c</a></li>
              <li><a href="#Exercice-4">Exercice 4 : demandeconntcp.c</a></li>
           </ul>
        </li>
    <li><a href="#Test-de-quelques-outils">Test de quelques outils d’attaques</a></li>
   </ol>

# Les attaques actives

> Les attaques actives sont mises en place par l’injection, la modification ou la suppression de
> paquets. L’attaquant peut ainsi laisser les traces des attaques qu’il lance. Ces traces peuvent
> être exploitées par l’administrateur réseau pour déceler l’existence d’attaques et identifier
> l’attaquant si possible. 



# Objectifs-de-ce-TP
> - Implémenter quelques attaques et les tester
> - Mise en place de quelques attaques en utilisant des outils d’attaques

# Outils-logiciels
> - Linux, 
> - wireshark ou ethereal, 
> - utilitaire ARPflood, 
> - utilitaire dhcpstarv, 
> - logiciel “cat Karat packet
builder”

# Implémentation-attaques

> Vous trouvez avec le présent fichier, les 4 exercices ci-dessous déjà mis sous forme de
> programmes C. (`pingfragments.c`, `pingsur2frag.c` ,`demandeconntcp.`c , `my_ping.c`)
> Vous pouvez compiler ces programmes sous un interpréteur de commandes shell unix en
> utilisant le compilateur cc ou gcc.
> 
> Exemple :
> - `cc -c my_ping.c`  pour compiler
> - `cc my_ping.c –o myping`  pour générer l’exécutable
> - `./myping 127.0.0.1 127.0.0.1 500`  pour envoyer un paquet de taille 528
> octets=500+20+8

# Exercice-1

Ce premier programme concerne un paquet IP encapsulant un paquet ICMP echo. Il s’agit
d’envoyer un paquet ICMP echo d’une machine A en donnant comme adresse destination
celle de B et comme adresse source celle de C. Vous devez observez en utilisant un sniffer
(ethereal ou tcpdump) un paquet icmp echo et sa réponse transmise de la machine B vers la
machine C. Avec ce premier exercice vous serez donc capable de générer un paquet IP et donc
de maîtriser parfaitement les différents champs de IP


<div align="center">
    <img src="images/myping.png">
</div>

<div align="center">
    <img src="images/tcpMyPing.png">
</div>

<p align="right">(<a href="#top">back to top</a>)</p>


# Exercice-2
Le deuxième exercice consiste à concevoir deux fragments de paquet IP contenant à eux deux
un paquet ICMP echo avec les mêmes types d’adresses que l’exercice précédent. Cet exercice
vous permet de bien maîtriser la conception de fragments IP, ce qui est nécessaire pour
l’attaque Teardrop. Vous remarquerez dans cette exercice que pour l’offset on fait un décalage
de trois bits ….Dans le cas on vous ne comprenez ce calcul. Il vous est demander de faire un
ping avec une taille de 2000 octets sur le lien ethernet et avec le sniffer vous analyserez les
différents champs des fragments IP ainsi générés.

<div align="center">
    <img src="images/pingFrag.png">
</div>

<div align="center">
    <img src="images/tcpPingFrag.png">
</div>

<p align="right">(<a href="#top">back to top</a>)</p>

# Exercice-3

<div align="center">
    <img src="images/ping2.png">
</div>

<div align="center">
    <img src="images/tcpPing2.png">
</div>

<p align="right">(<a href="#top">back to top</a>)</p>

# Exercice-4

<div align="center">
    <img src="images/demande.png">
</div>

<div align="center">
    <img src="images/tcpdemande.png">
</div>

<p align="right">(<a href="#top">back to top</a>)</p>


## Test-de-quelques-outils

Dhcp starv :

> DHCP configuration:

`sudo nano /etc/dhcp/dhcpd.conf`

```sh

authoritative;
default-lease-time 600;
max-lease-time 7200;

subnet 192.168.1.0 netmask 255.255.255.0
{
        range 192.168.1.50 192.168.1.80;
        option routers 192.168.1.1;
        interface eth1;
}

```
 `sudo nano /etc/network/interfaces`

```sh
auto lo
iface lo inet loopback

allow-hotplug eth1
iface eth1 inet static
        address 192.168.1.1
        gateway 192.168.1.1
```

Manipulation :
```sh
tar –xvf dhcpstarv-0.2.1.tar.gz
cd dhcpstarv-0.2.1
./configure
make
make install
```

<p align="right">(<a href="#top">back to top</a>)</p>



# Attaque “Man In The Middle” basée sur l'attaque “ARP spoofing”

Pour réaliser cette attaque, nous avons besoin de trois nœuds connectés à un switch (cas réel ou sur 
GNS3) ou à un point d'accès sans fil. Le nœud attaquant sera la machine sur laquelle est installé le 
système d'exploitation « Kali Linux »

> les tables arp avant l'attaque 

![WhatsApp Image 2022-01-09 at 1 34 41 PM (1)](https://user-images.githubusercontent.com/85891554/148683986-43cd377d-b2d6-4ffa-9faa-92c455682e79.jpeg)

## Etape 1 : activer le routage dans le nœud attaquant (Kali Linux)
Tester si le routage est activé en utilisant la commande sysctl ou en cherchant la valeur de ip_forward
dans /proc/sys/ipv4

```cpp
Root# sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 0
```
Activer le routage :
 ```cpp
 root# sysctl -w net.ipv4.ip_forward=1
 ```
 ![WhatsApp Image 2022-01-09 at 1 34 40 PM](https://user-images.githubusercontent.com/85891554/148683977-f164ced7-6431-496b-b188-84eca5fa7282.jpeg)
 
 ## Etape 2 : empoisonner les tables ARP des nœuds victimes
 
 Lancer une communication entre les deux nœuds légtimes (exemple : ping) puis afficher le contenu de 
leur table arp (arp -a). Exécuter, ensuite, l'attaque arpspoof.

```cpp
root#arpspoof -i eth0 -t 192.168.1.3 192.168.1.4
root#arpspoof -i eth0 -t 192.168.1.4 192.168.1.3
```
![WhatsApp Image 2022-01-09 at 1 34 39 PM](https://user-images.githubusercontent.com/85891554/148683990-821599f6-b064-40c2-aa23-ed6c57d0dc78.jpeg)

on utilise Wireshark pour afficher le trafic capturé par l'attaquant

![WhatsApp Image 2022-01-09 at 1 34 41 PM](https://user-images.githubusercontent.com/85891554/148683982-89957d90-bcec-4a83-b30d-ee870fa679ca.jpeg)
les tables arp apres l'attaque 

![WhatsApp Image 2022-01-09 at 1 34 41 PM (2)](https://user-images.githubusercontent.com/85891554/148684361-d9e26b60-781f-406c-b538-43345a2a8517.jpeg)

# Attaque “usurpation d’identité”
Dans cette partie, nous utiliserons l'outil « cat Karat packet Builder » :
Tout d'abord, on choisit l'interface qu'on veut attaquer, puis on choisit le protocole
et la taille des paquets :
![Capture d’écran 2022-01-10 000202](https://user-images.githubusercontent.com/85891554/148735360-ce8d50a8-fdee-4d6a-8362-df80abb38147.png)

## manip 1:

Maintenant, nous envoyons une requête Arp :
Nous devons déterminer l'adresse IP et l'adresse Mac de la source et de la destination

![Capture d’écran 2022-01-10 000202](https://user-images.githubusercontent.com/85891554/148735360-ce8d50a8-fdee-4d6a-8362-df80abb38147.png)

Nous suivons le trafic en utilisant le wireshark :

![Capture d’écran 2022-01-10 000221](https://user-images.githubusercontent.com/85891554/148735366-3796d3bc-e52e-4781-83e4-c934c58ce70a.png)

## manip 2:

**Nous devons déterminer l'adresse IP et l'adresse Mac de la source et de la destination**

![Capture d’écran 2022-01-10 000202](https://user-images.githubusercontent.com/85891554/148735360-ce8d50a8-fdee-4d6a-8362-df80abb38147.png)
![Capture d’écran 2022-01-10 000221](https://user-images.githubusercontent.com/85891554/148735366-3796d3bc-e52e-4781-83e4-c934c58ce70a.png)

## Attaque “ARP cache poisoning”
# Ettercap 

![WhatsApp Image 2022-01-09 at 1 34 37 PM](https://user-images.githubusercontent.com/85891554/148774118-b1334275-d10f-4231-84ac-d7077e8fa394.jpeg)
![WhatsApp Image 2022-01-09 at 1 34 37 PM (1)](https://user-images.githubusercontent.com/85891554/148774129-7c4309aa-2631-4863-8cd0-46766bcb604f.jpeg)

![WhatsApp Image 2022-01-09 at 1 34 38 PM](https://user-images.githubusercontent.com/85891554/148774136-e528849b-1f3c-4b28-915c-dfd89a0265b6.jpeg)
![WhatsApp Image 2022-01-09 at 1 34 38 PM (1)](https://user-images.githubusercontent.com/85891554/148774143-266064db-6c27-4032-a945-a9b072be429c.jpeg)







Out Team - [AIT EL KADI Ilyas](https://github.com/IlyasKadi) - [AZIZ Oussama](https://github.com/ATAMAN0) - [BENCHEDI Yahia](https://github.com/Ben776ya)

Project Link: [https://github.com/IlyasKadi/Attaques_passives--sniffing_passif](https://github.com/IlyasKadi/Attaques_passives--sniffing_passif)

<p align="right">(<a href="#top">back to top</a>)</p>
