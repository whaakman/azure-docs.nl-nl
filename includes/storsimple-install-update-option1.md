<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-download-hotfixes"></a>Hotfixes downloaden
Voer de volgende stappen uit voor het downloaden van de software-update.

1. Start Internet Explorer en blader naar [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Als dit de eerste keer is dat u de Microsoft Update-catalogus op deze computer gebruikt, klikt u op **Installeren** wanneer u wordt gevraagd of u de invoegtoepassing voor de Microsoft Update-catalogus wilt installeren.
    ![Installeren van de catalogus](./media/storsimple-install-update-option-1/HCS_InstallCatalog-include.png)
3. Voer het nummer van het Knowledge Base (KB) van de hotfix die u downloaden wilt, bijvoorbeeld in het zoekvak van de Microsoft Update Catalog **3063418**, en klik vervolgens op **Search**.
4. U ziet de **StorSimple Update 1.2 toestel Update** bundel. Klik op **Add**. De update wordt toegevoegd aan het mandje.
5. Zoeken naar aanvullende hotfixes die worden vermeld in de bovenstaande tabel (**3043005** en **3063416**), en elk het mandje toe te voegen.
6. Klik op **Mandje weergeven**.
   
    ![Mandje weergeven](./media/storsimple-install-update-option-1/HCS_InstallBasket-include.png)
7. Klik op **Downloaden**. Typ of **blader naar** een lokale locatie waar u de downloads wilt weergeven. De updates worden naar de opgegeven locatie gedownload en in een submap met dezelfde naam als de update geplaatst. De map kan ook worden gekopieerd naar een netwerkshare die bereikbaar is vanaf het apparaat.

> [!NOTE]
> De hotfixes moet toegankelijk zijn via beide domeincontrollers voor het detecteren van mogelijke foutberichten van de peer-controller.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Hotfixes in de normale modus installeren en controleren
De volgende stappen uit om te installeren en controleer of de hotfixes normale modus uitvoeren. Als u al deze met de Azure Portal geïnstalleerd, gaat u verder met [installeren en testen van onderhoud modus hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).

1. Toegang tot de Windows PowerShell-interface op de seriële console van het StorSimple-apparaat voor het installeren van de software-update. Volg de gedetailleerde instructies in [PuTTY gebruiken om verbinding te maken met de seriële console van het apparaat](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Druk op de opdrachtregel op **Enter**.
2. Selecteer **Optie 1** als u zich met volledige toegang wilt aanmelden bij het apparaat.
3. Voor het installeren van het updatepakket achter de opdrachtprompt, typt u:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Gebruik in bovenstaande opdracht IP in plaats van DNS in het Pad naar share. De referentieparameter wordt alleen gebruikt voor toegang tot een geverifieerde share.
   
    U wordt geadviseerd om de referentieparameter te gebruiken voor toegang tot shares. Zelfs shares die voor 'iedereen' zijn geopend, zijn gewoonlijk niet geopend voor niet-geverifieerde gebruikers.
   
    Hieronder ziet u een voorbeeld van de uitvoer.
   
    ```
    Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
    \hcsmdssoftwareupdate.exe -Credential contoso\John

    Confirm

    This operation starts the hotfix installation and could reboot one or
    both of the controllers. If the device is serving I/Os, these will not
    be disrupted. Are you sure you want to continue?
    [Y] Yes [N] No [?] Help (default is "Y"): Y
    ```

4. Typ **J** wanneer u wordt gevraagd de hotfixinstallatie te bevestigen.
5. Bewaak de update met behulp van de cmdlet `Get-HcsUpdateStatus`.
   
    Hieronder ziet u een voorbeeld van uitvoer terwijl de update nog bezig is. De `RunInprogress` is `True` wanneer de update nog bezig is.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp : 9/02/2015 10:36:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
     In de volgende voorbeelduitvoer wordt aangegeven dat de update is voltooid. De `RunInProgress` is `False` wanneer de update is voltooid.

    ```
    Controller1>Get-HcsUpdateStatus

    RunInprogress       : False
    LastHotfixTimestamp : 9/02/2015 10:56:13 PM
    LastUpdateTimestamp : 9/02/2015 10:35:25 PM
    Controller0Events   :
    Controller1Events   :
    ```
   
   > [!NOTE]
   > Af en toe meldt de cmdlet `False` wanneer de update nog wordt uitgevoerd. Als u er zeker van wilt zijn dat de hotfix is voltooid, wacht u enkele minuten en voert u deze opdracht opnieuw uit. Controleer of de `RunInProgress` `False` is. Als dat het geval is, is de hotfix voltooid.
   > 
   > 
6. Wanneer de software-update is voltooid, controleert u de versies van de systeemsoftware. Typ de volgende opdracht:
   
    `Get-HcsSystem`
   
    De volgende versies moeten worden weergegeven:
   
   * HcsSoftwareVersion: 6.3.9600.17584
   * CisAgentVersion: 1.0.9049.0
   * MdsAgentVersion: 26.0.4696.1433
     
     Als het versienummer niet wijzigen nadat de update hebt toegepast, betekent dit dat de hotfix toe te passen is mislukt. Neem in dat geval contact op met [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) voor verdere hulp.
7. Herhaal stap 3-5 voor de installatie van de resterende normale modus hotfix (KB3043005).

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Hotfixes in de onderhoudsmodus installeren en controleren
Gebruik KB3063416 schijf firmware-updates te installeren. Deze updates zijn verstoren en ongeveer 30 tot 45 minuten duren om te voltooien. U kunt ervoor kiezen om deze te installeren in een gepland onderhoudsvenster door verbinding te maken met de console van het seriële apparaat.

Volg onderstaande instructies om de updates van de schijffirmware te installeren.

1. Plaats het apparaat in de onderhoudsmodus. Houd er rekening mee dat u Windows PowerShell op afstand niet gebruiken moet bij het verbinden met een apparaat in de onderhoudsmodus. U moet deze cmdlet uitvoeren op de controller van het apparaat wanneer u verbonden zijn via de seriële console van het apparaat. Type:
   
    `Enter-HcsMaintenanceMode`
   
    Hieronder ziet u een voorbeeld van de uitvoer.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update1-8100-SHG0997879L76YD
        Software Version: 6.3.9600.17584
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Beide domeincontrollers start opnieuw op in de onderhoudsmodus.
2. Typ het volgende om de update van de schijffirmware te installeren:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Hieronder ziet u een voorbeeld van de uitvoer.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\DiskFirmwarePackage.exe -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. Bewaak de installatievoortgang met de opdracht `Get-HcsUpdateStatus`. De update is voltooid als de `RunInProgress` verandert in `False`.
4. Nadat de installatie voltooid is, wordt de domeincontroller waarop de onderhoudsmodus modus hotfix is geïnstalleerd opnieuw worden opgestart. Meld u aan als in optie 1 met volledige toegang en controleer de versie van de schijffirmware. Type:
   
   `Get-HcsFirmwareVersion`
   
   De verwachte versies van de schijffirmware zijn:
   
   `XMGG, XGEE, KZ50, F6C2, VR08`
   
   Voer op de tweede controller de opdracht `Get-HcsFirmwareVersion` uit om te controleren of de versie van de software is bijgewerkt. Daarna kunt u de onderhoudsmodus afsluiten. Typ de volgende opdracht voor elke domeincontroller apparaat:
   
   `Exit-HcsMaintenanceMode`
5. De controllers opnieuw opstarten als u de onderhoudsmodus afsluit. Wanneer de updates van de schijffirmware met succes zijn toegepast en de onderhoudsmodus is afgesloten, gaat u weer naar de klassieke Azure-portal. Houd er rekening mee dat de portal kan niet worden weergegeven dat u de updates van de modus onderhoud voor 24 uur geïnstalleerd.

