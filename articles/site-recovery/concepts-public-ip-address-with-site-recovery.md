---
title: Openbare IP-adressen gebruiken na een failover met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe u voor het instellen van openbare IP-adressen met Azure Site Recovery en Azure Traffic Manager voor de migratie en herstel na noodgevallen
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: eaf18cf7d7a5f150d3400e133bf0703bb03e7b8f
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52845974"
---
# <a name="set-up-public-ip-addresses-after-failover"></a>Openbare IP-adressen instellen na een failover

Door openbare IP-adressen te gebruiken, zijn internetbronnen in staat inkomende communicatie voor Azure-resources te verwerken. Openbare IP-adressen maken het ook mogelijk dat Azure-resources uitgaande communicatie naar internet en openbare Azure-services kunnen afhandelen via een IP-adres dat is toegewezen aan de resource.
- Binnenkomende communicatie via Internet tot de resource, zoals Azure Virtual Machines (VM), Application Gateways voor Azure, Azure Load Balancers, Azure VPN-Gateways en anderen. U kunt nog steeds met de communiceren met enkele resources, zoals virtuele machines, via Internet, als een virtuele machine geen openbaar IP-adres toegewezen, als de virtuele machine deel uit van een back-end-pool van load balancer maakt, en de load balancer een openbaar IP-adres wordt toegewezen.
- Uitgaande verbinding met Internet via een voorspelbare IP-adres. Een virtuele machine kan bijvoorbeeld communiceren uitgaand naar het Internet zonder een openbaar IP-adres toegewezen aan deze, maar het adres is vertaald door Azure op een onvoorspelbare openbare adres, standaard-netwerkadres. Een openbaar IP-adres toewijzen aan een resource, kunt u weten welk IP-adres wordt gebruikt voor de uitgaande verbindingen. Hoewel voorspelbare, kunt het adres wijzigen, afhankelijk van de gekozen methode voor het toewijzen. Zie voor meer informatie, [een openbaar IP-adres maken](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address). Zie voor meer informatie over uitgaande verbindingen van Azure-resources, [uitleg over uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

In Azure Resource Manager, een openbaar IP-adres is een resource die zijn eigen eigenschappen heeft. U kunt sommige resources aan een openbare IP-adresresource koppelen, zoals:

* Netwerkinterfaces van virtuele machines
* Internetgerichte load balancers
* VPN-gateways
* Toepassingsgateways

Dit artikel wordt beschreven hoe u openbare IP-adressen met Site Recovery kunt gebruiken.

## <a name="public-ip-address-assignment-using-recovery-plan"></a>Openbare IP-adrestoewijzing met behulp van het herstelplan

Openbare IP-adres van de productietoepassing **kunnen niet worden behouden bij failover**. Workloads naar gebracht als onderdeel van failover-proces moet een openbaar IP-Azure-resource beschikbaar in de doelregio worden toegewezen. Deze stap kan worden gedaan handmatig of met herstelplannen is geautomatiseerd. Een herstelplan verzamelt machines in herstelgroepen. Hiermee kunt u voor het definiëren van een systematische herstelproces. U kunt een plan voor herstel gebruiken om op te leggen volgorde, en de acties die nodig zijn bij elke stap automatiseren met behulp van Azure Automation-runbooks voor failover naar Azure of scripts.

De installatie is als volgt:
- Maak een [herstelplan](../site-recovery/site-recovery-create-recovery-plans.md#create-a-recovery-plan) en groeperen van uw workloads zo nodig in het plan.
- Het plan aanpassen door het toevoegen van een stap voor het koppelen van een openbare IP-adres met [Azure Automation-runbooks](../site-recovery/site-recovery-runbook-automation.md#customize-the-recovery-plan) scripts na een failover via virtuele machine.

 
## <a name="public-endpoint-switching-with-dns-level-routing"></a>Openbaar eindpunt overschakelen met DNS-niveau routering

Met Azure Traffic Manager kunnen DNS-niveau routering tussen de eindpunten en kunnen u helpen bij [waardoor uw RTO's](../site-recovery/concepts-traffic-manager-with-site-recovery.md#recovery-time-objective-rto-considerations) voor herstel na noodgevallen. 

Meer informatie over failover-scenario's met Traffic Manager:
1. [On-premises naar Azure failover](../site-recovery/concepts-traffic-manager-with-site-recovery.md#on-premises-to-azure-failover) met Traffic Manager 
2. [Azure naar Azure failover](../site-recovery/concepts-traffic-manager-with-site-recovery.md#azure-to-azure-failover) met Traffic Manager 

De installatie is als volgt:
- Maak een [Traffic Manager-profiel](../traffic-manager/traffic-manager-create-profile.md).
- Met behulp van de **prioriteit** routeringsmethode maakt twee eindpunten: **primaire** voor bron en **Failover** voor Azure. **Primaire** prioriteit 1 is toegewezen en **Failover** prioriteit 2 is toegewezen.
- De **primaire** eindpunt mag [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) of [externe](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) afhankelijk van of uw bronomgeving binnen of buiten Azure.
- De **Failover** -eindpunt is gemaakt als een **Azure** eindpunt. Gebruik een **statisch openbaar IP-adres** zoals dit gestart worden externe internetgerichte eindpunt voor Traffic Manager in de gebeurtenis na noodgevallen.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Traffic Manager met Azure Site Recovery](../site-recovery/concepts-traffic-manager-with-site-recovery.md)
- Meer informatie over Traffic Manager [routeringsmethoden](../traffic-manager/traffic-manager-routing-methods.md).
- Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md) toepassing failover wilt automatiseren.
