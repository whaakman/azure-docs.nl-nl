---
title: Fouten oplossen tijdens de failback naar on-premises van Azure en daarna naar Azure beveiligt | Microsoft Docs
description: In dit artikel wordt beschreven hoe het oplossen van veelvoorkomende fouten in mislukken naar on-premises van Azure en tijdens het opnieuw beveiligen
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/19/2017
ms.author: rajanaki
ms.openlocfilehash: d487c1fcf7fb6444c2b8489df839a6450715ae0a
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="troubleshoot-errors-reported-during-the-process-of-failback"></a>Fouten hebben gerapporteerd tijdens het proces van failback oplossen
Failback omvat in principe twee belangrijke stappen. Een beveiligt virtuele machines van Azure met on-premises, tweede is het daadwerkelijk uitvoeren van de failback van Azure met on-premises.

## <a name="troubleshoot-errors-when-reprotecting-a-virtual-machine-back-to-on-premises-after-failover"></a>Fouten bij het opnieuw te beveiligen van een virtuele machine naar on-premises na een failover oplossen
U wordt een van de volgende fouten tijdens het uitvoeren van een virtuele machine in Azure beveiligt. Voor het oplossen, kunt u de stappen beschreven voor elke fout.


### <a name="error-code-95226"></a>Foutcode 95226

*Beveiligt, is mislukt omdat de virtuele machine van Azure niet kunnen bereiken van de lokale configuratie-server.*

Dit gebeurt wanneer 
1. Virtuele machine van Azure kan de lokale configuratie-server kan niet worden bereikt en daarom niet worden gedetecteerd en geregistreerd voor de configuratieserver. 
2. De InMage Scout Application service op de virtuele machine van Azure die moet worden uitgevoerd om te communiceren met de lokale configuratie-server mogelijk niet na failover worden uitgevoerd.

Dit probleem op te lossen
1. U moet ervoor zorgen dat het netwerk van virtuele machine van Azure is zodanig geconfigureerd dat de virtuele machine met de lokale configuratie-server communiceren kan. U doet dit door een Site naar Site VPN terug naar uw on-premises datacentrum instellen of configureren van een ExpressRoute-verbinding met de persoonlijke peering in het virtuele netwerk van virtuele machine van Azure. 
2. Als u al een netwerk zodanig geconfigureerd dat de virtuele machine van Azure met de lokale configuratie-server communiceren kan hebt, meldt u zich vervolgens naar de virtuele machine en 'InMage Scout Application Service' controleren. Als u ziet dat de InMage Scout Application Service niet wordt uitgevoerd, start de service handmatig en zorg ervoor dat het starttype van de service is ingesteld op automatisch.

### <a name="error-code-78052"></a>Foutcode 78052
Beveiligt mislukt met het volgende foutbericht: *beveiliging voor de virtuele machine kan niet worden voltooid.*

Dit kan gebeuren als gevolg van twee redenen
1. De virtuele machine die u opnieuw uit te beveiligen is een Windows Server 2016. Momenteel dit besturingssysteem wordt niet ondersteund voor failback, maar zal binnenkort worden ondersteund.
2. Er bestaat al een virtuele machine met dezelfde naam op de Master-doelserver u terug naar mislukken.

U kunt dit probleem oplossen door een andere hoofddoelserver op een andere host selecteren zodat de beveiligt de machine op een andere host maakt, waarbij de namen geen conflict veroorzaken. U kunt ook het hoofddoel naar een andere host waar het conflict naam niet gebeurt vMotion. Als de bestaande virtuele machine een losse machine is, kunt u deze alleen wijzigen zodat de nieuwe virtuele machine ophalen op dezelfde ESXi-host gemaakt kan.

### <a name="error-code-78093"></a>Foutcode 78093

*De virtuele machine wordt niet uitgevoerd in een vastgelopen status, of is niet toegankelijk.*

Als u wilt een mislukte via de virtuele machine naar on-premises beveiligt, moet u de virtuele machine van Azure uitgevoerd. Dit is zodat de mobility-service met de configuratieserver registreert on-premises en kunt beginnen met het repliceren van door de communicatie met de processerver. Als de machine zich op een netwerk onjuist of wordt niet uitgevoerd (is vastgelopen of afsluiten), bereiken niet de mobility-service in de virtuele machine om te beginnen met het beveiligt door de configuratieserver. U kunt de virtuele machine opnieuw zodat deze communicatie back lokale kunt starten. De taak opnieuw beveiligen opnieuw starten na het starten van de virtuele machine van Azure

### <a name="error-code-8061"></a>Foutcode 8061

*Het gegevensarchief is niet toegankelijk is vanaf ESXi-host.*

Raadpleeg de [de hoofdsleutel van de doel-vereisten](site-recovery-how-to-reprotect.md#common-things-to-check-after-completing-installation-of-the-master-target-server) en de [datastores ondersteunen](site-recovery-how-to-reprotect.md#what-datastore-types-are-supported-on-the-on-premises-esxi-host-during-failback) voor failback


## <a name="troubleshoot-errors-when-performing-a-failback-of-an-azure-virtual-machine-back-to-on-premises"></a>Fouten bij het uitvoeren van een failback van een virtuele machine van Azure naar on-premises oplossen
U wordt een van de volgende fouten tijdens het uitvoeren van failback van een virtuele machine van Azure naar on-premises. Voor het oplossen, kunt u de stappen beschreven voor elke fout.

### <a name="error-code-8038"></a>Foutcode 8038

*Kan niet online zetten van de on-premises virtuele machine vanwege de fout*

Dit gebeurt wanneer de on-premises virtuele machine op een host die beschikt niet over voldoende geheugen ingericht wordt opgeroepen.

Dit probleem op te lossen

1. U kunt meer geheugen op de host ESXi inrichten.
1. de virtuele machine naar een andere ESXi-host die voldoende geheugen heeft om de virtuele machine opstarten vMotion.