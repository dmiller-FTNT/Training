
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

2. Authenticator - Tim
	- Create Root Cert
	- Create Intermediate signed by root
	- Create Certificate for Wireless
	- Create CA cert for SSL Decryption
	- Possibly create device certs?

3. Basic FortiDevices Setup - Tim
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

4. WIDS Labs - Derek

5. AIOps setup - Derek

6. SSID Setups - Derek
	- Corporate SSID 
	- Guest SSID 
	- MPSK SSID

7. Assigning certificates - Tim ( Might do without need )

8. Fortigate + AIOPs Lab - Derek

9. Event Log Forwarder - Derek

10. SDWAN Configuration - Tim

11. Firewall Policies - Tim
	- SSL Decryption ( Corporate )

12. Testing and connecting - Derek / Tim

13. Dashboards and Troubleshooting - Derek

14. Data Retention - Derek

15. AI-DARRP - Derek
