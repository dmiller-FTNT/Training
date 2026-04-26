# Wireless for your friends
#### Setting up captive guest portal

### Authenticator for Guests
1. Navagate to Authentication > User Management > User Groups and select Create New
   - Name : Guests
   - Type : Local
   - Radius Attributes : Add one
     - Vendor Fortinet
     - Attribute ID : Fortinet-Group-Name
     - Value Type : Static
     - Value : Guest
   - Click Save
2. Navagate to Authentication > Portals > Portals and select Create New.
   - Name : Guest-Portal
   - User Account Self-Registration : Enabled
   - Account Expires after : 3 Hour
   - Place Registered users into a group : Select Guests
   - Password Creation : User-Defined
   - Account delivery options ... : Display on browser page 
   - Leave the rest default and click Save
3. Navagate to Authentication > Portals > Access Points and select Create New.
   - Name : All
   - Client address : Range > 0.0.0.0-255.255.255.255
   - Click Save
4. Navagate to Authentication > Portals > Policies, click Captive Portal at the top and Create New
   - Policy type
     - Name : Guest-Policy
     - Poral : Guest-Portal
     - Click Next
   - Portal selection criteria
     - HTTP Parameter : ssid
     - Operator : [string]exact_match
     - Value : Guest
     - Click Next
   - Authorized clients
     - Move All to Chosen Access Points
     - Move FGTBr01 Over to Chosen Radius Clients
  ![](media/Lab7-1.png)
   - Click Next
   - Authentication type
     - Leave this default
     - Click Next 
   - Identity sources
     - Leave this default
     - Click  Next 
   - Authentication factors
     - Authentication Methods: Password-only
     - Next
   - RADIUS response
     - Click  Save and Exit

5. Logging into your Fortimanager Navagate to Policy & Objects > User & Authentication > User Groups and Create New
   - Name : Guest
   - Remote Authentication Servers : Create New
     - Remote Server : FortiAuthenticator
     - Group Name : Guest
   - Click OK

6. Setup SSID