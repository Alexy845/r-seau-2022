# TP4 : TCP, UDP et services réseau

🌞 Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP

- avec Wireshark, on va faire les chirurgiens réseau

    - opera(vidéo youtube)

         IP et port du serveur `142.250.178.142:443`

         Port local `50638`

            [opera.exe]
            UDP    [::]:5353              *:* 
        
         [wireshark Opera](./Opera.pcapng)

    - spotify(musique)

         IP et port du serveur `35.186.224.25:443`

         Port local `57479`

          [Spotify.exe]
           UDP    10.3.1.15:57479          *:*

         [wireshark Spotify](./Spotify.pcapng)

    - Valorant(partie entrainement)

         IP et port du serveur `162.249.72.1:7428`

         Port local `64627`

          [RiotClientServices.exe]
          UDP    0.0.0.0:64627          *:*
         [wireshark Valorant](./Valorant.pcapng)

    - discord

         IP et port du serveur `162.159.136.234:443`

         Port local `57734`

           [Discord.exe]
            TCP    10.33.16.244:57734     162.159.136.234:443    ESTABLISHED

         [wireshark Discord](./Discord.pcapng)
    - epic game

         IP et port du serveur `52.73.231.66:443`

         Port local `61895`

          [EpicGamesLauncher.exe]
          TCP    10.33.16.244:61895     52.73.231.66:443

         [wireshark Epic games](./Epic%20game.pcapng)

         


# II. Mise en place

## 1. SSH
🖥️ Machine node1.tp4.b1

🌞 Examinez le trafic dans Wireshark

[Wireshark ssh](./sshtri.pcapng)

🌞 Demandez aux OS
 - netstat -n -b
 ```powershell
TCP    10.4.1.20:49724        10.4.1.11:22           ESTABLISHED
[ssh.exe]
```
```powershell
ss
tcp     ESTAB   0        52                           10.4.1.11:ssh          10.4.1.20:49724
```

# III. DNS

## 1. Présentation

🌞 Dans le rendu, je veux

- sudo cat /etc/named.conf
```powershell
//
// named.conf
//
// Provided by Red Hat bind package to configure the ISC BIND named(8) DNS
// server as a caching only nameserver (as a localhost DNS resolver only).
//
// See /usr/share/doc/bind*/sample/ for example named configuration files.
//

options {
        listen-on port 53 { 127.0.0.1; any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; any; };
        allow-query-cache { localhost; any; };


        /*
         - If you are building an AUTHORITATIVE DNS server, do NOT enable recursion.
         - If you are building a RECURSIVE (caching) DNS server, you need to enable
           recursion.
         - If your recursive DNS server has a public IP address, you MUST enable access
           control to limit queries to your legitimate users. Failing to do so will
           cause your server to become part of large scale DNS amplification
           attacks. Implementing BCP38 within your network would greatly
           reduce such attack surface
        */
        recursion yes;

        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";
        geoip-directory "/usr/share/GeoIP";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "tp4.b1" IN {
     type master;
     file "tp4.b1.db";
     allow-update { none; };
     allow-query {any; };
};

zone "1.4.10.in-addr.arpa" IN {
     type master;
     file "tp4.b1.rev";
     allow-update { none; };
     allow-query { any; };
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key"; 
```

```powershell
sudo cat /var/named/tp4.b1.db

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

; Enregistrements DNS pour faire correspondre des noms à des IPs
dns-server IN A 10.4.1.201
node1      IN A 10.4.1.11
```

```powershell
sudo cat /var/named/tp4.b1.rev

$TTL 86400
@ IN SOA dns-server.tp4.b1. admin.tp4.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns-server.tp4.b1.

;Reverse lookup for Name Server
201 IN PTR dns-server.tp4.b1.
11 IN PTR node1.tp4.b1.
```

```powershell
systemctl status named
● named.service - Berkeley Internet Name Domain (DNS)
     Loaded: loaded (/usr/lib/systemd/system/named.service; enabled; vendor preset: disabled)
     Active: active (running) since Thu 2022-11-10 11:32:48 CET; 9min ago
   Main PID: 42357 (named)
      Tasks: 8 (limit: 5904)
     Memory: 26.3M
        CPU: 43ms
     CGroup: /system.slice/named.service
             └─42357 /usr/sbin/named -u named -c /etc/named.conf

Nov 10 11:32:48 dns-server.tp4.b1 named[42357]: network unreachable resolving './DNSKEY/IN': 2001:dc3::35#53
Nov 10 11:32:48 dns-server.tp4.b1 named[42357]: network unreachable resolving './NS/IN': 2001:dc3::35#53
Nov 10 11:32:48 dns-server.tp4.b1 named[42357]: network unreachable resolving './DNSKEY/IN': 2001:500:9f::42#53
Nov 10 11:32:48 dns-server.tp4.b1 named[42357]: network unreachable resolving './NS/IN': 2001:500:9f::42#53
Nov 10 11:32:48 dns-server.tp4.b1 named[42357]: zone localhost/IN: loaded serial 0
Nov 10 11:32:48 dns-server.tp4.b1 named[42357]: all zones loaded
Nov 10 11:32:48 dns-server.tp4.b1 named[42357]: running
Nov 10 11:32:48 dns-server.tp4.b1 systemd[1]: Started Berkeley Internet Name Domain (DNS).
Nov 10 11:32:48 dns-server.tp4.b1 named[42357]: resolver priming query complete
Nov 10 11:32:48 dns-server.tp4.b1 named[42357]: managed-keys-zone: Initializing automatic trust anchor management >
lines 1-20/20 (END)
```

```powershell
sudo ss

tcp     LISTEN   0        10            10.4.1.201:53            0.0.0.0:*      users:(("named",pid=42357,fd=32))
tcp     LISTEN   0        10            10.4.1.201:53            0.0.0.0:*      users:(("named",pid=42357,fd=33))
tcp     LISTEN   0        10             127.0.0.1:53            0.0.0.0:*      users:(("named",pid=42357,fd=26))
tcp     LISTEN   0        10             127.0.0.1:53            0.0.0.0:*      users:(("named",pid=42357,fd=27))
tcp     LISTEN   0        4096           127.0.0.1:953           0.0.0.0:*      users:(("named",pid=42357,fd=23))
tcp     LISTEN   0        10                 [::1]:53               [::]:*      users:(("named",pid=42357,fd=36))
tcp     LISTEN   0        10                 [::1]:53               [::]:*      users:(("named",pid=42357,fd=37))
tcp     LISTEN   0        4096               [::1]:953              [::]:*      users:(("named",pid=42357,fd=38))
```

🌞 Ouvrez le bon port dans le firewall
```powershell
sudo firewall-cmd --add-port=53/tcp --permanent
[sudo] password for alexy:
success
```
```powershell
sudo firewall-cmd --list-all

[sudo] password for alexy:
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s8
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
  ```

## 3. Test

🌞 Sur la machine node1.tp4.b1

```powershell
sudo firewall-cmd --add-port=53/udp --permanent
success
[alexy@dns-server ~]$ sudo firewall-cmd --reload
success
```

```powershell
dig node1.tp4.b1

; <<>> DiG 9.16.23-RH <<>> node1.tp4.b1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 27649
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;node1.tp4.b1.                  IN      A

;; AUTHORITY SECTION:
.                       86400   IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2022111001 1800 900 604800 86400

;; Query time: 27 msec
;; SERVER: 1.1.1.1#53(1.1.1.1)
;; WHEN: Thu Nov 10 12:04:04 CET 2022
;; MSG SIZE  rcvd: 116

[alexy@node1 ~]$ dig dns-server.tp4.b1

; <<>> DiG 9.16.23-RH <<>> dns-server.tp4.b1
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 190
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;dns-server.tp4.b1.             IN      A

;; AUTHORITY SECTION:
.                       86400   IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2022111001 1800 900 604800 86400

;; Query time: 27 msec
;; SERVER: 1.1.1.1#53(1.1.1.1)
;; WHEN: Thu Nov 10 12:04:10 CET 2022
;; MSG SIZE  rcvd: 121

[alexy@node1 ~]$ dig www.google.com

; <<>> DiG 9.16.23-RH <<>> www.google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 43207
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
;; QUESTION SECTION:
;www.google.com.                        IN      A

;; ANSWER SECTION:
www.google.com.         259     IN      A       142.250.201.164

;; Query time: 23 msec
;; SERVER: 1.1.1.1#53(1.1.1.1)
;; WHEN: Thu Nov 10 12:04:20 CET 2022
;; MSG SIZE  rcvd: 59
```

🌞 Sur votre PC

```powershell
C:\WINDOWS\system32> nslookup node1.tp4.b1 10.4.1.201
Serveur :   dns-server.tp4.b1
Address:  10.4.1.201

Nom :    node1.tp4.b1
Address:  10.4.1.11
```
[Wireshark DNS](./DNS.pcapng)