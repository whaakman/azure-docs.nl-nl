---
title: Verbinding met extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van PowerShell
description: Uw azure-cloud service-toepassing met behulp van PowerShell om toe te staan van verbindingen met extern bureaublad configureren
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: bf2f70a4-20dc-4302-a91a-38cd7a2baa62
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeconnoc
ms.openlocfilehash: 1b1aa8105ab90b0016863f0bf3c47f6aa815d3e7
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001031"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Verbinding met extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Extern bureaublad kunt u toegang tot het bureaublad van een functie die wordt uitgevoerd in Azure. U kunt een extern bureaublad-verbinding oplossen en diagnosticeren van problemen met uw toepassing terwijl deze wordt uitgevoerd.

In dit artikel wordt beschreven hoe u extern bureaublad inschakelen op uw Cloud Service-functies met behulp van PowerShell. Zie [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview) voor de vereisten voor dit artikel. PowerShell maakt gebruik van de extern bureaublad-extensie, zodat u extern bureaublad inschakelen kunt nadat de toepassing is geïmplementeerd.

## <a name="configure-remote-desktop-from-powershell"></a>Extern bureaublad vanuit PowerShell configureren

De [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet kunt u extern bureaublad inschakelen op de opgegeven rollen of alle functies van uw cloud service-implementatie. De cmdlet kunt u de gebruikersnaam en wachtwoord opgeven voor de extern bureaublad-gebruiker via de *referentie* parameter een PSCredential-object accepteert.

Als u PowerShell interactief gebruikt, kunt u eenvoudig de PSCredential-object instellen door het aanroepen van de [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

```
$remoteusercredentials = Get-Credential
```

Deze opdracht wordt een dialoogvenster waarin u de gebruikersnaam en wachtwoord invoeren voor de externe gebruiker op een veilige manier weergegeven.

Omdat PowerShell bij het automation-scenario's helpt, kunt u ook instellen de **PSCredential** object op een manier die geen tussenkomst van de gebruiker vereist. Eerst moet u voor het instellen van een beveiligd wachtwoord. U begint met het opgeven van een wachtwoord als tekst zonder opmaak converteren naar een beveiligde tekenreeks met [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx). Vervolgens moet u een gecodeerde tekenreeks voor standard met behulp van deze beveiligde tekenreeks converteren [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx). Nu kunt u deze versleutelde standaardtekenreeks opslaan naar een bestand met [Set-inhoud](https://technet.microsoft.com/library/ee176959.aspx).

U kunt ook een bestand beveiligd-wachtwoordverificatie maken zodat u niet hoeft te typen in het wachtwoord van elke keer. Een bestand beveiligd-wachtwoordverificatie is ook beter dan een bestand met tekst zonder opmaak. Gebruik de volgende PowerShell om een beveiligd-wachtwoordverificatie-bestand te maken:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> Als u het wachtwoord instelt, zorg ervoor dat u voldoet aan de [complexiteitsvereisten](https://technet.microsoft.com/library/cc786468.aspx).

Voor het maken van het referentieobject uit het bestand beveiligd-wachtwoordverificatie, moet u de inhoud van het bestand lezen en terug te converteren naar een beveiligde tekenreeks met [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).

De [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet accepteert ook een *verlopen* parameter, die Hiermee geeft u een **datum-/** aan het gebruikersaccount dat is verlopen. U kunt bijvoorbeeld het account verloopt binnenkort een paar dagen vanaf de huidige datum en tijd instellen.

Dit PowerShell-voorbeeld laat zien hoe de extern bureaublad-extensie op een cloudservice instellen:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
U kunt eventueel ook de implementatiesleuf en functies die u inschakelen van extern bureaublad wilt op opgeven. Als deze parameters niet zijn opgegeven, de cmdlet schakelt u extern bureaublad op alle rollen in de **productie** implementatiesite.

De extern bureaublad-extensie is gekoppeld aan een implementatie. Als u een nieuwe implementatie voor de service maakt, moet u extern bureaublad inschakelen op deze implementatie. Als u altijd hebben van extern bureaublad is ingeschakeld wilt, klikt u vervolgens u moet rekening houden met de PowerShell-scripts integreren in uw werkstroom-implementatie.

## <a name="remote-desktop-into-a-role-instance"></a>Extern bureaublad in een rolinstantie

De [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet wordt gebruikt voor het externe bureaublad naar een specifieke rol exemplaar van uw cloudservice. U kunt de *LocalPath* parameter voor het downloaden van de RDP-bestand lokaal. Of u kunt de *starten* parameter voor het dialoogvenster verbinding met extern bureaublad voor toegang tot de cloudservicerolinstantie rechtstreeks te starten.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```

## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Controleer of de extern bureaublad-extensie is ingeschakeld op een service

De [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet geeft weer die extern bureaublad is ingeschakeld of uitgeschakeld op een service-implementatie. De cmdlet retourneert de gebruikersnaam voor de extern bureaublad-gebruiker en de functies die voor de extern bureaublad-uitbreiding is ingeschakeld. Dit gebeurt op de implementatiesleuf standaard, en u kunt in plaats daarvan de staging-site gebruiken.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Extern bureaublad-extensie verwijderen uit een service

Als u de extern bureaublad-extensie op een implementatie al hebt ingeschakeld en moet de instellingen voor extern bureaublad, moet u eerst de extensie verwijderen. En deze opnieuw inschakelen met de nieuwe instellingen. Bijvoorbeeld, als u wilt een nieuw wachtwoord voor het externe gebruikersaccount instellen of het account is verlopen. Dit is vereist op bestaande implementaties waarvoor het externe bureaublad uitbreiding is ingeschakeld. Voor nieuwe implementaties kunt u de extensie gewoon rechtstreeks toepassen.

Als u wilt de extern bureaublad-extensie verwijderen uit de implementatie, kunt u de [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet. U kunt eventueel ook de implementatiesleuf en de rol van waaruit u wilt verwijderen van de extern bureaublad-extensie opgeven.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> Als u wilt de configuratie van de extensie volledig verwijderen, moet u aanroepen de *verwijderen* cmdlet met de **UninstallConfiguration** parameter.
>
> De **UninstallConfiguration** parameter Hiermee verwijdert u een configuratie van de extensie die wordt toegepast op de service. Elke configuratie van de extensie is gekoppeld aan de configuratie van de service. Aanroepen van de *verwijderen* cmdlet zonder **UninstallConfiguration** verbroken de <mark>implementatie</mark> van de configuratie van de extensie, dus effectief verwijderen van de de extensie. De configuratie van de extensie blijft echter gekoppeld aan de service.

## <a name="additional-resources"></a>Aanvullende resources

[Cloud Services configureren](cloud-services-how-to-configure-portal.md)
