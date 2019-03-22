---
title: Gegevens in Azure te herstellen naar een Windows server of Windows-computer
description: Informatie over het herstellen van gegevens die zijn opgeslagen in Azure met een Windows server of Windows-computer.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 9/7/2018
ms.author: saurse
ms.openlocfilehash: d58b51f06c21c787e4aa720c803ab6533544d55c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58110814"
---
# <a name="restore-files-to-windows-by-using-the-azure-resource-manager-deployment-model"></a>Bestanden herstellen naar Windows met behulp van het implementatiemodel Azure Resource Manager

In dit artikel wordt uitgelegd hoe u gegevens terugzetten vanaf een back-upkluis. Voor het herstellen van gegevens, gebruikt u de wizard gegevens herstellen in de Microsoft Azure Recovery Services Agent (MARS). U kunt:

* Gegevens herstellen naar dezelfde computer van waaruit de back-ups zijn uitgevoerd.
* Gegevens herstellen naar een alternatieve machine

Gebruik de functie voor direct terugzetten om te koppelen van een beschrijfbare herstelmomentopname als een volume herstellen. U kunt de recovery volume en kopieer de bestanden vervolgens verkennen met een lokale computer, waardoor selectief terugzetten van bestanden.

> [!NOTE]
> De [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is vereist als u wilt gebruiken direct herstellen om gegevens te herstellen. De back-upgegevens moeten ook worden beveiligd in kluizen in de landinstellingen die worden vermeld in het ondersteuningsartikel. Raadpleeg de [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) voor de meest recente lijst met landinstellingen die ondersteuning bieden voor direct terugzetten.
>

Direct herstellen met Recovery Services-kluizen in Azure portal gebruiken. Als u gegevens in de back-upkluizen opgeslagen, hebben ze omgezet in Recovery Services-kluizen. Als u wilt gebruiken direct herstellen, de MARS-update downloaden en de procedures volgen die direct herstellen worden vermeld.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Instant Restore gebruiken om gegevens op dezelfde computer te herstellen

Als u per ongeluk een bestand verwijderd en u herstellen op dezelfde computer wilt (van waaruit de back-up is gemaakt), ziet de volgende stappen u de gegevens worden hersteld.

1. Open de **Microsoft Azure Backup**-module. Als u niet waar dat de module is geïnstalleerd weet, zoekt u de computer of server voor **Microsoft Azure Backup**.

    De desktop-app moet worden weergegeven in de lijst met zoekresultaten.

2. Selecteer **gegevens herstellen** om de wizard te starten.

    ![Schermopname van Azure-up en herstellen van gegevens die zijn gemarkeerd](./media/backup-azure-restore-windows-server/recover.png)

3. Op de **aan de slag** pagina, de gegevens te herstellen naar dezelfde server of computer, selecteer **deze server (`<server name>`)** > **volgende**.

    ![Schermafbeelding van herstellen gegevens Wizard pagina aan de slag](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Op de **Selecteer herstelmodus** pagina, kies **afzonderlijke bestanden en mappen** > **volgende**.

    ![Schermafbeelding van herstellen gegevens Wizard Selecteer herstelmodus pagina](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)
   > [!IMPORTANT]
   > De optie voor het herstellen van afzonderlijke bestanden en mappen vereist .NET Framework 4.5.2 of hoger. Als u niet ziet de **afzonderlijke bestanden en mappen** optie, moet u .NET Framework upgraden naar versie 4.5.2 of hoger, en probeer het opnieuw.

   > [!TIP]
   > De **afzonderlijke bestanden en mappen** optie kunnen voor snelle toegang tot de herstelpuntgegevens. Het is geschikt voor het herstellen van afzonderlijke bestanden met grootten niet meer dan 80 GB een, en biedt overdracht of kopie versnelt maximaal 6 MBps tijdens het herstel. De **Volume** optie worden alle back-ups van gegevens in een bepaald volume hersteld. Deze optie biedt snellere overdracht snelheden (tot maximaal 60 MBps) die is ideaal voor het herstellen van gegevens grote of het hele volumes.

5. Op de **Selecteer Volume en datum** pagina, selecteert u het volume met de bestanden en mappen die u wilt herstellen.

    Selecteer een herstelpunt op de kalender. Datums in **vet** geven van de beschikbaarheid van ten minste één herstelpunt. Als meerdere herstelpunten beschikbaar binnen een enkele datum zijn, kiest u het specifieke herstelpunt uit de **tijd** vervolgkeuzelijst.

    ![Schermafbeelding van herstellen gegevens Wizard selecteert u Volume en datum-pagina](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Nadat u hebt gekozen het herstelpunt te herstellen, selecteert u **koppelen**.

    Azure Backup koppelt u het lokale herstelpunt en gebruikt deze als een herstelvolume.

7. Op de **bladeren en bestanden herstellen** weergeeft, schakelt **Bladeren** open Windows Verkenner en de bestanden en mappen die u wilt zoeken.

    ![Schermafbeelding van herstellen gegevens Wizard bladeren en bestanden herstellen](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. In Windows Verkenner, Kopieer de bestanden en mappen die u wilt herstellen en plak ze op elke locatie die lokaal op de server of de computer. U kunt openen of de bestanden rechtstreeks vanuit het herstelvolume streamen en controleer of dat u de juiste versies herstelt.

    ![Schermopname van Windows Explorer, met behulp van kopie gemarkeerd](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)


9. Wanneer u bent klaar, op de **bladeren en bestanden herstellen** weergeeft, schakelt **ontkoppelen**. Selecteer vervolgens **Ja** om te bevestigen dat u wilt ontkoppelen van het volume.

    ![Schermafbeelding van herstellen gegevens Wizard bladeren en bestanden herstellen](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Als u niet selecteert **ontkoppelen**, het herstelvolume blijven gekoppeld gedurende zes uur vanaf het moment waarop deze is gekoppeld. De tijd van het koppelpunt wordt echter uitgebreid tot een maximum van 24 uur in het geval van een doorlopende bestandskopie. Er is geen back-upbewerkingen worden uitgevoerd terwijl het volume is gekoppeld. Een back-upbewerking is gepland om te worden uitgevoerd tijdens de tijd waarop het volume is gekoppeld wordt uitgevoerd nadat het herstelvolume ontkoppeld is.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Instant Restore gebruiken om gegevens te herstellen naar een alternatieve machine
Als uw hele server verloren gegaan is, kunt u nog steeds gegevens herstellen vanaf Azure back-up naar een andere computer. De volgende stappen ziet de werkstroom.


Deze stappen omvatten de volgende terminologie:

* *Broncomputer* : de oorspronkelijke virtuele machine van de back-up is gemaakt en dit is momenteel niet beschikbaar.
* *Doelcomputer* – de machine waarop de gegevens worden hersteld.
* *Voorbeeld kluis* – de Recovery Services-kluis waarop de bronmachine en de doelcomputer zijn geregistreerd. <br/>

> [!NOTE]
> Back-ups kunnen niet worden hersteld naar een doel-VM die een eerdere versie van het besturingssysteem wordt uitgevoerd. Bijvoorbeeld, kan een back-up van een computer met Windows 7 worden hersteld op een Windows 8 (of hoger). Een back-up van een computer met Windows 8 kan niet worden hersteld naar een computer met Windows 7.
>
>

1. Open de **Microsoft Azure Backup** -module op de doelcomputer.

2. Zorg ervoor dat de doelcomputer en de broncomputer zijn geregistreerd bij dezelfde Recovery Services-kluis.

3. Selecteer **gegevens herstellen** openen de **Wizard gegevens herstellen**.

    ![Schermopname van Azure-up en herstellen van gegevens die zijn gemarkeerd](./media/backup-azure-restore-windows-server/recover.png)

4. Op de **aan de slag** weergeeft, schakelt **een andere server**.

    ![Schermafbeelding van herstellen gegevens Wizard pagina aan de slag](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Geef het kluisreferentiebestand dat overeenkomt met de voorbeeld-kluis en selecteer **volgende**.

    Als het kluisreferentiebestand is ongeldig (of verlopen), moet u een nieuw kluisreferentiebestand downloaden uit de voorbeeld-kluis in Azure portal. Nadat u een geldige kluis-referentie opgegeven, wordt de naam van de bijbehorende back-upkluis weergegeven.


6. Op de **back-up-Server selecteren** pagina, selecteert u de bron-VM uit de lijst met computers weergegeven en de wachtwoordzin opgeven. Selecteer vervolgens **Volgende**.

    ![Schermafbeelding van herstellen gegevens Wizard Back-up pagina Server selecteren](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Op de **Selecteer herstelmodus** pagina, selecteert u **afzonderlijke bestanden en mappen** > **volgende**.

    ![Schermafbeelding van herstellen gegevens Wizard Selecteer herstelmodus pagina](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Op de **Selecteer Volume en datum** pagina, selecteert u het volume met de bestanden en mappen die u wilt herstellen.

    Selecteer een herstelpunt op de kalender. Datums in **vet** geven van de beschikbaarheid van ten minste één herstelpunt. Als meerdere herstelpunten beschikbaar binnen een enkele datum zijn, kiest u het specifieke herstelpunt uit de **tijd** vervolgkeuzelijst.

    ![Schermafbeelding van herstellen gegevens Wizard selecteert u Volume en datum-pagina](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Selecteer **koppelen** lokaal koppelen van het herstelpunt dat als een volume herstellen op de doelcomputer.

10. Op de **bestanden bladeren en herstellen** weergeeft, schakelt **Bladeren** open Windows Verkenner en de bestanden en mappen die u wilt zoeken.

    ![Schermafbeelding van herstellen Wizard bladeren en herstellen van gegevensbestanden pagina](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. In Windows Verkenner de bestanden en mappen te kopiëren vanuit het herstelvolume en plak ze op de doellocatie voor de machine. U kunt openen of de bestanden rechtstreeks vanuit het herstelvolume streamen en controleer of dat de juiste versies worden hersteld.

    ![Schermopname van Windows Explorer, met behulp van kopie gemarkeerd](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Wanneer u bent klaar, op de **bladeren en bestanden herstellen** weergeeft, schakelt **ontkoppelen**. Selecteer vervolgens **Ja** om te bevestigen dat u wilt ontkoppelen van het volume.

    ![Schermafbeelding van herstellen Wizard bladeren en herstellen van gegevensbestanden pagina](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Als u niet selecteert **ontkoppelen**, het herstelvolume blijven gekoppeld gedurende zes uur vanaf het moment waarop deze is gekoppeld. De tijd van het koppelpunt wordt echter uitgebreid tot een maximum van 24 uur in het geval van een doorlopende bestandskopie. Er is geen back-upbewerkingen worden uitgevoerd terwijl het volume is gekoppeld. Een back-upbewerking is gepland om te worden uitgevoerd tijdens de tijd waarop het volume is gekoppeld wordt uitgevoerd nadat het herstelvolume ontkoppeld is.
    >

## <a name="next-steps"></a>Volgende stappen
Nu u uw bestanden en mappen hebt hersteld, kunt u [uw back-ups beheren](backup-azure-manage-windows-server.md).
