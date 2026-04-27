SSID Configuration


802.1x with WPA 3 [done]



Additional SSID profiles

802.1X Auth with FortiAuthenticator.

Configure FAC as Radius Server -> 

Create User 1 will AUTH with returned vlan 150
Create User 2 will Auth with returned vlan 160

Configure on FAC FortiGates as Radius Client

Test User Auth from FortiGate to ensure connectivity.

Create SSID "PodNumber"8021x Auth this will use FAC for Radius

Under this VLAN create two VLANs 150 and 160

172.30.150.x/24
172.30.160.x/24

MPSK 

Create new MPSK Profile

Devices-WPA2
Add new MPSK Group List.

Create a New PSK 

Name : Laptops
VLAN Type : Fixed VLAN
VLAN ID : 160

In MPSK List click Add 

Create Key

Enter the Name as Laptops

Enter the Pre shared Key as "WirelessLab1"

Set the Client Limit Type to Unlimited

Set MPSK Provile to Always

![alt text](media/Lab5-7-Skeleton-3.png)

Click OK

Create a New PSK 

Name : BYOD
VLAN Type : Fixed VLAN
VLAN ID : 170

In MPSK List click Add 

Create Key

Enter the Name as Laptops

Enter the Pre shared Key as "WirelessLab2"

Set the Client Limit Type to Unlimited

Set MPSK Provile to Always

Click OK

![alt text](media/Lab5-7-Skeleton-4.png)

Click OK

Right Click on Devices-WPA2 and select Clone

Rename this Profile to Devices - WPA3

Set the type to WPA3 SAE Transition

Edit the Laptops PSK Entry

Set type to WPA3 SAE

Enter the fixed mac address of your laptop here. Note WPA3 doesnt allow for access without defining the mac address.

set the SAE Password to "WirlessLab1"

Click OK

Click Ok again to exit the Pre Shared Key Entry Table

Click on BYOD 

Select Edit
Click on the BYOD Entry 
Edit on the BYOD Entry on the MPSK List

Change the type to WPA3 SAE transition

Set Type to WPA3

Set the SAE Password to "WirelessLab2"

Enter the fixed MAC address of your phone or other device.

Click OK

Change the type of the MPSK Profile from WPA3 SAE-Transition to WPA3-SAE



Create NEW SSID
Name "PodNumber"-Devices-WPA2
SSID "PodNumber"-Devices
Set Beacon Advertising to Name
Set Security Mode to WPA2 Personal
Set Mode to Multiple
Select Devices-WPA2
Ensure Dynamic VLAN Assignment is Toggled On
Click Ok


Create NEW SSID
Name "PodNumber"-Devices-WPA3
SSID "PodNumber"-Devices
Set Beacon Advertising to Name
Set Security Mode to WPA3 SAE
Set Mode to Multiple
Select Devices-WPA3
Ensure Dynamic VLAN Assignment is Toggled On
Click Ok

