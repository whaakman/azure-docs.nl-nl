# Overzicht
## [Over VPN Gateway](vpn-gateway-about-vpngateways.md)
## [Veelgestelde vragen VPN Gateway](vpn-gateway-vpn-faq.md)
## [Abonnements- en servicebeperkingen](../azure-subscription-service-limits.md?toc=%2fazure%2fvpn-gateway%2ftoc.json)

# Aan de slag
## [Planning en ontwerp voor VPN Gateway](vpn-gateway-plan-design.md)
## [Over instellingen voor VPN Gateway](vpn-gateway-about-vpn-gateway-settings.md)
## [Over VPN-apparaten](vpn-gateway-about-vpn-devices.md)
## [Over cryptografische vereisten](vpn-gateway-about-compliance-crypto.md)
## [Over BGP en VPN Gateway](vpn-gateway-bgp-overview.md)
## [Info over maximaal beschikbare verbindingen](vpn-gateway-highlyavailable.md)
## [Over point-to-site-verbindingen](point-to-site-about.md)

# Procedures
## Site-to-siteverbindingen configureren
### [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
### [Azure PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
### [Azure CLI](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
### [Azure Portal (klassiek)](vpn-gateway-howto-site-to-site-classic-portal.md)

## Point-to-site-verbindingen configureren – systeemeigen Azure-certificaatverificatie
### Een point-to-site-VPN configureren
#### [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
#### [Azure PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
#### [Azure Portal (klassiek)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
### Zelfondertekende certificaten genereren
#### [Azure PowerShell](vpn-gateway-certificates-point-to-site.md)
#### [Makecert](vpn-gateway-certificates-point-to-site-makecert.md)
### [VPN-clientconfiguratiebestanden maken en installeren](point-to-site-vpn-client-configuration-azure-cert.md)
### [Clientcertificaten installeren](point-to-site-how-to-vpn-client-install-azure-cert.md)

## Point-to-site-verbindingen configureren – RADIUS-verificatie
### Een point-to-site-VPN configureren
#### [Azure PowerShell](point-to-site-how-to-radius-ps.md)
### [VPN-clientconfiguratiebestanden maken en installeren](point-to-site-vpn-client-configuration-radius.md)

## Verbindingen tussen VNet's configureren
### [Azure Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
### [Azure PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
### [Azure CLI](vpn-gateway-howto-vnet-vnet-cli.md)
### [Azure Portal (klassiek)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
## Een VNet-naar-VNet-verbinding tussen implementatiemodellen configureren
### [Azure Portal](vpn-gateway-connect-different-deployment-models-portal.md)
### [Azure PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
## Gelijktijdige site-naar-site- en ExpressRoute-verbindingen configureren
### [Azure PowerShell](../expressroute/expressroute-howto-coexist-resource-manager.md?toc=%2fazure%2fvpn-gateway%2ftoc.json)
## Meerdere site-to-siteverbindingen configureren
### [Azure Portal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
### [Azure PowerShell (klassiek)](vpn-gateway-multi-site.md)
## Meerdere op beleid gebaseerde VPN-apparaten verbinden
### [Azure PowerShell](vpn-gateway-connect-multiple-policybased-rm-ps.md)
## IPSec-/IKE-beleid voor verbindingen configureren
### [Azure PowerShell](vpn-gateway-ipsecikepolicy-rm-powershell.md)
## Maximaal beschikbare actief-actief-verbindingen configureren
### [Azure PowerShell](vpn-gateway-activeactive-rm-powershell.md)
## Een BGP configureren voor een VPN-gateway
### [Azure PowerShell](vpn-gateway-bgp-resource-manager-ps.md)
### [Azure CLI](bgp-how-to-cli.md)
## Geforceerde tunneling configureren
### [Azure PowerShell](vpn-gateway-forced-tunneling-rm.md)
### [Azure PowerShell (klassiek)](vpn-gateway-about-forced-tunneling.md)
## Gateway-instellingen van lokaal netwerk aanpassen
### [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
### [Azure PowerShell](vpn-gateway-modify-local-network-gateway.md)
### [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
## [Een VPN-gatewayverbinding verifiëren](vpn-gateway-verify-connection-resource-manager.md)
## [Een VPN-gateway opnieuw instellen](vpn-gateway-resetgw-classic.md)
## Een VPN-gateway verwijderen
### [Azure Portal](vpn-gateway-delete-vnet-gateway-portal.md)
### [Azure PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
### [Azure PowerShell (klassiek)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
## [Een VPN-gateway configureren (klassiek)](vpn-gateway-configure-vpn-gateway-mp.md)
## [Gateway-SKU's (oud)](vpn-gateway-about-skus-legacy.md)
## VPN-apparaten van derden configureren
### [Overzicht en Azure-configuratie](vpn-gateway-3rdparty-device-config-overview.md)
### [Voorbeeld: Cisco ASA-apparaat (IKEv2/geen BGP)](vpn-gateway-3rdparty-device-config-cisco-asa.md)
## [Migratie van het klassieke model naar het Resource Manager-model](vpn-gateway-classic-resource-manager-migration.md)
## Problemen oplossen
### [VPN-doorvoer naar een VNet valideren](vpn-gateway-validate-throughput-to-vnet.md)
### [Door de community voorgestelde VPN- of firewallinstellingen voor apparaten](vpn-gateway-third-party-settings.md)
### [Probleem met het verbindingstype punt-naar-site](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
### [Site-naar-Site-verbinding wordt herhaaldelijk verbroken](vpn-gateway-troubleshoot-site-to-site-disconnected-intermittently.md)
### [Kan geen Site-naar-Site-verbinding maken](vpn-gateway-troubleshoot-site-to-site-cannot-connect.md) 
### [VNet- of VPN-verbindingen configureren en valideren](https://support.microsoft.com/help/4032151/configuring-and-validating-vnet-or-vpn-connections)

# Naslaginformatie
## [Azure PowerShell](/powershell/module/azurerm.network/?view=azurermps-4.0.0#vpn)
## [Azure PowerShell (klassiek)](/powershell/module/azure/?view=azuresmps-3.7.0#networking)
## [REST](/rest/api/network/virtualnetworkgateways)
## [REST (klassiek)](https://msdn.microsoft.com/library/jj154113)
## [Azure-CLI](/cli/azure/network/vnet-gateway)

# Verwant
## [Virtueel netwerk](/azure/virtual-network/)
## [Application Gateway](/azure/application-gateway/)
## [Azure DNS](/azure/dns/)
## [Traffic Manager](/azure/traffic-manager/)
## [Load balancer](/azure/load-balancer/)
## [ExpressRoute](/azure/expressroute/)

# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=networking)
## [Blog](https://azure.microsoft.com/blog/topics/networking)
## [Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [SLA](https://azure.microsoft.com/support/legal/sla)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=vpn-gateway)
