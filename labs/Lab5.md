# Lab 5 - Bring on the Radiation 802.1x Style
#### Setting up 802.1x SSID and support
| Device             | Username/PW        |
| ------------------ | ------------------ |
| FabricStudio       | admin/fortinet1!   |
| FortiManager       | admin/fortinet4A!! |
| Fortiauthenticator | admin/fortinet4A!! |



#### Preparing 802.1x Authentication
1. Connect to FortiAuthenticator and Login

2. Navagate to Certificate Managment > End Entities > Local Services and Create New
   - Certificate ID: Radius_Service
   - Certificate Authority: int_fortiacme_net
   - issuer : Local CA
   - Name (CN) : radius.fortiacme.net
   - Subject Alternate Name > DNS : radius.fortiacme.net
   - Click Save
3. Navagate to Autheticaton > Radius Service > General
   - Assign your Radius.Service to both EAP Server and RADSEC Server Certificates, And change Local CA's to the FortiAcme Intermediate
![](media/Lab5-1.png)
   - Click Ok

1. Navagate to Authenticaton > Radius Service > Clients and Create New
   - Name : FGTBr01
   - IP/FQDN : 172.30.250.1
   - Secret : fortinet4A!!
   - Leave the rest default
![](media/Lab5-2.png)
   - Click Save
1. Navagate to Authenticaton > Radius Service > Policies and Create New
    1. Radius Clients
       - Policy Name: Wireless8021x
       - Move FGTBR01 over to Chosen Clients
       - Next
    2. Radius Criteria 
         - Next
    3. Authenticaton type
       - password/OTP Authentication
       - Accept EAP > Accept PEAP and EAP-TTLS
       - next
    4. Identity sources 
         - Next
    5. Authentication Factors
        - Password only
        - next
    6. Radius Respose 
         - Save and Exit

2.  Navagate to User Managment > Local Users and Create two users
    1. User 1
       - Username : user1
       - Password and confirm : fortinet4A!! 
       - Allow Radius Authentication
       - save and save
    2. User 2
       - Username : user2
       - Password and confirm : fortinet4A!! 
       - Allow Radius Authentication
       - save and save

3. Return back into **both** users and set the following radius attributes
    1. Radius Attrubute 1
       - Vendor : Default
       - Attribute ID : Tunnel-Type
       - Value : Vlan
    2. Radius Attrubute 2
       - Vendor : Default
       - Attribute ID : Tunnel-Medium-Type
       - Value : IEEE-802
    3. Radius Attrubute 3
       - Vendor : Default
       - Attribute ID : Tunnel-Private-Group-Id
       - Value : 150 for user1 / 160 for user2
![](media/Lab5-3.png)


#### Create SSID and Vlans
9. Logging back into Fortimanager Navagate to Policy & Objects > User & Authentication > Radius Servers and Create New
10. Configure the Server as follows
    - Name : FortiAuthenticator
    - Primary Server Name/IP : 172.30.100.10
    - Primary Server Secret : fortinet4A!! 
    - Create new Per-Device Mapping
      - Mapped Device : FGTBr01
      - Leave everything the same and in advanced options
        - Source IP : 172.30.250.1
    - ok and ok add change notes

11. Navagate to AP Manager > SSIDs and create new
    - Name : 8021x Auth
    - Traffic Mode : Bridge
    - SSID: xx-8021x (xx is your pod number)
    - Security Mode : WPA3 Enterprise
    - Authentication : Radius Server - Select FortiAuthenticator
    - Dynamic Vlan Assignment : Enable
    - Schedule : Always

12. Navagate to FortiSwitch Manager > Vlan and create two new Vlans
    1.  User1 Vlan
        - Interface Name : User1 Vlan
        - Vlan ID : 150
        - IP/Netmask : 172.30.150.254/255.255.255.0
        - DHCP Server : Enable
        - Address Range : 172.30.150.2 - 172.30.150.250
        - Netmask : Sepcify : 255.255.255.0
        - DNS Server : Same as System DNS
        - Advanced
          - NTP Server : Same as System NTP
        - Map to Normalized Interface > Create New (+)
          - Name : User1 Vlan
          - Per-Platform Mapping > Create New
            - Matched Platform : all
            - Mapped Interface : User1 Vlan
        - ok, ok
    2.  User2 Vlan
        - Interface Name : User2 Vlan
        - Vlan ID : 160
        - IP/Netmask : 172.30.160.254/255.255.255.0
        - DHCP Server : Enable
        - Address Range : 172.30.160.2 - 172.30.160.250
        - Netmask : Sepcify : 255.255.255.0
        - DNS Server : Same as System DNS
        - Advanced
          - NTP Server : Same as System NTP
        - Map to Normalized Interface > Create New (+)
          - Name : User2 Vlan
          - Per-Platform Mapping > Create New
            - Matched Platform : all
            - Mapped Interface : User2 Vlan
        - ok, ok

13. Navagate to Policy & Objects > Policy Packages > FGTBranch and Create two new policies
    1. DNS
       - Name : 8021x Vlans -> DNS n NTP
       - Action : Accept
       - Incomming Interface : User1 Vlan and User2 Vlan
       - Outgoing Interface : Overlay
       - Source : User1 Vlan and User2 Vlan
       - Destination : NS01
       - Service : DNS and NTP
       - Ok
    2. Clone 8021x Vlans -> DNS and change
       - Name : 8021x Vlans -> Internet
       - Action : Accept
       - Incomming Interface : User1 Vlan and User2 Vlan
       - Outgoing Interface : Underlay
       - Source : User1 Vlan and User2 Vlan
       - Destination : All
       - Service : All
       - NAT : Enabled
       - Ok

14. Lastly Navagate AP Manager > Operation Profiles > FortiAP Profiles and Edit your Profile Already Created
    - Change SSID's on Raidio 2 and 3 to Manual > add 8021x Auth
    - OK
![](media/Lab5-4.png)

1.  Do a full install ( policy and Device ) to FGTBr01

#### Testing
16. Connect to your SSID, use either username or password and provide any certificate ( it doesn't check ). It might require some accepting of the certs user if you didn't add your Root and Intermediate.

#### Lab complete move onto Lab 6


