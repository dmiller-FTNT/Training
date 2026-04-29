Navigate to the AI Insights Menu

Select the Network Assurance 

This provides a wireless Health Score and identifies the lowest performing AP.
It also identifies the SLA Score and Shows Success Failures and Details of SLA Violations.

![alt text](media/lab11-1.png)

Explore this and find details of the issues that have impacted the scores.

![alt text](media/lab11-2.png)

You will se details in the summary the logs are turned into simple english for your review and consideration.

![alt text](media/lab11-3.png)

Explore this for the other areas.

Move to imacted SLAs
![alt text](media/lab11-4.png)

As you look though this and consider what was availible previously to in FAZ and FortiOS alone. The ability for AIOPS to provide the insight you need for effective understanding of your enviroment should be becoming apparant.

Not only are the issues identifed its giving you advice on how to resolve them.

![alt text](media/lab11-5.png)

SLA is configured in the configuration section within this menu

Move the Time to connect SLA to Dynamic Baselines Configuration.

AIOPS will learn what is normal for that AP by lookking at logs and as each AP is placed in a different place this may be different for each AP. Alternativley you can do this by ADOM or Fortigate as desired.

![alt text](media/lab11-6.png)

Now Navigate to the following Wireless then Wireless Clients

Select a Client and select view details

![alt text](media/lab11-7.png)

You will get alot of infomration about that client and its current capabilites. Move down the AI Insights and find more details on issues and examine the options to resolve them offered by AIOPS

![alt text](media/lab11-8.png)

Review the other menus of Performance, Application, Destinations, Policies and Logs.

Note Applications require APP inspection to be enabled on your profile.

AIOPS provides Insight in SDWAN and Forcasting.

Click on Wireless -> Access Points then select your AP

Make a mental note of what Channel your operating on on 5ghz

Click on view details.

Navigate to Spectrum Analysis

Select the Band for 5ghz and the Channel Range to include your channel.

Click Start

![alt text](media/lab11-9.png)

On your device connected to your AP run speedtest and scroll down to duty cycle.

![alt text](media/lab11-10.png)

You should notice this increases. Consider though this is normal. its normal to consume airtime when we are moving data during something like a speedtest. If we see high channel utilization consistently under normal operation this is where it becomes a problem and re enforces the requirement to consider capacity planning for networks.

You can review the spectorgram as this shows the duty cycle over time. 

![alt text](media/lab11-11.png)

If you see alot of red it shows high duty cycle which is the RF term for Channel Utlization. However we normally only think about Channel utilization in Wi-Fi terms. Remember WiFi uses channels that dont have licencing and are not dedicated just to Wi-Fi.