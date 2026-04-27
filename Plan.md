Reconfiguire Pods
1. Check Vlan IDs match pod ID
2. Under Fabricstudio Terminal
 1. system interfaces mgmt configure
 2. set address 172.31.1xx.1 255.255.255.0 
 3. set gateway 172.31.1xx.254
 4. end



domain = fortiacme.net

| Device             | IP             | DNS           | NTP                | Hostname           |
| ------------------ | -------------- | ------------- | ------------------ | ------------------ |
| FortiGate          | 172.30.100.254 | 172.30.100.60 | 172.30.100.60      | fgtdc01            |
| FortiGate          | 172.31.1xx.200 | 172.30.100.60 | 172.30.100.60      | fgtbr01            |
| FortiAIOps         | 172.30.100.25  | 172.30.100.60 | 172.30.100.60      | fortiaiops         |
| FortiManager       | 172.30.100.11  | 172.30.100.60 | ns01.fortiacme.net | fortimanager       |
| FortiAuthenticator | 172.30.100.10  | 172.30.100.60 | ns01.fortiacme.net | fortiauthenticator |
| FortiAnalyzer      | 172.30.100.12  | 172.30.100.60 | ns01.fortiacme.net | fortianalyzer      |
| DNS/NTP            | 172.30.100.60  | 1.1.1.1       | pools              | ns01               |

| Name            | Network         | Gateway        | Vlan ID | pre-created Address? | pre-Normalized Interface? |
| --------------- | --------------- | -------------- | ------- | -------------------- | ------------------------- |
| Services Vlan   | 172.30.100.0/24 | 172.30.100.254 |         | y                    | y (technicall port 3)     |
| Pod Vlan        | 172.31.1xx.0/24 | 172.31.1xx.254 | 1XX     |                      |                           |
| AP Mgmt         | 172.30.200.0/24 | 172.30.200.254 | 200     | n                    | n                         |
| FortiLink       | 10.255.1.0/24   | 10.255.1.1     |         |                      | y                         |
| User1 Vlan      | 172.30.150.0/24 | 172.30.150.254 | 150     | y                    | n                         |
| User2 Vlan      | 172.30.160.0/24 | 172.30.160.254 | 160     | y                    | n                         |
| User Access     | 172.30.210.0/24 | 172.30.210.254 | 210     | n                    | n                         |
| Wireless Guest  | 172.30.220.0/24 | 172.30.220.254 |         | y                    | n                         |
| ~~WPA2 Native~~ | 172.30.230.0/24 | 172.30.230.254 |         |                      |                           |
| WPA2 Laptop     | 172.30.161.0/24 | 172.30.161.254 | 161     | y                    | y                         |
| WPA2 BYOD       | 172.30.171.0/24 | 172.30.171.254 | 171     | y                    | y                         |
| ~~WPA3 Native~~ | 172.30.240.0/24 | 172.30.240.254 |         |                      |                           |
| WPA3 Laptop     | 172.30.162.0/24 | 172.30.162.254 | 162     | y                    | y                         |
| WPA3 BYOD       | 172.30.172.0/24 | 172.30.172.254 | 172     | y                    | y                         |
| ServicesLB      | 172.30.250.1/32 | xxx            |         | y                    | y                         |

Tunnel SSID VLANS
Devices-WPA2

Lab Tasks 

1. FabricStudio Tasks - Tim
	- Check your Pod IP's [done]
	- Check your Fortigate IP's [done]
	- Check Fortigate Default Route [done]
	- Check Fortimanager Pod ID [done]

2. Connecting Physical Fortigate - Tim
	- ~~Connect to fortimanager [done]~~
	- ~~Configure Branch based on Template [done]~~
	- ~~Add metavariables for pod_id and wan_interface [done]~~
	- ~~Powerup Fortigate and connect [done]~~
	- ~~Watch the magic [done]~~

3. Authenticator - Tim
	- Create Root Cert **[pre-created]** [done]
	- Create Intermediate signed by root **[pre-created]** [done]
	- Create star.certificate gui **[pre-created]** [done]
	- Create Certificate for Wireless
	- Create CA cert for SSL Decryption
	- Possibly create device certs?

4. Basic FortiDevices Setup - Tim
	- ~~Setup Fortilink [done]~~
	- ~~Create APMgmt Vlan [done]~~
	- ~~Create User Vlan [done]~~
	- ~~Configure switch to have AP Mgmt Port and User Vlan [done]~~
	- ~~Configure AP Operational Profile - Derek to decide ( likely 231K ) [done]~~
		- ~~Disable 2.4 [done]~~
	- ~~Setup NTP and DNS [done]~~
		- ~~Setup DNS entries [done]~~
		- ~~Setup Local DNS server [done]~~
	- ~~Add AP's and Switches  [done]~~
	- ~~Authorize AP and switch [done]~~

5. WIDS Labs - Derek

6. AIOps setup - Derek

7. SSID Setups - Derek - NEEDs Smoothing out
	- Corporate SSID 
	
	- MPSK SSID
	- Guest SSID 

8. Assigning certificates - Tim ( Might do without need )

9.  Fortigate + AIOPs Lab - Derek (In Lab4)

10. Event Log Forwarder - Derek (In lab 4)

11. ~~SDWAN Configuration - Tim [Done]~~

12. Firewall Policies - Tim
	- SSL Decryption ( Corporate ) 

13. Testing and connecting - Derek / Tim (Lab 5+6+7+8)

14. Dashboards and Troubleshooting - Derek 

15. Data Retention - Derek

16. AI-DARRP - Derek
