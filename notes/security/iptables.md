# IPtable

- FILTER – this is the default table, which contains the built in chains for:
  - INPUT  – packages destined for local sockets
  - FORWARD – packets routed through the system
  - OUTPUT – packets generated locally
- NAT – a table that is consulted when a packet tries to create a new connection. It has the following built-in:
  - PREROUTING – used for altering a packet as soon as it’s received
  - OUTPUT – used for altering locally generated packets
  - POSTROUTING – used for altering packets as they are about to go out
- MANGLE – this table is used for packet altering. Until kernel version 2.4 this table had only two chains, but they are now 5:
  - PREROUTING – for altering incoming connections
  - OUTPUT – for altering locally generated  packets
  - INPUT – for incoming packets
  - POSTROUTING – for altering packets as they are about to go out
  - FORWARD – for packets routed through the box

```sh
systemctl <start|stop|restart> iptables
iptables -L -n -v           # Service State
  iptables -t nat -L -v -n  # -t: show a specific table
iptables -A INPUT -s xxx.xxx.xxx.xxx -j DROP  # Block and specific address
  iptables -A INPUT -p tcp -s xxx.xxx.xxx.xxx -j DROP # -p: protocol
iptables -D INPUT -s xxx.xxx.xxx.xxx -j DROP  # Unblock IP address
iptables -A <OUTPUT|INPUT> -p tcp --dport xxx -j DROP
  iptables -A <INPUT|OUTPUT>  -p tcp -m multiport --<d|s>ports 22,80,443 -j ACCEPT  # multiple ports

iptables -A OUTPUT -p tcp -d 192.168.100.0/24 --dport 22 -j ACCEPT  # Network range on port 22
## Block facebook
host facebook.com                 # -> facebook.com has address 66.220.156.68
whois 66.220.156.68 | grep CIDR   # -> CIDR: 66.220.144.0/20
iptables -A OUTPUT -p tcp -d 66.220.144.0/20 -j DROP
```

## References

- 25 Useful IPtable Firewall Rules[https://www.tecmint.com/linux-iptables-firewall-rules-examples-commands/]
