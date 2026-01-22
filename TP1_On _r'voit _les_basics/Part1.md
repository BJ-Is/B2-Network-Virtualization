## 3. Know your MAC

\*\*\* 🌞 Déterminer l'adresse MAC de vos deux machines

- depuis le terminal de chacun des clients (VM Rocky ou VPCS), déterminer son adresse MAC

- depuis le terminal de chacun des clients (VM Rocky ou VPCS), déterminer son adresse MAC
  une seule commande est nécessaire sur chaque client

```bash
PC1> set pcname node1.tp1

node1.tp1> show ip

NAME        : node1.tp1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 10004
RHOST:PORT  : 127.0.0.1:10005
MTU:        : 1500

```

```bash
PC2> set pcname node2.tp1

node2.tp1> show ip

NAME        : node2.tp1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 10002
RHOST:PORT  : 127.0.0.1:10003
MTU:        : 1500

node2.tp1>

```

## 4.IP Setup

\*\*\* 🌞 Définir une IP statique sur les deux machines

- depuis le terminal des machines Linux
- indiquez dans le compte-rendu l'ensemble des commandes réalisées
- montrez le contenu des fichiers que vous éditez (si vous en éditez)
  \*\*\* 🌞 Proof !

- prouver que votre changement d'IP est effectif, en une commande

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
LPORT       : 10004
RHOST:PORT  : 127.0.0.1:10005
MTU:        : 1500

node1.tp1>

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
LPORT       : 10002
RHOST:PORT  : 127.0.0.1:10003
MTU:        : 1500

node2.tp1>

```

\*\*\* 🌞 Effectuer un ping d'une machine à l'autre

- c'est la commande ping, sans surprise vous me direz

```bash
node1.tp1> ping 10.1.1.2
84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=1.107 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=9.676 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=0.366 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=0.550 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=0.422 ms

```

```bash
node2.tp1> ping 10.1.1.1
84 bytes from 10.1.1.1 icmp_seq=1 ttl=64 time=1.201 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=64 time=0.831 ms
84 bytes from 10.1.1.1 icmp_seq=3 ttl=64 time=1.603 ms
84 bytes from 10.1.1.1 icmp_seq=4 ttl=64 time=1.017 ms
84 bytes from 10.1.1.1 icmp_seq=5 ttl=64 time=0.358 ms

```

## 5. Analyze

\*\* ➜ Wireshark !

- vous pouvez faire un clic droit sur un câble dans GNS3 pour lancer une capture des trames qui passent dans le câble
- visualiser le ping entre les deux machines
- enregistrez la capture avec Wireshark (format .pcap ou .pcapng), et vous la joindrez au compte-rendu dans le dépôt git

\*\*\* 🌞 Protocolz ?

- le protocole tcp

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
