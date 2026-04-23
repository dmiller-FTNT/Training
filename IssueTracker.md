## TODO
1. Check default gateway on ns01, seems to be broken ( sudo ip route replace default via 172.30.100.254 dev ens4 , verify ip r / timedatectl status ) **[ Fixed in new Branch ]**
2. Change template for branch to listen NTP on fortilink ( is broken, server dissabled need to enable and add fortilink ) **[ Fixed in new Branch ]**
3. Add Further SDWAN config to support AI OPS **[ Fixed in new Branch ]**
4. Add Interface Estimated Bandwidth as part of SD-WAN Monitoring AIOPS Lab
5. Bug found in fortimanager/Fortigate when pushing DHCP subnets **[ Added Note in Guide to Notify ]**