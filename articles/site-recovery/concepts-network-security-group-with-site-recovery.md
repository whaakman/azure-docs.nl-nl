---
title: Netwerkbeveiligingsgroepen met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe u Netwerkbeveiligingsgroepen gebruikt met Azure Site Recovery voor herstel na noodgevallen en migratie
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: b0570a7fe0cb6f334d18f82b1f06d5fa770b5af8
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37921311"
---
# <a name="network-security-groups-with-azure-site-recovery"></a>Netwerkbeveiligingsgroepen met Azure Site Recovery

Netwerkbeveiligingsgroepen worden gebruikt voor het beperken van netwerkverkeer tot resources in een virtueel netwerk. Een [Netwerkbeveiligingsgroep (NSG)](../virtual-network/security-overview.md#network-security-groups) bevat een lijst met beveiligingsregels die toestaan of weigeren van binnenkomend of uitgaand netwerkverkeer op basis van de bron of doel-IP-adres, poort en protocol.

Nsg's kunnen worden gekoppeld aan subnetten of afzonderlijke netwerkinterfaces in het Resource Manager-implementatiemodel. Wanneer een NSG is gekoppeld aan een subnet, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet. Verkeer kan verder worden beperkt door ook een NSG koppelen aan afzonderlijke netwerkinterfaces binnen een subnet waarvoor al een gekoppelde NSG.

Dit artikel wordt beschreven hoe u Netwerkbeveiligingsgroepen kunt gebruiken met Azure Site Recovery.

## <a name="using-network-security-groups"></a>Met behulp van Netwerkbeveiligingsgroepen

Een afzonderlijk subnet kan zijn nul of één Netwerkbeveiligingsgroep gekoppeld. Een afzonderlijke netwerkinterfaces kan ook zijn nul of één Netwerkbeveiligingsgroep gekoppeld. U kunt dus effectief dual verkeer beperking voor een virtuele machine te koppelen van een NSG eerst aan een subnet, en vervolgens op een andere NSG aan de netwerkinterface van de virtuele machine hebben. De toepassing van NSG-regels zijn in dit geval afhankelijk van de richting van verkeer en de prioriteit van toegepaste beveiligingsregels.

Houd rekening met een eenvoudig voorbeeld met één virtuele machine als volgt:
-   De virtuele machine wordt geplaatst in de **Contoso Subnet**.
-   **Contoso-Subnet** is gekoppeld aan **Subnet-NSG**.
-   De VM-netwerkinterface is ook gekoppeld aan **VM NSG**.

![NSG met Site Recovery](./media/concepts-network-security-group-with-site-recovery/site-recovery-with-network-security-group.png)

In dit voorbeeld voor binnenkomend verkeer de Subnet-NSG, wordt eerst geëvalueerd. Verkeer dat is toegestaan via de Subnet-NSG wordt vervolgens geëvalueerd door VM NSG. Het omgekeerde geldt voor uitgaand verkeer met VM-NSG wordt eerst geëvalueerd. Verkeer dat is toegestaan via VM NSG wordt vervolgens geëvalueerd door de Subnet-NSG.

Hierdoor Regeltoepassing verfijnde beveiliging. U wilt bijvoorbeeld inkomende internet toegang tot een paar toepassings-VM's (zoals de front-end VM's) in een subnet, maar binnenkomende toegang tot internet beperken tot andere virtuele machines (zoals een database en andere back-end-VM's). U kunt in dit geval hebt een soepeler regel op de Subnet-NSG, zodat internetverkeer, en toegang tot specifieke virtuele machines beperken door het weigeren van toegang op VM NSG. Hetzelfde kan worden toegepast voor uitgaand verkeer.

Bij het instellen van dergelijke NSG-configuraties, zorg ervoor dat de juiste prioriteiten worden toegepast op de [beveiligingsregels](../virtual-network/security-overview.md#security-rules). Regels worden verwerkt in volgorde van prioriteit, waarbij lagere getallen worden verwerkt vóór hogere getallen omdat lagere getallen een hogere prioriteit hebben. Zodra het verkeer overeenkomt met een regel, wordt de verwerking beëindigd. Daardoor worden regels met een lagere prioriteit (een hoger getal) die dezelfde kenmerken hebben als regels met een hogere prioriteit, niet verwerkt.

U ben er mogelijk niet altijd van op de hoogte wanneer netwerkbeveiligingsgroepen worden toegepast op zowel een netwerkinterface als een subnet. U kunt controleren of de cumulatieve regels toegepast op een netwerkinterface hand de [effectieve beveiligingsregels](../virtual-network/virtual-network-network-interface.md#view-effective-security-rules) voor een netwerkinterface. U kunt ook de [IP-stroom controleren](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) -mogelijkheid in [Azure Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) om te bepalen of de communicatie is toegestaan naar of van een netwerkinterface. Deze functie vertelt u of communicatie is toegestaan en welke netwerkbeveiligingsregel verkeer toestaat of weigert.

## <a name="on-premises-to-azure-replication-with-nsg"></a>On-premises naar Azure-replicatie met NSG

Azure Site Recovery maakt herstel na noodgevallen en migratie naar Azure voor on-premises [Hyper-V virtuele machines](hyper-v-azure-architecture.md), [virtuele VMware-machines](vmware-azure-architecture.md), en [fysieke servers](physical-azure-architecture.md). Replicatiegegevens worden voor alle on-premises naar Azure-scenario's, verzonden naar en opgeslagen in een Azure Storage-account. Tijdens de replicatie betaalt u geen kosten voor elke virtuele machine. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch virtuele machines van Azure IaaS.

Zodra VM's zijn gemaakt na een failover naar Azure, kunnen de nsg's worden gebruikt om te beperken van netwerkverkeer naar het virtuele netwerk en VM's. Site Recovery maakt geen nsg's als onderdeel van de failoverbewerking. Het is raadzaam om het maken van de vereiste Azure-NGSs voordat de failover wordt gestart. U kunt vervolgens nsg's koppelen aan virtuele machines automatisch failover tijdens de failover, met behulp van automatiseringsscripts met Site Recovery van krachtige [herstelplannen](site-recovery-create-recovery-plans.md).

Bijvoorbeeld, als de configuratie van de virtuele machine na een failover is vergelijkbaar met de [voorbeeldscenario](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) hierboven beschreven:
-   U kunt maken **Contoso VNet** en **Contoso Subnet** als onderdeel van herstel na Noodgevallen plannen op de doel-Azure-regio.
-   U kunt ook maken en configureren van beide **Subnet-NSG** , evenals **VM NSG** als onderdeel van de dezelfde herstel na Noodgevallen plannen.
-   **Subnet-NSG** vervolgens direct kunnen worden gekoppeld aan **Contoso Subnet**, zoals de NSG en voor het subnet al beschikbaar zijn.
-   **VM NSG** kan worden gekoppeld aan virtuele machines tijdens de failover met behulp van herstelplannen.

Zodra de nsg's zijn gemaakt en geconfigureerd, wordt aangeraden die wordt uitgevoerd een [testfailover](site-recovery-test-failover-to-azure.md) om te controleren of een script vastgelegd NSG koppelingen en VM-connectiviteit na een failover.

## <a name="azure-to-azure-replication-with-nsg"></a>Replicatie van Azure naar Azure met NSG

Azure Site Recovery maakt herstel na noodgevallen van [virtuele Azure-machines](azure-to-azure-architecture.md). Bij het inschakelen van replicatie voor virtuele machines van Azure kan Site Recovery maken van de replica virtuele netwerken (met inbegrip van subnetten en gateway-subnetten) op de doelregio en maken de vereiste toewijzingen tussen de bron en doel van de virtuele netwerken. U kunt ook vooraf de doel-kant netwerken en subnetten maken en gebruiken dezelfde tijdens het inschakelen van replicatie. Site Recovery maakt geen virtuele machines op de doel-Azure-regio voorafgaand aan de [failover](azure-to-azure-tutorial-failover-failback.md).

Zorg ervoor dat de NSG-regels op de bron-Azure-regio toestaan voor Azure VM-replicatie, [uitgaande connectiviteit](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) voor replicatieverkeer. U kunt ook testen en controleren of deze vereiste regels via deze [NSG voorbeeldconfiguratie](azure-to-azure-about-networking.md#example-nsg-configuration).

Site Recovery niet maken of repliceren nsg's als onderdeel van de failoverbewerking. Het is raadzaam om het maken van de vereiste NGSs op de doel-Azure-regio voordat de failover wordt gestart. U kunt vervolgens nsg's koppelen aan virtuele machines automatisch failover tijdens de failover, met behulp van automatiseringsscripts met Site Recovery van krachtige [herstelplannen](site-recovery-create-recovery-plans.md).

U overweegt de [voorbeeldscenario](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) die eerder zijn beschreven:
-   Site Recovery kunt maken van replica's van **Contoso VNet** en **Contoso Subnet** op de doel-Azure-regio wanneer replicatie is ingeschakeld voor de virtuele machine.
-   Kunt u de gewenste replica's van **Subnet-NSG** en **VM NSG** (met de naam, bijvoorbeeld **doel Subnet-NSG** en **doel VM NSG**, respectievelijk) op het doel Azure-regio, zodat voor eventuele extra regels die zijn vereist op de doelregio.
-   **Gericht op Subnet-NSG** kunnen vervolgens worden onmiddellijk die zijn gekoppeld aan het doelsubnet regio als zowel de NSG en het subnet al beschikbaar zijn.
-   **Richt VM NSG** kan worden gekoppeld aan virtuele machines tijdens de failover met behulp van herstelplannen.

Zodra de nsg's zijn gemaakt en geconfigureerd, wordt aangeraden die wordt uitgevoerd een [testfailover](azure-to-azure-tutorial-dr-drill.md) om te controleren of een script vastgelegd NSG koppelingen en VM-connectiviteit na een failover.

## <a name="next-steps"></a>Volgende stappen
-   Meer informatie over [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md#network-security-groups).
-   Meer informatie over NSG [beveiligingsregels](../virtual-network/security-overview.md#security-rules).
-   Meer informatie over [effectieve beveiligingsregels](../virtual-network/diagnose-network-traffic-filter-problem.md) voor een NSG.
-   Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md) toepassing failover wilt automatiseren.
