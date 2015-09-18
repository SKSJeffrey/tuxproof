# SKXXX - ConnectShip Toolkit 6.5
## Purpose - Configure a ConnectShip Server to manage all shipments
## Procedure

## Windows Server 2008 R2: Microsoft IIS Installation
Microsoft IIS has to be installed first before the ConnectShip Toolkit, otherwise, the Progistics Start Page will not load properly

Once the Progistics Start Page is loaded via Internet Explorer > View Compatibility Settings > add localhost

## Role Services
**Common HTTP Features**  
Static Content  
Default Document  
Directory Browsing  
HTTP Errors  

**Application Development**  
ASP.NET  
.NET Extensibility  
ASP  
CGI  
ISAPI Extensions  
ISAPI Filters  
Server Side Includes  

**Health and Diagnostics**  
HTTP Logging  
Request Monitor  

**Security**  
Basic Authentication  
Windows Authentication  
Digest Authentication  
Client Certificate Mapping Authentication  
IIS Client Certificate Mapping Authentication  
URL Authorization  
Request Filtering  
IP and Domain Restrictions  

**Performance**  
Static Content Compression  

**Management Tools**  
IIS Management Console  
IIS Management Scripts and Tools  
Management Service  

# Configuration
Create Folder > "C:\\UPSAST\\ConnectShipDownload"  
Create Folder > "C:\\UPSAST\\ConnectShipUpdates"

Start > All Programs > ConnectShip Progistics > Right-click **Progistics Management Console** > Open as Administrator

### Enterprise > Licensing  
License ID: 93693  
Password: szr6hVBP  

### Versions and Updates > Updates  
Double-click each update and save it into C:\\UPSAST\\ConnectShipUpdates  
First time, it will prompt for a ConnectShip Members Area Login  
Username: robertdesarbo  
Password: Con2012necT  
Install Now > No

Close Progistics Management Console > Navigate into C:\\UPSAST\\ConnectShipUpdates  
Order of installation:  
Install the COREs first  
Install the COMs second  
Install the remainder in any order

Re-open the Progistics Management Console as Administrator  
Versions and Updates > Updates > Verify that all of the updates have been installed

After every update, **maintenance** is required on that server component  
Right-click Server Components > Run Maintenance on all server components  

Right-click Shipper Configuration > Add Shipper  

**Watervliet**  
Abbreviation: SHIP1  
Company: SKS Bottle & Packaging, Inc.  
Address 1: 2600 7th Ave  
City: Watervliet  
State: NY   
Postal Code: 12189-0000  
Country: United States  
Country Alias: US  
Contact: SKS Industries  
Phone: 518-880-6980

Right-click Shipper Configuration > Add Shipper  

**Sparks**  
Abbreviation: SHIP2  
Company: SKS Bottle & Packaging, Inc.  
Address 1: 46 Insidor Ct.  
City: Sparks  
State: NV   
Postal Code: 89436-0000  
Country: United States  
Country Alias: US  
Contact: SKS Industries  
Phone: 518-880-6980

Shipper Configuration > Select SHIP1 > Select UPS (Consolidated) > Right-click > Register  
Shipper Configuration > Select SHIP1 > Select UPS (UPS Freight) > Right-click > Register  

Shipper Configuration > Select SHIP2 > Select UPS (Consolidated) > Right-click > Register  
Shipper Configuration > Select SHIP2 > Select UPS (UPS Freight) > Right-click > Register  

## SHIP1

Shipper Configuration > SKS Bottle & Packaging, Inc. (SHIP1) > UPS (Consolidated) > Account Configuration  
_General Tab_  
**Shipper Account**  
UPS Account Number: 104183  
Origin Location Postal Code: 12189-1958  
Watervliet

**Tracking Numbers**  
Check "Automatically Generate Tracking Numbers"  
Start: Check the latest UPS package and increase that number by a little to avoid re-using of UPS tracking numbers  
End: 9999999

_Contract Options Tab_  
**Book Numbers**  
PLD Book Number 1: 1234567  
PLD Book Number 2: 1234568

**Permitted Service Options**  
Check "Paperless Invoice"

Save

Shipper Configuration > SKS Bottle & Packaging, Inc. (SHIP1) > UPS (UPS Freight) > Account Configuration  
_General Tab_  
Origin Postal Code: 12189

Save

## SHIP2
Shipper Configuration > SKS Bottle & Packaging, Inc. (SHIP2) > UPS (Consolidated) > Account Configuration

_General Tab_  
**Shipper Account**  
UPS Account Number: E83956  
Origin Location Postal Code: 89436-0000  
Sparks

**Tracking Numbers**  
Check "Automatically Generate Tracking Numbers"  
Start: Check the latest UPS package and increase that number by a little to avoid re-using of UPS tracking numbers  
End: 9999999

_Contract Options Tab_  
**Book Numbers**  
PLD Book Number 1: 1234567  
PLD Book Number 2: 1234568

**Permitted Service Options**  
Check "Paperless Invoice"

Save  
Shipper Configuration > SKS Bottle & Packaging, Inc. (SHIP2) > UPS (UPS Freight) > Account Configuration  
_General Tab_  
Origin Postal Code: 89436  

Save

## Development Environment
Carrier Configuration > UPS (Consolidated)  
Check "Use Test Mode (disables all data communication with the server)"

## Production Environment: Commission a Production Server
On the old ConnectShip server, record the `Last Tracking Number Used`

Carrier Configuration > UPS (Consolidated) > SKS Bottle & Packaging, Inc. (SHIP1) > Account Configuration  
_General Tab_  
Advanced... > Record `Last Tracking Number Used`  

Carrier Configuration > UPS (Consolidated) > SKS Bottle & Packaging, Inc. (SHIP2) > Account Configuration  
_General Tab_  
Advanced... > Record `Last Tracking Number Used`

On the new ConnectShip server, increase the `Last Tracking Number Used` by a couple thousand to avoid tracking number conflicts

Carrier Configuration > UPS (Consolidated)  
_UPSlinkHTTP Setup Tab_
Commission: SKS Bottle & Packaging, Inc. (SHIP1)  
Site Contact: Steven R. Horan  
Site Name: SKS Bottle & Packaging, Inc.  
Site Phone Number: 518-880-6980  
Site Address 1: 2600 7th Ave  
Site City: Watervliet  
Site Postal Code: 12189  
Site State: New York  
Site Country: United States  

Register Additional Shippers  
Shipper: SKS Bottle & Packaging, Inc. (SHIP2)  

Download Configuration Parameters > Download and load parameters

Download Distribution Objects > Download and load objects

