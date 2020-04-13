## 🌞 Prouvez que chacun des points de la préparation de l'environnement ci-dessus ont été respectés :

## Carte NAT désactivée : commande ip a :

### client :

2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
link/ether 08:00:27:7d:8d:2f brd ff:ff:ff:ff:ff:ff

### routeur :

2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
link/ether 08:00:27:7d:8d:2f brd ff:ff:ff:ff:ff:ff

### server :

2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
link/ether 08:00:27:7d:8d:2f brd ff:ff:ff:ff:ff:ff

## server SSH fonctionnel qui écoute sur le port 7777/tcp : commande ss -ltunp :

### client :

tcp LISTEN 0 128 _:7777 _:\* users:(("sshd",pid=1093,fd=3))

### routeur :

tcp LISTEN 0 128 _:7777 _:\* users:(("sshd",pid=1134,fd=3))

### server :

tcp LISTEN 0 128 _:7777 _:\* users:(("sshd",pid=1093,fd=3))

## Pare-feu activé et configuré : commande firewall-cmd --list-all

### client :

target: default
icmp-block-inversion: no
interfaces: enp0s8
sources:
services: dhcpv6-client ssh
ports: 80/tcp 1025/tcp 6666/tcp 5555/tcp 22/tcp 7777/tcp
protocols:
masquerade: no
forward-ports:
source-ports:
icmp-blocks:
rich rules:

### routeur :

target: default
icmp-block-inversion: no
interfaces: enp0s8 enp0s9
sources:
services: dhcpv6-client ssh
ports: 80/tcp 1025/tcp 6666/tcp 5555/tcp 22/tcp 7777/tcp
protocols:
masquerade: no
forward-ports:
source-ports:
icmp-blocks:
rich rules:

### server :

target: default
icmp-block-inversion: no
interfaces: enp0s8
sources:
services: dhcpv6-client ssh
ports: 80/tcp 1025/tcp 6666/tcp 5555/tcp 22/tcp 7777/tcp
protocols:
masquerade: no
forward-ports:
source-ports:
icmp-blocks:
rich rules:

## Nom configuré : commande hostname :

### client :

[root@client1 ~]# hostname
client1.net1.tp3

### routeur :

[root@router ~]# hostname
router.tp3

### server :

[root@server1 ~]# hostname
server1.net2.tp3

## Fichiers /etc/hosts de toutes les machines configurés : cat /etc/hosts

### client :

[root@client1 ~]# cat /etc/hosts
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
10.3.2.11 server1.net2.tp3

### routeur :

[root@router ~]# cat /etc/hosts
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
10.3.1.11 cient1.net1.tp3
10.3.2.11 server1.net2.tp3

### server :

[root@server1 ~]# cat /etc/hosts
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1 localhost localhost.localdomain localhost6 localhost6.localdomain6
10.3.1.11 cient1.net1.tp3

## Réseaux et adressage des machines : commandes ip a et ping :

### client1 <> router

    depuis client1, ping router doit marcher :

    [root@client1 ~]# ping 10.3.1.254

PING 10.3.1.254 (10.3.1.254) 56(84) bytes of data.
64 bytes from 10.3.1.254: icmp_seq=1 ttl=64 time=1.42 ms
64 bytes from 10.3.1.254: icmp_seq=2 ttl=64 time=0.843 ms
64 bytes from 10.3.1.254: icmp_seq=3 ttl=64 time=0.966 ms
64 bytes from 10.3.1.254: icmp_seq=4 ttl=64 time=0.847 ms
64 bytes from 10.3.1.254: icmp_seq=5 ttl=64 time=0.899 ms
64 bytes from 10.3.1.254: icmp_seq=6 ttl=64 time=0.886 ms
64 bytes from 10.3.1.254: icmp_seq=7 ttl=64 time=0.889 ms
^C
--- 10.3.1.254 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6027ms
rtt min/avg/max/mdev = 0.843/0.965/1.425/0.191 ms

    depuis router, ping client1 doit marcher :*

    [root@router ~]# ping 10.3.1.11

PING 10.3.1.11 (10.3.1.11) 56(84) bytes of data.
64 bytes from 10.3.1.11: icmp_seq=1 ttl=64 time=0.720 ms
64 bytes from 10.3.1.11: icmp_seq=2 ttl=64 time=0.592 ms
64 bytes from 10.3.1.11: icmp_seq=3 ttl=64 time=0.851 ms
^C
--- 10.3.1.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 0.592/0.721/0.851/0.105 ms

### server1 <> router

    depuis server1, ping router doit marcher :

    [root@server1 ~]# ping 10.3.2.254

PING 10.3.2.254 (10.3.2.254) 56(84) bytes of data.
64 bytes from 10.3.2.254: icmp_seq=1 ttl=64 time=0.498 ms
64 bytes from 10.3.2.254: icmp_seq=2 ttl=64 time=0.367 ms
64 bytes from 10.3.2.254: icmp_seq=3 ttl=64 time=0.402 ms
^C
--- 10.3.2.254 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 0.367/0.422/0.498/0.057 ms

    depuis router, ping server1 doit marcher :

    [root@router ~]# ping 10.3.2.11

PING 10.3.2.11 (10.3.2.11) 56(84) bytes of data.
64 bytes from 10.3.2.11: icmp_seq=1 ttl=64 time=0.742 ms
64 bytes from 10.3.2.11: icmp_seq=2 ttl=64 time=0.336 ms
64 bytes from 10.3.2.11: icmp_seq=3 ttl=64 time=0.291 ms
^C
--- 10.3.2.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2011ms
rtt min/avg/max/mdev = 0.291/0.456/0.742/0.203 ms

# I. Mise en place du routage

## 1. Configuration du routage sur router

## 🌞 Effectuez cette commande sur la machine router.

[root@router ~]# sysctl -w net.ipv4.conf.all.forwarding=1
net.ipv4.conf.all.forwarding = 1

## 2. Ajouter les routes statique

## 🌞 Ajouter les routes nécessaires pour que...

### client1 puisse joindre net2 :

Dans client1 faire un dossier "route-enp0s8" dans lequel on rentrera l'adresse ip du net2 en passant par le net1.

nano route-enp0s8 : 10.3.2.0/24 via 10.3.1.254 dev eth0

### vérifier l'ajout de la route avec ip r s :

[root@client1 network-scripts]# ip route show
10.3.1.0/24 dev enp0s8 proto kernel scope link src 10.3.1.11 metric 101
10.3.2.0/24 via 10.3.1.254 dev enp0s8 proto static metric 101

On voit que le client1 peut atteindre net2 via le routeur.

### server1 puisse joindre net1 :

Dans server1 faire un dossier "route-enp0s8" dans lequel on rentrera l'adresse ip du net1 en passant par le net2.

nano route-enp0s8 : 10.3.1.0/24 via 10.3.2.254 dev eth0

### vérifier l'ajout de la route avec ip r s :

[root@server1 network-scripts]# ip route show
10.3.1.0/24 via 10.3.2.254 dev enp0s8 proto static metric 101
10.3.2.0/24 dev enp0s8 proto kernel scope link src 10.3.2.11 metric 101

On voit que le server1 peut atteindre net1 via le routeur.

### tester le bon fonctionnement :

Je ping server1 depuis client1 :

[root@client1 network-scripts]# ping 10.3.2.11
PING 10.3.2.11 (10.3.2.11) 56(84) bytes of data.
64 bytes from 10.3.2.11: icmp_seq=1 ttl=63 time=4.65 ms
64 bytes from 10.3.2.11: icmp_seq=2 ttl=63 time=1.69 ms
64 bytes from 10.3.2.11: icmp_seq=3 ttl=63 time=1.78 ms
^C
--- 10.3.2.11 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2005ms
rtt min/avg/max/mdev = 1.694/2.711/4.655/1.375 ms

La communication marche entre client1 et server1 la route est donc fonctionelle.

De plus si je change la route, le ping ne marche plus.

[root@client1 network-scripts]# ip route show
10.3.1.0/24 dev enp0s8 proto kernel scope link src 10.3.1.11 metric 101
10.3.8.0/24 via 10.3.1.254 dev enp0s8 proto static metric 101
[root@client1 network-scripts]# ping 10.3.2.11
connect: Network is unreachable

Donc la connexion s'effectue donc bien avec la route que je lui donnais.

### vérification du passage par router avec la commande traceroute :

Depuis le client faire :

[root@client1 ~]# traceroute 10.3.2.11
traceroute to 10.3.2.11 (10.3.2.11), 30 hops max, 60 byte packets
1 10.3.1.254 (10.3.1.254) 0.232 ms 0.153 ms 0.111 ms
2 10.3.1.254 (10.3.1.254) 0.074 ms !X 0.077 ms !X 0.081 ms !X

|                                             | MAC src           | MAC dst           | IP src    | IP dst    |
| ------------------------------------------- | ----------------- | ----------------- | --------- | --------- |
| Dans `net1` (trame qui entre dans `router`) | 08:00:27:04:e8:cb | 08:00:27:f1:5a:0e | 10.3.1.11 | 10.3.2.11 |
| Dans `net2` (trame qui sort de `router`)    | 08:00:27:b3:98:b3 | 08:00:27:a6:39:03 | 10.3.1.11 | 10.3.2.11 |

# II. ARP

## 1. Tables ARP

## 🌞 Affichez la table ARP de chacun des machines et expliquez toutes les lignes

### J'utilise la commande ip neigh show :

### Sur le client :

[root@client1 ~]# ip neigh show
10.3.1.254 dev enp0s8 lladdr 08:00:27:f1:5a:0e STALE : Ip du routeur dans le net1 (même réseau que le client)
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0b DELAY : Ip de mon hôte connecté au client dans le net1

### Sur le routeur :

[root@router ~]# ip neigh show
10.3.1.11 dev enp0s8 lladdr 08:00:27:04:e8:cb STALE : Ip du client
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0b STALE : Ip de mon hôte connecté au routeur dans le net1
10.3.2.1 dev enp0s9 lladdr 0a:00:27:00:00:08 DELAY : Ip de mon hôte connecté au routeur dans le net2
10.3.2.11 dev enp0s9 lladdr 08:00:27:a6:39:03 STALE : Ip du serveur

### Sur le server :

[root@server1 ~]# ip neigh show
10.3.2.254 dev enp0s8 lladdr 08:00:27:b3:98:b3 STALE : Ip du routeur dans le net2 (même réseau que le server)
10.3.2.1 dev enp0s8 lladdr 0a:00:27:00:00:08 DELAY : Ip de mon hôte connecté au server dans le net2

## 2. Requêtes ARP

## A. Table ARP 1

videz la table ARP de client1 ET router :

    sudo ip neigh flush all

## 🌞 mettez en évidence le changement dans la table ARP de client1 :

    Après avoir vidé la table ARP de client1, voici à quoi elle ressemble :

[root@client1 ~]# ip neigh show
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0b REACHABLE

On peut voir qu'il ne contient plus l'Ip du routeur.

Mais si on ping server1 et que l'on refait "ip neigh show" voici ce que la table contient :

[root@client1 ~]# ip neigh show
10.3.1.254 dev enp0s8 lladdr 08:00:27:f1:5a:0e REACHABLE
10.3.1.1 dev enp0s8 lladdr 0a:00:27:00:00:0b REACHABLE

On peut voir qu'il à retrouvé l'ip du routeur dans net1 car client1 en avait besoin pour se connecter à net2 où se trouve server1.

## B. Table ARP 2

videz la table ARP de client1 ET server1 ET router :

## 🌞 mettez en évidence le changement dans la table ARP de server1

    Après avoir vidé la table ARP de server1, voici à quoi elle ressemble :

[root@server1 ~]# ip neigh show
10.3.2.1 dev enp0s8 lladdr 0a:00:27:00:00:08 REACHABLE

On peut voir qu'il ne contient plus l'Ip du routeur.

Mais si on ping server1 et que l'on refait "ip neigh show" voici ce que la table contient :

[root@server1 ~]# ip neigh show
10.3.2.254 dev enp0s8 lladdr 08:00:27:b3:98:b3 REACHABLE
10.3.2.1 dev enp0s8 lladdr 0a:00:27:00:00:08 DELAY

On peut voir qu'il à retrouvé l'ip du routeur dans net2 car server1 en avait besoin pour recevoir ce qu'a envoyer client1. (Je pense que le routeur à plutot demandé qui était 10.3.2.11 et que le server à répondu.)

### C. tcpdump 1

videz la table ARP de client1 ET router : ip neigh flush all
sur client1, lancez tcpdump pour capturer toutes les trames que client1 envoie : tcpdump -i enp0s8 -w capture10.3.1.11.pcap
sur client1, envoyez un paquet à server1 (ping par exemple) : ping 10.3.2.11
sur client1, coupez tcpdump : ctrl + d

## 🌞 mettez en évidence toutes les trames ARP capturées lors de cet échange, et expliquer chacune d'entre elles

5 3.326755 PcsCompu_04:e8:cb Broadcast ARP 42 Who has 10.3.1.254? Tell 10.3.1.11 : Cette trame nous dit que client1 (10.3.1.11) demande à tout le monde (broadcast) qui possède l'Ip 10.3.1.254.

8 3.327055 PcsCompu_f1:5a:0e PcsCompu_04:e8:cb ARP 60 10.3.1.254 is at 08:00:27:f1:5a:0e : Cette trame nous dit que le routeur de net1 (10.3.1.254) envois à client1 son adresse MAC.

### D. tcpdump 2

videz la table ARP de client1 ET server1 ET router
sur server1, lancez tcpdump pour capturer toutes les trames que client1 envoie
sur client1, envoyez un paquet à server1 (ping par exemple)
sur server1, coupez tcpdump

## 🌞 mettez en évidence toutes les trames ARP capturées lors de cet échange, et expliquer chacune d'entre elles

3 2.490267 PcsCompu_b3:98:b3 Broadcast ARP 60 Who has 10.3.2.11? Tell 10.3.2.254 : Cette trame nous dit que le routeur dans net2 (10.3.2.11) demande à tout le monde (broadcast) qui possède l'Ip 10.3.2.11.

4 2.490285 PcsCompu_a6:39:03 PcsCompu_b3:98:b3 ARP 42 10.3.2.11 is at 08:00:27:a6:39:03 : Cette trame nous dit que server1 (10.3.2.11) envois au routeur du net2 (10.3.2.254) son adresse MAC.

E. u okay bro ?

## 🌞 Expliquer, en une suite d'étapes claires, toutes les trames ARP échangées lorsque client1 envoie un ping vers server1, en traversant la machine router.

Quand on demande de ping 10.3.2.11, le client1 comprend qu'il n'est pas dans son réseau; il va donc regarder dans ses routes et il voit qu'il peut rejoindre 10.3.2.0 en demandant à 10.3.1.254, le client demande donc à tout le monde dans son réseau "Hé c'est qui 10.3.1.254 ? Répondez à l'adresse MAC ....." et le routeur lui répond "Hé c'est moi 10.3.1.254 mon adresse MAC c'est ....." à ce moment client1 et le routeur de net1 ajoutent dans leur table ARP comment se joindre.

Une fois cela fait le paquet pars de client1 pour aller dans le routeur de net1 pour arriver dans le net2, mais la le routeur ne connais pas qui possède l'adresse IP du ping donc il demande à tout le monde dans le net2 "Hé c'est qui 10.3.2.11 ? Répondez à l'adresse MAC ....." et server1 répond "Hé c'est moi qui à cette adresse IP mon adresse MAC c'est ....." à ce moment le routeur et server1 ajoutent dans leur table ARP comment se joindre.

## 🌞Permettre un accès WAN (Internet) à client1

configurez router

### ré-activez la carte NAT du router uniquement : Sur router faire ifup ifcfg-enp0s3

firewall-cmd --add-masquerade --permanent
firewall-cmd --reload

### configurez client1

ajout d'une route par défaut ; rajouter dans client1 /etc/sysconfig/network-scripts/routes-enp0s8 la ligne GATEWAY=10.3.1.254

vérifier en affichant la table de routage

configuration d'un serveur DNS

vérifiez que vous avez un accès au WAN (internet) avec les commandes :

envoi d'un message simple vers un serveur en ligne

\$ ping 8.8.8.8 : OK

test de la résolution de nom DNS

\$ dig <NOM_DUN_SITE_INTERNET> : OK

### test d'installation de paquet

$ sudo yum install -y epel-release : OK
$ sudo yum install -y sl : OK

### lancement de la locomotiiiiiive

\$ sl : OK

# III. Plus de tcpdump

## 1. TCP et UDP

### A. Warm-up

Effectuez une connexion nc où le client est client1 et le serveur server1.

testez avec le port 9999/tcp

testez avec le port 9999/udp

n'oubliez pas d'ouvrir les ports dans le firewall afin de les utiliser :) : firewall-cmd --add-port=9999/tcp --permanent

Sur server1 faire "nc -l 9999" puis sur client1 faire "nc 10.3.2.11 9999" et la connexion s'établie.

### B. Analyse de trames

## 🌞 TCP :

lancez un tcpdump sur le router
effectuez un nc de client vers le serveur, en TCP

échangez quelques messages
coupez la connexion (CTRL + C)

### mettez en évidence le 3-way handshake TCP

7 4.478559 10.3.1.11 10.3.2.11 TCP 74 36020 → 9999 [SYN] Seq=0 Win=29200 Len=0 MSS=1460 SACK_PERM=1 TSval=114196858 TSecr=0 WS=128 : Cette trame nous dit que client1 demande à server1 de se connecter ensemble.

8 4.478790 10.3.2.11 10.3.1.11 TCP 74 9999 → 36020 [SYN, ACK] Seq=0 Ack=1 Win=28960 Len=0 MSS=1460 SACK_PERM=1 TSval=114334268 TSecr=114196858 WS=128 : Cette trame nous dis que server1 à bien reçu la demande à client1 d'ouvrir un de ses ports.

9 4.478900 10.3.1.11 10.3.2.11 TCP 66 36020 → 9999 [ACK] Seq=1 Ack=1 Win=29312 Len=0 TSval=114196858 TSecr=114334268 : Cette trame nous dit que client1 à bien reçu la demande de server1 et qu'il va ouvrir un de ses ports.

observez la suite des échanges (PSH, ACK, etc) :

12 10.429690 10.3.1.11 10.3.2.11 TCP 74 36020 → 9999 [PSH, ACK] Seq=1 Ack=1 Win=29312 Len=8 TSval=114202809 TSecr=114334268 : Cette trame part de client1 et va à server1, le paquet contient le message que j'ai tapé(d'où le PSH).

13 10.430279 10.3.2.11 10.3.1.11 TCP 66 9999 → 36020 [ACK] Seq=1 Ack=9 Win=29056 Len=0 TSval=114340219 TSecr=114202809 Cette trame part de server1 et va à client1 pour lui confirmer qu'il à bien reçu son paquet(d'où le ACK).

14 13.017343 10.3.2.11 10.3.1.11 TCP 69 9999 → 36020 [PSH, ACK] Seq=1 Ack=9 Win=29056 Len=3 TSval=114342806 TSecr=114202809 : Cette trame part de server1 et va à client1 et contient un message(d'où le PSH).

15 13.017595 10.3.1.11 10.3.2.11 TCP 66 36020 → 9999 [ACK] Seq=9 Ack=4 Win=29312 Len=0 TSval=114205397 TSecr=114342806 : Cette tram part du client1 et va à server1 pour lui confirmer qu'il a bien reçu son paquet.

16 13.325197 10.3.2.11 10.3.1.11 TCP 66 9999 → 36020 [FIN, ACK] Seq=4 Ack=9 Win=29056 Len=0 TSval=114343114 TSecr=114205397 : Cette trame part de server1 et va à client1 pour lui dire qu'il se déconnecte(d'où le FIN).

17 13.366176 10.3.1.11 10.3.2.11 TCP 66 36020 → 9999 [ACK] Seq=9 Ack=5 Win=29312 Len=0 TSval=114205746 TSecr=114343114 : Cette tram part du client1 et va à server1 pour lui confirmer qu'il a bien reçu son paquet(sa déconnexion).

18 14.223456 10.3.1.11 10.3.2.11 TCP 66 36020 → 9999 [FIN, ACK] Seq=9 Ack=5 Win=29312 Len=0 TSval=114206603 TSecr=114343114 : Cette tram part du client1 et va à server1 pour lui dire qu'il se déconnecte(d'où le FIN).

19 14.223779 10.3.2.11 10.3.1.11 TCP 66 9999 → 36020 [ACK] Seq=5 Ack=10 Win=29056 Len=0 TSval=114344013 TSecr=114206603 : Cette trame part de server1 et va à client1 pour lui confirmer qu'il a bien reçu son paquet(sa déconnexion).

## 🌞 UDP :

lancez un tcpdump sur le router
effectuez un nc de client vers le serveur, en UDP

échangez quelques messages
coupez la connexion (CTRL + C)

### mettez en évidence les différences entre TCP et UDP :

3 6.633529 10.3.1.11 10.3.2.11 UDP 60 36104 → 9999 Len=3 : Cette trame nous indique que client1 envoie un paquet a server1.

4 8.982084 10.3.2.11 10.3.1.11 UDP 50 9999 → 36104 Len=8 : Cette trame nous indique que server1 envoie un paquet a client1.

différences : Pas de 3-way handshake, + pas d'accusé de réception (message comme quoi on dir à l'envoyeur que le receveur à bien reçu le paquet).

Le TCP prend donc beaucoup de données pour être sûr que tout les paquets sont bien arrivés alors que l'UDP rush sans savoir si cela arrive à destination.

## 2. SSH

## 🌞 Effectuez une connexion SSH depuis client1 vers server1

utilisez tcpdump pour mettre en évidence des paquets échangés par SSH
trouvez quel protocole utilise SSH : TCP ou UDP ?

SSH utilise le protoole TCP, car on voit un 3-way handshake, ainsi que des accusés de réception(et puis c'est marqué sur wireshark 🤠).

### IV. Bonus

## 1. ARP cache poisoning

Mettez en place de l'ARP cache poisoning ou ARP spoofing :

## 🌞 Par exemple, créez une deuxième machine client client2. Depuis client2, faites croire à client1 que vous êtes sa passerelle :

client2 est dans le même réseau que client1, on donne à client2 l'ip 10.3.1.12.

On fais aussi la commande "sudo sysctl net.ipv4.ip_nonlocal_bind=1", pour permettre à notre machine de lier des sockets à des adresses IP qu'il ne possède pas.

Puis on fais la commande : arping -c 1 -U -s <IP que l on veut> -I enp0s8 <IP victime>

Donc dans mon cas :

arping -c 1 -U -s 10.3.1.13 -I enp0s8 10.3.1.11

ainsi client2 va envoyer à client1 qu'il possède l'adresse IP 10.3.1.13 alors que c'est faux, mais client1 va quand même l'enregistrer dans sa table ARP sans savoir que c'est faux.

Mais ceci est éphémère car un test est effectué au bout de 1 minute si aucune connexion n'a été faite.
