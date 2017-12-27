# Overzicht
## [Wat is ExpressRoute?](expressroute-introduction.md)
## [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)
## [Connectiviteitsmodellen](expressroute-connectivity-models.md)
## [Circuits en routeringsdomeinen](expressroute-circuit-peerings.md)
## [Locaties en partners](expressroute-locations.md)
### [Providers per locatie](expressroute-locations-providers.md)
### [Locaties per provider](expressroute-locations.md)
## [Virtuele netwerkgateways voor ExpressRoute](expressroute-about-virtual-network-gateways.md)

# Aan de slag
## [Vereisten](expressroute-prerequisites.md)
## [Werkstromen](expressroute-workflows.md)
## [Routeringsvereisten](expressroute-routing.md)
## [QoS-vereisten](expressroute-qos.md)
## [Circuits verplaatsen van het klassieke model naar Resource Manager](expressroute-move.md)

# Procedures
## Een circuit maken en wijzigen
### [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
### [Azure PowerShell](expressroute-howto-circuit-arm.md)
### [Azure CLI](howto-circuit-cli.md)
## Een peeringconfiguratie maken en wijzigen
### [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
### [Azure PowerShell](expressroute-howto-routing-arm.md)
### [Azure CLI](howto-routing-cli.md)
## Een virtueel netwerk koppelen aan een ExpressRoute-circuit
### [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
### [Azure PowerShell](expressroute-howto-linkvnet-arm.md)
### [Azure CLI](howto-linkvnet-cli.md)
## [Een site-naar-site-VPN configureren via Microsoft-peering](site-to-site-vpn-over-microsoft-peering.md)
## Een virtuele netwerkgateway configureren voor ExpressRoute
### [Azure Portal](expressroute-howto-add-gateway-portal-resource-manager.md)
### [Azure PowerShell](expressroute-howto-add-gateway-resource-manager.md)
## [Gelijktijdige ExpressRoute- en site-naar-site-verbindingen configureren](expressroute-howto-coexist-resource-manager.md)
## Routefilters voor Microsoft-peering configureren
### [Azure Portal](how-to-routefilter-portal.md)
### [Azure PowerShell](how-to-routefilter-powershell.md)
### [Azure CLI](how-to-routefilter-cli.md)
## [Van openbare peering overstappen op Microsoft-peering](how-to-move-peering.md)
## [Een circuit van klassiek naar Resource Manager verplaatsen](expressroute-howto-move-arm.md)
## [Gekoppelde virtuele netwerken van het klassieke model naar Resource Manager migreren](expressroute-migration-classic-resource-manager.md)
## Een router voor ExpressRoute configureren
### [Een router configureren](expressroute-config-samples-routing.md)
### [Voorbeelden van routerconfiguraties voor NAT](expressroute-config-samples-nat.md)
## [Netwerkprestatiemeter configureren voor ExpressRoute](how-to-npm.md)
## Artikelen over het klassieke implementatiemodel
### [Een circuit wijzigen](expressroute-howto-circuit-classic.md)
### [Een peeringconfiguratie maken en wijzigen](expressroute-howto-routing-classic.md)
### [Een virtueel netwerk koppelen aan een ExpressRoute-circuit](expressroute-howto-linkvnet-classic.md)
### [Gelijktijdige ExpressRoute- en S2S-verbindingen configureren](expressroute-howto-coexist-classic.md)
### [Een gateway aan een VNet toevoegen](expressroute-howto-add-gateway-classic.md)

## Beste praktijken
### [Aanbevolen procedures voor netwerkbeveiliging en cloudservices](../best-practices-network-security.md)
### [Routering optimaliseren](expressroute-optimize-routing.md)
### [Asymmetrisch routeren](expressroute-asymmetric-routing.md)
### [NAT voor ExpressRoute](expressroute-nat.md)

## Problemen oplossen
### [ExpressRoute-connectiviteit controleren](expressroute-troubleshooting-expressroute-overview.md)
### [Problemen met de netwerkprestaties oplossen](expressroute-troubleshooting-network-performance.md)
### [Een mislukt circuit opnieuw instellen](reset-circuit.md)
### [ARP-tabellen ophalen](expressroute-troubleshooting-arp-resource-manager.md)
### [ARP-tabellen ophalen (klassiek)](expressroute-troubleshooting-arp-classic.md)

# Naslaginformatie
## [Azure PowerShell](/powershell/module/azurerm.network/?view=azurermps-4.0.0#expressroute)
## [Azure CLI](/cli/azure/network/express-route)
## [REST](https://msdn.microsoft.com/library/azure/mt586720)
## [REST (klassiek)](https://msdn.microsoft.com/library/azure/dn606310)

# Verwant
## [Virtueel netwerk](/azure/virtual-network/)
## [VPN Gateway](/azure/vpn-gateway/)
## [Virtuele machines](/azure/virtual-machines/)
## [Load Balancer](/azure/load-balancer/)
## [Traffic Manager](/azure/traffic-manager/)

# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=networking)
## [Casestudy's](https://customers.microsoft.com/Pages/advancedsearch.aspx?mrmcproducts=More%20Products)
## [Netwerkblog](https://azure.microsoft.com/blog/topics/networking/)
## [Prijzen](https://azure.microsoft.com/pricing/details/expressroute/)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Service-updates](https://azure.microsoft.com/updates/?product=expressroute)
## [SLA](https://azure.microsoft.com/support/legal/sla/)
## [Abonnements- en servicebeperkingen](../azure-subscription-service-limits.md?toc=%2fazure%2fexpressroute%2ftoc.json)
## [ExpressRoute voor Cloud Solution Providers (CSP)](expressroute-for-cloud-solution-providers.md)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=expressroute)
### [Een virtuele netwerkgateway verbinden met een circuit](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit/)
### [Een virtueel netwerk maken voor ExpressRoute](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-virtual-network/)
### [Een virtuele netwerkgateway maken voor ExpressRoute](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network/)
### [Een ExpressRoute-circuit maken](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit/)
### [Uw netwerkinfrastructuur ontwikkelen voor connectiviteit](https://go.microsoft.com/fwlink/p/?LinkId=615124)
### [Privépeering instellen voor uw circuit](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit/)
### [Hybride partnerverbanden: on-premises scenario's mogelijk maken](https://go.microsoft.com/fwlink/p/?LinkId=615125)
### [Microsoft-peering instellen voor uw circuit](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit/)
### [Openbare peering instellen voor uw circuit](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit/)
