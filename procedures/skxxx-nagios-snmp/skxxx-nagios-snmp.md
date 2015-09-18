# SKXXX - Nagios: Simple Network Management Protocol
## Purpose - Automatically monitor assets' uptime
## Procedure

## Installation
### SNMP
```bash
sudo yum -y install net-snmp net-snmp-utils
sudo systemctl enable snmpd.service
```
## Configuration
### Create a new snmpd.conf file
```bash
mv /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.old
vim /etc/snmp/snmpd.conf
```
> _Insert_:
> ```bash
> rocommunity	public	192.168.4.30
> syslocation	"Watervliet, New York"
> syscontact	"JeffreyW@sks-bottle.com
> ```

## Allow the SNMP port
```bash
sudo firewall-cmd --permanent --zone=public --add-port=161/udp
sudo firewall-cmd --reload
```
## Add the SNMP user
```bash
sudo systemctl stop snmpd.service
net-snmp-create-v3-user -ro -A LINux1986$%^ -a SHA -X snmpv3encPass -x AES snmpnagios
sudo systemctl start snmpd.service
```
## Test SNMP
1. Locally
```bash
snmpwalk -u snmpnagios -A LINux1986$%^ -a SHA -X snmpv3encPass -x AES -l authPriv 127.0.0.1 -v3
```
2. Remotely
```bash
snmpwalk -u snmpnagios -A snmpnagios -a SHA -X snmpv3encPass -x AES -l authPriv [remote_ip_address] -v3
```

## Restart SNMP
```bash
sudo systemctl restart snmpd.service
```

\pagebreak

## Additional Knowledge
### Delete the SNMPv3 user
```bash
sudo sed -i '/rouser/d' /etc/snmp/snmpd.conf
sudo sed -i '/usmUser/d' /var/lib/net-snmp/snmpd.conf
sudo systemctl restart snmpd.service
```
### SNMP Object Identifiers
|Parameter|OID|
|:-|:-|
|1 Minute Load|.1.3.6.1.4.1.2021.10.1.3.1|
|5 Minute Load|.1.3.6.1.4.1.2021.10.1.3.2|
|15 Minute Load|.1.3.6.1.4.1.2021.10.1.3.3|
|Ideal CPU Time|.1.3.6.1.4.1.2021.11.11.0|
|System Uptime|.1.3.6.1.2.1.25.1.1.0|
|Total Swap|.1.3.6.1.4.1.2021.4.3.0|
|Available Swap|.1.3.6.1.4.1.2021.4.4.0|
|Total RAM|.1.3.6.1.4.1.2021.4.5.0|
|Total Free RAM|.1.3.6.1.4.1.2021.4.11.0|
|RAM Used|.1.3.6.1.4.1.2021.4.6.0|

