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

<div align="center">
    <img src="images/myping.png">
</div>

<div align="center">
    <img src="images/tcpMyPing.png">
</div>

<p align="right">(<a href="#top">back to top</a>)</p>


# Exercice-2

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
    <img src="images/tcpMyPing2.png">
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



<p align="right">(<a href="#top">back to top</a>)</p>








Out Team - [AIT EL KADI Ilyas](https://github.com/IlyasKadi) - [AZIZ Oussama](https://github.com/ATAMAN0) - [BENCHEDI Yahia](https://github.com/Ben776ya)

Project Link: [https://github.com/IlyasKadi/Attaques_passives--sniffing_passif](https://github.com/IlyasKadi/Attaques_passives--sniffing_passif)

<p align="right">(<a href="#top">back to top</a>)</p>
