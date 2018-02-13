---
title: Azure Virtual machines terug naar de primaire Azure-regio met Azure Site Recovery beveiligt failover | Microsoft Docs
description: Hierin wordt beschreven hoe virtuele Azure-machines in een secundaire regio na failover van een primaire regio, met Azure Site Recovery beveiligt.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: rajanaki
ms.openlocfilehash: d24376c57c468a562fc6d6dd52b4e9b01b53c3da
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Azure Virtual machines beveiligt failover naar de primaire regio


>[!NOTE]
>
> Replicatie van site Recovery voor Azure VM's is momenteel in preview.



Wanneer u [failover](site-recovery-failover.md) Azure virtuele machines van één regio is voor het gebruik van een andere [Azure Site Recovery](site-recovery-overview.md), de virtuele machines opstart in de secundaire regio, in een onbeveiligde toestand. Als de virtuele machines mislukken terug naar de primaire regio, moet u het volgende doen:

- Beveilig de virtuele machines in de secundaire regio opnieuw zodat deze beginnen te repliceren naar de primaire regio. 
- Nadat de beveiligingspoging is voltooid en repliceren van de virtuele machines, kunt u ze failover van de secundaire naar primaire regio.

> [!WARNING]
> Als u hebt [gemigreerd](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration) machines van de primaire naar de secundaire regio verplaatst van de virtuele machine naar een andere resourcegroep of is verwijderd van de Azure VM, u kan niet opnieuw beveiligen van de virtuele machine of failback uit.


## <a name="prerequisites"></a>Vereisten
1. De VM-failover van de primaire naar secundaire regio moet worden doorgevoerd.
2. De primaire doelsite moet beschikbaar zijn en moet u kunnen openen of maken van resources in deze regio.

## <a name="reprotect-a-vm"></a>Een virtuele machine opnieuw beveiligen

1. In **kluis** > **gerepliceerde items**, met de rechtermuisknop op de mislukte via de virtuele machine en selecteer **opnieuw beveiligen**. De richting van de beveiligingspoging moet van secundaire op primaire weergeven. 

  ![Opnieuw beveiligen](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Controleer de resource-sets voor groep-, netwerk-, opslag- en beschikbaarheid. Klik vervolgens op **OK**. Als er geen bronnen die zijn gemarkeerd als nieuw, worden ze gemaakt als onderdeel van het proces opnieuw beveiliging inschakelen.
3. De taak waarvoor de seeding van de doelsite met de meest recente gegevens. Nadat die is voltooid, wordt de replicatie van verschillen uitgevoerd. Vervolgens kunt u failover terug naar de primaire site. U kunt het storage-account selecteren of het netwerk die u wilt gebruiken tijdens beveiligt, met de optie aanpassen.

  ![Optie aanpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Beveiligt instellingen aanpassen

U kunt de volgende eigenschappen van het doel VMe tijdens beveiligingspoging aanpassen.

![Aanpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Eigenschap |Opmerkingen  |
|---------|---------|
|Doelresourcegroep     | Wijzig de doelresourcegroep waarin de virtuele machine wordt gemaakt. Als het onderdeel van de beveiligingspoging, het doel-virtuele machine wordt verwijderd. U kunt een nieuwe resourcegroep waaronder wilt maken van de virtuele machine na een failover.        |
|Doel virtueel netwerk     | Het doelnetwerk kan niet worden gewijzigd tijdens de taak opnieuw beveiligen. Als u wilt wijzigen in het netwerk, de netwerktoewijzing bij het opnieuw.         |
|Doelopslag     | U kunt het opslagaccount dat gebruikmaakt van de virtuele machine na een failover kunt wijzigen.         |
|Cache-opslag     | U kunt opgeven dat een opslagaccount van de cache moet worden gebruikt tijdens de replicatie. Standaard is een nieuw opslagaccount voor de cache worden gemaakt, als deze niet bestaat.         |
|Beschikbaarheidsset     |Als de virtuele machine in de secundaire regio deel van een beschikbaarheidsset uitmaakt, kunt u een beschikbaarheidsset voor het VM-doel voor de primaire regio kiezen. Standaard Site Recovery gezocht naar de bestaande beschikbaarheidsset voor de primaire regio en deze gebruiken. Aangepast, kunt u een nieuwe beschikbaarheidsset.         |


### <a name="what-happens-during-reprotection"></a>Wat er gebeurt tijdens beveiligingspoging?

Standaard gebeurt het volgende:

1. Een opslagaccount van de cache wordt gemaakt in de primaire regio
2. Als het doel-storage-account (het oorspronkelijke opslagaccount in de primaire regio) niet bestaat, wordt een nieuw gemaakt. Naam van het toegewezen opslagaccount is de naam van het opslagaccount dat wordt gebruikt door de secundaire virtuele machine, voorafgegaan door 'asr'.
3. Als de doel-beschikbaarheidsset niet bestaat, wordt een nieuw gemaakt als onderdeel van de taak opnieuw beveiligen indien nodig. Als u de instellingen waarvoor hebt aangepast, wordt de geselecteerde set gebruikt.

Wanneer u een taak opnieuw beveiligen en het doel dat VM bestaat activeert, gebeurt het volgende:

1. De vereiste onderdelen worden gemaakt als onderdeel van het beveiligt. Als ze al aanwezig zijn, worden ze opnieuw gebruikt.
2. De doel-zijde die VM is uitgeschakeld als deze wordt uitgevoerd.
3. De doelschijf side-VM wordt als een seed-blob naar een container gekopieerd door Site Recovery.
4. De doel-zijde VM wordt vervolgens verwijderd.
5. De seed-blob wordt gebruikt door de huidige bron zijde (secundair) VM te repliceren. Dit zorgt ervoor dat alleen de delta's worden gerepliceerd.
6. Grote wijzigingen tussen de bronschijf en de blob seed worden gesynchroniseerd. Dit kan enige tijd duren.
7. Nadat de taak opnieuw beveiligen is voltooid, wordt de replicatie van verschillen begint en maakt een herstelpunt in overeenstemming met het replicatiebeleid.
8. Nadat de taak opnieuw beveiligen is geslaagd, krijgt de virtuele machine in een beveiligde status.

## <a name="next-steps"></a>Volgende stappen

Nadat de virtuele machine is beveiligd, kunt u een failover starten. De failover de virtuele machine in de secundaire regio wordt afgesloten en wordt gemaakt en VM wordt opgestart in de primaire regio, met kleine uitval. Het is raadzaam een tijd dienovereenkomstig kiest en dat u een testfailover uitvoeren, maar het initiëren van een volledige failover naar de primaire site. [Meer informatie](site-recovery-failover.md) over failover.

