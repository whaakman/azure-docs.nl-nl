---
title: Oplossen van back-up van SQL Server-database met behulp van Azure Backup | Microsoft Docs
description: Informatie over probleemoplossing voor back-ups van SQL Server-databases die worden uitgevoerd op virtuele Azure-machines met Azure Backup.
services: backup
author: anuragm
manager: sivan
ms.service: backup
ms.topic: article
ms.date: 06/18/2019
ms.author: anuragm
ms.openlocfilehash: b2822a3c7dfa23065f2cbd35ef4e506efae026f2
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704854"
---
# <a name="troubleshoot-sql-server-database-backup-by-using-azure-backup"></a>Back-up van SQL Server-database met behulp van Azure Backup-problemen

Dit artikel bevat informatie over probleemoplossing voor SQL Server-databases op Azure virtual machines worden uitgevoerd.

Zie voor meer informatie over de back-upproces en beperkingen, [over SQL Server-back-up in Azure VM's](backup-azure-sql-database.md#feature-consideration-and-limitations).

## <a name="sql-server-permissions"></a>SQL Server-machtigingen

Voor het configureren van beveiliging voor een SQL Server-database op een virtuele machine, moet u de **AzureBackupWindowsWorkload** -extensie op die virtuele machine. Als u de foutmelding krijgt **UserErrorSQLNoSysadminMembership**, betekent dit dat uw SQL Server-exemplaar beschikt niet over de vereiste back-machtigingen. U kunt deze fout oplossen, volg de stappen in [VM stelt machtigingen](backup-azure-sql-database.md#set-vm-permissions).

## <a name="error-messages"></a>Foutberichten

### <a name="backup-type-unsupported"></a>Type back-up wordt niet ondersteund

| Severity | Description | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|---|
| Waarschuwing | Huidige instellingen voor deze database ondersteuning geen voor bepaalde back-uptypen aanwezig zijn in het bijbehorende beleid. | <li>Alleen een volledige database back-upbewerking kan worden uitgevoerd op de database master. Differentiële back-up noch transactielogboekback-up is mogelijk. </li> <li>Elke database in het eenvoudige herstelmodel is niet toegestaan voor de back-ups van transactielogboeken.</li> | De database-instellingen wijzigen zodat de back-typen in het beleid worden ondersteund. Of het huidige beleid zodanig dat alleen de ondersteunde back-uptypen wijzigen. Anders wordt de niet-ondersteunde back-uptypen overgeslagen tijdens de geplande back-up of de back-uptaak mislukt voor ad-hoc back-up.


### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Deze SQL-database biedt geen ondersteuning voor de gevraagde type back-up. | Treedt op wanneer het herstelmodel van de database niet toegestaan om de gevraagde type back-up. De fout kan optreden in de volgende situaties: <br/><ul><li>Een database die van een eenvoudig herstelmodel gebruikmaakt toelaat logboekback-up niet.</li><li>Differentiële en logboekback-ups zijn niet toegestaan voor een hoofddatabase.</li></ul>Zie voor meer informatie de [herstelmodellen van SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) documentatie. | Als de logboekback-up voor de database in het eenvoudige herstelmodel mislukt, probeert u een van deze opties:<ul><li>Als de database zich in de modus voor eenvoudig herstel, schakelt u logboekback-ups.</li><li>Gebruik de [documentatie van SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) te wijzigen van het herstelmodel van de database naar een volledige of bulksgewijs geregistreerde. </li><li> Als u niet wilt dat om het herstelmodel te wijzigen, en u een standaardbeleid hebt voor back-up van meerdere databases die niet kunnen worden gewijzigd, kunt u de fout negeren. Uw volledige en differentiële back-ups werkt per schema. De logboekback-ups worden overgeslagen, die in dit geval wordt verwacht.</li></ul>Als een database master is en u hebt geconfigureerd differentiële of meld u back-up, gebruikt u een van de volgende stappen uit:<ul><li>Gebruik de portal om de planning van de back-upbeleid voor de database master te volledige wijzigen.</li><li>Als u een standaardbeleid hebt voor back-up van meerdere databases die niet kunnen worden gewijzigd, moet u de fout negeren. Uw volledige back-up werkt per schema. Differentiële of logboek back-ups niet uitgevoerd, die in dit geval wordt verwacht.</li></ul> |
| Bewerking is geannuleerd omdat een conflicterende bewerking al werd uitgevoerd op dezelfde database. | Zie de [blogbericht over back-up en herstel beperkingen](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) die gelijktijdig worden uitgevoerd.| [SQL Server Management Studio (SSMS) gebruiken voor het bewaken van de back-uptaken](manage-monitor-sql-database-backup.md). Nadat de conflicterende bewerking is mislukt, start u de bewerking opnieuw.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| SQL-database bestaat niet. | De database is verwijderd of gewijzigd. | Controleer als de database per ongeluk zijn verwijderd of hernoemd.<br/><br/> Als de database per ongeluk is verwijderd, de database om door te gaan van de back-ups, herstellen naar oorspronkelijke locatie.<br/><br/> Als de database hebt verwijderd en niet moeten toekomstige back-ups, klikt u vervolgens in de Recovery Services-kluis selecteert **back-up stoppen** met **back-upgegevens behouden** of **back-upgegevens verwijderen**. Zie voor meer informatie, [beheren en bewaken back-up wordt SQL Server-databases](manage-monitor-sql-database-backup.md).

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Logboekketen is onderbroken. | De database of de virtuele machine is back-ups via een andere back-upoplossing, die de logboekback-upketen worden afgekapt.|<ul><li>Controleer of een andere back-upoplossing of het script wordt gebruikt. Als dit het geval is, stopt u de back-upoplossing. </li><li>Als de back-up een ad-hoc logboekback-up is, activeert u een volledige back-up voor het starten van een nieuwe logboekback-upketen. Voor geplande logboekback-ups, hoeft u geen actie vereist omdat de Azure Backup-service wordt automatisch geactiveerd voor een volledige back-up om dit probleem te verhelpen.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Azure Backup kan geen verbinding maken met de SQL-exemplaar. | Azure Backup kan geen verbinding maken met de SQL Server-exemplaar. | Aan de hand van de aanvullende gegevens in het menu Fout bij de Azure portal om te beperken de oorzaken. Raadpleeg [SQL back-probleemoplossing](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) om de fout te herstellen.<br/><ul><li>Als de standaard SQL-instellingen niet toestaan van externe verbindingen, moet u de instellingen wijzigen. Zie de volgende artikelen voor meer informatie over het wijzigen van de instellingen:<ul><li>[MSSQLSERVER_-1](/previous-versions/sql/sql-server-2016/bb326495(v=sql.130))</li><li>[MSSQLSERVER_2](/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[MSSQLSERVER_53](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Als er problemen met aanmelding, gebruikt u deze koppelingen om op te lossen:<ul><li>[MSSQLSERVER_18456](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[MSSQLSERVER_18452](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Eerste volledige back-up ontbreekt voor deze gegevensbron. | Volledige back-up ontbreekt voor de database. Logboek- en differentiële back-ups zijn ouders aan om een volledige back-up, dus zorg ervoor dat u de volledige back-ups nemen voordat u activeert differentiële of logboekback-ups. | Een ad-hoc volledige back-up activeren.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Kan geen back-up maken omdat het transactielogboek voor de gegevensbron vol is. | De ruimte in database transactionele logboek is vol. | U kunt dit probleem oplossen, raadpleegt u de [documentatie van SQL Server](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Database met dezelfde naam bestaat al op de doellocatie | De doellocatie voor herstel heeft al een database met dezelfde naam.  | <ul><li>Wijzig de naam van de doeldatabase.</li><li>Of gebruik de optie voor het overschrijven van kracht op de gelijknamige pagina.</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Herstellen is mislukt, omdat de database niet offline kan worden gezet. | Terwijl u een herstelbewerking uitvoert, moet de doeldatabase offline worden gezet. Azure Backup kan niet deze gegevens offline brengen. | Aan de hand van de aanvullende gegevens in het menu Fout bij de Azure portal om te beperken de oorzaken. Zie voor meer informatie de [documentatie van SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |

###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Kan het certificaat van de server met de vingerafdruk niet vinden op de doelcomputer. | De database master op het doel-exemplaar beschikt niet over de vingerafdruk van een geldige versleuteling. | De vingerafdruk van het geldig certificaat gebruikt op het bronexemplaar, in de doelinstantie importeren. |

### <a name="usererrorrestorenotpossiblebecauselogbackupcontainsbulkloggedchanges"></a>UserErrorRestoreNotPossibleBecauseLogBackupContainsBulkLoggedChanges

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| De logboekback-up die is gebruikt voor herstel bevat bulksgewijs geregistreerde wijzigingen. Deze kan niet worden gebruikt om op een willekeurig tijdstip te stoppen volgens de SQL-richtlijnen. | Wanneer een database in de herstelmodus bulksgewijs geregistreerde is, kan de gegevens tussen een transactie bulksgewijs geregistreerde en de volgende logboekbestanden transactie kan niet worden hersteld. | Kies een ander punt in tijd voor herstel. [Meer informatie](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms186229(v=sql.105)).


### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Er kan niet worden voldaan aan de voorkeur van Backup voor de SQL AlwaysOn-beschikbaarheidsgroep omdat bepaalde knooppunten van de beschikbaarheidsgroep niet zijn geregistreerd. | Knooppunten die nodig zijn om uit te voeren van de back-ups worden niet geregistreerd of niet bereikbaar zijn. | <ul><li>Zorg ervoor dat alle knooppunten die nodig zijn om uit te voeren van de back-ups van deze database geregistreerd en die in orde is zijn en probeer het vervolgens opnieuw.</li><li>Wijzig de back-upvoorkeur voor de SQL Server Always On-beschikbaarheidsgroep.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| SQL server-machine is afgesloten of niet toegankelijk voor Azure Backup-service. | De virtuele machine wordt afgesloten. | Zorg ervoor dat de SQL Server-exemplaar wordt uitgevoerd. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Azure Backup-service maakt gebruik van Azure VM-gastagent voor het uitvoeren van back-up, maar guest-agent is niet beschikbaar op de doelserver. | De gastagent is niet ingeschakeld of niet in orde is. | [Installeren van de VM-gastagent](../virtual-machines/extensions/agent-windows.md) handmatig. |

### <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Automatische beveiliging kunt u lezen wat is verwijderd of is niet meer geldig. | Wanneer u automatische beveiliging van een exemplaar van SQL Server, inschakelt **back-up configureren** taken worden uitgevoerd voor alle databases in dat exemplaar. Als u automatische beveiliging uitschakelt terwijl de taken worden uitgevoerd, dan zal de **In uitvoering** taken worden geannuleerd met deze foutcode. | Schakel automatische beveiliging opnieuw om het te beveiligen van de resterende databases. |

## <a name="re-registration-failures"></a>Vernieuwde registratie fouten

Controleer voor een of meer van de volgende symptomen voordat u de bewerking opnieuw te registreren activeert:

* Alle bewerkingen (zoals back-ups, herstellen en back-up configureren) mislukken op de virtuele machine met een van de volgende foutcodes: **WorkloadExtensionNotReachable**, **UserErrorWorkloadExtensionNotInstalled**, **WorkloadExtensionNotPresent**, **WorkloadExtensionDidntDequeueMsg**.
* De **back-upstatus** gebied van de back-upitem wordt weergegeven **niet bereikbaar**. Sprake is van alle andere oorzaken die tot de dezelfde status leiden mogelijk:

  * Gebrek aan machtigingen voor het uitvoeren van bewerkingen met betrekking tot back-up op de virtuele machine  
  * Afsluiten van de virtuele machine, zodat back-ups kunnen niet uitgevoerd worden
  * Netwerkproblemen  

  ![De status 'Niet bereikbaar' in een virtuele machine opnieuw te registreren](./media/backup-azure-sql-database/re-register-vm.png)

* In het geval van een beschikbaarheidsgroep altijd op de back-ups aan de slag mislukt nadat u de back-upvoorkeur gewijzigd of na een failover.

Deze problemen kunnen optreden voor een of meer van de volgende redenen:

* Een extensie is verwijderd of is verwijderd uit de portal. 
* Een extensie is verwijderd van **Configuratiescherm** op de virtuele machine onder **verwijderen of wijzigen van een programma**.
* De virtuele machine is teruggezet terug in tijd via in-place schijven herstellen.
* De virtuele machine is afgesloten gedurende een langere periode, waardoor de configuratie van de extensie erop is verlopen.
* De virtuele machine is verwijderd en een andere virtuele machine is gemaakt met dezelfde naam en in dezelfde resourcegroep bevinden als de verwijderde virtuele machine.
* Een van de knooppunten van de groep beschikbaarheid van is de volledige back-upconfiguratie ontvangen. Dit kan gebeuren wanneer de beschikbaarheidsgroep is geregistreerd bij de kluis of wanneer een nieuw knooppunt wordt toegevoegd.

U wordt aangeraden dat u een bewerking opnieuw te registreren op de virtuele machine activeren in het voorgaande scenario's. Deze optie is op dit moment alleen beschikbaar via PowerShell.

## <a name="size-limit-for-files"></a>Maximale grootte voor bestanden

De totale grootte van bestanden is afhankelijk van het niet alleen op het aantal bestanden, maar ook op hun namen en paden. Voor elk databasebestand ontvang de logische bestandsnaam en het fysieke pad. U kunt deze SQL-query:

```
SELECT mf.name AS LogicalName, Physical_Name AS Location FROM sys.master_files mf
               INNER JOIN sys.databases db ON db.database_id = mf.database_id
               WHERE db.name = N'<Database Name>'"
```

Nu rangschik ze in de volgende indeling:

```
[{"path":"<Location>","logicalName":"<LogicalName>","isDir":false},{"path":"<Location>","logicalName":"<LogicalName>","isDir":false}]}
```

Hier volgt een voorbeeld:

```
[{"path":"F:\\Data\\TestDB12.mdf","logicalName":"TestDB12","isDir":false},{"path":"F:\\Log\\TestDB12_log.ldf","logicalName":"TestDB12_log","isDir":false}]}
```

Als de grootte van de tekenreeks van de inhoud groter is dan 20.000 bytes, worden de bestanden van de gegevens anders opgeslagen. Tijdens het herstel niet mogelijk om in te stellen van het pad naar het doelbestand voor de terugzetbewerking. De bestanden worden teruggezet naar het standaardpad SQL geleverd door de SQL Server.

### <a name="override-the-default-target-restore-file-path"></a>Het standaardpad van het doel-herstel onderdrukken

U kunt het doelpad herstellen in het bestand tijdens de herstelbewerking overschrijven door een JSON-bestand met de toewijzing van het databasebestand op het doelpad voor herstel. Maak een `database_name.json` -bestand en plaats deze in de locatie *C:\Program Files\Azure werkbelasting Backup\bin\plugins\SQL*.

De inhoud van het bestand moet deze indeling hebben:
```
[
  {
    "Path": "<Restore_Path>",
    "LogicalName": "<LogicalName>",
    "IsDir": "false"
  },
  {
    "Path": "<Restore_Path>",
    "LogicalName": "LogicalName",
    "IsDir": "false"
  },  
]
```

Hier volgt een voorbeeld:

```
[
  {
   "Path": "F:\\Data\\testdb2_1546408741449456.mdf",
   "LogicalName": "testdb7",
   "IsDir": "false"
  },
  {
    "Path": "F:\\Log\\testdb2_log_1546408741449456.ldf",
    "LogicalName": "testdb7_log",
    "IsDir": "false"
  },  
]
```

In de voorgaande inhoud, kunt u de logische naam van het databasebestand opvragen met behulp van de volgende SQL-query:

```
SELECT mf.name AS LogicalName FROM sys.master_files mf
                INNER JOIN sys.databases db ON db.database_id = mf.database_id
                WHERE db.name = N'<Database Name>'"
  ```


Dit bestand moet worden geplaatst voordat u de herstelbewerking opnieuw activeren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Backup voor SQL Server-VM's (preview-versie) [Azure back-up voor SQL-VM's](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
