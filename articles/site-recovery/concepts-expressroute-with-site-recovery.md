---
title: Over het gebruik van Azure ExpressRoute met Azure Site Recovery voor migratie en herstel na noodgevallen | Microsoft Docs
description: Beschrijft hoe u Azure ExpressRoute gebruiken met de Azure Site Recovery-service voor de migratie en herstel na noodgevallen.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: mayg
ms.openlocfilehash: 14d74c879ee37306f541c89d2ba325109a4ff7f3
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50214865"
---
# <a name="azure-expressroute-with-azure-site-recovery"></a>Met Azure ExpressRoute met Azure Site Recovery

Met Microsoft Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider. Met ExpressRoute kunt u verbindingen tot stand brengen met Microsoft Cloud-services, zoals Microsoft Azure, Office 365 en Dynamics 365.

Dit artikel wordt beschreven hoe u Azure ExpressRoute kunt gebruiken met Azure Site Recovery voor migratie en herstel na noodgevallen.

## <a name="expressroute-circuits"></a>ExpressRoute-circuits

Een ExpressRoute-circuit vertegenwoordigt een logische verbinding tussen uw on-premises infrastructuur en Microsoft cloud-services via een connectiviteitsprovider. U kunt meerdere ExpressRoute-circuits bestellen. Elk circuit kan in de dezelfde of verschillende regio's, en kan worden verbonden met uw locatie via verschillende connectiviteitsproviders. Meer informatie over ExpressRoute-circuits [hier](../expressroute/expressroute-circuit-peerings.md).

## <a name="expressroute-routing-domains"></a>ExpressRoute-routeringsdomeinen

Een ExpressRoute-circuit heeft meerdere Routeringsdomeinen gekoppeld:
-   [Persoonlijke Azure-peering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering) : Azure compute-services, namelijk virtuele machines (IaaS), en cloudservices (PaaS) die zijn geïmplementeerd in een virtueel netwerk kunnen worden verbonden via het domein voor persoonlijke peering. Het domein voor persoonlijke peering wordt beschouwd als een vertrouwde uitbreiding van uw Basisnetwerk in Microsoft Azure.
-   [Openbare Azure-peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) -Services zoals Azure Storage, SQL-databases en Websites zijn beschikbaar voor openbare IP-adressen. U kunt privé verbinding maken met services die worden gehost op openbare IP-adressen, met inbegrip van VIP's van uw cloudservices, via de openbare peering routeringsdomein. Openbare peering is afgeschaft voor nieuwe bewerkingen voor het maken en Microsoft-Peering in plaats daarvan moet worden gebruikt voor Azure PaaS-services.
-   [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoft-peering) -connectiviteit met Microsoft online services (Office 365, Dynamics 365 en Azure PaaS-services), is via de Microsoft-peering. Microsoft-peering is de aanbevolen routeringsdomein verbinding maken met Azure PaaS-services.

Meer informatie over en vergelijken van ExpressRoute-Routeringsdomeinen [hier](../expressroute/expressroute-circuit-peerings.md#routing-domain-comparison).

## <a name="on-premises-to-azure-replication-with-expressroute"></a>On-premises naar Azure-replicatie met ExpressRoute

Azure Site Recovery maakt herstel na noodgevallen en migratie naar Azure voor on-premises [Hyper-V virtuele machines](hyper-v-azure-architecture.md), [virtuele VMware-machines](vmware-azure-architecture.md), en [fysieke servers](physical-azure-architecture.md). Replicatiegegevens worden voor alle on-premises naar Azure-scenario's, verzonden naar en opgeslagen in een Azure Storage-account. Tijdens de replicatie betaalt u geen kosten voor elke virtuele machine. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch virtuele machines van Azure IaaS.

Site Recovery repliceert gegevens naar een Azure Storage-account via een openbaar eindpunt. U kunt gebruiken voor het gebruik van ExpressRoute voor Site Recovery-replicatie, [openbare peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) of [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoft-peering). Microsoft-peering is de aanbevolen routeringsdomein voor replicatie. Nadat de virtuele machines of servers een failover uitvoeren naar een Azure-netwerk, kunt u ze openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering). Replicatie wordt niet ondersteund via persoonlijke peering.

De gecombineerde scenario wordt weergegeven in het volgende diagram: ![On-premises-to-Azure met ExpressRoute](./media/concepts-expressroute-with-site-recovery/site-recovery-with-expressroute.png)

## <a name="azure-to-azure-replication-with-expressroute"></a>Replicatie van Azure naar Azure met ExpressRoute

Azure Site Recovery maakt herstel na noodgevallen van [virtuele Azure-machines](azure-to-azure-architecture.md). Afhankelijk van of uw virtuele machines van Azure gebruik [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md), replicatiegegevens worden verzonden naar een Azure Storage-account of een replica beheerde schijf op de doel-Azure-regio. Hoewel de replicatie-eindpunten openbaar zijn, door replicatieverkeer voor Azure VM-replicatie, standaard, het Internet, ongeacht welke Azure-regio de bron van virtueel netwerk bestaat in niet bladeren. U kunt onderdrukken van Azure voor standaardroute systeem voor het adresvoorvoegsel 0.0.0.0/0 vervangen door een [aangepaste route](../virtual-network/virtual-networks-udr-overview.md#custom-routes) en omleiden van verkeer van de virtuele machine naar een on-premises virtueel netwerkapparaat (NVA), maar deze configuratie wordt niet aanbevolen voor Site Recovery replicatie. Als u aangepaste routes, moet u [maken van een service-eindpunt voor virtueel netwerk](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) in uw virtuele netwerk voor 'Opslag' zodat het replicatieverkeer niet de Azure-grens verlaat.

Voor noodherstel van Azure-VM is standaard ExpressRoute niet vereist voor replicatie. Nadat de virtuele machines, failover naar de doel-Azure-regio, kunt u ze openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

Als u verbinding maken tussen uw on-premises datacenter en de Azure VM's in de regio van de gegevensbron al ExpressRoute gebruikt, kunt u van plan bent voor het opnieuw tot stand brengen van ExpressRoute-connectiviteit in de doelregio failover. Verbinding maken met de doelregio via een nieuwe virtuele netwerkverbinding of gebruikmaken van een afzonderlijke ExpressRoute-circuit en de verbinding voor herstel na noodgevallen kunt u hetzelfde ExpressRoute-circuit. De verschillende mogelijke scenario's worden beschreven [hier](azure-vm-disaster-recovery-with-expressroute.md#fail-over-azure-vms-when-using-expressroute).

U kunt virtuele Azure-machines repliceren naar een Azure-regio binnen hetzelfde geografische cluster als gedetailleerde [hier](../site-recovery/azure-to-azure-support-matrix.md#region-support). Als de gekozen Azure-doelregio niet binnen dezelfde geopolitieke regio als de bron valt, moet u mogelijk ExpressRoute Premium inschakelen. Raadpleeg voor meer informatie het [ExpressRoute-locaties](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) en [prijzen voor ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [ExpressRoute-circuits](../expressroute/expressroute-circuit-peerings.md).
- Meer informatie over [ExpressRoute-Routeringsdomeinen](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Meer informatie over [ExpressRoute-locaties](../expressroute/expressroute-locations.md).
- Meer informatie over herstel na noodgevallen van [Azure virtual machines met ExpressRoute ](azure-vm-disaster-recovery-with-expressroute.md).
