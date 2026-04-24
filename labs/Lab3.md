# Lab 3 - Provisioning AP

| Device       | Username/PW        |
| ------------ | ------------------ |
| FabricStudio | admin/fortinet1!   |
| FortiManager | admin/fortinet4A!! |

### Creating ARRP Profile
1. Navagating to AP Manager > Managed FortiAPs > Operational Profiles > FortiAP Profiles
![](media/Lab3-1.png)
2. Create a new ARRP Profile with the following settings:
   - Name : aarp-lowdensity-highinterference
   - Weight Managed AP : 100
   - Weight Rogue AP : 30
   - Weight Weather Channel : 0
   - Weight DFS Channel : 0
   - Threshold Channel Load : 45
   - Leave Rest Default
![](media/Lab3-6.png)
### Creating Operation Profile
1. Navagate to Operation Profiles: FortiAP Profiles
![](media/Lab3-5.png)
2. Create a new Profile
   - Name : FAP-241K-SELab
   - Platform : FortiAP-241k
   - Region : Canada
   - AP Login Password : Set to "fortinet"
   - Administrative Access : enable https and ssh
   - Radio 1 : Disabled ( 2.4Ghz from every pod = bad)
   - Radio 2 :
     - Radio Resouce Provision : Toggle On
     - ARRP Profile : arrp-default
     - Bands : 802.11 be/ax/ac/n/a
     - Short Guard Interval : Toggle On
     - Transmit Power: Toggle dBm and set to 10 dBm
     - SSID : Tunneled
     - Advanced Options 
    - Airtime-Fairness : Toggle On
   - Radio 3 :
     - Radio Resource Provision : Toggle On
     - ARRP Profile : arrp-default
     - Bands : 802.11 be/ax
     - Channel Width : 80MHz
     - Short Guard Interval : Toggle On
     - Transmit Power: Toggle Percent : 100%
     - SSID : Tunneled
     - Advanced Options 
     - Airtime-Fairness : Toggle On
   - Radio 4:
     - Mode : Dedicated to Monitor
     - WIDS Profile : default
   - Leave everything else as Default and click OK
>[!Note]
>As you haven't added the AP you you'll need to toggle "View All Profiles" to make it visable

>[!warning]
>- Double check everything looks right, it seems a 50% that the R3 Profile dosne't save. You just need to go back in and toggle it to access point instead of dedicated monitor
>##### Good
>![](media/Lab3-3.png)
>
>##### Bad
>![](media/Lab3-7.png)



### Adding a Model AP
3. Navagate to AP Manager > Managed FortiAPs
4. Create New Model AP
   - Fortigate : FGTBr01
   - Serial Number : FP241K****000000
   - Name: AP-241K-01
   - FortiAP Profile: FAP-241K-SELab
>[!caution]
>If you're not using a FP241K replace the first 6 of the SN with the first 6 of the actual Switch SN you have
 - Click OK

### Policy for NTP / DNS
>[!note]
>Lastly before adding the AP we need to make sure NTP and DNS work correctly. We could enable those both on the fortigate, but instead we'll create a policy to allow the AP's to reach out to our NS01

5. Navagate to Policy & Objects > FGTBranch Firewall Policy and Create New
   - Name : APServices -> NS01
   - Action : Accept
   - Incoming Interface : AP Mgmt
   - Outgoing Interface : overlay
   - Source : **Create New Address**
     - Name : AP Mgmt Subnet
     - IP: 172.30.200.0/24
     - Click Ok, Add change note, and Add as a source
    - Destination : Select DC Services
    - Service : DNS and NTP
   - Leave the rest Default, Click OK, fill in change note

![](media/Lab3-4.png)

6. Install changes and connect your AP to Port1
    - You can either wait for it to finish or continue on this will take around 5 - 10 minutes to autolink reboot and finish

![](media/Lab3-2.png)

#### Lab complete move onto Lab 4


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

