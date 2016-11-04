Wanneer u een virtuele netwerkgateway maakt, moet u de gewenste gateway-SKU opgeven. Wanneer u een hogere gateway-SKU selecteert, wordt meer CPU en bandbreedte van het netwerk toegewezen aan de gateway. Daardoor kan de gateway hogere netwerkdoorvoer aan het virtuele netwerk ondersteunen.

VPN Gateway kan de volgende SKU’s gebruiken:

* Basic
* Standard
* HighPerformance

Wanneer u een SKU selecteert, gelden de volgende beperkingen:

* Als u een PolicyBased VPN-type gebruikt, moet u de gateway-SKU Basic gebruiken. PolicyBased VPN-verbindingen (voorheen statische routering) worden niet ondersteund op andere SKU's.
* BGP wordt niet ondersteund op de basis-SKU.
* ExpressRoute-VPN-Gateway-configuraties die naast elkaar bestaan, worden niet ondersteund in de basis-SKU.

<!--HONumber=Oct16_HO3-->


