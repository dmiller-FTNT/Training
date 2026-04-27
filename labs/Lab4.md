FortiAIOps intergration

Note that FortiAnalyzer in your lab is set up to forward all logs to AIOPS.

Modify your Device template to forward logs to FortiAnalyer


Device Manager > Provisioning Templates

System Templates Tab

Select Branch_System

Scroll to Log Settings

"Send logs to FortiAnalyzer/FortiManager" Toggle the Enable.

Sendto : Specify IP Address

Upload Option : Real time

Serial Number :FAZ-VMTM26007957 (Check on your FAZ before doing this to ensure you have the right serial number for your pod)

![alt text](image-12.png)

Click OK
Enter your Change Note: Send logs to FAZ from Branch



Then select Device Manager and groups

You will see this is modified.

![alt text](image-14.png)


Right click on your branch Fortigate and Quick install the device DB.

![alt text](image-15.png)

Branch Fortigate will show as an anauthorized device
![alt text](image-17.png)

Authorize the device

In our case this will be part of the root ADOM.

click ok and then close.

Navigate to System Settings -> Advanced
Log Forwarding Tab
Create New

Set Name : FortiAIOPS
Remote Server Type : Syslog

Click OK


Log into FortiManager

System Settings
Fabric Management

Toggle Status to Enable

Confirm
![alt text](image-20.png)

Set Status to Enable

Set Role to Standalone
Click Apply

![alt text](image-21.png)


Login to AIOps

First we need to add the certificates from FortiAutheticator.

Navigate to Security Fabric
CA Certificates

Install your fortiacme Root then intermediate certificate
followed by your star certificates.

![alt text](image-19.png)

In Security Fabric Menu 
Select Fabric Connector
Set the Deployment mode to Fortimanager
Select Connect New


Enter the IP of your fortimanger 172.30.100.11
![alt text](image-18.png)

This will show Connected.

Click on Authrorize

Accept the Certificate

![alt text](image-22.png)

Log into fortimanager

Fabric View

Enable FortiAIOPS fabric connector
![alt text](image-23.png)

Toggle to enabel then Authrorize the AIOPS connector.

![
    
](image-24.png)

Confirm the action

![alt text](image-26.png)

Click Save

You will now see the AIOPS connector enabled.

Go back to AIOPS 

Select the Root Adom by clicking on the adom on the top bar.
then select root
