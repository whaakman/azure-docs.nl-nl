---
title: Veelgestelde vragen over back-ups van SQL Server-databases op Azure Virtual machines met Azure Backup
description: Vind antwoorden op veelgestelde vragen over back-ups van SQL Server-databases op Azure Virtual machines met Azure Backup.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: sogup
ms.openlocfilehash: 3b7649a029c6c44cd8a25ea553ff2091f816dd3c
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57214823"
---
# <a name="faq-about-sql-server-databases-that-are-running-on-an-azure-vm-backup"></a>Veelgestelde vragen over SQL Server-databases die worden uitgevoerd op een Azure-VM back-up

In dit artikel vindt u antwoorden op veelgestelde vragen over back-ups van SQL Server-databases die uitvoeren op Azure virtual machines (VM's) en die gebruikmaken van de [Azure Backup](backup-overview.md) service.

> [!NOTE]
> Deze functie is momenteel beschikbaar als openbare preview-versie.

## <a name="can-i-throttle-the-backup-speed"></a>Kan ik de snelheid van de back-up beperken?

Ja. De snelheid waarmee het back-upbeleid voor het minimaliseren van de impact op een exemplaar van SQL Server wordt uitgevoerd, kunt u beperken. Ga als volgt te werk om de instelling te wijzigen:
1. Op de SQL Server-exemplaar in de *C:\Program Files\Azure werkbelasting Backup\bin* map, maken de *ExtensionSettingsOverrides.json* bestand.
2. In de *ExtensionSettingsOverrides.json* bestand, wijzig de **DefaultBackupTasksThreshold** instellen op een lagere waarde (bijvoorbeeld 5). <br>
  ` {"DefaultBackupTasksThreshold": 5}`

3. Sla uw wijzigingen op en sluit het bestand.
4. Open **Taakbeheer** op het SQL Server-exemplaar. Start de service **AzureWLBackupCoordinatorSvc** opnieuw.

## <a name="can-i-run-a-full-backup-from-a-secondary-replica"></a>Kan ik een volledige back-up van een secundaire replica uitvoeren?
Nee. Deze functie wordt niet ondersteund.

## <a name="do-successful-backup-jobs-create-alerts"></a>Maken succesvolle back-uptaken waarschuwingen?

Nee. Succesvolle back-uptaken maken geen waarschuwingen. Er worden alleen waarschuwingen verzonden voor mislukte back-uptaken.

## <a name="can-i-see-scheduled-backup-jobs-in-the-jobs-menu"></a>Kan ik de geplande back-uptaken in het menu taken zien?

Nee. De **back-uptaken** menu geeft taakdetails op aanvraag, maar geen geplande back-uptaken. Als een geplande back-uptaken mislukken, vindt u meer informatie in de waarschuwingen van de mislukte taak. U kunt alle geplande en ongeplande back-uptaken controleren met [SQL Server Management Studio](manage-monitor-sql-database-backup.md).

## <a name="are-future-databases-automatically-added-for-backup"></a>Toekomstige databases automatisch toegevoegd voor back-up?

Nee. Bij het instellen van beveiliging voor een SQL Server-exemplaar, worden alle databases worden toegevoegd wanneer u de optie voor de server selecteert. Na het instellen van beveiliging, moet u handmatig nieuwe databases om ze te beveiligen toevoegen. Nieuwe databases worden niet automatisch beveiligd.

##  <a name="how-do-i-restart-protection-after-i-change-recovery-type"></a>Hoe ik beveiliging starten nadat ik hersteltype wijzigen?

Activeer een volledige back-up. Logboekback-ups worden gestart zoals verwacht.

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
