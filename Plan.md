Reconfiguire Pods
1. Check Vlan IDs match pod ID
2. Under Fabricstudio Terminal
 1. system interfaces mgmt configure
 2. set address 172.31.1xx.1 255.255.255.0 
 3. set gateway 172.31.1xx.254
 4. end

## TODO
1. Check default gateway on ns01, seems to be broken ( sudo ip route replace default via 172.30.100.254 dev ens4 , verify ip r / timedatectl status )
2. Change template for branch to listen NTP on fortilink ( is broken, server dissabled need to enable and add fortilink )

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

| Name               | Network         | Gateway        | Vlan ID |
| ------------------ | --------------- | -------------- | ------- |
| Services Vlan      | 172.30.100.0/24 | 172.30.100.254 |         |
| Pod Vlan           | 172.31.1xx.0/24 | 172.31.1xx.254 | 1XX     |
| Wireless MGMT Vlan | 172.30.200.0/24 | 172.30.200.254 |         |
| FortiLink          | 10.255.1.0/24 | 10.255.1.1 |         |
| User Vlan          | 172.30.210.0/24 | 172.30.210.254 |         |
| Corp SSID          | 172.30.220.0/24 | 172.30.220.254 |         |
| Guest SSID         | 172.30.230.0/24 | 172.30.230.254 |         |
| MPSK SSID          | 172.30.240.0/24 | 172.30.240.254 |         |
| ServicesLB         | 172.30.250.1/32 | xxx            |         |

Lab Tasks 

1. FabricStudio Tasks - Tim
	- Check your Pod IP's [done]
	- Check your Fortigate IP's [done]
	- Check Fortigate Default Route [done]

2. Connecting Physical Fortigate - Tim
	- Connect to fortimanager [done]
	- Configure Branch based on Template [done]
	- Add metavariables for pod_id and wan_interface [done]
	- Powerup Fortigate and connect [done]
	- Watch the magic [done]

3. Authenticator - Tim
	- Create Root Cert **[pre-created]** [done]
	- Create Intermediate signed by root **[pre-created]** [done]
	- Create star.certificate gui **[pre-created]** [done]
	- Create Certificate for Wireless
	- Create CA cert for SSL Decryption
	- Possibly create device certs?

4. Basic FortiDevices Setup - Tim
	- Setup Fortilink [done]
	- Create APMgmt Vlan [done]
	- Create User Vlan [done]
	- Configure switch to have AP Mgmt Port and User Vlan [done]
	- Configure AP Operational Profile - Derek to decide ( likely 231K ) [done]
		- Disable 2.4 [done]
	- Setup NTP and DNS [done]
		- Setup DNS entries [done]
		- Setup Local DNS server [done]
	- Add AP's and Switches  [done]
	- Authorize AP and switch [done]

5. WIDS Labs - Derek

6. AIOps setup - Derek

7. SSID Setups - Derek
	- Corporate SSID 
	- Guest SSID 
	- MPSK SSID

8. Assigning certificates - Tim ( Might do without need )

9.  Fortigate + AIOPs Lab - Derek

10. Event Log Forwarder - Derek

11. SDWAN Configuration - Tim [Done]

12. Firewall Policies - Tim
	- SSL Decryption ( Corporate )

13. Testing and connecting - Derek / Tim

14. Dashboards and Troubleshooting - Derek

15. Data Retention - Derek

16. AI-DARRP - Derek
