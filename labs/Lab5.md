#### Lab 5 - Bring on the Radiation: 802.1x Style

| Device             | Username/PW        |
| ------------------ | ------------------ |
| FabricStudio       | admin/fortinet1!   |
| FortiManager       | admin/fortinet4A!! |
| FortiAuthenticator | admin/fortinet4A!! |

### Preparing 802.1x Authentication

1. Connect to FortiAuthenticator and log in

2. Navigate to Certificate Management > End Entities > Local Services and click Create New
   - Certificate ID: Radius_Service
   - Certificate Authority: int_fortiacme_net
   - Issuer: Local CA
   - Name (CN): radius.fortiacme.net
   - Subject Alternate Name > DNS: radius.fortiacme.net
   - Click Save

3. Navigate to Authentication > Radius Service > General
   - Assign your Radius_Service certificate to both EAP Server and RADSEC Server Certificates
   - Change Local CAs to the FortiAcme Intermediate

  ![alt text](media/Lab5-1.png)

   - Click OK

4. Navigate to Authentication > Radius Service > Clients and click Create New
   - Name: FGTBr01
   - IP/FQDN: 172.30.250.1
   - Secret: fortinet4A!!
   - Leave the rest default

  ![alt text](media/Lab5-2.png)

   - Click Save

5. Navigate to Authentication > Radius Service > Policies and click Create New
   1. Radius Clients
      - Policy Name: Wireless8021x
      - Move FGTBr01 over to Chosen Clients
      - Click Next
   2. Radius Criteria
      - Click Next
   3. Authentication Type
      - Password/OTP Authentication
      - Accept EAP > Accept PEAP and EAP-TTLS
      - Click Next
   4. Identity Sources
      - Click Next
   5. Authentication Factors
      - Password only
      - Click Next
   6. Radius Response
      - Click Save and Exit

6. Navigate to User Management > Local Users and create two users
   1. User 1
      - Username: user1
      - Password: fortinet4A!!
      - Allow Radius Authentication
      - Click Save
   2. User 2
      - Username: user2
      - Password: fortinet4A!!
      - Allow Radius Authentication
      - Click Save

7. Return to **both** users and set the following RADIUS attributes
   1. Radius Attribute 1
      - Vendor: Default
      - Attribute ID: Tunnel-Type
      - Value: VLAN
   2. Radius Attribute 2
      - Vendor: Default
      - Attribute ID: Tunnel-Medium-Type
      - Value: IEEE-802
   3. Radius Attribute 3
      - Vendor: Default
      - Attribute ID: Tunnel-Private-Group-Id
      - Value: 150 for user1 / 160 for user2

  ![alt text](media/Lab5-3.png)

### Creating the SSID and VLANs

8. In FortiManager, navigate to Policy & Objects > User & Authentication > Radius Servers and click Create New
9. Configure the server as follows
   - Name: FortiAuthenticator
   - Primary Server Name/IP: 172.30.100.10
   - Primary Server Secret: fortinet4A!!
   - Create New Per-Device Mapping
     - Mapped Device: FGTBr01
     - Advanced Options > Source IP: 172.30.250.1
   - Click OK and add change notes

10. Navigate to AP Manager > SSIDs and click Create New
    - Name: 8021x Auth
    - Traffic Mode: Bridge
    - SSID: xx-8021x (xx is your pod number)
    - Security Mode: WPA3 Enterprise
    - Authentication: Radius Server - select FortiAuthenticator
    - Dynamic VLAN Assignment: Enable
    - Schedule: Always

11. Navigate to FortiSwitch Manager > VLAN and create two new VLANs
    1. User1 VLAN
       - Interface Name: User1 Vlan
       - VLAN ID: 150
       - IP/Netmask: 172.30.150.254/255.255.255.0
       - DHCP Server: Enable
       - Address Range: 172.30.150.2 - 172.30.150.250
       - Netmask: Specify - 255.255.255.0
       - DNS Server: Same as System DNS
       - Advanced > NTP Server: Same as System NTP
       - Map to Normalized Interface > Create New
         - Name: User1 Vlan
         - Per-Platform Mapping > Create New
           - Matched Platform: All
           - Mapped Interface: User1 Vlan
       - Click OK
    2. User2 VLAN
       - Interface Name: User2 Vlan
       - VLAN ID: 160
       - IP/Netmask: 172.30.160.254/255.255.255.0
       - DHCP Server: Enable
       - Address Range: 172.30.160.2 - 172.30.160.250
       - Netmask: Specify - 255.255.255.0
       - DNS Server: Same as System DNS
       - Advanced > NTP Server: Same as System NTP
       - Map to Normalized Interface > Create New
         - Name: User2 Vlan
         - Per-Platform Mapping > Create New
           - Matched Platform: All
           - Mapped Interface: User2 Vlan
       - Click OK

12. Navigate to Policy & Objects > Policy Packages > FGTBranch and create two new policies
    1. DNS and NTP Policy
       - Name: 8021x Vlans -> DNS n NTP
       - Action: Accept
       - Incoming Interface: User1 Vlan and User2 Vlan
       - Outgoing Interface: Overlay
       - Source: User1 Vlan and User2 Vlan
       - Destination: NS01
       - Service: DNS and NTP
       - Click OK
    2. Clone the policy above and change the following
       - Name: 8021x Vlans -> Internet
       - Outgoing Interface: Underlay
       - Destination: All
       - Service: All
       - NAT: Enabled
       - Click OK

13. Navigate to AP Manager > Operational Profiles > FortiAP Profiles and edit your existing profile
    - Change SSIDs on Radio 2 and Radio 3 to Manual and add 8021x Auth
    - Click OK

  ![alt text](media/Lab5-4.png)

14. Run a full install (Policy and Device) to FGTBr01

### Testing

15. Connect to your SSID using either username and password and accept any certificate prompt
    - Note: If you did not add your Root and Intermediate certificates, you may need to accept additional certificate warnings

#### Lab complete — move on to Lab 6
