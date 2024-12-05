# GNS3-tp-


# GNS3-tp-
## partie 1
##Déterminer l'adresse MAC de vos deux machines
ip a
##node 1
0c:f0:c9:a4:00:00
##node 2 
0c:5c:37:d1:00:00 
##Définir une IP statique sur les deux machines
sudo nano /etc/network/interfaces
## node 1
 Static config for ens4
auto ens4
#and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

#The loopback network interface
auto lo
iface lo inet loopback

#DHCP config for ens4
#auto ens4
#iface ens4 inet dhcp
iface ens4 inet static
        address 10.1.1.1
        netmask 255.255.255.0
        gateway 10.1.1.100
        dns-nameservers 10.1.1.100
##node 2 
#and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

#The loopback network interface
auto lo
iface lo inet loopback

#DHCP config for ens4
#auto ens4
#iface ens4 inet dhcp      
 Static config for ens4
auto ens4
iface ens4 inet static
        address 10.1.1.2
        netmask 255.255.255.0
        gateway 10.1.1.2000
        dns-nameservers 10.1.1.200

sudo systemctl restart networking
sudo ifup ens4
ip a 
## node 1
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:f0:c9:a4:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.1/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::ef0:c9ff:fea4:0/64 scope link
       valid_lft forever preferred_lft forever
## node 2
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:5c:37:d1:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.2/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::e5c:37ff:fed1:0/64 scope link
       valid_lft forever preferred_lft forever


## effectuer un ping d'une machine à l'autre

ping 10.1.1.2
64 bytes from 10.1.1.2: icmp_seq=1 ttl=64 time=10.9 ms
64 bytes from 10.1.1.2: icmp_seq=2 ttl=64 time=4.89 ms
64 bytes from 10.1.1.2: icmp_seq=3 ttl=64 time=2.82 ms
64 bytes from 10.1.1.2: icmp_seq=4 ttl=64 time=4.52 ms
64 bytes from 10.1.1.2: icmp_seq=5 ttl=64 time=3.00 ms
64 bytes from 10.1.1.2: icmp_seq=6 ttl=64 time=3.88 ms
64 bytes from 10.1.1.2: icmp_seq=7 ttl=64 time=2.84 ms

## ARP

ip neigh show
10.1.1.2 dev ens4 lladdr 0c:5c:37:d1:00:00 STALE

## partie 2
##Déterminer l'adresse MAC de vos trois machines

ip link show ens4 | grep link/ether
##node1:
0c:f0:c9:a4:00:00
##node2:
0c:5c:37:d1:00:00
##node3:
0c:a9:37:ed:00:00

##Définir une IP statique sur les trois machines

sudo nano /etc/network/interfaces

##ip node 1:
 Static config for ens4
auto ens4
iface ens4 inet static
        address 10.1.1.1
        netmask 255.255.255.0
        gateway 10.1.1.100
        dns-nameservers 10.1.1.100

##ip node  2:        
 Static config for ens4
auto ens4
iface ens4 inet static
        address 10.1.1.2
        netmask 255.255.255.0
        gateway 10.1.1.200
        dns-nameservers 10.1.1.200

##ip node 3:        
 Static config for ens4
auto ens4
iface ens4 inet static
        address 10.1.1.3
        netmask 255.255.255.0
        gateway 10.1.1.300
        dns-nameservers 10.1.1.300


sudo systemctl restart networking
ip addr show ens4
ip a 
##ip node 1:
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:f0:c9:a4:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.1/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::ef0:c9ff:fea4:0/64 scope link
       valid_lft forever preferred_lft forever
##ip2 node 2:
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:5c:37:d1:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.2/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::e5c:37ff:fed1:0/64 scope link
       valid_lft forever preferred_lft forever
##ip node 3 :
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:a9:37:ed:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.3/24 brd 10.1.1.255 scope global ens4
       valid_lft forever preferred_lft forever
    inet6 fe80::ea9:37ff:feed:0/64 scope link
       valid_lft forever preferred_lft forever

##effectuer des ping d'une machine à l'autre 

##node1 - node2 

 ping 10.1.1.2
PING 10.1.1.2 (10.1.1.2) 56(84) bytes of data.
64 bytes from 10.1.1.2: icmp_seq=1 ttl=64 time=6.25 ms
64 bytes from 10.1.1.2: icmp_seq=2 ttl=64 time=1.61 ms
64 bytes from 10.1.1.2: icmp_seq=3 ttl=64 time=2.48 ms
64 bytes from 10.1.1.2: icmp_seq=4 ttl=64 time=1.76 ms

##node1 - node 3

 ping 10.1.1.3
PING 10.1.1.3 (10.1.1.3) 56(84) bytes of data.
64 bytes from 10.1.1.3: icmp_seq=1 ttl=64 time=3.32 ms
64 bytes from 10.1.1.3: icmp_seq=2 ttl=64 time=2.92 ms
64 bytes from 10.1.1.3: icmp_seq=3 ttl=64 time=2.54 ms
64 bytes from 10.1.1.3: icmp_seq=4 ttl=64 time=2.13 ms

##node2 - node3

 ping 10.1.1.3
PING 10.1.1.3 (10.1.1.3) 56(84) bytes of data.
64 bytes from 10.1.1.3: icmp_seq=1 ttl=64 time=5.70 ms
64 bytes from 10.1.1.3: icmp_seq=2 ttl=64 time=1.99 ms
64 bytes from 10.1.1.3: icmp_seq=3 ttl=64 time=1.94 ms
64 bytes from 10.1.1.3: icmp_seq=4 ttl=64 time=2.56 ms

---------
##etape3
Donner un accès internet à la machine 
sudo nano /etc/network/interfaces

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
 ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=110 time=55.6 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=110 time=52.8 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=110 time=51.4 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=110 time=48.5 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=110 time=115 ms
64 bytes from 8.8.8.8: icmp_seq=6 ttl=110 time=64.4 ms

##Installer et configurer un serveur DHCP

sudo nano /etc/network/interfaces

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

apt update

apt install isc-dhcp-server

nano /etc/dhcp/dhcpd.conf

subnet 10.1.1.0 netmask 255.255.255.0 {
  range 10.1.1.10 10.1.1.50;
  option routers 10.1.1.100;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  default-lease-time 600;
  max-lease-time 7200;
}
nano /etc/default/isc-dhcp-server

INTERFACESv4="ens4"
INTERFACESv6=""

systemctl start isc-dhcp-server
systemctl enable isc-dhcp-server

##Récupérer une IP automatiquement depuis les 3 nodes

nano /etc/network/interfaces
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

sudo systemctl restart networking.service
sudo ifup ens4


node1 
 ip a
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:f0:c9:a4:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.10/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 349sec preferred_lft 349sec
    inet6 fe80::ef0:c9ff:fea4:0/64 scope link
       valid_lft forever preferred_lft forever
       
node2
ip a
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:5c:37:d1:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.11/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 587sec preferred_lft 587sec
    inet6 fe80::e5c:37ff:fed1:0/64 scope link
       valid_lft forever preferred_lft forever
node3

root@debian:/home/debian# ip a
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:a9:37:ed:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.12/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 598sec preferred_lft 598sec
    inet6 fe80::ea9:37ff:feed:0/64 scope link
       valid_lft forever preferred_lft forever


etape final 

Configurez dnsmasq
sudo apt install dnsmasq
sudo nano /etc/dnsmasq.conf

dhcp-authoritative
# Plage DHCP
dhcp-range=10.1.1.210,10.1.1.250,12h
# Netmask
dhcp-option=1,255.255.255.0
# Route
dhcp-option=3,192.168.1.1
/etc/init.d/dnsmasq restart
 Test !
 
 ip a
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 0c:a9:37:ed:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.219/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 43133sec preferred_lft 43133sec
    inet6 fe80::e9e:37ff:fe0:0/64 scope link
       valid_lft forever preferred_lft forever
       
Now race

1er essai 

root@debian:/home/debian# ip a
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
       
      
2éme essai

root@debian:/home/debian# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default     qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group     default qlen 1000
    link/ether 0c:5c:37:d1:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.16/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 597sec preferred_lft 597sec
    inet6 fe80::e5c:37ff:fed1:0/64 scope link
       valid_lft forever preferred_lft forever
       
       
3 éme essai 

root@debian:/home/debian# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default     qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute
       valid_lft forever preferred_lft forever
2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group     default qlen 1000
    link/ether 0c:a9:37:ed:00:00 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    inet 10.1.1.17/24 brd 10.1.1.255 scope global dynamic ens4
       valid_lft 546sec preferred_lft 546sec
    inet6 fe80::ea9:37ff:feed:0/64 scope link
       valid_lft forever preferred_lft forever
