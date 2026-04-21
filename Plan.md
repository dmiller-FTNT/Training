
domain = fortiacme.net

| Device             | IP             | DNS           | NTP                | Hostname           |
| ------------------ | -------------- | ------------- | ------------------ | ------------------ |
| FortiGate          | 172.30.100.254 | 172.30.100.60 | 172.30.100.60      | fgtdc01            |
| FortiAIOps         | 172.30.100.25  | 172.30.100.60 | 172.30.100.60      | fortiaiops         |
| FortiManager       | 172.30.100.11  | 172.30.100.60 | ns01.fortiacme.net | fortimanager       |
| FortiAuthenticator | 172.30.100.10  | 172.30.100.60 | ns01.fortiacme.net | fortiauthenticator |
| FortiAnalyzer      | 172.30.100.12  | 172.30.100.60 | ns01.fortiacme.net | fortianalyzer      |
| DNS/NTP            | 172.30.100.60  | 1.1.1.1       | pools              | ns01               |

Lab Tasks 

1. FabricStudio Tasks - Tim
	- Check your Pod IP's
	- Check your Fortigate IP
	- Check Fortigate Default route

2. Connecting Physical Fortigate - Tim

3. Authenticator - Tim
	- Create Root Cert **[pre-created]**
	- Create Intermediate signed by root **[pre-created]**
	- Create star.certificate gui **[pre-created]**
	- Create Certificate for Wireless
	- Create CA cert for SSL Decryption
	- Possibly create device certs?

4. Basic FortiDevices Setup - Tim
	- Setup Fortilink
	- Create APMgmt Vlan
	- Create User Vlan
	- Configure switch to have AP Mgmt Port and User Vlan
	- Configure AP Operational Profile - Derek to decide ( likely 231K )
		- Disable 2.4
	- Setup NTP and DNS
		- Setup DNS entries
		- Setup Local DNS server
	- Add AP's and Switches 
	- Authorize AP and switch

5. WIDS Labs - Derek

6. AIOps setup - Derek

7. SSID Setups - Derek
	- Corporate SSID 
	- Guest SSID 
	- MPSK SSID

8. Assigning certificates - Tim ( Might do without need )

9. Fortigate + AIOPs Lab - Derek

10. Event Log Forwarder - Derek

11. SDWAN Configuration - Tim

12. Firewall Policies - Tim
	- SSL Decryption ( Corporate )

13. Testing and connecting - Derek / Tim

14. Dashboards and Troubleshooting - Derek

15. Data Retention - Derek

16. AI-DARRP - Derek
