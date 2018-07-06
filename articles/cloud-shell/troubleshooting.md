---
title: Het oplossen van Azure Cloud Shell | Microsoft Docs
description: Oplossen van problemen met Azure Cloudshell
services: azure
documentationcenter: ''
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: damaerte
ms.openlocfilehash: 21bc0633a9cc607325b48998791cb12631ecd0d7
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856484"
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Het oplossen van & beperkingen van Azure Cloud Shell

Bekende oplossingen voor het oplossen van problemen in Azure Cloud Shell zijn onder andere:

## <a name="general-troubleshooting"></a>Algemene probleemoplossing

### <a name="early-timeouts-in-firefox"></a>Vroege time-outs in FireFox
- **Details**: Cloud Shell gebruikmaakt van een open websocket invoer/uitvoer doorgeven aan de browser. FireFox is vooraf ingestelde beleidsregels die van de websocket voortijdig veroorzaakt vroege time-outs in Cloud Shell kunnen sluiten.
- **Resolutie**: Open FireFox en navigeert u naar ' over: config ' in het vak URL. Zoeken naar 'network.websocket.timeout.ping.request' en wijzig de waarde van 0 tot en met 10.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Opslag-dialoogvenster - fout: 403 RequestDisallowedByPolicy
- **Details**: bij het maken van een storage-account via de Cloud Shell, is mislukt vanwege een Azure policy geplaatst door uw beheerder. Foutbericht omvat: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Resolutie**: Neem contact op met uw Azure-beheerder als u wilt verwijderen of bijwerken van de Azure policy weigeren opslag maken.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Opslag-dialoogvenster - fout: 400 DisallowedOperation
 - **Details**: wanneer u een Azure Active Directory-abonnement, kunt u opslag kan niet maken.
 - **Resolutie**: een Azure-abonnement geschikt is voor het maken van de storage-resources gebruiken. Azure AD-abonnementen zijn niet in staat om Azure-resources te maken.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Terminal uitvoer - fout: kan geen verbinding maken terminal: websocket kan niet tot stand worden gebracht. Druk op `Enter` verbinding te maken.
 - **Details**: Cloud Shell is de mogelijkheid tot stand brengen van een websocket-verbinding met de Cloud Shell-infrastructuur vereist.
 - **Resolutie**: Controleer u de netwerkinstellingen van uw om in te schakelen verzenden https-aanvragen en websocket-aanvragen voor domeinen op hebt geconfigureerd *. console.azure.com.

## <a name="bash-troubleshooting"></a>Bash-problemen oplossen

### <a name="cannot-run-the-docker-daemon"></a>De docker-daemon kan niet worden uitgevoerd.

- **Details**: Cloud Shell gebruikmaakt van een container voor het hosten van uw shell-omgeving, uitgevoerd als gevolg hiervan de daemon is niet toegestaan.
- **Resolutie**: gebruikmaken van [docker-machine](https://docs.docker.com/machine/overview/), die standaard voor het beheren van docker-containers vanaf een externe dockerhost is geïnstalleerd.

## <a name="powershell-troubleshooting"></a>Het oplossen van PowerShell

### <a name="gui-applications-are-not-supported"></a>GUI-toepassingen worden niet ondersteund

- **Details**: als een gebruiker een GUI-app start, retourneert de prompt geen. Bijvoorbeeld, wanneer een gebruiker een persoonlijke GitHub-opslagplaats die tweeledige verificatie ingeschakeld is kloont, wordt een dialoogvenster weergegeven voor het voltooien van de verificatie met twee factoren.  
- **Resolutie**: sluit en Open de shell.

### <a name="get-help--online-does-not-open-the-help-page"></a>Get-Help - online de help-pagina niet geopend

- **Details**: als een gebruiker `Get-Help Find-Module -online`, een foutmelding zoals ziet: `Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Resolutie**: Kopieer de url en het handmatig openen in uw browser.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Oplossen van problemen met extern beheer van virtuele Azure-machines

- **Details**: vanwege de standaard Windows Firewall-instellingen voor WinRM ziet de gebruiker mogelijk de volgende fout: `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Resolutie**: uitvoeren `Enable-AzureRmVMPSRemoting` om in te schakelen van alle aspecten van PowerShell voor externe toegang op de doelcomputer.
 

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir` het resultaat in de Azure-station in de cache opslaat

- **Details**: het resultaat van `dir` in de cache is opgeslagen in Azure-station.
- **Resolutie**: nadat u maakt of een resource in de weergave van de Azure-station te verwijderen, voert u `dir -force` om bij te werken.

## <a name="general-limitations"></a>Algemene beperkingen
Azure Cloud Shell bevat de volgende beperkingen:

### <a name="system-state-and-persistence"></a>Systeemstatus- en persistentie

De computer waarmee u uw Cloud Shell-sessie is tijdelijk en deze wordt gerecycled nadat uw sessie niet actief voor de 20 minuten is. Cloudshell is vereist voor een Azure-bestandsshare te koppelen. Uw abonnement moet als gevolg hiervan kunnen opslagresources instellen voor toegang tot Cloud Shell. Andere overwegingen zijn onder andere:

* Met gekoppelde opslag, alleen de wijzigingen in de `clouddrive` directory zijn opgeslagen. In Bash, uw `$Home` directory ook worden opgeslagen.
* Azure-bestandsshares kunnen worden gekoppeld alleen vanuit uw [regio toegewezen](persisting-shell-storage.md#mount-a-new-clouddrive).
  * Voer in Bash, `env` te vinden van uw regio instellen als `ACC_LOCATION`.
* Azure Files ondersteunt alleen lokaal redundante opslag en geografisch redundante opslag met accounts.

### <a name="browser-support"></a>Browserondersteuning

Cloudshell biedt ondersteuning voor de nieuwste versies van Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox en Apple Safari. Safari in de privémodus wordt niet ondersteund.

### <a name="copy-and-paste"></a>Kopiëren en plakken

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Voor een bepaalde gebruiker kan slechts één shell actief zijn

Gebruikers kunnen alleen starten één type shell op een tijdstip, ofwel **Bash** of **PowerShell**. U mogelijk echter meerdere exemplaren van de Bash- of PowerShell in één keer uitgevoerd. Wisselen tussen Bash of PowerShell oorzaken Cloud Shell te starten, waarmee bestaande sessies beëindigd.

### <a name="usage-limits"></a>Gebruiksbeperkingen

Cloudshell is bedoeld voor interactieve gebruiksvoorbeelden. Als gevolg hiervan, geen niet-interactieve langlopende-sessies worden beëindigd zonder waarschuwing.

## <a name="bash-limitations"></a>Bash-beperkingen

### <a name="user-permissions"></a>Gebruikersmachtigingen

Machtigingen zijn ingesteld als normale gebruikers zonder toegang tot sudo. Elke installatie buiten uw `$Home` directory is niet persistent.

### <a name="editing-bashrc"></a>.Bashrc bewerken

Nemen voorzichtig bij het bewerken van .bashrc, in dat geval kan onverwachte fouten veroorzaken in Cloud Shell.

## <a name="powershell-limitations"></a>PowerShell-beperkingen

### <a name="azuread-module-name"></a>`AzureAD` modulenaam

De `AzureAD` modulenaam is momenteel `AzureAD.Standard.Preview`, de module biedt dezelfde functionaliteit.

### <a name="sqlserver-module-functionality"></a>`SqlServer` modulefunctionaliteit

De `SqlServer` opgenomen in de Cloud Shell-module bevat alleen prerelease ondersteuning voor PowerShell Core. In het bijzonder `Invoke-SqlCmd` is nog niet beschikbaar.

### <a name="default-file-location-when-created-from-azure-drive"></a>Standaardlocatie wanneer gemaakt op basis van Azure-station:

Met behulp van PowerShell-cmdlets, kunnen gebruikers niet maken in de Azure-station bestanden. Wanneer gebruikers nieuwe bestanden met andere hulpprogramma's, zoals vim of nano, maakt de bestanden worden opgeslagen in de `$HOME` standaard. 

### <a name="gui-applications-are-not-supported"></a>GUI-toepassingen worden niet ondersteund

Als de gebruiker wordt uitgevoerd een opdracht die u een Windows-dialoogvenster, zoals maakt `Connect-AzureAD` of `Connect-AzureRmAccount`, een foutmelding zoals ziet: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="tab-completion-crashes-psreadline"></a>Tab-Aanvulling PSReadline loopt vast

Als de gebruiker EditMode in PSReadline is ingesteld op Emacs, de gebruiker probeert om weer te geven van alle mogelijkheden via de tab-aanvulling, en de-venstergrootte is te klein om weer te geven van alle mogelijkheden, PSReadline loopt vast.

### <a name="large-gap-after-displaying-progress-bar"></a>Wijd nadat de voortgangsbalk weergegeven

Als de gebruiker een actie uitvoert die wordt weergegeven een voortgangsbalk, zoals een tabblad voltooien tijdens het in de `Azure:` station, dan is het mogelijk dat de cursor is niet juist ingesteld en een onderbreking wordt weergegeven wanneer de voortgangsbalk is eerder.

### <a name="random-characters-appear-inline"></a>Willekeurige tekens inline worden weergegeven

De volgorde van de positie van cursor codes, bijvoorbeeld `5;13R`, kan worden weergegeven in de invoer van de gebruiker.  De tekens kunnen handmatig worden verwijderd.

## <a name="personal-data-in-cloud-shell"></a>Persoonlijke gegevens in de Cloud Shell

Azure Cloud Shell hecht waarde aan uw persoonlijke gegevens, de gegevens vastgelegd en opgeslagen door de Azure Cloud Shell-service worden gebruikt om de standaardwaarden voor uw ervaring bieden, zoals uw meest recent gebruikte shell, voorkeur tekengrootte voorkeur lettertype en bestand details delen die cloud terug station. Moet u wilt exporteren of verwijderen van deze gegevens, hebben we de volgende instructies opgenomen.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="export"></a>Exporteren
Om **exporteren** de gebruikersinstellingen Cloud Shell u bijvoorbeeld bespaart de voorkeur shell, tekengrootte en voer de volgende opdrachten uit het lettertype.

1. Bash in Cloudshell starten
2. Voer de volgende opdrachten uit:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
```

### <a name="delete"></a>Verwijderen
Om **verwijderen** uw gebruikersinstellingen Cloud Shell u bijvoorbeeld bespaart de voorkeur shell, tekengrootte en voer de volgende opdrachten uit het lettertype. De volgende keer dat u Cloud Shell start wordt u gevraagd om te onboarden een bestandsshare opnieuw. 

De werkelijke Azure Files share worden niet verwijderd als u uw instellingen voor gebruikers, gaat u naar Azure Files om deze actie te voltooien.

1. Bash in Cloudshell starten
2. Voer de volgende opdrachten uit:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
```
