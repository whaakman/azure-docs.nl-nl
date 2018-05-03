<!--author=alkohli last changed: 09/01/16-->

#### <a name="to-download-hotfixes"></a>Hotfixes downloaden
Voer de volgende stappen uit om de software-update te downloaden uit de Microsoft Update-catalogus.

1. Start Internet Explorer en blader naar [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).
2. Als dit de eerste keer is dat u de Microsoft Update-catalogus op deze computer gebruikt, klikt u op **Installeren** wanneer u wordt gevraagd of u de invoegtoepassing voor de Microsoft Update-catalogus wilt installeren.
    ![Installeren van de catalogus](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)
3. Voer het nummer van het Knowledge Base (KB) van de hotfix die u downloaden wilt, bijvoorbeeld in het zoekvak van de Microsoft Update Catalog **3186843**, en klik vervolgens op **Search**.
   
    De hotfix-wordt weergegeven, bijvoorbeeld **cumulatieve Software bundel Update 3.0 voor StorSimple 8000-serie**.
   
    ![Catalogus doorzoeken](./media/storsimple-install-update2-hotfix/HCS_SearchCatalog1-include.png)
4. Klik op **Add**. De update wordt toegevoegd aan het mandje.
5. Zoeken naar aanvullende hotfixes die worden vermeld in de bovenstaande tabel (**3186859**), en elk aan de mandje toe te voegen.
6. Klik op **Mandje weergeven**.
7. Klik op **Downloaden**. Typ of **blader naar** een lokale locatie waar u de downloads wilt weergeven. De updates zijn gedownload naar de opgegeven locatie en geplaatst in een submap met dezelfde naam als de update. De map kan ook worden gekopieerd naar een netwerkshare die bereikbaar is vanaf het apparaat.

> [!NOTE]
> De hotfixes moet toegankelijk zijn via beide domeincontrollers voor het detecteren van mogelijke foutberichten van de peer-controller.
> 
> 

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Hotfixes in de normale modus installeren en controleren
Voer de volgende stappen uit om hotfixes in de normale modus te installeren en te controleren. Als u al deze met de Azure Portal geïnstalleerd, gaat u verder met [installeren en testen van onderhoud modus hotfixes](#to-install-and-verify-maintenance-mode-hotfixes).

1. U installeert de hotfixes door de Windows PowerShell-interface op de seriële console van het StorSimple-apparaat te openen. Volg de gedetailleerde instructies in [PuTTY gebruiken om verbinding te maken met de seriële console van het apparaat](../articles/storsimple/storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console). Druk op de opdrachtregel op **Enter**.
2. Selecteer **Optie 1** als u zich met volledige toegang wilt aanmelden bij het apparaat. U wordt geadviseerd om de hotfix eerst op de passieve controller te installeren.
3. Installeer de hotfix door achter de opdrachtprompt het volgende te typen:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Gebruik in bovenstaande opdracht IP in plaats van DNS in het Pad naar share. De referentieparameter wordt alleen gebruikt voor toegang tot een geverifieerde share.
   
    U wordt geadviseerd om de referentieparameter te gebruiken voor toegang tot shares. Zelfs shares die voor 'iedereen' zijn geopend, zijn gewoonlijk niet geopend voor niet-geverifieerde gebruikers.
   
    Geef het wachtwoord op wanneer dit wordt gevraagd.
   
    Hieronder ziet u een voorbeeld van de uitvoer.
   
        ````
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \hcsmdssoftwareupdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ````
4. Typ **J** wanneer u wordt gevraagd de hotfixinstallatie te bevestigen.
5. Bewaak de update met behulp van de cmdlet `Get-HcsUpdateStatus`. De update wordt eerst voltooid op de passieve controller. Zodra de passieve controller is bijgewerkt, wordt er een failover uitgevoerd en wordt de update toegepast op de andere controller. De update is voltooid wanneer beide controllers zijn bijgewerkt.
   
    Hieronder ziet u een voorbeeld van uitvoer terwijl de update nog bezig is. De `RunInprogress` is `True` wanneer de update nog bezig is.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 8/29/2016 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     In de volgende voorbeelduitvoer wordt aangegeven dat de update is voltooid. De `RunInProgress` is `False` wanneer de update is voltooid.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 8/30/2016 9:15:55 AM
    LastUpdateTimestamp : 8/30/2016 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE] 
    > Af en toe meldt de cmdlet `False` wanneer de update nog wordt uitgevoerd. Als u er zeker van wilt zijn dat de hotfix is voltooid, wacht u enkele minuten en voert u deze opdracht opnieuw uit. Controleer of de `RunInProgress` `False` is. Als dat het geval is, is de hotfix voltooid.

1. Wanneer de software-update is voltooid, controleert u de versies van de systeemsoftware. Type:
   
    `Get-HcsSystem`
   
    De volgende versies moeten worden weergegeven:
   
   * `HcsSoftwareVersion: 6.3.9600.17759`
   * `CisAgentVersion:  1.0.9343.0`
   * `MdsAgentVersion: 30.0.4698.16`
     
     Als het versienummer niet wijzigen nadat de update hebt toegepast, betekent dit dat de hotfix toe te passen is mislukt. Neem in dat geval contact op met [Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) voor verdere hulp.
     
     > [!IMPORTANT]
     > U moet de actieve controller opnieuw opstarten via de cmdlet `Restart-HcsController` voordat u de overige updates toepast. 
     > 
     > 
2. Herhaal stap 3-5 voor de installatie van de hotfix voor stuurprogramma's en firmware LSI **KB3186859**. Nadat de hotfix is geïnstalleerd, gebruikt u de `Get-HcsSystem` cmdlet. De versie LSI moet:
   
   * `Lsisas2Version: 2.0.78.00`
3. Herhaal stap 3-5 voor de installatie van de update Storport en Spaceport **KB3121261**.
4. Als u van Update 2 of oudere versie bijwerken wilt, moet u ook downloaden:
   
   * iSCSI-oplossing met behulp van KB3146621
   * WMI-oplossing met behulp van KB3103616

#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Hotfixes in de onderhoudsmodus installeren en controleren
Gebruik KB3121899 schijf firmware-updates te installeren. Dit zijn updates waarvoor de computer opnieuw moet worden opgestart. De uitvoering ervan neemt ongeveer 30 minuten in beslag. U kunt ervoor kiezen om deze te installeren in een gepland onderhoudsvenster door verbinding te maken met de console van het seriële apparaat.

Als de schijffirmware van de schijf al is bijgewerkt, hoeft u deze updates niet te installeren. Voer de cmdlet `Get-HcsUpdateAvailability` uit vanaf de console van het seriële apparaat om te controleren of er updates beschikbaar zijn en of de computer voor de updates opnieuw moet worden opgestart (onderhoudsmodus) of niet (normale modus).

Volg onderstaande instructies om de updates van de schijffirmware te installeren.

1. Het apparaat in de onderhoudsmodus plaatsen. Houd er rekening mee dat u Windows PowerShell op afstand niet gebruiken moet bij het verbinden met een apparaat in de onderhoudsmodus. In plaats daarvan deze cmdlet uitvoeren op de controller van het apparaat wanneer u verbonden zijn via de seriële console van het apparaat. Type:
   
    `Enter-HcsMaintenanceMode`
   
    Hieronder ziet u een voorbeeld van de uitvoer.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8100
        Name: Update2-8100-SHG0997879L76673
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
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. Bewaak de installatievoortgang met de opdracht `Get-HcsUpdateStatus`. De update is voltooid als de `RunInProgress` verandert in `False`.
4. Wanneer de installatie is voltooid, wordt de controller waarop de hotfix van de onderhoudsmodus is geïnstalleerd, opnieuw opstart. Meld u aan als in optie 1 met volledige toegang en controleer de versie van de schijffirmware. Type:
   
   `Get-HcsFirmwareVersion`
   
   De verwachte versies van de schijffirmware zijn:
   
   `XMGG, XGEG, KZ50, F6C2, VR08`
   
   Hieronder ziet u een voorbeeld van de uitvoer.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8100
       Name: Update2-8100-SHG0997879L76YD
       Software Version: 6.3.9600.17705
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
         ActiveBIOS:0.45.0006
         BackupBIOS:0.45.0008
         MainCPLD:17.0.0005
         ActiveBMCRoot:2.0.000E
         BackupBMCRoot:2.0.000E
         BMCBoot:2.0.0001
         LsiFirmware:19.00.00.00
         LsiBios:07.37.00.00
         Battery1Firmware:06.29
         Battery2Firmware:06.29
         DomFirmware:X231600
         CanisterFirmware:3.5.0.32
         CanisterBootloader:5.03
         CanisterConfigCRC:0xD1B030A4
         CanisterVPDStructure:0x06
         CanisterGEMCPLD:0x17
         CanisterVPDCRC:0xEE3504B4
         MidplaneVPDStructure:0x0C
         MidplaneVPDCRC:0xA6BD4F64
         MidplaneCPLD:0x10
         PCM1Firmware:1.00|1.05
         PCM1VPDStructure:0x05
         PCM1VPDCRC:0x41BEF99C
         PCM2Firmware:1.00|1.05
         PCM2VPDStructure:0x05
         PCM2VPDCRC:0x41BEF99C
   
         DisksFirmware
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST400FM0073:XGEG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
         SEAGATE:ST4000NM0023:XMGG
   
    Voer op de tweede controller de opdracht `Get-HcsFirmwareVersion` uit om te controleren of de versie van de software is bijgewerkt. Daarna kunt u de onderhoudsmodus afsluiten. Daartoe typt u de volgende opdracht voor elke apparaatcontroller:
   
   `Exit-HcsMaintenanceMode`
5. De controllers opnieuw opstarten als u de onderhoudsmodus afsluit. Wanneer de updates van de schijffirmware met succes zijn toegepast en de onderhoudsmodus is afgesloten, gaat u weer naar de klassieke Azure-portal. Houd er rekening mee dat de portal kan niet worden weergegeven dat u de updates van de modus onderhoud voor 24 uur geïnstalleerd.

