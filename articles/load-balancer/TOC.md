# [Documentatie voor Load Balancer](index.md)

# Overzicht
## [Wat is Load Balancer?](load-balancer-overview.md)
## [Wat is Load Balancer Standard?](load-balancer-standard-overview.md)
## [Load balancer-tests](load-balancer-custom-probe-overview.md)
## [Poorten met hoge beschikbaarheid](load-balancer-ha-ports-overview.md)
## [IPv6-ondersteuning](load-balancer-ipv6-overview.md)
## [Meerdere frontends](load-balancer-multivip-overview.md)
## [Uitgaande verbindingen](load-balancer-outbound-connections.md)
## [De standaardversie van Load Balancer en beschikbaarheidszones](load-balancer-standard-availability-zones.md)
## [Metrische en diagnostische gegevens voor een load balancer van het type Standard](load-balancer-standard-diagnostics.md)

# Aan de slag
## [Een load balancer van het type Basic maken](quickstart-create-basic-load-balancer-portal.md)
### [Een load balancer van het type Basic maken (CLI)](quickstart-create-basic-load-balancer-cli.md)
### [Een load balancer van het type Basic maken (PowerShell)](quickstart-create-basic-load-balancer-powershell.md)
## [Een load balancer van het type Standard maken](quickstart-load-balancer-standard-public-portal.md)
### [Een load balancer van het type Standard maken (CLI)](quickstart-load-balancer-standard-public-cli.md)

# Procedures

## [Een zoneredundante openbare load balancer van het type Standard maken](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
### [Een zoneredundante openbare load balancer van het type Standard maken (PowerShell)](load-balancer-get-started-internet-az-powershell.md)
### [Een zoneredundante openbare load balancer van het type Standard maken (CLI)](load-balancer-get-started-internet-az-cli.md)
## [Een zonegebonden openbare load balancer van het type Standard maken](load-balancer-get-started-internet-availability-zones-zonal-portal.md)
### [Een zonegebonden openbare load balancer van het type Standard maken (PowerShell)](load-balancer-get-started-internet-availability-zones-zonal-powershell.md)
### [Een zonegebonden openbare load balancer van het type Standard maken (CLI)](load-balancer-get-started-internet-availability-zones-zonal-cli.md)
## [Taken over VM's in meerdere beschikbaarheidszones verdelen](load-balancer-standard-public-availability-zones-portal.md)
###  [Taken over VM's in meerdere beschikbaarheidszones verdelen (CLI)](load-balancer-standard-public-zone-redundant-cli.md)
##  [Taken over VM's binnen een zone verdelen (CLI)](load-balancer-standard-public-zonal-cli.md)   
## [Een load balancer van het type Basic maken (sjabloon)](load-balancer-get-started-internet-arm-template.md)
## [Een openbare load balancer maken met IPv6](load-balancer-ipv6-internet-ps.md)
### [Een openbare load balancer maken met IPv6 (CLI)](load-balancer-ipv6-internet-cli.md)
### [Een openbare load balancer maken met IPv6 (sjabloon)](load-balancer-ipv6-internet-template.md)
## [Een interne load balancer configureren](load-balancer-get-started-ilb-arm-portal.md)
### [Een interne load balancer configureren (PowerShell)](load-balancer-get-started-ilb-arm-ps.md)
### [Een interne load balancer configureren (CLI)](load-balancer-get-started-ilb-arm-cli.md)
### [Een interne load balancer configureren (sjabloon)](load-balancer-get-started-ilb-arm-template.md)
## [Time-out voor TCP-inactiviteit configureren voor load balancer](load-balancer-tcp-idle-timeout.md)
## [Distributiemodus configureren voor load balancer](load-balancer-distribution-mode.md)
## [Taakverdelingsservices combineren](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fazure%2fload-balancer%2ftoc.json)
## [Meerdere IP-configuraties gebruiken](load-balancer-multiple-ip.md)
### [Meerdere IP-configuraties gebruiken (CLI)](load-balancer-multiple-ip-cli.md)
### [Meerdere IP-configuraties gebruiken (PowerShell)](load-balancer-multiple-ip-powershell.md)
## [Logboekanalyse voor Azure Load Balancer](load-balancer-monitor-log.md)
## [Poorten voor hoge beschikbaarheid configureren voor de interne Load Balancer](load-balancer-configure-ha-ports.md)

## Problemen oplossen
### [Problemen met Azure Load Balancer oplossen](load-balancer-troubleshoot.md)

## Artikelen over het klassieke implementatiemodel
### [Uitgaande verbindingen (klassiek)](load-balancer-outbound-connections-classic.md)
## [Meerdere VIP's configureren voor cloudservice](load-balancer-multivip.md)
### [Een interne load balancer configureren voor cloudservices](load-balancer-get-started-ilb-classic-cloud.md)
#### [Een interne load balancer configureren voor cloudservices (PowerShell)](load-balancer-get-started-ilb-classic-ps.md)
#### [Een interne load balancer configureren voor cloudservices (CLI)](load-balancer-get-started-ilb-classic-cli.md)
### [Een openbare load balancer configureren (klassieke PowerShell)](load-balancer-get-started-internet-classic-ps.md)
#### [Een openbare load balancer configureren (klassieke cloud)](load-balancer-get-started-internet-classic-cloud.md)
#### [Een openbare load balancer configureren (klassieke CLI)](load-balancer-get-started-internet-classic-cli.md)

# Naslaginformatie
## [Codevoorbeelden](https://azure.microsoft.com/en-us/resources/samples/?service=load-balancer)
## [Azure PowerShell](/powershell/module/azurerm.network)
## [Azure CLI](/cli/azure/network/lb)
## [.NET](/dotnet/api/microsoft.azure.management.network.models)
## [Java](/java/api/com.microsoft.azure.management.network)
## [Node.js](http://azure.github.io/azure-sdk-for-node/azure-arm-network/latest/LoadBalancers.html)
## [Ruby](http://www.rubydoc.info/gems/azure_mgmt_network/Azure/ARM/Network/LoadBalancers)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.mgmt.network.operations.html#azure.mgmt.network.operations.LoadBalancersOperations)
## [REST](https://msdn.microsoft.com/library/azure/mt163651.aspx)

# Verwant
## [Application Gateway](/azure/application-gateway/)
## [ExpressRoute](/azure/expressroute/)
## [Virtueel netwerk](/azure/virtual-network/)
## [VPN Gateway](/azure/vpn-gateway/)
## [Virtuele machine](/azure/virtual-machines/)
## [Traffic Manager](/azure/traffic-manager/)
## [DNS](/azure/dns/)

# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=networking)
## [Prijzen](https://azure.microsoft.com/pricing/details/load-balancer/)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Service-updates](https://azure.microsoft.com/updates/?product=load-balancer)
