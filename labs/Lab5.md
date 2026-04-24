SSID Configuration



Additional SSID profiles

Prework the 802.1x AUTH SSID will operate with Bridged SSID.

the MPSK SSID will be in tunnel mode this is to show the concepts of the two approaches.

Create two more use FSW vlans and ensure they are allowed on the FortiSwitch VLANs 140 and VLANS 150
Dont forget to create a policy that can access the internet.



802.1X Auth with FortiAuthenticator.

Configure FAC as Radius Server -> FAC

Create User 1 will AUTH with returned vlan 140
Create User 2 will Auth with returned vlan 150

config wireless-controller vap
    edit 8021xSecureWPA3
        set ssid PodNumber-8021x
        set security wpa3-only-enterprise
        set pmf enable
        set 80211v disable
        set fast-bss-transition enable
        set ft-mobility-domain 1200
        set auth radius
        set radius-server FAC
        set local-bridging enable
        set schedule always
        set vlanid 20
        set dynamic-vlan enable
        set multicast-rate 24000
        set mu-mimo disable 
        set broadcast-suppression dhcp-up dhcp-starvation dhcp-ucast arp-known arp-poison netbios-ns netbios-ds
        set radio-sensitivity enable
        set radio-5g-threshold "-65"
        set radio-2g-threshold "-70"
        set rates-11a 24-basic 36 48-basic 54
        set rates-11bg 24-basic 36 48-basic 54
        set rates-11n-ss12 mcs3/1 mcs4/1 mcs5/1 mcs6/1 mcs7/1 mcs11/2 mcs12/2 mcs13/2 mcs14/2 mcs15/2
        set rates-11n-ss34 mcs19/3 mcs20/3 mcs21/3 mcs22/3 mcs23/3 mcs27/4 mcs28/4 mcs29/4 mcs30/4 mcs31/4
        set beacon-advertising name
    next

    config wireless-controller vap
    edit 8021xSecureWPA2
        set ssid PodNumber-8021x
        set security wpa2-only-enterprise
        set fast-bss-transition enable
        set ft-mobility-domain 1201
        set auth radius
        set radius-server FAC
        set local-bridging enable
        set schedule "always"
        set vlanid 20
        set dynamic-vlan enable
        set multicast-rate 24000
        set mu-mimo disable 
        set radio-sensitivity enable
        set radio-5g-threshold "-65"
        set radio-2g-threshold "-70"
        set rates-11a 24-basic 36 48-basic 54
        set rates-11bg 24-basic 36 48-basic 54
        set rates-11n-ss12 mcs3/1 mcs4/1 mcs5/1 mcs6/1 mcs7/1 mcs11/2 mcs12/2 mcs13/2 mcs14/2 mcs15/2
        set rates-11n-ss34 mcs19/3 mcs20/3 mcs21/3 mcs22/3 mcs23/3 mcs27/4 mcs28/4 mcs29/4 mcs30/4 mcs31/4
        set beacon-advertising name
    next



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

![alt text](image-3.png)

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

![alt text](image-4.png)

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

Create VLANS under the Device SSIDs (Note although they are on the same VLAN ID they are seperate L2)
Create a policy to allow each of the corresponding VLANS to communicate with each other along with internet.
VLANS 160 and VLAN 170





