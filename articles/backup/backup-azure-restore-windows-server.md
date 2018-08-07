---
title: Gegevens in Azure te herstellen naar een Windows Server of Windows-computer
description: Informatie over het herstellen van gegevens die zijn opgeslagen in Azure op een Windows Server of Windows-computer.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 8/6/2018
ms.author: saurse
ms.openlocfilehash: ddde297de49edb5f6543d03dfdb972771533301b
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576182"
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Met het Resource Manager implementatiemodel bestanden herstellen op een Windows-server of Windows-clientcomputer

In dit artikel wordt uitgelegd hoe u gegevens terugzetten vanaf een back-upkluis. Voor het herstellen van gegevens, gebruikt u de wizard gegevens herstellen in de Microsoft Azure Recovery Services agent (MARS). Wanneer u gegevens herstelt, is het mogelijk om te:

* Gegevens herstellen naar dezelfde computer van waaruit de back-ups zijn uitgevoerd.
* Gegevens herstellen naar een alternatieve machine.

Microsoft uitgebracht in januari 2017 een Preview-update voor de MARS-agent. Samen met het oplossen van problemen met deze update kan direct herstellen, zodat u kunt een momentopname van een beschrijfbare recovery point als een herstelvolume koppelen. U kunt vervolgens de herstel-volume en kopieer de bestanden naar een lokale computer, waardoor selectief terugzetten van bestanden verkennen.

> [!NOTE]
> De [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is vereist als u wilt gebruiken direct herstellen om gegevens te herstellen. De back-upgegevens moeten ook worden beveiligd in kluizen in de landinstellingen die worden vermeld in het ondersteuningsartikel. Raadpleeg de [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) voor de meest recente lijst met landinstellingen die ondersteuning bieden voor direct terugzetten. Instant Restore is **niet** momenteel beschikbaar in alle landen.
>

Direct herstellen met Recovery Services-kluizen in Azure portal gebruiken. Als u gegevens in de back-upkluizen opgeslagen, hebben ze omgezet in Recovery Services-kluizen. Als u wilt gebruiken direct herstellen, de MARS-update downloaden en de procedures volgen die direct herstellen worden vermeld.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Instant Restore gebruiken om gegevens op dezelfde computer te herstellen

Als u per ongeluk een bestand verwijderd en u herstellen op dezelfde computer wilt (van waaruit de back-up is gemaakt), ziet de volgende stappen u de gegevens worden hersteld.

1. Open de **Microsoft Azure Backup** uitlijnen in. Als u niet waar de module is geïnstalleerd weet, zoekt u de computer of server voor **Microsoft Azure Backup**.

    De desktop-app moet worden weergegeven in de lijst met zoekresultaten.

2. Klik op **gegevens herstellen** om de wizard te starten.

    ![Gegevens herstellen](./media/backup-azure-restore-windows-server/recover.png)

3. Op de **aan de slag** in het deelvenster voor het terugzetten van de gegevens op dezelfde server of computer, selecteer **deze server (`<server name>`)** en klikt u op **volgende**.

    ![Kies deze serveroptie voor het terugzetten van de gegevens op de machine](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Op de **Selecteer herstelmodus** deelvenster Kies **afzonderlijke bestanden en mappen** en klik vervolgens op **volgende**.

    ![Door bestanden bladeren](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)
> [!IMPORTANT]
> De optie om terug te zetten *afzonderlijke bestanden en mappen* vereist .NET Framework 4.5.2 of hoger. Als u niet ziet de *afzonderlijke bestanden en mappen* optie, moet u .NET Framework upgraden naar versie 4.5.2 of hoger en probeer het opnieuw.

5. Op de **Selecteer Volume en datum** deelvenster, selecteert u het volume met de bestanden en/of mappen die u wilt herstellen.

    Selecteer een herstelpunt op de kalender. U kunt herstellen vanaf een herstelpunt in de tijd. Datums in **vet** geven van de beschikbaarheid van ten minste één herstelpunt. Wanneer u een datum selecteert als er meerdere herstelpunten beschikbaar zijn, kiest u het specifieke herstelpunt uit de **tijd** vervolgkeuzelijst.

    ![Volume en datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Nadat u ervoor het herstelpunt gekozen hebt te herstellen, klikt u op **koppelen**.

    Azure Backup koppelt u het lokale herstelpunt en gebruikt deze als een herstelvolume.

7. Op de **bladeren en bestanden herstellen** deelvenster, klikt u op **Bladeren** open Windows Verkenner en de bestanden en mappen die u wilt zoeken.

    ![Opties voor herstel](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. Kopieer de bestanden en/of mappen die u wilt herstellen en plak ze naar een locatie die lokaal op de server of de computer in Windows Verkenner. U kunt openen of de bestanden rechtstreeks vanuit het herstelvolume streamen en controleer of dat u de juiste versies herstelt.

    ![Bestanden en mappen van gekoppeld volume naar een lokale locatie kopiëren en plakken](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. Als u hebt de bestanden en/of mappen terugzetten op de **bladeren en herstelbestanden** deelvenster, klikt u op **ontkoppelen**. Klik vervolgens op **Ja** om te bevestigen dat u wilt ontkoppelen van het volume.

    ![Ontkoppel het volume en bevestigen](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Als u niet ontkoppelen, zijn het Herstelvolume blijven gekoppeld gedurende zes uur vanaf het moment waarop deze is gekoppeld. De tijd van het koppelpunt is echter uitgebreide van maximaal 24 uur in het geval van een doorlopende bestand kopiëren. Er is geen back-upbewerkingen worden uitgevoerd terwijl het volume is gekoppeld. Een back-upbewerking is gepland om te worden uitgevoerd tijdens de tijd waarop het volume is gekoppeld, wordt uitgevoerd nadat het herstelvolume is ontkoppeld.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Instant Restore gebruiken om gegevens te herstellen naar een alternatieve machine
Als uw hele server verloren gegaan is, kunt u nog steeds gegevens herstellen vanaf Azure back-up naar een andere computer. De volgende stappen ziet de werkstroom.


De termen die in deze stappen bevat:

* *Broncomputer* : de oorspronkelijke computer waarop de back-up is gemaakt en die momenteel niet beschikbaar.
* *Doelcomputer* – de machine waarop de gegevens worden hersteld.
* *Voorbeeld kluis* – de Recovery Services-kluis waarnaar de *bronmachine* en *doelmachine* zijn geregistreerd. <br/>

> [!NOTE]
> Back-ups kunnen niet worden hersteld naar een doel-VM met een eerdere versie van het besturingssysteem. Bijvoorbeeld, een back-up die is overgenomen uit een Windows 7 computer kan worden hersteld op een Windows 8 of hoger, computer. Een back-up van een computer met Windows 8 kan niet worden hersteld naar een computer met Windows 7.
>
>

1. Open de **Microsoft Azure Backup** uitlijnen in op de *doelmachine*.

2. Zorg ervoor dat de *doelmachine* en de *bronmachine* zijn geregistreerd bij dezelfde Recovery Services-kluis.

3. Klik op **gegevens herstellen** openen de **van de wizard gegevens herstellen**.

    ![Gegevens herstellen](./media/backup-azure-restore-windows-server/recover.png)

4. Op de **aan de slag** venster **een andere server**

    ![Een andere Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Geef het kluisreferentiebestand dat overeenkomt met de *voorbeeld kluis*, en klikt u op **volgende**.

    Als het kluisreferentiebestand is ongeldig (of verlopen), downloadt u een nieuw kluisreferentiebestand van de *voorbeeld kluis* in Azure portal. Wanneer u een geldige kluis referenties opgeeft, wordt de naam van de bijbehorende Backup-kluis wordt weergegeven.


6. Op de **back-up-Server selecteren** deelvenster, selecteer de *bronmachine* uit de lijst met computers weergegeven en de wachtwoordzin opgeven. Klik op **Volgende**.

    ![Lijst met computers](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Op de **Selecteer herstelmodus** deelvenster Selecteer **afzonderlijke bestanden en mappen** en klikt u op **volgende**.

    ![Search](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Op de **Selecteer Volume en datum** deelvenster, selecteert u het volume met de bestanden en/of mappen die u wilt herstellen.

    Selecteer een herstelpunt op de kalender. U kunt herstellen vanaf een herstelpunt in de tijd. Datums in **vet** geven van de beschikbaarheid van ten minste één herstelpunt. Wanneer u een datum selecteert als er meerdere herstelpunten beschikbaar zijn, kiest u het specifieke herstelpunt uit de **tijd** vervolgkeuzelijst.

    ![Zoekitems](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Klik op **koppelen** lokaal koppelen van het herstelpunt dat als een volume herstellen op uw *doelmachine*.

10. Op de **bladeren en bestanden herstellen** deelvenster, klikt u op **Bladeren** open Windows Verkenner en de bestanden en mappen die u wilt zoeken.

    ![Versleuteling](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. In Windows Verkenner, kopieert u de bestanden en/of mappen vanuit het herstelvolume en plak ze aan uw *doelmachine* locatie. U kunt openen of de bestanden rechtstreeks vanuit het herstelvolume streamen en controleer of dat de juiste versies worden hersteld.

    ![Versleuteling](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Als u hebt de bestanden en/of mappen terugzetten op de **bladeren en herstelbestanden** deelvenster, klikt u op **ontkoppelen**. Klik vervolgens op **Ja** om te bevestigen dat u wilt ontkoppelen van het volume.

    ![Versleuteling](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Als u niet ontkoppelen, zijn het Herstelvolume blijven gekoppeld gedurende zes uur vanaf het moment waarop deze is gekoppeld. De tijd van het koppelpunt is echter uitgebreide van maximaal 24 uur in het geval van een doorlopende bestand kopiëren. Er is geen back-upbewerkingen worden uitgevoerd terwijl het volume is gekoppeld. Een back-upbewerking is gepland om te worden uitgevoerd tijdens de tijd waarop het volume is gekoppeld, wordt uitgevoerd nadat het herstelvolume is ontkoppeld.
    >

## <a name="next-steps"></a>Volgende stappen
* Nu u uw bestanden en mappen hebt hersteld, kunt u [uw back-ups beheren](backup-azure-manage-windows-server.md).
