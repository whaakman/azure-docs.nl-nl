# Overzicht
## [Virtuele netwerken](virtual-networks-overview.md)
## [Routering](virtual-networks-udr-overview.md)
## [Peering op virtueel netwerk](virtual-network-peering-overview.md)
## [Service-eindpunten voor virtueel netwerk](virtual-network-service-endpoints-overview.md)
## [Virtueel netwerk voor Azure-services](virtual-network-for-azure-services.md)
## [Beveiliging](security-overview.md)
## [Containernetwerken](container-networking.md)
## [Bedrijfscontinuïteit](virtual-network-disaster-recovery-guidance.md)
## [IP-adressering](virtual-network-ip-addresses-overview-arm.md)
## [DDoS-beveiliging](ddos-protection-overview.md)
## [Veelgestelde vragen](virtual-networks-faq.md)
## Klassiek
### [IP-adressering](virtual-network-ip-addresses-overview-classic.md)
### [Toegangsbeheerlijsten](virtual-networks-acl.md)

# Aan de slag
## [Uw eerste virtuele netwerk maken](virtual-network-get-started-vnet-subnet.md)

# Procedures
## Plannen en ontwerpen
### [Virtuele netwerken](virtual-network-vnet-plan-design-arm.md)
### [Netwerkbeveiligingsgroepen](virtual-networks-nsg.md)

## Implementeren
### [Virtuele netwerken](virtual-networks-create-vnet-arm-pportal.md)
#### [Azure PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [Azure-CLI](virtual-networks-create-vnet-arm-cli.md)
#### [Sjabloon](virtual-networks-create-vnet-arm-template-click.md)

### Netwerkbeveiligingsgroepen
#### [Azure Portal](virtual-networks-create-nsg-arm-pportal.md)
#### [Azure PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [Azure-CLI](virtual-networks-create-nsg-arm-cli.md)
#### [Sjabloon](virtual-networks-create-nsg-arm-template.md)
#### [Beveiligingsgroepen voor toepassing](create-network-security-group-preview.md)
#### Klassiek
##### [Azure PowerShell](virtual-networks-create-nsg-classic-ps.md)
##### [Azure CLI 1.0](virtual-networks-create-nsg-classic-cli.md)

### Door de gebruiker gedefinieerde routes
#### [Azure Portal](create-user-defined-route-portal.md)
#### [Azure PowerShell](virtual-network-create-udr-arm-ps.md)
#### [Azure-CLI](virtual-network-create-udr-arm-cli.md)
#### [Sjabloon](virtual-network-create-udr-arm-template.md)
#### Klassiek
##### [Azure PowerShell](virtual-network-create-udr-classic-ps.md)
##### [Azure-CLI](virtual-network-create-udr-classic-cli.md)

### Peering op virtueel netwerk
#### [Hetzelfde implementatiemodel - hetzelfde abonnement](virtual-network-create-peering.md)
#### [Hetzelfde implementatiemodel - verschillende abonnementen](create-peering-different-subscriptions.md)
#### [Verschillende implementatiemodellen - hetzelfde abonnement](create-peering-different-deployment-models.md)
#### [Verschillende implementatiemodellen - verschillende abonnementen](create-peering-different-deployment-models-subscriptions.md)

### [Service-eindpunten voor virtueel netwerk](virtual-network-service-endpoints-configure.md)

### Openbaar IP-adres - beschikbaarheidszone
#### [Azure Portal](create-public-ip-availability-zone-portal.md)
#### [Azure-CLI](create-public-ip-availability-zone-cli.md)
#### [PowerShell](create-public-ip-availability-zone-powershell.md)

### Virtuele machines
#### [Netwerkdoorvoer van virtuele machines](virtual-machine-network-throughput.md)
#### Een virtuele machine met een statisch openbaar IP-adres maken
##### [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [Azure PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [Azure-CLI](virtual-network-deploy-static-pip-arm-cli.md)
##### [Sjabloon](virtual-network-deploy-static-pip-arm-template.md)
##### Klassiek
###### [Azure PowerShell](virtual-networks-reserved-public-ip.md)

#### Een virtuele machine maken - statisch privé-IP-adres
##### [Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [Azure PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [Azure-CLI](virtual-networks-static-private-ip-arm-cli.md)
##### Klassiek
###### [Azure Portal](virtual-networks-static-private-ip-classic-pportal.md)
###### [Azure PowerShell](virtual-networks-static-private-ip-classic-ps.md)
###### [Azure-CLI](virtual-networks-static-private-ip-classic-cli.md)

#### Een virtuele machine maken - meerdere netwerkinterfaces
##### [Azure PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Azure-CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Sjabloon](virtual-network-deploy-multinic-arm-template.md)

##### Klassiek
###### [Azure PowerShell](virtual-network-deploy-multinic-classic-ps.md)
###### [Azure-CLI](virtual-network-deploy-multinic-classic-cli.md)

#### Een virtuele machine maken - meerdere IP-adressen
##### [Azure Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [Azure PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [Azure-CLI](virtual-network-multiple-ip-addresses-cli.md)
##### [Sjabloon](virtual-network-multiple-ip-addresses-template.md)

#### Een virtuele machine maken - versnelde netwerken
##### [Azure PowerShell](create-vm-accelerated-networking-powershell.md)
##### [Azure-CLI](create-vm-accelerated-networking-cli.md)

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
##### [Een DMZ met NSG's maken](virtual-networks-dmz-nsg.md)
##### [Een DMZ met NSG's (klassiek) maken](virtual-networks-dmz-nsg-asm.md)
##### [Een DMZ met een firewall en NSG's (klassiek) maken](virtual-networks-dmz-nsg-fw-asm.md)
##### [DMZ met firewall, UDR en NSG's (klassiek)](virtual-networks-dmz-nsg-fw-udr-asm.md)

##### [Voorbeeldtoepassing](virtual-networks-sample-app.md)

### Klassiek
#### [Virtueel netwerk](create-virtual-network-classic.md)
##### [Azure Portal](virtual-networks-create-vnet-classic-pportal.md)
##### [Azure PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)
##### [Azure-CLI](virtual-networks-create-vnet-classic-cli.md)
#### [DNS-instellingen in een virtueel-netwerkconfiguratiebestand opgeven](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
#### [DNS-instellingen in een serviceconfiguratiebestand opgeven](virtual-networks-specifying-dns-settings-in-a-service-configuration-file.md)

## Configureren
### Virtuele machines
#### [Netwerkinterfaces toevoegen of verwijderen](virtual-network-network-interface-vm.md)
#### [Naamomzetting voor virtuele machines en cloudservices](virtual-networks-name-resolution-for-vms-and-role-instances.md)
#### [Dynamische DNS gebruiken om hostnamen te registreren op uw eigen DNS-server](virtual-networks-name-resolution-ddns.md)
#### [Netwerkdoorvoer optimaliseren](virtual-network-optimize-network-bandwidth.md)
#### [Hostnamen weergeven en wijzigen](virtual-networks-viewing-and-modifying-hostnames.md)
#### Klassiek
##### Statische IP-adressen
###### [PowerShell](virtual-networks-reserved-private-ip.md)
###### [CLI](virtual-networks-static-private-ip-classic-cli.md)
##### [Openbare IP-adressen op exemplaarniveau](virtual-networks-instance-level-public-ip.md)

### Klassiek
#### Toegangsbeheerlijsten
##### [Azure Portal](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Azure PowerShell](virtual-networks-acl-powershell.md)

## Beheren
### [Virtuele netwerken](virtual-network-manage-network.md)
#### [Subnets](virtual-network-manage-subnet.md)
#### [Peerings](virtual-network-manage-peering.md)
#### Klassiek
##### [Netwerkconfiguratiebestand](virtual-networks-using-network-configuration-file.md)
##### [Migreren van een affiniteitsgroep naar een regio](virtual-networks-migrate-to-regional-vnet.md)
### Netwerkbeveiligingsgroepen
#### [Azure Portal](virtual-network-manage-nsg-arm-portal.md)
#### [Azure PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [Azure-CLI](virtual-network-manage-nsg-arm-cli.md)

#### [Logboeken](virtual-network-nsg-manage-log.md)
### Netwerkinterfaces (NIC's)
#### [NIC's maken, wijzigen of verwijderen](virtual-network-network-interface.md)
#### [IP-adressen toevoegen, wijzigen of verwijderen](virtual-network-network-interface-addresses.md)
### Virtuele machines
#### [Een VM verplaatsen naar een ander subnet](virtual-networks-move-vm-role-to-subnet.md)
### [Openbare IP-adressen](virtual-network-public-ip-address.md)
### DDoS-beveiliging
#### [Azure Portal](ddos-protection-manage-portal.md)
#### [Azure PowerShell](ddos-protection-manage-ps.md)

## Problemen oplossen
### Netwerkbeveiligingsgroepen
#### [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [Azure PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Routes
#### [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
#### [Azure PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Doorvoer testen](virtual-network-bandwidth-testing.md)
### [Kan virtuele netwerken niet detecteren](virtual-network-troubleshoot-cannot-delete-vnet.md)
### [Problemen met connectiviteit van VM naar VM](virtual-network-troubleshoot-connectivity-problem-between-vms.md)

# Naslaginformatie
## [Codevoorbeelden](https://azure.microsoft.com/en-us/resources/samples/?service=virtual-network)
## [Azure PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [Azure PowerShell (klassiek)](/powershell/module/azure/)
## [Azure-CLI](/cli/azure/network)
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

# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=networking)
## [Netwerkblog](http://azure.microsoft.com/blog/topics/networking)
## [Netwerkforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Prijzen](https://azure.microsoft.com/pricing/details/virtual-network)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
## [Netwerkresourceprovider](resource-groups-networking.md)
