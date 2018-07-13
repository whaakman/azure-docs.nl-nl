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
ms.openlocfilehash: 3e31dbce7bd24b3c3bb0f24561464e6303f3908e
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990593"
---
# <a name="virtual-network-integration-for-azure-services"></a>Integratie van virtuele netwerken voor Azure-services

Integratie van Azure-services met een Azure-netwerk kan persoonlijke toegang van exemplaren van een service die is geïmplementeerd in het virtuele netwerk.

U kunt Azure-services integreren met uw virtuele netwerk met de volgende opties:
- Rechtstreeks implementeren van specifieke exemplaren van de service in een virtueel netwerk. De specifieke exemplaren van deze services zijn privé toegankelijk vanuit het virtuele netwerk en on-premises netwerken.
- Door het uitbreiden van een virtueel netwerk naar de service, via service-eindpunten. Service-eindpunten kunt afzonderlijke-serviceresources worden beveiligd met het virtuele netwerk.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Azure-services implementeren in virtuele netwerken

U kunt met de meeste Azure-resources communiceren via het Internet en openbare IP-adressen. Bij het implementeren van Azure-services in een [virtueel netwerk](virtual-networks-overview.md), kunt u communiceren met de service-resources privé, via privé-IP-adressen.

![Services die zijn geïmplementeerd in een virtueel netwerk](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Implementeren van services binnen een virtueel netwerk biedt de volgende mogelijkheden:

- Resources binnen het virtuele netwerk kunnen communiceren met elkaar privé, via privé-IP-adressen. Bijvoorbeeld rechtstreeks gegevensoverdracht tussen HDInsight en SQL Server wordt uitgevoerd op een virtuele machine, in het virtuele netwerk.
- On-premises resources toegang hebben tot resources in een virtueel netwerk met behulp van privé-IP-adressen via een [Site-naar-Site-VPN (VPN-Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) of [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuele netwerken kunnen zich [gekoppeld](virtual-network-peering-overview.md) zodat resources in de virtuele netwerken om te communiceren met elkaar met behulp van privé-IP-adressen.
- Service-exemplaren in een virtueel netwerk zijn volledig wordt beheerd door de Azure-service, om te controleren van status van de instanties en vereiste schalen, op basis van belasting.
- Service-exemplaren zijn geïmplementeerd in een toegewezen subnet in een virtueel netwerk. Inkomend en uitgaand netwerkverkeer toegang moet worden geopend via [netwerkbeveiligingsgroepen](security-overview.md#network-security-groups) voor het subnet op basis van de richtlijnen die worden geleverd door de services.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Services die kunnen worden geïmplementeerd in een virtueel netwerk

Elke service die rechtstreeks zijn geïmplementeerd in virtuele netwerk heeft specifieke vereisten voor de Routering en de soorten netwerkverkeer naar en van subnetten moeten worden toegestaan. Zie voor meer informatie: 
 
- Virtuele machines: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Service fabric](../service-fabric/service-fabric-patterns-networking.md?toc=%2fazure%2fvirtual-network%2ftoc.json#existingvnet)
- [Virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [App Service-omgeving](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [RedisCache](../redis-cache/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Application Gateway (intern)](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Kubernetes Service (AKS)](../aks/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Container Service Engine](https://github.com/Azure/acs-engine) met de Azure Virtual Network CNI [invoegtoepassing](https://github.com/Azure/acs-engine/tree/master/examples/vnet)
- [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)
- [Cloudservices](https://msdn.microsoft.com/library/azure/jj156091): virtuele alleen dit netwerk (klassiek)

U kunt implementeren een [interne Azure load balancer](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) laden saldo veel van de resources in de vorige lijst. In sommige gevallen kan de service automatisch maakt en implementeert een load balancer, wanneer u een resource maken.

## <a name="service-endpoints-for-azure-services"></a>Service-eindpunten voor Azure-services

Sommige Azure-services kunnen niet worden geïmplementeerd in virtuele netwerken. U kunt de toegang beperken tot een aantal van de service-resources naar subnetten van alleen specifieke virtueel netwerk, als u kiest, door in te schakelen van een service-eindpunt voor virtueel netwerk. Meer informatie over [virtual network-service-eindpunten](virtual-network-service-endpoints-overview.md), en de services die eindpunten kunnen worden ingeschakeld voor.

## <a name="virtual-network-integration-across-multiple-azure-services"></a>Integratie van virtuele netwerken tussen meerdere Azure-services

U kunt een Azure-service implementeren in een subnet in een virtueel netwerk en beveiligde voor kritieke serviceresources op dat subnet. U kunt bijvoorbeeld HDInsight implementeren in uw virtuele netwerk en een opslagaccount naar het HDInsight-subnet beveiligen.





