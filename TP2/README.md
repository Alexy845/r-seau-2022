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

- résultats : <br>
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
