---
title: Netwerkbeveiligingsgroepen met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe u Netwerkbeveiligingsgroepen met Azure Site Recovery voor de migratie en herstel na noodgevallen
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/21/2018
ms.author: manayar
ms.openlocfilehash: 2b35578e2dc49cd47689f62fc47c5341ea8767a4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655251"
---
# <a name="network-security-groups-with-azure-site-recovery"></a>Netwerkbeveiligingsgroepen met Azure Site Recovery

Netwerkbeveiligingsgroepen worden gebruikt voor het verkeer beperkt tot bronnen in een virtueel netwerk. Een [Netwerkbeveiligingsgroep (NSG)](../virtual-network/security-overview.md#network-security-groups) bevat een lijst met regels voor toestaan of weigeren van binnenkomend of uitgaand netwerkverkeer op basis van de bron of bestemming IP-adres, poort en protocol.

Nsg's kunnen worden gekoppeld aan subnetten of afzonderlijke netwerkinterfaces onder het implementatiemodel van Resource Manager. Wanneer een NSG is gekoppeld aan een subnet, zijn de regels van toepassing op alle resources die zijn verbonden met het subnet. Verkeer kan verder worden beperkt door het ook een NSG koppelen aan afzonderlijke netwerkinterfaces binnen een subnet waarop al een gekoppelde NSG.

Dit artikel wordt beschreven hoe u Netwerkbeveiligingsgroepen kunt gebruiken met Azure Site Recovery.

## <a name="using-network-security-groups"></a>Met behulp van Netwerkbeveiligingsgroepen

Een afzonderlijk subnet kan hebben nul of één, NSG gekoppeld. Kan ook een afzonderlijke netwerkinterfaces hebben nul of één, NSG gekoppeld. U kunt dus effectief dual verkeer beperking voor een virtuele machine hebben waarbij een NSG eerst aan een subnet, en vervolgens een andere NSG aan het VM-netwerkinterface. De toepassing van het NSG-regels zijn in dit geval afhankelijk van de richting van verkeer en prioriteit van regels voor beveiliging toegepast.

Houd rekening met een eenvoudig voorbeeld aan één virtuele machine als volgt:
-   De virtuele machine wordt geplaatst in de **Contoso Subnet**.
-   **Contoso Subnet** is gekoppeld aan **NSG voor Subnet**.
-   De VM-netwerkinterface is ook gekoppeld aan **VM NSG**.

![NSG met Site Recovery](./media/concepts-network-security-group-with-site-recovery/site-recovery-with-network-security-group.png)

In dit voorbeeld voor binnenkomend verkeer is het NSG voor Subnet eerst geëvalueerd. Verkeer toegestaan via het NSG voor Subnet wordt vervolgens geëvalueerd door VM NSG. Het omgekeerde geldt voor uitgaand verkeer met VM NSG wordt eerst geëvalueerd. Verkeer toegestaan via VM NSG wordt vervolgens geëvalueerd door het NSG voor Subnet.

Hierdoor Regeltoepassing gefragmenteerde beveiliging. U wilt bijvoorbeeld inkomende internet toegang tot enkele toepassing VM's (zoals frontend VM's) onder een subnet, maar binnenkomende toegang tot internet beperken tot andere virtuele machines (zoals database en andere back-end virtuele machines). In dit geval kunt u een soepeler regel hebben op het NSG voor Subnet, internet-verkeer toestaat en toegang tot specifieke virtuele machines beperken door de toegang op VM NSG wordt geweigerd. Hetzelfde kan worden toegepast voor uitgaand verkeer.

Bij het instellen van dergelijke NSG-configuraties, zorg ervoor dat de juiste prioriteiten worden toegepast op de [beveiligingsregels](../virtual-network/security-overview.md#security-rules). Regels worden verwerkt in volgorde van prioriteit, waarbij lagere getallen worden verwerkt vóór hogere getallen omdat lagere getallen een hogere prioriteit hebben. Zodra het verkeer overeenkomt met een regel, wordt de verwerking beëindigd. Daardoor worden regels met een lagere prioriteit (een hoger getal) die dezelfde kenmerken hebben als regels met een hogere prioriteit, niet verwerkt.

U ben er mogelijk niet altijd van op de hoogte wanneer netwerkbeveiligingsgroepen worden toegepast op zowel een netwerkinterface als een subnet. U kunt controleren of de cumulatieve regels toegepast op een netwerkinterface door de [effectieve beveiligingsregels](../virtual-network/virtual-network-network-interface.md#view-effective-security-rules) voor een netwerkinterface. U kunt ook de [IP-stroom controleren](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) mogelijkheden in [Azure-netwerk-Watcher](../network-watcher/network-watcher-monitoring-overview.md) om te bepalen of de communicatie wordt toegestaan of naar een netwerkinterface. Deze functie vertelt u of communicatie is toegestaan en welke netwerkbeveiligingsregel verkeer toestaat of weigert.

## <a name="on-premises-to-azure-replication-with-nsg"></a>On-premises naar Azure replicatie met NSG

Azure Site Recovery kunt u herstel na noodgevallen en migratie naar Azure voor on-premises [Hyper-V virtuele machines](hyper-v-azure-architecture.md), [virtuele VMware-machines](vmware-azure-architecture.md), en [fysieke servers](physical-azure-architecture.md). Voor alle on-premises naar Azure scenario's, replicatiegegevens verzonden naar en opgeslagen in een Azure Storage-account. Tijdens de replicatie betaalt u geen eventuele kosten voor de virtuele machine. Wanneer u een failover naar Azure uitvoert, maakt Site Recovery automatisch Azure IaaS virtuele machines.

Zodra de VM's zijn gemaakt na een failover naar Azure, kunnen nsg's worden gebruikt om verkeer beperkt tot het virtuele netwerk en de virtuele machines. Site Recovery maakt geen nsg's als onderdeel van de failoverbewerking. Het is raadzaam om de vereiste Azure NGSs maken voordat u failover start. U kunt vervolgens koppelen nsg's aan virtuele machines automatisch failover tijdens failover met automatiseringsscripts met Site-Recovery krachtig [herstelplannen](site-recovery-create-recovery-plans.md).

Bijvoorbeeld, als de configuratie van de virtuele machine na een failover is vergelijkbaar met de [voorbeeldscenario](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) beschreven:
-   U kunt maken **Contoso VNet** en **Contoso Subnet** als onderdeel van DR plannen op de doel-Azure-regio.
-   U kunt ook maken en configureren van beide **NSG voor Subnet** , evenals **VM NSG** als onderdeel van de dezelfde DR plannen.
-   **NSG voor subnet** vervolgens onmiddellijk kan worden gekoppeld aan **Contoso Subnet**, zoals zowel de NSG en het subnet al beschikbaar zijn.
-   **VM NSG** kan worden gekoppeld aan virtuele machines tijdens de failover met herstelplannen.

Als het nsg's zijn gemaakt en geconfigureerd, wordt aangeraden die wordt uitgevoerd een [testfailover](site-recovery-test-failover-to-azure.md) om te controleren of een script vastgelegd NSG koppelingen en na een failover VM connectiviteit.

## <a name="azure-to-azure-replication-with-nsg"></a>Replicatie van Azure naar Azure met NSG

Azure Site Recovery kunt herstel na noodgevallen van [virtuele Azure-machines](azure-to-azure-architecture.md). Bij het inschakelen van replicatie voor virtuele machines van Azure kan Site Recovery de replica op de doelregio virtuele netwerken (inclusief subnetten en gatewaysubnetten) maken en de vereiste toewijzingen maken tussen de bron en doel virtuele netwerken. U kunt ook vooraf maken de doelnetwerken aan clientzijde en subnetten en gebruik van dezelfde tijdens het inschakelen van replicatie. Site Recovery maakt niet alle virtuele machines op de doel-Azure-regio voordat ze worden [failover](azure-to-azure-tutorial-failover-failback.md).

Zorg ervoor dat het NSG-regels op de bron-Azure-regio toestaan voor replicatie van de virtuele machine in Azure, [uitgaande verbinding](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) voor replicatieverkeer. U kunt ook testen en controleren of deze vereiste regels via deze [NSG voorbeeldconfiguratie](azure-to-azure-about-networking.md#example-nsg-configuration).

Site Recovery niet maken of nsg's als onderdeel van de failoverbewerking repliceren. Het is raadzaam om de vereiste NGSs maken op de doel-Azure-regio voor u failover start. U kunt vervolgens koppelen nsg's aan virtuele machines automatisch failover tijdens failover met automatiseringsscripts met Site-Recovery krachtig [herstelplannen](site-recovery-create-recovery-plans.md).

U overweegt de [voorbeeldscenario](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) eerder beschreven:
-   Site Recovery kunt maken van replica's van **Contoso VNet** en **Contoso Subnet** op de doel-Azure-regio wanneer replicatie is ingeschakeld voor de virtuele machine.
-   Kunt u de gewenste replica's van **NSG voor Subnet** en **VM NSG** (naam, bijvoorbeeld **doel Subnet NSG** en **doel VM NSG**, respectievelijk) op de doelhost Azure-regio, zodat voor eventuele extra regels vereist op de doelregio.
-   **NSG voor Subnet als doel** vervolgens kunnen worden onmiddellijk die zijn gekoppeld aan het subnet van de doel-regio als zowel de NSG en het subnet al beschikbaar zijn.
-   **Doel VM NSG** kan worden gekoppeld aan virtuele machines tijdens de failover met herstelplannen.

Als het nsg's zijn gemaakt en geconfigureerd, wordt aangeraden die wordt uitgevoerd een [testfailover](azure-to-azure-tutorial-dr-drill.md) om te controleren of een script vastgelegd NSG koppelingen en na een failover VM connectiviteit.

## <a name="next-steps"></a>Volgende stappen
-   Meer informatie over [Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md#network-security-groups).
-   Meer informatie over het NSG [beveiligingsregels](../virtual-network/security-overview.md#security-rules).
-   Meer informatie over [effectieve beveiligingsregels](../virtual-network/diagnose-network-traffic-filter-problem.md) voor een NSG.
-   Meer informatie over [herstelplannen](site-recovery-create-recovery-plans.md) toepassing failover te automatiseren.
