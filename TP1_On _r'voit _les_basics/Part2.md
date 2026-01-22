## Intro

## 3. Même chose en fast

\*\* 🌞 Déterminer l'adresse MAC de vos trois machines

- une commande dans le terminal de chaque machine

```bash
PC1> set pcname node3.tp1

node3.tp1> sh ip

NAME        : node3.tp1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:03
LPORT       : 10002
RHOST:PORT  : 127.0.0.1:10003
MTU:        : 1500

node3.tp1>

```

```bash
VPCS> set pcname node1.tp1

node1.tp1> show ip

NAME        : node1.tp1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500

```

```bash
VPCS> set pcname node2.tp1

node2.tp1> show ip

NAME        : node2.tp1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10006
RHOST:PORT  : 127.0.0.1:10007
MTU:        : 1500

```

\*\*\* 🌞 Définir une IP statique sur les trois machines

```bash
node1.tp1> ip 10.1.1.1
Checking for duplicate address...
PC1 : 10.1.1.1 255.255.255.0

node1.tp1> show ip

NAME        : node1.tp1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10008
RHOST:PORT  : 127.0.0.1:10009
MTU:        : 1500

```

```bash
node2.tp1> ip 10.1.1.2
Checking for duplicate address...
PC1 : 10.1.1.2 255.255.255.0

node2.tp1> show ip

NAME        : node2.tp1[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10006
RHOST:PORT  : 127.0.0.1:10007
MTU:        : 1500

```

```bash
node3.tp1> ip 10.1.1.3
Checking for duplicate address...
PC1 : 10.1.1.3 255.255.255.0

node3.tp1> sh ip

NAME        : node3.tp1[1]
IP/MASK     : 10.1.1.3/24
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:03
LPORT       : 10002
RHOST:PORT  : 127.0.0.1:10003
MTU:        : 1500

node3.tp1>

```

\*\*\* 🌞 Effectuer des ping d'une machine à l'autre

- vérifiez que tout le monde peut se joindre
- node1 à node2

```bash
node1.tp1> ping 10.1.1.2
84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=1.210 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=2.987 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=2.925 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=2.225 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=2.849 ms

```

- node2 à node3

```bash
node2.tp1> ping 10.1.1.3
84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.598 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=2.141 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=1.008 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=1.087 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=2.044 ms

```

- node1 à node3

```bash
node1.tp1> ping 10.1.1.3
84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=1.435 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=1.998 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=2.002 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=2.362 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=2.096 ms
```

\*\*🌞 Afficher la table ARP de node1

- on doit y voir les adresses MAC de node2 et node3, associées à leurs adresses IP respectives

```bash
node1.tp1> arp

00:50:79:66:68:01  10.1.1.2 expires in 88 seconds
00:50:79:66:68:03  10.1.1.3 expires in 98 seconds
```

```bash

```

```bash

```

```bash

```

```bash

```

```bash

```
