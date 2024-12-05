
# GNS3-tp-

## Partie 1

### Déterminer l'adresse MAC de vos deux machines

```bash
ip a
```

**Node 1**  
`0c:f0:c9:a4:00:00`  

**Node 2**  
`0c:5c:37:d1:00:00`  

### Définir une IP statique sur les deux machines

```bash
sudo nano /etc/network/interfaces
```

**Node 1**  
Static config for `ens4`:

```plaintext
auto ens4
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# DHCP config for ens4
# auto ens4
# iface ens4 inet dhcp
iface ens4 inet static
        address 10.1.1.1
        netmask 255.255.255.0
        gateway 10.1.1.100
        dns-nameservers 10.1.1.100
```

**Node 2**  
Static config for `ens4`:

```plaintext
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# DHCP config for ens4
# auto ens4
# iface ens4 inet dhcp      
Static config for ens4
auto ens4
iface ens4 inet static
        address 10.1.1.2
        netmask 255.255.255.0
        gateway 10.1.1.200
        dns-nameservers 10.1.1.200
```

```bash
sudo systemctl restart networking
sudo ifup ens4
ip a 
```

**Node 1**
```plaintext
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:f0:c9:a4:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.1/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::ef0:c9ff:fea4:0/64 scope link
       valid_lft forever preferred_lft forever
```

**Node 2**
```plaintext
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:5c:37:d1:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.2/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::e5c:37ff:fed1:0/64 scope link
       valid_lft forever preferred_lft forever
```

### Effectuer un ping d'une machine à l'autre

```bash
ping 10.1.1.2
```

```plaintext
64 bytes from 10.1.1.2: icmp_seq=1 ttl=64 time=10.9 ms
64 bytes from 10.1.1.2: icmp_seq=2 ttl=64 time=4.89 ms
64 bytes from 10.1.1.2: icmp_seq=3 ttl=64 time=2.82 ms
64 bytes from 10.1.1.2: icmp_seq=4 ttl=64 time=4.52 ms
64 bytes from 10.1.1.2: icmp_seq=5 ttl=64 time=3.00 ms
64 bytes from 10.1.1.2: icmp_seq=6 ttl=64 time=3.88 ms
64 bytes from 10.1.1.2: icmp_seq=7 ttl=64 time=2.84 ms
```
 **Wireshark
    **ping.pcapng 
### ARP

```bash
ip neigh show
```

```plaintext
10.1.1.2 dev ens4 lladdr 0c:5c:37:d1:00:00 STALE
```



## Partie 2

### Déterminer l'adresse MAC de vos trois machines

```bash
ip link show ens4 | grep link/ether
```

**Node 1**  
`0c:f0:c9:a4:00:00`  

**Node 2**  
`0c:5c:37:d1:00:00`  

**Node 3**  
`0c:a9:37:ed:00:00`  

### Définir une IP statique sur les trois machines

```bash
sudo nano /etc/network/interfaces
```

**IP Node 1**  
Static config for `ens4`:

```plaintext
auto ens4
iface ens4 inet static
        address 10.1.1.1
        netmask 255.255.255.0
        gateway 10.1.1.100
        dns-nameservers 10.1.1.100
```

**IP Node 2**  
Static config for `ens4`:

```plaintext
auto ens4
iface ens4 inet static
        address 10.1.1.2
        netmask 255.255.255.0
        gateway 10.1.1.200
        dns-nameservers 10.1.1.200
```

**IP Node 3**  
Static config for `ens4`:

```plaintext
auto ens4
iface ens4 inet static
        address 10.1.1.3
        netmask 255.255.255.0
        gateway 10.1.1.300
        dns-nameservers 10.1.1.300
```

```bash
sudo systemctl restart networking
ip addr show ens4
ip a 
```

**IP Node 1**  
```plaintext
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:f0:c9:a4:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.1/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::ef0:c9ff:fea4:0/64 scope link
       valid_lft forever preferred_lft forever
```

**IP Node 2**  
```plaintext
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:5c:37:d1:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.2/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::e5c:37ff:fed1:0/64 scope link
       valid_lft forever preferred_lft forever
```

**IP Node 3**  
```plaintext
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:a9:37:ed:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.3/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::ea9:37ff:feed:0/64 scope link
       valid_lft forever preferred_lft forever
```

### Effectuer des ping d'une machine à l'autre

**Node 1 - Node 2**  

```bash
ping 10.1.1.2
```

```plaintext
PING 10.1.1.2 (10.1.1.2) 56(84) bytes of data.
64 bytes from 10.1.1.2: icmp_seq=1 ttl=64 time=6.25 ms
64 bytes from 10.1.1.2: icmp_seq=2 ttl=64 time=1.61 ms
64 bytes from 10.1.1.2: icmp_seq=3 ttl=64 time=2.48 ms
64 bytes from 10.1.1.2: icmp_seq=4 ttl=64 time=1.76 ms
```

**Node 1 - Node 3**  

```bash
ping 10.1.1.3
```

```plaintext
PING 10.1.1.3 (10.1.1.3) 56(84) bytes of data.
64 bytes from 10.1.1.3: icmp_seq=1 ttl=64 time=3.32 ms
64 bytes from 10.1.1.3: icmp_seq=2 ttl=64 time=2.92 ms
64 bytes from 10.1.1.3: icmp_seq=3 ttl=64 time=2.54 ms
64 bytes from 10.1.1.3: icmp_seq=4 ttl=64 time=2.13 ms
```

**Node 2 - Node 3**  

```bash
ping 10.1.1.3
```

```plaintext
PING 10.1.1.3 (10.1.1.3) 56(84) bytes of data.
64 bytes from 10.1.1.3: icmp_seq=1 ttl=64 time=5.70 ms
64 bytes from 10.1.1.3: icmp_seq=2 ttl=64 time=1.99 ms
64 bytes from 10.1.1.3: icmp_seq=3 ttl=64 time=1.94 ms
64 bytes from 10.1.1.3: icmp_seq=4 ttl=64 time=2.56 ms
```

## Étape 3

### Donner un accès internet à la machine

```bash
sudo nano /etc/network/interfaces
```

**Configuration :**

```plaintext
# The loopback network interface
auto lo
iface lo inet loopback
# DHCP config for ens4
auto ens4
iface ens4 inet dhcp
# Static config for ens4
#auto ens4
#iface ens4 inet static
#       address 10.1.1.253
#       netmask 255.255.255.0
#       gateway 10.1.1.50
#       dns-nameservers 10.1.1.50
```

**Tester la connectivité internet :**

```bash
ping 8.8.8.8
```

**Résultat :**

```plaintext
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=110 time=55.6 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=110 time=52.8 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=110 time=51.4 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=110 time=48.5 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=110 time=115 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=110 time=64.4 ms
```

### Installer et configurer un serveur DHCP

```bash
sudo nano /etc/network/interfaces
```

**Configuration :**

```plaintext
# The loopback network interface
auto lo
iface lo inet loopback
# DHCP config for ens4
auto ens4
iface ens4 inet dhcp
# Static config for ens4
#auto ens4
#iface ens4 inet static
#        address 10.1.1.253
#        netmask 255.255.255.0
#        gateway 10.10.1.150
#        dns-nameservers 10.1.1.150
```

**Installation du serveur DHCP :**

```bash
apt update
apt install isc-dhcp-server
```

**Configuration du fichier DHCP :**

```bash
nano /etc/dhcp/dhcpd.conf
```

**Contenu :**

```plaintext
subnet 10.1.1.0 netmask 255.255.255.0 {
  range 10.1.1.10 10.1.1.50;
  option routers 10.1.1.100;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  default-lease-time 600;
  max-lease-time 7200;
}
```

**Configuration du serveur DHCP :**

```bash
nano /etc/default/isc-dhcp-server
```

**Contenu :**

```plaintext
INTERFACESv4="ens4"
INTERFACESv6=""
```

**Démarrage du serveur DHCP :**

```bash
systemctl start isc-dhcp-server
systemctl enable isc-dhcp-server
```

### Récupérer une IP automatiquement depuis les 3 nodes

**Configuration :**

```bash
nano /etc/network/interfaces
```

```plaintext
# The loopback network interface
auto lo
iface lo inet loopback
# DHCP config for ens4
auto ens4
iface ens4 inet dhcp
# Static config for ens4
#auto ens4
#iface ens4 inet static
#       address 192.168.1.100
#       netmask 255.255.255.0
#       gateway 192.168.1.1
#       dns-nameservers 192.168.1.1
```

**Redémarrage du réseau :**

```bash
sudo systemctl restart networking.service
sudo ifup ens4
```

**Node 1 :**

```plaintext
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:f0:c9:a4:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.10/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 349sec preferred_lft 349sec
    inet6 fe80::ef0:c9ff:fea4:0/64 scope link
       valid_lft forever preferred_lft forever
```

**Node 2 :**

```plaintext
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:5c:37:d1:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.11/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 587sec preferred_lft 587sec
    inet6 fe80::e5c:37ff:fed1:0/64 scope link
       valid_lft forever preferred_lft forever
```

 **Wireshark
   **dhcp.pcapng
**Node 3 :**

```plaintext
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:a9:37:ed:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.12/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 598sec preferred_lft 598sec
    inet6 fe80::ea9:37ff:feed:0/64 scope link
       valid_lft forever preferred_lft forever
```



## Étape finale

### Configurer dnsmasq

**Installation et configuration de dnsmasq :**

```bash
sudo apt install dnsmasq
sudo nano /etc/dnsmasq.conf
```

**Contenu du fichier `/etc/dnsmasq.conf` :**

```plaintext
dhcp-authoritative
# Plage DHCP
dhcp-range=10.1.1.210,10.1.1.250,12h
# Netmask
dhcp-option=1,255.255.255.0
# Route
dhcp-option=3,192.168.1.1
```

**Redémarrer dnsmasq :**

```bash
/etc/init.d/dnsmasq restart
```

**Test :**

```bash
ip a
```

**Résultat :**

```plaintext
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:a9:37:ed:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.219/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 43133sec preferred_lft 43133sec
    inet6 fe80::e9e:37ff:fe0:0/64 scope link
       valid_lft forever preferred_lft forever
```

### Now race

#### 1er essai

```bash
ip a
```

**Résultat :**

```plaintext
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:f0:c9:a4:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.15/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 594sec preferred_lft 594sec
    inet6 fe80::ef0:c9ff:fea4:0/64 scope link
       valid_lft forever preferred_lft forever
```

#### 2ème essai

```bash
ip a
```

**Résultat :**

```plaintext
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:5c:37:d1:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.16/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 597sec preferred_lft 597sec
    inet6 fe80::e5c:37ff:fed1:0/64 scope link
       valid_lft forever preferred_lft forever
```

#### 3ème essai

```bash
ip a
```

**Résultat :**

```plaintext
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:a9:37:ed:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.17/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 546sec preferred_lft 546sec
    inet6 fe80::ea9:37ff:feed:0/64 scope link
       valid_lft forever preferred_lft forever
```
**  Wireshark
     **race.pcapng
