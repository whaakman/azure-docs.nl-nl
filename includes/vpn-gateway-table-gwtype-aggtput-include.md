|    | **Doorvoer VPN-gateway (1)** | **Max. IPsec-tunnels VPN-gateway (2)** | **Doorvoer ExpressRoute-gateway** | **VPN-gateway en ExpressRoute bestaan tegelijk**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Basis-SKU**              |  100 Mbps | 10                         |  500 Mbps                           | Nee   |
| **Standaard SKU (3)**           |  100 Mbps | 10                         | 1000 Mbps                           | Ja  |
| **High Performance SKU (3)**   | 200 Mbps  | 30                         | 2000 Mbps                           | Ja  |

- (1) De VPN-doorvoer is een ruwe schatting op basis van metingen tussen VNET's in dezelfde Azure-regio. Het is geen gegarandeerde doorvoer voor cross-premises verbindingen via internet. Het is een meting van de maximaal mogelijke doorvoer.
- (2) Voor het aantal tunnels raadpleegt u de VPN op basis van route. Een op beleid gebaseerde VPN biedt alleen ondersteuning voor één site-naar-site VPN-tunnel.
- (3) Op beleid gebaseerde VPN-verbindingen worden niet ondersteund op deze SKU. Ze worden alleen ondersteund op de Basic SKU.

<!--HONumber=Sep16_HO4-->


