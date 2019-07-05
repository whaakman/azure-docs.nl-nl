---
title: Over het gebruik van Azure ExpressRoute met Azure Site Recovery voor migratie en herstel na noodgevallen | Microsoft Docs
description: Beschrijft hoe u Azure ExpressRoute gebruiken met de Azure Site Recovery-service voor de migratie en herstel na noodgevallen.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 6/27/2019
ms.author: mayg
ms.openlocfilehash: 35fa26112a6026ab05bd59b38621de7ee802c715
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491898"
---
# <a name="azure-expressroute-with-azure-site-recovery"></a>Met Azure ExpressRoute met Azure Site Recovery

Met Microsoft Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft Cloud uitbreiden via een persoonlijke verbinding die wordt gefaciliteerd door een connectiviteitsprovider. Met ExpressRoute kunt u verbindingen tot stand brengen met Microsoft Cloud-services, zoals Microsoft Azure, Office 365 en Dynamics 365.

Dit artikel wordt beschreven hoe u Azure ExpressRoute kunt gebruiken met Azure Site Recovery voor migratie en herstel na noodgevallen.

## <a name="expressroute-circuits"></a>ExpressRoute-circuits

Een ExpressRoute-circuit vertegenwoordigt een logische verbinding tussen uw on-premises infrastructuur en Microsoft cloud-services via een connectiviteitsprovider. U kunt meerdere ExpressRoute-circuits bestellen. Elk circuit kan in de dezelfde of verschillende regio's, en kan worden verbonden met uw locatie via verschillende connectiviteitsproviders. Meer informatie over ExpressRoute-circuits [hier](../expressroute/expressroute-circuit-peerings.md).

Een ExpressRoute-circuit heeft meerdere Routeringsdomeinen die ermee verbonden zijn. Meer informatie over en vergelijken van ExpressRoute-Routeringsdomeinen [hier](../expressroute/expressroute-circuit-peerings.md#peeringcompare).

## <a name="on-premises-to-azure-replication-with-expressroute"></a>On-premises naar Azure-replicatie met ExpressRoute

Azure Site Recovery maakt herstel na noodgevallen en migratie naar Azure voor on-premises [Hyper-V virtuele machines](hyper-v-azure-architecture.md), [virtuele VMware-machines](vmware-azure-architecture.md), en [fysieke servers](physical-azure-architecture.md). Replicatiegegevens worden voor alle on-premises naar Azure-scenario's, verzonden naar en opgeslagen in een Azure Storage-account. Tijdens de replicatie betaalt u geen kosten voor elke virtuele machine. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch virtuele machines van Azure IaaS.

Site Recovery repliceert gegevens naar een Azure Storage-account of een replica beheerde schijf op de doel-Azure-regio via een openbaar eindpunt. U kunt gebruiken voor het gebruik van ExpressRoute voor Site Recovery-replicatieverkeer, [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) of een bestaande [openbare peering](../expressroute/expressroute-circuit-peerings.md#publicpeering) (afgeschaft voor nieuw aangemaakte). Microsoft-peering is de aanbevolen routeringsdomein voor replicatie. Houd er rekening mee dat de replicatie wordt niet ondersteund via persoonlijke peering.

Zorg ervoor dat de [vereisten voor netwerken](vmware-azure-configuration-server-requirements.md#network-requirements) voor de configuratieserver is voldaan. Verbinding met specifieke URL's is vereist voor de configuratieserver voor het indelen van Site Recovery-replicatie. ExpressRoute kan niet worden gebruikt voor deze connectiviteit. 

Als u gebruik van proxy op on-premises en ExpressRoute gebruikt voor replicatieverkeer wilt, moet u de proxylijst configureren op de configuratieserver en de proces-Servers. Volg de onderstaande stappen:

- Download PsExec-hulpprogramma op [hier](https://aka.ms/PsExec) voor toegang tot de gebruiker systeemcontext.
- Open Internet Explorer in de systeemcontext van de gebruiker door het uitvoeren van de volgende opdrachtregel psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"
- Proxy-instellingen in Internet Explorer toevoegen
- De bypass-lijst, voeg de URL van de Azure storage *. blob.core.windows.net

Dit zorgt ervoor dat alleen replicatieverkeer via ExpressRoute stroomt terwijl de communicatie via proxy kunt gaan.

Nadat de virtuele machines of servers een failover uitvoeren naar een Azure-netwerk, kunt u ze openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#privatepeering). 

De gecombineerde scenario wordt weergegeven in het volgende diagram: ![On-premises-to-Azure with ExpressRoute](./media/concepts-expressroute-with-site-recovery/site-recovery-with-expressroute.png)

## <a name="azure-to-azure-replication-with-expressroute"></a>Replicatie van Azure naar Azure met ExpressRoute

Azure Site Recovery maakt herstel na noodgevallen van [virtuele Azure-machines](azure-to-azure-architecture.md). Afhankelijk van of uw virtuele machines van Azure gebruik [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md), replicatiegegevens worden verzonden naar een Azure Storage-account of een replica beheerde schijf op de doel-Azure-regio. Hoewel de replicatie-eindpunten openbaar zijn, door replicatieverkeer voor Azure VM-replicatie, standaard, het Internet, ongeacht welke Azure-regio de bron van virtueel netwerk bestaat in niet bladeren. U kunt onderdrukken van Azure voor standaardroute systeem voor het adresvoorvoegsel 0.0.0.0/0 vervangen door een [aangepaste route](../virtual-network/virtual-networks-udr-overview.md#custom-routes) en omleiden van verkeer van de virtuele machine naar een on-premises virtueel netwerkapparaat (NVA), maar deze configuratie wordt niet aanbevolen voor Site Recovery replicatie. Als u aangepaste routes, moet u [maken van een service-eindpunt voor virtueel netwerk](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) in uw virtuele netwerk voor 'Opslag' zodat het replicatieverkeer niet de Azure-grens verlaat.

Voor noodherstel van Azure-VM is standaard ExpressRoute niet vereist voor replicatie. Nadat de virtuele machines, failover naar de doel-Azure-regio, kunt u ze openen met behulp van [privépeering](../expressroute/expressroute-circuit-peerings.md#privatepeering).

Als u verbinding maken tussen uw on-premises datacenter en de Azure VM's in de regio van de gegevensbron al ExpressRoute gebruikt, kunt u van plan bent voor het opnieuw tot stand brengen van ExpressRoute-connectiviteit in de doelregio failover. Verbinding maken met de doelregio via een nieuwe virtuele netwerkverbinding of gebruikmaken van een afzonderlijke ExpressRoute-circuit en de verbinding voor herstel na noodgevallen kunt u hetzelfde ExpressRoute-circuit. De verschillende mogelijke scenario's worden beschreven [hier](azure-vm-disaster-recovery-with-expressroute.md#fail-over-azure-vms-when-using-expressroute).

U kunt virtuele Azure-machines repliceren naar een Azure-regio binnen hetzelfde geografische cluster als gedetailleerde [hier](../site-recovery/azure-to-azure-support-matrix.md#region-support). Als de gekozen Azure-doelregio niet binnen dezelfde geopolitieke regio als de bron valt, moet u mogelijk ExpressRoute Premium inschakelen. Raadpleeg voor meer informatie het [ExpressRoute-locaties](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) en [prijzen voor ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [ExpressRoute-circuits](../expressroute/expressroute-circuit-peerings.md).
- Meer informatie over [ExpressRoute-Routeringsdomeinen](../expressroute/expressroute-circuit-peerings.md#peeringcompare).
- Meer informatie over [ExpressRoute-locaties](../expressroute/expressroute-locations.md).
- Meer informatie over herstel na noodgevallen van [Azure virtual machines met ExpressRoute](azure-vm-disaster-recovery-with-expressroute.md).
