---
title: Probleemoplossing voor Azure Cloud Shell | Microsoft Docs
description: Azure-Cloud-Shell probleemoplossing
services: azure
documentationcenter: 
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: damaerte
ms.openlocfilehash: ca11a0db4cdb435aef26e7ae214cca24679c6ea1
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshooting-azure-cloud-shell"></a>Azure-Cloud-Shell probleemoplossing

Bekende oplossingen voor problemen in de Azure-Cloud-Shell zijn onder andere:

## <a name="general-resolutions"></a>Algemene oplossingen

### <a name="early-timeouts-in-firefox"></a>Vroege time-outs in FireFox
- **Details**: Cloud Shell maakt gebruik van een openstaande websocket input/output doorgeven aan de browser. FireFox heeft een vooraf ingestelde beleidsregels die de websocket is voortijdig waardoor vroegtijdig time-outs in de Cloud-Shell kunnen sluiten.
- **Resolutie**: Open FireFox en navigeert u naar ' over: config ' in het vak URL. Zoek naar 'network.websocket.timeout.ping.request' en wijzig de waarde van 0 tot en met 10.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Opslag-dialoogvenster - fout: 403 RequestDisallowedByPolicy
- **Details**: bij het maken van een opslagaccount via Cloud-Shell, is het mislukt als gevolg van een Azure-beleid geplaatst door uw beheerder. Foutbericht omvatten:`The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Resolutie**: Neem contact op met uw Azure-beheerder om te verwijderen of bijwerken van het Azure-beleid voor het weigeren van het maken van opslag.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Opslag-dialoogvenster - fout: 400 DisallowedOperation
 - **Details**: wanneer u een abonnement op Azure Active Directory, kunt u opslag kan niet maken.
 - **Resolutie**: een Azure-abonnement te maken van de storage-resources gebruiken. Azure AD-abonnementen kunnen geen Azure-resources te maken.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Terminal output - fout: kan geen verbinding maken terminal: websocket kan niet worden vastgesteld. Druk op `Enter` opnieuw verbinding te maken.
 - **Details**: Cloud Shell is de mogelijkheid tot stand brengen van een websocket-verbinding met de Cloud Shell-infrastructuur vereist.
 - **Resolutie**: Controleer u de netwerkinstellingen zodat verzenden https-aanvragen en websocket-aanvragen voor domeinen op hebt geconfigureerd *. console.azure.com.

## <a name="bash-resolutions"></a>Bash-oplossingen

### <a name="cannot-run-az-login"></a>Az aanmelding kan niet worden uitgevoerd.

- **Details**: uitgevoerd `az login` werkt niet als u al onder het account dat wordt gebruikt voor aanmelding bij Cloud-Shell of Azure portal zijn geverifieerd.
- **Resolutie**: gebruikmaken van je account aanmelden of afmelden en verifiëren met uw Azure-account van de beoogde gebruikt.

### <a name="cannot-run-the-docker-daemon"></a>De docker-daemon kan niet worden uitgevoerd.

- **Details**: een container voor het hosten van uw shell-omgeving maakt gebruik van Cloud-Shell, uitgevoerd als gevolg hiervan de daemon is niet toegestaan.
- **Resolutie**: gebruikmaken van [docker-machine](https://docs.docker.com/machine/overview/), dat standaard docker-containers beheren vanaf een externe Docker-host is geïnstalleerd.

## <a name="powershell-resolutions"></a>PowerShell-oplossingen

### <a name="no-home-directory-persistence"></a>No $Home directory persistentie

- **Details**: alle gegevens die toepassing (bijvoorbeeld: git, vim en andere) schrijft naar `$Home` is niet persistent via PowerShell-sessies.
- **Resolutie**: In uw profiel PowerShell een symbolische koppeling maken naar specifieke map van de toepassing in `clouddrive` naar $Home.

### <a name="ctrlc-doesnt-exit-out-of-a-cmdlet-prompt"></a>CTRL + C sluiten buiten een prompt van de Cmdlet niet

- **Details**: bij een poging om af te sluiten van een prompt Cmdlet `Ctrl+C` achter de opdrachtprompt niet bestaat.
- **Resolutie**: druk achter de opdrachtprompt om af te sluiten op `Ctrl+C` vervolgens `Enter`.

### <a name="gui-applications-are-not-supported"></a>GUI-toepassingen worden niet ondersteund

- **Details**: als een gebruiker een GUI-app is gestart, de prompt geen waarde retourneert. Bijvoorbeeld: wanneer een gebruiker wordt gekloond van een persoonlijke GitHub-repo tweeledige verificatie ingeschakeld is, wordt een dialoogvenster weergegeven voor het voltooien van de verificatie met twee factoren.  
- **Resolutie**: sluit en Open de shell.


### <a name="get-help--online-does-not-open-the-help-page"></a>Get-Help - online help-pagina niet geopend

- **Details**: als een gebruiker `Get-Help Find-Module -online`, een foutbericht zoals ziet:`Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Resolutie**: Kopieer de url en het handmatig openen in uw browser.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Het oplossen van extern beheer van Azure Virtual machines

- **Details**: vanwege de standaard Windows Firewall-instellingen voor WinRM mogelijk ziet u de gebruiker de volgende fout:`Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Resolutie**: Zorg ervoor dat uw virtuele machine wordt uitgevoerd. U kunt uitvoeren `Get-AzureRmVM -Status` om erachter te komen de VM-Status.  Vervolgens voegt u een nieuwe firewallregel op de externe VM naar WinRM verbindingen toestaan vanaf een subnet, bijvoorbeeld:

 ``` Powershell
 New-NetFirewallRule -Name 'WINRM-HTTP-In-TCP-PSCloudShell' -Group 'Windows Remote Management' -Enabled True -Protocol TCP -LocalPort 5985 -Direction Inbound -Action Allow -DisplayName 'Windows Remote Management - PSCloud (HTTP-In)' -Profile Public
 ```
 U kunt [extensie voor aangepaste scripts Azure](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-customscript) om te voorkomen dat aanmelding voor uw externe VM voor het toevoegen van de nieuwe firewallregel.
 U kunt dit script opslaan naar een bestand spreek `addfirerule.ps1`, en dit uploaden naar uw Azure storage-container.
 Probeer de volgende opdracht:

 ``` Powershell
 Get-AzureRmVM -Name MyVM1 -ResourceGroupName MyResourceGroup | Set-AzureRmVMCustomScriptExtension -VMName MyVM1 -FileUri https://mystorageaccount.blob.core.windows.net/mycontainer/addfirerule.ps1 -Run 'addfirerule.ps1' -Name myextension
 ```

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir`het resultaat in Azure station caches

- **Details**: het resultaat van `dir` in cache wordt opgeslagen in Azure-station.
- **Resolutie**: nadat u maakt of een resource in de weergave van de Azure-station te verwijderen, voert u `dir -force` om bij te werken.
