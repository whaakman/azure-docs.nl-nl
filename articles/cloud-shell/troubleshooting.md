---
title: Het oplossen van Azure Cloud Shell | Microsoft Docs
description: Oplossen van problemen met Azure Cloudshell
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: damaerte
ms.openlocfilehash: 089c623ff2c53a59c60c3fe1a53876c16a5353dd
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159013"
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Het oplossen van & beperkingen van Azure Cloud Shell

Bekende oplossingen voor het oplossen van problemen in Azure Cloud Shell zijn onder andere:

## <a name="general-troubleshooting"></a>Algemene probleemoplossing

### <a name="early-timeouts-in-firefox"></a>Vroege time-outs in FireFox

- **Details**: Cloud Shell gebruikmaakt van een open websocket invoer/uitvoer doorgeven aan de browser. FireFox is vooraf ingestelde beleidsregels die van de websocket voortijdig veroorzaakt vroege time-outs in Cloud Shell kunnen sluiten.
- **Resolutie**: Open FireFox en navigeert u naar ' over: config ' in het vak URL. Zoeken naar 'network.websocket.timeout.ping.request' en wijzig de waarde van 0 tot en met 10.

### <a name="disabling-cloud-shell-in-a-locked-down-network-environment"></a>Cloud Shell uitschakelen in een vergrendelde netwerk omgeving

- **Details**: beheerders mogelijk willen toegang tot Cloud Shell voor hun gebruikers uitschakelen. Cloudshell gebruikmaakt van toegang tot de `ux.console.azure.com` domein dat kan worden geweigerd, toegang heeft tot de Cloud Shell-toegangspunten, waaronder portal.azure.com, shell.azure.com, Visual Studio Code Azure-accountextensie en docs.microsoft.com stoppen.
- **Resolutie**: beperken van toegang tot `ux.console.azure.com` via instellingen om uw omgeving. Het pictogram van Cloud Shell wordt nog steeds aanwezig zijn in portal.azure.com, maar wordt geen verbinding maken met de service.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Opslag-dialoogvenster - fout: 403 RequestDisallowedByPolicy

- **Details**: bij het maken van een storage-account via de Cloud Shell, is mislukt vanwege een Azure policy geplaatst door uw beheerder. Foutbericht omvat: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Resolutie**: Neem contact op met uw Azure-beheerder als u wilt verwijderen of bijwerken van de Azure policy weigeren opslag maken.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Opslag-dialoogvenster - fout: 400 DisallowedOperation

- **Details**: wanneer u een Azure Active Directory-abonnement, kunt u opslag kan niet maken.
- **Resolutie**: een Azure-abonnement geschikt is voor het maken van de storage-resources gebruiken. Azure AD-abonnementen zijn niet in staat om Azure-resources te maken.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Terminal uitvoer - fout: kan geen verbinding maken terminal: websocket kan niet tot stand worden gebracht. Druk op `Enter` verbinding te maken.
- **Details**: Cloud Shell is de mogelijkheid tot stand brengen van een websocket-verbinding met de Cloud Shell-infrastructuur vereist.
- **Resolutie**: Controleer u de netwerkinstellingen van uw om in te schakelen verzenden https-aanvragen en websocket-aanvragen voor domeinen op hebt geconfigureerd *. console.azure.com.

### <a name="set-your-cloud-shell-connection-to-support-using-tls-12"></a>Instellen van uw Cloud Shell-verbinding met de ondersteuning voor het gebruik van TLS 1.2
 - **Details**: voor het definiëren van de TLS-versie voor de verbinding naar Cloud Shell, moet u specifieke browserinstellingen instellen.
 - **Resolutie**: Ga naar de beveiligingsinstellingen van uw browser en schakel het selectievakje naast 'Gebruik TLS 1.2'.

## <a name="bash-troubleshooting"></a>Bash-problemen oplossen

### <a name="cannot-run-the-docker-daemon"></a>De docker-daemon kan niet worden uitgevoerd.

- **Details**: Cloud Shell gebruikmaakt van een container voor het hosten van uw shell-omgeving, uitgevoerd als gevolg hiervan de daemon is niet toegestaan.
- **Resolutie**: gebruikmaken van [docker-machine](https://docs.docker.com/machine/overview/), die standaard voor het beheren van docker-containers vanaf een externe dockerhost is geïnstalleerd.

## <a name="powershell-troubleshooting"></a>Het oplossen van PowerShell

### <a name="gui-applications-are-not-supported"></a>GUI-toepassingen worden niet ondersteund

- **Details**: als een gebruiker wordt gestart met een GUI-toepassing, de prompt geen geretourneerd. Bijvoorbeeld, wanneer een kloon een persoonlijke GitHub-opslagplaats waarvoor tweeledige verificatie ingeschakeld, wordt een dialoogvenster weergegeven voor het voltooien van de verificatie met twee factoren.
- **Resolutie**: sluit en Open de shell.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Oplossen van problemen met extern beheer van virtuele Azure-machines

- **Details**: vanwege de standaard Windows Firewall-instellingen voor WinRM ziet de gebruiker mogelijk de volgende fout: `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Resolutie**: uitvoeren `Enable-AzureRmVMPSRemoting` om in te schakelen van alle aspecten van PowerShell voor externe toegang op de doelcomputer.

### <a name="dir-does-not-update-the-result-in-azure-drive"></a>`dir` het resultaat in de Azure-station niet bijgewerkt

- **Details**: standaard, om te optimaliseren voor de resultaten van de gebruikerservaring `dir` in de cache is opgeslagen in Azure-station.
- **Resolutie**: nadat u maken, bijwerken of verwijderen van een Azure-resource, voert u `dir -force` om bij te werken van de resultaten in de Azure-station.

## <a name="general-limitations"></a>Algemene beperkingen

Azure Cloud Shell bevat de volgende beperkingen:

### <a name="system-state-and-persistence"></a>Systeemstatus- en persistentie

De computer waarmee u uw Cloud Shell-sessie is tijdelijk en deze wordt gerecycled nadat uw sessie niet actief voor de 20 minuten is. Cloudshell is vereist voor een Azure-bestandsshare te koppelen. Uw abonnement moet als gevolg hiervan kunnen opslagresources instellen voor toegang tot Cloud Shell. Andere overwegingen zijn onder andere:

- Met gekoppelde opslag, alleen de wijzigingen in de `clouddrive` directory zijn opgeslagen. In Bash, uw `$HOME` directory ook worden opgeslagen.
- Azure-bestandsshares kunnen worden gekoppeld alleen vanuit uw [regio toegewezen](persisting-shell-storage.md#mount-a-new-clouddrive).
  - Voer in Bash, `env` te vinden van uw regio instellen als `ACC_LOCATION`.
- Azure Files ondersteunt alleen lokaal redundante opslag en geografisch redundante opslag met accounts.

### <a name="browser-support"></a>Browserondersteuning

Cloudshell biedt ondersteuning voor de nieuwste versies van de volgende browsers:

- Microsoft Edge
- Microsoft Internet Explorer
- Google Chrome
- Mozilla Firefox
- Apple Safari
  - Safari in de privémodus wordt niet ondersteund.

### <a name="copy-and-paste"></a>Kopiëren en plakken

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Voor een bepaalde gebruiker kan slechts één shell actief zijn

Gebruikers kunnen alleen starten één type shell op een tijdstip, ofwel **Bash** of **PowerShell**. U mogelijk echter meerdere exemplaren van de Bash- of PowerShell in één keer uitgevoerd. Wisselen tussen Bash of PowerShell oorzaken Cloud Shell te starten, waarmee bestaande sessies beëindigd.

### <a name="usage-limits"></a>Gebruiksbeperkingen

Cloudshell is bedoeld voor interactieve gebruiksvoorbeelden. Als gevolg hiervan, geen niet-interactieve langlopende-sessies worden beëindigd zonder waarschuwing.

### <a name="user-permissions"></a>Gebruikersmachtigingen

Machtigingen zijn ingesteld als normale gebruikers zonder toegang tot sudo. Elke installatie buiten uw `$Home` directory is niet persistent.

## <a name="bash-limitations"></a>Bash-beperkingen

### <a name="editing-bashrc"></a>.Bashrc bewerken

Nemen voorzichtig bij het bewerken van .bashrc, in dat geval kan onverwachte fouten veroorzaken in Cloud Shell.

## <a name="powershell-limitations"></a>PowerShell-beperkingen

### <a name="preview-version-of-azuread-module"></a>Preview-versie van de module AzureAD

Op dit moment `AzureAD.Standard.Preview`, een preview-versie van .NET Standard-based, module beschikbaar is. Deze module biedt dezelfde functionaliteit als `AzureAD`.

### <a name="sqlserver-module-functionality"></a>`SqlServer` modulefunctionaliteit

De `SqlServer` opgenomen in de Cloud Shell-module bevat alleen prerelease ondersteuning voor PowerShell Core. In het bijzonder `Invoke-SqlCmd` is nog niet beschikbaar.

### <a name="default-file-location-when-created-from-azure-drive"></a>Standaardlocatie wanneer gemaakt op basis van Azure-station

Met behulp van PowerShell-cmdlets, kunnen geen gebruikers bestanden in de Azure-station maken. Wanneer gebruikers nieuwe bestanden met andere hulpprogramma's, zoals vim of nano, maakt de bestanden worden opgeslagen in de `$HOME` standaard.

### <a name="commands-that-create-gui-pop-ups-are-not-supported"></a>Opdrachten die GUI pop-ups maakt worden niet ondersteund

Als de gebruiker wordt uitgevoerd een opdracht die u een Windows-dialoogvenster, zoals maakt `Connect-AzureAD` of `Connect-AzureRmAccount`, een foutmelding zoals ziet: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="tab-completion-can-throw-psreadline-exception"></a>Tab-Aanvulling kunt PSReadline uitzondering genereren

Als de gebruiker PSReadline EditMode is ingesteld op Emacs, de gebruiker probeert om weer te geven van alle mogelijkheden via de tab-aanvulling, en de-venstergrootte is te klein om weer te geven van alle mogelijkheden, PSReadline niet-verwerkte uitzondering genereert.

### <a name="large-gap-after-displaying-progress-bar"></a>Wijd nadat de voortgangsbalk weergegeven

Als een opdracht of een gebruiker actie een voortgangsbalk, zoals een tabblad wordt voltooien tijdens het in de `Azure:` station, dan is het mogelijk dat de cursor is niet juist ingesteld en een onderbreking wordt weergegeven wanneer de voortgangsbalk is eerder.

### <a name="random-characters-appear-inline"></a>Willekeurige tekens inline worden weergegeven

De volgorde van de positie van cursor codes, bijvoorbeeld `5;13R`, kan worden weergegeven in de invoer van de gebruiker. De tekens kunnen handmatig worden verwijderd.

## <a name="personal-data-in-cloud-shell"></a>Persoonlijke gegevens in de Cloud Shell

Azure Cloud Shell hecht waarde aan uw persoonlijke gegevens, de gegevens vastgelegd en opgeslagen door de Azure Cloud Shell-service worden gebruikt om de standaardwaarden voor uw ervaring bieden, zoals uw meest recent gebruikte shell, voorkeur tekengrootte voorkeur lettertype en bestand details delen die cloud terug station. Moet u wilt exporteren of verwijderen van deze gegevens, gebruik de volgende instructies.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="export"></a>Exporteren
Om **exporteren** de gebruikersinstellingen Cloud Shell u bijvoorbeeld bespaart de voorkeur shell, tekengrootte en voer de volgende opdrachten uit het lettertype.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Open Azure Cloudshell")](https://shell.azure.com)
2. Voer de volgende opdrachten in Bash of PowerShell:

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
  ```

PowerShell:

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  ((Invoke-WebRequest -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}).Content | ConvertFrom-Json).properties | Format-List
```

### <a name="delete"></a>Verwijderen
Om **verwijderen** uw gebruikersinstellingen Cloud Shell u bijvoorbeeld bespaart de voorkeur shell, tekengrootte en voer de volgende opdrachten uit het lettertype. De volgende keer dat u Cloud Shell start wordt u gevraagd om te onboarden een bestandsshare opnieuw. 

>[!Note]
> Als u de instellingen van de gebruiker verwijdert, wordt de werkelijke Azure Files-share niet verwijderd. Ga naar uw Azure-bestanden om deze actie te voltooien.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Open Azure Cloudshell")](https://shell.azure.com)
2. Voer de volgende opdrachten in Bash of PowerShell:

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
  ```

PowerShell:

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  Invoke-WebRequest -Method Delete -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}
  ```
