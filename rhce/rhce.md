## Configure Network Services to Start Automatically at Boot: Sysvinit Method
Old Style
```bash
$ chkconfig telnet on
$ chkconfig telnet off

$ service telnet start
$ service telnet stop

$ service telnet status
```

## Configure Network Services to Start Automatically at Boot: SystemD Method
New Style
```bash
$ systemctl enable telnet
$ systemctl disable telnet

$ systemctl start telnet
$ systemctl stop telnet

$ systemctl status telnet
```

To verify if the service is on
```bash
$ ss -tnlp | grep ##
-t: TCP packets
-n: numeric
-l: listening
-p: process
```

## Implement Packet Filtering
Amazing guide from RedHat that describes basic [IPTables](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html-single/Security_Guide/index.html#sect-Security_Guide-Firewalls-Using_IPTables)
INPUT - If the destination is __to__ this server

OUTPUT - If the destination is __from__ this server

The default policy for a chain can be either DROP or ACCEPT. For security-minded individuals, a default policy chain of DROP is recommended.
```bash
# Block all incoming and outgoing packets on a network gateway
$ iptables -P INPUT DROP
$ iptables -P OUTPUT DROP
-P: Policy for the chain (INPUT, OUTPUT, FORWARD)
```

FORWARD - Network traffic that is routed from the firewall to its destination node

To restrict internal clients from inadvertent exposure to the Internet
```bash
$ iptables -P FORWARD DROP
```

To save the IPTables' rules for it to become persistent after a reboot
```bash
$ service iptables save
```

Allow access to port 80 on the firewall
```bash
$ iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
-A: append 1+ rule to the end of a selected chain
-p: protocol
-m: specifies a match to use
-j: specifies the jump (target) of the rule; what to do if the packet matches
```

Allow access to port 443 on the firewall (for HTTPS sites)
```bash
$ iptables -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT
```

Order is important. All network traffic must be allowed first and then dropped. To input a rule in position 1
```bash
$ iptables -I INPUT 1 -i lo all -j ACCEPT
```

Flush out all of the IPTables and deny ICMP packets
```bash
$ iptables --flush

# Drop all ICMP packets (still works via localhost)
$ iptables -A INPUT --protocol icmp --in-interface <interface> -j DROP
# Same thing as
$ iptables -A INPUT -p icmp -i <interface> -j DROP
```

## Monitor Network Performance
```bash
$ ss -tnlp | grep ##
-t: TCP packets
-n: numeric
-l: listening
-p: process

$ nmap -A localhost
```
