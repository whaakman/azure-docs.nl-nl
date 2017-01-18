# Overzicht
## [Virtuele netwerken](virtual-networks-overview.md)
## [Door de gebruiker gedefinieerde routes en doorsturen via IP](virtual-networks-udr-overview.md)
## [Peering op virtueel netwerk](virtual-network-peering-overview.md)
## [Bedrijfscontinuïteit](virtual-network-disaster-recovery-guidance.md)
## [Veelgestelde vragen](virtual-networks-faq.md)
## IP-adressering
### [Resource Manager](virtual-network-ip-addresses-overview-arm.md)
### [Klassiek](virtual-network-ip-addresses-overview-classic.md)
## Virtuele machines
### [Netwerkinterfaces](virtual-network-network-interface-overview.md)
### [Naamomzetting](virtual-networks-name-resolution-for-vms-and-role-instances.md)

# Aan de slag
## [Een virtueel netwerk maken](virtual-networks-create-vnet-arm-pportal.md)
## [Een VM implementeren naar een virtueel netwerk](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

# Procedures
## Plannen en ontwerpen
### [Virtuele netwerken](virtual-network-vnet-plan-design-arm.md)
### [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md)

## Implementeren
### Virtuele netwerken
#### [Portal](virtual-networks-create-vnet-arm-pportal.md)
#### [PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [CLI](virtual-networks-create-vnet-arm-cli.md)
#### [Sjabloon](virtual-networks-create-vnet-arm-template-click.md)
#### [Portal (klassiek)](virtual-networks-create-vnet-classic-pportal.md)
#### [PowerShell (klassiek)](virtual-networks-create-vnet-classic-netcfg-ps.md)
#### [CLI (klassiek)](virtual-networks-create-vnet-classic-cli.md)

### Netwerkbeveiligingsgroepen
#### [Portal](virtual-networks-create-nsg-arm-pportal.md)
#### [PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [CLI](virtual-networks-create-nsg-arm-cli.md)
#### [Sjabloon](virtual-networks-create-nsg-arm-template.md)
#### [PowerShell (klassiek)](virtual-networks-create-nsg-classic-ps.md)
#### [CLI (klassiek)](virtual-networks-create-nsg-classic-cli.md)

### Door de gebruiker gedefinieerde routes
#### [PowerShell](virtual-network-create-udr-arm-ps.md)
#### [CLI](virtual-network-create-udr-arm-cli.md)
#### [Sjabloon](virtual-network-create-udr-arm-template.md)
#### [PowerShell (klassiek)](virtual-network-create-udr-classic-ps.md)
#### [CLI (klassiek)](virtual-network-create-udr-classic-cli.md)

### Peering op virtueel netwerk
#### [Portal](virtual-networks-create-vnetpeering-arm-portal.md)
#### [PowerShell](virtual-networks-create-vnetpeering-arm-ps.md)
#### [Sjabloon](virtual-networks-create-vnetpeering-arm-template-click.md)

### Virtuele machines

#### Statische openbare IP-adressen
##### [Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [CLI](virtual-network-deploy-static-pip-arm-cli.md)
##### [Sjabloon](virtual-network-deploy-static-pip-arm-template.md)
##### [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

#### Statische privé IP-adressen
##### [Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [CLI](virtual-networks-static-private-ip-arm-cli.md)
##### [Portal (klassiek)](virtual-networks-static-private-ip-classic-pportal.md)
##### [PowerShell (klassiek)](virtual-networks-static-private-ip-classic-ps.md)
##### [CLI (klassiek)](virtual-networks-static-private-ip-classic-cli.md)

#### Meerdere netwerkinterfaces
##### [PowerShell](virtual-network-deploy-multinic-arm-ps.md)
##### [CLI](virtual-network-deploy-multinic-arm-cli.md)
##### [Sjabloon](virtual-network-deploy-multinic-arm-template.md)
##### [PowerShell (klassiek)](virtual-network-deploy-multinic-classic-ps.md)
##### [CLI (klassiek)](virtual-network-deploy-multinic-classic-cli.md)

#### Meerdere IP-adressen
##### [Azure Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [CLI](virtual-network-multiple-ip-addresses-cli.md)
##### [Sjabloon](virtual-network-multiple-ip-addresses-template.md)

### Connectiviteitsscenario's
#### [Virtueel netwerk (VNet) naar VNet](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNet (Resource Manager) naar een VNet (klassiek)](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNet naar on-premises netwerk (VPN)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [VNet naar on-premises netwerk (ExpressRoute)](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Zeer beschikbare hybride netwerkarchitectuur](../guidance/guidance-hybrid-network-expressroute-vpn-failover.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

### Beveiligingsscenario’s
#### [Netwerken beveiligen met virtuele apparaten](virtual-network-scenario-udr-gw-nva.md)
#### [DMZ tussen Azure en internet](../guidance/guidance-iaas-ra-secure-vnet-dmz.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Cloudservice en netwerkbeveiliging](../best-practices-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Eenvoudige DMZ met NSG’s](virtual-networks-dmz-nsg-asm.md)
##### [DMZ met firewall en NSG’s](virtual-networks-dmz-nsg-fw-asm.md)
##### [DMZ met firewall, UDR en NSG’s](virtual-networks-dmz-nsg-fw-udr-asm.md)
##### [Voorbeeldtoepassing](virtual-networks-sample-app.md)

## Configureren
### Versneld netwerken
#### [Azure Portal](virtual-network-accelerated-networking-portal.md)
#### [PowerShell](virtual-network-accelerated-networking-powershell.md)
### Toegangsbeheerlijsten
#### [Klassieke portal](virtual-networks-acl.md)
#### [PowerShell](virtual-networks-acl-powershell.md)

## Beheren
### Netwerkbeveiligingsgroepen
#### [Portal](virtual-network-manage-nsg-arm-portal.md)
#### [PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [CLI](virtual-network-manage-nsg-arm-cli.md)
#### [Logboeken](virtual-network-nsg-manage-log.md)
#### Problemen oplossen
##### [Portal](virtual-network-nsg-troubleshoot-portal.md)
##### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Problemen met routes oplossen
#### [Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### Virtuele machines
#### [Hostnamen weergeven en wijzigen](virtual-networks-viewing-and-modifying-hostnames.md)
#### [Een VM verplaatsen naar een ander subnet](virtual-networks-move-vm-role-to-subnet.md)

# Naslaginformatie
## [PowerShell (Resource Manager)](https://msdn.microsoft.com/library/mt163510(v=azure.300))
## [PowerShell (klassiek)](https://msdn.microsoft.com/library/mt270335(v=azure.300))
## [Azure CLI](/cli/azure/)
## [Java](/java/api/)
## [REST (Resource Manager)](https://msdn.microsoft.com/library/mt163658.aspx)
## [REST (klassiek)](https://msdn.microsoft.com/library/jj157182.aspx)


# Verwant
## [Virtuele machines](/azure/virtual-machines/)
## [Application Gateway](/azure/application-gateway/)
## [Azure DNS](/azure/dns/)
## [Traffic Manager](/azure/traffic-manager/)
## [Load balancer](/azure/load-balancer/)
## [VPN Gateway](/azure/vpn-gateway/)
## [ExpressRoute](/azure/expressroute/)

# Bronnen
## [Netwerkblog](http://azure.microsoft.com/blog/topics/networking)
## [Netwerkforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Prijzen](https://azure.microsoft.com/pricing/details/virtual-network)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)


<!--HONumber=Jan17_HO3-->


