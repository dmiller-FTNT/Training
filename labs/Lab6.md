#### Lab 6 - Split Personality with MPSK

| Device             | Username/PW        |
| ------------------ | ------------------ |
| FabricStudio       | admin/fortinet1!   |
| FortiManager       | admin/fortinet4A!! |
| FortiAuthenticator | admin/fortinet4A!! |

> [!NOTE]
> **What is MPSK and why does it matter?**
>
> MPSK (Multiple Pre-Shared Key) allows a single SSID to accept many different pre-shared keys simultaneously, with each key mapped to its own VLAN and group policy. This solves one of the most common pain points in wireless network design: the tradeoff between simplicity and segmentation.
>
> **The problem with traditional PSK:**
> In a standard PSK deployment, every device shares the same key and lands on the same VLAN. To segment device types — corporate laptops, BYOD phones, IoT sensors — you typically need a separate SSID for each group. More SSIDs means more beacon traffic, more RF overhead, and more management complexity. On a congested 2.4 GHz band especially, every extra SSID costs you airtime.
>
> **How MPSK fixes this:**
> - **One SSID, many groups** — A single MPSK-enabled SSID can serve corporate devices, BYOD, and IoT simultaneously, each landing on its own VLAN, without broadcasting multiple SSIDs.
> - **Per-key revocation** — If a key is compromised, only that key is changed. With a shared PSK, rekeying forces every device to reconnect with a new password.
> - **IoT segmentation without 802.1x** — IoT devices rarely support certificate-based authentication. MPSK delivers VLAN segmentation using simple pre-shared keys that any device can use.
> - **Scheduled and limited keys** — Keys can be restricted to a schedule (e.g., contractor hours) and capped at a maximum number of connected devices, then deleted when no longer needed.
>
> **How to leverage it in production:**
> - Replace multiple SSIDs serving different device classes with a single MPSK SSID
> - Assign IoT devices a dedicated key mapped to an isolated IoT VLAN with no lateral movement
> - Map managed corporate devices to the corporate VLAN; BYOD devices get a restricted internet-only VLAN
> - Give contractors a time-limited key with a device cap — when the engagement ends, delete the key

##### Setting up MPSK SSID

1. In FortiManager, navigate to AP Manager > Managed FortiAPs > Connectivity Profiles > MPSK Profiles and click Create New

  ![alt text](media/Lab6-1.png)

> [!TIP]
> We are creating both WPA2 and WPA3 profiles here. 6 GHz only supports WPA3 (it's the rules — we don't make them), and WPA2 is included to support legacy devices.

2. Configure the first profile
   - Name: Devices-WPA2
   - Under MPSK Group List, click Add to create a group
     - Name: Laptops
     - VLAN Type: Fixed VLAN
     - VLAN ID: 161
     - In MPSK Key List, click Add
       - Name: Laptops
       - Pre-shared Key: WirelessLab1
       - Key Limit Type: Unlimited
       - Schedule: Always
       - Click OK

  ![alt text](media/Lab6-2.png)

     - Click OK
   - Create another group
     - Name: BYOD VLAN
     - VLAN Type: Fixed VLAN
     - VLAN ID: 171
     - In MPSK Key List, click Add
       - Name: BYOD VLAN
       - Pre-shared Key: WirelessLab2
       - Key Limit Type: Unlimited
       - Schedule: Always
       - Click OK
     - Click OK

  ![alt text](media/Lab6-3.png)

   - Click OK

> [!TIP]
> Notice how two completely different device groups — corporate laptops and BYOD — connect to the same SSID but are automatically placed on separate VLANs based solely on which key they use. No 802.1x infrastructure required for this level of segmentation.

> [!NOTE]
> WPA3 does not allow access without defining the MAC address. Be aware that most operating systems randomize MAC addresses for security purposes (macOS in particular) — you will need to disable MAC randomization when connecting to this SSID.

3. Create a second profile for WPA3
   - Name: Devices-WPA3
   - Under MPSK Group List, click Add to create a group
     - Name: Laptops
     - VLAN Type: Fixed VLAN
     - VLAN ID: 162
     - In MPSK Key List, click Add
       - Name: Laptops
       - Type: WPA3 SAE
       - SAE Password: WirelessLab1
       - MAC Address: [your laptop's MAC]
       - Schedule: Always
       - Click OK
     - Click OK
   - Create another group
     - Name: BYOD VLAN
     - VLAN Type: Fixed VLAN
     - VLAN ID: 172
     - In MPSK Key List, click Add
       - Name: BYOD VLAN
       - Pre-shared Key: WirelessLab2
       - MAC Address: [**NOT** your laptop's MAC] (cannot overlap)
       - Schedule: Always
       - Click OK
     - Click OK

4. Navigate to SSIDs and click Create New
   - Name: MPSK-WPA2
   - Traffic Mode: Tunnel
   - SSID: xx-Devices-WPA2 (xx is your pod number)
   - Security Mode: WPA2-Personal
   - Pre-shared Key: Multiple
   - MPSK Profile: Devices-WPA2
   - Dynamic VLAN Assignment: Enable
   - Schedule: Always

5. Clone the SSID above and change the following
   - Name: MPSK-WPA3
   - SSID: xx-Devices-WPA3 (xx is your pod number)
   - Security Mode: WPA3-SAE
   - SAE Password: fortinet1!
   - Hash-to-Element (H2E) Only: Enable
   - MPSK Profile: Devices-WPA3
   - Dynamic VLAN Assignment: Enable
   - Schedule: Always

> [!TIP]
> In a real deployment you would typically broadcast just one SSID name regardless of WPA version — the AP negotiates the strongest security the client supports. We are using two here to clearly demonstrate the difference in behaviour, particularly around MAC address requirements in WPA3.

6. Navigate to Operation Profiles > FortiAP Profiles and edit your existing profile
   - Add MPSK-WPA2 and MPSK-WPA3 to Radio 2
   - Add MPSK-WPA3 to Radio 3

  ![alt text](media/Lab6-5.png)

7. Run the VLAN/policy install script — mapped interfaces and policies have already been created for you

#### Lab complete — move on to Lab 7
