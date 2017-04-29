Wanneer u een virtuele netwerkgateway maakt, moet u de gewenste gateway-SKU opgeven. Wanneer u een hogere gateway-SKU selecteert, wordt meer CPU en bandbreedte van het netwerk toegewezen aan de gateway. Daardoor kan de gateway hogere netwerkdoorvoer aan het virtuele netwerk ondersteunen.

VPN Gateway kan de volgende SKU’s gebruiken:

* Basic
* Standard
* HighPerformance

VPN Gateway maakt geen gebruik van de gateway-SKU UltraPerformance. Meer informatie over de SKU UltraPerformance vindt u in het document [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Houd rekening met het volgende als u een SKU selecteert:

* Als u een PolicyBased VPN-type gebruikt, moet u Basis-SKU gebruiken. PolicyBased VPN-verbindingen (voorheen statische routering) worden niet ondersteund op andere SKU's.
* BGP wordt niet ondersteund op de basis-SKU.
* ExpressRoute-VPN-Gateway-configuraties die naast elkaar bestaan, worden niet ondersteund in de basis-SKU.
* Actief-actief S2S VPN-gatewayverbindingen kunnen alleen worden geconfigureerd op de HighPerformance SKU.

