# TP3 : on va router des trucs

## 1. Echange ARP
🌞Générer des requêtes ARP
<br>marcel 10.3.2.12
<br>john 10.3.2.11
- effectuer un ping d'une machine à l'autre
    - Ping John sur Marcel , ping 10.3.2.12 et les packets sont bien transmits
- observer les tables ARP des deux machines
    - ip neigh show john sur marcel par exemple

    10.3.2.12 dev enp0s8 lladdr 08:00:27:fa:05:2e STALE

    10.3.2.16 dev enp0s8 lladdr 0a:00:27:00:00:12 REACHABLE

- repérer l'adresse MAC de john dans la table ARP de marcel et vice-versa

    - ip neigh show 10.3.2.12 john sur marcel

    10.3.2.12 dev enp0s8 lladdr 08:00:27:fa:05:2e STALE

    - ip neigh show 10.3.2.11 marcel sur john

    10.3.2.11 dev enp0s8 lladdr 08:00:27:8e:02:d1 STALE 

- prouvez que l'info est correcte (que l'adresse MAC que vous voyez dans la table est bien celle de la machine correspondante)
    - et une commande pour afficher la MAC de marcel, depuis marcel

        - ip neigh show 10.3.2.11 marcel sur marcel

            10.3.2.11 dev enp0s8 lladdr 08:00:27:8e:02:d1 STALE

## 2. Analyse de trames

🌞Analyse de trames

- utilisez la commande tcpdump pour réaliser une capture de trame
    -  sudo tcpdump -i enp0s8 -c 10
- videz vos tables ARP, sur les deux machines, puis effectuez un ping
    - sudo ip neigh flush all et ping 10.3.2.12 et ping 10.3.2.12
    
    les packets sont bien transmits 