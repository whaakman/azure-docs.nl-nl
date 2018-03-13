---
title: Problemen oplossen tijdens de failback van virtuele machines van Azure naar lokale VMware met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe algemene failback en beveiligingspoging fouten oplossen tijdens de failback naar VMware van Azure met behulp van Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/09/2018
ms.author: rajanaki
ms.openlocfilehash: 6dcecce78de3caaefb40cb3fe4853d5d550163b4
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="troubleshoot-failback-from-azure-to-vmware"></a>Failback vanuit Azure naar VMware oplossen

Dit artikel wordt beschreven voor het oplossen van problemen die u tegenkomen kunt wanneer u weer Azure VM's naar uw on-premises VMware-infrastructuur na een failover voor het gebruik van Azure mislukken [Azure Site Recovery](site-recovery-overview.md).

Failback omvat in principe twee belangrijke stappen. Na een failover moet u virtuele Azure-machines beveiligt met on-premises, zodat deze beginnen te repliceren. De tweede stap is het uitvoeren van een failover van Azure, naar de failback naar uw on-premises site.

## <a name="troubleshoot-reprotection-errors"></a>Beveiligingspoging fouten oplossen

Deze sectie details veelvoorkomende beveiligingspoging fouten en hoe deze corrigeren.

### <a name="error-code-95226"></a>Foutcode 95226

**Beveiligt, is mislukt omdat de virtuele machine van Azure niet kunnen bereiken van de lokale configuratie-server.**

Deze fout treedt op wanneer:

1. De virtuele machine van Azure kan de configuratie van lokale server niet bereiken. De virtuele machine kan niet worden gedetecteerd en geregistreerd voor de configuratieserver.
2. De InMage Scout Application service wordt niet uitgevoerd op de Azure VM na een failover. De service is vereist voor communicatie met de lokale configuratie-server.

Dit probleem oplossen:

1. Controleer dat het Azure VM-netwerk de virtuele machine van Azure om te communiceren met de configuratieserver lokale toestaat. U doet dit door een site-naar-site-VPN naar uw on-premises datacentrum instellen of configureren van een ExpressRoute-verbinding met de persoonlijke peering in het virtuele netwerk van de Azure VM.
2. Als de virtuele machine met de lokale configuratie-server communiceren kan, meld u vervolgens op de virtuele machine, en controleren van de 'InMage Scout Application Service'. Als u ziet dat deze niet wordt uitgevoerd, start de service handmatig en controleer of het starttype van de service is ingesteld op automatisch.

### <a name="error-code-78052"></a>Foutcode 78052

**Beveiliging kan niet worden voltooid voor de virtuele machine.**

Dit kan gebeuren als er al een virtuele machine met dezelfde naam op de hoofddoelserver waaraan u bent niet kan terug.

Dit probleem als volgt oplossen:
1. Selecteer een andere hoofddoelserver op een andere host, zodat de beveiligingspoging maakt de machine op een andere host, waarbij de namen geen conflict veroorzaken.
2. U kunt ook vMotion het hoofddoel naar een andere host op die het conflict naam zal niet worden uitgevoerd. Als de bestaande virtuele machine een losse machine, de naam wijzigen zodat de nieuwe virtuele machine kan worden gemaakt op dezelfde ESXi-host.

### <a name="error-code-78093"></a>Foutcode 78093

**De virtuele machine wordt niet uitgevoerd in een vastgelopen status, of is niet toegankelijk.**

Als u wilt een mislukte via VM beveiligt, moet de virtuele machine in Azure worden uitgevoerd. Dit is zodat de Mobility-service wordt geregistreerd bij de configuratie server on-premises, en kan worden gestart door de communicatie met de processerver repliceren. Als de machine zich in een netwerk onjuist of is niet uitgevoerd (is vastgelopen of afsluiten), bereiken niet de Mobility-service op de virtuele machine, om te beginnen met de beveiligingspoging door de configuratieserver.

1. Start de virtuele machine opnieuw zodat deze communicatie back lokale kunt starten.
2. De taak opnieuw beveiligen opnieuw starten na het starten van de virtuele machine van Azure

### <a name="error-code-8061"></a>Foutcode 8061

**Het gegevensarchief is niet toegankelijk is vanaf ESXi-host.**

Controleer de [de hoofdsleutel van de doel-vereisten en ondersteunde gegevensarchieven](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) voor failback.


## <a name="troubleshoot-failback-errors"></a>Failback fouten oplossen

Deze sectie worden veelvoorkomende fouten tijdens de failback optreden kunnen.

### <a name="error-code-8038"></a>Foutcode 8038

**Kan niet online zetten van de on-premises virtuele machine vanwege de fout**

Dit gebeurt wanneer de lokale virtuele machine op een host die niet over voldoende geheugen ingericht wordt opgeroepen. Dit probleem oplossen:

1. Meer geheugen op de host ESXi inrichten.
2. Bovendien kunt u de virtuele machine naar een andere ESXi-host die voldoende geheugen heeft om de virtuele machine opstarten vMotion.
