% SK870 - How to Setup a New Computer
% 2015-08-18
% Jeffrey Wen

## Purpose: Ensure that a new computer has the proper configurations for daily operation 
___
**Unjoin** computer from the SKS domain - Administrator: Windows Powershell
```bash
Remove-Computer -Credential "SKS\Domain-Administrator"; Restart-Computer
```
___
**Join** computer to the SKS domain - Administrator: Windows Powershell
```bash
Add-Computer -DomainName sks.com -Credential "SKS\Domain-Administrator"; Restart-Computer
```
___
### Connect peripherals
Install the necessary drivers for printers and scanners

Obtain the drivers from the manufacturer's website or the network drive `\\sksna01\Software\Standalone\Printer Drivers`
