---
title: 'Azure back-up: Systeemstatus herstellen naar een WindowsServer'
description: Voor stap door stap uitleg voor Windows Server-systeemstatus herstellen vanuit een back-up in Azure.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 8/18/2017
ms.author: saurse
ms.openlocfilehash: 31784c44d7561c690278aa4764e98992fbca73ae
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51230781"
---
# <a name="restore-system-state-to-windows-server"></a>De systeemstatus herstellen naar WindowsServer

In dit artikel wordt uitgelegd hoe u Windows Server System State back-ups herstellen vanuit een Azure Recovery Services-kluis. Als u de systeemstatus herstellen, moet u een systeemstatusback-up (gemaakt met behulp van de instructies in [maakt u een Back-up van systeemstatus](backup-azure-system-state.md#back-up-windows-server-system-state), en zorg ervoor dat u hebt geïnstalleerd het [meest recente versie van de Microsoft Azure Recovery Services (MARS) Agent](https://aka.ms/azurebackup_agent). Systeemstatus van Windows Server-gegevens herstellen vanaf een Azure Recovery Services-kluis is een proces in twee stappen:

1. Systeemstatus herstellen als bestanden vanuit Azure Backup. Bij het herstellen van de systeemstatus als bestanden vanuit Azure Backup, kunt u deze:
  * Systeemstatus herstellen op dezelfde server waar de back-ups zijn uitgevoerd, of
  * Bestand-systeemstatus herstellen naar een andere server.

2. De herstelde bestanden van de systeemstatus van toepassing op een Windows-Server.


## <a name="recover-system-state-files-to-the-same-server"></a>Systeemstatusbestanden herstellen op dezelfde server
De volgende stappen wordt uitgelegd hoe u uw Windows Server-configuratie terugdraaien naar een eerdere status. Uw server-configuratie terugdraaien naar een bekende, stabiele status, kan zeer waardevol zijn. De volgende stappen uit herstellen van de server-systeemstatus van een Recovery Services-kluis. 

1. Open de **Microsoft Azure Backup**-module. Als u niet waar dat de module is geïnstalleerd weet, zoekt u de computer of server voor **Microsoft Azure Backup**.

    De desktop-app moet worden weergegeven in de lijst met zoekresultaten.

2. Klik op **gegevens herstellen** om de wizard te starten.

    ![Gegevens herstellen](./media/backup-azure-restore-windows-server/recover.png)

3. Op de **aan de slag** in het deelvenster voor het terugzetten van de gegevens op dezelfde server of computer, selecteer **deze server (`<server name>`)** en klikt u op **volgende**.

    ![Kies deze serveroptie voor het terugzetten van de gegevens op de machine](./media/backup-azure-restore-system-state/samemachine.png)

4. Op de **Selecteer herstelmodus** deelvenster Kies **systeemstatus** en klik vervolgens op **volgende**.

    ![Door bestanden bladeren](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. In de agenda van **Selecteer Volume en datum** deelvenster, selecteert u een herstel verwijzen. 

    U kunt herstellen vanaf een herstelpunt in de tijd. Datums in **vet** geven van de beschikbaarheid van ten minste één herstelpunt. Wanneer u een datum selecteert als er meerdere herstelpunten beschikbaar zijn, kiest u het specifieke herstelpunt uit de **tijd** vervolgkeuzelijst.

    ![Volume en datum](./media/backup-azure-restore-system-state/select-date.png)

6. Nadat u ervoor het herstelpunt gekozen hebt te herstellen, klikt u op **volgende**.

    Azure Backup koppelt u het lokale herstelpunt en gebruikt deze als een herstelvolume.

7. Geef de bestemming voor de herstelde bestanden van de systeemstatus op het volgende deelvenster en klik op **Bladeren** open Windows Verkenner en de bestanden en mappen die u wilt zoeken. De optie **kopieën maken zodat u beide versies**, kopieën gemaakt van afzonderlijke bestanden in een bestaande systeemstatus bestandsarchief in plaats van de kopie van het hele archief van de systeemstatus maken.

    ![Opties voor herstel](./media/backup-azure-restore-system-state/recover-as-files.png)

8. Controleer de details van het herstel op de **bevestiging** deelvenster en klik op **herstellen**.

   ![Klik op herstellen om te bevestigen dat de actie herstellen](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kopieer de *WindowsImageBackup* Active directory in het hersteldoel met een niet-kritieke volume van de server. Het volume met de Windows-besturingssysteem is meestal het essentiële volume.

10. Wanneer het herstel voltooid is, volg de stappen in de sectie [toepassen systeemstatusbestanden hersteld naar de Windows-Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), om de systeemstatus herstellen-proces te voltooien.

## <a name="recover-system-state-files-to-an-alternate-server"></a>Systeemstatusbestanden herstellen naar een andere server

Als uw Windows-Server beschadigd of niet toegankelijk is en u herstellen naar een stabiele status wilt door het herstellen van de systeemstatus van Windows Server, kunt u de beschadigde server-systeemstatus herstellen vanaf een andere server. Gebruik de volgende stappen voor het terugzetten van de systeemstatus op een afzonderlijke server.  

De termen die in deze stappen bevat:

- *Broncomputer* : de oorspronkelijke computer waarop de back-up is gemaakt en die momenteel niet beschikbaar.
- *Doelcomputer* – de machine waarop de gegevens worden hersteld.
- *Voorbeeld kluis* – de Recovery Services-kluis waarnaar de *bronmachine* en *doelmachine* zijn geregistreerd. <br/>

> [!NOTE]
> Back-ups die zijn overgenomen uit één machine kunnen niet worden hersteld naar een machine met een eerdere versie van het besturingssysteem. Bijvoorbeeld, kunnen niet back-ups die vanaf een computer met Windows Server 2016 worden hersteld naar Windows Server 2012 R2. De inverse is echter mogelijk. U kunt back-ups van Windows Server 2012 R2 gebruiken om te herstellen van Windows Server 2016.
>

1. Open de **Microsoft Azure Backup** -module op de *doelmachine*.
2. Zorg ervoor dat de *doelmachine* en de *bronmachine* zijn geregistreerd bij dezelfde Recovery Services-kluis.
3. Klik op **gegevens herstellen** starten van de werkstroom.
4. Selecteer **een andere server**

    ![Een andere Server](./media/backup-azure-restore-system-state/anotherserver.png)

5. Geef het kluisreferentiebestand dat overeenkomt met de *voorbeeld kluis*. Als het kluisreferentiebestand is ongeldig (of verlopen), downloadt u een nieuw kluisreferentiebestand van de *voorbeeld kluis* in Azure portal. Zodra het kluisreferentiebestand is opgegeven, wordt de Recovery Services-kluis die zijn gekoppeld aan het kluisreferentiebestand weergegeven.

6. Selecteer in de back-up-Server selecteren de *bronmachine* uit de lijst met computers weergegeven.
7. Kies in het deelvenster Selecteer herstelmodus **systeemstatus** en klikt u op **volgende**. 

    ![Search](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. In de agenda van de **Selecteer Volume en datum** deelvenster, selecteert u een herstel verwijzen. U kunt herstellen vanaf een herstelpunt in de tijd. Datums in **vet** geven van de beschikbaarheid van ten minste één herstelpunt. Wanneer u een datum selecteert als er meerdere herstelpunten beschikbaar zijn, kiest u het specifieke herstelpunt uit de **tijd** vervolgkeuzelijst. 

    ![Zoekitems](./media/backup-azure-restore-system-state/select-date.png)

9. Nadat u ervoor het herstelpunt gekozen hebt te herstellen, klikt u op **volgende**.

10. Op de **System State-herstelmodus Selecteer** deelvenster geeft u de doellocatie waar u bestanden van de systeemstatus te worden hersteld, en klik vervolgens op **volgende**.

    ![Versleuteling](./media/backup-azure-restore-system-state/recover-as-files.png)

    De optie **kopieën maken zodat u beide versies**, kopieën gemaakt van afzonderlijke bestanden in een bestaande systeemstatus bestandsarchief in plaats van de kopie van het hele archief van de systeemstatus maken.

11. Controleer de details van het herstel in het deelvenster bevestiging en klik op **herstellen**. 

    ![Klik op de knop herstellen om te bevestigen dat het herstelproces](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. Kopieer de *WindowsImageBackup* map naar een niet-kritieke volume van de server (bijvoorbeeld D:\). Het volume met de Windows-besturingssysteem is meestal het essentiële volume.

13. Gebruik de volgende sectie op voor het voltooien van het herstelproces, [toepassen van de herstelde systeemstatus bestanden op een Windows-Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).




## <a name="apply-restored-system-state-on-a-windows-server"></a>Herstelde systeemstatus toepassen op een Windows-Server

Nadat u de systeemstatus als bestanden met behulp van Azure Recovery Services-Agent hebt hersteld, gebruikt u het hulpprogramma voor Windows Server back-up de herstelde systeemstatus toepassen op Windows Server. Het hulpprogramma Windows Server back-up is al beschikbaar op de server. De volgende stappen wordt uitgelegd hoe u kunt de herstelde systeemstatus toepassen.

1. De volgende opdrachten gebruiken voor het opnieuw opstarten van uw server in *Directory Services Repair Mode*. In een opdrachtprompt met verhoogde bevoegdheid:

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. Na het opnieuw opstarten, open de module Windows Server back-up. Als u niet waar dat de module is geïnstalleerd weet, zoekt u de computer of server voor **Windows Server back-up**.

    De desktop-app wordt weergegeven in de lijst met zoekresultaten.

3. Selecteer in de module **lokale back-up**.

    ![Lokale back-up te herstellen van daaruit selecteren](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. Op de lokale back-up-console in de **actiedeelvenster**, klikt u op **herstellen** om de Wizard herstel te openen.

5. Selecteer de optie **een back-up die is opgeslagen in een andere locatie**, en klikt u op **volgende**.

   ![ervoor kiezen om te herstellen naar een andere server](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. Wanneer u het locatietype opgeeft, selecteert u **externe gedeelde map** als uw systeemstatusback-up is hersteld naar een andere server. Als de status van het systeem lokaal is hersteld, selecteert u **lokale stations**. 

    ![Selecteer of u wilt herstellen van de lokale server of een andere](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. Geef het pad naar de *WindowsImageBackup* directory, of Kies het lokale station dat met deze map (bijvoorbeeld D:\WindowsImageBackup), die wordt hersteld als onderdeel van de systeemstatus bestanden herstellen met behulp van Azure Recovery Services Agent en klik op **volgende**.

    ![pad naar het gedeelde bestand](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. Selecteer de systeemstatus-versie die u wilt herstellen en klik op **volgende**.

9. Selecteer in het deelvenster Type herstelbewerking selecteren **systeemstatus** en klikt u op **volgende**.

10. Voor de locatie van de systeemstatus te herstellen, selecteert u **oorspronkelijke locatie**, en klikt u op **volgende**.

11. Bekijk de details bevestigen, Controleer de instellingen voor opnieuw opstarten en klikt u op **herstellen** op de herstelde systeemstatus applly bestanden.

    ![Start het terugzetten van bestanden van de systeemstatus](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a>Speciale overwegingen voor het herstel van de systeemstatus op Active Directory-server

De systeemstatusback-up bevat Active Directory-gegevens. Gebruik de volgende stappen uit om te herstellen van Active Directory Domain Services (AD DS) van de huidige status naar een eerdere status.

1. Start opnieuw op de domeincontroller in de Directory Services Restore Mode (DSRM).
2. Volg de stappen [hier](https://technet.microsoft.com/library/cc794755(v=ws.10).aspx) Windows Server Backup-cmdlets gebruikt om te herstellen van AD DS.


## <a name="troubleshoot-failed-system-state-restore"></a>Mislukt herstel van systeemstatus oplossen

Als het vorige proces van het toepassen van de systeemstatus niet wordt voltooid, gebruikt u de Windows Recovery Environment (Windows RE) om te herstellen van uw Windows-Server. De volgende stappen wordt uitgelegd hoe u kunt herstellen met behulp van Windows RE. Gebruik deze optie alleen als Windows Server niet normaal na het herstellen van de systeemstatus opgestart. Het volgende proces worden de gegevens in niet-systeem, Wees voorzichtig gewist. 

1. Start op uw Windows-Server in de Windows Recovery Environment (Windows RE).

2. Problemen oplossen uit de drie beschikbare opties selecteren.

    ![menu openen](./media/backup-azure-restore-system-state/winre-1.png)

3. Uit de **geavanceerde opties** scherm, selecteer **opdrachtprompt** en geef de gebruikersnaam voor de beheerder en het wachtwoord.

   ![menu openen](./media/backup-azure-restore-system-state/winre-2.png)

4. Geef de gebruikersnaam voor de beheerder en het wachtwoord.

    ![menu openen](./media/backup-azure-restore-system-state/winre-3.png)

5. Wanneer u de opdrachtprompt in de beheerdersmodus opent, Voer u de volgende opdracht voor het ophalen van de back-versies van de systeemstatus.

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![back-versies van de systeemstatus ophalen](./media/backup-azure-restore-system-state/winre-4.png)

6. Voer de volgende opdracht uit om op te halen van alle volumes die beschikbaar zijn in de back-up.

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![back-versies van de systeemstatus ophalen](./media/backup-azure-restore-system-state/winre-5.png)

7. De volgende opdracht worden alle volumes die deel van de systeemstatusback-up uitmaken herstelt. Houd er rekening mee dat deze stap herstelt alleen de essentiële volumes die deel van de systeemstatus uitmaken. Alle andere gegevens worden gewist.

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![back-versies van de systeemstatus ophalen](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a>Volgende stappen
* Nu u uw bestanden en mappen hebt hersteld, kunt u [uw back-ups beheren](backup-azure-manage-windows-server.md).
