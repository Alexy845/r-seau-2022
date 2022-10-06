
# TP1 - Premier pas réseau

# Déroulement et rendu du TP

# I. Exploration locale en solo

## 1. Affichage d'informations sur la pile TCP/IP locale

### En ligne de commande

🌞 **Affichez les infos des cartes réseau de votre PC**

- nom, adresse MAC et adresse IP de l'interface WiFi
<br>
<br>
Carte réseau sans fil Wi-Fi , F8-B5-4D-6C-58-0E , 10.33.16.103
<br>
<br>
- nom, adresse MAC et adresse IP de l'interface Ethernet
<br>
<br>
Carte Ethernet Ethernet, D8-BB-C1-B6-05-39 pas trouvé car il n'est connecté à rien.

🌞 **Affichez votre gateway**

Passerelle par défaut: 10.33.19.254

🌞 **Déterminer la MAC de la passerelle**
<br>
 arp -a : passerelle par défaut :10.33.19.254  / Mac de la passerelle: 00-c0-e7-e0-04-4e

### En graphique (GUI : Graphical User Interface)

En utilisant l'interface graphique de votre OS :  

🌞 **Trouvez comment afficher les informations sur une carte IP (change selon l'OS)**

Panneau de configuration > Réseau et Internet > Centre Réseau et partage > Wi-Fi (WiFi@YNOV) > Détails...

- Adresse IPv4 : 10.33.16.103 , Adresse physique: F8-B5-4D-6C-58-0E , gateway : 10.33.19.254

## 2. Modifications des informations

### A. Modification d'adresse IP (part 1)  

🌞 **Utilisez l'interface graphique de votre OS pour changer d'adresse IP :**

Paramètres > Réseau et Internet > Wi-Fi > Propriétés de WiFi@YNOV

Modifier les paramètres IP pour tous les réseaux Wi-Fi

- changez l'adresse IP de votre carte WiFi pour une autre
10.33.19.104

🌞 **Il est possible que vous perdiez l'accès internet.** Que ce soit le cas ou non, expliquez pourquoi c'est possible de perdre son accès internet en faisant cette opération.**

Car le réseau n'arrive pas à détecter la nouvelle adresse IP.

# II. Exploration locale en duo

## 1. Prérequis

## 2. Câblage

## Création du réseau (oupa)

## 3. Modification d'adresse IP

🌞 **Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau**

- choisissez une IP qui commence par "10.10.10."
<br>
<br>
IP choisie: 10.10.10.21

🌞 **Vérifier à l'aide d'une commande que votre IP a bien été changée**

commande: ipconfig /all

Carte Ethernet Ethernet :
Adresse IPv4. . . . . . . . . . . . . .: 10.10.10.21(préféré)

La commande affiche bien 10.10.10.21, l'IP a bien été changé.

🌞 **Vérifier que les deux machines se joignent**

- utilisez la commande `ping` pour tester la connectivité entre les deux machines

commande utilisée: ping 10.10.10.22

Statistiques Ping pour 10.10.10.22:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 3ms, Maximum = 4ms, Moyenne = 3ms


🌞 **Déterminer l'adresse MAC de votre correspondant**

- pour cela, affichez votre table ARP

arp -a

Interface : 10.10.10.21 --- 0x12

Adresse Internet      Adresse physique      Type
  10.10.10.22           a8-5e-45-37-f4-45     dynamique

## 4. Utilisation d'un des deux comme gateway    

🌞**Tester l'accès internet**

- essayez de ping l'adresse IP `1.1.1.1`, c'est un serveur connu de CloudFlare (demandez-moi si vous comprenez pas trop la démarche)

ping 1.1.1.1

Statistiques Ping pour 1.1.1.1:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 24ms, Maximum = 30ms, Moyenne = 26ms

🌞 **Prouver que la connexion Internet passe bien par l'autre PC**

tracert 1.1.1.1

Détermination de l'itinéraire vers one.one.one.one [1.1.1.1]
avec un maximum de 30 sauts :

  1     4 ms     3 ms     3 ms  LAPTOP-EV3OHFA1 [192.168.137.1]

La connexion Internet passe alors bien sur l'autre PC.

## 5. Petit chat privé


🌞 **sur le PC *serveur*** avec par exemple l'IP 192.168.1.1
- `nc.exe -l -p 8888`
  - "`netcat`, écoute sur le port numéro 8888 stp"
- il se passe rien ? Normal, faut attendre qu'un client se connecte

Commande : .\nc64.exe -l -p 8888

🌞 **sur le PC *client*** avec par exemple l'IP 192.168.1.2

- `nc.exe 192.168.1.1 8888`
  - "`netcat`, connecte toi au port 8888 de la machine 192.168.1.1 stp"
- une fois fait, vous pouvez taper des messages dans les deux sens
- appelez-moi quand ça marche ! :)
- si ça marche pas, essayez d'autres options de `netcat`

---

.\nc64.exe 10.10.10.22 8888

Les messages s'envoies correctement dans les deux sens.

🌞 **Visualiser la connexion en cours**

- sur tous les OS, il existe une commande permettant de voir les connexions en cours
- ouvrez un deuxième terminal pendant une session `netcat`, et utilisez la commande correspondant à votre OS pour repérer la connexion `netcat` :

netstat -a -n -b

  TCP    10.10.10.21:55711      10.10.10.22:8888       ESTABLISHED
 [nc64.exe]

🌞 **Pour aller un peu plus loin**

- si vous faites un `netstat` sur le serveur AVANT que le client `netcat` se connecte, vous devriez observer que votre serveur `netcat` écoute sur toutes vos interfaces
  - c'est à dire qu'on peut s'y connecter depuis la wifi par exemple :D
- il est possible d'indiquer à `netcat` une interface précise sur laquelle écouter
  - par exemple, on écoute sur l'interface Ethernet, mais pas sur la WiFI

Commande : .\nc64.exe -l -p 8888

 Commande : netstat -a -n -b | Select-String 8888

 TCP    0.0.0.0:8888           0.0.0.0:0              LISTENING

 Commande : .\nc64.exe -l -p 8888 -s 10.10.10.22

 Commande : netstat -a -n -b | Select-String 8888

 TCP    10.10.10.22:8888       0.0.0.0:0              LISTENING

```bash
# Sur Windows/MacOS
$ nc.exe -l -p PORT_NUMBER -s IP_ADDRESS
# Par exemple
$ nc.exe -l -p 9999 -s 192.168.1.37
```

## 6. Firewall

Toujours par 2.

Le but est de configurer votre firewall plutôt que de le désactiver

🌞 **Activez et configurez votre firewall**

- autoriser les `ping`
  - configurer le firewall de votre OS pour accepter le `ping`
  - aidez vous d'internet
  - on rentrera dans l'explication dans un prochain cours mais sachez que `ping` envoie un message *ICMP de type 8* (demande d'ECHO) et reçoit un message *ICMP de type 0* (réponse d'écho) en retour
- autoriser le traffic sur le port qu'utilise `nc`
  - on parle bien d'ouverture de **port** TCP et/ou UDP
  - on ne parle **PAS** d'autoriser le programme `nc`
  - choisissez arbitrairement un port entre 1024 et 20000
  - vous utiliserez ce port pour communiquer avec `netcat` par groupe de 2 toujours
  - le firewall du *PC serveur* devra avoir un firewall activé et un `netcat` qui fonctionne

Après réactivation et configuration du pare-feu de sorte à autoriser le ping,

commande utilisée: ping 10.10.10.22

résultat obtenu:

Envoi d'une requête 'Ping'  10.10.10.22 avec 32 octets de données :

Réponse de 10.10.10.22 : octets=32 temps=3 ms TTL=128

Réponse de 10.10.10.22 : octets=32 temps=3 ms TTL=128

Réponse de 10.10.10.22 : octets=32 temps=3 ms TTL=128

Réponse de 10.10.10.22 : octets=32 temps=3 ms TTL=128

Statistiques Ping pour 10.10.10.22:

Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),

Durée approximative des boucles en millisecondes :

Minimum = 3ms, Maximum = 3ms, Moyenne = 3ms

La commande fonctionne correctement même avec le pare-feu
En reconnectant le serveur la connexion fonctionne et l'échange de messages est possible dans les 2 sens.
  
# III. Manipulations d'autres outils/protocoles côté client

## 1. DHCP

Bon ok vous savez définir des IPs à la main. Mais pour être dans le réseau YNOV, vous l'avez jamais fait.  

C'est le **serveur DHCP** d'YNOV qui vous a donné une IP.

Une fois que le serveur DHCP vous a donné une IP, vous enregistrer un fichier appelé *bail DHCP* qui contient, entre autres :

- l'IP qu'on vous a donné
- le réseau dans lequel cette IP est valable

🌞**Exploration du DHCP, depuis votre PC**

- afficher l'adresse IP du serveur DHCP du réseau WiFi YNOV

commande effectuée: ipconfig /all
résulat obtenue pour l'adresse DHCP du réseau WiFi YNOV :
Serveur DHCP . . . . . . . . . . . . . : 10.33.19.254

- cette adresse a une durée de vie limitée. C'est le principe du ***bail DHCP*** (ou *DHCP lease*). Trouver la date d'expiration de votre bail DHCP

Bail expirant. . . . . . . . . . . . . : vendredi 7 octobre 2022 08:46:16


- vous pouvez vous renseigner un peu sur le fonctionnement de DHCP dans les grandes lignes. On aura un cours là dessus :)

> Chez vous, c'est votre box qui fait serveur DHCP et qui vous donne une IP quand vous le demandez.



## 2. DNS

Le protocole DNS permet la résolution de noms de domaine vers des adresses IP. Ce protocole permet d'aller sur `google.com` plutôt que de devoir connaître et utiliser l'adresse IP du serveur de Google.  

Un **serveur DNS** est un serveur à qui l'on peut poser des questions (= effectuer des requêtes) sur un nom de domaine comme `google.com`, afin d'obtenir les adresses IP liées au nom de domaine.  

Si votre navigateur fonctionne "normalement" (il vous permet d'aller sur `google.com` par exemple) alors votre ordinateur connaît forcément l'adresse d'un serveur DNS. Et quand vous naviguez sur internet, il effectue toutes les requêtes DNS à votre place, de façon automatique.

🌞** Trouver l'adresse IP du serveur DNS que connaît votre ordinateur**

Serveurs DNS. . .  . . . . . . . . . . :
8.8.8.8

8.8.8.4

1.1.1.1

🌞 Utiliser, en ligne de commande l'outil `nslookup` (Windows, MacOS) ou `dig` (GNU/Linux, MacOS) pour faire des requêtes DNS à la main

- faites un *lookup* (*lookup* = "dis moi à quelle IP se trouve tel nom de domaine")
  - pour `google.com`

  commande utilisée: nslookup google.com
Serveur :   dns.google
Address:  8.8.8.8
Réponse ne faisant pas autorité :
Nom :    google.com
Addresses:  2a00:1450:4007:80f::200e
142.250.179.110

  - pour `ynov.com`

  commande utilisée: nslookup ynov.com

Serveur :   dns.google
Address:  8.8.8.8

Réponse ne faisant pas autorité :
Nom :    ynov.com
Addresses:  2606:4700:20::ac43:4ae2
2606:4700:20::681a:be9
2606:4700:20::681a:ae9
104.26.10.233
172.67.74.226
104.26.11.233
  - interpréter les résultats de ces commandes

  Ces commandes nous montre les différentes adresses IP des différents serveurs liées au nom de domaine recherché, via a une demande au serveur dns.google et une réponse envoyer par ce dernier.
Donc ici on peut voir que Ynov possède différents serveur
- déterminer l'adresse IP du serveur à qui vous venez d'effectuer ces requêtes

Serveur :   dns.google
Address:  8.8.8.8


- faites un *reverse lookup* (= "dis moi si tu connais un nom de domaine pour telle IP")


  - pour l'adresse `78.73.21.21`

commande utilisée: nslookup 231.34.113.12

Serveur :   dns.google
Address:  8.8.8.8
*** dns.google ne parvient pas à trouver 231.34.113.12 : Non-existent domain

  - pour l'adresse `22.146.54.58`

  commande utilisée: nslookup 78.34.2.17

Serveur :   dns.google
Address:  8.8.8.8
Nom :    cable-78-34-2-17.nc.de
Address:  78.34.2.17
  - interpréter les résultats
  - *si vous vous demandez, j'ai pris des adresses random :)*

  On demande ici au serveur DNS de google si l'adresse que l'on recherche est liée à un nom de domaine et donc si il existe.



# IV. Wireshark

**Wireshark est un outil qui permet de visualiser toutes les trames qui sortent et entrent d'une carte réseau.**

On appelle ça un **sniffer**, ou **analyseur de trames.**

![Wireshark](./pics/wireshark.jpg)

Il peut :

- enregistrer le trafic réseau, pour l'analyser plus tard
- afficher le trafic réseau en temps réel

**On peut TOUT voir.**

Un peu austère aux premiers abords, une manipulation très basique permet d'avoir une très bonne compréhension de ce qu'il se passe réellement.

➜ **[Téléchargez l'outil Wireshark](https://www.wireshark.org/).**

🌞 Utilisez le pour observer les trames qui circulent entre vos deux carte Ethernet. Mettez en évidence :

- un `ping` entre vous et votre passerelle

<img src="image TP1/image 0.png"/>

- un `netcat` entre vous et votre mate, branché en RJ45

<img src="image TP1/e.png"/>

- une requête DNS. Identifiez dans la capture le serveur DNS à qui vous posez la question.
- prenez moi des screens des trames en question
- on va prendre l'habitude d'utiliser Wireshark souvent dans les cours, pour visualiser ce qu'il se passe

# Bilan

**Vu pendant le TP :**

- visualisation de vos interfaces réseau (en GUI et en CLI)
- extraction des informations IP
  - adresse IP et masque
  - calcul autour de IP : adresse de réseau, etc.
- connaissances autour de/aperçu de :
  - un outil de diagnostic simple : `ping`
  - un outil de scan réseau : `nmap`
  - un outil qui permet d'établir des connexions "simples" (on y reviendra) : `netcat`
  - un outil pour faire des requêtes DNS : `nslookup` ou `dig`
  - un outil d'analyse de trafic : `wireshark`
- manipulation simple de vos firewalls

**Conclusion :**

- Pour permettre à un ordinateur d'être connecté en réseau, il lui faut **une liaison physique** (par câble ou par *WiFi*).  
- Pour réceptionner ce lien physique, l'ordinateur a besoin d'**une carte réseau**. La carte réseau porte une adresse MAC  
- **Pour être membre d'un réseau particulier, une carte réseau peut porter une adresse IP.**
Si deux ordinateurs reliés physiquement possèdent une adresse IP dans le même réseau, alors ils peuvent communiquer.  
- **Un ordintateur qui possède plusieurs cartes réseau** peut réceptionner du trafic sur l'une d'entre elles, et le balancer sur l'autre, servant ainsi de "pivot". Cet ordinateur **est appelé routeur**.
- Il existe dans la plupart des réseaux, certains équipements ayant un rôle particulier :
  - un équipement appelé *passerelle*. C'est un routeur, et il nous permet de sortir du réseau actuel, pour en joindre un autre, comme Internet par exemple
  - un équipement qui agit comme **serveur DNS** : il nous permet de connaître les IP derrière des noms de domaine
  - un équipement qui agit comme **serveur DHCP** : il donne automatiquement des IP aux clients qui rejoigne le réseau
  - **chez vous, c'est votre Box qui fait les trois :)**