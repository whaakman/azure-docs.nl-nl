# Overzicht
## [Virtuele netwerken](virtual-networks-overview.md)
## [Door de gebruiker gedefinieerde routes en doorsturen via IP](virtual-networks-udr-overview.md)
## [Peering op virtueel netwerk](virtual-network-peering-overview.md)
## [Bedrijfscontinuïteit](virtual-network-disaster-recovery-guidance.md)
## [Veelgestelde vragen](virtual-networks-faq.md)
## IP-adressering
### [Resource Manager](virtual-network-ip-addresses-overview-arm.md)
### [Klassiek](virtual-network-ip-addresses-overview-classic.md)

# Aan de slag
## [Uw eerste virtuele netwerk maken](virtual-network-get-started-vnet-subnet.md)

# Procedures
## Plannen en ontwerpen
### [Virtuele netwerken](virtual-network-vnet-plan-design-arm.md)
### [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md)

## Implementeren
### [Virtuele netwerken](virtual-networks-create-vnet-arm-pportal.md)
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

### [Peering op virtueel netwerk](virtual-network-create-peering.md)

### Virtuele machines
#### Een virtuele machine met een statisch openbaar IP-adres maken
##### [Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [CLI](virtual-network-deploy-static-pip-arm-cli.md)
##### [Sjabloon](virtual-network-deploy-static-pip-arm-template.md)
##### [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

#### Een virtuele machine met een statisch privé-IP-adres maken
##### [Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [CLI](virtual-networks-static-private-ip-arm-cli.md)
##### [Portal (klassiek)](virtual-networks-static-private-ip-classic-pportal.md)
##### [PowerShell (klassiek)](virtual-networks-static-private-ip-classic-ps.md)
##### [CLI (klassiek)](virtual-networks-static-private-ip-classic-cli.md)

#### Een virtuele machine met meerdere netwerkinterfaces maken
##### [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [PowerShell (klassiek)](virtual-network-deploy-multinic-classic-ps.md)
##### [CLI (klassiek)](virtual-network-deploy-multinic-classic-cli.md)

#### Een virtuele machine met meerdere IP-adressen maken
##### [Azure Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [CLI](virtual-network-multiple-ip-addresses-cli.md)
##### [Sjabloon](virtual-network-multiple-ip-addresses-template.md)

#### [Een virtuele machine maken met versneld netwerken](virtual-network-create-vm-accelerated-networking.md)

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

### Toegangsbeheerlijsten
#### [Klassieke portal](virtual-networks-acl.md)
#### [PowerShell](virtual-networks-acl-powershell.md)
### Virtuele machines
#### [Netwerkinterfaces toevoegen of verwijderen](virtual-network-network-interface-vm.md)
#### [Naamomzetting voor virtuele machines en cloudservices](virtual-networks-name-resolution-for-vms-and-role-instances.md)
#### [Netwerkdoorvoer optimaliseren](virtual-network-optimize-network-bandwidth.md)
#### [Hostnamen weergeven en wijzigen](virtual-networks-viewing-and-modifying-hostnames.md)

## Beheren
### [Virtuele netwerken](virtual-network-manage-network.md)
#### [Subnets](virtual-network-manage-subnet.md)
#### [Peerings](virtual-network-manage-peering.md)
### Netwerkbeveiligingsgroepen
#### [Portal](virtual-network-manage-nsg-arm-portal.md)
#### [PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [CLI](virtual-network-manage-nsg-arm-cli.md)
#### [Logboeken](virtual-network-nsg-manage-log.md)
### Netwerkinterfaces (NIC's)
#### [NIC's maken, wijzigen of verwijderen](virtual-network-network-interface.md)
#### [IP-adressen toevoegen, wijzigen of verwijderen](virtual-network-network-interface-addresses.md)
### Virtuele machines
#### [Een VM verplaatsen naar een ander subnet](virtual-networks-move-vm-role-to-subnet.md)
### [Openbare IP-adressen](virtual-network-public-ip-address.md)

## Problemen oplossen
### Netwerkbeveiligingsgroepen
#### [Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Routes
#### [Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Doorvoer testen](virtual-network-bandwidth-testing.md)

# Naslaginformatie
## [PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [PowerShell (klassiek)](/powershell/module/azure/?view=azuresmps-3.7.0)
## [Azure CLI](/cli/azure/network)
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
