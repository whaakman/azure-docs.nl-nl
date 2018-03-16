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
ms.openlocfilehash: 480c3524ad4fb8a8c6ea02f09b8d27f254da9b08
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="troubleshoot-failback-from-azure-to-vmware"></a>Failback vanuit Azure naar VMware oplossen

Dit artikel wordt beschreven hoe u tegenkomen kunt wanneer u weer Azure VM's naar uw on-premises VMware-infrastructuur na een failover naar Azure met behulp van mislukken problemen [Azure Site Recovery](site-recovery-overview.md).

Failback omvat in principe twee belangrijke stappen. Voor de eerste stap moet na een failover u virtuele Azure-machines beveiligt met on-premises, zodat deze beginnen te repliceren. De tweede stap is het uitvoeren van een failover van Azure naar de failback naar uw on-premises site.

## <a name="troubleshoot-reprotection-errors"></a>Beveiligingspoging fouten oplossen

Deze sectie details veelvoorkomende beveiligingspoging fouten en hoe deze corrigeren.

### <a name="error-code-95226"></a>Foutcode 95226

**Beveiligt, is mislukt omdat de virtuele machine van Azure niet kunnen bereiken van de lokale configuratie-server.**

Deze fout treedt op wanneer:

* De virtuele machine van Azure kan de configuratie van lokale server niet bereiken. De virtuele machine kan niet worden gedetecteerd en geregistreerd voor de configuratieserver.
* De InMage Scout application service wordt niet uitgevoerd op de Azure VM na een failover. De service is vereist voor communicatie met de lokale configuratie-server.

Dit probleem oplossen:

* Controleer dat het Azure VM-netwerk de virtuele machine van Azure om te communiceren met de configuratieserver lokale toestaat. U kunt een site-naar-site VPN instellen voor uw on-premises datacentrum of configureren van een Azure ExpressRoute-verbinding met de persoonlijke peering in het virtuele netwerk van de Azure VM.
* Als de virtuele machine met de lokale configuratie-server communiceren kan, moet u zich aanmelden bij de virtuele machine. Controleer vervolgens de InMage Scout application service. Als u ziet dat deze niet wordt uitgevoerd, start u de service handmatig. Controleer de service starttype is ingesteld op **automatische**.

### <a name="error-code-78052"></a>Foutcode 78052

**Beveiliging kan niet worden voltooid voor de virtuele machine.**

Dit probleem kan gebeuren als er al een virtuele machine met dezelfde naam op de hoofddoelserver waaraan u bent niet kan terug.

Dit probleem oplossen:

* Selecteer een andere hoofddoelserver op een andere host zodat beveiligingspoging de machine op een andere host maakt, waarbij de naam niet conflicteren.
* U kunt ook vMotion gebruiken het hoofddoel verplaatsen naar een andere host waar het conflict naam zal niet worden uitgevoerd. Als de bestaande virtuele machine een losse machine, de naam wijzigen zodat de nieuwe virtuele machine kan worden gemaakt op dezelfde ESXi-host.


### <a name="error-code-78093"></a>Foutcode 78093

**De virtuele machine wordt niet uitgevoerd in een vastgelopen status, of is niet toegankelijk.**

Dit probleem oplossen:

Als u wilt een failover-virtuele machine beveiligt, moet de Azure VM worden uitgevoerd zodat Mobility-Service wordt geregistreerd bij de configuratie server on-premises en kunnen starten door de communicatie met de processerver repliceren. Als de machine zich op een onjuist wordt niet uitgevoerd (vastgelopen status- of afsluiten omlaag) of via het netwerk, kan de configuratieserver Mobility-Service op de virtuele machine om te beginnen met de beveiligingspoging niet bereiken.

* Start de virtuele machine opnieuw zodat deze communicatie back lokale kunt starten.
* De taak opnieuw beveiligen opnieuw starten na het starten van de virtuele machine van Azure.

### <a name="error-code-8061"></a>Foutcode 8061

**Het gegevensarchief is niet toegankelijk is vanaf ESXi-host.**

Controleer de [de hoofdsleutel van de doel-vereisten en ondersteunde gegevensarchieven](vmware-azure-reprotect.md#deploy-a-separate-master-target-server) voor failback.


## <a name="troubleshoot-failback-errors"></a>Failback fouten oplossen

Deze sectie worden veelvoorkomende fouten tijdens de failback optreden kunnen.

### <a name="error-code-8038"></a>Foutcode 8038

**Kan niet online zetten van de on-premises virtuele machine vanwege de fout.**

Dit probleem treedt op als de lokale virtuele machine op een host die niet over voldoende geheugen ingericht wordt opgeroepen. 

Dit probleem oplossen:

* Meer geheugen op de host ESXi inrichten.
* Bovendien kunt u vMotion verplaatsen van de virtuele machine naar een andere ESXi-host die voldoende geheugen heeft om de virtuele machine worden opgestart.
