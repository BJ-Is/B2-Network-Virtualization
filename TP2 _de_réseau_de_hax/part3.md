## B. Proofs¶

\*\* 🌞 Test test test : ajouter un nouveau VPCS au LAN1, le bro node6.tp2.efrei

- demander une adresse IP en DHCP avec lui
- constater que vous avez aussi récupéré l'adresse de l'attaquant en passerelle (et 1.1.1.1 en serveur DNS)
- constater que vous pouvez instantanément ping efrei.fr, malheureusement

```bash
node6> ip dhcp
DORA IP 10.2.1.201/24 GW 10.2.1.252

node6> ping efrei.fr
efrei.fr resolved to 51.210.229.203

84 bytes from 51.210.229.203 icmp_seq=1 ttl=53 time=40.609 ms
84 bytes from 51.210.229.203 icmp_seq=2 ttl=53 time=29.417 ms
84 bytes from 51.210.229.203 icmp_seq=3 ttl=53 time=38.238 ms
84 bytes from 51.210.229.203 icmp_seq=4 ttl=53 time=38.230 ms
84 bytes from 51.210.229.203 icmp_seq=5 ttl=53 time=42.701 ms

node6> sh ip

NAME        : node6[1]
IP/MASK     : 10.2.1.201/24
GATEWAY     : 10.2.1.252
DNS         : 1.1.1.1
DHCP SERVER : 10.2.1.252
DHCP LEASE  : 401, 600/300/525
MAC         : 00:50:79:66:68:05
LPORT       : 20027
RHOST:PORT  : 127.0.0.1:20028
MTU         : 1500

node6>

```
