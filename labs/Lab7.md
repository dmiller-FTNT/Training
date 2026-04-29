#### Lab 7 - Wireless for Your Friends: Captive Guest Portal

| Device             | Username/PW        |
| ------------------ | ------------------ |
| FabricStudio       | admin/fortinet1!   |
| FortiManager       | admin/fortinet4A!! |
| FortiAuthenticator | admin/fortinet4A!! |

> [!NOTE]
> **Why guest wireless access needs to be done properly**
>
> Offering Wi-Fi to visitors, contractors, and customers is expected in almost every modern workplace — but simply handing out the corporate Wi-Fi password creates serious problems. A guest on the corporate network can potentially reach internal resources, spread malware, or be used as a pivot point for an attack. Regulatory frameworks like PCI-DSS and HIPAA explicitly require that guest traffic be isolated from systems in scope.
>
> A properly designed guest portal solves this with three things: **isolation**, **accountability**, and **control**.
>
> **Isolation** — Guest traffic lands on a dedicated SSID and VLAN with firewall policies that permit only internet access. No guest device can reach the corporate LAN, servers, printers, or other endpoints.
>
> **Accountability** — Rather than open access, users register with an email address or accept terms of service. This creates an audit trail: you know who was on your network, when, and for how long. This is increasingly important for legal and compliance reasons, and in some jurisdictions is a legal requirement for public Wi-Fi operators.
>
> **Control** — Accounts can be time-limited (3 hours in this lab), rate-limited, and quarantined if suspicious behaviour is detected. A single administrator action can revoke a guest's access immediately.
>
> **Self-registration vs. pre-provisioned accounts:**
> - **Self-registration** (used here) lets guests create their own accounts on arrival. It scales effortlessly — no IT involvement required for each guest — and works well for general visitor access.
> - **Pre-provisioned accounts** are better for contractors or events where you want tighter control over who can register. An admin creates accounts in advance and distributes credentials directly.
>
> **How the portal flow works:**
> A guest connects to the open SSID and is placed in a restricted zone where only DNS and HTTPS to FortiAuthenticator are permitted. Any web request is intercepted and redirected to the captive portal. After registering and accepting terms, the FortiGate receives confirmation from FortiAuthenticator via RADIUS and opens internet access for that session — with the guest placed into the Guest user group, which the firewall policies use to permit outbound traffic.

##### Configuring FortiAuthenticator for Guests

1. Navigate to Authentication > User Management > User Groups and click Create New
   - Name: Guests
   - Type: Local
   - Radius Attributes > Add one
     - Vendor: Fortinet
     - Attribute ID: Fortinet-Group-Name
     - Value Type: Static
     - Value: Guest
   - Click Save

> [!TIP]
> The Fortinet-Group-Name RADIUS attribute is how FortiAuthenticator tells the FortiGate which group to place the authenticated user into. The FortiGate uses this group membership to match the guest firewall policies you will create later. This is the same dynamic group assignment mechanism used in 802.1x — the identity system drives the policy, not the network device.

2. Navigate to Authentication > Portals > Portals and click Create New
   - Name: Guest-Portal
   - User Account Self-Registration: Enabled
   - Account Expires After: 3 Hours
   - Place Registered Users into a Group: Guests
   - Password Creation: User-Defined
   - Account Delivery Options: Display on Browser Page
   - Leave the rest default and click Save

3. Navigate to Authentication > Portals > Access Points and click Create New
   - Name: All
   - Client Address: Range > 0.0.0.0 - 255.255.255.255
   - Click Save

4. Navigate to System > Network > Interfaces > Edit Port2
   - Under Services > HTTPS, enable Captive Portals

  ![alt text](media/Lab7-4.png)

   - Click Save and log back in

5. Navigate to Authentication > Portals > Policies, click Captive Portal at the top, and click Create New
   - Policy Type
     - Name: Guest-Policy
     - Portal: Guest-Portal
     - Click Next
   - Portal Selection Criteria
     - HTTP Parameter: ssid
     - Operator: [string] exact_match
     - Value: xx-GuestWireless (xx is your pod number)
     - Click Next
   - Authorized Clients
     - Move All to Chosen Access Points
     - Move FGTBr01 to Chosen Radius Clients

  ![alt text](media/Lab7-1.png)

     - Click Next
   - Authentication Type
     - Leave default and click Next
   - Identity Sources
     - Leave default and click Next
   - Authentication Factors
     - Authentication Methods: Password-only
     - Click Next
   - RADIUS Response
     - Click Save and Exit

> [!TIP]
> **Matching the portal to the SSID by name** is a powerful feature — it means you can run multiple portals from a single FortiAuthenticator instance, each with different branding, expiry times, and policies, each triggered by whichever SSID the client connected to. A conference SSID could show a branded event portal with a 1-day expiry, while a standard visitor SSID shows a corporate portal with a 4-hour expiry — all from the same infrastructure.

##### Configuring FortiManager

6. In FortiManager, navigate to Policy & Objects > User & Authentication > User Groups and click Create New
   - Name: Guest
   - Remote Authentication Servers > Create New
     - Remote Server: FortiAuthenticator
     - Group Name: Guest
   - Click OK

7. Navigate to AP Manager > Managed FortiAPs > SSIDs and click Create New
   - Name: Wireless Guest
   - Mode: Tunnel
   - IP/Network Mask: 172.30.220.0/24
   - DHCP Server: Enabled
   - Address Range: 172.30.220.2 - 172.30.220.250
   - Netmask: Specify > 255.255.255.0
   - DNS Server: Same as System DNS
   - SSID: xx-GuestWireless (xx is your pod number)
   - Security Mode: WPA3 SAE / WPA2 Personal / Open (your choice)
   - Captive Portal: Enabled
   - Portal Type: Authentication
   - Authentication Portal: https://fortiauthenticator.fortiacme.net/portal/
   - User Groups: Guest (make sure it is the RADIUS one)
   - Exempt Destinations: FortiAuthenticator and NS01
   - Redirect After Captive Portal: Specific URL > https://www.fortinet.com/
   - Schedule: Always
   - Quarantine Host: Enabled

  ![alt text](media/Lab7-2.png)

  ![alt text](media/Lab7-3.png)

> [!NOTE]
> **Quarantine Host** enables the FortiGate to automatically isolate a guest device if it is flagged by threat detection — blocking it from all network access without requiring manual intervention. This is particularly valuable in a guest environment where you have no control over the security posture of connected devices.

8. Navigate to Policy & Objects > Policy Packages > FGTBranch and create two firewall policies
   1. Guest Services Policy
      - Name: GuestWireless -> Services
      - Action: Accept
      - Incoming Interface: Guest Wireless
      - Outgoing Interface: Overlay
      - Source: Wireless Guest
      - Destination: FortiAuthenticator, NS01
      - Services: DNS, HTTPS
      - Click OK and add a change note
   2. Guest Internet Policy
      - Name: GuestWireless -> Internet
      - Action: Accept
      - Incoming Interface: Guest Wireless
      - Outgoing Interface: Underlay
      - Source: Wireless Guest
      - User/Group: Guest (the RADIUS one)
      - Destination: All
      - Services: All
      - NAT: Enabled
      - Click OK and add a change note

9. Navigate to Device Manager > Provisioning Templates > Certificate (may be hidden under ...) and open Certificate Operations
   - Select FGTBr01 and click OK
   - Wait for the certificate to be retrieved via SCEP

10. Navigate to Device & Group > FGTBr01 > CLI Configurations > Firewall > Auth-Portal
    - Portal-addr: portal.fortiacme.net
    - Click OK

11. Navigate to Device & Group > FGTBr01 > CLI Configurations > User > Settings
    - Auth-cert: portal
    - Auth-type: HTTP and HTTPS
    - Click OK

12. Add the Guest Wireless SSID to your AP Manager profile

#### Lab complete — move on to Lab 8
