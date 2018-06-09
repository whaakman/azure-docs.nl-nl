---
title: Back-up van SQL Server-werkbelasting op Azure-Stack
description: Gebruik Azure Backup-Server ter bescherming van de werkbelasting van de SQL Server op Azure-Stack.
services: backup
author: pvrk
manager: Shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/8/2018
ms.author: pullabhk
ms.openlocfilehash: 5541a2fff6bb54f5d62518e7edf54fb9150e3109
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35249285"
---
# <a name="back-up-sql-server-on-azure-stack"></a>Back-up van SQL Server op Azure-Stack
Gebruik dit artikel voor het configureren van Microsoft Azure Backup Server (MABS) ter bescherming van SQL Server-databases op Azure-Stack.

Het beheer van SQL Server-database back-up naar Azure en herstel van Azure omvat drie stappen:

1. Maak een back-upbeleid ter bescherming van SQL Server-databases naar Azure.
2. Maken van back-ups op aanvraag naar Azure.
3. Herstel de database uit Azure.

## <a name="before-you-start"></a>Voordat u begint

[Installeren en de Azure Backup-Server voorbereiden](backup-mabs-install-azure-stack.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Maak een back-upbeleid ter bescherming van SQL Server-databases naar Azure
1. Klik op de Azure Backup-Server-gebruikersinterface op de **beveiliging** werkruimte.

2. Klik op het lint op **nieuw** een nieuwe beveiligingsgroep maken.

    ![Beveiligingsgroep maken](./media/backup-azure-backup-sql/protection-group.png)

    Azure Backup-Server start de wizard beveiligingsgroep, die u bij het maken van helpt een **beveiligingsgroep**. Klik op **Volgende**.

3. In de **Type beveiligingsgroep selecteren** scherm, selecteert u **Servers**.

    ![Type beveiligingsgroep - 'Servers' selecteren](./media/backup-azure-backup-sql/pg-servers.png)

4. In de **groepsleden selecteren** scherm, de lijst Beschikbare leden bevat verschillende gegevensbronnen. Klik op **+** naar een map en de submappen weergeven. Klik op het selectievakje in om een item te selecteren.

    ![SQL-database selecteren](./media/backup-azure-backup-sql/pg-databases.png)

    Alle geselecteerde items worden weergegeven in de lijst met geselecteerde leden. Selecteer de servers of databases die u wilt beveiligen en klik op **volgende**.

5. In de **methode voor gegevensbeveiliging selecteren** scherm, Geef een naam op voor de beveiligingsgroep en selecteer de **ik wil online beveiliging** selectievakje.

    ![Methode voor gegevensbeveiliging - kortetermijndoelstellingen voor schijfbeleid & Online Azure](./media/backup-azure-backup-sql/pg-name.png)

6. In de **Kortetermijndoelen opgeven** scherm, bevatten de benodigde invoeren voor het maken van back-uppunten naar schijf en klikt u op **volgende**.

    In het voorbeeld **bewaartermijn** is **5 dagen**, **Synchronisatiefrequentie** is één keer elke **15 minuten**, dit is de back-up de frequentie. **Snelle volledige back-up** is ingesteld op **20:00 uur**.

    ![Korte-termijndoelstellingen](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > In het voorbeeld weergegeven op elke dag die een back-up is gemaakt door de gewijzigde gegevens overbrengen van 20:00 uur back-up van de vorige dag 20:00 uur serviceverbindingspunt. Dit proces wordt genoemd **snelle volledige back-up**. Transactielogboeken zijn om de 15 minuten gesynchroniseerd. Als u moet de database herstellen op 9:00 uur, de is gemaakt op basis van de logboeken van de laatste snelle volledige back-up punt (8 PM in dit geval).
   >
   >

7. Op de **Controleer toegewezen schijfruimte** scherm, controleert u of de totale opslagruimte beschikbaar is en de potentiële schijfruimte. Klik op **Volgende**.

    ![Toegewezen schijfruimte](./media/backup-azure-backup-sql/pg-storage.png)

    Azure Backup-Server maakt standaard één volume per gegevensbron (SQL Server-database) dat wordt gebruikt voor de eerste back-up. Met deze benadering beperkt de Logical Disk Manager (LDM) Azure Backup-beveiliging met 300 gegevensbronnen (SQL Server-databases). U kunt deze beperking omzeilen, selecteer **dezelfde gegevens in de DPM-opslaggroep plaatsen**. Azure Backup-Server één volume gebruikt voor meerdere gegevensbronnen en maximaal 2000 databases van SQL Server kunt beveiligen met CO-locatie.

    Als u selecteert **volumes automatisch vergroten**, Azure Backup-Server voor het toegenomen volume voor back-accounts wanneer productiegegevens groeit. Als u de optie niet selecteert, beperkt Azure Backup-Server de back-upopslag gebruikt voor de gegevensbronnen in de beveiligingsgroep.

8. In de **methode voor maken van selecteren Replica**, kiest u het eerste herstelpunt maken. U kunt de eerste back-up handmatig (uit netwerk) overdragen om overbelasting van de bandbreedte te voorkomen of via het netwerk. Als u wachten om over te dragen van de eerste back-up wilt, kunt u de tijd voor de eerste overdracht opgeven. Klik op **Volgende**.

    ![Methode voor eerste replicatie](./media/backup-azure-backup-sql/pg-manual.png)

    De eerste back-up vereist overzetten van de gehele gegevensbron (SQL Server-database) van productieserver (SQL Server-machine) naar Azure Backup-Server. Deze gegevens mogelijk te groot worden en de gegevens worden overgedragen via het netwerk kan groter zijn dan bandbreedte. Daarom kunt u de eerste back-up overdragen: **handmatig** (met behulp van verwijderbare media) om te voorkomen dat de bandbreedte-congestie of **automatisch via het netwerk** (op een opgegeven periode).

    Nadat de eerste back-up is voltooid, zijn de rest van de back-ups incrementele back-ups op de eerste back-up. Incrementele back-ups zijn meestal kort en eenvoudig worden overgebracht via het netwerk.

9. Kies wanneer u de consistentiecontrole uitvoeren en klik op **volgende**.

    ![Consistentiecontrole](./media/backup-azure-backup-sql/pg-consistent.png)

    Azure Backup-Server voert een consistentiecontrole op de integriteit van de back-punt. De controlesom van de back-upbestand op de productieserver (SQL Server-machine in dit scenario) en de gegevens waarvan een back-up is gemaakt voor het bestand berekend op basis van de Azure Backup-Server. Als er een conflict optreedt, wordt ervan uitgegaan dat de back-upbestand op Azure Backup-Server is beschadigd. Azure Backup-Server herstelt de gegevens waarvan een back-up is gemaakt door te sturen de blokken die overeenkomt met de checksum komt niet overeen. Omdat consistentiecontroles van de prestatie-intensieve, kunt u de consistentiecontrole plannen of automatisch uitgevoerd.

10. Als u online beveiliging van de gegevensbronnen, schakelt u de databases worden beveiligd door Azure en klikt u op **volgende**.

    ![Selecteer de gegevensbronnen](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. Kies de back-upschema's en bewaarbeleidsregels die aan de behoeften van de beleidsregels van de organisatie.

    ![Planning en retentie](./media/backup-azure-backup-sql/pg-schedule.png)

    In dit voorbeeld worden back-ups eenmaal per dag uitgevoerd op 12:00 uur en 20: 00 (onderste gedeelte van het scherm)

    > [!NOTE]
    > Het is raadzaam om een aantal herstelpunten op korte termijn op schijf voor een snel herstel. Deze herstelpunten worden gebruikt voor operationeel herstel. Azure fungeert als een goede externe locatie met hogere Sla's en beschikbaarheid gegarandeerd.
    >
    >

    **Kunt u beter**: als u back-ups naar Azure te starten na het voltooien van de back-ups van de lokale schijf plant, de meest recente back-ups van schijf altijd worden gekopieerd naar Azure.

12. Kies het bewaarschema voor beleid. De details over de werking van het bewaarbeleid zijn beschikbaar op [gebruik Azure Backup ter vervanging van uw tape-infrastructuur artikel](backup-azure-backup-cloud-as-tape.md).

    ![Bewaarbeleid](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    In dit voorbeeld:

    * Back-ups eenmaal per dag worden uitgevoerd op 12:00 uur en 20: 00 (onderste gedeelte van het scherm) en 180 dagen worden bewaard.
    * De back-up elke zaterdag om 12:00 uur 104 weken wordt bewaard
    * De back-up op de laatste zaterdag om 12:00 uur 60 maanden bewaard
    * De back-up op de laatste zaterdag maart om 12:00 uur tien jaar worden bewaard
13. Klik op **volgende** en selecteer de relevante optie voor het overdragen van de eerste back-up naar Azure. U kunt kiezen **automatisch via het netwerk** of **Offline back-up**.

    * **Automatisch via het netwerk** worden de back-upgegevens naar Azure volgens het schema voor back-up is gekozen.
    * **Offline back-ups** wordt uitgelegd op [Offlineback-upwerkstroom in Azure Backup](backup-azure-backup-import-export.md).

    Kies de relevante gegevensoverdrachtmechanisme voor het verzenden van de eerste back-up naar Azure, klik op **volgende**.

14. Zodra u de details van het beleid in controleren de **samenvatting** scherm, klikt u op **groep maken** om de werkstroom te voltooien. U kunt klikken op **sluiten** en voortgang van de taak in de werkruimte bewaking.

    ![Maken van een beveiligingsgroep In uitvoering](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>Op aanvraag back-up van een SQL Server-database
Tijdens de vorige stappen een back-upbeleid gemaakt, wordt 'herstelpunt' alleen gemaakt als de eerste back-up plaatsvindt. In plaats van de planner te starten wacht, wijst de stappen onder het maken van een herstel trigger handmatig.

1. Wacht totdat de beveiligingsstatus van de groep bevat **OK** voor de database voordat u het herstelpunt maakt.

    ![Leden van beveiligingsgroep](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Met de rechtermuisknop op de database en selecteer **herstelpunt maken**.

    ![Online herstelpunt maken](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Kies **Onlinebeveiliging** in de vervolgkeuzelijst en klik op **OK** aanmaak van een herstelpunt in Azure te starten.

    ![Herstelpunt maken](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. De voortgang van de taak in de **bewaking** werkruimte.

    ![Bewaking van console](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Een SQL Server-database herstellen van Azure
De volgende stappen zijn vereist voor het herstellen van een beveiligde entiteit (SQL Server-database) van Azure.

1. Open de beheerconsole voor Azure Backup-Server. Navigeer naar **herstel** werkruimte waarin u de beveiligde servers kunt zien. Blader op de vereiste database (in dit geval ReportServer$ MSDPM2012). Selecteer een **herstel vanaf** tijd die is opgegeven als een **Online** verwijzen.

    ![Herstelpunt selecteren](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Met de rechtermuisknop op de naam van de database en klik op **herstellen**.

    ![Herstellen van Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. DPM geeft de details van het herstelpunt. Klik op **Volgende**. Voor het overschrijven van de database, selecteert u het hersteltype **herstellen naar oorspronkelijk exemplaar van SQL Server**. Klik op **Volgende**.

    ![Herstellen naar oorspronkelijke locatie](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    In dit voorbeeld herstelt DPM de database naar een ander exemplaar van SQL Server of naar een netwerkmap zelfstandige.

4. In de **herstelopties opgeven** scherm kunt u de opties voor Systeemherstel zoals netwerkbandbreedtegebruik de bandbreedte die wordt gebruikt door de herstelbewerking beperken. Klik op **Volgende**.

5. In de **samenvatting** scherm ziet u alle herstel-configuraties die tot nu toe is opgegeven. Klik op **herstellen**.

    De herstelstatus ziet u de database wordt hersteld. U kunt klikken op **sluiten** naar de wizard te sluiten en de voortgang in de **bewaking** werkruimte.

    ![Proces voor accountherstel initiëren](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Nadat het herstel is voltooid, is de teruggezette database consistent-toepassing.

## <a name="next-steps"></a>Volgende stappen

Zie de [back-up van bestanden en toepassing](backup-mabs-files-applications-azure-stack.md) artikel.
Zie de [back-up van SharePoint op Azure-Stack](backup-mabs-sharepoint-azure-stack.md) artikel.
