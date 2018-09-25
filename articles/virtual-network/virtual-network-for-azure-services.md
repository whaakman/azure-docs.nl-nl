---
title: Virtueel netwerk voor Azure-services | Microsoft Docs
description: Meer informatie over de voordelen van het implementeren van resources in een virtueel netwerk. Resources in virtuele netwerken met elkaar kunnen communiceren, en on-premises resources, zonder dat verkeer via Internet worden verzonden.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: 06af3351f5669f5cd9aeeb9c4cb2168666476b52
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994629"
---
# <a name="virtual-network-integration-for-azure-services"></a>Integratie van virtuele netwerken voor Azure-services

Integratie van Azure-services met een Azure-netwerk kunt persoonlijke toegang aan de service van virtuele machines of compute-resources in het virtuele netwerk.
U kunt Azure-services integreren in uw virtuele netwerk met de volgende opties: rechtstreeks implementeren van specifieke exemplaren van de service in een virtueel netwerk. De services vervolgens privé toegankelijk zijn vanuit het virtuele netwerk en on-premises netwerken.
Door het uitbreiden van een virtueel netwerk naar de service, via service-eindpunten. Service-eindpunten kunt afzonderlijke-serviceresources worden beveiligd met het virtuele netwerk.

Als u wilt integreren in meerdere Azure-services met het virtuele netwerk, kunt u een of meer van de bovenstaande patronen combineren. U kunt bijvoorbeeld HDInsight implementeren in uw virtuele netwerk en een opslagaccount om het HDInsight-subnet via Service-eindpunten te beveiligen.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Azure-services implementeren in virtuele netwerken

Bij het implementeren van speciale Azure-services in een [virtueel netwerk](virtual-networks-overview.md), kunt u communiceren met de service-resources privé, via privé-IP-adressen.

![Services die zijn geïmplementeerd in een virtueel netwerk](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Implementeren van services binnen een virtueel netwerk biedt de volgende mogelijkheden:

- Resources binnen het virtuele netwerk kunnen communiceren met elkaar privé, via privé-IP-adressen. Bijvoorbeeld rechtstreeks gegevensoverdracht tussen HDInsight en SQL Server wordt uitgevoerd op een virtuele machine, in het virtuele netwerk.
- On-premises resources toegang hebben tot resources in een virtueel netwerk met behulp van privé-IP-adressen via een [Site-naar-Site-VPN (VPN-Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) of [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuele netwerken kunnen zich [gekoppeld](virtual-network-peering-overview.md) zodat resources in de virtuele netwerken om te communiceren met elkaar met behulp van privé-IP-adressen.
- Service-exemplaren in een virtueel netwerk zijn volledig wordt beheerd door de Azure-service, om te controleren van status van de instanties en vereiste schalen, op basis van belasting.
- Service-exemplaren zijn geïmplementeerd in een subnet in een virtueel netwerk. Inkomend en uitgaand netwerkverkeer toegang moet worden geopend via [netwerkbeveiligingsgroepen](security-overview.md#network-security-groups) voor het subnet op basis van de richtlijnen die worden geleverd door de services.
- (Optioneel) services mogelijk een [overgedragen subnet](virtual-network-manage-subnet.md#add-a-subnet) als een expliciete id dat een subnet kan worden gebruikt voor het hosten van een bepaalde service. Subnet delegering biedt expliciete machtigingen voor de service servicespecifieke om resources te maken in het subnet.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Services die kunnen worden geïmplementeerd in een virtueel netwerk

Elke service die rechtstreeks zijn geïmplementeerd in virtuele netwerk heeft specifieke vereisten voor de Routering en de soorten netwerkverkeer naar en van subnetten moeten worden toegestaan. De verschillende services die kunnen worden geïmplementeerd in een virtueel netwerk worden gecategoriseerd hieronder. Selecteer de specifieke service in de tabel voor meer informatie over deze en hoe u deze integreren met uw virtuele netwerk. 


|Categorie|Service|
|-|-|
| Compute | Virtuele machines: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Cloud Service](https://msdn.microsoft.com/library/azure/jj156091): virtuele alleen dit netwerk (klassiek)<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)  |
| Netwerk | [Application Gateway - WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Firewall van Azure](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Virtueel netwerk-Applicances](/windowsserverdocs/WindowsServerDocs/networking/sdn/manage/Use-Network-Virtual-Appliances-on-a-VN.md) 
|Gegevens|[RedisCache](../redis-cache/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Beheerd exemplaar voor Azure SQL Database](../sql-database/sql-database-managed-instance-vnet-configuration.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
Analyse | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Identiteit | [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |
| Containers | [Azure Kubernetes Service (AKS)](../aks/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Container Instance (ACI)](http://www.aka.ms/acivnet)<br/>[Azure Container Service Engine](https://github.com/Azure/acs-engine) met Azure Virtual Network CNI [invoegtoepassing](https://github.com/Azure/acs-engine/tree/master/examples/vnet)||
| Web | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[App Service-omgeving](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Web-Apps](http://www.aka.ms/WebAppsVNet)
| Gehost *| [Azure NetApp-bestanden (basis van bewerkingen)](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure toegewezen Hardware Security Module](http://www.aka.ms/azurededicatedhsm)
| | |
<br/>
* Gespecialiseerde services worden doorgaans uitgevoerd op speciaal ontwikkelde hardware.



## <a name="service-endpoints-for-azure-services"></a>Service-eindpunten voor Azure-services

Sommige Azure-services kunnen niet worden geïmplementeerd in virtuele netwerken. U kunt de toegang beperken tot een aantal van de service-resources naar subnetten van alleen specifieke virtueel netwerk, als u kiest, door in te schakelen van een service-eindpunt voor virtueel netwerk.  Meer informatie over [virtual network-service-eindpunten](virtual-network-service-endpoints-overview.md), en de services die eindpunten kunnen worden ingeschakeld voor.