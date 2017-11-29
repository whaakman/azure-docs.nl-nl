---
title: Hoe beveiligt failover van virtuele machines terug naar de primaire Azure-regio in Azure | Microsoft Docs
description: "U kunt na een failover van virtuele machines van één Azure-regio naar een andere Azure Site Recovery gebruiken ter bescherming van de machines in omgekeerde richting. Meer informatie over de stappen procedure een beveiligt voordat opnieuw een failover."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: rajanaki
ms.openlocfilehash: 3e614b6c3c8358585f3b502f301cc659d2088e2f
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="reprotect-from-failed-over-azure-region-back-to-primary-region"></a>Azure-regio terug naar de primaire regio beveiligt van failover



>[!NOTE]
>
> Site Recovery replicatie voor virtuele machines in Azure is momenteel in preview.


## <a name="overview"></a>Overzicht
Wanneer u [failover](site-recovery-failover.md) de virtuele machines van één Azure-regio naar een andere, de virtuele machines zijn in een onbeveiligde toestand. Als u zorgen dat ze terug naar de primaire regio wilt, moet u eerst de virtuele machines en failover opnieuw te beveiligen. Er is geen verschil tussen het failover in één richting of andere. Inschakelen van de beveiliging van de virtuele machines op dezelfde manier te posten, er is geen verschil tussen de beveiligt post failover of post failback.
Om uit te leggen van de werkstromen van beveiligt, en om verwarring te voorkomen, verwijzen naar de primaire site van de beveiligde machines als Oost-Azië regio en de site recovery machines als Zuidoost-Azië regio. Tijdens de failover, wordt de virtuele machines worden opgestart in de regio Zuidoost-Azië. Voordat u failback moet u de virtuele machines van Zuidoost-Azië terug naar de Oost-Azië beveiligt. In dit artikel beschrijft de stappen voor het opnieuw beveiligen.

> [!WARNING]
> Als u hebt [migratie voltooid](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), de virtuele machine naar een andere resourcegroep verplaatst of verwijderd van de virtuele machine van Azure, u kunt geen beveiligt of failback van de virtuele machine.

Nadat beveiligt is voltooid en de beveiligde virtuele machines worden gerepliceerd, kunt u een failover op de virtuele machines zodat deze terug naar de Oost-Azië regio starten.

Opmerkingen of vragen plaatsen aan het einde van dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Vereisten
1. De virtuele machines moet zijn doorgevoerd.
2. De doelsite - in dit geval de Oost-Azië Azure-regio moet beschikbaar zijn en moet toegang/maken van nieuwe resources in deze regio.

## <a name="steps-to-reprotect"></a>Stappen voor het opnieuw beveiligen

Hier volgen de stappen voor een virtuele machine met de standaardinstellingen opnieuw beveiligen.

1. In **kluis** > **gerepliceerde items**, met de rechtermuisknop op de virtuele machine die failover uitgevoerd is is en selecteer vervolgens **opnieuw beveiligen**. U kunt ook klikt u op de machine en selecteer **opnieuw beveiligen** van de opdrachtknoppen.

![Klik met de rechtermuisknop om het opnieuw beveiligen](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. U ziet dat de blade de richting van beveiliging, **naar Oost-Azië Zuidoost-Azië**, is al geselecteerd.

![Blade opnieuw beveiligen](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Controleer de **resourcegroep, netwerk-, opslag- en beschikbaarheid sets** informatie en klik op OK. Als er geen bronnen gemarkeerd (nieuw), wordt ze worden gemaakt als onderdeel van de beveiligt.

De taak opnieuw beveiligen seeding van de doelsite (SEA in dit geval) met de meest recente gegevens eerst verzorgt en zodra dat is voltooid, wordt de delta's voordat u failover repliceren naar Zuidoost-Azië.

### <a name="reprotect-customization"></a>Aanpassing beveiligt
Als u het extract storage-account of het netwerk kiezen tijdens het beveiligt wilt, kunt u doen met behulp van de optie aanpassen die op de blade opnieuw beveiligen.

![Optie aanpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

U kunt de volgende eigenschappen van de doel-virtuele machine aanpassen tijdens het opnieuw beveiligen.

![Blade aanpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Eigenschap |Opmerkingen  |
|---------|---------|
|Doelresourcegroep     | U kunt wijzigen van de doelresourcegroep waarin de virtuele machine wordt gemaakt. Als het onderdeel van het beveiligt, de virtuele doelmachine worden verwijderd, daarom kunt u een nieuwe resourcegroep van de virtuele machine na failover maken         |
|Doel virtueel netwerk     | Netwerk kan niet worden gewijzigd tijdens de beveiligt jb. Als u wilt wijzigen in het netwerk, de netwerktoewijzing bij het opnieuw.         |
|Doelopslag     | U kunt het storage-account waarnaar de virtuele machine wordt gemaakt na failover wijzigen.         |
|Cache-opslag     | U kunt opgeven dat een cache-storage-account die wordt gebruikt tijdens de replicatie. Als u de verbinding met de standaardinstellingen, wordt een nieuw opslagaccount voor de cache gemaakt, als deze niet al bestaat.         |
|Beschikbaarheidsset     |Als de virtuele machine in Oost-Azië deel van een beschikbaarheidsset uitmaakt, kunt u een beschikbaarheidsset voor de virtuele doelmachine in Zuidoost-Azië. Standaardinstellingen van de bestaande beschikbaarheidsset voor SEA zoekt en probeert te gebruiken. Aangepast, kunt u een volledig nieuw AV-set.         |


### <a name="what-happens-during-reprotect"></a>Wat er gebeurt tijdens het opnieuw beveiligen?

Net zoals nadat de eerste beveiliging inschakelt, volgen de artefacten die worden gemaakt als u de standaardinstellingen gebruikt.
1. Een opslagaccount van de cache wordt gemaakt in de regio Oost-Azië.
2. Als het doel-storage-account (het oorspronkelijke opslagaccount van de VM Zuidoost-Azië) niet bestaat, wordt een nieuw gemaakt. De naam is van virtuele machine van de Oost-Azië opslagaccount voorafgegaan door 'asr'.
3. Als de doelset AV niet bestaat en de standaardinstellingen die detecteren het moet een nieuwe set met AV maken en deze wordt gemaakt als onderdeel van de taak opnieuw beveiligen. Als u hebt aangepast de beveiligt, wordt de geselecteerde AV-set worden gebruikt.
4.

Hieronder vindt u de lijst met stappen die ontstaan wanneer u een taak beveiligt opnieuw activeren. Dit is in het geval dat de virtuele doelmachine van de kant bestaat.

1. De vereiste artefacten worden gemaakt als onderdeel van het beveiligt. Als ze al aanwezig zijn, worden ze hergebruikt.
2. De virtuele doelmachine van de kant (Zuidoost-Azië) wordt eerst uitgeschakeld, als deze wordt uitgevoerd.
3. Schijf doel aan clientzijde van de virtuele machine wordt door Azure Site Recovery gekopieerd naar een container als een seed-blob.
4. De virtuele doelmachine van de zijde wordt vervolgens verwijderd.
5. De seed-blob wordt gebruikt door de huidige virtuele machine van de bron aan clientzijde (Oost-Azië) om te repliceren. Dit zorgt ervoor dat alleen de delta's worden gerepliceerd.
6. Grote wijzigingen tussen de bronschijf en de blob seed worden gesynchroniseerd. Dit kan enige tijd duren.
7. Zodra de taak opnieuw beveiligen is voltooid, begint de replicatie van verschillen die u een herstelpunt dat aan de hand van het beleid maakt.

> [!NOTE]
> U kunt geen op een plan herstel niveau. U kunt alleen opnieuw beveiligen op een per VM-niveau.

Nadat de taak opnieuw beveiligen slaagt, wordt de virtuele machine een beveiligde status invoeren.

## <a name="next-steps"></a>Volgende stappen

Nadat de virtuele machine heeft een beschermd zijn ingevoerd, kunt u een failover starten. De failover sluit de virtuele machine in Azure Oost-Azië regio en vervolgens maakt en start de virtuele machine op Zuidoost-Azië regio. Daarom is er een kleine uitvaltijd voor de toepassing. Kies de tijd voor failover zodat, wanneer uw toepassing een uitvaltijd kan tolereren. Het is raadzaam eerst de failover van de virtuele machine om ervoor te zorgen dat dit komt goed voordat u begint een failover te testen.

-   [Stappen voor het initiëren testen failover van de virtuele machine](site-recovery-test-failover-to-azure.md)

-   [Stappen voor de failover van de virtuele machine starten](site-recovery-failover.md)
