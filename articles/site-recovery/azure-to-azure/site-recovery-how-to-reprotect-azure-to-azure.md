---
title: Hoe beveiligt failover van virtuele machines terug naar de primaire Azure-regio in Azure | Microsoft Docs
description: "U kunt na een failover van virtuele machines van één Azure-regio naar een andere Azure Site Recovery gebruiken ter bescherming van de machines in omgekeerde richting. Meer informatie over de stappen procedure een beveiligt voordat opnieuw een failover."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 0004323fee44c1433cdd3c39fc1fa7dad965dbf7
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="reprotect-azure-vms-back-to-the-primary-region"></a>Beveiligt virtuele Azure-machines terug naar de primaire regio



>[!NOTE]
>
> Replicatie van site Recovery voor Azure virtual machines (VM's) is momenteel in preview.


Wanneer u [failover](../site-recovery-failover.md) VM's van een Azure-regio naar een andere, failover VM's zijn in een onbeveiligde toestand. Als u zorgen dat ze terug naar de primaire regio wilt, moet u eerst de virtuele machines repliceren en voert vervolgens failover opnieuw. Er is geen verschil in failover in één richting of andere. Op deze manier nadat u replicatie en inschakelt is er geen verschil tussen beveiligingspoging na een failover of post failback.

Het proces waarvoor uitleggen als voorbeeld gaan we ervan uit dat de primaire site van de beveiligde virtuele machines Oost-Azië regio is en de site recovery Zuidoost-Azië. Tijdens de failover schakelt u over de virtuele machines voor de regio Zuidoost-Azië. Voordat u een failback uit, moet u de virtuele machines van Zuidoost-Azië, repliceren naar Oost-Azië. In dit artikel beschrijft de stappen die nodig zijn voor beveiligingspoging.

> [!WARNING]
> Als u de migratie voltooid en de virtuele machine verplaatst naar een andere resourcegroep (of verwijderd van virtuele machine van Azure) kunt geen u failback.

Nadat de beveiligingspoging is voltooid en repliceren van de virtuele machines, kunt u een failover op de virtuele machines, zodat u deze terug naar de regio Oost-Azië starten.

## <a name="prerequisites"></a>Vereisten
1. De virtuele machine moet zijn doorgevoerd.
2. De doelsite (Oost-Azië) moet beschikbaar zijn en moet u kunnen toegang/maken van nieuwe resources in deze regio.

## <a name="reprotect"></a>Opnieuw beveiligen

Volg deze stappen voor een virtuele machine met de standaardinstellingen opnieuw beveiligen.

1. In **kluis** > **gerepliceerde items**, met de rechtermuisknop op de virtuele machine met failover en selecteer vervolgens **opnieuw beveiligen**. U kunt ook klikt u op de machine en selecteer **opnieuw beveiligen** van de opdrachtknoppen.

  ![Opnieuw beveiliging inschakelen](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Controleer de replicatierichting **naar Oost-Azië Zuidoost-Azië**, is geselecteerd.

  ![Opnieuw beveiligen](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Controleer de **resourcegroep, netwerk-, opslag- en beschikbaarheid sets** informatie en klik op **OK**. Als er geen bronnen gemarkeerd (nieuw), moeten ze worden gemaakt tijdens de beveiligingspoging.

Dit activeert een taak beveiligingspoging die de seeding van de doelsite met de meest recente gegevens, en zodra dat is voltooid, de delta's repliceert voordat u een failover naar Zuidoost-Azië.

### <a name="reprotect-customization"></a>Aanpassing beveiligt
Als u het extract storage-account of het netwerk kiezen tijdens het beveiligt wilt, kunt u doen met behulp van de optie aanpassen op de pagina beveiligingspoging opgegeven.

![Optie aanpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

U kunt de volgende eigenschappen van de virtuele doelmachine tijdens beveiligingspoging aanpassen.

![Aanpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Eigenschap |Opmerkingen  |
|---------|---------|
|Doelresourcegroep     | U kunt wijzigen van de doelresourcegroep waarin de virtuele machine wordt gemaakt. Als het onderdeel van het beveiligt, de virtuele doelmachine worden verwijderd, daarom kunt u een nieuwe resourcegroep van de virtuele machine na failover maken         |
|Doel virtueel netwerk     | Netwerk kan niet worden gewijzigd tijdens beveiligingspoging. Als u wilt wijzigen in het netwerk, de netwerktoewijzing bij het opnieuw.         |
|Doelopslag     | U kunt het storage-account waarnaar de virtuele machine wordt gemaakt na failover wijzigen.         |
|Cache-opslag     | U kunt opgeven dat een cache-storage-account die wordt gebruikt tijdens de replicatie. Als u de verbinding met de standaardinstellingen, wordt een nieuw opslagaccount voor de cache gemaakt, als deze niet al bestaat.         |
|Beschikbaarheidsset     |Als de virtuele machine in Oost-Azië deel van een beschikbaarheidsset uitmaakt, kunt u een beschikbaarheidsset voor de virtuele doelmachine in Zuidoost-Azië. Standaardinstellingen van de bestaande beschikbaarheidsset voor SEA zoekt en probeert te gebruiken. Aangepast, kunt u een volledig nieuw AV-set.         |


### <a name="what-happens-during-reprotect"></a>Wat er gebeurt tijdens het opnieuw beveiligen?

Net zoals nadat de eerste beveiliging inschakelt, volgen de artefacten die worden gemaakt als u de standaardinstellingen gebruikt.
1. Een opslagaccount van de cache wordt gemaakt in de regio Oost-Azië.
2. Als het doel-storage-account (het oorspronkelijke opslagaccount van de VM Zuidoost-Azië) niet bestaat, wordt een nieuw gemaakt. De naam is van virtuele machine van de Oost-Azië opslagaccount voorafgegaan door 'asr'.
3. Als de doelset AV niet bestaat en de standaardinstellingen die detecteren moeten worden gemaakt van een nieuwe set met AV en deze wordt gemaakt als onderdeel van de taak opnieuw beveiliging inschakelen op. Als u beveiligingspoging hebt aangepast, wordt de geselecteerde AV-set worden gebruikt.


Hieronder vindt u de lijst met stappen die ontstaan wanneer u een taak beveiligingspoging activeren. Dit is in het geval dat de virtuele doelmachine van de kant bestaat.

1. De vereiste artefacten worden gemaakt als onderdeel van het beveiligt. Als ze al aanwezig zijn, worden ze hergebruikt.
2. De virtuele doelmachine van de kant (Zuidoost-Azië) wordt eerst uitgeschakeld, als deze wordt uitgevoerd.
3. Schijf doel aan clientzijde van de virtuele machine wordt door Azure Site Recovery gekopieerd naar een container als een seed-blob.
4. De virtuele doelmachine van de zijde wordt vervolgens verwijderd.
5. De seed-blob wordt gebruikt door de huidige virtuele machine van de bron aan clientzijde (Oost-Azië) om te repliceren. Dit zorgt ervoor dat alleen de delta's worden gerepliceerd.
6. Grote wijzigingen tussen de bronschijf en de blob seed worden gesynchroniseerd. Dit kan enige tijd duren.
7. Zodra de taak opnieuw beveiliging inschakelen is voltooid, begint de replicatie van verschillen die u een herstelpunt dat aan de hand van het beleid maakt.

> [!NOTE]
> U kunt geen op een plan herstel niveau. U kunt alleen opnieuw beveiligen op een per VM-niveau.

Nadat de beveiligingspoging is geslaagd, wordt de virtuele machine een beveiligde status invoeren.

## <a name="next-steps"></a>Volgende stappen

Nadat de virtuele machine heeft een beschermd zijn ingevoerd, kunt u een failover starten. De failover sluit de virtuele machine in Azure Oost-Azië regio en vervolgens maakt en start de virtuele machine op Zuidoost-Azië regio. Daarom is er een kleine uitvaltijd voor de toepassing. Kies de tijd voor failover zodat, wanneer uw toepassing een uitvaltijd kan tolereren. Het wordt aangeraden dat u een testfailover van de virtuele machine eerst uitvoert, om te controleren of dit komt goed voordat u een failover start.

-   [Stappen voor het initiëren testen failover van de virtuele machine](../site-recovery-test-failover-to-azure.md)

-   [Stappen voor de failover van de virtuele machine starten](../site-recovery-failover.md)
