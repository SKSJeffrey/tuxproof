% SK870 - How to Setup a New Computer
% 2015-08-18
% Jeffrey Wen

## Purpose: Ensure that a new computer has the proper configurations for daily operation 
___
### Remove an old computer
**Unjoin** computer from the SKS domain - Administrator: Windows Powershell
```bash
remove-computer -cred "SKS\Jeffrey Wen"; shutdown -r -t 0
```
___
### Add a new computer
**Join** computer to the SKS domain - Administrator: Windows Powershell
```bash
add-computer -domain sks.com -cred "SKS\Jeffrey Wen"; shutdown -r -t 0
```
___
### Connect peripherals
Install the necessary drivers for printers and scanners

Obtain the drivers from the manufacturer's website or the network drive `\\sksna01\Software\Standalone\Printer Drivers`
