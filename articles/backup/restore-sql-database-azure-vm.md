---
title: Azure Backup gebruiken om te herstellen van back-up wordt SQL Server-databases op een Azure-VM | Microsoft Docs
description: In dit artikel wordt beschreven hoe u SQL Server-databases die worden uitgevoerd op een Azure VM en die worden opgeslagen met Azure Backup herstellen.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 7f25f26ac1cefa0f2dc3b9b7e30f4a4fc2901c9f
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57436524"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Herstellen van SQL Server-databases op Azure Virtual machines 

In dit artikel wordt beschreven hoe u een SQL Server-database die wordt uitgevoerd op een Azure-machine (VM) die de [Azure Backup](backup-overview.md) service heeft een back-up naar een Azure Backup Recovery Services-kluis.

> [!NOTE]
> Back-up van SQL Server-databases die worden uitgevoerd op een Azure-virtuele machine die gebruikmaakt van Azure Backup is momenteel in openbare preview.

In dit artikel wordt beschreven hoe u SQL Server-databases te herstellen. Zie voor meer informatie, [maakt u een Back-up van SQL Server-databases op Azure Virtual machines](backup-azure-sql-database.md).

## <a name="restore-to-a-time-or-a-recovery-point"></a>Herstellen naar een tijd of een herstelpunt

Azure Backup kunt SQL Server-databases die worden uitgevoerd op virtuele Azure-machines als volgt herstellen:

- Met transactielogboekback-ups voor het herstellen naar een specifieke datum of tijd (voor de tweede). Azure Backup bepaalt automatisch de juiste volledige differentiële back-up en de keten van logboekback-ups die nodig zijn om terug te zetten op basis van de geselecteerde tijd.
- Een specifieke volledige of differentiële back-up te herstellen naar een specifiek herstelpunt herstellen.


## <a name="prerequisites"></a>Vereisten

Voordat u een database herstelt, Let op het volgende:

- U kunt de database herstellen naar een exemplaar van SQL Server in dezelfde Azure-regio.
- De doelserver moet zijn geregistreerd bij dezelfde kluis als de bron.
- TDE-versleutelde database herstellen naar een andere SQL Server, moet u eerst [herstellen van het certificaat naar de doelserver](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
- Voordat u de 'master'-database hebt hersteld, start u de SQL Server-exemplaar in de modus voor één gebruiker met behulp van de opstartoptie **-m AzureWorkloadBackup**.
    - De waarde voor **-m** is de naam van de client.
    - Alleen de naam van de opgegeven client kunt openen van de verbinding.
- Voor alle systeemdatabases (model, master, msdb), door de SQL Server Agent-service te stoppen voordat u de terugzetbewerking kan worden geactiveerd.
- Sluit alle toepassingen die mogelijk probeert uit te voeren van een verbinding met een van deze databases.

## <a name="restore-a-database"></a>Een database herstellen

Als u wilt herstellen, moet u de volgende machtigingen:

* **Back-up van Operator** machtigingen in de kluis waar u de herstelbewerking uitvoert.
* **Inzender (schrijven)** toegang tot de bron-VM waarvan een back-up wordt gemaakt.
* **Inzender (schrijven)** toegang tot de doel-VM:
    - Als u naar dezelfde virtuele machine herstellen wilt, is dit de bron-VM.
    - Als u naar een alternatieve locatie herstellen bent, is dit de nieuwe doel-VM. 

Als volgt herstellen:
1. Open de kluis waarin de SQL Server-VM is geregistreerd.
2. Op het kluisdashboard onder **gebruik**, selecteer **back-Upitems**.
3. In **back-Upitems**onder **Type back-upbeheer**, selecteer **SQL in Azure VM**.

    ![SQL in Azure VM selecteren](./media/backup-azure-sql-database/sql-restore-backup-items.png)

4. Selecteer de database te herstellen.

    ![De te herstellen database selecteren](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

5. Bekijk het databasemenu. Het biedt informatie over de databaseback-up, met inbegrip van: 

    * De oudste en meest recente herstelpunten.
    * De status van back-logboek voor de afgelopen 24 uur voor databases die zijn opgenomen in de modus voor volledige en bulksgewijs geregistreerde herstel en die zijn geconfigureerd voor transactionele logboekback-ups.

6. Selecteer **Restore DB**. 

    ![DB herstellen selecteren](./media/backup-azure-sql-database/restore-db-button.png)

7. In **configuratie herstellen**, Geef op waar om de gegevens te herstellen:
    - **Alternatieve locatie**: De database herstellen naar een alternatieve locatie en houd de oorspronkelijke brondatabase.
    - **DB overschrijven**: De gegevens herstellen naar hetzelfde exemplaar van SQL Server als de oorspronkelijke bron. Deze optie wordt de oorspronkelijke database overschreven.

    > [!Important]
    > Als de geselecteerde database bij een AlwaysOn-beschikbaarheidsgroep hoort, niet de database worden overschreven door SQL Server toegestaan. Alleen **alternatieve locatie** beschikbaar is.
    >

    ![Het menu Configuratie herstellen](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Herstellen naar een alternatieve locatie

1. In de **configuratie herstellen** menu onder **waar terugzetten**, selecteer **alternatieve locatie**.
2. Selecteer de naam van SQL Server en exemplaar waarnaar u wilt herstellen van de database.
3. Geef in het vak **Herstelde databasenaam** de naam van de doeldatabase op.
4. indien van toepassing, selecteert u **overschrijven als de database met dezelfde naam al in het geselecteerde SQL-exemplaar bestaat**.
5. Selecteer **OK**.

    ![Waarden opgeven voor het menu Configuratie herstellen](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. In **herstelpunt selecteren**, selecteert of [herstellen naar een bepaald punt in tijd](#restore-to-a-specific-point-in-time) of [herstellen naar een specifiek herstelpunt](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > De point-in-time-restore is alleen beschikbaar voor logboekback-ups voor databases die zich in de volledige en bulksgewijs geregistreerde herstelmodus. 

### <a name="restore-and-overwrite"></a>Terugzetten en overschrijven

1. In de **configuratie herstellen** menu onder **waar terugzetten**, selecteer **DB overschrijven** > **OK**.

    ![DB overschrijven selecteren](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. In **herstelpunt selecteren**, selecteer **Logboeken (tijdstip)** naar [herstellen naar een bepaald punt in tijd](#restore-to-a-specific-point-in-time). Of selecteer **volledig en differentieel** om te herstellen naar een [specifieke herstelpunt](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > De point-in-time-restore is alleen beschikbaar voor logboekback-ups voor databases die zich in de volledige en bulksgewijs geregistreerde herstelmodus. 

### <a name="restore-to-a-specific-point-in-time"></a>Herstellen naar een bepaald punt in tijd

Als u hebt geselecteerd **Logboeken (tijdstip)** als het type herstel het volgende doen:

1.  Onder **datum/tijd herstellen**, opent u de agenda. De datums waarop herstelpunten worden vet weergegeven in de kalender en de huidige datum is gemarkeerd.
1. Selecteer een datum waarvoor herstelpunten. U kunt geen datums waarvoor geen herstelpunten selecteren.

    ![De agenda openen](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

1. Als u een datum hebt geselecteerd, worden de beschikbare herstelpunten in chronologische volgorde weergegeven in de tijdlijngrafiek.
1. Geef een tijd voor het herstel in de tijdlijngrafiek of Selecteer een tijdstip. Selecteer vervolgens **OK**.

    ![Selecteer een tijdstip herstellen](./media/backup-azure-sql-database/recovery-point-logs-graph.png)

 
1. Op de **Advanced Configuration** menu, als u wilt de database niet werkt behouden na het herstel inschakelen **herstellen met GEENHERSTEL**.
1. Als u wijzigen van de locatie voor terugzetten op de doelserver wilt, voert u een nieuw doelpad.
1. Selecteer **OK**.

    ![Het menu Geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. Selecteer **Herstellen** in het menu **Herstellen** om de hersteltaak te starten.
1. De terugzetbewerking voortgang volgen in de **meldingen** gebied, of door te selecteren die volgen **hersteltaken** in het menu van de database.

    ![Voortgang hersteltaak](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-to-a-specific-restore-point"></a>Herstellen naar een specifiek herstelpunt

Als u hebt geselecteerd **volledig en differentieel** als het type herstel het volgende doen:

1. Selecteer een herstelpunt in de lijst en selecteer **OK** om de procedure voor het herstelpunt te voltooien.

    ![Een volledig herstelpunt kiezen](./media/backup-azure-sql-database/choose-fd-recovery-point.png)
        
1. Op de **Advanced Configuration** menu, als u wilt de database niet werkt behouden na het herstel inschakelen **herstellen met GEENHERSTEL**.
1. Als u wijzigen van de locatie voor terugzetten op de doelserver wilt, voert u een nieuw doelpad. 
1. Selecteer **OK**.

    ![Het menu Geavanceerde configuratie](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. Selecteer **Herstellen** in het menu **Herstellen** om de hersteltaak te starten.
1. De terugzetbewerking voortgang volgen in de **meldingen** gebied, of door te selecteren die volgen **hersteltaken** in het menu van de database.

    ![Voortgang hersteltaak](./media/backup-azure-sql-database/restore-job-notification.png)

## <a name="next-steps"></a>Volgende stappen

[Beheren en bewaken van](manage-monitor-sql-database-backup.md) SQL Server-databases die worden ondersteund door Azure Backup.
