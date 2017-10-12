# Overzicht
## [Over Azure-netwerken](networking-overview.md)
## Architectuur
### [Virtuele datacenters](networking-virtual-datacenter.md)
### [Asymmetrische routering met meerdere netwerkpaden](../expressroute/expressroute-asymmetric-routing.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Netwerkverkeer beveiligen](../best-practices-network-security.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Hub-spoke topologie](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
### [Aanbevolen procedures voor netwerkbeveiliging](../security/azure-security-network-security-best-practices.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Virtuele netwerkapparaten met hoge beschikbaarheid](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha )
### [Taakverdelingsmethoden combineren](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Plannen en ontwerpen
### [Virtuele netwerken](../virtual-network/virtual-network-vnet-plan-design-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Verkeer filteren](../virtual-network/virtual-networks-nsg.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Cross-premises-connectiviteit - VPN](../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Cross-premises-connectiviteit - exclusieve privéverbinding](../expressroute/expressroute-workflows.md?toc=%2fazure%2fnetworking%2ftoc.json)

##  Concepten
### [Virtuele netwerken](../virtual-network/virtual-networks-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Taakverdeling voor netwerken](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Taakverdeling voor toepassingen](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [DNS](../dns/dns-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Verkeersdistributie op basis van DNS](../traffic-manager/traffic-manager-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [On-premises verbinden - VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [On-premises verbinden - toegewezen](../expressroute/expressroute-introduction.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Aan de slag
## [Uw eerste virtuele netwerk maken](../virtual-network/virtual-network-get-started-vnet-subnet.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Procedures
## Internetconnectiviteit
### [Netwerktaakverdeling op openbare servers](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Toepassingstaakverdeling op openbare servers](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Webtoepassingen beschermen](../application-gateway/application-gateway-web-application-firewall-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Verkeer distribueren naar meerdere locaties](../traffic-manager/traffic-manager-configure-geographic-routing-method.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Interne connectiviteit
### [Netwerktaakverdeling op privéservers](../load-balancer/load-balancer-get-started-ilb-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Toepassingstaakverdeling op privéservers](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Verbinding maken met virtuele netwerken (dezelfde locatie)](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Verbinding maken met virtuele netwerken (andere locaties)](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
## Cross-premises-connectiviteit
### [Een S2S-VPN-verbinding maken (IPsec/IKE)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Een P2S-VPN-verbinding maken (SSTP met certificaten)](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Een exclusieve privéverbinding maken (ExpressRoute)](../expressroute/expressroute-howto-circuit-portal-resource-manager.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Beheer
### [Topologie van netwerk weergeven](../network-watcher/network-watcher-topology-powershell.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Pakketopname beheren](../network-watcher/network-watcher-packet-capture-manage-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Routering van volgende hop bepalen](../network-watcher/network-watcher-check-next-hop-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [IP-stromen controleren voor een virtuele machine](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

## Voorbeeldscripts
### [Azure-CLI](cli-samples.md)
### [Azure PowerShell](powershell-samples.md)

## Zelfstudies
### [Taakverdelingen maken voor virtuele machines](../virtual-machines/linux/tutorial-load-balance-nodejs.md?toc=%2fazure%2fnetworking%2ftoc.json)
### [Een computer verbinden met een virtueel netwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)

# Naslaginformatie
## [Azure-CLI](https://docs.microsoft.com/cli/azure/network)
## [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/?view=azurermps-3.8.0)
## [.Net](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.network?view=azuremgmtnetwork-9.1.0-preview)
## [Node.js](https://azure.microsoft.com/develop/nodejs/#azure-sdk)
## [REST](https://msdn.microsoft.com/library/mt163658.aspx)

# Resources
## [Auteursjablonen](/azure/azure-resource-manager/resource-group-authoring-templates?toc=%2fazure%2fnetworking%2ftoc.json)
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=networking)
## [Communitysjablonen](https://azure.microsoft.com/resources/templates/)
## [Netwerkblog](http://azure.microsoft.com/blog/topics/networking)
## [Prijzen](https://azure.microsoft.com/pricing)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Regionale beschikbaarheid](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
## [Video's](https://azure.microsoft.com/resources/videos/index/?services=virtual-network)

