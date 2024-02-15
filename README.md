# IPV6-Tunnel

### Learn how to set up and configure 6to4 IPv6 tunneling on Linux with this guide.

## دستورات تانل سرور ایران
1= Open the `/etc/rc.local file:`

```
nano /etc/rc.local
```

2= Copy and paste the following commands into the file, replacing the placeholder IPs with your external and Iranian IPs:

```
#!/bin/bash

ip tunnel add 6to4tun_IR mode sit remote ipharej local ipiran
ip -6 addr add 2001:470:1f10:e1f::1/64 dev 6to4tun_IR
ip link set 6to4tun_IR mtu 1480
ip link set 6to4tun_IR up
# confige tunnele GRE6 ya IPIPv6 IR
ip -6 tunnel add GRE6Tun_IR mode ip6gre remote 2001:470:1f10:e1f::2 local 2001:470:1f10:e1f::1
ip addr add 172.16.1.1/30 dev GRE6Tun_IR
ip link set GRE6Tun_IR mtu 1436
ip link set GRE6Tun_IR up

iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X
iptables -t mangle -F
iptables -t mangle -X
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
iptables -A FORWARD  -j ACCEPT
sudo sysctl -w net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward=1" > /etc/sysctl.conf
sysctl -p
service iptables save
service iptables restart
service iptables stop
service iptables start
```

3= After saving and exiting, run the following commands in order:

```
chmod +x /etc/rc.local
echo "net.ipv4.ip_forward=1" > /etc/sysctl.conf 
sysctl -p
/etc/rc.local
```
## دستورات تانل سرور خارج

1.Open the `/etc/rc.local file`:

```
nano /etc/rc.local
```

2= Copy and paste the following commands into the file, replacing the placeholder IPs with your external and Iranian IPs:

```
#!/bin/bash
ip tunnel add 6to4tun_KH mode sit remote ipIran local ipKharej
ip -6 addr add 2001:470:1f10:e1f::2/64 dev 6to4tun_KH
ip link set 6to4tun_KH mtu 1480
ip link set 6to4tun_KH up

ip -6 tunnel add GRE6Tun_KH mode ip6gre remote 2001:470:1f10:e1f::1 local 2001:470:1f10:e1f::2
ip addr add 172.16.1.2/30 dev GRE6Tun_KH
ip link set GRE6Tun_KH mtu 1436
ip link set GRE6Tun_KH up

iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X
iptables -t mangle -F
iptables -t mangle -X
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
iptables -A FORWARD  -j ACCEPT
sudo sysctl -w net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward=1" > /etc/sysctl.conf
sysctl -p
service iptables save
service iptables restart
service iptables stop
service iptables start
```

3= After saving and exiting, run the following commands in order:

```
chmod +x /etc/rc.local
echo "net.ipv4.ip_forward=1" > /etc/sysctl.conf 
sysctl -p
/etc/rc.local
```


Now both of your servers are connected, and you have a local IPv6 address:

IP Kharej: `2001:470:1f10:e1f::2`

IP Iran: `2001:470:1f10:e1f::1`
### الان از آیپی ساخته شده برای خارج میتونید برای تانل مثل دکودمودور استفاده کنید
