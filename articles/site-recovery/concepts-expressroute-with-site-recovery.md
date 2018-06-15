---
title: Azure ExpressRoute met Azure Site Recovery | Microsoft Docs
description: Hierin wordt beschreven hoe u Azure ExpressRoute gebruiken met Azure Site Recovery voor de migratie en herstel na noodgevallen
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: 7cc4c84ebae7ade4169f8d85a2d5cc11f1df6f87
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
ms.locfileid: "34071444"
---
# <a name="azure-expressroute-with-azure-site-recovery"></a>Azure ExpressRoute met Azure Site Recovery

Met Microsoft Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider. Met ExpressRoute kunt u verbindingen tot stand brengen met Microsoft Cloud-services, zoals Microsoft Azure, Office 365 en Dynamics 365.

Dit artikel wordt beschreven hoe u Azure ExpressRoute kunt gebruiken met Azure Site Recovery voor de migratie en herstel na noodgevallen.

## <a name="expressroute-circuits"></a>ExpressRoute-circuits

Een ExpressRoute-circuit vertegenwoordigt een logische verbinding tussen uw on-premises infrastructuur en Microsoft cloud-services via een connectiviteitsprovider. U kunt meerdere ExpressRoute-circuits bestellen. Elk circuit kan zich in dezelfde of verschillende regio's en kan worden verbonden met uw lokale via verschillende connectiviteitsproviders. Meer informatie over ExpressRoute-circuits [hier](../expressroute/expressroute-circuit-peerings.md).

## <a name="expressroute-routing-domains"></a>ExpressRoute-routeringsdomeinen

Een ExpressRoute-circuit heeft meerdere Routeringsdomeinen gekoppeld:
-   [Persoonlijke Azure-peering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering) - Azure compute-services, namelijk virtuele machines (IaaS), en cloudservices (PaaS) die zijn geïmplementeerd in een virtueel netwerk kunnen worden verbonden via het persoonlijke peering domein. Het domein voor persoonlijke peering wordt beschouwd als een vertrouwde uitbreiding van uw Basisnetwerk in Microsoft Azure.
-   [Openbare Azure-peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) -Services zoals Azure Storage, SQL-databases en Websites worden aangeboden op het openbare IP-adressen. U kunt privé verbinding maken met services die worden gehost op openbare IP-adressen, met inbegrip van VIP's van uw cloud-services via openbare peering routeringsdomein. Openbare peering voor nieuw aangemaakte is afgeschaft en Microsoft-Peering in plaats daarvan moet worden gebruikt voor Azure PaaS-services.
-   [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoft-peering) -connectiviteit met Microsoft online services (Office 365, Dynamics 365 en Azure PaaS-services) is via het Microsoft-peering. Microsoft-peering, is het aanbevolen routeringsdomein verbinding maken met Azure PaaS-services.

Meer informatie over en vergelijkt u ExpressRoute-Routeringsdomeinen [hier](../expressroute/expressroute-circuit-peerings.md#routing-domain-comparison).

## <a name="on-premises-to-azure-replication-with-expressroute"></a>On-premises naar Azure replicatie met ExpressRoute

Azure Site Recovery kunt u herstel na noodgevallen en migratie naar Azure voor on-premises [Hyper-V virtuele machines](hyper-v-azure-architecture.md), [virtuele VMware-machines](vmware-azure-architecture.md), en [fysieke servers](physical-azure-architecture.md). Voor alle on-premises naar Azure scenario's, replicatiegegevens verzonden naar en opgeslagen in een Azure Storage-account. Tijdens de replicatie betaalt u geen eventuele kosten voor de virtuele machine. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch Azure IaaS virtuele machines.

Site Recovery repliceert gegevens naar een Azure Storage-account via een openbaar eindpunt. Voor het gebruik van ExpressRoute voor replicatie van Site Recovery, kunt u gebruikmaken van [openbare peering](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) of [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoft-peering). Microsoft-peering, is het aanbevolen routeringsdomein voor replicatie. Nadat de virtuele machines of servers worden via failover naar een Azure-netwerk, kunt u deze kunt openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering). Replicatie wordt niet ondersteund via persoonlijke peering.

De gecombineerde scenario wordt weergegeven in het volgende diagram: ![op lokale-die naar Azure met ExpressRoute](./media/concepts-expressroute-with-site-recovery/site-recovery-with-expressroute.png)

## <a name="azure-to-azure-replication-with-expressroute"></a>Replicatie van Azure naar Azure met ExpressRoute

Azure Site Recovery kunt herstel na noodgevallen van [virtuele Azure-machines](azure-to-azure-architecture.md). Afhankelijk van of uw Azure virtual machines gebruik [Azure beheerd schijven](../virtual-machines/windows/managed-disks-overview.md), gegevens van replicatie wordt verzonden naar een Azure Storage-account of replica beheerd schijf op de doel-Azure-regio. Hoewel de replicatie-eindpunten openbaar zijn, wordt in replicatieverkeer voor replicatie van de virtuele machine in Azure, standaard Internet, ongeacht welke Azure-regio het virtuele netwerk van de bron bestaat in niet passeren. U kunt Azure standaardroute systeem voor het adresvoorvoegsel 0.0.0.0/0 met onderdrukken een [aangepaste route](../virtual-network/virtual-networks-udr-overview.md#custom-routes) en het omleiden van verkeer van de virtuele machine naar een virtueel apparaat voor lokale netwerk (NVA), maar deze configuratie wordt niet aanbevolen voor de Site Recovery replicatie. Als u aangepaste routes gebruikt, moet u [maken van een service-eindpunt van het virtuele netwerk](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) in uw virtuele netwerk voor 'Opslag' zodat het replicatieverkeer de grens van Azure niet laat.

Voor noodherstel van de virtuele machine in Azure is standaard ExpressRoute niet vereist voor replicatie. Nadat de virtuele machines, failover met de doel-Azure-regio, kunt u deze kunt openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

Als u al van ExpressRoute gebruikmaakt vanaf uw on-premises datacentrum verbinding maken met de Azure VM's op de bron-regio, kunt u plannen voor het herstellen van ExpressRoute-verbinding op de doelregio failover. U kunt hetzelfde ExpressRoute-circuit gebruiken verbinding voor de doelregio via een nieuwe virtuele netwerkverbinding maken met of gebruikmaken van een afzonderlijke ExpressRoute-circuit en de verbinding voor herstel na noodgevallen. De verschillende mogelijke scenario's worden beschreven [hier](azure-vm-disaster-recovery-with-expressroute.md#failover-models-with-expressroute).

U kunt Azure virtuele machines repliceren naar een Azure-regio in hetzelfde geografische cluster zoals beschreven [hier](../site-recovery/azure-to-azure-support-matrix.md#region-support). Als het gekozen doel-Azure-regio niet binnen dezelfde geopolitieke regio als de bron is, moet u mogelijk ExpressRoute Premium inschakelen. Raadpleeg voor meer details [ExpressRoute-locaties](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) en [ExpressRoute prijzen](https://azure.microsoft.com/pricing/details/expressroute/).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [ExpressRoute-circuits](../expressroute/expressroute-circuit-peerings.md).
- Meer informatie over [ExpressRoute-Routeringsdomeinen](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Meer informatie over [ExpressRoute-locaties](../expressroute/expressroute-locations.md).
- Meer informatie over herstel na noodgevallen van [Azure virtuele machines met ExpressRoute ](azure-vm-disaster-recovery-with-expressroute.md).
