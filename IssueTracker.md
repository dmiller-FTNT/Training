## TODO
1. Add Interface Estimated Bandwidth as part of SD-WAN Monitoring AIOPS Lab
2. Add to the basic lab start connections to fortianalyzer for DC and Branch as well as setup syslog forwarder for FAZ to AI-Ops


#### Outstanding issues
1. FortiManager and FortiAnalyzer seem to be tied to a single session so if you log in one, you kick out the other
   1. Not fixing this... low hanging fruit


#### Completed
1. Check default gateway on ns01, was broken by FabricStudio **[ Fixed in new Branch ]**
2. Change template for branch to listen NTP on fortilink ( is broken, server dissabled need to enable and add fortilink ) **[ Fixed in new Branch ]**
3. Add Further SDWAN config to support AI OPS **[ Fixed in new Branch ]**
5. Bug found in fortimanager/Fortigate when pushing DHCP subnets 
   1. ~~**[ Added Note in Lab Guide to Notify - Will look at upgrading to 8.0.0 release to see if it was fixed]**~~
   2. **This seems to be fixed in 8.0.0 release but ive left it as a note in the guide to notify the se's should they run into it**
6. Issue with ARRP Profile default changes
   1. Seems to be ok when you create your own profile and don't change the default, Might have been fixed in 8.0.0 Release
7. 50% of the time when you create the custom Operational Profile for AP's the first time it switches radio 3 to monitor, independant of setting.
   1. I've added a note to the lab to check for this, the settings are saved, you just need to go back in and change it back to radio profile **[ not fixed in 8.0.0 release ]** 