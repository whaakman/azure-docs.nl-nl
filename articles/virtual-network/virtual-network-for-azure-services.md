---
title: Virtueel netwerk voor Azure-services | Microsoft Docs
description: Meer informatie over de voordelen van het implementeren van resources in een virtueel netwerk. Resources in virtuele netwerken met elkaar kunnen communiceren, en on-premises resources, zonder verkeer vastgelegd dat het Internet.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: 2c3ffb8432fae41b376cc71bb600a0b1c490f345
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="virtual-network-integration-for-azure-services"></a>Integratie van virtueel netwerk voor Azure-services

Integratie van Azure-services tot een virtuele Azure-netwerk, kunt persoonlijke toegang van exemplaren van een service die is geïmplementeerd in het virtuele netwerk.

U kunt Azure-services integreren met het virtuele netwerk met de volgende opties:
- Specifieke exemplaren van de service in een virtueel netwerk rechtstreeks worden geïmplementeerd. De specifieke exemplaren van deze services kunnen afzonderlijk worden benaderd binnen het virtuele netwerk en van on-premises netwerken.
- Door het uitbreiden van een virtueel netwerk met de service via de service-eindpunten. Service-eindpunten kunnen afzonderlijke servicebronnen worden beveiligd met het virtuele netwerk.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Azure-services implementeren in virtuele netwerken

U kunt communiceren met de meeste Azure-resources via het Internet en openbare IP-adressen. Wanneer u Azure-services in implementeert een [virtueel netwerk](virtual-networks-overview.md), kunt u communiceren met de serviceresources privé, via privé IP-adressen.

![Services die zijn geïmplementeerd in een virtueel netwerk](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

De implementatie van services in een virtueel netwerk biedt de volgende mogelijkheden:

- Resources binnen het virtuele netwerk kunnen communiceren met elkaar privé, via privé IP-adressen. Bijvoorbeeld rechtstreeks gegevensoverdracht tussen HDInsight en SQL Server wordt uitgevoerd op een virtuele machine, in het virtuele netwerk.
- Lokale bronnen kunnen krijgen tot bronnen in een virtueel netwerk met particuliere IP-adressen via een [Site-naar-Site-VPN (VPN-Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) of [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuele netwerken kunnen zich [brengen](virtual-network-peering-overview.md) zodat bronnen in de virtuele netwerken met elkaar communiceren met particuliere IP-adressen.
- Service-exemplaren in een virtueel netwerk worden volledig beheerd door de Azure-service voor het bewaken van status van de exemplaren en vereiste schaal, op basis van load bieden.
- Service-exemplaren worden geïmplementeerd in een specifieke subnet in een virtueel netwerk. Binnenkomende en uitgaande toegang moet worden geopend via [netwerkbeveiligingsgroepen](security-overview.md#network-security-groups) voor het subnet van de basis van de richtlijnen die worden geleverd door de services.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Services die kunnen worden geïmplementeerd in een virtueel netwerk

Elke service rechtstreeks worden geïmplementeerd in virtueel netwerk heeft specifieke vereisten voor Routering en de soorten netwerkverkeer naar en van subnetten moet worden toegestaan. Zie voor meer informatie: 
 
- Virtuele machines: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Het service fabric](../service-fabric/service-fabric-patterns-networking.md?toc=%2fazure%2fvirtual-network%2ftoc.json#existingvnet)
- [Virtuele-machineschaalsets](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [App Service-omgeving](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [RedisCache](../redis-cache/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Toepassingsgateway (intern)](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Container Service-Engine](../container-service/container-service-intro.md?toc=%2fazure%2fvirtual-network%2ftoc.json): de Azure Container Service maakt een standaard virtueel netwerk. Kunt u een aangepaste virtueel netwerk gebruiken met de [Azure Container Service-Engine](https://github.com/Azure/acs-engine/tree/master/examples/vnet).
- [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json): virtuele alleen dit netwerk (klassiek)
- [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration): virtuele alleen dit netwerk (klassiek)
- [Cloudservices](https://msdn.microsoft.com/library/azure/jj156091): virtuele alleen dit netwerk (klassiek)

U kunt een [interne Azure load balancer](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) laden saldo veel van de resources in de vorige lijst. In sommige gevallen kan de service automatisch maakt en implementeert een load balancer, wanneer u een resource maakt.

## <a name="service-endpoints-for-azure-services"></a>Service-eindpunten voor Azure-services

Sommige Azure-services kunnen niet worden geïmplementeerd in virtuele netwerken. U kunt de toegang beperken tot een aantal van de serviceresources met subnetten van alleen specifieke virtueel netwerk, als u kiest, doordat de service-eindpunt van een virtueel netwerk. Meer informatie over [virtueel netwerk service-eindpunten](virtual-network-service-endpoints-overview.md).

Service-eindpunten worden momenteel ondersteund voor de volgende services: 
- **Azure Storage**: [beveiligen van Azure Storage-accounts naar virtuele netwerken](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- **Azure SQL Database**: [Azure SQL-Database beveiligen met virtuele netwerken](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="virtual-network-integration-across-multiple-azure-services"></a>Integratie van virtueel netwerk op meerdere Azure-services

U kunt een Azure-service implementeren in een subnet in een virtueel netwerk en beveiligde kritieke serviceresources op dat subnet. U kunt bijvoorbeeld HDInsight implementeren in uw virtueel netwerk en beveiligen van een opslagaccount met het HDInsight-subnet.





