---
title: Voorbereiden en aanpassen van een master VHD-installatiekopie - Azure
description: Het voorbereiden, aanpassen en een virtuele Windows-bureaublad preview master-installatiekopie uploaden naar Azure.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: ca186090f28f04811030e83b159782a9bfeb87f9
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400776"
---
# <a name="prepare-and-customize-a-master-vhd-image"></a>Een VHD-hoofdinstallatiekopie voorbereiden en aanpassen

In dit artikel wordt uitgelegd hoe u een installatiekopie van het model virtuele harde schijf (VHD) voorbereiden voor het uploaden naar Azure, met inbegrip van virtuele machines (VM's) maken en te installeren en configureren van software op deze. Deze instructies zijn voor een virtuele Windows-bureaublad Preview-specifieke configuratie die kan worden gebruikt met de bestaande processen van uw organisatie.

## <a name="create-a-vm"></a>Een virtuele machine maken

Meerdere Windows 10 Enterprise-sessie is beschikbaar in de galerie met installatiekopieën van Azure. Er zijn twee opties voor het aanpassen van deze installatiekopie.

De eerste optie is voor het inrichten van een virtuele machine (VM) in Azure door de instructies in [een virtuele machine maken vanaf een beheerde installatiekopie](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed), en gaat u verder met [Software voorbereidings- en installatie](set-up-customize-master-image.md#software-preparation-and-installation).

De tweede optie is voor het maken van de installatiekopie lokaal downloaden van de installatiekopie, inrichten van een Hyper-V-VM en aan te passen aan uw behoeften, waarover meer in de volgende sectie.

### <a name="local-image-creation"></a>Het maken van lokale installatiekopieën

Nadat u de installatiekopie hebt gedownload naar een lokale locatie, opent u **Hyper-V-beheer** aan een virtuele machine maken met de VHD die u zojuist hebt gekopieerd. Hieronder volgt de eenvoudige versie, maar u vindt meer gedetailleerde instructies in [maken van een virtuele machine in Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v).

Een virtuele machine maken met de gekopieerde VHD:

1. Open de **Wizard Nieuwe virtuele Machine**.

2. Selecteer op de pagina generatie opgeven **generatie 1**.

    ![Een schermafbeelding van de pagina generatie opgeven. De optie "Generation 1" is geselecteerd.](media/a41174fd41302a181e46385e1e701975.png)

3. Onder Type controlepunt uitschakelen controlepunten Hiervoor schakelt het selectievakje in.

    ![Een schermafbeelding van het Type controlepunt-gedeelte van de pagina controlepunten.](media/20c6dda51d7cafef33251188ae1c0c6a.png)

U kunt ook de volgende cmdlet uitvoeren in PowerShell om uit te schakelen van controlepunten.

```powershell
Set-VM -Name <VMNAME> -CheckpointType Disabled
```

### <a name="fixed-disk"></a>Vaste schijf

Als u een virtuele machine vanaf een bestaande VHD maken, maakt het een dynamische schijf standaard. Het kan worden gewijzigd naar een vaste schijf door het selecteren van **schijf bewerken...**  zoals wordt weergegeven in de volgende afbeelding. Zie voor meer instructies, [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

![Een schermafbeelding van de schijf bewerken wordt gebruikt.](media/35772414b5a0f81f06f54065561d1414.png)

U kunt ook de volgende PowerShell-cmdlet als u wilt wijzigen van de schijf naar een vaste schijf uitvoeren.

```powershell
Convert-VHD –Path c:\\test\\MY-VM.vhdx –DestinationPath c:\\test\\MY-NEW-VM.vhd -VHDType Fixed
```

## <a name="software-preparation-and-installation"></a>Software voorbereidings- en installatie

In deze sectie bevat informatie over het voorbereiden en installeren van Office 365 ProPlus, OneDrive, FSLogix, Windows Defender en andere veelgebruikte toepassingen. Als uw gebruikers nodig hebben voor toegang tot bepaalde LOB-toepassingen, raden wij dat u deze installeren na het voltooien van deze sectie instructies.

In deze sectie wordt ervan uitgegaan dat u hebt met verhoogde bevoegdheden voor toegang op de virtuele machine, of deze ingericht in Azure of Hyper-V-beheer.

### <a name="install-office-in-shared-computer-activation-mode"></a>Office installeren in de modus voor activering van gedeelde computers

Gebruik de [Office Deployment Tool](https://www.microsoft.com/download/details.aspx?id=49117) Office te installeren. Meerdere sessies van Windows 10 Enterprise biedt alleen ondersteuning voor Office 365 ProPlus, niet-Office 2019 eeuwige.

De Office Deployment Tool is een XML-configuratiebestand vereist. Voor het aanpassen van het volgende voorbeeld, Zie de [configuratieopties voor de Office Deployment Tool](https://docs.microsoft.com/deployoffice/configuration-options-for-the-office-2016-deployment-tool).

De voorbeeldconfiguratie-XML zijn er beschikbaar gesteld voor dit wordt de volgende dingen doen:

- Office installeren vanaf het Insiders-kanaal en updates van het kanaal Insiders leveren wanneer ze worden uitgevoerd.
- Gebruik de x64-architectuur.
- Automatische updates uitschakelen.
- Visio en Project installeren.
- Verwijderen van eventuele bestaande installaties van Office en hun instellingen worden gemigreerd.
- Gedeelde computer gebruikt voor bewerking in een omgeving met terminal server-licentieverlening inschakelen.

Dit is wat deze voorbeeldconfiguratie XML wordt niet doen:

- Installeren van Skype voor bedrijven
- OneDrive installeren in de modus per gebruiker. Zie voor meer informatie, [OneDrive installeren in de modus per computer](#install-onedrive-in-per-machine-mode).

>[!NOTE]
>Gedeelde Computerlicenties kan worden ingesteld via groepsbeleidsobjecten (GPO's) of registerinstellingen. Het groepsbeleidsobject bevindt zich in **Computerconfiguratie\\beleid\\Beheersjablonen\\Microsoft Office 2016 (Machine)\\licentie-instellingen**

De Office Deployment Tool bevat setup.exe. Voer de volgende opdracht in een opdrachtregel voor het installeren van Office:

```batch
Setup.exe /configure configuration.xml
```

#### <a name="sample-configurationxml"></a>Voorbeeld configuration.xml

De volgende XML-voorbeeld wordt de Insiders-versie, ook wel bekend als Fast Insiders of Insiders Main installeren.

```xml
<Configuration>
    <Add OfficeClientEdition="64" SourcePath="http://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f">
        <Product ID="O365ProPlusRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Groove" />
            <ExcludeApp ID="Lync" />
            <ExcludeApp ID="OneDrive" />
            <ExcludeApp ID="Teams" />
        </Product>
        <Product ID="VisioProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" /> 
        </Product>
        <Product ID="ProjectProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" />
        </Product>
    </Add>
    <RemoveMSI All="True" />
    <Updates Enabled="FALSE" UpdatePath="http://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f" />
    <Display Level="None" AcceptEULA="TRUE" />
    <Logging Level="Verbose" Path="%temp%\WVDOfficeInstall" />
    <Property Value="TRUE" Name="FORCEAPPSHUTDOWN"/>
    <Property Value="1" Name="SharedComputerLicensing"/>
    <Property Value="TRUE" Name="PinIconsToTaskbar"/>
</Configuration>
```

>[!NOTE]
>Het team van Office adviseert om 64-bits installatie voor de **OfficeClientEdition** parameter.

Na de installatie van Office, kunt u het standaardgedrag voor Office bijwerken. Voer de volgende opdrachten afzonderlijk of in een batch-bestand om bij te werken van het gedrag.

```batch
rem Mount the default user registry hive
reg load HKU\TempDefault C:\Users\Default\NTUSER.DAT
rem Must be executed with default registry hive mounted.
reg add HKU\TempDefault\SOFTWARE\Policies\Microsoft\office\16.0\common /v InsiderSlabBehavior /t REG_DWORD /d 2 /f
rem Set Outlook's Cached Exchange Mode behavior
rem Must be executed with default registry hive mounted.
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v enable /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v syncwindowsetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSettingMonths  /t REG_DWORD /d 1 /f
rem Unmount the default user registry hive
reg unload HKU\TempDefault

rem Set the Office Update UI behavior.
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideupdatenotifications /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideenabledisableupdates /t REG_DWORD /d 1 /f
```

U kunt Automatische Updates handmatig uitschakelen.

Automatische Updates uitschakelen:

1. Office 365 installeren door de instructies in [Office installatiekopie voorbereiding](set-up-customize-master-image.md#office-image-preparation).
2. Eventuele extra toepassingen installeren door de instructies in [gebruikersprofielinstellingen (FSLogix)](set-up-customize-master-image.md#user-profile-setup-fslogix), [Windows Defender](set-up-customize-master-image.md#windows-defender), en [registerconfiguratieenanderetoepassingen](set-up-customize-master-image.md#other-applications-and-registry-configuration).
3. Windows Automatische Update-Service op de lokale virtuele machine uitschakelen.
4. Open **Editor voor lokaal groepsbeleid\\Beheersjablonen\\Windows-onderdelen\\Windows Update**.
5. Met de rechtermuisknop op **automatische updates configureren** en stel deze in op **uitgeschakelde**.

U kunt ook de volgende opdracht uitvoeren op de opdrachtregel om Automatische Updates uitschakelen.

```batch
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\\WindowsUpdate\AU /v NoAutoUpdate /t REG_DWORD /d 1 /f
```

Voer deze opdracht uit om op te geven van een indeling Start voor Windows 10-pc's.

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer" /v SpecialRoamingOverrideAllowed /t REG_DWORD /d 1 /f
```

### <a name="install-onedrive-in-per-machine-mode"></a>OneDrive installeren in de modus per computer

OneDrive is normaal gesproken geïnstalleerd per gebruiker. In deze omgeving moet per computer geïnstalleerd.

Dit is het installeren van OneDrive in de modus per computer:

1. Maak eerst een locatie voor het voorbereiden van het installatieprogramma OneDrive. Map van een lokale schijf of [\\\\unc] (file://unc) locatie is niets mis mee.

2. OneDriveSetup.exe downloaden naar uw gefaseerde locatie met deze koppeling: <https://aka.ms/OneDriveWVD-Installer>

3. Als u office met OneDrive geïnstalleerd zonder  **\<ExcludeApp-ID = "OneDrive" /\>**, eventuele bestaande installaties van de OneDrive-per-gebruiker vanaf een opdrachtprompt met verhoogde bevoegdheid verwijderen door het uitvoeren van de volgende opdracht:
    
    ```batch
    "[staged location]\OneDriveSetup.exe" /uninstall
    ```

4. Deze opdracht uitvoert vanaf een opdrachtprompt met verhoogde bevoegdheid om in te stellen de **AllUsersInstall** registerwaarde:

    ```batch
    REG ADD "HKLM\Software\Microsoft\OneDrive" /v "AllUsersInstall" /t REG_DWORD /d 1 /reg:64
    ```

5. Met deze opdracht voor het installeren van OneDrive in de modus per computer worden uitgevoerd:

    ```batch
    Run "[staged location]\OneDriveSetup.exe /allusers"
    ```

6. Voer deze opdracht voor het configureren van OneDrive om te beginnen bij aanmelding voor alle gebruikers:

    ```batch
    REG ADD "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v OneDrive /t REG_SZ /d "C:\\Program Files (x86)\Microsoft OneDrive\OneDrive.exe /background" /f
    ```

7. Schakel **gebruikersaccount op de achtergrond configureren** door het uitvoeren van de volgende opdracht uit.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "SilentAccountConfig" /t REG_DWORD /d 1 /f
    ```

8. Omleidings- en Windows-mappen in OneDrive bekend met de volgende opdracht verplaatsen.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "KFMSilentOptIn" /t REG_SZ /d "<your-AzureAdTenantId>" /f
    ```

### <a name="teams-and-skype"></a>Teams en Skype

Virtuele Windows-bureaublad officieel ondersteunt geen Skype voor bedrijven en Teams.

### <a name="set-up-user-profile-container-fslogix"></a>Instellen van de gebruiker profiel container (FSLogix)

Als u wilt de container FSLogix opnemen als onderdeel van de installatiekopie, volg de instructies in [instellen van een share van de gebruiker profiel voor een groep host](create-host-pools-user-profile.md#configure-the-fslogix-profile-container).

Bij het configureren van de registersleutel van bestand delen, gebruikt u de bestandsshare die u hebt gemaakt in [machtigingen configureren voor de bestandsserver](set-up-customize-master-image.md#configure-permissions-for-the-file-server) waar u van plan bent voor het opslaan van de profiel-containers. U kunt ook de functionaliteit van de FSLogix-container met behulp van deze testen [snelstartgids](https://docs.fslogix.com/display/20170529/Profile+Containers+-+Quick+Start).

### <a name="configure-windows-defender"></a>Windows Defender configureren

Als Windows Defender is geconfigureerd in de virtuele machine, zorg er dan voor dat deze heeft geconfigureerd voor niet-scan de volledige inhoud van de VHD en VHDX-bestanden tijdens het koppelen van hetzelfde.

Deze configuratie alleen verwijderd VHD en VHDX-bestanden worden gescand tijdens de bijlage, maar heeft geen invloed op realtime scannen.

Zie voor meer instructies voor het configureren van Windows Defender in Windows Server, [configureren van Windows Defender Antivirus uitsluitingen voor Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus).

Zie voor meer informatie over het configureren van bepaalde bestanden uitsluiten van scannen met Windows Defender, [configureren en valideren van uitsluitingen op basis van de extensie en mapnaam bestandslocatie](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-extension-file-exclusions-windows-defender-antivirus).

### <a name="configure-session-timeout-policies"></a>Configureren van beleid voor time-out van sessie

Externe sessie-beleid kunnen worden afgedwongen op het niveau van Groepsbeleid, omdat alle virtuele machines in een groep host deel van dezelfde beveiligingsgroep uitmaken.

Externe sessie-beleid configureren:

1. Navigeer naar **Beheersjablonen** > **Windows-onderdelen** > **extern bureaublad-Services**  >  **Extern bureaublad-sessiehost** > **sessie tijdslimieten**.
2. Selecteer in het deelvenster aan de rechterkant de **tijdslimiet voor actieve maar niet-actieve sessies van extern bureaublad-Services** beleid.
3. Nadat het modale venster wordt weergegeven, wijzigt u de beleidsoptie van **niet geconfigureerd** naar **ingeschakeld** aan het beleid te activeren.
4. In de vervolgkeuzelijst onder de optie voor de hoeveelheid tijd te instellen **4 uur**.

U kunt ook externe sessie-beleid handmatig configureren door het uitvoeren van de volgende opdrachten:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fResetBroken /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxConnectionTime /t REG_DWORD /d 600000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxDisconnectionTime /t REG_DWORD /d 5000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxIdleTime /t REG_DWORD /d 5000 /f
```

### <a name="set-up-time-zone-redirection"></a>Instellen van de tijdzone-omleiding

Tijdzone-omleiding kan worden afgedwongen op het niveau van Groepsbeleid, omdat alle virtuele machines in een groep host deel van dezelfde beveiligingsgroep uitmaken.

Om te leiden tijdzones:

1. Open op de Active Directory-server, de **Group Policy Management Console**.
2. Vouw uw domein en groepsbeleidsobjecten.
3. Met de rechtermuisknop op de **Group Policy Object** dat u hebt gemaakt voor de instellingen voor Groepsbeleid en selecteer **bewerken**.
4. In de **Editor voor Groepsbeleidsbeheer**, gaat u naar **Computerconfiguratie** > **beleid** > **Administrative Sjablonen** > **Windows-onderdelen** > **Horizon weergave RDSH Services** > **extern bureaublad-sessiehost Host** > **apparaat en Resource-omleiding**.
5. Schakel de **tijdzoneomleiding toestaan** instelling.

U kunt ook deze opdracht uitvoeren op de master-image tijdzones omleiden:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fEnableTimeZoneRedirection /t REG_DWORD /d 1 /f
```

### <a name="disable-storage-sense"></a>Opslaginzicht uitschakelen

Voor virtuele Windows-bureaublad-sessiehost die gebruikmaken van Windows 10 Enterprise of Windows 10 Enterprise meerdere sessies, raden wij Opslaginzicht uitschakelen. U kunt Opslaginzicht uitschakelen in het menu instellingen onder **opslag**, zoals weergegeven in de volgende schermafbeelding:

![Een schermafbeelding van het Storage-menu onder instellingen. De optie 'Opslaginzicht' is uitgeschakeld.](media/storagesense.png)

U kunt ook de instelling met het register wijzigen door het uitvoeren van de volgende opdracht uit:

```batch
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy /v 01 /t REG_DWORD /d 0 /f
```

### <a name="include-additional-language-support"></a>Ondersteuning voor extra talen opnemen

Dit artikel wordt niet beschreven hoe het configureren van taal en regionale ondersteuning. Raadpleeg voor meer informatie de volgende artikelen:

- [Talen toevoegen aan Windows-installatiekopieën](https://docs.microsoft.com/windows-hardware/manufacture/desktop/add-language-packs-to-windows)
- [Onderdelen op verzoek](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)
- [Taal en regio onderdelen op verzoek (DOM)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-language-fod)

### <a name="other-applications-and-registry-configuration"></a>Andere toepassingen en de Register-configuratie

In deze sectie bevat informatie over toepassings- en configuratie van besturingssysteem. Alle configuratie in deze sectie vindt plaats via registervermeldingen die kunnen worden uitgevoerd door de opdrachtregel en regedit hulpprogramma's.

>[!NOTE]
>Aanbevolen procedures kunt u implementeren in de configuratie met algemene, groepsbeleidsobjecten (GPO's) of invoer van register. De beheerder kan een van beide opties op basis van de behoeften van hun organisatie kiezen.

Voor feedback hub verzameling van telemetriegegevens op meerdere Windows 10 Enterprise-sessie, kunt u deze opdracht uitvoeren:

```batch
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection "AllowTelemetry"=dword:00000003
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection /v AllowTelemetry /d 3
```

Voer de volgende opdracht om op te lossen Watson crashes:

```batch
remove CorporateWerServer* from Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting
```

Voer de volgende opdrachten in de Register-editor om op te lossen 5 k resolutie ondersteuning. U kunt de opdrachten moet uitvoeren voordat u de side-by-side-stack kunt inschakelen.

```batch
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp]
"MaxMonitors"=dword:00000004
"MaxXResolution"=dword:00001400
"MaxYResolution"=dword:00000b40

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs]
"MaxMonitors"=dword:00000004
"MaxXResolution"=dword:00001400
"MaxYResolution"=dword:00000b40
```

## <a name="prepare-the-image-for-upload-to-azure"></a>De installatiekopie van het voorbereiden voor upload naar Azure

Nadat u hebt de configuratie voltooid en alle toepassingen geïnstalleerd, volgt u de instructies in [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) voorbereiden van de installatiekopie.

Na het voorbereiden van de afbeelding voor het uploaden, zorg ervoor dat de virtuele machine blijft in de status uitgeschakeld of toewijzing ongedaan gemaakt.

## <a name="upload-master-image-to-a-storage-account-in-azure"></a>Master-image te uploaden naar een opslagaccount in Azure

In deze sectie is alleen van toepassing wanneer de master-image lokaal is gemaakt.

De volgende instructies wordt uitgelegd hoe u een master-image te uploaden naar een Azure storage-account. Als u nog een Azure storage-account hebt, volgt u de instructies in [in dit artikel](https://code.visualstudio.com/tutorials/static-website/create-storage) een te maken.

1. Converteren naar vaste de VM-installatiekopie (VHD) als u dat nog niet gedaan hebt. Als u niet de installatiekopie van het converteren naar vaste, kunt u de installatiekopie kan niet met succes maken.

2. Upload de VHD naar een blobcontainer in uw storage-account. U kunt snel uploaden met de [Opslagverkenner hulpprogramma](https://azure.microsoft.com/features/storage-explorer/). Zie voor meer informatie over het hulpprogramma Storage Explorer, [in dit artikel](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows).

    ![Een schermafbeelding van Microsoft Azure Storage Explorer van het hulpprogramma zoekvenster. Het selectievakje '.vhd- of vhdx-bestanden uploaden als pagina-blobs (aanbevolen)' is ingeschakeld.](media/897aa9a9b6acc0aa775c31e7fd82df02.png)

3. Ga vervolgens naar de Azure-portal in uw browser en zoek naar 'Installatiekopieën'. Uw zoekopdracht moet leiden u de **afbeelding maken** pagina, zoals wordt weergegeven in de volgende schermafbeelding:

    ![Een schermafbeelding van de pagina van de installatiekopie maken van de Azure-portal, gevuld met voorbeelden van waarden voor de installatiekopie.](media/d3c840fe3e2430c8b9b1f44b27d2bf4f.png)

4. Als u de installatiekopie hebt gemaakt, ziet u een melding zoals in de volgende schermafbeelding:

    ![Een schermafbeelding van de melding 'installatiekopie is gemaakt'.](media/1f41b7192824a2950718a2b7bb9e9d69.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u een installatiekopie hebt, kunt u maken of bijwerken van de host van toepassingen. Zie de volgende artikelen voor meer informatie over het maken en bijwerken van de host van toepassingen:

- [Een host-pool maken met een Azure Resource Manager-sjabloon](create-host-pools-arm-template.md)
- [Zelfstudie: Een host-pool maken met Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Een host-pool maken met PowerShell](create-host-pools-powershell.md)
- [Instellen van een share van de gebruiker profiel voor een groep host](create-host-pools-user-profile.md)
- [Configureren van de taakverdelingsmethode voor een virtuele Windows-bureaublad](configure-host-pool-load-balancing.md)
