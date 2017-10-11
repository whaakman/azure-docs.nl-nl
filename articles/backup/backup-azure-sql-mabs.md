---
title: Azure Backup voor SQL Server-werkbelastingen met Azure Backup-Server | Microsoft Docs
description: Een inleiding tot de back-ups van SQL Server-databases via Azure Backup-Server
services: backup
documentationcenter: 
author: pvrk
manager: Shivamg
editor: 
ms.assetid: c8b1f7ec-26b1-4ef0-a3f2-91aec959daea
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 2af9ebaa8f52690ed63406cbd85b77544d2d900d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-sql-server-to-azure-with-azure-backup-server"></a>Back-up van SQL Server naar Azure met Azure Backup-Server
In dit artikel begeleidt u bij de configuratiestappen voor back-up van SQL Server-databases via Microsoft Azure Backup-Server (MABS).

Het beheer van SQL Server-database back-up naar Azure en herstel van Azure omvat drie stappen:

1. Maak een back-upbeleid ter bescherming van SQL Server-databases naar Azure.
2. Maken van back-ups op aanvraag naar Azure.
3. Herstel de database uit Azure.

## <a name="before-you-start"></a>Voordat u begint
Voordat u begint, zorg ervoor dat er [geïnstalleerd en de Azure Backup-Server voorbereid](backup-azure-microsoft-azure-backup.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Maak een back-upbeleid ter bescherming van SQL Server-databases naar Azure
1. Klik op de Azure Backup-Server-gebruikersinterface op de **beveiliging** werkruimte.
2. Klik op het lint op **nieuw** een nieuwe beveiligingsgroep maken.

    ![Beveiligingsgroep maken](./media/backup-azure-backup-sql/protection-group.png)
3. MABS toont het startscherm met de richtlijnen over het maken van een **beveiligingsgroep**. Klik op **Volgende**.
4. Selecteer **Servers**.

    ![Type beveiligingsgroep - 'Servers' selecteren](./media/backup-azure-backup-sql/pg-servers.png)
5. Vouw de SQL Server-computer waar de databases naar een back-up aanwezig zijn. MABS bevat verschillende gegevensbronnen die u kunnen een back-up van die server. Vouw de **alle SQL-Shares** en selecteer de databases (in dit geval wordt geselecteerd ReportServer$ MSDPM2012 en ReportServer$ MSDPM2012TempDB) naar het back-up worden gemaakt. Klik op **Volgende**.

    ![SQL-database selecteren](./media/backup-azure-backup-sql/pg-databases.png)
6. Geef een naam voor de beveiligingsgroep en selecteer de **ik wil online beveiliging** selectievakje.

    ![Methode voor gegevensbeveiliging - kortetermijnbeveiliging op schijf en Online Azure](./media/backup-azure-backup-sql/pg-name.png)
7. In de **Kortetermijndoelen opgeven** scherm, bevatten de benodigde invoeren voor het maken van back-uppunten op schijf.

    Hier ziet u dat **bewaartermijn** is ingesteld op *5 dagen*, **Synchronisatiefrequentie** is ingesteld op één keer elke *15 minuten* dit is de de frequentie waarmee back-up is gemaakt. **Snelle volledige back-up** is ingesteld op *20:00 uur*.

    ![Doelen voor de korte termijn](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Op 20:00 uur (op basis van de invoer scherm) wordt een back-uppunt elke dag gemaakt door het overdragen van de gegevens die zijn gewijzigd van back-uppunt van de vorige dag 20:00 uur. Dit proces wordt genoemd **snelle volledige back-up**. Tijdens de transactie logboeken worden gesynchroniseerd snelle om de 15 minuten, als er nodig is 21:00 uur: de database herstellen en vervolgens een is gemaakt door de logboeken van de laatste volledige back-up punt (8 pm in dit geval).
   >
   >

8. Klik op **Volgende**

    MABS ziet u de totale opslagruimte beschikbaar is en de mogelijke schijfgebruik voor ruimte.

    ![Toegewezen schijfruimte](./media/backup-azure-backup-sql/pg-storage.png)

    MABS wordt standaard één volume per gegevensbron (SQL Server-database) dat wordt gebruikt voor de eerste back-up gemaakt. Met deze benadering beperkt de Logical Disk Manager (LDM) MABS beveiliging met 300 gegevensbronnen (SQL Server-databases). U kunt deze beperking omzeilen, selecteer de **dezelfde gegevens in de DPM-opslaggroep plaatsen**, optie. Als u deze optie gebruikt, MABS maakt gebruik van één volume voor meerdere gegevensbronnen, waardoor MABS maximaal 2000 SQL-databases beveiligen.

    Als **volumes automatisch vergroten** optie is geselecteerd, MABS kunt account voor het toegenomen back-volume wanneer de productiegegevens groeit. Als **volumes automatisch vergroten** optie niet is geselecteerd, MABS beperkt de back-upopslag gebruikt voor de gegevensbronnen in de beveiligingsgroep.
9. Beheerders de keuze van het overdragen van deze eerste back-up handmatig (uit netwerk) om overbelasting van de bandbreedte te voorkomen of via het netwerk krijgt. Ze kunnen ook de tijd waarop de eerste overdracht kan zich voordoen configureren. Klik op **Volgende**.

    ![Methode voor eerste replicatie](./media/backup-azure-backup-sql/pg-manual.png)

    De eerste back-up is vereist voor overdracht van de gehele gegevensbron (SQL Server-database) van productieserver (SQL Server-machine) naar MABS. Deze gegevens mogelijk te groot worden en de gegevens worden overgedragen via het netwerk kan groter zijn dan bandbreedte. Om deze reden kunnen beheerders de eerste back-up overdragen: **handmatig** (met behulp van verwijderbare media) om te voorkomen dat de bandbreedte-congestie of **automatisch via het netwerk** (op een opgegeven periode).

    Nadat de eerste back-up is voltooid, zijn de rest van de back-ups incrementele back-ups op de eerste back-up. Incrementele back-ups zijn meestal kort en eenvoudig worden overgebracht via het netwerk.
10. Kies wanneer u de consistentiecontrole uitvoeren en klik op **volgende**.

    ![Consistentiecontrole](./media/backup-azure-backup-sql/pg-consistent.png)

    MABS kunt uitvoeren een consistentiecontrole uit de integriteit van de back-punt. De controlesom van de back-upbestand op de productieserver (SQL Server-machine in dit scenario) en de gegevens waarvan een back-up is gemaakt voor het bestand op MABS wordt berekend. In het geval van een conflict optreedt, wordt ervan uitgegaan dat de back-upbestand op MABS beschadigd is. De back-upgegevens herstelt MABS door te sturen de blokken die overeenkomt met de checksum komt niet overeen. De consistentiecontrole is een prestatie-intensieve bewerking, hebben beheerders de mogelijkheid de consistentiecontrole plannen of automatisch uitgevoerd.
11. Als u online beveiliging van de gegevensbronnen, schakelt u de databases worden beveiligd door Azure en klikt u op **volgende**.

    ![Selecteer de gegevensbronnen](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. Beheerders kunnen kiezen voor back-upschema en bewaarbeleidsregels die aan de behoeften van de beleidsregels van hun organisatie.

    ![Planning en retentie](./media/backup-azure-backup-sql/pg-schedule.png)

    In dit voorbeeld worden back-ups eenmaal per dag uitgevoerd op 12:00 uur en 20: 00 (onderste gedeelte van het scherm)

    > [!NOTE]
    > Het is raadzaam om een aantal herstelpunten op korte termijn op schijf voor een snel herstel. Deze herstelpunten worden gebruikt voor 'operationeel herstel'. Azure fungeert als een goede externe locatie met hogere Sla's en beschikbaarheid gegarandeerd.
    >
    >

    **Kunt u beter**: Zorg ervoor dat Azure back-ups zijn gepland na de voltooiing van de lokale schijf back-ups met behulp van DPM. Hierdoor is de laatste schijfback-up naar Azure moet worden gekopieerd.

13. Kies het bewaarschema voor beleid. De details over de werking van het bewaarbeleid zijn beschikbaar op [gebruik Azure Backup ter vervanging van uw tape-infrastructuur artikel](backup-azure-backup-cloud-as-tape.md).

    ![Bewaarbeleid](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    In dit voorbeeld:

    * Back-ups eenmaal per dag worden uitgevoerd op 12:00 uur en 20: 00 (onderste gedeelte van het scherm) en 180 dagen worden bewaard.
    * De back-up elke zaterdag om 12:00 uur 104 weken wordt bewaard
    * De back-up op de laatste zaterdag om 12:00 uur 60 maanden bewaard
    * De back-up op de laatste zaterdag maart om 12:00 uur tien jaar worden bewaard
14. Klik op **volgende** en selecteer de relevante optie voor het overdragen van de eerste back-up naar Azure. U kunt kiezen **automatisch via het netwerk** of **Offline back-up**.

    * **Automatisch via het netwerk** worden de back-upgegevens naar Azure volgens het schema voor back-up is gekozen.
    * Hoe **Offline back-up** works wordt uitgelegd op [Offlineback-upwerkstroom in Azure Backup](backup-azure-backup-import-export.md).

    Kies de relevante gegevensoverdrachtmechanisme voor het verzenden van de eerste back-up naar Azure, klik op **volgende**.
15. Zodra u de details van het beleid in controleren de **samenvatting** scherm, klikt u op de **groep maken** knop om de werkstroom te voltooien. U kunt klikken op de **sluiten** knop en de voortgang van de taak in de werkruimte bewaking.

    ![Maken van een beveiligingsgroep In uitvoering](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>Op aanvraag back-up van een SQL Server-database
Tijdens de vorige stappen een back-upbeleid gemaakt, wordt 'herstelpunt' alleen gemaakt als de eerste back-up plaatsvindt. In plaats van de planner te starten wacht, wijst de stappen onder het maken van een herstel trigger handmatig.

1. Wacht totdat de beveiligingsstatus van de groep bevat **OK** voor de database voordat u het herstelpunt maakt.

    ![Leden van beveiligingsgroep](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Met de rechtermuisknop op de database en selecteer **herstelpunt maken**.

    ![Online herstelpunt maken](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Kies **Onlinebeveiliging** in de vervolgkeuzelijst en klik op **OK**. Hiermee start u het maken van een herstelpunt in Azure.

    ![Herstelpunt maken](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. Vindt u de voortgang van de taak in de **bewaking** werkruimte vindt u een onderhanden taak zoals die worden beschreven in de volgende afbeelding.

    ![Bewaking van console](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Een SQL Server-database herstellen van Azure
De volgende stappen zijn vereist voor het herstellen van een beveiligde entiteit (SQL Server-database) van Azure.

1. Open de beheerconsole van de DPM-server. Navigeer naar **herstel** werkruimte waarin u de servers kunt zien een back-up door DPM. Blader op de vereiste database (in dit geval ReportServer$ MSDPM2012). Selecteer een **herstel vanaf** die eindigt op **Online**.

    ![Herstelpunt selecteren](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Met de rechtermuisknop op de naam van de database en klik op **herstellen**.

    ![Herstellen van Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. DPM geeft de details van het herstelpunt. Klik op **Volgende**. Voor het overschrijven van de database, selecteert u het hersteltype **herstellen naar oorspronkelijk exemplaar van SQL Server**. Klik op **Volgende**.

    ![Herstellen naar oorspronkelijke locatie](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    In dit voorbeeld kan DPM herstel van de database naar een ander exemplaar van SQL Server of naar een netwerkmap zelfstandige.
4. In de **herstelopties opgeven** scherm kunt u de opties voor Systeemherstel zoals netwerkbandbreedtegebruik de bandbreedte die wordt gebruikt door de herstelbewerking beperken. Klik op **Volgende**.
5. In de **samenvatting** scherm ziet u alle herstel-configuraties die tot nu toe is opgegeven. Klik op **herstellen**.

    De herstelstatus ziet u de database wordt hersteld. U kunt klikken op **sluiten** naar de wizard te sluiten en de voortgang in de **bewaking** werkruimte.

    ![Proces voor accountherstel initiëren](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Nadat het herstel is voltooid, is de teruggezette database consistent-toepassing.

### <a name="next-steps"></a>Volgende stappen
• [Veelgestelde vragen over azure Backup](backup-azure-backup-faq.md)
