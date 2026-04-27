# Lab 2 - Provisioning Switch

| Device       | Username/PW        |
| ------------ | ------------------ |
| FabricStudio | admin/fortinet1!   |
| FortiManager | admin/fortinet4A!! |
| FGTDC01      | admin/fortinet4A!! |

### Adding the Fortiswitch

>[!NOTE]
> This Lab has dissabled central managment for Switch and AP manager to focus more closely on ease configruation. Please leave them off

1. Navagate to FortiSwitch Manager > Managed FortiSwitches and "Create New" Fortiswitch
![](media/Lab2-3.png)
2. Exploring Wildcard Switches, please add the following
   - Fortigate : FGTBr01
   - Serial Number : SM10GF****000000
   - Name : FS-SM10GF-01
>[!caution]
>If you're not using a SM10FG replace the first 6 of the SN with the first 6 of the actual Switch SN you have
3. click ok

![](media/Lab2-1.png)

>[!TIP]
> Under Managed Fortiswitches > Cli Configuration > Switch-Profile, you can find the ability to change the the default password on fortiswitches
>
> ![](media/Lab2-2.png)

### Adding Vlans

4. Before Connecting the fortiswitch lets provision a pair of vlans for use later

| Name        | Network         | Gateway        | Vlan ID |
| ----------- | --------------- | -------------- | ------- |
| AP Mgmt     | 172.30.200.0/24 | 172.30.200.254 | 200     |
| User Access | 172.30.210.0/24 | 172.30.210.254 | 210     |

5. Navagate to FortiSwitch Manager > Managed FortiSwitches > Vlan and "Create New" Vlan Interface

>[!caution]
>If when creating your DHCP scopes for the following Vlans you notice no range go back up to the addressing mode and reset it to "manual" and reconfirm
6. First Vlan will be :
    - Interface Name : AP Mgmt
    - Vlan ID : 200
    - IP/Netmask : 172.30.200.254/24
    - Admin Access : Ping and Secuirty Fabric Connection Enabled
    - DHCP Server Enabled
      - address Range : 172.30.200.2 - 172.30.200.250
      - Netmask : Specify - 255.255.255.0
      - Change DNS to be "Same as System DNS"
      - Under Advanced Change NTP to "Same as System NTP"
      - Change Wireless Controllers to "Same as Interface IP"
![](media/Lab2-4.png)
   - Lastly at the bottem Select Map to Normalized Interfaces and create new
![](media/Lab2-5.png)
   - Name : AP Mgmt
   - Per-Platform Mapping : Create New
     - Matched Platform : All
     - Mapped Interface Name : AP Mgmt
![](media/Lab2-6.png)
     - Click OK and add change notes
   - Leave the rest Default
   - Click OK

1. The second Vlan:
   - Interface Name : User Access
   - Vlan ID : 210
   - IP/Netmask : 172.30.210.254/24
   - Admin Access : Ping, HTTPS and SSH Enabled
   - DHCP Server Enabled
      - address Range : 172.30.210.2 - 172.30.210.250
      - Netmask : Specify - 255.255.255.0
      - Change DNS to be "Same as System DNS"
      - Under Advanced Change NTP to "Same as System NTP"
   - Lastly at the bottem Select Map to Normalized Interfaces and create new
   - Name : User Access
   - Per-Platform Mapping : Create New
     - Matched Platform : All
     - Mapped Interface Name : User Access
     - Click OK and add change notes 
   - Leave the rest Default
   - Click OK

>You should now have two New Vlans ![](media/Lab2-7.png)

1. Lastly, Lets configure two switchports on the Model Fortiswitch. Navagate back to the Fortiswitch you created and Edit
2. Configure: 
    - Port1 Native Vlan : AP Mgmt / Allowed Vlans : All
    - Port2 Native Vlan : User Access 

![](media/Lab2-8.png)

10. From the top use the install wizzard to install device settings only
![](media/Lab2-11.png)
11. Once the install is complete power up and connect your Switch PortA on FGTBr01 to Port 10 on your switch
    - You can either wait for it to finish or continue on this will take around 5 - 10 minutes to autolink reboot and finish

![](media/Lab2-9.png)

#### Lab complete move onto Lab 3


>[!note]
>###### This appears to be fixed in 8.0 Release but I'm leaving it here incase you run into it
> There seems to be a bug with this Beta version of Manager/Gate that causes the Netmask of DHCP servers not to be correctly passed to the fortigate. Please Log onto the Fortigate directly after the install and check that your server ranges for the two vlans you created have a valid Netmask, they will likely have a 0.0.0.0
> ![](media/Lab2-10.png)
> After you've fixed these go ahead and do a quick install on FGTBr01, it will go back to happy

