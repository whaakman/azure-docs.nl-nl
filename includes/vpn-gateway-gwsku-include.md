Wanneer u een virtuele netwerkgateway maakt, moet u de gewenste gateway-SKU opgeven. Selecteer de SKU's die aan uw vereisten voldoen op basis van de typen werkbelasting, doorvoer, functies en SLA's.

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <a name="workloads"></a>Productie *vs.* werkbelastingen voor ontwikkelen en testen

Vanwege de verschillen in SLA's en functiesets, raden we de volgende SKU's aan voor productie *vs.* ontwikkelen en testen:

| **Workload**                       | **SKU's**               |
| ---                                | ---                    |
| **Productie, kritieke werkbelastingen** | VpnGw1, VpnGw2, VpnGw3 |
| **Ontwikkelen en testen of conceptontwerpen**   | Basic                  |
|                                    |                        |

Als u de oude SKU's gebruikt, zijn de aanbevelingen voor de productie-SKU Standard- en HighPerformance-SKU's. Zie [Gateway-SKU's (verouderde SKU's)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md) voor informatie over de oude SKU's.

###  <a name="feature"></a>Gateway-SKU-functiesets

De nieuwe gateway SKU's stroomlijnen de functiesets die worden aangeboden op de gateways:

| **SKU**| **Functies**|
| ---    | ---         |
|**Basic**   | **Route gebaseerde VPN-**: 10 tunnels met P2S; er zijn geen RADIUS-verificatie voor P2S; er is geen IKEv2 voor P2S<br>**Op beleid gebaseerd VPN**: (IKEv1): 1 tunnel; geen P2S|
| **VpnGw1, VpnGw2 en VpnGw3** | **Op route gebaseerd VPN**: maximaal 30 tunnels ( * ), P2S, BGP, actief-actief, aangepast IPsec/IKE-beleid, ExpressRoute/VPN samen |
|        |             |

( * ) U kunt "PolicyBasedTrafficSelectors" configureren om een op route gebaseerde VPN-gateway (VpnGw1, VpnGw2, VpnGw3) te verbinden met meerdere on-premises, op beleid gebaseerde firewallapparaten. Raadpleeg [Connect VPN gateways to multiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) (VPN-gateways verbinden met meerdere on-premises,op beleid gebaseerde VPN-apparaten met behulp van PowerShell) voor meer informatie.

###  <a name="resize"></a>Het formaat van gateway-SKU's wijzigen

1. U kunt wisselen tussen VpnGw1-, VpnGw2- en VpnGw3-SKU's.
2. Als u met de oude gateway-SKU's werkt, kunt u wisselen tussen Basic-, Standard- en HighPerformance-SKU's.
2. U kunt**niet** wisselen van Basic-/Standard-/HighPerformance-SKU's naar de nieuwe VpnGw2-/VpnGw1-/VpnGw3-SKU's. U moet in plaats daarvan naar de nieuwe SKU's [migreren](#migrate).

###  <a name="migrate"></a>Van oude SKU's naar nieuwe SKU's migreren

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
