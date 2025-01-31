---
title: Back-up van een Exchange-server op Azure Backup met Azure Backup Server
description: Meer informatie over het back-up van een Exchange-server op Azure Backup met behulp van Azure Backup Server
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 03/24/2017
ms.author: kasinh
ms.openlocfilehash: 40541596b4da9e0590d497785afd7d6d7f4cbcb4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60641430"
---
# <a name="back-up-an-exchange-server-to-azure-with-azure-backup-server"></a>Back-up van een Exchange-server naar Azure met Azure Backup Server
In dit artikel wordt beschreven hoe u Microsoft Azure Backup-Server (MABS) naar de back-up van een Microsoft Exchange-server naar Azure te configureren.  

## <a name="prerequisites"></a>Vereisten
Voordat u doorgaat, zorg ervoor dat Azure Backup Server [geïnstalleerd en voorbereid](backup-azure-microsoft-azure-backup.md).

## <a name="mabs-protection-agent"></a>MABS-beveiligingsagent
Voor het installeren van de MAB-beveiligingsagent op de Exchange server, de volgende stappen uit:

1. Zorg ervoor dat de firewalls correct zijn geconfigureerd. Zie [firewalluitzonderingen voor de agent configureren](https://technet.microsoft.com/library/Hh758204.aspx).
2. Installeer de agent op de Exchange-server door te klikken op **Management > Agents > installeren** in MABS Administrator-Console. Zie [Installeer de beveiligingsagent MABS](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) voor gedetailleerde stappen.

## <a name="create-a-protection-group-for-the-exchange-server"></a>Maak een beveiligingsgroep voor de Exchange-server
1. Klik in de MABS Administrator-Console op **Protection**, en klik vervolgens op **nieuw** op het lint te openen de **nieuwe beveiligingsgroep maken** wizard.
2. Op de **Welkom** scherm van de wizard klikt u op **volgende**.
3. Op de **type beveiligingsgroep selecteren** scherm, schakel **Servers** en klikt u op **volgende**.
4. Selecteer de Exchange server-database die u wilt beveiligen en klikt u op **volgende**.

   > [!NOTE]
   > Als u Exchange 2013 beveiligt, controleert u de [Exchange 2013-vereisten](https://technet.microsoft.com/library/dn751029.aspx).
   >
   >

    In het volgende voorbeeld wordt is het Exchange 2010-database geselecteerd.

    ![Groepsleden selecteren](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Selecteer de methode voor gegevensbeveiliging.

    Naam van de beveiligingsgroep en selecteer vervolgens beide van de volgende opties:

   * Ik wil kortetermijnbeveiliging met schijf.
   * Ik kies voor online beveiliging.
6. Klik op **volgende**.
7. Selecteer de **Eseutil uitvoeren om gegevensintegriteit te controleren** optie als u wilt controleren de integriteit van de Exchange Server-databases.

    Nadat u deze optie selecteert, consistentiecontroles back-up wordt uitgevoerd op MABS om te voorkomen dat het i/o-verkeer dat wordt gegenereerd door het uitvoeren van de **eseutil** opdracht op de Exchange-server.

   > [!NOTE]
   > Als u wilt deze optie gebruikt, moet u de bestanden Ese.dll en Eseutil.exe kopiëren naar de map C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin op de MAB-server. Anders wordt de volgende fout geactiveerd:  
   > ![Eseutil-fout](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Klik op **volgende**.
9. Selecteer de database voor **kopieback-up**, en klik vervolgens op **volgende**.

   > [!NOTE]
   > Als u 'Volledige back-up' voor ten minste één DAG kopie van de database niet selecteert, wordt de logboeken worden niet afgekapt.
   >
   >
10. Configureren van de doelstellingen voor **kortetermijnback-up**, en klik vervolgens op **volgende**.
11. Bekijk de beschikbare schijfruimte en klik vervolgens op **volgende**.
12. Selecteer de tijd waarbinnen de MAB-Server maken van de initiële replicatie en klik vervolgens op **volgende**.
13. De opties voor consistentiecontrole selecteren en klik vervolgens op **volgende**.
14. Kies de database die u wilt back-up naar Azure, en klik vervolgens op **volgende**. Bijvoorbeeld:

    ![Gegevens voor online beveiliging opgeven](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Geef het schema voor **Azure Backup**, en klik vervolgens op **volgende**. Bijvoorbeeld:

    ![Onlineback-upschema opgeven](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Houd er rekening mee Online herstelpunten zijn afhankelijk van de express full-herstelpunten. Daarom moet u het online herstelpunt plannen nadat de tijd die opgegeven voor de snelle volledige herstelpunt.
    >
    >
16. Configureren van het bewaarbeleid voor **Azure Backup**, en klik vervolgens op **volgende**.
17. Kies een replicatieoptie online en klikt u op **volgende**.

    Hebt u een grote database, kan het lang duren voor de eerste back-up moet worden gemaakt via het netwerk. U kunt dit probleem, kunt u een offline back-up maken.  

    ![Onlinebewaarbeleid opgeven](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Bevestig de instellingen en klik vervolgens op **groep maken**.
19. Klik op **Sluiten**.

## <a name="recover-the-exchange-database"></a>De Exchange-database herstellen
1. Als u wilt herstellen van een Exchange-database, klikt u op **Recovery** in de MABS Administrator-Console.
2. Zoek de Exchange-database die u wilt herstellen.
3. Selecteer een online herstelpunt uit de *hersteltijd* vervolgkeuzelijst.
4. Klik op **herstellen** om te beginnen de **Herstelwizard**.

Er zijn vijf typen voor herstel voor online herstelpunten:

* **Herstellen naar oorspronkelijke Exchange Server-locatie:** De gegevens worden hersteld naar de oorspronkelijke Exchange server.
* **Herstellen naar een andere database op een Exchange-Server:** De gegevens worden hersteld naar een andere database op een andere Exchange-server.
* **Herstellen naar een hersteldatabase:** De gegevens worden hersteld naar een Exchange Recovery-Database (RDB).
* **Naar een netwerkmap kopiëren:** De gegevens worden hersteld naar een netwerkmap.
* **Naar tape kopiëren:** Als u een tapewisselaar of een zelfstandig tapestation gekoppeld en geconfigureerd op MABS hebt, wordt het herstelpunt worden gekopieerd naar een vrije tape.

    ![Kies onlinereplicatie](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure Backup](backup-azure-backup-faq.md)
