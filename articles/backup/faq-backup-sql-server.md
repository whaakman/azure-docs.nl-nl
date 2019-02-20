---
title: Veelgestelde vragen over back-ups van SQL Server-databases op Azure Virtual machines met Azure Backup
description: Vindt u antwoorden op veelgestelde vragen over back-ups van SQL Server-databases op Azure Virtual machines met Azure Backup.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: sogup
ms.openlocfilehash: a14406733ff60d53d4bf7792ff0c9a015c57d9b3
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430778"
---
# <a name="faq-on-sql-server-running-on-azure-vm-backup"></a>Veelgestelde vragen over SQL Server op Azure VM backup

In dit artikel vindt u antwoorden op veelgestelde vragen over back-ups van SQL Server-databases die worden uitgevoerd op virtuele Azure-machines met de [Azure Backup](backup-overview.md) service.

> [!NOTE]
> Deze functie is momenteel in openbare preview.



## <a name="can-i-throttle-the-backup-speed"></a>Kan ik de snelheid van de back-up beperken?

Ja. De snelheid waarmee het back-upbeleid voor het minimaliseren van de impact op een exemplaar van SQL Server wordt uitgevoerd, kunt u beperken. Ga als volgt te werk om de instelling te wijzigen:
1. Maak op het SQL Server-exemplaar in de *map C:\Program Files\Azure Workload Backup\bin* het bestand **ExtensionSettingsOverrides.json**.
2. Wijzig in het bestand **ExtensionSettingsOverrides.json** de instelling **DefaultBackupTasksThreshold** in een lagere waarde (bijvoorbeeld 5). <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Sla uw wijzigingen op. Sluit het bestand.
4. Open **Taakbeheer** op het SQL Server-exemplaar. Start de service **AzureWLBackupCoordinatorSvc** opnieuw.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Kan ik een volledige back-up van een secundaire replica uitvoeren?
Nee. Deze functie wordt niet ondersteund.

## <a name="do-successful-backup-jobs-create-alerts"></a>Maken succesvolle back-uptaken waarschuwingen?

Nee. Succesvolle back-uptaken maken geen waarschuwingen. Er worden alleen waarschuwingen verzonden voor mislukte back-uptaken.

## <a name="can-i-see-scheduled-backup-jobs-in-the-jobs-menu"></a>Kan ik de geplande back-uptaken in het menu taken zien?

Nee. De **back-uptaken** menu geeft taakdetails op aanvraag, maar geen geplande back-uptaken. Als een geplande back-uptaak mislukt, zijn de details beschikbaar in de waarschuwingen voor mislukte taken. Voor het bewaken van alle geplande en ad-hocback-uptaken gebruikt u [SQL Server Management Studio](manage-monitor-sql-database-backup.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>Toekomstige databases automatisch toegevoegd voor back-up?

Nee. Wanneer u de beveiliging voor een SQL Server-exemplaar configureert op serverniveau, worden alle databases toegevoegd. Als u databases aan een SQL Server-exemplaar toevoegt na het configureren van de beveiliging, moet u de nieuwe databases handmatig toevoegen om ze te beveiligen. De databases worden niet automatisch opgenomen in de geconfigureerde beveiliging.

##  <a name="how-do-i-restart-protection-after-changing-recovery-type"></a>Hoe ik beveiliging na het wijzigen van type herstelbewerking opnieuw starten?

Activeer een volledige back-up. Logboekback-ups worden gestart zoals verwacht.

## <a name="can-i-protect-availability-groups-on-premises"></a>Kan ik Availability Groups on-premises beveiligen?

Nee. Azure Backup beveiligt SQL Server-exemplaren die worden uitgevoerd in Azure. Als een beschikbaarheidsgroep wordt verdeeld over Azure en on-premises machines, kan de beschikbaarheidsgroep alleen worden beveiligd als de primaire replica wordt uitgevoerd in Azure. Azure Backup beveiligt ook alleen de knooppunten die worden uitgevoerd in dezelfde Azure-regio als de Recovery Services-kluis.

## <a name="can-i-protect-availability-groups-across-regions"></a>Kan ik Availability Groups beveiligen in regio's?

Met de Recovery Services-kluis van Azure Backup kunt u alle knooppunten detecteren en beveiligen die zich in dezelfde regio als de Recovery Services-kluis bevinden. Als u een SQL AlwaysOn-beschikbaarheidsgroep hebt die meerdere Azure-regio's omvat, moet u de back-up configureren vanuit de regio met het primaire knooppunt. Azure Backup kan alle databases in de beschikbaarheidsgroep detecteren en beveiligen op basis van de back-upvoorkeur. Als niet aan de back-upvoorkeur wordt voldaan, zullen de back-ups mislukken en wordt u gewaarschuwd voor fouten.

## <a name="can-i-exclude-databases-with-auto-protection-enabled"></a>Kan ik databases uitsluiten met automatische beveiliging is ingeschakeld?

Nee, [automatische beveiliging](backup-azure-sql-database.md#enable-auto-protection) is van toepassing op het hele exemplaar. U kunt niet selectief databases op een exemplaar beveiligen met behulp van automatische beveiliging.

## <a name="can-i-have-different-policies-in-an-auto-protected-instance"></a>Kan ik verschillende beleidsregels hebben in een exemplaar automatisch wordt beveiligd?

Als u al beveiligde databases op een exemplaar hebt, blijven deze beveiligd met hun respectievelijke beleid, ook als u de optie voor [automatische beveiliging](backup-azure-sql-database.md#enable-auto-protection) inschakelt (met **ON**). Alle niet-beveiligde databases en de databases die u in de toekomst toevoegt, hebben echter slechts één beleid: het beleid dat u definieert onder **Back-up configureren** nadat de databases zijn geselecteerd. In feite kunt u, in tegenstelling tot andere beveiligde databases, het beleid voor een database onder een exemplaar met automatische beveiliging niet wijzigen.
De enige manier om dat te doen, is de automatische beveiliging voor het exemplaar uit te schakelen en vervolgens het beleid voor die database te wijzigen. Nu kunt u de automatische beveiliging voor dit exemplaar weer inschakelen.

## <a name="if-i-delete-a-database-from-auto-protection-will-backups-stop"></a>Als ik een database niet uit de automatische beveiliging verwijderen, back-ups niet meer?

Nee, als een database wordt verwijderd van een exemplaar met automatische beveiliging, worden de back-ups voor die database nog steeds uitgevoerd. Dit betekent dat de verwijderde database wordt weergegeven als 'niet in orde' onder **Back-upitems** en nog steeds wordt behandeld alsof deze is beveiligd.

De enige manier om de beveiliging van deze database te stoppen, is de [automatische beveiliging](backup-azure-sql-database.md#enable-auto-protection) van het exemplaar voorlopig te stoppen en vervolgens **Back-up stoppen** te kiezen onder **Back-upitems** voor die database. Nu kunt u de automatische beveiliging voor dit exemplaar weer inschakelen.

##  <a name="why-cant-i-see-an-added-database-for-an-auto-protected-instance"></a>Waarom zie ik een database toegevoegd voor een exemplaar automatisch wordt beveiligd niet?

Mogelijk ziet u niet een nieuw toegevoegde database naar een [automatisch beveiligd](backup-azure-sql-database.md#enable-auto-protection) exemplaar beveiligd direct onder beveiligde items. Dat komt doordat de detectie doorgaans om de 8 uur wordt uitgevoerd. De gebruiker kan echter databases handmatig detecteren met behulp van de optie **DB's herstellen** om nieuwe databases onmiddellijk te detecteren en beveiligen, zoals u kunt zien in de onderstaande afbeelding:

  ![Zojuist toegevoegde database weergeven](./media/backup-azure-sql-database/view-newly-added-database.png)

## <a name="next-steps"></a>Volgende stappen

[Leer hoe u](backup-azure-sql-database.md) maakt u een back-up van een SQL Server-database die wordt uitgevoerd op een Azure-VM.
