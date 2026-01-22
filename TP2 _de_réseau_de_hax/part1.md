## 7. Proof or lie¶

\*\* 🌞 Tout le monde doit pouvoir se ping

- au sein du même LAN :

- node1 peut ping node2

```bash
node1> ping 10.2.1.12

10.2.1.12 icmp_seq=1 timeout
10.2.1.12 icmp_seq=2 timeout
10.2.1.12 icmp_seq=3 timeout
10.2.1.12 icmp_seq=4 timeout
10.2.1.12 icmp_seq=5 timeout

```

- node3 peut ping node4

```bash
node3> ping 10.2.2.12

84 bytes from 10.2.2.12 icmp_seq=1 ttl=64 time=3.027 ms
84 bytes from 10.2.2.12 icmp_seq=2 ttl=64 time=1.164 ms
84 bytes from 10.2.2.12 icmp_seq=3 ttl=64 time=0.545 ms
84 bytes from 10.2.2.12 icmp_seq=4 ttl=64 time=0.481 ms
84 bytes from 10.2.2.12 icmp_seq=5 ttl=64 time=1.157 ms


```

- le routage doit fonctionner :

- node1 peut ping node3

```bash
node1> ping 10.2.2.11

84 bytes from 10.2.2.11 icmp_seq=1 ttl=63 time=33.680 ms
84 bytes from 10.2.2.11 icmp_seq=2 ttl=63 time=17.702 ms
84 bytes from 10.2.2.11 icmp_seq=3 ttl=63 time=21.541 ms
84 bytes from 10.2.2.11 icmp_seq=4 ttl=63 time=15.067 ms
84 bytes from 10.2.2.11 icmp_seq=5 ttl=63 time=15.611 ms

```

- node4 peut ping node2

```bash
node4> ping 10.2.1.11

84 bytes from 10.2.1.11 icmp_seq=1 ttl=63 time=21.371 ms
84 bytes from 10.2.1.11 icmp_seq=2 ttl=63 time=16.818 ms
84 bytes from 10.2.1.11 icmp_seq=3 ttl=63 time=22.708 ms
84 bytes from 10.2.1.11 icmp_seq=4 ttl=63 time=16.655 ms
84 bytes from 10.2.1.11 icmp_seq=5 ttl=63 time=12.676 ms

```
