---
title: Het oplossen van SQL Server-database te maken met Azure Backup | Microsoft Docs
description: Informatie over probleemoplossing voor back-ups van SQL Server-databases die worden uitgevoerd op virtuele Azure-machines met Azure Backup.
services: backup
author: anuragm
manager: shivamg
ms.service: backup
ms.topic: article
ms.date: 03/13/2019
ms.author: anuragm
ms.openlocfilehash: b8fb6e2b23c275d198ac58fec874ad6627a7b43e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58007180"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Maak een Back-up van SQL Server op Azure oplossen

Dit artikel bevat informatie over probleemoplossing voor het beveiligen van SQL Server-VM's op Azure (Preview).

## <a name="feature-consideration-and-limitations"></a>Functie overwegingen en beperkingen

U wilt weergeven van de functie overweging, Zie het artikel [over SQL Server-back-up in Azure VM's](backup-sql-server-azure-vms.md#feature-consideration-and-limitations).

## <a name="sql-server-permissions"></a>SQL Server-machtigingen

Beveiliging voor een SQL Server-database configureren op een virtuele machine, de **AzureBackupWindowsWorkload** extensie moet worden geïnstalleerd op deze virtuele machine. Als u de foutmelding **UserErrorSQLNoSysadminMembership**, betekent dit dat uw SQL-exemplaar beschikt niet over de vereiste back-machtigingen. U kunt deze fout oplossen, volg de stappen in [machtigingen instellen voor niet-marketplace SQL-VM's](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

## <a name="troubleshooting-errors"></a>Het oplossen van problemen

Gebruik de informatie in de volgende tabellen om het oplossen van problemen en fouten opgetreden bij het beveiligen van SQL Server naar Azure.

## <a name="alerts"></a>Waarschuwingen

### <a name="backup-type-unsupported"></a>Type back-up wordt niet ondersteund

| Severity | Description | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|---|
| Waarschuwing | Huidige instellingen voor deze database bieden geen ondersteuning voor bepaalde soorten back-uptypen aanwezig zijn in het bijbehorende beleid. | <li>**DB-master**: Alleen een volledige database back-upbewerking kan worden uitgevoerd op de database master. geen van beide **differentiële** back-ups of transactie **logboeken** back-up is mogelijk. </li> <li>Elke database in **eenvoudige herstelmodel** is niet toegestaan voor de transactie **logboeken** back-up moeten worden uitgevoerd.</li> | De database-instellingen wijzigen zodat de back-typen in het beleid worden ondersteund. U kunt ook het huidige beleid zodanig dat alleen de ondersteunde back-uptypen wijzigen. Anders wordt de niet-ondersteunde back-uptypen overgeslagen tijdens de geplande back-up of de back-uptaak mislukt voor ad-hoc back-up.


## <a name="backup-failures"></a>Mislukte back-ups

De volgende tabellen worden geordend op foutcode.

### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Deze SQL-database biedt geen ondersteuning voor de gevraagde type back-up. | Treedt op wanneer het herstelmodel van de database niet toegestaan om de gevraagde type back-up. De fout kan optreden in de volgende situaties: <br/><ul><li>Een database met behulp van een eenvoudig herstelmodel toelaat logboekback-up niet.</li><li>Differentiële en logboekback-ups zijn niet toegestaan voor een model database.</li></ul>Zie voor meer informatie de [SQL herstelmodellen](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) documentatie. | Als de logboekback-up voor de database in het eenvoudige herstelmodel is mislukt, probeert u een van deze opties:<ul><li>Als de database zich in de modus voor eenvoudig herstel, schakelt u logboekback-ups.</li><li>Gebruik de [documentatie bij SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) te wijzigen van het herstelmodel van de database naar een volledige of bulksgewijs geregistreerd. </li><li> Als u niet wilt dat om het herstelmodel te wijzigen, en u een standaardbeleid hebt voor back-up van meerdere databases die niet kunnen worden gewijzigd, kunt u de fout negeren. Uw volledige en differentiële back-ups werkt per schema. De logboekback-ups worden overgeslagen, die in dit geval wordt verwacht.</li></ul>Als het een Master-database en u hebt geconfigureerd differentiële of logboek back-up, gebruik een van de volgende stappen uit:<ul><li>Gebruik de portal voor het wijzigen van de planning van de back-upbeleid voor de Master-database, op volledig.</li><li>Als u een standaardbeleid hebt voor back-up van meerdere databases die niet kunnen worden gewijzigd, moet u de fout negeren. Uw volledige back-up werkt per schema. Differentiële of logboek back-ups niet uitgevoerd, die in dit geval wordt verwacht.</li></ul> |
| Bewerking is geannuleerd omdat een conflicterende bewerking al werd uitgevoerd op dezelfde database. | Zie de [blogbericht over back-up en beperkingen bij het herstellen](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) die gelijktijdig worden uitgevoerd.| [SQL Server Management Studio (SSMS) gebruiken voor het bewaken van de back-uptaken.](manage-monitor-sql-database-backup.md) Zodra de conflicterende bewerking mislukt, start u de bewerking opnieuw.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| SQL-database bestaat niet. | De database is verwijderd of gewijzigd. | Controleer als de database per ongeluk zijn verwijderd of hernoemd.<br/><br/> Als de database per ongeluk is verwijderd, de database om door te gaan van de back-ups, herstellen naar oorspronkelijke locatie.<br/><br/> Als de database hebt verwijderd en niet moeten toekomstige back-ups, klikt u vervolgens in de Recovery Services-kluis op [back-up stoppen met 'Gegevens verwijderen/behouden'](manage-monitor-sql-database-backup.md).

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Logboekketen is onderbroken. | De database of de virtuele machine is back-ups met behulp van een andere back-upoplossing, die de logboekback-upketen worden afgekapt.|<ul><li>Controleer of een andere back-upoplossing of het script wordt gebruikt. Als dit het geval is, stopt u de back-upoplossing. </li><li>Als de back-up een ad-hoc logboekback-up is, activeert u een volledige back-up voor het starten van een nieuwe logboekback-upketen. Voor geplande logboekback-ups, is geen actie vereist omdat Azure Backup-service wordt automatisch geactiveerd voor een volledige back-up om dit probleem te verhelpen.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Azure Backup kan geen verbinding maken met de SQL-exemplaar. | Azure Backup kan geen verbinding maken met de SQL-exemplaar. | Aan de hand van de aanvullende gegevens in het menu van Azure portal fout om de belangrijkste oorzaken vast te stellen. Raadpleeg [SQL back-probleemoplossing](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) om de fout te herstellen.<br/><ul><li>Als de standaardinstellingen van de SQL geen externe verbindingen toestaan, moet u de instellingen wijzigen. Raadpleeg de onderstaande koppelingen voor het wijzigen van de instellingen.<ul><li>[https://msdn.microsoft.com/library/bb326495.aspx](https://msdn.microsoft.com/library/bb326495.aspx)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Als er problemen met aanmelding zijn, raadpleegt u de onderstaande koppelingen om op te lossen:<ul><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Eerste volledige back-up ontbreekt voor deze gegevensbron. | Volledige back-up ontbreekt voor de database. Logboek- en differentiële back-ups ouder voor een volledige back-up, zodat het volledige back-ups moeten worden genomen voordat u activeert differentiële of logboekback-ups. | Een ad-hoc volledige back-up activeren.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Kan geen back-up maken omdat het transactielogboek voor de gegevensbron vol is. | De ruimte in database transactionele logboek is vol. | U kunt dit probleem oplossen, raadpleegt u de [documentatie bij SQL](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |
| Deze SQL-database biedt geen ondersteuning voor de gevraagde type back-up. | Secundaire replica's altijd op AG bieden geen ondersteuning voor volledige en differentiële back-ups. | <ul><li>Als u een ad-hoc back-up hebt geactiveerd, activeert u de back-ups op het primaire knooppunt.</li><li>Als de back-up is gemaakt door het beleid, zorg er dan voor dat het primaire knooppunt is geregistreerd. Voor het registreren van het knooppunt [Volg de stappen voor het detecteren van een SQL Server-database](backup-azure-sql-database.md#discover-sql-server-databases).</li></ul> |

## <a name="restore-failures"></a>Herstellen van fouten

De volgende foutcodes worden weergegeven bij het herstellen taken mislukken.

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Database met dezelfde naam bestaat al op de doellocatie | De doellocatie voor herstel heeft al een database met dezelfde naam.  | <ul><li>Wijzig de naam van de doeldatabase</li><li>Of gebruik de optie force overschrijven in de gelijknamige pagina</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Herstellen is mislukt omdat de database niet offline kan worden gebracht. | Bij het uitvoeren van een herstelpunt, moet de doeldatabase moet offline worden gezet. Azure Backup kan niet deze gegevens offline te brengen. | Aan de hand van de aanvullende gegevens in het menu van Azure portal fout om de belangrijkste oorzaken vast te stellen. Zie voor meer informatie de [documentatie bij SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |


###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Kan het certificaat van de server met de vingerafdruk niet vinden op de doelcomputer. | De Master-database op de doel-exemplaar beschikt niet over de vingerafdruk van een geldige versleuteling. | De vingerafdruk van het geldig certificaat gebruikt op het bronexemplaar, in de doelinstantie importeren. |

## <a name="registration-failures"></a>Registratiefouten

De volgende foutcodes zijn voor registratiefouten.

### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Back-upvoorkeur voor de SQL AlwaysOn-beschikbaarheidsgroep kan niet worden voldaan omdat bepaalde knooppunten van de beschikbaarheidsgroep niet zijn geregistreerd. | Knooppunten die nodig zijn om uit te voeren van de back-ups worden niet geregistreerd of niet bereikbaar zijn. | <ul><li>Zorg ervoor dat alle knooppunten die nodig zijn om uit te voeren van de back-ups van deze database geregistreerd en die in orde is zijn en probeer het vervolgens opnieuw.</li><li>Back-upvoorkeur wijziging SQL AlwaysOn-beschikbaarheidsgroep.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| SQL server-machine is afgesloten of niet toegankelijk voor Azure Backup-service. | Virtuele machine is afgesloten. | Zorg ervoor dat de SQL-server wordt uitgevoerd. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Azure Backup-service maakt gebruik van Azure VM-gastagent voor het uitvoeren van back-up, maar guest-agent is niet beschikbaar op de doelserver. | Gastagent is niet ingeschakeld of niet in orde is | [Installeren van de VM-gastagent](../virtual-machines/extensions/agent-windows.md) handmatig. |

## <a name="configure-backup-failures"></a>Back-up fouten configureren

De volgende fout codes zijn voor configureren back-up maken van de fouten.

### <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Automatische beveiliging kunt u lezen wat is verwijderd of is niet meer geldig. | Wanneer u automatische beveiliging op een SQL-exemplaar inschakelt **back-up configureren** taken worden uitgevoerd voor alle databases in dat exemplaar. Als u automatische beveiliging uitschakelt terwijl de taken worden uitgevoerd, dan zal de **In uitvoering** taken worden geannuleerd met deze foutcode. | Schakel automatische beveiliging opnieuw naar de resterende databases beveiligen. |

## <a name="re-registration-failures"></a>Vernieuwde registratie fouten

Controleren op een of meer van de [symptomen](#symptoms) voordat u activeert de bewerking opnieuw te registreren.

### <a name="symptoms"></a>Symptomen

* Alle bewerkingen zoals back-ups, herstel, en de configuratie van back-ups mislukken op de virtuele machine met een van de volgende foutcodes: **WorkloadExtensionNotReachable**, **UserErrorWorkloadExtensionNotInstalled**, **WorkloadExtensionNotPresent**, **WorkloadExtensionDidntDequeueMsg**
* De **back-up Status** voor de back-up item wordt weergegeven **niet bereikbaar**. Hoewel u moet sprake is van alle andere redenen die ook tot de dezelfde status leiden kunnen:

  * Gebrek aan machtigingen voor het uitvoeren van back-gerelateerde bewerkingen op de virtuele machine  
  * Virtuele machine is afgesloten vanwege die back-ups kunnen niet uitgevoerd worden
  * Netwerkproblemen  

    ![Virtuele machine opnieuw te registreren](./media/backup-azure-sql-database/re-register-vm.png)

* In het geval van AlwaysOn-beschikbaarheidsgroep, de back-ups gestart nadat u de back-upvoorkeur gewijzigd of wanneer er een failover is mislukt

### <a name="causes"></a>Oorzaken
Deze problemen kunnen worden veroorzaakt door een of meer van de volgende redenen:

  * Extensie is verwijderd of is verwijderd van de portal 
  * Extensie is verwijderd uit de **Configuratiescherm** van de virtuele machine onder **verwijderen of wijzigen van een programma** UI
  * Virtuele machine is hersteld terug in tijd met in-place schijven herstellen
  * Virtuele machine is afgesloten gedurende een langere periode vanwege waarvan de configuratie van de extensie erop verlopen
  * Virtuele machine is verwijderd en een andere virtuele machine is gemaakt met dezelfde naam en in dezelfde resourcegroep bevinden als de verwijderde virtuele machine
  * Een van de knooppunten AG de volledige back-upconfiguratie is ontvangen, dit kan gebeuren op het moment van registratie van de beschikbaarheid van groep naar de kluis of wanneer een nieuw knooppunt wordt toegevoegd  <br>
    In de bovenstaande scenario's, is het aanbevolen voor het activeren van de bewerking op de virtuele machine opnieuw te registreren. Deze optie is alleen beschikbaar via PowerShell en is binnenkort beschikbaar in Azure portal.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Backup voor SQL Server-VM's (preview-versie) [Azure back-up voor SQL-VM's (Preview-versie)](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
