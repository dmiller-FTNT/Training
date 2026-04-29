#### Lab 3 - Provisioning AP

| Device       | Username/PW        |
| ------------ | ------------------ |
| FabricStudio | admin/fortinet1!   |
| FortiManager | admin/fortinet4A!! |

> [!NOTE]
> **Centralised AP Management and Radio Planning**
>
> Managing APs individually — logging into each one, configuring channels, power, and SSIDs — does not scale. FortiManager's AP Manager lets you define a single FortiAP profile that governs every radio setting for every AP of a given type across your entire estate. Change the profile and push it; every AP updates simultaneously.
>
> **ARRP — Automatic Radio Resource Provisioning:**
> In a dense wireless environment, APs on the same or adjacent channels cause co-channel interference, which degrades performance for everyone. ARRP is FortiManager's automated channel and power management system. Rather than assigning channels statically and hoping nothing changes, ARRP continuously monitors the RF environment and uses a weighted scoring algorithm to select the best channel for each AP based on factors like neighbouring managed APs, rogue APs, DFS channels, and channel load. The result is a self-optimising RF environment that adapts as conditions change.
>
> **Radio design principles used in this lab:**
> - **Radio 1 (2.4 GHz) is disabled** — In a classroom or office environment with many APs in close proximity, every AP broadcasting on 2.4 GHz causes severe interference. There are only three non-overlapping channels (1, 6, 11), and with this many pods, saturation is inevitable. Disabling 2.4 GHz here is a deliberate RF hygiene choice. In production, consider whether 2.4 GHz is needed at all — for environments where all clients support 5 GHz or 6 GHz, removing 2.4 GHz reduces interference and airtime contention significantly.
> - **Radio 2 (5 GHz) — broad compatibility** — configured for 802.11 a/n/ac/ax/be to support the widest range of client devices
> - **Radio 3 (6 GHz) — Wi-Fi 6E/7 only** — restricted to 802.11ax/be because 6 GHz is exclusively WPA3, providing a clean, uncongested band for modern devices
> - **Radio 4 — Dedicated Monitor with WIDS** — not used for client traffic; instead it continuously scans the RF environment for rogue APs, unauthorised devices, and wireless attacks. This is your passive wireless intrusion detection system.

##### Creating an ARRP Profile

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

> [!TIP]
> **Understanding the ARRP weights:**
> Each weight tells the algorithm how much to penalise a channel for a given condition. A high Weight Managed AP (100) means the system strongly avoids placing two managed APs on the same channel. Weight Rogue AP (30) adds a moderate penalty for channels where rogue devices are detected. Weather and DFS channels are set to 0 here, meaning the algorithm will use them freely — in production environments near airports or with radar sources, you may want to increase the DFS weight to avoid channels that may be interrupted by radar events. The Channel Load Threshold (45%) triggers a channel change when a channel is heavily utilised, preventing any single channel from becoming a bottleneck.

##### Creating an Operation Profile

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

> [!TIP]
> **Airtime Fairness** prevents a single slow client (e.g., an older 802.11n device) from consuming a disproportionate share of airtime and degrading performance for faster clients on the same AP. Without it, a slow client transmitting at 6 Mbps takes the same amount of time to send data as a fast client sending at 600 Mbps — effectively throttling the entire cell to the speed of the slowest device.

> [!NOTE]
> As you haven't added the AP yet, you'll need to toggle "View All Profiles" to make it visible.

> [!WARNING]
> Double-check everything looks correct — there is approximately a 50% chance the Radio 3 profile does not save. If so, go back in and toggle it back to Access Point instead of Dedicated Monitor.
>
> ##### Good
>
> ![alt text](media/Lab3-3.png)
>
> ##### Bad
>
> ![alt text](media/Lab3-7.png)

##### Adding a Model AP

> [!TIP]
> **Model APs — pre-staging before hardware arrives:**
> Just like model devices for FortiGates, a model AP lets you pre-configure the AP in FortiManager before the physical hardware is on site. You assign it a wildcard or specific serial number, attach a FortiAP profile, and it will receive its full configuration the moment it connects to the FortiGate. In a large deployment with hundreds of APs across many sites, this means field technicians only need to plug in the AP — all configuration is delivered automatically.

5. Navigate to AP Manager > Managed FortiAPs and click Create New Model AP
   - FortiGate: FGTBr01
   - Serial Number: FP241K****000000
   - Name: AP-241K-01
   - FortiAP Profile: FAP-241K-SELab

> [!CAUTION]
> If you are not using a FP241K, replace the first 6 characters of the serial number with the first 6 of your actual AP serial number.

   - Click OK

##### Policy for NTP and DNS

> [!NOTE]
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
