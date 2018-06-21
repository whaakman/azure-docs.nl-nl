---
title: Azure Backup probleemoplossingsgids voor SQL Server-VM's | Microsoft Docs
description: Informatie over probleemoplossing voor back-ups van VM's van SQL Server naar Azure.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 1c87382c2aae70b022fb391f80f7c75b0a4e5fe6
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296209"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Maak een Back-up van SQL Server op Azure oplossen

Dit artikel bevat informatie over probleemoplossing voor het beveiligen van SQL Server-VM's in Azure (Preview).

## <a name="public-preview-limitations"></a>Openbare Preview-beperkingen

Als u wilt weergeven van de openbare Preview-beperkingen, Zie het artikel [maakt u een Back-up van SQL Server-database in Azure](backup-azure-sql-database.md#public-preview-limitations).

## <a name="sql-server-permissions"></a>SQL Server-machtigingen

Beveiliging voor SQL Server-database configureren op een virtuele machine, de **AzureBackupWindowsWorkload** extensie moet worden geïnstalleerd op deze virtuele machine. Als u de foutmelding **UserErrorSQLNoSysadminMembership**, betekent dit uw SQL-exemplaar is niet gemachtigd de vereiste back-up. U kunt deze fout oplossen door u de stappen in [machtigingen instellen voor niet-marketplace SQL VMs](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

## <a name="troubleshooting-errors"></a>Het oplossen van problemen

Gebruik de informatie in de volgende tabellen om het oplossen van problemen en fouten zijn opgetreden tijdens het beveiligen van SQL Server naar Azure.

## <a name="backup-failures"></a>Mislukte back-ups

De volgende tabellen worden geordend op foutcode.

### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Deze SQL-database biedt geen ondersteuning voor het aangevraagde type back-up. | Treedt op wanneer het herstelmodel van de database niet in het aangevraagde type back-up is toegestaan. De fout kan optreden in de volgende situaties: <br/><ul><li>Een database met behulp van een eenvoudig herstelmodel kan geen back-up.</li><li>Differentiële en logboekback-ups zijn niet toegestaan voor een model database.</li></ul>Zie voor meer details over de [SQL herstelmodellen](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) documentatie. | Als de logboekback-up voor de database in het eenvoudige herstelmodel is mislukt, probeert u een van deze opties:<ul><li>Als de database zich in de modus voor eenvoudig herstel, schakelt u logboekback-ups.</li><li>Gebruik de [documentatie van SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) databaseherstelmodel volledige of bulksgewijs geregistreerde wijzigen. </li><li> Als u niet wilt wijzigen van het herstelmodel en u een standaard beleid hebt voor het back-up van meerdere databases die niet kunnen worden gewijzigd, moet u de fout negeren. Uw volledige en differentiële back-ups werken per schema. De logboekback-ups worden overgeslagen, die in dit geval wordt verwacht.</li></ul>Als het een Master-database en u hebt geconfigureerd differentiële of logboek back-up, gebruik een van de volgende stappen uit:<ul><li>Gebruik de portal voor het wijzigen van de planning van de back-upbeleid voor de Master-database, op Full.</li><li>Als u een standaard beleid hebt voor het back-up van meerdere databases die niet kunnen worden gewijzigd, moet u de fout negeren. Uw volledige back-up werkt per schema. Back-ups van de differentiële of logboekbestand zal niet worden uitgevoerd, die in dit geval wordt verwacht.</li></ul> |
| Bewerking geannuleerd omdat een conflicterende bewerking is al wordt uitgevoerd op dezelfde database. | Zie de [blogbericht over back-up en herstellen van beperkingen](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) die gelijktijdig worden uitgevoerd.| [SQL Server Management Studio (SSMS) gebruiken om te controleren van de back-uptaken.](backup-azure-sql-database.md#manage-azure-backup-operations-for-sql-on-azure-vms) Zodra de conflicterende bewerking mislukt, start u de bewerking opnieuw.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| SQL-database bestaat niet. | De database is verwijderd of hernoemd. | <ul><li>Controleer of de database per ongeluk zijn verwijderd of hernoemd.</li><li>Als de database per ongeluk is verwijderd, de database om door te gaan van de back-ups herstellen naar de oorspronkelijke locatie.</li><li>Als u de database verwijderd en niet moet toekomstige back-ups, en vervolgens in de Recovery Services-kluis op [stop back-up met ' Delete/behouden van de gegevens'](backup-azure-sql-database.md#manage-azure-backup-operations-for-sql-on-azure-vms).</li>|

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Logboekketen is onderbroken. | De database of de virtuele machine is een back-up met behulp van een andere back-upoplossing die de logboekketen worden afgekapt.|<ul><li>Controleer of een andere back-upoplossing of script wordt gebruikt. Zo ja, stopt u de back-upoplossing. </li><li>Als de back-up een back-up van de ad-hoc is, activeert u een volledige back-up voor het starten van een nieuwe logboekketen. Voor geplande logboekback-ups, is geen actie nodig, zoals Azure Backup-service wordt automatisch geactiveerd voor een volledige back-up als dit probleem wilt oplossen.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Azure Backup kan geen verbinding maken met de SQL-exemplaar. | Azure Backup kan geen verbinding maken met de SQL-exemplaar. | Gebruik van de aanvullende details in het menu van Azure portal fout de hoofdoorzaken beperken. Raadpleeg [SQL-back-probleemoplossing](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) om de fout te corrigeren.<br/><ul><li>Als u de standaardinstellingen van SQL geen externe verbindingen toestaan, moet u de instellingen wijzigen. Raadpleeg de onderstaande koppelingen voor het wijzigen van de instellingen.<ul><li>[https://msdn.microsoft.com/library/bb326495.aspx](https://msdn.microsoft.com/library/bb326495.aspx)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Als er problemen met aanmelden zijn, raadpleegt u de onderstaande koppelingen om dit te herstellen:<ul><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Er ontbreekt een eerste volledige back-up voor deze gegevensbron. | Volledige back-up ontbreekt voor de database. Logboek en differentiële back-ups bovenliggend item naar een volledige back-up, zodat volledige back-ups moeten worden genomen voordat differentiële of logboekback-ups. | Activeert een ad-hoc volledige back-up.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Kan niet worden back-up, zoals het transactielogboek voor de gegevensbron is vol. | De databaseruimte transactionele logboek is vol. | U kunt dit probleem oplossen door verwijzen naar de [documentatie van SQL](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |
| Deze SQL-database biedt geen ondersteuning voor het aangevraagde type back-up. | Altijd op AG secundaire replica's bieden geen ondersteuning voor volledige en differentiële back-ups. | <ul><li>Als u een ad-hoc back-up hebt geactiveerd, activeert u de back-ups op het primaire knooppunt.</li><li>Als de back-up door het beleid is gepland, zorg er dan voor dat het primaire knooppunt dat is geregistreerd. Registreren van het knooppunt [Volg de stappen voor het detecteren van een SQL Server-database](backup-azure-sql-database.md#discover-sql-server-databases).</li></ul> | 

## <a name="restore-failures"></a>Herstellen van fouten

De volgende foutcodes worden weergegeven wanneer het herstel taken mislukken.

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite 

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Database met dezelfde naam bestaat al op de doellocatie | De doellocatie voor herstel heeft al een database met dezelfde naam.  | <ul><li>De naam van de doel-database wijzigen</li><li>Of gebruik de optie force overschrijven op de pagina terugzetten</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Terugzetten is mislukt omdat de database kan niet offline worden gezet. | Tijdens het uitvoeren van een herstelpunt, moet de doeldatabase offline worden gezet. Azure Backup is niet tot deze gegevens offline brengen. | Gebruik van de aanvullende details in het menu van Azure portal fout de hoofdoorzaken beperken. Zie voor meer informatie de [documentatie van SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |


###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Het certificaat van de server met de vingerafdruk voor het doel is niet gevonden. | De Master-database op de doel-exemplaar heeft geen een geldige vingerafdruk. | De vingerafdruk van het geldig certificaat gebruikt op het bronexemplaar in de doelinstantie importeren. |

## <a name="registration-failures"></a>Registratiefouten

De volgende foutcodes zijn voor registratiefouten.

### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError 

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Back-up voorkeur voor de SQL AlwaysOn-beschikbaarheidsgroep kan niet worden voldaan omdat sommige knooppunten van de beschikbaarheidsgroep niet zijn geregistreerd. | Knooppunten nodig voor het uitvoeren van back-ups zijn niet geregistreerd of niet bereikbaar zijn. | <ul><li>Zorg ervoor dat alle knooppunten nodig voor het uitvoeren van back-ups van deze database geregistreerd en in orde zijn en probeer het opnieuw.</li><li>Back-up voorkeur wijziging SQL AlwaysOn-beschikbaarheidsgroep.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| SQL server VM wordt ofwel afgesloten en niet toegankelijk zijn voor Azure Backup-service. | Virtuele machine is afgesloten. | Zorg ervoor dat de SQL server wordt uitgevoerd. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Foutbericht | Mogelijke oorzaken | Aanbevolen actie |
|---|---|---|
| Azure Backup-service gebruikt Azure VM-gastagent voor het uitvoeren van back-up maar guest-agent is niet beschikbaar op de doelserver. | Guest-agent is niet ingeschakeld of niet in orde is | [Installeer de VM-gastagent](../virtual-machines/extensions/agent-windows.md) handmatig. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Backup voor SQL Server-VM's (openbare preview) [Azure Backup voor virtuele SQL-machines (openbare Preview)](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).