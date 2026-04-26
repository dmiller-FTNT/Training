FortiAIOps intergration

Note that FortiAnalyzer in your lab is set up to forward all logs to AIOPS.

Modify your Device template to forward logs to FortiAnalyer


Device Manager > Provisioning Templates

Select Branch_System

Scroll to Log Settings

"Send logs to FortiAnalyzer/FortiManager" Toggle the Enable.

Sendto : Specify IP Address

Upload Option : Real time

Click OK
Enter your Change Note: Send logs to FAZ from Branch



Then select Device Manager and groups

You will see this is modified.

Right click on your branch Fortigate and Quick install the device DB.


You can inspect the configuration in FAZ

Login to AIOps

In Security Fabrick Menu 
Select Fabric Connector
Set the Deployment mode to Fortimanager
Select Connect New
![alt text](image-1.png)

Enter the IP of your fortimanger 172.30.100.11

![alt text](image-2.png)

This will show disconnected.

Log into fortimanager

Fabric View

Enable FortiAIOPS fabric connector
![alt text](image.png)


