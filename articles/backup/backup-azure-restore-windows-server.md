---
title: Herstellen van gegevens in Azure met een Windows Server of Windows-computer | Microsoft Docs
description: Informatie over het herstellen van gegevens die zijn opgeslagen in Azure met een Windows Server of Windows-computer.
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/4/2018
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 3444b13972ab9e5c435fc009e8ddb51bcafb1a41
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Met het Resource Manager implementatiemodel bestanden herstellen op een Windows-server of Windows-clientcomputer

In dit artikel wordt uitgelegd hoe u gegevens terugzetten vanuit een back-upkluis. Voor het herstellen van gegevens, kunt u de wizard herstellen in de Microsoft Azure Recovery Services (MARS)-agent gebruiken. Wanneer u gegevens herstelt, is het mogelijk om te:

* Gegevens terugzetten op dezelfde machine waaruit de back-ups zijn uitgevoerd.
* Gegevens herstellen naar een andere virtuele machine.

Microsoft uitgebracht in januari 2017 een Preview-update voor de MARS-agent. Samen met oplossingen voor problemen kan deze update direct herstellen, zodat u kunt het koppelen van een momentopname van een beschrijfbare herstelpunt als een volume herstel. U kunt vervolgens de herstelde volume en kopieer de bestanden naar een lokale computer, waardoor selectief terugzetten bestanden verkennen.

> [!NOTE]
> De [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is vereist als u wilt gebruiken direct herstellen om gegevens te herstellen. De back-upgegevens moeten ook worden beveiligd in kluizen in de landinstellingen die worden vermeld in het support-artikel. Raadpleeg de [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) voor de meest recente lijst van landinstellingen die ondersteuning bieden voor chatberichten te herstellen. Direct herstellen is **niet** momenteel in alle landen beschikbaar.
>

Direct herstellen met Recovery Services-kluizen in de Azure portal gebruiken. Als u gegevens in de Backup-kluizen hebt opgeslagen, zijn die geconverteerd naar een Recovery Services-kluizen. Als u wilt gebruiken direct herstellen, de MARS-update downloaden en de procedures volgen die vermeld direct herstellen.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Direct herstellen gebruiken om gegevens op de machine te herstellen

Als u per ongeluk een bestand verwijderd en u herstellen op de machine wilt (van waaruit de back-up is gemaakt), kunt de volgende stappen de gegevens te herstellen.

1. Open de **Microsoft Azure Backup** uitlijnen in. Als u niet waar de module is geïnstalleerd weet, zoekt u de computer of server voor **Microsoft Azure Backup**.

    De bureaublad-app moet worden weergegeven in de zoekresultaten.

2. Klik op **gegevens herstellen** om de wizard te starten.

    ![Gegevens herstellen](./media/backup-azure-restore-windows-server/recover.png)

3. Op de **aan de slag** deelvenster voor het terugzetten van de gegevens op dezelfde server of computer, selecteer **deze server (`<server name>`)** en klik op **volgende**.

    ![Selecteer deze serveroptie als de gegevens herstelt op de machine](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Op de **herstelmodus Selecteer** deelvenster kiezen **afzonderlijke bestanden en mappen** en klik vervolgens op **volgende**.

    ![Blader door bestanden](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. Op de **Volume selecteert en datum** deelvenster, selecteert u het volume met de bestanden en/of de mappen die u wilt herstellen.

    Selecteer een herstelpunt op de kalender. U kunt vanaf elk willekeurig herstelpunt herstellen in de tijd. Datums **vet** geven aan de beschikbaarheid van ten minste één herstelpunt. Wanneer u een datum selecteren als er meerdere herstelpunten beschikbaar zijn, kiest u de specifieke herstelpunt in de **tijd** vervolgkeuzelijst.

    ![Volume en datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Nadat u ervoor het herstelpunt gekozen hebt te herstellen, klikt u op **koppelen**.

    Azure Backup koppelt het lokale herstelpunt en gebruikt als een volume herstel.

7. Op de **bladeren en bestanden herstellen** deelvenster, klikt u op **Bladeren** open Windows Verkenner en de bestanden en mappen die u wilt zoeken.

    ![Opties voor herstel](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. Kopieer de bestanden en/of de mappen die u wilt herstellen en plak ze naar een locatie die lokaal op de server of de computer in Windows Verkenner. U kunt openen of de bestanden rechtstreeks van het volume herstel stream en controleer of dat de juiste versies zijn hersteld.

    ![Kopiëren en plakken van bestanden en mappen van het gekoppelde volume naar lokale locatie](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. Wanneer u klaar voor het herstellen van de bestanden en/of mappen bent, op de **bladeren en herstelbestanden** deelvenster, klikt u op **ontkoppelen**. Klik vervolgens op **Ja** om te bevestigen dat u wilt ontkoppelen van het volume.

    ![Ontkoppelen van het volume en bevestigen](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Als u niet ontkoppelen op, blijft het Volume Recovery gekoppelde gedurende 6 uur vanaf het moment waarop het is gekoppeld. De koppeltijd is echter uitgebreide tot aan maximaal 24 uur in het geval van een doorlopende kopiëren van bestanden. Er is geen back-upbewerkingen worden uitgevoerd terwijl het volume is gekoppeld. Een back-upbewerking die is gepland tijdens de tijd wanneer het volume is gekoppeld, wordt uitgevoerd nadat het herstel volume is ontkoppeld.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Direct herstellen gebruiken om gegevens te herstellen naar een andere computer
Als uw hele server verloren gegaan is, kunt u nog steeds gegevens herstellen vanaf Azure back-up naar een andere computer. De volgende stappen laten zien dat de werkstroom.


De terminologie die wordt gebruikt in deze stap omvat het volgende:

* *Bronmachine* : de oorspronkelijke computer waarop de back-up is gemaakt en die momenteel niet beschikbaar.
* *Doelcomputer* : de computer waarop de gegevens worden hersteld.
* *Voorbeeld kluis* – de Recovery Services-kluis waarnaar de *bronmachine* en *doelmachine* zijn geregistreerd. <br/>

> [!NOTE]
> Back-ups kunnen niet worden hersteld naar een doelcomputer met een eerdere versie van het besturingssysteem. Bijvoorbeeld, een back-up van een Windows 7 computer kan worden hersteld op een Windows 8 of hoger, computer. Een back-up van een computer met Windows 8 kan niet worden hersteld naar een computer met Windows 7.
>
>

1. Open de **Microsoft Azure Backup** uitlijnen in op de *doelmachine*.

2. Zorg ervoor dat de *doelmachine* en de *bronmachine* zijn geregistreerd bij dezelfde Recovery Services-kluis.

3. Klik op **gegevens herstellen** openen de **wizard gegevens herstellen**.

    ![Gegevens herstellen](./media/backup-azure-restore-windows-server/recover.png)

4. Op de **aan de slag** deelvenster **een andere server**

    ![Een andere Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Geef het kluisreferentiebestand die overeenkomt met de *voorbeeld kluis*, en klik op **volgende**.

    Als het kluisreferentiebestand is ongeldig (of verlopen), downloadt u een nieuw kluisreferentiebestand van de *voorbeeld kluis* in de Azure portal. Zodra u de referenties van een geldige kluis opgeeft, wordt de naam van de bijbehorende Backup-kluis weergegeven.


6. Op de **back-up-Server selecteren** deelvenster, selecteer de *bronmachine* uit de lijst met weergegeven machines en geef de wachtwoordzin. Klik op **Volgende**.

    ![Lijst met computers](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Op de **herstelmodus Selecteer** deelvenster Selecteer **afzonderlijke bestanden en mappen** en klik op **volgende**.

    ![Search](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Op de **Volume selecteert en datum** deelvenster, selecteert u het volume met de bestanden en/of de mappen die u wilt herstellen.

    Selecteer een herstelpunt op de kalender. U kunt vanaf elk willekeurig herstelpunt herstellen in de tijd. Datums **vet** geven aan de beschikbaarheid van ten minste één herstelpunt. Wanneer u een datum selecteren als er meerdere herstelpunten beschikbaar zijn, kiest u de specifieke herstelpunt in de **tijd** vervolgkeuzelijst.

    ![Zoeken naar objecten](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Klik op **koppelen** lokaal koppelen van het herstelpunt dat als een volume herstel aan uw *doelmachine*.

10. Op de **bladeren en bestanden herstellen** deelvenster, klikt u op **Bladeren** open Windows Verkenner en de bestanden en mappen die u wilt zoeken.

    ![Versleuteling](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. In Windows Verkenner, kopieert u de bestanden en/of mappen van het volume herstel en plak ze aan uw *doelmachine* locatie. U kunt openen of de bestanden rechtstreeks van het volume herstel stream en controleer of dat de juiste versies zijn hersteld.

    ![Versleuteling](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Wanneer u klaar voor het herstellen van de bestanden en/of mappen bent, op de **bladeren en herstelbestanden** deelvenster, klikt u op **ontkoppelen**. Klik vervolgens op **Ja** om te bevestigen dat u wilt ontkoppelen van het volume.

    ![Versleuteling](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Als u niet ontkoppelen op, blijft het Volume Recovery gekoppelde gedurende 6 uur vanaf het moment waarop het is gekoppeld. De koppeltijd is echter uitgebreide tot aan maximaal 24 uur in het geval van een doorlopende kopiëren van bestanden. Er is geen back-upbewerkingen worden uitgevoerd terwijl het volume is gekoppeld. Een back-upbewerking die is gepland tijdens de tijd wanneer het volume is gekoppeld, wordt uitgevoerd nadat het herstel volume is ontkoppeld.
    >

## <a name="troubleshooting"></a>Problemen oplossen
Als Azure Backup het herstel volume niet met succes koppelt zelfs na enkele minuten van het klikken op **koppelen** of koppelen van het volume herstel met een of meer fouten niet de volgende stappen om te beginnen met het herstellen van normaal.

1.  Het actieve koppelpunt annuleren als deze actief is geweest gedurende enkele minuten.

2.  Zorg ervoor dat u over de nieuwste versie van de Azure Backup agent. Voor de versie-informatie van Azure backup-agent, klikt u op **over Microsoft Azure Recovery Services Agent** op de **acties** deelvenster van de Microsoft Azure Backup console en zorg ervoor dat de **versie** getal gelijk is aan of hoger is dan de versie vermeld in [in dit artikel](https://go.microsoft.com/fwlink/?linkid=229525). U kunt de nieuwste versie van downloaden [hier](https://go.microsoft.com/fwLink/?LinkID=288905)

3.  Ga naar **Apparaatbeheer** -> **opslagcontrollers** en zorg ervoor dat u kunt vinden **Microsoft iSCSI-Initiator**. Als u deze vinden kunt, rechtstreeks gaat u naar stap 7 hieronder. 

4.  Als u kunt Microsoft iSCSI Initiator-service kan niet vinden, zoals vermeld in stap 3, controleert u als vindt u een item onder **Apparaatbeheer** -> **opslagcontrollers** aangeroepen **onbekend apparaat** met Hardware-ID **ROOT\ISCSIPRT**.

5.  Klik met de rechtermuisknop op **onbekend apparaat** en selecteer **Update stuurprogramma's**.

6.  Het stuurprogramma bijwerken door de optie voor **zoeken voor bijgewerkte stuurprogramma's automatisch**. Voltooiing van de update moet worden gewijzigd **onbekend apparaat** naar **Microsoft iSCSI-Initiator** zoals hieronder wordt weergegeven. 

    ![Versleuteling](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Ga naar **Taakbeheer** -> **Services (lokaal)** -> **Microsoft iSCSI Initiator-Service**. 

    ![Versleuteling](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Microsoft iSCSI Initiator-service opnieuw starten met de rechtermuisknop op de service, te klikken op **stoppen** en verdere rechtermuisknop opnieuw te klikken en te klikken op **Start**.

9.  Probeer herstellen met behulp van directe herstellen. 

Als het herstel nog steeds mislukt, start opnieuw op uw server /-client. Als opnieuw opstarten niet wenselijk is of het herstel nog steeds mislukt nadat de server opnieuw opstarten vanaf een andere virtuele Machine en neem contact op met ondersteuning van Azure door te gaan naar [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) en een ondersteuningsaanvraag indienen.

## <a name="next-steps"></a>Volgende stappen
* Nu dat u uw bestanden en mappen hebt hersteld, kunt u [beheren van uw back-ups](backup-azure-manage-windows-server.md).
