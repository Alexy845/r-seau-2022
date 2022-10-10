# TP2 : Ethernet, IP, et ARP

# I. Setup IP
🌞 Mettez en place une configuration réseau fonctionnelle entre les deux machines
- Commande utilisée : 
<br>
    netsh interface ip set address name="Ethernet" static 10.10.42.18 255.255.252.0

- résulats :

    Ip PC1 : 10.10.42.12
<br>
Ip PC2 : 10.10.42.18
<br>
Ip Réseaux : 10.10.40.0
<br>
Ip Broadcast : 10.10.43.255 
<br>
Masque 255.255.252.0

🌞 Prouvez que la connexion est fonctionnelle entre les deux machines

- commande utilisée :
ping 10.10.42.12

- résultats : 
<br>
Envoi d’une requête 'Ping'  10.10.42.12 avec 32 octets de données :<br>
Réponse de 10.10.42.12 : octets=32 temps=1 ms TTL=128<br>
Réponse de 10.10.42.12 : octets=32 temps=2 ms TTL=128<br>
Réponse de 10.10.42.12 : octets=32 temps=2 ms TTL=128<br>
Réponse de 10.10.42.12 : octets=32 temps=2 ms TTL=128<br>
Statistiques Ping pour 10.10.42.12:<br>
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :<br>
    Minimum = 1ms, Maximum = 2ms, Moyenne = 1ms
    

🌞 Wireshark it <br>
- commande utilisée :
    ping 10.10.42.12

- résultats :

    [Wireshark ping PC2](./ICMP.pcapng)

    type envoyé : 8

    type reçu : 0 

# II. ARP my bro

🌞 Check the ARP table
- commande utilisée : arp -a 

- résulats  : <br>
MAC mate a8-5e-45-37-f4-45 <br> 
MAC gateway 00-c0-e7-e0-04-4e

🌞 Manipuler la table ARP
- arp -d

- avant :


    Interface : 192.168.30.1 --- 0x10

  Adresse Internet      /   Adresse physique /   Type

  192.168.30.255     /   ff-ff-ff-ff-ff-ff   /  statique <br>
  224.0.0.22         /   01-00-5e-00-00-16   /  statique <br>
  224.0.0.251         /  01-00-5e-00-00-fb    / statique<br>
  239.255.255.250      / 01-00-5e-7f-ff-fa     /statique<br>

    Interface : 10.33.16.244 --- 0x12

    Adresse Internet  /    Adresse physique /     Type

    10.33.19.254       /   00-c0-e7-e0-04-4e /    dynamique<br>
  10.33.19.255        /  ff-ff-ff-ff-ff-ff  /   statique<br>
  224.0.0.22        /    01-00-5e-00-00-16   /  statique<br>
  224.0.0.251        /   01-00-5e-00-00-fb    / statique<br>
  239.255.255.250     /  01-00-5e-7f-ff-fa     /statique<br>

    Interface : 192.168.136.1 --- 0x14

  Adresse Internet  /    Adresse physique  /    Type 

  192.168.136.255    /   ff-ff-ff-ff-ff-ff  /   statique<br>
  224.0.0.22       /     01-00-5e-00-00-16   /  statique<br>
  224.0.0.251       /    01-00-5e-00-00-fb    / statique<br>
  239.255.255.250    /   01-00-5e-7f-ff-fa     /statique<br>

- après :

    Interface : 192.168.30.1 --- 0x10
    
  Adresse Internet   /   Adresse physique  /    Type<br>
  224.0.0.22          /  01-00-5e-00-00-16  /   statique

    Interface : 10.33.16.244 --- 0x12

  Adresse Internet     / Adresse physique    /  Type

  10.33.19.254          /00-c0-e7-e0-04-4e    / dynamique<br>
  224.0.0.22         /   01-00-5e-00-00-16     /statique<br>

    Interface : 192.168.136.1 --- 0x14

  Adresse Internet    /  Adresse physique    /  Type

  224.0.0.22           / 01-00-5e-00-00-16    / statique

- le mate réapparé dans la table ARP

🌞 Wireshark it

- [Wireshark ARP](./ARP.pcapng)
