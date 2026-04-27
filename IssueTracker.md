## TODO
1. Add Interface Estimated Bandwidth as part of SD-WAN Monitoring AIOPS Lab
2. setup syslog forwarder for FAZ to AI-Ops
3. 
4. 
5. 



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
8. I've created a script for the Interfacse for MPSK Lab, just need to add it to template **[ Fixed in new Branch on 25th]**
   - Added as part of the lab to run this script
9.  Add to the basic lab start connections to fortianalyzer for DC and Branch **[ Fixed in new Branch on 25th]**
    - This This was untested with Branch and fixed again in later branch
10. Radius isn't allowed on the FAC network interface, go turn it on **[ Fixed in new Branch on 25th]**
    - Enabled on interface of fac
11. SD-WAN order is wrong... Fix it **[ Fixed in new Branch on 25th]**
    - Re Ordered Policies because intenet was above internal, leaking internal externally
12. Go back and Redo all the subnets to use .254 gateway and fix DHCP scopes to not include that **[ Fixed in new Branch on 25th]**
13. Pre Create address objects for all subnets, They don't need to do that more than once **[ Fixed in new Branch on 25th]**
14. Create Mapped interfaces for all of the MPSK lab, they've done enough of that **[ Fixed in new Branch on 25th]**
15. Fix connection for FGBr01 to fortianalyzer **[ Fixed in new Branch on 26th]**
    - Fixed by adding Pre-config that uses source IP of ServicesLB
16. ca cert chain looks broken... fix **[ Fixed in new Branch on 26th]**
    - Recreated and confirmed uploading of both to FGTDC01. Fortigate origionally didn't like the duplicate nature of the two so instead created Root and Intermediate only with CN.
17. Configure Star Cert and add it to FMG to push to FGT with root and Inter **[ Fixed in new Branch on 26th]**
    - Configured SCEP on Fauth to serve out what ever cert we want, also configured two new templates in fortimanager for Star and Portal that I'll use as part of the labs later
18. add portal.fortiacme.com to dns server for 172.30.220.254 **[ Fixed in new Branch on 26th]**
    - Added
19. Add addresses for NS01 and all FortiDevices as premade **[ Fixed in new Branch on 26th]**
    - added all but portal and fgts
20. Fortiauthenticator default cert went dodo, regenerated it **[ Fixed in new Branch on 26th V2]**
    - I've also fixed pod6 and pod7 so no need to worry running into this in current pods
