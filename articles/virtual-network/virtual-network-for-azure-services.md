---
title: Virtueel netwerk voor Azure-services
titlesuffix: Azure Virtual Network
description: Meer informatie over de voordelen van het implementeren van resources in een virtueel netwerk. Resources in virtuele netwerken met elkaar kunnen communiceren, en on-premises resources, zonder dat verkeer via Internet worden verzonden.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: malop;kumud
ms.openlocfilehash: e5481b0e262021e28a398b72b5ad022673947609
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65409513"
---
# <a name="virtual-network-integration-for-azure-services"></a>Integratie van virtuele netwerken voor Azure-services

Integratie van Azure-services met een Azure-netwerk kunt persoonlijke toegang aan de service van virtuele machines of compute-resources in het virtuele netwerk.
U kunt Azure-services integreren in uw virtuele netwerk met de volgende opties:
- Toegewezen instanties van de service in een virtueel netwerk implementeren. De services vervolgens privé toegankelijk zijn vanuit het virtuele netwerk en on-premises netwerken.
- Uitbreiden van een virtueel netwerk naar de service, via service-eindpunten. Service-eindpunten kunt afzonderlijke-serviceresources worden beveiligd met het virtuele netwerk.

Als u wilt integreren in meerdere Azure-services met het virtuele netwerk, kunt u een of meer van de bovenstaande patronen combineren. U kunt bijvoorbeeld HDInsight implementeren in uw virtuele netwerk en een opslagaccount om het HDInsight-subnet via Service-eindpunten te beveiligen.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Azure-services implementeren in virtuele netwerken

Bij het implementeren van speciale Azure-services in een [virtueel netwerk](virtual-networks-overview.md), kunt u communiceren met de service-resources privé, via privé-IP-adressen.

![Services die zijn geïmplementeerd in een virtueel netwerk](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Implementeren van services binnen een virtueel netwerk biedt de volgende mogelijkheden:

- Resources binnen het virtuele netwerk kunnen communiceren met elkaar privé, via privé-IP-adressen. Bijvoorbeeld rechtstreeks gegevensoverdracht tussen HDInsight en SQL Server wordt uitgevoerd op een virtuele machine, in het virtuele netwerk.
- On-premises resources toegang hebben tot resources in een virtueel netwerk met behulp van privé-IP-adressen via een [Site-naar-Site-VPN (VPN-Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) of [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuele netwerken kunnen zich [gekoppeld](virtual-network-peering-overview.md) zodat resources in de virtuele netwerken om te communiceren met elkaar met behulp van privé-IP-adressen.
- Service-exemplaren in een virtueel netwerk worden doorgaans volledig beheerd door de Azure-service. Dit omvat de status van de resources bewaken en schalen met een belasting.
- Service-exemplaren zijn geïmplementeerd in een subnet in een virtueel netwerk. Inkomende en uitgaande toegang tot het subnet moet worden geopend via [netwerkbeveiligingsgroepen](security-overview.md#network-security-groups), basis van de richtlijnen die worden geleverd door de service.
- Bepaalde services ook leggen beperkingen op aan het subnet dat ze zijn geïmplementeerd in de toepassing van beleid, routes of het combineren van VM's en serviceresources binnen hetzelfde subnet te beperken. Neem contact op met elke service op de specifieke beperkingen wanneer ze na verloop van tijd kunnen veranderen. Voorbeelden van dergelijke services zijn Azure-bestanden voor NetApp, toegewezen HSM, Azure Container Instances, App Service. 
- (Optioneel) services mogelijk een [overgedragen subnet](virtual-network-manage-subnet.md#add-a-subnet) als een expliciete id dat een subnet kan worden gebruikt voor het hosten van een bepaalde service. Door over te dragen, krijgen expliciete machtigingen voor het maken van service-specifieke resources in de gedelegeerde subnet.
- Bekijk een voorbeeld van een REST-API-reactie op een [virtueel netwerk met een gedelegeerde subnet](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/get#get_virtual_network_with_a_delegated_subnet). Een uitgebreide lijst met services die van het model gedelegeerde subnet gebruikmaken kan worden verkregen via de [beschikbaar overdrachten](https://docs.microsoft.com/rest/api/virtualnetwork/availabledelegations/list) API.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Services die kunnen worden geïmplementeerd in een virtueel netwerk

|Category|Service| Dedicated¹ Subnet
|-|-|-|
| Compute | Virtuele machines: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Cloud Service](https://msdn.microsoft.com/library/azure/jj156091): Virtuele alleen dit netwerk (klassiek)<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)| Nee <br/> Nee <br/> Nee <br/> No²
| Netwerk | [Application Gateway - WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Firewall](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Virtuele netwerkapparaten](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) | Ja <br/> Ja <br/> Ja <br/> Nee
|Gegevens|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Beheerd exemplaar voor Azure SQL Database](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fazure%2fvirtual-network%2ftoc.json)| Ja <br/> Ja <br/> 
|Analytische gegevens | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |No² <br/> No² <br/> 
| Identiteit | [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |Nee <br/>
| Containers | [Azure Kubernetes Service (AKS)](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Container Instance (ACI)](https://www.aka.ms/acivnet)<br/>[Azure Container Service Engine](https://github.com/Azure/acs-engine) met Azure Virtual Network CNI [invoegtoepassing](https://github.com/Azure/acs-engine/tree/master/examples/vnet)|No²<br/> Ja <br/><br/> Nee
| Internet | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[App Service-omgeving](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Ja <br/> Ja <br/> Ja
| Gehost | [Azure toegewezen HSM](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Ja <br/> Ja <br/>
| | |

¹ Dedicated impliceert dat alleen bepaalde-serviceresources in dit subnet kunnen worden geïmplementeerd en kunnen niet worden gecombineerd met de klant virtuele machine/VMSSs <br/> ² aanbevolen, maar niet verplicht die zijn opgelegd door de service.


## <a name="service-endpoints-for-azure-services"></a>Service-eindpunten voor Azure-services

Sommige Azure-services kunnen niet worden geïmplementeerd in virtuele netwerken. U kunt de toegang beperken tot een aantal van de service-resources naar subnetten van alleen specifieke virtueel netwerk, als u kiest, door in te schakelen van een service-eindpunt voor virtueel netwerk.  Meer informatie over [virtual network-service-eindpunten](virtual-network-service-endpoints-overview.md), en de services die eindpunten kunnen worden ingeschakeld voor.
