---
title: Back-up van SQL Server-workloads in Azure Stack
description: Azure Backup Server gebruiken om te beveiligen van SQL Server-werkbelasting op Azure Stack.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/8/2018
ms.author: adigan
ms.openlocfilehash: fb064c39fa014515fb2a3f4ccc96ce216f2f7b2e
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55493504"
---
# <a name="back-up-sql-server-on-stack"></a>Back-up van SQL Server op de Stack
In dit artikel gebruiken om te configureren van Microsoft Azure Backup-Server (MABS) ter bescherming van SQL Server-databases in Azure Stack.

Het beheer van SQL Server-database back-up naar Azure en het herstel van Azure omvat drie stappen:

1. Een back-upbeleid ter bescherming van SQL Server-databases maken
2. Back-ups op aanvraag maken
3. De database herstellen van schijven en naar Azure

## <a name="before-you-start"></a>Voordat u begint

[Installeren en Azure Backup Server voorbereiden](backup-mabs-install-azure-stack.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Een back-upbeleid ter bescherming van SQL Server-databases in Azure maken
1. Klik op de gebruikersinterface van Azure Backup Server, de **Protection** werkruimte.

2. Klik op het lint **nieuw** om een nieuwe beveiligingsgroep te maken.

    ![Beveiligingsgroep maken](./media/backup-azure-backup-sql/protection-group.png)

    Azure Backup-Server start de wizard beveiligingsgroep, die u bij het maken van helpt een **beveiligingsgroep**. Klik op **volgende**.

3. In de **Type beveiligingsgroep selecteren** scherm, schakel **Servers**.

    ![Type beveiligingsgroep - 'Servers' selecteren](./media/backup-azure-backup-sql/pg-servers.png)

4. In de **groepsleden selecteren** scherm, de lijst met beschikbare leden worden weergegeven voor de verschillende gegevensbronnen. Klik op **+** naar een map en de submappen weergeven. Klik op het selectievakje in om een item te selecteren.

    ![SQL-database selecteren](./media/backup-azure-backup-sql/pg-databases.png)

    Alle geselecteerde items worden weergegeven in de lijst met geselecteerde leden. Na het selecteren van de servers of databases die u wilt beveiligen, klikt u op **volgende**.

5. In de **methode voor gegevensbeveiliging selecteren** scherm, Geef een naam op voor de beveiligingsgroep en selecteer de **ik Kies voor online beveiliging** selectievakje.

    ![Methode voor gegevensbeveiliging - kortetermijndoelstellingen voor schijfbeleid en Online Azure](./media/backup-azure-backup-sql/pg-name.png)

6. In de **Kortetermijndoelen opgeven** scherm, bevatten de benodigde invoeren voor het maken van back-uppunten naar schijf en klikt u op **volgende**.

    In het voorbeeld **bewaartermijn** is **5 dagen**, **Synchronisatiefrequentie** eenmaal is elke **15 minuten**, dit is de back-up de frequentie. **Snelle volledige back-up** is ingesteld op **8:00 uur**.

    ![Korte-termijndoelstellingen](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > In het voorbeeld, om 8:00 uur op elke dag is een back-uppunt gemaakt door het overdragen van de gewijzigde gegevens van de vorige dag 8:00 uur back-uppunt. Dit proces wordt genoemd **snelle volledige back-up**. Transactielogboeken zijn om de 15 minuten gesynchroniseerd. Als u nodig hebt voor het herstellen van de database om 21:00 uur, het herstelpunt is gemaakt op basis van de logboeken van de laatste snelle volledige back-uppunt (8 PM in dit geval).
   >
   >

7. Op de **schijftoewijzing controleren** scherm, controleert u of de totale opslagruimte beschikbaar is en de potentiële schijfruimte. Klik op **volgende**.

8. In de **Replica methode voor maken**, kiezen hoe u uw eerste herstelpunt maken. U kunt de eerste back-up handmatig (uit netwerk) om te voorkomen opstoppingen in het bandbreedte overdragen of via het netwerk. Als u ervoor kiest om over te dragen van de eerste back-up wachten, kunt u de tijd voor de initiële overdracht opgeven. Klik op **volgende**.

    ![Methode van initiële replicatie](./media/backup-azure-backup-sql/pg-manual.png)

    De eerste back-up moet overdragen van de gehele gegevensbron (SQL Server-database) van productieserver (SQL Server-machine) naar Azure Backup-Server. Deze gegevens zijn mogelijk grote, en bandbreedte als de gegevens worden overgebracht via het netwerk kunt overschrijden. U kunt kiezen om deze reden om over te dragen van de eerste back-up: **Handmatig** (met behulp van verwijderbare media) om te voorkomen opstoppingen in het bandbreedte, of **automatisch via het netwerk** (op een bepaald tijdstip).

    Nadat de eerste back-up is voltooid, zijn de rest van de back-ups incrementele back-ups op de eerste back-up. Incrementele back-ups zijn meestal klein en eenvoudig worden overgedragen via het netwerk.

9. Kies wanneer u de consistentiecontrole uitvoeren en klikt u op **volgende**.

    ![Consistentiecontrole](./media/backup-azure-backup-sql/pg-consistent.png)

    Azure Backup Server voert een consistentiecontrole uit op de integriteit van de back-uppunt. Azure Backup Server berekent de controlesom van de back-upbestand op de productieserver (SQL Server-machine in dit scenario) en de gegevens waarvan een back-up is gemaakt voor dat bestand. Als er een conflict optreedt, wordt ervan uitgegaan dat het bestand waarvan een back-up is gemaakt op Azure Backup Server is beschadigd. Azure Backup Server herstelt de gegevens waarvan een back-up is gemaakt door te sturen, de blokken die overeenkomt met de controlesom komt niet overeen. Omdat consistentiecontroles van de prestatie-intensieve zijn, kunt u de consistentiecontrole plannen of automatisch laten uitvoeren.

10. Als u online beveiliging van de gegevensbronnen, selecteert u de databases moeten worden beveiligd op Azure en klikt u op **volgende**.

    ![Gegevensbronnen selecteren](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. Kies back-upschema's en voor het bewaren van die geschikt is voor de beleidsregels van de organisatie.

    ![Schema en de retentie](./media/backup-azure-backup-sql/pg-schedule.png)

    In dit voorbeeld worden back-ups één keer per dag uitgevoerd om 12:00 uur en 8 uur (onderste gedeelte van het scherm)

    > [!NOTE]
    > Het is raadzaam om een aantal herstelpunten op korte termijn op schijf voor snel herstel. Deze herstelpunten worden gebruikt voor operationeel herstel. Azure doet dienst als een goede externe locatie met hogere Sla's en beschikbaarheid gegarandeerd.
    >
    >

    **Best Practice**: Als u back-ups naar Azure te starten na het voltooien van de lokale schijf back-ups plannen, worden altijd de meest recente back-ups van schijf gekopieerd naar Azure.

12. Kies het bewaarschema voor beleid. De details over de werking van het bewaarbeleid zijn opgegeven op [gebruik Azure Backup vervangt u uw tape-infrastructuur-artikel](backup-azure-backup-cloud-as-tape.md).

    ![Bewaarbeleid](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    In dit voorbeeld:

    * Back-ups gaat één keer per dag om 12:00 uur en 8 uur (onderste gedeelte van het scherm) en 180 dagen worden bewaard.
    * De back-up op zaterdag om 12:00 uur 104 weken wordt bewaard
    * De back-up op de laatste zaterdag om 12:00 uur 60 maanden wordt bewaard
    * De back-up op de laatste zaterdag van maart om 12:00 uur tien jaar worden bewaard
13. Klik op **volgende** en selecteer de gewenste optie voor het overdragen van de eerste back-up naar Azure. U kunt ervoor kiezen **automatisch via het netwerk**

14. Nadat u de details van het beleid in controleren de **samenvatting** scherm, klikt u op **groep maken** om de werkstroom te voltooien. U kunt klikken op **sluiten** en controleren van de voortgang van de taak in de werkruimte bewaking.

    ![Het maken van de beveiligingsgroep wordt uitgevoerd](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>On-demand back-up van een SQL Server-database
Tijdens de vorige stappen hebt gemaakt van een back-upbeleid, wordt een 'herstelpunt' alleen gemaakt als de eerste back-up plaatsvindt. In plaats van te wachten tot de scheduler geactiveerd, wordt de stappen hieronder trigger het maken van een herstel handmatig verwijzen.

1. Wacht totdat de status van de beveiliging groep bevat **OK** voor de database voordat u het herstelpunt maakt.

    ![Leden van beveiligingsgroep](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Met de rechtermuisknop op de database en selecteer **herstelpunt maken**.

    ![Online herstelpunt maken](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Kies **Onlinebeveiliging** in de vervolgkeuzelijst en klik op **OK** om te beginnen met het maken van een herstelpunt in Azure.

    ![Herstelpunt maken](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. Bekijk de voortgang van de taak in de **bewaking** werkruimte.

    ![Bewaking van console](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Een SQL Server-database herstellen van Azure
De volgende stappen zijn vereist voor het herstellen van een beveiligde entiteit (SQL Server-database) van Azure.

1. Open de beheerconsole van de Azure Backup-Server. Navigeer naar **Recovery** werkruimte waar u de beveiligde servers kunt zien. Blader door de vereiste database (in dit geval ReportServer$ MSDPM2012). Selecteer een **herstel vanaf** tijd die is opgegeven als een **Online** verwijzen.

    ![Herstelpunt selecteren](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Met de rechtermuisknop op de naam van de database en klik op **herstellen**.

    ![Herstellen van Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. MABS worden de details van het herstelpunt. Klik op **volgende**. Als u wilt overschrijven van de database, selecteert u het hersteltype **herstellen naar oorspronkelijk exemplaar van SQL Server**. Klik op **volgende**.

    ![Herstellen naar oorspronkelijke locatie](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    In dit voorbeeld wordt de database naar een ander exemplaar van SQL Server of naar een netwerkmap zelfstandige hersteld met MABS.

4. In de **herstelopties opgeven** scherm, kunt u de herstelopties zoals beperking van de netwerkbandbreedte te beperken van de bandbreedte die wordt gebruikt door herstel. Klik op **volgende**.

5. In de **samenvatting** scherm ziet u alle van de recovery-configuraties geleverd tot nu toe. Klik op **herstellen**.

    De herstelstatus ziet u de database wordt hersteld. U kunt klikken op **sluiten** aan de wizard te sluiten en de voortgang in de **bewaking** werkruimte.

    ![Herstelproces te starten](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Nadat het herstel is voltooid, wordt door de herstelde database toepassingsconsistent is.

## <a name="next-steps"></a>Volgende stappen

Zie de [back-up van bestanden en de toepassing](backup-mabs-files-applications-azure-stack.md) artikel.
Zie de [back-up van SharePoint op Azure Stack](backup-mabs-sharepoint-azure-stack.md) artikel.
