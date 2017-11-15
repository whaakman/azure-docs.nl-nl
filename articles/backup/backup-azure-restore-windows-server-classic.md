---
title: Gegevens herstellen naar een Windows Server of Windows-Client van Azure met het klassieke implementatiemodel | Microsoft Docs
description: Informatie over het herstellen van een Windows Server of Windows-Client.
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: cb08d5c80954e1c231bf6e8b781b09700ef0959d
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-the-classic-deployment-model"></a>Met het klassieke implementatiemodel bestanden herstellen op een Windows-server of Windows-clientcomputer
> [!div class="op_single_selector"]
> * [Klassieke portal](backup-azure-restore-windows-server-classic.md)
> * [Azure Portal](backup-azure-restore-windows-server.md)
>
>

Dit artikel wordt uitgelegd hoe u kunt gegevens herstellen vanaf een back-upkluis en een server of de computer herstellen. U start in maart 2017, kunt u niet langer maken back-upkluizen in de klassieke portal.

> [!IMPORTANT]
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Na **30 November 2017**, u zich niet PowerShell gebruiken voor het maken van de Backup-kluizen. <br/> **Starten van 30 November 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
>

Voor het herstellen van gegevens, kunt u de wizard herstellen in de Microsoft Azure Recovery Services (MARS)-agent gebruiken. Wanneer u gegevens herstelt, is het mogelijk om te:

* Gegevens terugzetten op dezelfde machine waaruit de back-ups zijn uitgevoerd.
* Gegevens herstellen naar een andere virtuele machine.

Microsoft uitgebracht in januari 2017 een Preview-update voor de MARS-agent. Samen met oplossingen voor problemen kan deze update direct herstellen, zodat u kunt het koppelen van een momentopname van een beschrijfbare herstelpunt als een volume herstel. U kunt vervolgens de herstelde volume en kopieer de bestanden naar een lokale computer, waardoor selectief terugzetten bestanden verkennen.

> [!NOTE]
> De [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is vereist als u wilt gebruiken direct herstellen om gegevens te herstellen. De back-upgegevens moeten ook worden beveiligd in kluizen in de landinstellingen die worden vermeld in het support-artikel. Raadpleeg de [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) voor de meest recente lijst van landinstellingen die ondersteuning bieden voor chatberichten te herstellen. Direct herstellen is **niet** momenteel in alle landen beschikbaar.
>

Chatberichten terugzetten is beschikbaar voor gebruik in de Recovery Services-kluizen in de Azure-portal en de Backup-kluizen in de klassieke portal. Als u wilt gebruiken direct herstellen, de MARS-update downloaden en de procedures volgen die vermeld direct herstellen.


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
    > Als u niet ontkoppelen op, blijft het Volume Recovery gekoppelde gedurende zes uur van de tijd wanneer deze is gekoppeld. Er is geen back-upbewerkingen worden uitgevoerd terwijl het volume is gekoppeld. Een back-upbewerking die is gepland tijdens de tijd wanneer het volume is gekoppeld, wordt uitgevoerd nadat het herstel volume is ontkoppeld.
    >


## <a name="recover-data-to-the-same-machine"></a>Gegevens op de machine herstellen
Als u per ongeluk een bestand verwijderd en u herstellen op de machine wilt (van waaruit de back-up is gemaakt), kunt de volgende stappen de gegevens te herstellen.

1. Open de **Microsoft Azure Backup** uitlijnen in.
2. Klik op **gegevens herstellen** starten van de werkstroom.

    ![Gegevens herstellen](./media/backup-azure-restore-windows-server-classic/recover.png)
3. Selecteer de  **deze server (*yourmachinename*) ** optie voor het herstellen van de back-upbestand op dezelfde machine.

    ![Dezelfde machine](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. Kies voor **zoeken naar bestanden** of **zoeken naar bestanden**.

    Als u van plan bent een of meer bestanden waarvan het pad is bekend zijn terug te zetten, laat u de standaardoptie. Als u niet zeker van de mapstructuur bent, maar u wilt zoeken naar een bestand, kies de **zoeken naar bestanden** optie. In deze sectie gaat er nu verder met de standaardoptie.

    ![Blader door bestanden](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. Selecteer het volume van waaruit u wilt terugzetten van het bestand.

    U kunt herstellen vanaf elk punt in tijd. Datums die worden weergegeven in **vet** geven aan de beschikbaarheid van een herstelpunt in de kalender. Nadat een datum is geselecteerd, op basis van uw back-upschema (en het succes van een back-upbewerking), kunt u een punt in tijd van de **tijd** vervolgkeuzelijst.

    ![Volume en datum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. Selecteer de items te herstellen. U kunt meerdere mappen/bestanden die u wilt herstellen.

    ![Bestanden selecteren](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. Geef de parameters van het herstel.

    ![Opties voor herstel](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * U hebt een optie herstellen naar de oorspronkelijke locatie (waarin de bestandsmap zou worden overschreven) of naar een andere locatie in dezelfde machine.
   * Als het bestand of de map die u wilt herstellen in de doellocatie bestaat, kunt u kopieën (twee versies van hetzelfde bestand) maken, overschrijven de bestanden in de doellocatie of het herstel van de bestanden die aanwezig zijn in de doel-overslaan.
   * Het is raadzaam dat u laat de standaardoptie voor het herstellen van de ACL's voor de bestanden die zijn hersteld.
8. Zodra deze invoer zijn opgegeven, klikt u op **volgende**. De herstelwerkstroom, waarbij de bestanden in deze computer wordt hersteld, wordt gestart.

## <a name="recover-to-an-alternate-machine"></a>Herstellen naar een andere virtuele machine
Als uw hele server verloren gegaan is, kunt u nog steeds gegevens herstellen vanaf Azure back-up naar een andere computer. De volgende stappen laten zien dat de werkstroom.  

De terminologie die wordt gebruikt in deze stap omvat het volgende:

* *Bronmachine* : de oorspronkelijke computer waarop de back-up is gemaakt en die momenteel niet beschikbaar.
* *Doelcomputer* : de computer waarop de gegevens worden hersteld.
* *Voorbeeld kluis* – de Backup-kluis waarnaar de *bronmachine* en *doelmachine* zijn geregistreerd. <br/>

> [!NOTE]
> Back-ups die van een virtuele machine kunnen niet worden teruggezet op een computer waarop een eerdere versie van het besturingssysteem wordt uitgevoerd. Bijvoorbeeld, als back-ups worden gemaakt van een machine met Windows 7, kan deze worden hersteld op een Windows 8 of hoger machine. Echter, de omgekeerd niet bijhoudt true.
>
>

1. Open de **Microsoft Azure Backup** uitlijnen in op de *doelmachine*.
2. Zorg ervoor dat de *doelmachine* en de *bronmachine* zijn geregistreerd bij de dezelfde back-upkluis.
3. Klik op **gegevens herstellen** starten van de werkstroom.

    ![Gegevens herstellen](./media/backup-azure-restore-windows-server-classic/recover.png)
4. Selecteer **een andere server**

    ![Een andere Server](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. Geef het kluisreferentiebestand die overeenkomt met de *voorbeeld kluis*. Als het kluisreferentiebestand is ongeldig (of verlopen) download een nieuw kluisreferentiebestand van de *voorbeeld kluis* in de klassieke Azure portal. Zodra het kluisreferentiebestand is opgegeven, wordt de back-upkluis tegen het kluisreferentiebestand weergegeven.
6. Selecteer de *bronmachine* uit de lijst met computers weergegeven.

    ![Lijst met computers](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. Selecteer de **zoeken naar bestanden** of **zoeken naar bestanden** optie. In deze sectie gebruiken we de **zoeken naar bestanden** optie.

    ![Search](./media/backup-azure-restore-windows-server-classic/search.png)
8. Selecteer het volume en de datum in het volgende scherm. Zoeken naar de map/bestand-naam die u wilt herstellen.

    ![Zoeken naar objecten](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. Selecteer de locatie waar de bestanden moeten worden hersteld.

    ![Locatie herstellen](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. Geef de wachtwoordzin voor versleuteling dat is opgegeven tijdens de *van de bronmachine* registratie *voorbeeld kluis*.

    ![Versleuteling](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. Zodra de invoer is opgegeven, klikt u op **herstellen**, dat het herstellen van de back-upbestanden naar de bestemming opgegeven activeert.

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
    > Als u niet ontkoppelen op, blijft het Volume Recovery gekoppelde gedurende zes uur van de tijd wanneer deze is gekoppeld. Er is geen back-upbewerkingen worden uitgevoerd terwijl het volume is gekoppeld. Een back-upbewerking die is gepland tijdens de tijd wanneer het volume is gekoppeld, wordt uitgevoerd nadat het herstel volume is ontkoppeld.
    >


## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure Backup](backup-azure-backup-faq.md)
* Ga naar de [Azure back-Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933).

## <a name="learn-more"></a>Meer informatie
* [Overzicht van Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [Back-virtuele machines in Azure](backup-azure-vms-introduction.md)
* [Back-up van de werkbelasting van Microsoft](backup-azure-dpm-introduction.md)
