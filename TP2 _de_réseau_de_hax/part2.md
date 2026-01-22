\*\* 🌞 Prouver que...

- r1 peut ping internet (une adresse IP publique)

- malgré cela, aucun client n'a internet pour le moment (prouvez le avec un ping qui ne fonctionne pas depuis l'un des clients)

```bash
R1#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 20/26/32 ms

```

```bash

node1> ping 8.8.8.8

host (255.255.255.0) not reachable

```

\*\* 🌞 Proooooooooof or lie

- vérifier que toutes les machines peuvent désormais ping internet

```bash
node1> ping 1.1.1.1

84 bytes from 1.1.1.1 icmp_seq=1 ttl=58 time=29.749 ms
84 bytes from 1.1.1.1 icmp_seq=2 ttl=58 time=28.224 ms
84 bytes from 1.1.1.1 icmp_seq=3 ttl=58 time=23.965 ms
84 bytes from 1.1.1.1 icmp_seq=4 ttl=58 time=23.638 ms
84 bytes from 1.1.1.1 icmp_seq=5 ttl=58 time=39.712 ms
```

\*\* 🌞 Prove it

- depuis node1, effectuez un ping vers efrei.fr

```bash
node1> ip dns 1.1.1.1

node1> sh ip

NAME        : node1[1]
IP/MASK     : 10.2.1.11/24
GATEWAY     : 10.2.1.254
DNS         : 1.1.1.1
MAC         : 00:50:79:66:68:00
LPORT       : 20011
RHOST:PORT  : 127.0.0.1:20012
MTU         : 1500

node1> ping efrei.fr
efrei.fr resolved to 51.210.229.203

84 bytes from 51.210.229.203 icmp_seq=1 ttl=53 time=43.492 ms
84 bytes from 51.210.229.203 icmp_seq=2 ttl=53 time=34.488 ms
84 bytes from 51.210.229.203 icmp_seq=3 ttl=53 time=38.095 ms
84 bytes from 51.210.229.203 icmp_seq=4 ttl=53 time=34.462 ms
84 bytes from 51.210.229.203 icmp_seq=5 ttl=53 time=44.711 ms

```

\*\* 🌞 Test test test : ajouter un nouveau VPCS au LAN1, le bro node5.tp2.efrei

- demander une adresse IP en DHCP avec lui

- constater que vous avez aussi récupéré l'adresse de la passerelle et l'adresse du serveur DNS
- constater que vous pouvez instantanément ping efrei.fr

```bash

node5> dhcp
DORA IP 10.2.1.104/24 GW 10.2.1.254

node5> sh ip

NAME        : node5[1]
IP/MASK     : 10.2.1.104/24
GATEWAY     : 10.2.1.254
DNS         : 1.1.1.1
DHCP SERVER : 10.2.1.253
DHCP LEASE  : 573, 579/289/506
DOMAIN NAME : srv.world
MAC         : 00:50:79:66:68:04
LPORT       : 20022
RHOST:PORT  : 127.0.0.1:20023
MTU         : 1500

node5> ping efrei.fr
efrei.fr resolved to 51.210.229.203

84 bytes from 51.210.229.203 icmp_seq=1 ttl=50 time=28.658 ms
84 bytes from 51.210.229.203 icmp_seq=2 ttl=50 time=22.301 ms
84 bytes from 51.210.229.203 icmp_seq=3 ttl=50 time=40.435 ms
84 bytes from 51.210.229.203 icmp_seq=4 ttl=50 time=33.201 ms
84 bytes from 51.210.229.203 icmp_seq=5 ttl=50 time=36.839 ms

node5>

```

