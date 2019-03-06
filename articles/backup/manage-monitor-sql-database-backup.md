---
title: Beheren en controleren van SQL Server-databases op een Azure-VM die wordt ondersteund door Azure Backup | Microsoft Docs
description: In dit artikel wordt beschreven hoe u SQL Server-databases die worden uitgevoerd op een Azure VM en die worden ondersteund door Azure Backup herstellen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: da4264047830b21b3ac4dae723dd1fd2f9d7a8f4
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57432852"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Beheren en bewaken back-ups van SQL Server-databases 


In dit artikel worden algemene taken beschreven voor het beheren en controleren van SQL Server-databases die worden uitgevoerd op een Azure-machine (VM) en die zijn back-ups op een Azure Backup Recovery Services-kluis door het [Azure Backup](backup-overview.md) service. U leert hoe u taken en waarschuwingen bewaken, stoppen en hervatten van databasebeveiliging, back-uptaken uitvoeren en een virtuele machine uit de back-ups van de registratie ongedaan maken.


> [!NOTE]
> De back-up van SQL Server-databases die worden uitgevoerd op een Azure-VM met Azure Backup is momenteel in openbare preview.


Als u dit nog niet hebt nog back-ups is geconfigureerd voor uw SQL Server-databases, [maakt u een Back-up van SQL Server-databases op Azure Virtual machines](backup-azure-sql-database.md)

##  <a name="monitor-manual-backup-jobs-in-the-portal"></a>Handmatige back-uptaken in de portal bewaken

Azure Backup worden alle handmatig geactiveerde taken in de **back-uptaken** portal. De taken u weergeven in deze portal opnemen database detecteren en registreren en back-up en herstelbewerkingen.

![De portal voor back-up-taken](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> De **back-uptaken** portal geplande back-uptaken wordt niet weergegeven. Geplande back-uptaken kunt u bewaken met SQL Server Management Studio, zoals wordt beschreven in het volgende gedeelte.
>

## <a name="monitor-scheduled-backup-jobs-in-sql-server-management-studio"></a>Geplande back-uptaken in SQL Server Management Studio controleren 

Azure Backup maakt gebruik van systeemeigen SQL-API's voor alle back-upbewerkingen. Gebruik de systeemeigen API's om alle taakgegevens op te halen uit de [SQL-tabel met back-ups](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) in de msdb-database.

Het volgende voorbeeld wordt een query waarmee alle back-uptaken voor een database met de naam worden opgehaald **DB1**. Pas de query voor geavanceerde bewaking naar wens aan.

```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  

```

## <a name="view-backup-alerts"></a>Waarschuwingen voor back-ups weergeven

Omdat logboekback-ups worden uitgevoerd om de 15 minuten, kan back-uptaken bewaking worden omslachtig. Azure Backup vereenvoudigt de bewaking in door te verzenden van e-mailwaarschuwingen. E-mailwaarschuwingen zijn:

- Alle mislukte back-ups wordt geactiveerd.
- Geconsolideerd op het databaseniveau van de-foutcode.
- Alleen voor een database de eerste back-upfouten verzonden. 

Voor het bewaken van de back-upwaarschuwingen database:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer op het kluisdashboard **waarschuwingen en gebeurtenissen**.

   ![Waarschuwingen en gebeurtenissen selecteren](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

1. In **waarschuwingen en gebeurtenissen**, selecteer **waarschuwingen voor back-up**.

   ![Waarschuwingen voor back-ups selecteren](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>Beveiliging van een SQL Server-database stoppen

U kunt stoppen back-ups van een SQL Server-database in een aantal manieren:

* Alle toekomstige back-uptaken stoppen en verwijderen van alle herstelpunten.
* Alle toekomstige back-uptaken stoppen en de herstelpunten te behouden.

Als u ervoor kiest om te laten herstelpunten, houd rekening met deze details:

* U laat herstelpunten worden opgeschoond op basis van het back-upbeleid. 
* Totdat alle herstelpunten worden opgeschoond, in rekening gebracht voor het beveiligde exemplaar en de verbruikte opslag. Zie voor meer informatie, [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Azure Backup blijft altijd één laatste herstelpunt totdat u de back-upgegevens verwijdert. 
* Als u een gegevensbron verwijdert zonder back-ups wordt gestopt, worden nieuwe back-ups mislukken. 
* Als uw database is ingeschakeld voor autoprotection, kunt u back-ups kan niet stoppen, tenzij u autoprotection uitschakelen.

Ga als volgt te werk om de beveiliging van een database te stoppen:

1. Op het kluisdashboard onder **gebruik**, selecteer **back-Upitems**.

1. Onder **Type back-upbeheer**, selecteer **SQL in Azure VM**.

    ![SQL in Azure VM selecteren](./media/backup-azure-sql-database/sql-restore-backup-items.png)


1. Selecteer de database waarvoor u wenst te beveiliging stoppen.

    ![De database selecteren waarvoor u de beveiliging wilt stoppen](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)


1. Selecteer in het databasemenu **back-up stoppen**.

    ![Back-up stoppen selecteren](./media/backup-azure-sql-database/stop-db-button.png)


1. Op de **back-up stoppen** in het menu selecteren of u wilt behouden of verwijderen van gegevens. Als u wilt, Geef een reden en de opmerking.

    ![Behouden of verwijderen van gegevens in het menu back-up stoppen](./media/backup-azure-sql-database/stop-backup-button.png)

1. Selecteer **back-up stoppen**.

  

## <a name="resume-protection-for-a-sql-database"></a>Beveiliging voor een SQL-database hervatten

Wanneer u de beveiliging voor de SQL-database stoppen als u selecteert de **back-upgegevens behouden** optie, kunt u later beveiliging hervatten. Als u niet de back-upgegevens behouden, kunt u de beveiliging niet hervatten.

Beveiliging voor een SQL-database hervatten:

1. Open het back-upitem en selecteer **back-up hervatten**.

    ![Back-up hervatten selecteren om de databasebeveiliging te hervatten](./media/backup-azure-sql-database/resume-backup-button.png)

2. Selecteer een beleid in het menu **Back-upbeleid** en selecteer vervolgens **Opslaan**.

## <a name="run-an-on-demand-backup"></a>Een on-demand back-up uitvoeren

U kunt verschillende typen back-ups op aanvraag uitvoeren:

* Volledige back-up
* Alleen te kopiëren volledige back-up
* Differentiële back-up
* Logboekback-up

Zie voor meer informatie, [SQL Server-back-uptypen](backup-architecture.md#sql-server-backup-types).

## <a name="unregister-a-sql-server-instance"></a>Registratie van een SQL Server-exemplaar ongedaan maken

Registratie van een exemplaar van SQL Server nadat u de beveiliging uitschakelen, maar voordat u de kluis verwijderen:

1. Op het kluisdashboard onder **beheren**, selecteer **back-upinfrastructuur**.  

   ![Back-upinfrastructuur selecteren](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. Onder **Beheerservers** selecteert u **Beveiligde servers**.

   ![Beveiligde servers selecteren](./media/backup-azure-sql-database/protected-servers.png)


3. In **beschermde Servers**, de server om de registratie te selecteren. Als u de kluis wilt verwijderen, moet u de registratie van alle servers ongedaan maken.

4. Met de rechtermuisknop op de beveiligde server en selecteer **verwijderen**.

   ![Verwijderen selecteren](./media/backup-azure-sql-database/delete-protected-server.png)


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [back-ups op een SQL Server-database oplossen](backup-sql-server-azure-troubleshoot.md).
