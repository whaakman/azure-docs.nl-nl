---
title: Azure-VM's opnieuw beveiligen een failover naar de primaire Azure-regio met Azure Site Recovery | Microsoft Docs
description: Beschrijft hoe u opnieuw beveiligen van virtuele Azure-machines in een secundaire regio na een failover van een primaire regio, met behulp van Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: eabb7d194a3ef65282befab1ae59e85ba56f2f5b
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472164"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Azure-VM's opnieuw beveiligen een failover naar de primaire regio


Wanneer u [failover](site-recovery-failover.md) Azure VM's van de ene regio voor het gebruik van een andere [Azure Site Recovery](site-recovery-overview.md), de VM's opstart in de secundaire regio, in een niet-beveiligde status. Als u failback-VM's naar de primaire regio, moet u het volgende doen:

- De VM's in de secundaire regio opnieuw beveiligen zodat ze gaan repliceren naar de primaire regio.
- Nadat het opnieuw beveiligen is voltooid en de virtuele machines worden gerepliceerd, kunt u een failover van de secundaire naar primaire regio.

## <a name="prerequisites"></a>Vereisten
1. De VM-failover van de primaire naar secundaire regio moet worden doorgevoerd.
2. De primaire doelsite moet beschikbaar zijn en moet u kunnen openen of maken van resources in die regio.

## <a name="reprotect-a-vm"></a>Een virtuele machine opnieuw beveiligen

1. In **kluis** > **gerepliceerde items**, met de rechtermuisknop op de mislukte uitvoert voor virtuele machine en selecteert u **opnieuw beveiligen**. De richting opnieuw beveiligen moet van de secundaire naar primaire weergeven.

   ![Opnieuw beveiligen](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Controleer in het resource-groep, het netwerk, opslag en beschikbaarheid wordt ingesteld. Klik vervolgens op **OK**. Als er resources gemarkeerd als nieuw zijn, worden ze gemaakt als onderdeel van het proces opnieuw beveiligen.
3. De taak opnieuw beveiligen seeding van de doelsite van de meest recente gegevens. Nadat die is voltooid, wordt de replicatie van verschillen plaatsvindt. Vervolgens kunt u failover naar de primaire site. U kunt het storage-account selecteren of het netwerk die u wilt gebruiken tijdens het opnieuw beveiligen, met de optie aanpassen.

   ![Optie aanpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Instellingen voor opnieuw beveiligen aanpassen

U kunt de volgende eigenschappen van het doel VMe aanpassen tijdens het opnieuw beveiligen.

![Aanpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Eigenschap |Opmerkingen  |
|---------|---------|
|Doelresourcegroep     | Wijzig de doelresourcegroep waarin de virtuele machine is gemaakt. Als onderdeel van opnieuw beveiligen, de doel-VM is verwijderd. U kunt een nieuwe resourcegroep voor het maken van de virtuele machine na een failover.        |
|Virtueel doelnetwerk     | Het doelnetwerk kan niet worden gewijzigd tijdens het opnieuw beveiligen. Als u wilt wijzigen van het netwerk, de netwerktoewijzing bij het opnieuw.         |
|Doel-Opslagaccount (secundaire virtuele machine gebruikt geen beheerde schijven)     | U kunt de storage-account dat gebruikmaakt van de virtuele machine na een failover kunt wijzigen.         |
|Beheerde replicaschijven (secundaire virtuele machine maakt gebruik van beheerde schijven)    | Site Recovery maakt beheerde replicaschijven in de primaire regio voor het spiegelen van beheerde schijven van de secundaire virtuele machine.         |
|Cache Storage     | U kunt een cache-opslagaccount tijdens replicatie moet worden gebruikt. Standaard is een nieuw cache-opslagaccount worden gemaakt, als deze niet bestaat.         |
|Beschikbaarheidsset     |Als de virtuele machine in de secundaire regio deel van een beschikbaarheidsset uitmaakt, kunt u een beschikbaarheidsset voor de doel-VM in de primaire regio. Standaard, Site Recovery probeert te vinden van de bestaande beschikbaarheidsset in de primaire regio, en deze gebruiken. Aangepast, kunt u een nieuwe beschikbaarheidsset.         |


### <a name="what-happens-during-reprotection"></a>Wat gebeurt er tijdens het opnieuw beveiligen?

Standaard gebeurt het volgende:

1. Een cache-opslagaccount is gemaakt in de regio waar de failover VM wordt uitgevoerd.
2. Als het doelopslagaccount (het oorspronkelijke opslagaccount in de primaire regio) niet bestaat, wordt een nieuwe gemaakt. Naam van het toegewezen opslagaccount is de naam van het opslagaccount dat wordt gebruikt door de secundaire virtuele machine, voorafgegaan door 'asr'.
3. Als uw virtuele machine gebruikmaakt van beheerde schijven, beheerde schijven worden gemaakt in de primaire regio voor het opslaan van de gegevens van de schijven van de secundaire virtuele machine gerepliceerd.
4. Als de beschikbaarheidsset niet bestaat, wordt een nieuwe resourcegroep gemaakt als onderdeel van de taak opnieuw beveiligen indien nodig. Als u de instellingen voor opnieuw beveiligen hebt aangepast, wordt de geselecteerde verzameling gebruikt.

Wanneer u activeert een taak opnieuw beveiligen en de doel-VM zich bevindt, gebeurt het volgende:

1. De doel-kant die VM wordt uitgeschakeld als deze wordt uitgevoerd.
2. Als de virtuele machine gebruikmaakt van beheerde schijven, een kopie van de oorspronkelijke schijven worden gemaakt met '-ASRReplica' achtervoegsel. De oorspronkelijke schijven worden verwijderd. De '-ASRReplica' kopieën worden gebruikt voor replicatie.
3. Als de virtuele machine is niet-beheerde schijven gebruikt, zijn de doel-VM-gegevensschijven losgekoppeld en gebruikt voor replicatie. Een kopie van de besturingssysteemschijf is gemaakt en die op de virtuele machine is gekoppeld. Een schijf met het oorspronkelijke besturingssysteem is losgekoppeld en gebruikt voor replicatie.
4. Alleen de wijzigingen tussen de bronschijf en de doelschijf worden gesynchroniseerd. De verschillen zijn berekend door het vergelijken van beide schijven en vervolgens overgedragen. De geschatte tijd Controleer dit hieronder vinden.
5. Nadat de synchronisatie is voltooid, wordt de replicatie van verschillen begint en maakt een herstelpunt in overeenstemming met het replicatiebeleid.

Wanneer u een taak opnieuw beveiligen activeert en de doel-VM en de schijven nog niet bestaan, gebeurt het volgende:
1. Als de virtuele machine van beheerde schijven gebruikmaakt, replicaschijven worden gemaakt met '-ASRReplica' achtervoegsel. De '-ASRReplica' kopieën worden gebruikt voor replicatie.
2. Als de virtuele machine is niet-beheerde schijven gebruikt, worden replicaschijven gemaakt in het doelopslagaccount.
3. De volledige schijven worden gekopieerd van de mislukte via regio naar de nieuwe doelregio.
4. Nadat de synchronisatie is voltooid, wordt de replicatie van verschillen begint en maakt een herstelpunt in overeenstemming met het replicatiebeleid.

#### <a name="estimated-time--to-do-the-reprotection"></a>Geschatte tijd voor het opnieuw beveiligen 

In de meeste gevallen niet Azure Site Recovery worden gerepliceerd van de volledige gegevens naar de bronregio. Hieronder vindt u de voorwaarden waarmee wordt bepaald hoeveel gegevens kan worden gerepliceerd:

1.  Als de bron-VM-gegevens verwijderd, beschadigd of niet toegankelijk als gevolg van een of andere reden, zoals resourcegroep is wijzigen/verwijderen tijdens volledige IR opnieuw beveiligen vervolgens gebeurt omdat er geen gegevens beschikbaar in de regio van de gegevensbron te gebruiken.
2.  Als de bron-VM-gegevens toegankelijk zijn alleen verschillen de berekend door het vergelijken van beide schijven en vervolgens overgedragen. Controleer de onderstaande tabel om op te halen van de geschatte tijd 

|** Voorbeeld situatie ** | ** Gebruikte tijd voor het opnieuw beveiligen ** |
|--- | --- |
|Regio van de gegevensbron is 1 virtuele machine met 1 TB standard Disk<br/>-Alleen 127 GB aan gegevens wordt gebruikt en de rest van de schijf is leeg<br/>-Schijftype is standaard 60 MiB/S-doorvoer<br/>-Er zijn geen gegevens wijzigen na een failover| Bij benadering de tijd 45 minuten – 1,5 uur<br/> -Site Recovery verschijnen tijdens het opnieuw beveiligen de controlesom van de volledige gegevens waarmee 127 GB / 45 MB ~ 45 minuten<br/>-Enige overhead tijd is vereist voor Site Recovery voor automatisch schalen die 20-30 minuten<br/>-Er zijn geen kosten voor uitgaand verkeer |
|Regio van de gegevensbron is 1 virtuele machine met 1 TB standard Disk<br/>-Alleen 127 GB aan gegevens wordt gebruikt en de rest van de schijf is leeg<br/>-Schijftype is standaard 60 MiB/S-doorvoer<br/>-Gegevens worden gewijzigd na een failover 45 GB| Geschatte tijd 1 uur – 2 uur<br/>-Site Recovery verschijnen tijdens het opnieuw beveiligen de controlesom van de volledige gegevens waarmee 127 GB / 45 MB ~ 45 minuten<br/>-Transfer tijd voor het toepassen van wijzigingen van 45 GB die 45 GB / 45 MBps ~ 17 minuten<br/>-Kosten voor uitgaand verkeer geldt alleen voor 45 GB aan gegevens niet voor de controlesom|
 



## <a name="next-steps"></a>Volgende stappen

Nadat de virtuele machine is beveiligd, kunt u een failover starten. De failover de virtuele machine in de secundaire regio wordt afgesloten en wordt gemaakt en virtuele machine wordt opgestart in de primaire regio, met enkele kleine downtime. U wordt aangeraden u een tijd dienovereenkomstig kiest en dat u een testfailover uitvoeren voordat u begint een volledige failover naar de primaire site. [Meer informatie](site-recovery-failover.md) over failover.
