---
title: 'Azure Backup: Systeemstatus terugzetten naar een WindowsServer | Microsoft Docs'
description: Stap door stap uitleg voor Windows Server-systeemstatus terugzetten vanaf een back-up in Azure.
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/18/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 320c85f8045d9b72cf7f430d2e2736ba8e5ec269
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="restore-system-state-to-windows-server"></a>Systeemstatus terugzetten naar WindowsServer

In dit artikel wordt uitgelegd hoe Windows Server System State back-ups herstellen uit een Azure Recovery Services-kluis. Als u systeemstatus herstellen, moet u een systeemstatusback-up hebben (gemaakt met behulp van de instructies in [Back-up van systeemstatus](backup-azure-system-state.md#back-up-windows-server-system-state-preview)), en zorg ervoor dat u hebt geïnstalleerd het [meest recente versie van de agent van Microsoft Azure Recovery Services (MARS)](http://aka.ms/azurebackup_agent). Systeemstatus van Windows Server-gegevens herstellen vanaf een Azure Recovery Services-kluis is een proces in twee stappen:

1. Systeemstatus terugzetten als bestanden vanuit Azure Backup. Bij het herstellen van systeemstatus als bestanden vanuit Azure Backup, kunt u:
  * Restore systeemstatus naar dezelfde server waarop de back-ups zijn gemaakt, of
  * Bestand van de systeemstatus herstellen naar een alternatieve server.

2. De herstelde bestanden van de systeemstatus van toepassing op een Windows-Server.


## <a name="recover-system-state-files-to-the-same-server"></a>Bestanden van de systeemstatus herstellen naar dezelfde server
De volgende stappen wordt uitgelegd hoe uw Windows Server-configuratie terugdraaien naar een eerdere status. Configuratie van uw server terugdraaien naar een bekende, stabiele status, kan zeer waardevol zijn. De systeemstatus van de server terugzetten de volgende stappen vanaf een Recovery Services-kluis. 

1. Open de **Microsoft Azure Backup** -module. Als u niet waar dat de module is geïnstalleerd weet, zoekt u de computer of server voor **Microsoft Azure Backup**.

    De bureaublad-app moet worden weergegeven in de zoekresultaten.

2. Klik op **gegevens herstellen** om de wizard te starten.

    ![Gegevens herstellen](./media/backup-azure-restore-windows-server/recover.png)

3. Op de **aan de slag** deelvenster voor het terugzetten van de gegevens op dezelfde server of computer, selecteer **deze server (`<server name>`)** en klik op **volgende**.

    ![Selecteer deze serveroptie als de gegevens herstelt op de machine](./media/backup-azure-restore-system-state/samemachine.png)

4. Op de **herstelmodus Selecteer** deelvenster kiezen **systeemstatus** en klik vervolgens op **volgende**.

    ![Blader door bestanden](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. In de kalender in **Volume selecteert en datum** deelvenster, selecteert u een herstel verwijzen. 

    U kunt vanaf elk willekeurig herstelpunt herstellen in de tijd. Datums **vet** geven aan de beschikbaarheid van ten minste één herstelpunt. Wanneer u een datum selecteren als er meerdere herstelpunten beschikbaar zijn, kiest u de specifieke herstelpunt in de **tijd** vervolgkeuzelijst.

    ![Volume en datum](./media/backup-azure-restore-system-state/select-date.png)

6. Nadat u ervoor het herstelpunt gekozen hebt te herstellen, klikt u op **volgende**.

    Azure Backup koppelt het lokale herstelpunt en gebruikt als een volume herstel.

7. Geef het doel voor de herstelde bestanden van de systeemstatus in het volgende deelvenster en klik op **Bladeren** open Windows Verkenner en de bestanden en mappen die u wilt zoeken. De optie **kopieën te maken, zodat beide versies**, maakt kopieën van afzonderlijke bestanden in een bestaande systeemstatus bestandsarchief in plaats van de kopie van het gehele archief van de systeemstatus te maken.

    ![Opties voor herstel](./media/backup-azure-restore-system-state/recover-as-files.png)

8. Controleer de details van herstelpunten op de **bevestiging** deelvenster en klik op **herstellen**.

   ![Klik op herstellen om de actie herstellen bevestigen](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kopieer de *WindowsImageBackup* map in de doel-herstel naar een niet-kritieke volume van de server. Het Windows-besturingssysteemvolume is meestal het essentiële volume.

10. Wanneer het herstel voltooid is, volg de stappen in de sectie [toepassen systeemstatus bestanden naar de Windows-Server hersteld](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), om de systeemstatus herstellen proces te voltooien.

## <a name="recover-system-state-files-to-an-alternate-server"></a>Bestanden van de systeemstatus herstellen naar een alternatieve server

Als uw Windows-Server beschadigd of niet toegankelijk is en u herstellen naar een stabiele status wilt door het herstellen van de systeemstatus van Windows Server, kunt u de systeemstatus van de beschadigde server herstellen vanaf een andere server. Gebruik de volgende stappen voor het terugzetten van systeemstatus op een afzonderlijke server.  

De terminologie die wordt gebruikt in deze stap omvat het volgende:

- *Bronmachine* : de oorspronkelijke computer waarop de back-up is gemaakt en die momenteel niet beschikbaar.
- *Doelcomputer* : de computer waarop de gegevens worden hersteld.
- *Voorbeeld kluis* – de Recovery Services-kluis waarnaar de *bronmachine* en *doelmachine* zijn geregistreerd. <br/>

> [!NOTE]
> Back-ups die afkomstig zijn uit één machine kunnen niet worden hersteld naar een machine met een eerdere versie van het besturingssysteem. Bijvoorbeeld: back-ups die afkomstig zijn uit een Windows Server 2016-machine kunnen niet worden teruggezet naar Windows Server 2012 R2. De inverse is echter mogelijk. U kunt back-ups van Windows Server 2012 R2 gebruiken om te herstellen van Windows Server 2016.
>

1. Open de **Microsoft Azure Backup** -module op de *doelmachine*.
2. Zorg ervoor dat de *doelmachine* en de *bronmachine* zijn geregistreerd bij dezelfde Recovery Services-kluis.
3. Klik op **gegevens herstellen** starten van de werkstroom.

    ![Gegevens herstellen](./media/backup-azure-restore-windows-server-classic/recover.png)

4. Selecteer **een andere server**

    ![Een andere Server](./media/backup-azure-restore-system-state/anotherserver.png)

5. Geef het kluisreferentiebestand die overeenkomt met de *voorbeeld kluis*. Als het kluisreferentiebestand is ongeldig (of verlopen), downloadt u een nieuw kluisreferentiebestand van de *voorbeeld kluis* in de Azure portal. Zodra het kluisreferentiebestand is opgegeven, wordt de Recovery Services-kluis die is gekoppeld aan het kluisreferentiebestand weergegeven.

6. Selecteer in het deelvenster Selecteer back-upserver van de *bronmachine* uit de lijst met computers weergegeven.

    ![Lijst met computers](./media/backup-azure-restore-windows-server-classic/machinelist.png)

7. Kies in het deelvenster herstelmodus Selecteer **systeemstatus** en klik op **volgende**. 

    ![Search](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. In de kalender in het **Volume selecteert en datum** deelvenster, selecteert u een herstel verwijzen. U kunt vanaf elk willekeurig herstelpunt herstellen in de tijd. Datums **vet** geven aan de beschikbaarheid van ten minste één herstelpunt. Wanneer u een datum selecteren als er meerdere herstelpunten beschikbaar zijn, kiest u de specifieke herstelpunt in de **tijd** vervolgkeuzelijst. 

    ![Zoeken naar objecten](./media/backup-azure-restore-system-state/select-date.png)

9. Nadat u ervoor het herstelpunt gekozen hebt te herstellen, klikt u op **volgende**.

10. Op de **System status herstelmodus Selecteer** deelvenster geeft u de doellocatie waar u bestanden van de systeemstatus te herstellen en klik vervolgens op **volgende**.

    ![Versleuteling](./media/backup-azure-restore-system-state/recover-as-files.png)

    De optie **kopieën te maken, zodat beide versies**, maakt kopieën van afzonderlijke bestanden in een bestaande systeemstatus bestandsarchief in plaats van de kopie van het gehele archief van de systeemstatus te maken.

11. Controleer de details van herstel in het deelvenster bevestiging en klik op **herstellen**. 

    ![Klik op de knop herstellen om te bevestigen van het herstelproces](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. Kopieer de *WindowsImageBackup* map naar een niet-kritieke volume van de server (bijvoorbeeld D:\). Het Windows-besturingssysteemvolume is meestal het essentiële volume.

13. Gebruik de volgende sectie voor het voltooien van het herstelproces [toepassen van de herstelde bestanden van de systeemstatus op een Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).




## <a name="apply-restored-system-state-on-a-windows-server"></a>Herstelde systeemstatus toepassen op een Windows Server

Eenmaal u systeemstatus als bestanden met behulp van de Azure Recovery Services Agent hebt hersteld, gebruikt u de Windows Server back-up de herstelde systeemstatus toepassen op Windows Server. De Windows Server back-up is al beschikbaar op de server. De volgende stappen wordt uitgelegd hoe de status van het herstelde toepassen.

1. Gebruik de volgende opdrachten opnieuw opstarten van uw server in *Directory Services Repair Mode*. In een opdrachtprompt met verhoogde bevoegdheid:

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. Open de module Windows Server Backup na het opstarten. Als u niet waar dat de module is geïnstalleerd weet, zoekt u de computer of server voor **Windows Server Backup**.

    De bureaublad-app wordt weergegeven in de zoekresultaten.

3. Selecteer in de module **lokale back-up**.

    ![Lokale back-up te herstellen vanaf daar selecteren](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. Op de lokale back-up-console in de **actiedeelvenster**, klikt u op **herstellen** om de Wizard herstel te openen.

5. Selecteer de optie **een back-up die is opgeslagen in een andere locatie**, en klik op **volgende**.

   ![herstellen naar een andere server kiezen](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. Selecteer bij het opgeven van het type **externe gedeelde map** als uw back-up van systeemstatus is hersteld naar een andere server. Als de status van het systeem lokaal zijn hersteld, selecteert u **lokale stations**. 

    ![Selecteer of u wilt herstellen van de lokale server of een ander](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. Geef het pad naar de *WindowsImageBackup* directory, of kies de lokale schijf met deze map (bijvoorbeeld D:\WindowsImageBackup) als onderdeel van de systeemstatus bestanden herstellen met behulp van de Azure Recovery Services Agent hersteld en klikt u op **volgende**.

    ![pad naar het gedeelde bestand](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. De systeemstatus versie selecteren die u wilt herstellen en klik op **volgende**.

9. Selecteer in het deelvenster Type herstelbewerking selecteren **systeemstatus** en klik op **volgende**.

10. Voor de locatie van de systeemstatus te herstellen, selecteert u **oorspronkelijke locatie**, en klik op **volgende**.

11. Bekijk de details van de bevestiging, Controleer de instellingen voor opnieuw opstarten en klikt u op **herstellen** naar applly de status van het herstelde bestanden.

    ![Start het terugzetten van bestanden van de systeemstatus](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a>Speciale overwegingen voor herstel van de systeemstatus op Active Directory-server

Systeemstatusback-up bevat Active Directory-gegevens. Gebruik de volgende stappen uit om terug te zetten van Active Directory Domain Services (AD DS) van de huidige status naar een eerdere status.

1. Start de domeincontroller opnieuw in de Directory Services Restore Mode (DSRM).
2. Volg de stappen [hier](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) Windows Server Backup-cmdlets gebruiken om te herstellen van AD DS.


## <a name="troubleshoot-failed-system-state-restore"></a>Problemen met mislukte systeemstatus herstellen

Als het vorige proces van het toepassen van de systeemstatus niet wordt voltooid, gebruikt u de Windows Herstelomgeving (Win RE) voor het herstellen van uw Windows-Server. De volgende stappen wordt uitgelegd hoe herstellen met behulp van Win opnieuw. Gebruik deze optie alleen als Windows Server niet normaal wordt opgestart na het terugzetten van de systeemstatus. Het volgende proces worden niet door het systeem gegevens, Wees voorzichtig gewist. 

1. Start op uw Windows-Server in de Windows Herstelomgeving (Windows RE).

2. Selecteer oplossen van de drie beschikbare opties.

    ![menu openen](./media/backup-azure-restore-system-state/winre-1.png)

3. Van de **geavanceerde opties** Schakel in het scherm **opdrachtprompt** en geef de gebruikersnaam voor de beheerder en het wachtwoord.

   ![menu openen](./media/backup-azure-restore-system-state/winre-2.png)

4. Geef de gebruikersnaam voor de beheerder en het wachtwoord.

    ![menu openen](./media/backup-azure-restore-system-state/winre-3.png)

5. Wanneer u de opdrachtprompt in de beheerdersmodus opent, Voer u de volgende opdracht voor het ophalen van de back-upversies systeemstatus.

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![ophalen van de back-upversies systeemstatus](./media/backup-azure-restore-system-state/winre-4.png)

6. Voer de volgende opdracht om op te halen van alle volumes die beschikbaar zijn in de back-up.

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![ophalen van de back-upversies systeemstatus](./media/backup-azure-restore-system-state/winre-5.png)

7. De volgende opdracht wordt alle volumes die deel van de systeemstatusback-up uitmaken hersteld. Houd er rekening mee dat deze stap alleen de essentiële volumes die deel van de systeemstatus uitmaken herstelt. Alle andere gegevens worden gewist.

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![ophalen van de back-upversies systeemstatus](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a>Volgende stappen
* Nu dat u uw bestanden en mappen hebt hersteld, kunt u [beheren van uw back-ups](backup-azure-manage-windows-server.md).
