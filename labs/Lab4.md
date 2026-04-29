FortiAIOps intergration

Note that FortiAnalyzer in your lab is set up to forward all logs to AIOPS.

Log into FortiAnalyzer and Authroize your device.

Authorize the device

In our case this will be part of the root ADOM.

click ok and then close.

Navigate to System Settings -> Advanced ->
Log Forwarding Tab
Create New

Set Name : FortiAIOPS
Remote Server Type : Syslog

Click OK

Navigate to your fortigate
https://172.31.1(PodID).200

Admin 
No Password

Change it to Fortinet4A!!

![alt text](media/lab4-1.png)


Select Login Read Only

Go to system then settings
Under the access tab download the HTTPS CA Certificate

![alt text](media/lab4-2.png)

Logout of your FortiGate.

Login to AIOps

Goto System -> CA Certificates
![alt text](media/lab4-3.png)

Install CA Certificate
Select the certificate from your fortigate
Certificate Name FGTBr01

Note if you change your HTTPS certificate due to an upgrade or a cert being loaded then this will need to be updated. Its recormended that customers use a wildcard cert here.

Cick on Inventory then Managed FortiGates
Click ADD Enter the IP or hostname of your fortigate in our case 172.31.1(PODID).200 eg for Pod1 172.31.101.200

username admin
password Fortinet4A!!

![alt text](media/lab4-4.png)

Shortly you Should see the Fortigate come online
![alt text](media/lab4-5.png)

