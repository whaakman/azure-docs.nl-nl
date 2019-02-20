---
title: Herstel back-ups van SQL Server-databases op een Azure-VM met Azure Backup | Microsoft Docs
description: In dit artikel wordt beschreven hoe u SQL Server-databases die worden uitgevoerd op een virtuele machine van Azure die worden ondersteund met Azure Backup herstellen
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 3571eb2471f9b3f06eb509937fd11866b4e0caa8
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430806"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Herstellen van SQL Server-databases op Azure Virtual machines 


Dit artikel wordt beschreven hoe u kunt herstellen van een SQL Server-database op een Azure-VM is de back-up naar een Azure Backup Recovery Services-kluis met de [Azure Backup](backup-overview.md) service.


> [!NOTE]
> Back-up van SQl Server-databases die worden uitgevoerd op een Azure-VM met Azure Backup is momenteel in openbare preview.


In dit artikel wordt beschreven hoe u SQL Server-databases te herstellen. Als u dit nog niet hebt geconfigureerd voor back-up voor de databases, volgt u de instructies in [in dit artikel](backup-azure-sql-database.md)



## <a name="about-restoring-databases"></a>Over het herstellen van databases

Azure Backup kunt SQL Server-databases die worden uitgevoerd op virtuele Azure-machines als volgt herstellen:

- Herstellen naar een bepaalde datum of tijd (voor de tweede), met behulp van transactielogboekback-ups. Azure Backup bepaalt automatisch de juiste volledige differentiële back-up en de keten van logboekback-ups, die nodig zijn om terug te zetten op basis van de geselecteerde tijd.
- Herstellen van een een specifieke volledige of differentiële back-up te herstellen naar een specifiek herstelpunt, in plaats van een bepaald tijdstip.


### <a name="prerequisites"></a>Vereisten

Voordat u een database terugzet Let op het volgende:

- U kunt de database herstellen naar een exemplaar van SQL Server in dezelfde Azure-regio.
- De doelserver moet zijn geregistreerd bij dezelfde kluis als de bron.
- Als u wilt een versleutelde TDE-database herstellen naar een andere SQL Server, moet u eerst [herstellen van het certificaat naar de doelserver](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
- Voordat u een herstel van de database 'master' activeert, start u de SQL Server-exemplaar in de modus voor één gebruiker met de opstartoptie **-m AzureWorkloadBackup**.
    - De waarde voor **-m** is de naam van de client.
    - Alleen de naam van de opgegeven client is toegestaan om de verbinding te openen.
- Voor alle systeemdatabases (model, master, msdb), moet u de SQL Server Agent-service stoppen voordat u de herstelbewerking activeert.
- Sluit alle toepassingen waarmee mogelijk een verbinding met een van deze databases kan worden 'gestolen'.

## <a name="restore-a-database"></a>Een database herstellen

Om terug te zetten moet u de volgende machtigingen:

* **Back-up van Operator** machtigingen in kluis de waarvan u de herstelbewerking uitvoert.
* **Inzender (schrijven)** toegang tot de bron-VM waarvan een back-up wordt gemaakt.
* **Inzender (schrijven)** toegang tot de doel-VM:
    - Als u wilt terugzetten naar dezelfde virtuele machine dit is de bron-VM.
    - Als u wilt herstellen naar een alternatieve locatie dit is de nieuwe doel-VM. 

Als volgt herstellen:
1. Open de kluis waarin de SQL Server-VM is geregistreerd.
2. Op het kluisdashboard onder **gebruik**, selecteer **back-Upitems**.

    ![Het menu Back-upitems openen](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

3. In **back-Upitems**onder **Type back-upbeheer**, selecteer **SQL in Azure VM**.

    ![SQL in Azure VM selecteren](./media/backup-azure-sql-database/sql-restore-backup-items.png)

4. Selecteer de database te herstellen.

    ![De te herstellen database selecteren](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

5. Bekijk het databasemenu. Het biedt informatie over de databaseback-up, met inbegrip van: 

    * De oudste en meest recente herstelpunten.
    * Back-upstatus voor de afgelopen 24 uur voor databases in de modus voor volledige en bulksgewijs geregistreerde herstel, meld u als geconfigureerd voor transactionele logboekback-ups.

6. Klik op **Restore DB**. 

    ![DB herstellen selecteren](./media/backup-azure-sql-database/restore-db-button.png)

7. In **configuratie herstellen**, opgeven waar u wilt de gegevens te herstellen:
    - **Alternatieve locatie**: De database herstellen naar een alternatieve locatie en de oorspronkelijke brondatabase behouden.
    - **DB overschrijven**: De gegevens herstellen naar hetzelfde exemplaar van SQL Server als de oorspronkelijke bron. Als u deze optie kiest, wordt de oorspronkelijke database overschreven.

    > [!Important]
    > Als de geselecteerde database tot een AlwaysOn-beschikbaarheidsgroep behoort, wordt het overschrijven van de database niet toegestaan door SQL Server. Alleen **alternatieve locatie** beschikbaar is.
    >

    ![Het menu Configuratie herstellen](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Herstellen naar een alternatieve locatie

1. In de **configuratie herstellen** menu onder **waar terugzetten**, selecteer **alternatieve locatie**.
2. Selecteer de naam van SQL Server en exemplaar waarnaar u wilt herstellen van de database.
3. In **databasenaam hersteld** voert u de naam van de doeldatabase.
4. indien van toepassing, selecteert u **overschrijven als de database met dezelfde naam al in het geselecteerde SQL-exemplaar bestaat**.
5. Klik op **OK**.

    ![Waarden opgeven voor het menu Configuratie herstellen](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. In **herstelpunt selecteren**, selecteert of [herstellen naar een bepaald punt in tijd](#restore-to-a-specific-point-in-time), of om te herstellen naar een [specifieke herstelpunt](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > De point-in-time-restore is alleen beschikbaar voor logboekback-ups voor databases met volledige en bulksgewijs geregistreerde herstelmodus. 


### <a name="restore-and-overwrite"></a>Terugzetten en overschrijven

1. In de **configuratie herstellen** menu onder **waar terugzetten**, selecteer **DB overschrijven** > **OK**.

    ![DB overschrijven selecteren](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. In **herstelpunt selecteren**, selecteer ** Logboeken (tijdstip) naar [herstellen naar een bepaald punt in tijd](#restore-to-a-specific-point-in-time), of **volledig en differentieel** om te herstellen naar een [specifieke herstelpunt](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > Herstel naar een bepaald tijdstip is alleen beschikbaar voor logboekback-ups voor databases met een volledige back-up en een herstelmodel met bulksgewijs geregistreerde wijzigingen. 




    
### <a name="restore-to-a-specific-point-in-time"></a>Herstellen naar een bepaald punt in tijd

Als u hebt geselecteerd **Logboeken (tijdstip)** als het type herstel het volgende doen:

1.  Onder **datum/tijd herstellen**, selecteert u de miniwerkbalk kalender. Op de **agenda**, de vet weergegeven datums hebben herstelpunten en de huidige datum is gemarkeerd.
2. Selecteer een datum met herstelpunten. Datums zonder herstelpunten kunnen niet worden geselecteerd.

    ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

3. Als u een datum hebt geselecteerd, worden de beschikbare herstelpunten in chronologische volgorde weergegeven in de tijdlijngrafiek.
4. Geef een tijd voor het herstellen met behulp van de tijdlijngrafiek of Selecteer een tijd. Klik vervolgens op **OK**.

    ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

 
4. Op de **Advanced Configuration** menu, als u wilt de database niet-operationeel te behouden na het herstel inschakelen **herstellen met GEENHERSTEL**.
5. Als u wijzigen van de locatie voor terugzetten op de doelserver wilt, voert u een nieuw doelpad.
6. Klik op **OK**.

    ![Het menu Geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)


7. Selecteer **Herstellen** in het menu **Herstellen** om de hersteltaak te starten.
8. De terugzetbewerking voortgang volgen in de **meldingen** gebied, of selecteer **hersteltaken** in het menu van de database.

    ![Voortgang hersteltaak](./media/backup-azure-sql-database/restore-job-notification.png)



### <a name="restore-to-a-specific-restore-point"></a>Herstellen naar een specifiek herstelpunt

Als u hebt geselecteerd **volledig en differentieel** als het type herstel het volgende doen:


1. Selecteer een herstelpunt in de lijst en klikt u op **OK** om het herstelpunt procedure te voltooien.

    ![Een volledig herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)
        
2. Op de **Advanced Configuration** menu, als u wilt de database niet-operationeel te behouden na het herstel inschakelen **herstellen met GEENHERSTEL**.
3. Als u wijzigen van de locatie voor terugzetten op de doelserver wilt, voert u een nieuw doelpad. 
4. Klik op **OK**.

    ![Het menu Geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

7. Selecteer **Herstellen** in het menu **Herstellen** om de hersteltaak te starten.
8. De terugzetbewerking voortgang volgen in de **meldingen** gebied, of selecteer **hersteltaken** in het menu van de database.

    ![Voortgang hersteltaak](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="next-steps"></a>Volgende stappen

[Beheren en bewaken van](manage-monitor-sql-database-backup.md) SQL Server-databases back-ups van door Azure Backup.
