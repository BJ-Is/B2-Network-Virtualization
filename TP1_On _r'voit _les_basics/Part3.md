## 3. Setuuuup

\*\* ➜ Donner un accès Internet à la machine dhcp.tp1.efrei

** pour ce faire, ajoutez une carte réseau NAT à la machine dans VirtualBox, à la main
** une fois la machine démarrée, prouvez en une commande que vous avez un accès internet

```bash
[israel@dhcp ~]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=255 time=20.2 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=255 time=16.6 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=255 time=17.3 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=255 time=26.2 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=255 time=19.9 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=255 time=17.0 ms
64 bytes from 8.8.8.8: icmp_seq=7 ttl=255 time=22.0 ms
^C
--- 8.8.8.8 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6014ms
rtt min/avg/max/mdev = 16.604/19.885/26.193/3.163 ms
```

## 🌞 Installer un serveur DHCP

- à réaliser sur dhcp.tp1.efrei

- je n'aime pas ré-écrire la roue, et préfère pour vous renvoyer vers des ressources dispos en ligne quand elles sont suffisantes

  - installez un serveur DHCP dnsmasq ou kea
    - pour des setups simplistes, les tutos/articles de server-world.info sont souvent straightforward
      - le serveur DHCP doit démarrer automatiquement quand la machine s'allume

- votre serveur DHCP doit attribuer des IP entre 10.1.1.10 et 10.1.1.50
  - toutes les commandes nécessaires dans le compte-rendu please

```bash
[israel@dhcp ~]$ sudo dnf -y install dhcp-server
Rocky Linux 9 - BaseOS                      2.8 kB/s | 4.3 kB     00:01
Rocky Linux 9 - AppStream                   7.2 kB/s | 4.8 kB     00:00
Rocky Linux 9 - Extras                      2.1 kB/s | 3.1 kB     00:01
Dependencies resolved.
============================================================================
 Package           Architecture Version                  Repository    Size
============================================================================
Installing:
 dhcp-server       x86_64       12:4.4.2-19.b1.el9       baseos       1.2 M
Installing dependencies:
 dhcp-common       noarch       12:4.4.2-19.b1.el9       baseos       128 k

Transaction Summary
============================================================================
Install  2 Packages

Total download size: 1.3 M
Installed size: 4.2 M
Downloading Packages:
(1/2): dhcp-common-4.4.2-19.b1.el9.noarch.r  80 kB/s | 128 kB     00:01
(2/2): dhcp-server-4.4.2-19.b1.el9.x86_64.r 496 kB/s | 1.2 MB     00:02
----------------------------------------------------------------------------
Total                                       499 kB/s | 1.3 MB     00:02
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                    1/1
  Installing       : dhcp-common-12:4.4.2-19.b1.el9.noarch              1/2
  Running scriptlet: dhcp-server-12:4.4.2-19.b1.el9.x86_64              2/2
  Installing       : dhcp-server-12:4.4.2-19.b1.el9.x86_64              2/2
  Running scriptlet: dhcp-server-12:4.4.2-19.b1.el9.x86_64              2/2
  Verifying        : dhcp-common-12:4.4.2-19.b1.el9.noarch              1/2
  Verifying        : dhcp-server-12:4.4.2-19.b1.el9.x86_64              2/2

Installed:
  dhcp-common-12:4.4.2-19.b1.el9.noarch
  dhcp-server-12:4.4.2-19.b1.el9.x86_64

Complete!
```

```bash
[israel@dhcp ~]$ sudo cat /etc/dhcp/dhcpd.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
# create new
# specify domain name
option domain-name     "srv.world";
# specify DNS server's hostname or IP address
option domain-name-servers     dlp.srv.world;
# default lease time
default-lease-time 600;
# max lease time
max-lease-time 7200;
# this DHCP server to be declared valid
authoritative;
# specify network address and subnetmask
subnet 10.1.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.1.1.10 10.1.1.50;
    # specify broadcast address
    option broadcast-address 10.1.1.255;
    # specify gateway
    option routers 10.1.1.1;
}
```

```bash
[israel@dhcp ~]$ sudo systemctl start dhcpd.service
[israel@dhcp ~]$ systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; preset>
     Active: active (running) since Thu 2026-01-08 11:26:10 CET; 2s ago
       Docs: man:dhcpd(8)
             man:dhcpd.conf(5)
   Main PID: 1364 (dhcpd)
     Status: "Dispatching packets..."
      Tasks: 1 (limit: 10640)
     Memory: 4.7M (peak: 4.9M)
        CPU: 17ms
     CGroup: /system.slice/dhcpd.service
             └─1364 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd>

Jan 08 11:26:10 dhcp.tp1 dhcpd[1364]:
Jan 08 11:26:10 dhcp.tp1 dhcpd[1364]: No subnet declaration for enp0s3 (10.>
Jan 08 11:26:10 dhcp.tp1 dhcpd[1364]: ** Ignoring requests on enp0s3.  If t>
Jan 08 11:26:10 dhcp.tp1 dhcpd[1364]:    you want, please write a subnet de>
Jan 08 11:26:10 dhcp.tp1 dhcpd[1364]:    in your dhcpd.conf file for the ne>
Jan 08 11:26:10 dhcp.tp1 dhcpd[1364]:    to which interface enp0s3 is attac>
Jan 08 11:26:10 dhcp.tp1 dhcpd[1364]:
Jan 08 11:26:10 dhcp.tp1 dhcpd[1364]: Sending on   Socket/fallback/fallback>
Jan 08 11:26:10 dhcp.tp1 dhcpd[1364]: Server starting service.
Jan 08 11:26:10 dhcp.tp1 systemd[1]: Started DHCPv4 Server Daemon.
lines 1-23/23 (END)
```

## 4. Proof or you're lying

\*\* 🌞 Récupérer une IP automatiquement depuis les 3 nodes

- là encore, montrez toutes les commandes réalisées
- et le contenu des fichiers que vous éditez, si vous en éditez
- prouver que votre changement d'IP est effectif, en une commande
  \*\* ➜ Wireshark !

- capture d'un échange DHCP
- l'échange est constitué de 4 messages, échangés entre le client et le serveur DHCP (DORA)
  📁 p3_dhcp.pcap

- contient l'échange DORA entre un client et votre serveur DHCP
- ne contient que 4 trames donc

```bash
node1.tp1> show ip

NAME        : node1.tp1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 10010
RHOST:PORT  : 127.0.0.1:10011
MTU:        : 1500

node1.tp1> dhcp
DORA IP 10.1.1.10/24 GW 10.1.1.1

node1.tp1> show ip

NAME        : node1.tp1[1]
IP/MASK     : 10.1.1.10/24
GATEWAY     : 10.1.1.1
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 452, 457/228/399
DOMAIN NAME : srv.world
MAC         : 00:50:79:66:68:02
LPORT       : 10010
RHOST:PORT  : 127.0.0.1:10011
MTU:        : 1500

```

## 5. DHCP lease

\*\* 🌞 Bail DHCP

- le serveur DHCP a créé un bail DHCP quand il a proposé une adresse au client
- genre il a enregistré dans un fichier toutes les infos concernant ce client
- bah trouvez et montrez moi ce fichier !

```bash
[israel@dhcp ~]$ cat /var/lib/dhcpd/dhcpd.leases
# The format of this file is documented in the dhcpd.leases(5) manual page.
# This lease file was written by isc-dhcp-4.4.2b1

# authoring-byte-order entry is generated, DO NOT DELETE
authoring-byte-order little-endian;

lease 10.1.1.10 {
  starts 4 2026/01/08 13:17:05;
  ends 4 2026/01/08 13:27:05;
  cltt 4 2026/01/08 13:17:05;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 00:50:79:66:68:02;
  uid "\001\000Pyfh\002";
  client-hostname "node1.tp11";
}
lease 10.1.1.11 {
  starts 4 2026/01/08 13:21:27;
  ends 4 2026/01/08 13:31:27;
  cltt 4 2026/01/08 13:21:27;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 00:50:79:66:68:01;
  uid "\001\000Pyfh\001";
  client-hostname "node2.tp11";
}
lease 10.1.1.12 {
  starts 4 2026/01/08 13:24:49;
  ends 4 2026/01/08 13:34:49;
  cltt 4 2026/01/08 13:24:49;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 00:50:79:66:68:00;
  uid "\001\000Pyfh\000";
  client-hostname "node3.tp11";
}
```

\*\* 🌞 Use grep

- avec un ptit | grep bien placé, mettez en évidence les infos concernant spécifiquement le client node1

```bash
[israel@dhcp ~]$ grep -B 10 "node1" /var/lib/dhcpd/dhcpd.leases
lease 10.1.1.10 {
  starts 4 2026/01/08 13:37:13;
  ends 4 2026/01/08 13:47:13;
  cltt 4 2026/01/08 13:37:13;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 00:50:79:66:68:02;
  uid "\001\000Pyfh\002";
  client-hostname "node1.tp11";
```
