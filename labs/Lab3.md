#### Lab 3 - Provisioning AP

| Device       | Username/PW        |
| ------------ | ------------------ |
| FabricStudio | admin/fortinet1!   |
| FortiManager | admin/fortinet4A!! |

### Creating an ARRP Profile

1. Navigate to AP Manager > Managed FortiAPs > Operational Profiles > FortiAP Profiles

  ![alt text](media/Lab3-1.png)

2. Create a new ARRP Profile with the following settings
   - Name: aarp-lowdensity-highinterference
   - Weight Managed AP: 100
   - Weight Rogue AP: 30
   - Weight Weather Channel: 0
   - Weight DFS Channel: 0
   - Threshold Channel Load: 45
   - Leave the rest default and click OK

  ![alt text](media/Lab3-6.png)

### Creating an Operation Profile

3. Navigate to Operation Profiles > FortiAP Profiles

  ![alt text](media/Lab3-5.png)

4. Create a new profile with the following settings
   - Name: FAP-241K-SELab
   - Platform: FortiAP-241K (or the AP model assigned to you)
   - Region: Canada
   - AP Login Password: fortinet
   - Administrative Access: Enable HTTPS and SSH
   - Radio 1: Disabled (2.4 GHz from every pod = interference)
   - Radio 2:
     - Radio Resource Provision: Toggle On
     - ARRP Profile: aarp-lowdensity-highinterference
     - Bands: 802.11 be/ax/ac/n/a
     - Short Guard Interval: Toggle On
     - Transmit Power: Toggle dBm and set to 10 dBm
     - SSID: Tunneled
     - Advanced Options > Airtime Fairness: Toggle On
   - Radio 3:
     - Radio Resource Provision: Toggle On
     - ARRP Profile: aarp-lowdensity-highinterference
     - Bands: 802.11 be/ax
     - Channel Width: 80 MHz
     - Short Guard Interval: Toggle On
     - Transmit Power: Toggle Percent - 100%
     - SSID: Tunneled
     - Advanced Options > Airtime Fairness: Toggle On
   - Radio 4:
     - Mode: Dedicated to Monitor
     - WIDS Profile: default
   - Leave everything else as default and click OK

>[!NOTE]
> As you haven't added the AP yet, you'll need to toggle "View All Profiles" to make it visible.

>[!WARNING]
> Double-check everything looks correct — there is approximately a 50% chance the Radio 3 profile does not save. If so, go back in and toggle it back to Access Point instead of Dedicated Monitor.
>
> ##### Good
>
> ![alt text](media/Lab3-3.png)
>
> ##### Bad
>
> ![alt text](media/Lab3-7.png)

### Adding a Model AP

5. Navigate to AP Manager > Managed FortiAPs and click Create New Model AP
   - FortiGate: FGTBr01
   - Serial Number: FP241K****000000
   - Name: AP-241K-01
   - FortiAP Profile: FAP-241K-SELab

>[!CAUTION]
> If you are not using a FP241K, replace the first 6 characters of the serial number with the first 6 of your actual AP serial number.

   - Click OK

### Policy for NTP and DNS

>[!NOTE]
> Before adding the AP, ensure NTP and DNS work correctly. Rather than enabling both on the FortiGate directly, we will create a policy to allow APs to reach NS01.

6. Navigate to Policy & Objects > FGTBranch Firewall Policy and click Create New
   - Name: APServices -> NS01
   - Action: Accept
   - Incoming Interface: AP Mgmt
   - Outgoing Interface: Overlay
   - Source: Create New Address
     - Name: AP Mgmt Subnet
     - IP: 172.30.200.0/24
     - Click OK, add change note, and add as a source
   - Destination: NS01
   - Service: DNS and NTP
   - Leave the rest default, click OK, and fill in the change note

  ![alt text](media/Lab3-4.png)

7. Run the install wizard and select Install Policy Package & Device Settings, then click Next

  ![alt text](media/Lab3-8.png)

8. Once the install is complete, connect your AP to Port 1
   - You can continue to the next lab while this completes — it will take approximately 5-10 minutes to autolink, reboot, and finish

  ![alt text](media/Lab3-2.png)

#### Lab complete — move on to Lab 4
