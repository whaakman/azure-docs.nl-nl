---
title: Veelgestelde vragen over back-ups van SQL Server-databases op Azure Virtual machines met Azure Backup
description: Vind antwoorden op veelgestelde vragen over back-ups van SQL Server-databases op Azure Virtual machines met Azure Backup.
services: backup
author: sachdevaswati
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: sachdevaswati
ms.openlocfilehash: 8d6323c73e5313a29b7b0df09ebdd24a190879f5
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58876425"
---
# <a name="faq-about-sql-server-databases-that-are-running-on-an-azure-vm-backup"></a>Veelgestelde vragen over SQL Server-databases die worden uitgevoerd op een Azure-VM back-up

In dit artikel vindt u antwoorden op veelgestelde vragen over back-ups van SQL Server-databases die uitvoeren op Azure virtual machines (VM's) en die gebruikmaken van de [Azure Backup](backup-overview.md) service.

## <a name="does-the-solution-retry-or-auto-heal-the-backups"></a>De oplossing opnieuw proberen of de back-ups automatisch-genezen?

Onder bepaalde omstandigheden, wordt de Azure Backup-service geactiveerd corrigerende back-ups. Automatisch-herstel knooppuntservice kunnen gebeuren voor elk van de zes onderstaande voorwaarden:

  - Als het logboek of de differentiële back-up is mislukt vanwege een fout tijdens het valideren van LSN, wordt in plaats daarvan volgende logboek of de differentiële back-up geconverteerd naar een volledige back-up.
  - Als er geen volledige back-up is opgetreden voordat een logboek of de differentiële back-up, wordt in plaats daarvan dat logboek of de differentiële back-up geconverteerd naar een volledige back-up.
  - Als de meest recente volledige back-up van point-in-time ouder dan 15 dagen is, wordt in plaats daarvan de volgende aanmelding of differentiële back-up geconverteerd naar een volledige back-up.
  - De back-uptaken die te worden geannuleerd vanwege een upgrade van een extensie zijn opnieuw geactiveerd nadat de upgrade is voltooid en de extensie wordt gestart.
  - Als u ervoor kiest om de database tijdens het terugzetten, het volgende logboekbestand/differentiële back-up mislukt en een volledige back-up in plaats daarvan wordt geactiveerd.
  - Een volledige opgehaald in gevallen waarin een volledige back-up is vereist voor het opnieuw instellen van het logboek-ketens vervaldatum in database-herstelmodel wijzigen, automatisch geactiveerd op het volgende schema.

Automatisch-herstel knooppuntservice als een functie is ingeschakeld voor alle gebruikers standaard; Maar als u kiest voor opt-out van it, voert u de onderstaande:

  * Op de SQL Server-exemplaar in de *C:\Program Files\Azure werkbelasting Backup\bin* map maken of bewerken van de **ExtensionSettingsOverrides.json** bestand.
  * In de **ExtensionSettingsOverrides.json**, stel *{"EnableAutoHealer": false}*.
  * Sla uw wijzigingen op en sluit het bestand.
  * Open op de SQL Server-exemplaar **taak beheren** en start vervolgens opnieuw de **AzureWLBackupCoordinatorSvc** service.  

## <a name="can-i-control-as-to-how-many-concurrent-backups-run-on-the-sql-server"></a>Kan ik bepalen dat het aantal gelijktijdige back-ups op de SQL server uitvoeren?

Ja. De snelheid waarmee het back-upbeleid voor het minimaliseren van de impact op een exemplaar van SQL Server wordt uitgevoerd, kunt u beperken. Ga als volgt te werk om de instelling te wijzigen:
1. Op de SQL Server-exemplaar in de *C:\Program Files\Azure werkbelasting Backup\bin* map, maken de *ExtensionSettingsOverrides.json* bestand.
2. In de *ExtensionSettingsOverrides.json* bestand, wijzig de **DefaultBackupTasksThreshold** instellen op een lagere waarde (bijvoorbeeld 5). <br>
  `{"DefaultBackupTasksThreshold": 5}`

3. Sla uw wijzigingen op en sluit het bestand.
4. Open **Taakbeheer** op het SQL Server-exemplaar. Start de service **AzureWLBackupCoordinatorSvc** opnieuw.

> [!NOTE]
> In de UX kunt u nog eens en zo veel back-ups plannen op een bepaald moment, maar ze worden verwerkt in een sliding window van bijvoorbeeld 5, aan de hand van het bovenstaande voorbeeld.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Kan ik een volledige back-up van een secundaire replica uitvoeren?
Aan de hand van beperkingen voor SQL, kunt u exemplaar alleen volledige back-up uitvoeren op de secundaire Replica. maar volledige back-up is niet toegestaan.

## <a name="can-i-protect-availability-groups-on-premises"></a>Kan ik beschikbaarheid groepen on-premises beveiligen?
Nee. Azure Backup beschermt SQL Server-databases die worden uitgevoerd in Azure. Als u een beschikbaarheidsgroep (AG) wordt verdeeld tussen Azure en on-premises machines, kan de AG worden beveiligd, alleen als de primaire replica wordt uitgevoerd in Azure. Azure Backup beschermt ook alleen de knooppunten die worden uitgevoerd in dezelfde Azure-regio als de Recovery Services-kluis.

## <a name="can-i-protect-availability-groups-across-regions"></a>Kan ik beschikbaarheidsgroepen in regio's beveiligen?
De Azure Backup Recovery Services-kluis kunt detecteren en alle knooppunten die zich in dezelfde regio bevinden als de kluis beveiligen. Als uw SQL Server Always On-beschikbaarheidsgroep meerdere Azure-regio's omvat, stelt u de back-up van de regio waaraan het primaire knooppunt. Azure Backup kunt detecteren en beveiligen van alle databases in de beschikbaarheidsgroep op basis van uw back-upvoorkeur. Als uw back-upvoorkeur niet wordt voldaan, back-ups mislukken en krijgt u de waarschuwing voor fout.

## <a name="do-successful-backup-jobs-create-alerts"></a>Maken succesvolle back-uptaken waarschuwingen?
Nee. Succesvolle back-uptaken maken geen waarschuwingen. Er worden alleen waarschuwingen verzonden voor mislukte back-uptaken. Gedetailleerde gedrag voor portal waarschuwingen wordt gedocumenteerd [hier](backup-azure-monitoring-built-in-monitor.md). Als u geïnteresseerd bent hoeven echter waarschuwingen zelfs voor geslaagde taken, kunt u [bewaken met behulp van Azure Monitor](backup-azure-monitoring-use-azuremonitor.md).

## <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Kan ik de geplande back-uptaken in het menu back-uptaken zien?
De **back-uptaak** menu wordt alleen weergegeven voor ad-hoc back-uptaken. Gebruik voor de geplande taak [bewaken met behulp van Azure Monitor](backup-azure-monitoring-use-azuremonitor.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>Toekomstige databases automatisch toegevoegd voor back-up?
Ja, kunt u deze mogelijkheid met bereiken [automatische beveiliging](backup-sql-server-database-azure-vms.md#enable-auto-protection).  

## <a name="if-i-delete-a-database-from-an-autoprotected-instance-what-will-happen-to-the-backups"></a>Als ik een database uit een exemplaar van autoprotected verwijdert, wat gebeurt er met de back-ups?
Als een database wordt verwijderd uit een exemplaar van autoprotected, worden er nog steeds de databaseback-ups geprobeerd. Dit betekent dat de verwijderde database moeten worden weergegeven als niet in orde onder begint **back-Upitems** en nog steeds is beveiligd.

De juiste manier om te stoppen met het beveiligen van de database van dit te doen is **back-up stoppen** met **gegevens verwijderen** voor deze database.  

## <a name="if-i-do-stop-backup-operation-of-an-autoprotected-database-what-will-be-its-behavior"></a>Wat is het gedrag als ik back-up van een database autoprotected stop?
Als u dit doet **back-up stoppen met gegevensbehoud**, geen toekomstige back-ups van plaatsvinden en de bestaande herstelpunten blijven intact. De database wordt nog steeds worden beschouwd als beveiligd en worden weergegeven onder de **back-up items**.

Als u dit doet **back-up stoppen en verwijder gegevens**, geen toekomstige back-ups van plaatsvinden en de bestaande herstelpunten worden ook verwijderd. De database wordt beschouwd als niet-beveiligde en worden weergegeven onder het exemplaar in de back-up configureren. Echter, in tegenstelling tot andere van beveiligde databases die handmatig kan worden geselecteerd of die autoprotected kunt ophalen, deze database wordt grijs weergegeven en kan niet worden geselecteerd. De enige manier om deze database opnieuw beveiligen is om uit te schakelen van automatische beveiliging op het exemplaar. U kunt nu deze database selecteren en configureren van beveiliging op deze of opnieuw opnieuw inschakelen automatische beveiliging op het exemplaar.

## <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-be-the-behavior"></a>Als ik de naam van de database wijzigen nadat deze is beveiligd, wat is het gedrag?
Een database opnieuw met de naam wordt behandeld als een nieuwe database. De service wordt daarom kan deze situatie behandelen alsof de database zijn niet gevonden en de back-ups met mislukt.

U kunt de database, die nu wordt gewijzigd en configureer beveiliging op deze selecteren. In het geval de automatische beveiliging is ingeschakeld op het exemplaar, zal de nieuwe naam database automatisch worden gedetecteerd en beveiligd.

##  <a name="why-cant-i-see-an-added-database-for-an-autoprotected-instance"></a>Waarom zie ik een database toegevoegd voor een exemplaar autoprotected niet?
Een database die u [toevoegen aan een exemplaar autoprotected](backup-sql-server-database-azure-vms.md#enable-auto-protection) mogelijk niet direct weergegeven onder beveiligde items. Dat komt doordat de detectie doorgaans om de 8 uur wordt uitgevoerd. U kunt echter detecteren en direct nieuwe databases beveiligen als u handmatig een detectie door te selecteren uitvoeren **databases herstellen**, zoals wordt weergegeven in de volgende afbeelding.

  ![Handmatig een nieuw toegevoegde database detecteren](./media/backup-azure-sql-database/view-newly-added-database.png)


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [maakt u een back-up van een SQL Server-database](backup-azure-sql-database.md) die wordt uitgevoerd op een Azure-VM.
