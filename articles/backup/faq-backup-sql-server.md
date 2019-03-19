---
title: Veelgestelde vragen over back-ups van SQL Server-databases op Azure Virtual machines met Azure Backup
description: Vind antwoorden op veelgestelde vragen over back-ups van SQL Server-databases op Azure Virtual machines met Azure Backup.
services: backup
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: sachdevaswati
ms.openlocfilehash: 3d02c8b3d763e3bd767964c8056fa948bfbb13d1
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57996048"
---
# <a name="faq-about-sql-server-databases-that-are-running-on-an-azure-vm-backup"></a>Veelgestelde vragen over SQL Server-databases die worden uitgevoerd op een Azure-VM back-up

In dit artikel vindt u antwoorden op veelgestelde vragen over back-ups van SQL Server-databases die uitvoeren op Azure virtual machines (VM's) en die gebruikmaken van de [Azure Backup](backup-overview.md) service.

> [!NOTE]
> Deze functie is momenteel beschikbaar als openbare preview-versie.

## <a name="can-i-control-as-to-how-many-concurrent-backups-run-on-the-sql-server"></a>Kan ik bepalen dat het aantal gelijktijdige back-ups op de SQL server uitvoeren?

Ja. De snelheid waarmee het back-upbeleid voor het minimaliseren van de impact op een exemplaar van SQL Server wordt uitgevoerd, kunt u beperken. Ga als volgt te werk om de instelling te wijzigen:
1. Op de SQL Server-exemplaar in de *C:\Program Files\Azure werkbelasting Backup\bin* map, maken de *ExtensionSettingsOverrides.json* bestand.
2. In de *ExtensionSettingsOverrides.json* bestand, wijzig de **DefaultBackupTasksThreshold** instellen op een lagere waarde (bijvoorbeeld 5). <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Sla uw wijzigingen op en sluit het bestand.
4. Open **Taakbeheer** op het SQL Server-exemplaar. Start de service **AzureWLBackupCoordinatorSvc** opnieuw.

> [!NOTE]
> In de UX kunt u nog eens en zo veel back-ups plannen op een bepaald moment, maar ze worden verwerkt in een sliding window van bijvoorbeeld 5, aan de hand van het bovenstaande voorbeeld.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Kan ik een volledige back-up van een secundaire replica uitvoeren?
Aan de hand van beperkingen voor SQL, kunt u exemplaar alleen volledige back-up uitvoeren op de secundaire Replica. maar volledige back-up is niet toegestaan.

## <a name="do-successful-backup-jobs-create-alerts"></a>Maken succesvolle back-uptaken waarschuwingen?
Nee. Succesvolle back-uptaken maken geen waarschuwingen. Er worden alleen waarschuwingen verzonden voor mislukte back-uptaken.

## <a name="can-i-see-scheduled-backup-jobs-in-the-jobs-menu"></a>Kan ik de geplande back-uptaken in het menu taken zien?
De **back-uptaak** menu wordt alleen weergegeven voor ad-hoc back-uptaken. Gebruik voor de geplande taak [bewaken met behulp van Azure Monitor](backup-azure-monitoring-use-azuremonitor.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>Toekomstige databases automatisch toegevoegd voor back-up?
Ja, met [automatische beveiliging](backup-azure-sql-database.md#enable-auto-protection)

## <a name="can-i-protect-availability-groups-on-premises"></a>Kan ik beschikbaarheid groepen on-premises beveiligen?
Nee. Azure Backup beschermt SQL Server-databases die worden uitgevoerd in Azure. Als u een beschikbaarheidsgroep (AG) wordt verdeeld tussen Azure en on-premises machines, kan de AG worden beveiligd, alleen als de primaire replica wordt uitgevoerd in Azure. Azure Backup beschermt ook alleen de knooppunten die worden uitgevoerd in dezelfde Azure-regio als de Recovery Services-kluis.

## <a name="can-i-protect-availability-groups-across-regions"></a>Kan ik beschikbaarheidsgroepen in regio's beveiligen?
De Azure Backup Recovery Services-kluis kunt detecteren en alle knooppunten die zich in dezelfde regio bevinden als de kluis beveiligen. Als uw SQL Server Always On-beschikbaarheidsgroep meerdere Azure-regio's omvat, stelt u de back-up van de regio waaraan het primaire knooppunt. Azure Backup kunt detecteren en beveiligen van alle databases in de beschikbaarheidsgroep op basis van uw back-upvoorkeur. Als uw back-upvoorkeur niet wordt voldaan, back-ups mislukken en krijgt u de waarschuwing voor fout.

## <a name="can-i-exclude-databases-with-autoprotection-enabled"></a>Kan ik databases uitsluiten met autoprotection ingeschakeld?
Nee. Autoprotection [is van toepassing op het gehele exemplaar](backup-azure-sql-database.md#enable-auto-protection). U kunt autoprotection niet gebruiken voor het selectief beveiligen van databases in een exemplaar.

## <a name="can-i-have-different-policies-in-an-autoprotected-instance"></a>Kan ik verschillende beleidsregels in een exemplaar autoprotected hebben?
Als uw exemplaar al een beveiligde databases bevat, blijven deze gewoon onder hun beleid worden beveiligd, ook als u [autoprotection inschakelen](backup-azure-sql-database.md#enable-auto-protection). Echter wordt alle niet-beveiligde databases en de databases die u later toevoegen slechts één beleid hebben. Definieert u dit beleid onder **back-up configureren** nadat u de databases hebt geselecteerd. In feite, in tegenstelling tot andere beveiligde databases wijzigen u niet ook het beleid voor een database die zich in een autoprotected-exemplaar.
De enige manier om het wijzigingsbeleid van die database is tijdelijk uit te schakelen autoprotection voor het exemplaar. Vervolgens u weer inschakelen autoprotection voor het exemplaar.

## <a name="if-i-delete-a-database-from-an-autoprotected-instance-will-backups-stop"></a>Als ik een database niet uit een autoprotected-exemplaar verwijderen, wordt back-ups dan stoppen?
Nee. Als een database wordt verwijderd uit een exemplaar van autoprotected, worden er nog steeds de databaseback-ups geprobeerd. Dit betekent dat de verwijderde database moeten worden weergegeven als niet in orde onder begint **back-Upitems** en nog steeds is beveiligd.

De enige manier om te stoppen met het beveiligen van deze database is tijdelijk [autoprotection uitschakelen](backup-azure-sql-database.md#enable-auto-protection) op het exemplaar. Klik vervolgens onder **back-Upitems** voor de database, selecteert u **back-up stoppen**. U autoprotection voor dit exemplaar vervolgens weer inschakelen.

##  <a name="why-cant-i-see-an-added-database-for-an-autoprotected-instance"></a>Waarom zie ik een database toegevoegd voor een exemplaar autoprotected niet?
Een database die u [toevoegen aan een exemplaar autoprotected](backup-azure-sql-database.md#enable-auto-protection) mogelijk niet direct weergegeven onder beveiligde items. Dat komt doordat de detectie doorgaans om de 8 uur wordt uitgevoerd. U kunt echter detecteren en direct nieuwe databases beveiligen als u handmatig een detectie door te selecteren uitvoeren **databases herstellen**, zoals wordt weergegeven in de volgende afbeelding.

  ![Handmatig een nieuw toegevoegde database detecteren](./media/backup-azure-sql-database/view-newly-added-database.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [maakt u een back-up van een SQL Server-database](backup-azure-sql-database.md) die wordt uitgevoerd op een Azure-VM.
