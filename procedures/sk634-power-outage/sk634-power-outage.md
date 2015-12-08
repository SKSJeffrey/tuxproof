# SK634 - Planned Power Outage
## Purpose - Properly shut down all electrical equipment
## Procedure

## General Office/Warehouse Equipment
* Use SpecOps on Active Directory to remotely shut down all Windows Computers
* SSH into each Linux machine in the warehouse to `telinit 0`

## VMware vSphere Client
Launch the VMware vSphere Client and enable SSH on ESXi01 and ESXi02

![ESXi-SSH1](ESXi-SSH1.jpg)

\pagebreak

![ESXi-SSH2](ESXi-SSH2.jpg)

## Power Off
### Virtual Machines
There are two options to turning off guest VMs

1. Use the vSphere client [ **DO NOT USE THIS - UNGRACEFUL SHUTDOWN** ]
	* Right-click each virtual machine and Power Off
2. SSH into each VM and gracefully poweroff. Below are VMs that require more special attention before powering off

|Server|IP Address|Procedure|
|:-|:-|:-|
|AlienVault Server|192.168.1.229|Option 7 - Shutdown Appliance|
|AlienVault Sensor|192.168.1.230|Option 7 - Shutdown Appliance|
|SKS-Bottle Slave|192.168.4.21|`mysqladmin -u root -p stop-slave; telinit 0`|
|SKS-Connect Slave|192.168.4.20|`mysqladmin -u root -p stop-slave; telinit 0`|
|SKS-Science Slave|192.168.4.22|`mysqladmin -u root -p stop-slave; telinit 0`|
|SKSDC01|192.168.1.224|`vim-cmd vmsvc/power.shutdown vID`|
|SKSDC02|192.168.1.7|`vim-cmd vmsvc/power.shutdown vID`|
|SKSVC01|192.168.223|`vim-cmd vmsvc/power.shutdown vID`|
|webdept|192.168.1.4|Windows Shutdown|

### ESXi Hosts
Once all of the guest VMs are turned off, only then, can the ESXI host can shut down

1. SSH into ESXi01/02

	* ESXi01 - 10.1.1.223
	* ESXi02 - 10.1.1.224

2. On both of the ESXi servers, bring them into Maintenance Mode, Shutdown delay, and Exit Maintenance Mode
```bash
esxcli system maintenanceMode set -e true -t 0
esxcli system shutdown poweroff -d 10 -r "Shell initiated system shutdown"
esxcli system maintenanceMode set -e false -t 0
```

### NetApp Storage
\pagebreak
Enter 5 minutes to initiate a clean system halt

![NetApp1](NetApp1.jpg)

Once the NetApp has come to a halt, flip the power switches on the back of the NetApp

### Firewall/Switches
Unplug the Switches

Unplug the Firewall

## Power On
The procedure would be the opposite 

## Common Issues
When all of the equipment is powered on, there may be no Internet access. This is related to duplicate VMs on both of the ESXI hosts. In order to resolve this issue, it would require a Windows computer with a vSphere client.

Log into the vSphere client with **one** of the following IP addresses:

> IP Address: 10.1.1.223 or 10.1.1.224

In the Summary Tab, it may ask a question that you will have to respond to

Turn on SKSDC01, SKSDC02, and SKSVC01 on **one** of the hosts (10.1.1.223 OR 10.1.1.224)

Once those are on, sign into the vSphere Client with Windows session credentials

If signing in does not work, double check the IP address of SKSVC01  
