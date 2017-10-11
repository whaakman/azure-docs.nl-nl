---
title: Maak een back-up van een Exchange-server op Azure Backup met Azure Backup-Server | Microsoft Docs
description: Meer informatie over het back-up van een Exchange-server op Azure Backup met behulp van Azure Backup-Server
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: e46557e8-2eaf-4ee0-99ea-00fbb8687dca
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 60b784fd00013c2b9504f8635c6b5c4c592563be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-azure-backup-server"></a>Back-up van een Exchange-server op Azure Backup met Azure Backup-Server
In dit artikel wordt beschreven hoe u van Microsoft Azure Backup Server (MABS) voor back-up van een Microsoft Exchange-server naar Azure.  

## <a name="prerequisites"></a>Vereisten
Voordat u doorgaat, zorg ervoor dat Azure Backup-Server [geïnstalleerd en voorbereid](backup-azure-microsoft-azure-backup.md).

## <a name="mabs-protection-agent"></a>De beveiligingsagent MABS
Als u wilt de MABS-beveiligingsagent installeren op de Exchange server, de volgende stappen uit:

1. Zorg ervoor dat de firewall correct zijn geconfigureerd. Zie [firewall-uitzonderingen voor de agent configureren](https://technet.microsoft.com/library/Hh758204.aspx).
2. Installeer de agent op de Exchange-server door te klikken op **Management > Agents > installeren** MABS Administrator-console. Zie [Installeer de beveiligingsagent MABS](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) voor gedetailleerde stappen.

## <a name="create-a-protection-group-for-the-exchange-server"></a>Maken van een beveiligingsgroep voor de Exchange-server
1. Klik in de MABS Administrator-Console op **beveiliging**, en klik vervolgens op **nieuw** op het lint openen de **nieuwe beveiligingsgroep maken** wizard.
2. Op de **Welkom** scherm van de wizard Klik **volgende**.
3. Op de **type beveiligingsgroep selecteren** scherm, selecteert u **Servers** en klik op **volgende**.
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
   * Ik wil online beveiliging.
6. Klik op **Volgende**.
7. Selecteer de **Eseutil uitvoeren om gegevensintegriteit te controleren** optie als u wilt controleren de integriteit van de Exchange Server-databases.

    Nadat u deze optie selecteert, back-consistentiecontrole wordt uitgevoerd op een MABS om te voorkomen dat het i/o-verkeer dat wordt gegenereerd door het uitvoeren van de **eseutil** opdracht op de Exchange server.

   > [!NOTE]
   > Om deze optie gebruikt, moet u de bestanden Ese.dll en Eseutil.exe kopiëren naar de map C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin op de server mal. Anders wordt is de volgende fout geactiveerd:  
   > ![Eseutil-fout](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. Klik op **Volgende**.
9. Selecteer de database voor **kopieback-up**, en klik vervolgens op **volgende**.

   > [!NOTE]
   > Als u 'Volledige back-up' voor ten minste één DAG kopie van een database niet selecteert, wordt de logboeken worden niet afgekapt.
   >
   >
10. Configureren van de doelstellingen voor **kortetermijnback-up**, en klik vervolgens op **volgende**.
11. Bekijk de beschikbare schijfruimte en klik vervolgens op **volgende**.
12. Selecteer de tijd waarop de Server mal maken van de initiële replicatie en klik vervolgens op **volgende**.
13. De opties voor consistentiecontrole selecteren en klik vervolgens op **volgende**.
14. Kies de database die u wilt back-up naar Azure, en klik vervolgens op **volgende**. Bijvoorbeeld:

    ![Gegevens voor online beveiliging opgeven](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. Definieer de planning voor **Azure Backup**, en klik vervolgens op **volgende**. Bijvoorbeeld:

    ![Onlineback-upschema opgeven](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Houd er rekening mee Online herstelpunten zijn gebaseerd op snelle volledige herstelpunten. Daarom moet u het online herstelpunt plannen nadat de tijd die opgegeven voor de snelle volledige van het herstelpunt.
    >
    >
16. Configureer het bewaarbeleid voor **Azure Backup**, en klik vervolgens op **volgende**.
17. Kies een replicatieoptie online en klikt u op **volgende**.

    Als er een grote database, kan het lang duren voor de eerste back-up worden gemaakt via het netwerk. Om dit te voorkomen, kunt u een offline back-up maken.  

    ![Onlinebewaarbeleid opgeven](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Bevestig de instellingen en klik vervolgens op **groep maken**.
19. Klik op **Sluiten**.

## <a name="recover-the-exchange-database"></a>De Exchange-database herstellen
1. Als u wilt herstellen op een Exchange-database, klikt u op **herstel** in de MABS Administrator-Console.
2. Zoek de Exchange-database die u wilt herstellen.
3. Selecteer een online herstelpunt uit de *hersteltijd* vervolgkeuzelijst.
4. Klik op **herstellen** starten de **Herstelwizard**.

Er zijn vijf herstel-typen voor online herstelpunten:

* **Herstellen naar oorspronkelijke Exchange Server-locatie:** de gegevens worden hersteld naar de oorspronkelijke Exchange server.
* **Herstellen naar een andere database op een Exchange-Server:** de gegevens worden hersteld naar een andere database op een andere Exchange-server.
* **Herstellen naar hersteldatabase:** de gegevens worden hersteld naar een Exchange Recovery Database (RDB).
* **Kopiëren naar een netwerkmap:** de gegevens worden hersteld naar een netwerkmap.
* **Kopiëren naar tape:** als u hebt een tapewisselaar of zelfstandig tapestation gekoppeld en geconfigureerd op MABS, het herstelpunt wordt gekopieerd naar een vrije tape.

    ![Kies onlinereplicatie](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure Backup](backup-azure-backup-faq.md)
