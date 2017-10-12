---
title: Architectuur van Operations Management Suite (OMS) | Microsoft Docs
description: Microsoft Operations Management Suite (OMS) is een cloudoplossing voor IT-beheer van Microsoft waarmee u uw on-premises en cloudinfrastructuur kunt beheren en beveiligen.  In dit artikel worden de verschillende services van OMS beschreven en vindt u koppelingen naar gedetailleerde inhoud.
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 40e41686-7e35-4d85-bbe8-edbcb295a534
ms.service: operations-management-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 76f69946724b5297b1f9a1f715819c69c4a4a51d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="oms-architecture"></a>OMS-architectuur
[Operations Management Suite (OMS)](https://azure.microsoft.com/documentation/services/operations-management-suite/) is een verzameling cloudservices voor beheer van on-premises en cloudomgevingen.  In dit artikel worden de verschillende on-premises en cloudonderdelen van OMS en hun hoogwaardige cloud computing-architectuur beschreven.  Raadpleeg de documentatie van elke service voor meer informatie.

## <a name="log-analytics"></a>Log Analytics
Alle gegevens die met [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) worden verzameld, worden opgeslagen in de OMS-opslagplaats die in Azure wordt beheerd.  Met verbonden bronnen worden gegevens gegenereerd die in de OMS-opslagplaats zijn verzameld.  Er zijn momenteel drie soorten verbonden bronnen die worden ondersteund.

* Een agent op een [Windows](../log-analytics/log-analytics-windows-agents.md)- of [Linux](../log-analytics/log-analytics-linux-agents.md)-computer die rechtstreeks met OMS is verbonden.
* Een SCOM-groep (System Center Operations Manager) [verbonden met Log Analytics](../log-analytics/log-analytics-om-agents.md) .  SCOM-agenten blijven communiceren met beheerservers die gegevens over gebeurtenissen en prestaties doorsturen naar Log Analytics.
* Een [Azure-opslagaccount](../log-analytics/log-analytics-azure-storage.md) waarmee [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md)-gegevens van een werkrol, webrol of virtuele machine in Azure worden verzameld.

Gegevensbronnen zijn de gegevens die met Log Analytics van verbonden bronnen worden verzameld, waaronder gebeurtenislogboeken en prestatiemeteritems.  Oplossingen voegen functionaliteit toe aan OMS en kunnen eenvoudig vanuit de [OMS-oplossingsgalerie](../log-analytics/log-analytics-add-solutions.md) aan uw werkruimte worden toegevoegd.  Voor sommige oplossingen is mogelijk een directe verbinding tussen Log Analytics en SCOM-agenten nodig, terwijl voor andere oplossingen ene extra agent moet worden geïnstalleerd.

Log Analytics bestaat uit een webportal waarmee u OMS-resources kunt beheren, OMS-oplossingen kunt toevoegen en configureren en gegevens in de OMS-opslagplaats kunt bekijken en analyseren.

![Hoogwaardige Log Analytics-architectuur](media/operations-management-suite-architecture/log-analytics.png)

## <a name="azure-automation"></a>Azure Automation
[Azure Automation-runbooks](http://azure.microsoft.com/documentation/services/automation) worden uitgevoerd in de Azure-cloud en hebben toegang tot resources in Azure, in andere cloudservices of op internet.  U kunt ook met [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md) on-premises computers opgeven in uw lokale datacenter, zodat runbooks toegang hebben tot lokale resources.

[DSC-configuraties](../automation/automation-dsc-overview.md) die zijn opgeslagen in Azure Automation kunnen rechtstreeks worden toegepast op virtuele Azure-machines.  Voor andere fysieke en virtuele machines kunnen configuraties via de pull-server van Azure Automation DSC zijn vereist.

Azure Automation heeft een OMS-oplossing waarmee statistieken en koppelingen voor het starten van de Azure-portal voor alle bewerkingen worden weergegeven.

![Hoogwaardige Azure Automation-architectuur](media/operations-management-suite-architecture/automation.png)

## <a name="azure-backup"></a>Azure Backup
Beveiligde gegevens in [Azure Backup](http://azure.microsoft.com/documentation/services/backup) worden opgeslagen in een Backup-kluis in een bepaalde geografische regio.  De gegevens worden in dezelfde regio gerepliceerd en kunnen, afhankelijk van het type kluis, ook naar een andere regio worden gerepliceerd voor meer redundantie.

Azure Backup heeft drie fundamentele scenario's.

* Windows-computer met Azure Backup-agent.  Hiermee kunt u vanaf elke Windows-server of -client rechtstreeks naar uw Azure Backup-kluis back-ups van bestanden en mappen maken.  
* System Center Data Protection Manager- (DPM) of Microsoft Azure Backup-server. Hiermee kunt u de DPM- of Microsoft Azure Backup-server gebruiken om van bestanden en mappen en werkbelastingen van toepassingen, zoals SQL en SharePoint, back-ups te maken naar de lokale opslag en deze vervolgens te repliceren naar uw Azure Backup Vault.
* Extensies voor virtuele Azure-machines.  Hiermee kunt u back-ups van virtuele machines van Azure naar uw Azure Backup Vault maken.

Azure Backup heeft een OMS-oplossing waarmee statistieken en koppelingen voor het starten van de Azure-portal voor alle bewerkingen worden weergegeven.

![Hoogwaardige Azure Backup-architectuur](media/operations-management-suite-architecture/backup.png)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Met [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) wordt de replicatie, failover en failback van virtuele machines en fysieke servers gecoördineerd. Replicatiegegevens worden uitgewisseld tussen Hyper-V-hosts, VMware-hypervisors en fysieke servers in de primaire en secundaire datacenters, of tussen het datacenter en de Azure-opslag.  Met Site Recovery worden metagegevens opgeslagen in kluizen in een bepaalde geografische Azure-regio. Er worden geen gerepliceerde gegevens opgeslagen met de Site Recovery-service.

Azure Site Recovery heeft drie fundamentele replicatiescenario's.

**Virtuele Hyper-V-machines repliceren**

* Als virtuele Hyper-V-machines worden beheerd in VMM-clouds, kunt u naar een secundair datacenter of naar de Azure-opslag repliceren.  Replicatie naar Azure gaat via een beveiligde internetverbinding.  Replicatie naar een secundair datacenter gaat via LAN.
* Als virtuele Hyper-V-machines niet met VMM worden beheerd, kunt u alleen naar de Azure-opslag repliceren.  Replicatie naar Azure gaat via een beveiligde internetverbinding.

**Virtuele VMware-machines repliceren**

* U kunt on-premises virtuele VMware-machines repliceren naar een secundair datacenter met VMware of naar de Azure-opslag.  Replicatie naar Azure kan plaatsvinden via een VPN tussen sites, via Azure ExpressRoute of via een beveiligde internetverbinding. Replicatie naar een secundair datacenter vindt plaats via het InMage Scout-gegevenskanaal.

**Fysieke Windows- en Linux-servers repliceren** 

* U kunt fysieke servers repliceren naar een secundair datacenter of naar de Azure-opslag. Replicatie naar Azure kan plaatsvinden via een VPN tussen sites, via Azure ExpressRoute of via een beveiligde internetverbinding. Replicatie naar een secundair datacenter vindt plaats via het InMage Scout-gegevenskanaal.  Azure Site Recovery heeft een OMS-oplossing waarmee enkele statistieken worden weergegeven. Voor bewerkingen moet u echter de Azure-portal gebruiken.

![Hoogwaardige Azure Site Recovery-architectuur](media/operations-management-suite-architecture/site-recovery.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
* Meer informatie over [Azure Automation](https://azure.microsoft.com/documentation/services/automation).
* Meer informatie over [Azure Backup](http://azure.microsoft.com/documentation/services/backup).
* Meer informatie over [Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery).

