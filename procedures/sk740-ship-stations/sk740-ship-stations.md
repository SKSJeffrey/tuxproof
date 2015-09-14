#SKXXX - How to Setup a Ship Station
## Purpose - Ensure the Ship Station has the necessary tools for daily operation
## Procedure -

## Ship station client software
Run as Administrator: PowerShell ISE

Execute `\\sksna01\Software\Standalone\PowerShellScripts\ShipStationSetup\ShipStation.ps1`

## Zebra printer setup
1. Press and hold the feed button until the status light flashes once

2. Two labels of configuration settings will print out, but the only setting that we need to pay attention to is the **IP Address**
        * It will be in the format of: `192.168.001.XXX`

3. Log into the web browser with the printer's IP address

4. View and Modify Printer Settings
        * Enter Password: 1234
        * Click to Proceed

5. Network Configuration
        * TCP/IP Settings
        * Input an available static IP address
        * Change IP Protocol to Permanent
        * Submit Changes
        * Save Current Configuration

7. Go back to the Home page
        * Printer Controls
        * Reset Printer

8. The printer's status light will turn orange, and then green once it is ready

9. Verify the static IP address took effect by logging into the web browser with said IP address

## Install Zebra Printer to Prd-ConnectShip
1. RDP into `Prd-ConnectShip` and add the zebra printer via it's static IP address

2. Install the Zebra ZP 450-200 driver

3. The share name should be `[Location]Zebra`

## Change Database Values
`UPDATE CS_Portal.ship_station SET ship_label_printer_path='\\\\Prd-ConnectShip\\<New_Zebra_Printer_Share_Name>' WHERE ship_id=#;`

