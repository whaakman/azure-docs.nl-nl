---
title: Beheren en controleren van SQL Server-databases op een Azure-VM back-up gemaakt door Azure Backup | Microsoft Docs
description: In dit artikel wordt beschreven hoe u SQL Server-databases die worden uitgevoerd op een virtuele machine van Azure die worden ondersteund met Azure Backup herstellen
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 1c2ce0ba42f0bc3efd1dcc951113b05ab6941b98
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430799"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Beheren en bewaken back-ups van SQL Server-databases 


In dit artikel worden algemene taken beschreven voor het beheren en controleren van SQL Server-databases die worden uitgevoerd op een virtuele machine van Azure die worden back-ups op een Azure Backup Recovery Services-kluis door het [Azure Backup](backup-overview.md) service. Taken met inbegrip van taken en waarschuwingen bewaken, stoppen en hervatten van databasebeveiliging, back-uptaken uitgevoerd en registratie van een virtuele machine vanuit back-up.


> [!NOTE]
> Back-up van SQl Server-databases die worden uitgevoerd op een Azure-VM met Azure Backup is momenteel in openbare preview.


TIf u nog niet hebt, maar geconfigureerde back-up voor de SQL Server-databases en volg de instructies in [in dit artikel](backup-azure-sql-database.md)

## <a name="monitor-backup-jobs"></a>Back-uptaken controleren

###  <a name="monitor-ad-hoc-jobs-in-the-portal"></a>Ad-hoc taken in de portal controleren

Azure Backup worden alle handmatig geactiveerde taken in de **back-uptaken** portal, onder andere detecteren en registreren van databases en back-up en herstelbewerkingen.

![De portal Back-uptaken](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Geplande back-uptaken worden niet weergegeven in de **back-uptaken** portal. Geplande back-uptaken kunt u bewaken met SQL Server Management Studio, zoals wordt beschreven in het volgende gedeelte.
>

### <a name="monitor-backup-jobs-with-sql-server-management-studio"></a>Back-uptaken controleren met SQL Server Management Studio 

Azure Backup maakt gebruik van systeemeigen SQL-API's voor alle back-upbewerkingen.

Gebruik de systeemeigen API's om alle taakgegevens op te halen uit de [SQL-tabel met back-ups](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) in de msdb-database.

Het volgende voorbeeld is een query waarmee alle back-uptaken voor een database met de naam **DB1** worden opgehaald. Pas de query voor geavanceerde bewaking naar wens aan.

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

Omdat logboekback-ups worden uitgevoerd om de 15 minuten, kan back-uptaken bewaking worden omslachtig. Azure Backup vereenvoudigt de bewaking met e-mailwaarschuwingen.

- Er worden waarschuwingen gegenereerd voor alle mislukte back-ups.
- Waarschuwingen worden geconsolideerd op databaseniveau per foutcode.
- Er wordt alleen een e-mailmelding verzonden voor de eerste back-upfout voor een database. 

Ga als volgt te werk om back-uptaken te controleren:

1. Aanmelden bij uw Azure-abonnement in de [Azure-portal](https://portal.azure.com) voor het bewaken van databasewaarschuwingen.

2. Selecteer op het kluisdashboard **waarschuwingen en gebeurtenissen**.

   ![Waarschuwingen en gebeurtenissen selecteren](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. In **waarschuwingen en gebeurtenissen**, selecteer **waarschuwingen voor back-up**.

   ![Waarschuwingen voor back-ups selecteren](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>Beveiliging van een SQL Server-database stoppen

U kunt stoppen back-ups van een SQL Server-database in een aantal manieren:

* Alle toekomstige back-uptaken stoppen en verwijderen van alle herstelpunten.
* Alle toekomstige back-uptaken stoppen maar de herstelpunten behouden.

Houd rekening met het volgende:

Als u de herstelpunten laat, wordt de punten worden opgeschoond in overeenstemming met de back-upbeleid. U betaalt kosten voor het beveiligde exemplaar en de verbruikte opslag totdat alle herstelpunten worden opgeschoond. [Meer informatie](https://azure.microsoft.com/pricing/details/backup/) over prijzen.
- Als u herstelpunten behouden, laat hoewel ze zijn verlopen volgens het bewaarbeleid, houdt Azure Backup altijd een laatste herstelpunt totdat u expliciet back-upgegevens verwijdert.
- Als u een gegevensbron zonder back-up stoppen verwijdert, nieuwe back-ups zullen mislukken. Nogmaals, de oude herstelpunten verlopen volgens het beleid, maar één laatste herstelpunt altijd bewaard totdat u back-up stoppen en verwijderen van de gegevens.
- U kunt geen back-up voor een database is ingeschakeld voor automatische beveiliging, totdat de automatische beveiliging is uitgeschakeld stoppen.

Ga als volgt te werk om de beveiliging van een database te stoppen:

1. In het dashboard van de kluis onder **gebruik**, selecteer **back-Upitems**.

    ![Het menu Back-upitems openen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

2. In **Type back-upbeheer**, selecteer **SQL in Azure VM**.

    ![SQL in Azure VM selecteren](./media/backup-azure-sql-database/sql-restore-backup-items.png)


3. Selecteer de database waarvoor u wenst te beveiliging stoppen.

    ![De database selecteren waarvoor u de beveiliging wilt stoppen](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)


5. Selecteer in het databasemenu **back-up stoppen**.

    ![Back-up stoppen selecteren](./media/backup-azure-sql-database/stop-db-button.png)


6. In **back-up stoppen** in het menu selecteren of u wilt behouden of verwijderen van gegevens. (Optioneel) Geef een reden en opmerking.

    ![Het menu Back-up stoppen](./media/backup-azure-sql-database/stop-backup-button.png)

7. Klik op **back-up stoppen** .

  

### <a name="resume-protection-for-a-sql-database"></a>Beveiliging voor een SQL-database hervatten

Als u de optie **Back-upgegevens behouden** hebt geselecteerd bij het stoppen van de beveiliging voor de SQL-database, kunt u de beveiliging hervatten. Als u de back-upgegevens niet hebt behouden, kan de beveiliging niet worden hervat.

1. Als u de beveiliging voor de SQL-database wilt hervatten, opent u het back-upitem en selecteert u **Back-up hervatten**.

    ![Back-up hervatten selecteren om de databasebeveiliging te hervatten](./media/backup-azure-sql-database/resume-backup-button.png)

2. Selecteer een beleid in het menu **Back-upbeleid** en selecteer vervolgens **Opslaan**.

## <a name="run-an-on-demand-backup"></a>Een on-demand back-up uitvoeren

U kunt verschillende typen back-ups op aanvraag uitvoeren:

* Volledige back-up
* Alleen te kopiëren volledige back-up
* Differentiële back-up
* Logboekback-up

[Meer informatie](backup-architecture.md#sql-server-backup-types) over SQL Server back-up van typen.

## <a name="unregister-a-sql-server-instance"></a>Registratie van een SQL Server-exemplaar ongedaan maken

Registratie van een exemplaar van SQL Server nadat u de beveiliging hebt uitgeschakeld, maar voordat u de kluis verwijderen:

1. Op het kluisdashboard onder **beheren**, selecteer **back-upinfrastructuur**.  

   ![Back-upinfrastructuur selecteren](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. Onder **Beheerservers** selecteert u **Beveiligde servers**.

   ![Beveiligde servers selecteren](./media/backup-azure-sql-database/protected-servers.png)


3. In **beschermde Servers**, de server om de registratie te selecteren. Als u de kluis wilt verwijderen, moet u de registratie van alle servers ongedaan maken.

4. Met de rechtermuisknop op de beveiligde server > **verwijderen**.

   ![Verwijderen selecteren](./media/backup-azure-sql-database/delete-protected-server.png)


## <a name="next-steps"></a>Volgende stappen

[Beoordeling](backup-sql-server-azure-troubleshoot.md) probleemoplossingsinformatie voor back-up van SQL Server-database.
