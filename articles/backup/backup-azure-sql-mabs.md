---
title: Azure Backup voor SQL Server-workloads met Azure Backup Server
description: Een inleiding tot de back-ups van SQL Server-databases met Azure Backup Server
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 03/24/2017
ms.author: kasinh
ms.openlocfilehash: 72b2368979f0c9e546e1c7ef7fc462bf1d64c947
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490462"
---
# <a name="back-up-sql-server-to-azure-with-azure-backup-server"></a>Back-up van SQL Server naar Azure met Azure Backup Server
In dit artikel leidt u door de configuratiestappen voor back-up van SQL Server-databases met behulp van Microsoft Azure Backup Server (MABS).

Het beheer van SQL Server-database back-up naar Azure en het herstel van Azure omvat drie stappen:

1. Maak een back-upbeleid ter bescherming van SQL Server-databases naar Azure.
2. Maak de back-ups op aanvraag naar Azure.
3. Herstel de database van Azure.

## <a name="before-you-start"></a>Voordat u begint
Voordat u begint, zorgt u ervoor dat u hebt [geïnstalleerd en de Azure Backup Server voorbereid](backup-azure-microsoft-azure-backup.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Een back-upbeleid ter bescherming van SQL Server-databases in Azure maken
1. Klik op de gebruikersinterface van Azure Backup Server, de **Protection** werkruimte.
2. Klik op het lint **nieuw** om een nieuwe beveiligingsgroep te maken.

    ![Beveiligingsgroep maken](./media/backup-azure-backup-sql/protection-group.png)
3. MABS ziet u het startscherm met de richtlijnen over het maken van een **beveiligingsgroep**. Klik op **volgende**.
4. Selecteer **Servers**.

    ![Type beveiligingsgroep - 'Servers' selecteren](./media/backup-azure-backup-sql/pg-servers.png)
5. Vouwt u de SQL Server-computer waar de databases naar een back-up aanwezig zijn. MABS bevat verschillende gegevensbronnen die een back-kunnen worden gemaakt van die server. Vouw de **alle SQL-Shares** en selecteer de databases (in dit geval we geselecteerde ReportServer$ MSDPM2012 en ReportServer$ MSDPM2012TempDB) naar het back-up worden gemaakt. Klik op **volgende**.

    ![SQL-database selecteren](./media/backup-azure-backup-sql/pg-databases.png)
6. Geef een naam op voor de beveiligingsgroep en selecteer de **ik Kies voor online beveiliging** selectievakje.

    ![Methode voor gegevensbeveiliging - kortetermijnbeveiliging op schijf en Online Azure](./media/backup-azure-backup-sql/pg-name.png)
7. In de **Kortetermijndoelen opgeven** scherm, bevatten de benodigde invoeren voor het maken van back-uppunten op schijf.

    Hier zien we dat **bewaartermijn** is ingesteld op *5 dagen*, **Synchronisatiefrequentie** is ingesteld op één keer elke *15 minuten* dit is de de frequentie waarmee back-up wordt gemaakt. **Snelle volledige back-up** is ingesteld op *8:00 uur*.

    ![Doelstellingen van de korte termijn](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Een back-uppunt wordt elke dag om 8:00 uur (op basis van de invoer van het scherm) door het overdragen van de gegevens die is gewijzigd van de vorige dag 8:00 uur back-uppunt gemaakt. Dit proces wordt genoemd **snelle volledige back-up**. Tijdens de transactie logboeken worden gesynchroniseerd snelle om de 15 minuten als nodig voor het herstellen van de database om 21:00 – is het punt is gemaakt door af te spelen de logboeken van de laatste volledige back-uppunt (8 pm in dit geval).
   >
   >

8. Klik op **Volgende**

    MABS toont de totale opslagruimte beschikbaar is en de mogelijke schijfgebruik voor ruimte.

    ![Toegewezen schijfruimte](./media/backup-azure-backup-sql/pg-storage.png)

    MABS maakt standaard één volume per gegevensbron (SQL Server-database) dat wordt gebruikt voor de eerste back-up. Met deze aanpak beperkt de Logical Disk Manager (LDM) MABS-beveiliging met 300 gegevensbronnen (SQL Server-databases). U kunt deze beperking omzeilen, selecteer de **dezelfde gegevens in de DPM-opslaggroep plaatsen**, optie. Als u deze optie gebruikt, MABS maakt gebruik van een enkel volume voor meerdere gegevensbronnen, waardoor MABS ter bescherming van maximaal 2000 SQL-databases.

    Als **volumes automatisch vergroten** optie is geselecteerd, MABS kunt rekening voor de verbeterde back-upvolume wanneer de productiegegevens groeit. Als **volumes automatisch vergroten** optie niet is ingeschakeld, MABS beperkt de back-upopslag gebruikt met de gegevensbronnen in de beveiligingsgroep.
9. Beheerders, krijgen de keuze van het overdragen van deze eerste back-up handmatig (uit netwerk) om te voorkomen opstoppingen in het bandbreedte of via het netwerk. Ze kunnen ook de tijd waarop de initiële overdracht kan gebeuren configureren. Klik op **volgende**.

    ![Methode van initiële replicatie](./media/backup-azure-backup-sql/pg-manual.png)

    De eerste back-up is vereist voor overdracht van de gehele gegevensbron (SQL Server-database) van productieserver (SQL Server-machine) naar MABS. Deze gegevens zijn mogelijk grote, en bandbreedte als de gegevens worden overgebracht via het netwerk kunt overschrijden. Beheerders kunnen kiezen om deze reden om over te dragen van de eerste back-up: **Handmatig** (met behulp van verwijderbare media) om te voorkomen opstoppingen in het bandbreedte, of **automatisch via het netwerk** (op een bepaald tijdstip).

    Nadat de eerste back-up is voltooid, zijn de rest van de back-ups incrementele back-ups op de eerste back-up. Incrementele back-ups zijn meestal klein en eenvoudig worden overgedragen via het netwerk.
10. Kies wanneer u de consistentiecontrole uitvoeren en klikt u op **volgende**.

    ![Consistentiecontrole](./media/backup-azure-backup-sql/pg-consistent.png)

    MABS kunt uitvoeren een consistentiecontrole uit de integriteit van de back-uppunt. De controlesom van de back-upbestand op de productieserver (SQL Server-machine in dit scenario) en de gegevens waarvan een back-up is gemaakt voor het bestand bij MABS wordt berekend. In het geval van een conflict optreedt, wordt ervan uitgegaan dat het bestand een back-up op MABS beschadigd is. MABS herstelt de gegevens waarvan een back-up is gemaakt door te sturen, de blokken die overeenkomt met de controlesom komt niet overeen. Als de consistentiecontrole een prestatie-intensieve bewerking is, hebben beheerders de mogelijkheid om de consistentiecontrole plannen of automatisch uitgevoerd.
11. Als u online beveiliging van de gegevensbronnen, selecteert u de databases moeten worden beveiligd op Azure en klikt u op **volgende**.

    ![Gegevensbronnen selecteren](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. Beheerders kunnen kiezen voor back-upschema's en voor het bewaren van die geschikt is voor het beleid van hun organisatie.

    ![Schema en de retentie](./media/backup-azure-backup-sql/pg-schedule.png)

    In dit voorbeeld worden back-ups één keer per dag uitgevoerd om 12:00 uur en 8 uur (onderste gedeelte van het scherm)

    > [!NOTE]
    > Het is raadzaam om een aantal herstelpunten op korte termijn op schijf voor snel herstel. Deze herstelpunten worden gebruikt voor 'operationeel herstel'. Azure doet dienst als een goede externe locatie met hogere Sla's en beschikbaarheid gegarandeerd.
    >
    >

    **Best Practice**: Zorg ervoor dat Azure back-ups zijn gepland na de voltooiing van de lokale schijf-back-ups met behulp van DPM. Hierdoor is de laatste schijfback-ups moeten worden gekopieerd naar Azure.

13. Kies het bewaarschema voor beleid. De details over de werking van het bewaarbeleid zijn opgegeven op [gebruik Azure Backup vervangt u uw tape-infrastructuur-artikel](backup-azure-backup-cloud-as-tape.md).

    ![Bewaarbeleid](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    In dit voorbeeld:

    * Back-ups gaat één keer per dag om 12:00 uur en 8 uur (onderste gedeelte van het scherm) en 180 dagen worden bewaard.
    * De back-up op zaterdag om 12:00 uur 104 weken wordt bewaard
    * De back-up op de laatste zaterdag om 12:00 uur 60 maanden wordt bewaard
    * De back-up op de laatste zaterdag van maart om 12:00 uur tien jaar worden bewaard
14. Klik op **volgende** en selecteer de gewenste optie voor het overdragen van de eerste back-up naar Azure. U kunt ervoor kiezen **automatisch via het netwerk** of **Offline back-up**.

    * **Automatisch via het netwerk** brengt de back-upgegevens naar Azure aan de hand van de planning voor back-up is gekozen.
    * Hoe **Offline back-up** werkt wordt uitgelegd op [Offlineback-upwerkstroom in Azure Backup](backup-azure-backup-import-export.md).

    Kies de relevante gegevensoverdrachtmechanisme voor het verzenden van de eerste back-up naar Azure en klikt u op **volgende**.
15. Nadat u de details van het beleid in controleren de **samenvatting** scherm, klikt u op de **groep maken** knop om de werkstroom te voltooien. U kunt klikken op de **sluiten** knop en controleren van de voortgang van de taak in de werkruimte bewaking.

    ![Het maken van de beveiligingsgroep wordt uitgevoerd](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>On-demand back-up van een SQL Server-database
Tijdens de vorige stappen hebt gemaakt van een back-upbeleid, wordt een 'herstelpunt' alleen gemaakt als de eerste back-up plaatsvindt. In plaats van te wachten tot de scheduler geactiveerd, wordt de stappen hieronder trigger het maken van een herstel handmatig verwijzen.

1. Wacht totdat de status van de beveiliging groep bevat **OK** voor de database voordat u het herstelpunt maakt.

    ![Leden van beveiligingsgroep](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Met de rechtermuisknop op de database en selecteer **herstelpunt maken**.

    ![Online herstelpunt maken](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Kies **Onlinebeveiliging** in de vervolgkeuzelijst en klik op **OK**. Hiermee start u het maken van een herstelpunt in Azure.

    ![Herstelpunt maken](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. U ziet de voortgang van de taak in de **bewaking** werkruimte waar vindt u een actieve taak, zoals de naam weergegeven de volgende afbeelding.

    ![Bewaking van console](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Een SQL Server-database herstellen van Azure
De volgende stappen zijn vereist voor het herstellen van een beveiligde entiteit (SQL Server-database) van Azure.

1. Open de beheerconsole van de DPM-server. Navigeer naar **Recovery** werkruimte waar u de servers kunt zien back-up gemaakt door DPM. Blader door de vereiste database (in dit geval ReportServer$ MSDPM2012). Selecteer een **herstel vanaf** die eindigt op **Online**.

    ![Herstelpunt selecteren](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Met de rechtermuisknop op de naam van de database en klik op **herstellen**.

    ![Herstellen van Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. DPM geeft de details van het herstelpunt. Klik op **volgende**. Als u wilt overschrijven van de database, selecteert u het hersteltype **herstellen naar oorspronkelijk exemplaar van SQL Server**. Klik op **volgende**.

    ![Herstellen naar oorspronkelijke locatie](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    In dit voorbeeld kan DPM herstel van de database naar een ander exemplaar van SQL Server of naar een netwerkmap zelfstandige.
4. In de **herstelopties opgeven** scherm, kunt u de herstelopties zoals beperking van de netwerkbandbreedte te beperken van de bandbreedte die wordt gebruikt door herstel. Klik op **volgende**.
5. In de **samenvatting** scherm ziet u alle van de recovery-configuraties geleverd tot nu toe. Klik op **herstellen**.

    De herstelstatus ziet u de database wordt hersteld. U kunt klikken op **sluiten** aan de wizard te sluiten en de voortgang in de **bewaking** werkruimte.

    ![Herstelproces te starten](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Nadat het herstel is voltooid, wordt door de herstelde database toepassingsconsistent is.

### <a name="next-steps"></a>Volgende stappen
• [Veelgestelde vragen over azure Backup](backup-azure-backup-faq.md)
