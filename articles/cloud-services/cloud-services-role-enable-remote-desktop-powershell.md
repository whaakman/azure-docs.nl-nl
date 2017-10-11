---
title: Verbinding met extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van PowerShell
description: Het configureren van uw azure-cloud-servicetoepassing extern bureaublad-verbindingen toestaan met behulp van PowerShell
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: bf2f70a4-20dc-4302-a91a-38cd7a2baa62
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 171f27c92ee9de14301ebb664e9ba3bcd98c394d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Verbinding met extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Klassieke Azure Portal](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)
>
>

Extern bureaublad kunt u toegang tot het bureaublad van een rol in Azure wordt uitgevoerd. U kunt een verbinding met extern bureaublad gebruiken oplossen en analyseren van problemen met uw toepassing, terwijl deze wordt uitgevoerd.

In dit artikel wordt beschreven hoe extern bureaublad inschakelen op rollen van uw Cloudservice met behulp van PowerShell. Zie [installeren en configureren van Azure PowerShell](/powershell/azure/overview) voor de vereisten die nodig zijn voor dit artikel. PowerShell maakt gebruik van de extern bureaublad-extensie zodat u extern bureaublad inschakelen kunt nadat de toepassing wordt geïmplementeerd.

## <a name="configure-remote-desktop-from-powershell"></a>Extern bureaublad vanuit PowerShell configureren
De [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet kunt u extern bureaublad inschakelen op de opgegeven rollen of alle rollen van uw cloud service-implementatie. De cmdlet kunt u de gebruikersnaam en wachtwoord opgeven voor de extern bureaublad-gebruiker via de *referentie* parameter een PSCredential-object accepteert.

Als u PowerShell interactief gebruikt, kunt u eenvoudig de PSCredential-object instellen door het aanroepen van de [Get-referenties](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

```
$remoteusercredentials = Get-Credential
```

Deze opdracht wordt een dialoogvenster waarin u de gebruikersnaam en wachtwoord invoeren voor de externe gebruiker op een veilige manier weergegeven.

Aangezien PowerShell in automation scenario's helpt, kunt u ook instellen de **PSCredential** object op een manier die geen tussenkomst van de gebruiker vereist. Eerst moet u een beveiligd wachtwoord instellen. U begint met het opgeven van een wachtwoord als tekst zonder opmaak converteren naar een veilige tekenreeks met [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx). Vervolgens moet u deze veilige tekenreeks converteren naar een versleutelde standaardtekenreeks met behulp [ConverterenVan SecureString](https://technet.microsoft.com/library/hh849814.aspx). Nu kunt u deze versleutelde standaardtekenreeks opslaan in een bestand met [Set inhoud](https://technet.microsoft.com/library/ee176959.aspx).

U kunt ook een bestand beveiligd wachtwoord maken zodat u niet hoeft te typen in het wachtwoord elke keer. Er is ook een bestand beveiligd wachtwoord beter dan een tekstbestand. De volgende PowerShell gebruiken om een bestand beveiligd wachtwoord te maken:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> Wanneer u het wachtwoord, zorg ervoor dat u voldoet aan de [complexiteitsvereisten](https://technet.microsoft.com/library/cc786468.aspx).
>
>

Voor het maken van het referentieobject uit het bestand beveiligd wachtwoord, moet u de bestandsinhoud lezen en terug te converteren naar een veilige tekenreeks met [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).

De [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet accepteert ook een *verlopen* parameter waarmee een **DateTime** op dat het gebruikersaccount is verlopen. U kunt bijvoorbeeld instellen dat de account verloopt een paar dagen van de huidige datum en tijd.

Deze PowerShell-voorbeeld laat zien hoe u de extern bureaublad-extensie instellen op een cloudservice:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
U kunt eventueel ook de implementatiesleuf en functies die u inschakelen van extern bureaublad wilt op opgeven. Als deze parameters niet zijn opgegeven, de cmdlet wordt ingeschakeld voor extern bureaublad op alle rollen in de **productie** implementatiesleuf.

De extern bureaublad-extensie is gekoppeld aan een implementatie. Als u een nieuwe implementatie voor de service maakt, hebt u de extern bureaublad inschakelen op implementatie. Als u altijd hebben van extern bureaublad is ingeschakeld wilt, moet vervolgens u de PowerShell-scripts integreren in uw werkstroom voor de implementatie.

## <a name="remote-desktop-into-a-role-instance"></a>Extern bureaublad in een rolinstantie
De [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet wordt gebruikt voor extern bureaublad in een specifieke rolexemplaar van de cloudservice. U kunt de *LocalPath* parameter voor het downloaden van de RDP-bestand lokaal. Of u kunt de *starten* -parameter voor het dialoogvenster verbinding met extern bureaublad voor toegang tot de cloud service-rolinstantie rechtstreeks te starten.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Controleer of de extern bureaublad-extensie is ingeschakeld voor een service
De [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet geeft weer die extern bureaublad is ingeschakeld of uitgeschakeld op een service-implementatie. De cmdlet retourneert de gebruikersnaam voor de extern bureaublad-gebruiker en de functies die de extern bureaublad-extensie is ingeschakeld voor. Dit gebeurt op de implementatiesleuf standaard en u kunt in plaats daarvan de faseringssleuf gebruiken.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Extern bureaublad-uitbreiding te verwijderen van een service
Als u de extern bureaublad-extensie op een implementatie al hebt ingeschakeld en moet de instellingen voor extern bureaublad bijwerken, moet u eerst de uitbreiding verwijderen. En deze opnieuw inschakelen met de nieuwe instellingen. Bijvoorbeeld als u wilt een nieuw wachtwoord instellen voor het externe gebruikersaccount of het account is verlopen. Dit is vereist op de bestaande implementaties waarvoor de extern bureaublad-extensie is ingeschakeld. Voor nieuwe implementaties, kunt u de extensie gewoon rechtstreeks toepassen.

Als u wilt de extern bureaublad-uitbreiding verwijderen uit de implementatie, kunt u de [verwijderen AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet. U kunt eventueel ook de implementatiesleuf en de rol van waaruit u wilt verwijderen van de extern bureaublad-extensie opgeven.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> Om volledig te verwijderen configuratie voor de uitbreiding, moet u aanroepen de *verwijderen* cmdlet uit met de **UninstallConfiguration** parameter.
>
> De **UninstallConfiguration** parameter Hiermee verwijdert u een extensie-configuratie die wordt toegepast op de service. De configuratie voor elke uitbreiding is gekoppeld aan de configuratie van de service. Het aanroepen van de *verwijderen* cmdlet zonder **UninstallConfiguration** instelt, scheidt u de <mark>implementatie</mark> van de configuratie voor de uitbreiding, dus effectief verwijderen van de extensie. Configuratie voor de uitbreiding blijft echter gekoppeld aan de service.
>
>

## <a name="additional-resources"></a>Aanvullende bronnen

[Cloud-Services configureren hoe](cloud-services-how-to-configure.md)
[extern bureaublad-services FAQ - Cloud](cloud-services-faq.md)
