# SKXXX - Connecting a Physical Switch to a Virtual Switch
## Purpose - Monitor all traffic going into a physical switch
## Procedure

## Create a SPAN port on the pSwitch
Assume the following:  
pSwitch Gi1/0/1 is connected to the Firewall  
pSwitch Gi1/0/2 is connected to the ESXI host  
The commands below will mirror the traffic going through the Firewall on Gi1/0/1 into Gi1/0/2  

## SSH into the pSwitch
```bash
# monitor session 1 source interface Gi1/0/1
# monitor session 1 destination interface Gi1/0/2 encapsulation replicate
```
Traffic should be mirroring onto port Gi1/0/2  

Run a cable from pSwitch Gi1/0/2 to the vSwitch (replacing the vMotion port)  

By now, there should be a cable running from the pSwitch to the vSwitch  

## Configure pSwitch Gi1/0/2 to be on the workstation VLAN
The workstation VLAN is typically VLAN 1

## SSH into the pSwitch
```bash
# configure terminal
(config) interface gigabitEthernet 1/0/2
(config) switchport mode access
(config) switchport access VLAN 1
(config) exit
# write memory
```
