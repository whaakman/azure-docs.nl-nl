---
title: Office te installeren op een master VHD-installatiekopie - Azure
description: Het installeren en aanpassen van Office op een virtuele Windows-bureaublad preview master-installatiekopie naar Azure.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/02/2019
ms.author: v-chjenk
ms.openlocfilehash: cb9edbb508ddd993dcefbf69eb06b4f0d4156485
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66742552"
---
# <a name="install-office-on-a-master-vhd-image"></a>Office installeren op een master-VHD-installatiekopie

In dit artikel wordt uitgelegd hoe u Office 365 ProPlus, OneDrive en andere veelgebruikte toepassingen installeren op een installatiekopie van het model virtuele harde schijf (VHD) voor het uploaden naar Azure. Als uw gebruikers nodig hebben voor toegang tot bepaalde line-of-business (LOB)-toepassingen, raden wij dat u deze installeren na het voltooien van de instructies in dit artikel.

In dit artikel wordt ervan uitgegaan dat u al een virtuele machine (VM) hebt gemaakt. Als dit niet het geval is, Zie [voorbereiden en aanpassen van een master VHD-installatiekopie](set-up-customize-master-image.md#create-a-vm)

In dit artikel ook wordt ervan uitgegaan dat u hebt met verhoogde bevoegdheden voor toegang op de virtuele machine, of deze ingericht in Azure of Hyper-V-beheer. Als dit niet het geval is, Zie [toegangsrechten voor het beheren van alle Azure-abonnement en beheer van groepen](https://docs.microsoft.com/azure/role-based-access-control/elevate-access-global-admin).

>[!NOTE]
>Deze instructies zijn voor een virtuele Windows-bureaublad Preview-specifieke configuratie die kan worden gebruikt met de bestaande processen van uw organisatie.

## <a name="install-office-in-shared-computer-activation-mode"></a>Office installeren in de modus voor activering van gedeelde computers

Activering van gedeelde computers kunt u Office 365 ProPlus implementeren op een computer in uw organisatie die wordt gebruikt door meerdere gebruikers. Zie voor meer informatie over activering van gedeelde computers [overzicht van de activering van gedeelde computers voor Office 365 ProPlus](https://docs.microsoft.com/DeployOffice/overview-of-shared-computer-activation-for-office-365-proplus).

Gebruik de [Office Deployment Tool](https://www.microsoft.com/download/details.aspx?id=49117) Office te installeren. Meerdere sessies van Windows 10 Enterprise biedt alleen ondersteuning voor de volgende versies van Office:
- Office 365 ProPlus
- Office 365 Business die wordt geleverd met een abonnement op Microsoft 365 Business

De Office Deployment Tool is een XML-configuratiebestand vereist. Voor het aanpassen van het volgende voorbeeld, Zie de [configuratieopties voor de Office Deployment Tool](https://docs.microsoft.com/deployoffice/configuration-options-for-the-office-2016-deployment-tool).

De voorbeeldconfiguratie-XML zijn er beschikbaar gesteld voor dit wordt de volgende dingen doen:

- Office installeren vanaf het Insiders-kanaal en updates van het kanaal Insiders leveren wanneer ze worden uitgevoerd.
- Gebruik de x64-architectuur.
- Automatische updates uitschakelen.
- Visio en Project installeren.
- Verwijderen van eventuele bestaande installaties van Office en hun instellingen worden gemigreerd.
- Activering van gedeelde computers inschakelen.

>[!NOTE]
>De zoekfunctie stencil in Visio werkt niet in virtuele Windows-bureaublad tijdens de preview-configuratie.

Dit is wat deze voorbeeldconfiguratie XML wordt niet doen:

- Installeren van Skype voor bedrijven
- OneDrive installeren in de modus per gebruiker. Zie voor meer informatie, [OneDrive installeren in de modus per computer](#install-onedrive-in-per-machine-mode).

>[!NOTE]
>Activering van gedeelde computers kan worden ingesteld via groepsbeleidsobjecten (GPO's) of registerinstellingen. Het groepsbeleidsobject bevindt zich in **Computerconfiguratie\\beleid\\Beheersjablonen\\Microsoft Office 2016 (Machine)\\licentie-instellingen**

De Office Deployment Tool bevat setup.exe. Voer de volgende opdracht in een opdrachtregel voor het installeren van Office:

```batch
Setup.exe /configure configuration.xml
```

#### <a name="sample-configurationxml"></a>Voorbeeld configuration.xml

De volgende XML-voorbeeld wordt de Insiders-versie installeren.

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

## <a name="install-onedrive-in-per-machine-mode"></a>OneDrive installeren in de modus per computer

OneDrive is normaal gesproken geïnstalleerd per gebruiker. In deze omgeving moet per computer geïnstalleerd.

Dit is het installeren van OneDrive in de modus per computer:

1. Maak eerst een locatie voor het voorbereiden van het installatieprogramma OneDrive. Map van een lokale schijf of [\\\\unc] (file://unc) locatie is niets mis mee.

2. OneDriveSetup.exe downloaden naar uw gefaseerde locatie met deze koppeling: <https://aka.ms/OneDriveWVD-Installer>

3. Als u office met OneDrive geïnstalleerd zonder  **\<ExcludeApp-ID = "OneDrive" /\>** , eventuele bestaande installaties van de OneDrive-per-gebruiker vanaf een opdrachtprompt met verhoogde bevoegdheid verwijderen door het uitvoeren van de volgende opdracht:
    
    ```batch
    "[staged location]\OneDriveSetup.exe" /uninstall
    ```

4. Deze opdracht uitvoert vanaf een opdrachtprompt met verhoogde bevoegdheid om in te stellen de **AllUsersInstall** registerwaarde:

    ```batch
    REG ADD "HKLM\Software\Microsoft\OneDrive" /v "AllUsersInstall" /t REG_DWORD /d 1 /reg:64
    ```

5. Met deze opdracht voor het installeren van OneDrive in de modus per computer worden uitgevoerd:

    ```batch
    Run "[staged location]\OneDriveSetup.exe" /allusers
    ```

6. Voer deze opdracht voor het configureren van OneDrive om te beginnen bij aanmelding voor alle gebruikers:

    ```batch
    REG ADD "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v OneDrive /t REG_SZ /d "C:\Program Files (x86)\Microsoft OneDrive\OneDrive.exe /background" /f
    ```

7. Schakel **gebruikersaccount op de achtergrond configureren** door het uitvoeren van de volgende opdracht uit.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "SilentAccountConfig" /t REG_DWORD /d 1 /f
    ```

8. Omleidings- en Windows-mappen in OneDrive bekend met de volgende opdracht verplaatsen.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "KFMSilentOptIn" /t REG_SZ /d "<your-AzureAdTenantId>" /f
    ```

## <a name="teams-and-skype"></a>Teams en Skype

Virtuele Windows-bureaublad biedt geen ondersteuning voor Skype voor bedrijven en Teams.

## <a name="next-steps"></a>Volgende stappen

Nu u Office aan de installatiekopie hebt toegevoegd, kunt u verder aanpassen van uw hoofd VHD-installatiekopie. Zie [voorbereiden en aanpassen van een master VHD-installatiekopie](set-up-customize-master-image.md).
