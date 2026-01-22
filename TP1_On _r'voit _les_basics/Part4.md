## DHCP spoofing

\*\* 🌞 Installez et configurez un serveur DHCP sur votre machine attaquante

- il doit uniquement faire serveur DHCP (car il peut aussi faire serveur DNS)
- il doit attribuer des adresses IP entre 10.1.1.210 et 10.1.1.250

```bash
israel@attack:~$ cat /etc/dhcp/dhcpd.conf

# dhcpd.conf
#
# Sample configuration file for ISC dhcpd
#

# option definitions common to all supported networks...
# option domain-name "example.org";
# option domain-name-servers ns1.example.org, ns2.example.org;

# default-lease-time 600;
# max-lease-time 7200;

# The ddns-updates-style parameter controls whether or not the server will
# attempt to do a DNS update when a lease is confirmed. We default to the
# behavior of the version 2 packages ('none', since DHCP v2 didn't
# have support for DDNS.)
ddns-update-style none;

# If this DHCP server is the official DHCP server for the local
# network, the authoritative directive should be uncommented.
#authoritative;

# Use this to send dhcp log messages to a different log file (you also
# have to hack syslog.conf to complete the redirection).
#log-facility local7;

# No service will be given on this subnet, but declaring it helps the
# DHCP server to understand the network topology.

#subnet 10.152.187.0 netmask 255.255.255.0 {
#}

# This is a very basic subnet declaration.

#subnet 10.254.239.0 netmask 255.255.255.224 {
#  range 10.254.239.10 10.254.239.20;
#  option routers rtr-239-0-1.example.org, rtr-239-0-2.example.org;
#}

# This declaration allows BOOTP clients to get dynamic addresses,
# which we don't really recommend.

#subnet 10.254.239.32 netmask 255.255.255.224 {
#  range dynamic-bootp 10.254.239.40 10.254.239.60;
#  option broadcast-address 10.254.239.31;
#  option routers rtr-239-32-1.example.org;
#}

# A slightly different configuration for an internal subnet.
#subnet 10.5.5.0 netmask 255.255.255.224 {
#  range 10.5.5.26 10.5.5.30;
#  option domain-name-servers ns1.internal.example.org;
#  option domain-name "internal.example.org";
#  option routers 10.5.5.1;
#  option broadcast-address 10.5.5.31;
#  default-lease-time 600;
#  max-lease-time 7200;
#}

# Hosts which require special configuration options can be listed in
# host statements.   If no address is specified, the address will be
# allocated dynamically (if possible), but the host-specific information
# will still come from the host declaration.

#host passacaglia {
#  hardware ethernet 0:0:c0:5d:bd:95;
#  filename "vmunix.passacaglia";
#  server-name "toccata.example.com";
#}

# Fixed IP addresses can also be specified for hosts.   These addresses
# should not also be listed as being available for dynamic assignment.
# Hosts for which fixed IP addresses have been specified can boot using
# BOOTP or DHCP.   Hosts for which no fixed address is specified can only
# be booted with DHCP, unless there is an address range on the subnet
# to which a BOOTP client is connected which has the dynamic-bootp flag
# set.
#host fantasia {
#  hardware ethernet 08:00:07:26:c0:a5;
#  fixed-address fantasia.example.com;
#}

# You can declare a class of clients and then do address allocation
# based on that.   The example below shows a case where all clients
# in a certain class get addresses on the 10.17.224/24 subnet, and all
# other clients get addresses on the 10.0.29/24 subnet.

#class "foo" {
#  match if substring (option vendor-class-identifier, 0, 4) = "SUNW";
#}

#shared-network 224-29 {
#  subnet 10.17.224.0 netmask 255.255.255.0 {
#    option routers rtr-224.example.org;
#  }
#  subnet 10.0.29.0 netmask 255.255.255.0 {
#    option routers rtr-29.example.org;
#  }
#  pool {
#    allow members of "foo";
#    range 10.17.224.10 10.17.224.250;
#  }
#  pool {
#    deny members of "foo";
#    range 10.0.29.10 10.0.29.230;
#  }
#}

# Subnet declaration
subnet 10.1.1.0 netmask 255.255.255.0 {

  # IP address range
  range 10.1.1.210 10.1.1.250;

  # Lease time configuration (in seconds)
  default-lease-time 600;
  max-lease-time 7200;
}
```

\*\* 🌞 Test !

- d'abord, stoppez le service DHCP "légitime" sur la machine dhcp.tp1.efrei

```bash
[israel@dhcp ~]$ systemctl status dhcpd
○ dhcpd.service - DHCPv4 Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; preset: disabled)
     Active: inactive (dead) since Mon 2026-01-12 20:24:21 CET; 1s ago
   Duration: 1min 21.562s
       Docs: man:dhcpd(8)
             man:dhcpd.conf(5)
    Process: 788 ExecStart=/usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid $DHCPDARGS (code=killed, signal=>
   Main PID: 788 (code=killed, signal=TERM)
     Status: "Dispatching packets..."
        CPU: 9ms

Jan 12 20:23:00 dhcp.tp1 dhcpd[788]:    you want, please write a subnet declaration
Jan 12 20:23:00 dhcp.tp1 dhcpd[788]:    in your dhcpd.conf file for the network segment
Jan 12 20:23:00 dhcp.tp1 dhcpd[788]:    to which interface enp0s3 is attached. **
Jan 12 20:23:00 dhcp.tp1 dhcpd[788]:
Jan 12 20:23:00 dhcp.tp1 dhcpd[788]: Sending on   Socket/fallback/fallback-net
Jan 12 20:23:00 dhcp.tp1 dhcpd[788]: Server starting service.
Jan 12 20:23:00 dhcp.tp1 systemd[1]: Started DHCPv4 Server Daemon.
Jan 12 20:24:21 dhcp.tp1 systemd[1]: Stopping DHCPv4 Server Daemon...
Jan 12 20:24:21 dhcp.tp1 systemd[1]: dhcpd.service: Deactivated successfully.
Jan 12 20:24:21 dhcp.tp1 systemd[1]: Stopped DHCPv4 Server Daemon.
lines 1-21/21 (END)
```

- vérifier qu'un client (VPCS) récupère bien une adresse IP dans la range configurée avec dnsmasq de la
  machine attaquante

```bash

node1.tp1> show ip

NAME        : node1.tp1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:20001
MTU         : 1500

node1.tp1> dhcp
DORA
node1.tp1> show ip IP 10.1.1.210/24

NAME        : node1.tp1[1]
IP/MASK     : 10.1.1.210/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.252
DHCP LEASE  : 533, 536/268/469
MAC         : 00:50:79:66:68:00
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:20001
MTU         : 1500

```

- on vérifie d'abord chill que la machine attaquante fait correctement tourner un serveur DHCP

```bash
israel@attack:~$ systemctl status isc-dhcp-server.service
● isc-dhcp-server.service - LSB: DHCP server
     Loaded: loaded (/etc/init.d/isc-dhcp-server; generated)
     Active: active (running) since Mon 2026-01-12 14:15:17 EST; 14min ago
 Invocation: c12f118eec1f4175997eae2ea566acf8
       Docs: man:systemd-sysv-generator(8)
    Process: 1171 ExecStart=/etc/init.d/isc-dhcp-server start (code=exited, status=0/SUCCESS)
      Tasks: 1 (limit: 2303)
     Memory: 6.7M (peak: 8.7M)
        CPU: 201ms
     CGroup: /system.slice/isc-dhcp-server.service
             └─1183 /usr/sbin/dhcpd -4 -q -cf /etc/dhcp/dhcpd.conf enp0s9
```

## B. Race !

\*\* ➜ Now race !

- rallumez le serveur DHCP légitime

```bash
[israel@dhcp ~]$ sudo systemctl start dhcpd
[sudo] password for israel:
Sorry, try again.
[sudo] password for israel:
[israel@dhcp ~]$ systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; preset: disabled)
     Active: active (running) since Mon 2026-01-12 20:33:46 CET; 2s ago
       Docs: man:dhcpd(8)
             man:dhcpd.conf(5)
   Main PID: 1323 (dhcpd)
     Status: "Dispatching packets..."
      Tasks: 1 (limit: 10640)
     Memory: 4.7M (peak: 4.9M)
        CPU: 11ms
     CGroup: /system.slice/dhcpd.service
             └─1323 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid

Jan 12 20:33:46 dhcp.tp1 dhcpd[1323]:
Jan 12 20:33:46 dhcp.tp1 dhcpd[1323]: No subnet declaration for enp0s3 (no IPv4 addresses).
Jan 12 20:33:46 dhcp.tp1 dhcpd[1323]: ** Ignoring requests on enp0s3.  If this is not what
Jan 12 20:33:46 dhcp.tp1 dhcpd[1323]:    you want, please write a subnet declaration
Jan 12 20:33:46 dhcp.tp1 dhcpd[1323]:    in your dhcpd.conf file for the network segment
Jan 12 20:33:46 dhcp.tp1 dhcpd[1323]:    to which interface enp0s3 is attached. **
Jan 12 20:33:46 dhcp.tp1 dhcpd[1323]:
Jan 12 20:33:46 dhcp.tp1 dhcpd[1323]: Sending on   Socket/fallback/fallback-net
Jan 12 20:33:46 dhcp.tp1 dhcpd[1323]: Server starting service.
Jan 12 20:33:46 dhcp.tp1 systemd[1]: Started DHCPv4 Server Daemon.
```

- demandez une IP avec un client (VPCS) et voyez qui répond

```bash
node1.tp1> dhcp
DORA
node1.tp1> show ip IP 10.1.1.210/24

NAME        : node1.tp1[1]
IP/MASK     : 10.1.1.210/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.252
DHCP LEASE  : 591, 600/300/525
MAC         : 00:50:79:66:68:00
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:20001
MTU         : 1500

```

- faites-le plusieurs fois pour voir si c'est consistant
- je vous conseille de pop un nouveau VPCS à chaque fois, c'est très rapide

```bash
node1.tp1> show ip IP 10.1.1.210/24

NAME        : node1.tp1[1]
IP/MASK     : 10.1.1.210/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.252
DHCP LEASE  : 597, 600/300/525
MAC         : 00:50:79:66:68:00
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:20001
MTU         : 1500

node1.tp1> clear ip
IPv4 address/mask, gateway, DNS, and DHCP cleared

node1.tp1> dhcp
DORA IP 10.1.1.12/24 GW 10.1.1.1

node1.tp1> clear ip
IPv4 address/mask, gateway, DNS, and DHCP cleared

node1.tp1> dhcp
DORA IP 10.1.1.12/24 GW 10.1.1.1

node1.tp1> clear ip
IPv4 address/mask, gateway, DNS, and DHCP cleared

node1.tp1> dhcp
DORA
node1.tp1> show ip IP 10.1.1.210/24

NAME        : node1.tp1[1]
IP/MASK     : 10.1.1.210/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.252
DHCP LEASE  : 462, 468/234/409
MAC         : 00:50:79:66:68:00
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:20001
MTU         : 1500

node1.tp1> clear ip
IPv4 address/mask, gateway, DNS, and DHCP cleared

node1.tp1> dhcp
DORA
node1.tp1> show ip IP 10.1.1.210/24

NAME        : node1.tp1[1]
IP/MASK     : 10.1.1.210/24
GATEWAY     : 0.0.0.0
DNS         :
DHCP SERVER : 10.1.1.252
DHCP LEASE  : 591, 600/300/525
MAC         : 00:50:79:66:68:00
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:20001
MTU         : 1500

```

\*\* ➜ Wireshark this please

- gagnez la course avec la machine attaquante 🚲
- capturez cette course :d
