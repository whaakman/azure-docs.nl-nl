---
title: Klonen van Web-App met behulp van PowerShell
description: Informatie over het klonen van uw Web-Apps naar de nieuwe Web-Apps met behulp van PowerShell.
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2016
ms.author: aelnably
ms.openlocfilehash: 30817a1a6a8079e7a896305ab0b59e48fad4d644
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service-App met behulp van PowerShell klonen
Een nieuwe optie met de release van Microsoft Azure PowerShell versie 1.1.0 is toegevoegd aan `New-AzureRMWebApp` waarmee u een bestaande Web-App naar een nieuwe app in een andere regio of in dezelfde regio klonen. Deze optie kan klanten voor het implementeren van een aantal apps over verschillende regio's snel en eenvoudig.

Klonen van de App is momenteel alleen ondersteund voor premium-laag app service-abonnementen. De nieuwe functie maakt gebruik van dezelfde beperkingen als de functie Web Apps back-up, Zie [Back-up van een web-app in Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Klonen van een bestaande App
Scenario: Een bestaande web-app in Zuid-centraal VS regio en u wilt klonen van de inhoud naar een nieuwe web-app in de regio Noord-centraal VS. Dit kunt doen met behulp van de Azure Resource Manager-versie van de PowerShell-cmdlet voor het maken van een nieuwe web-app met de `-SourceWebApp` optie.

Naam van de resourcegroep met de bron-web-app weet, kunt u de volgende PowerShell-opdracht voor de bron web-app-informatie (in dit geval naam `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Voor het maken van een nieuwe App Service-abonnement, kunt u `New-AzureRmAppServicePlan` opdracht zoals in het volgende voorbeeld

```PowerShell
New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

Met behulp van de `New-AzureRmWebApp` uitvoert, en u kunt nieuwe web-app maken in de regio Noord-centraal VS, en deze koppelen aan een bestaande premium-laag App Service-Plan. Bovendien kunt u dezelfde resourcegroep gebruiken als de bron-web-app of een nieuwe resourcegroep definiëren, zoals wordt weergegeven in de volgende opdracht:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

Een bestaande web-app, inclusief alle bijbehorende implementatiesites klonen, moet u de `IncludeSourceWebAppSlots` parameter. De volgende PowerShell-opdracht laat zien hoe u deze parameter met de `New-AzureRmWebApp` opdracht:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Als u wilt klonen van een bestaande web-app in dezelfde regio, moet u een nieuwe resourcegroep maken en een nieuwe app service plan in dezelfde regio en gebruik vervolgens de volgende PowerShell-opdracht voor het klonen van de web-app

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen van een bestaande App aan een App-serviceomgeving
Scenario: Een bestaande web-app in Zuid-centraal VS regio en u wilt klonen van de inhoud naar een nieuwe web-app aan een bestaand App Service omgeving (as-omgeving).

Naam van de resourcegroep met de bron-web-app weet, kunt u de volgende PowerShell-opdracht voor de bron web-app-informatie (in dit geval naam `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

De naam van de as-omgeving en de naam van de resourcegroep die deel uitmaakt van de as-omgeving te weten, kunt u de nieuwe web-app in de bestaande as-omgeving, zoals wordt weergegeven in de volgende opdracht:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

De `Location` parameter is vereist om verouderde reden, maar deze wordt genegeerd wanneer u de app in een as-omgeving maakt. 

## <a name="cloning-an-existing-app-slot"></a>Klonen van een bestaande App-sleuf
Scenario: U wilt klonen van een bestaande Web-Appsite ofwel een nieuwe Web-App of een nieuwe sleuf voor Web-App. De nieuwe Web-App kunnen zich in dezelfde regio bevinden als de oorspronkelijke sleuf voor Web-App of in een andere regio.

Naam van de resourcegroep met de bron-web-app weet, kunt u de volgende PowerShell-opdracht voor de bron web-appsite van informatie (in dit geval naam `source-webappslot`) gekoppeld aan de Web-App `source-webapp`:

```PowerShell
$srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot
```

De volgende opdracht toont het maken van een kloon van de bron-web-app in een nieuwe WebApp:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Traffic Manager tijdens het klonen van een app configureren
Meerdere landen/regio-web-apps maken en configureren van Azure Traffic Manager om verkeer te routeren naar alle deze web-apps, is een belangrijk scenario om ervoor te zorgen dat klanten apps maximaal beschikbaar zijn. Bij het klonen van een bestaande web-app, hebt u de mogelijkheid om zowel web-apps met een nieuw traffic manager-profiel of een bestaande. Alleen Azure Resource Manager-versie van Traffic Manager wordt ondersteund.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Een nieuw Traffic Manager-profiel maken tijdens het klonen van een app
Scenario: U wilt klonen van een web-app naar een andere regio tijdens het configureren van een Azure Resource Manager traffic manager-profiel met beide web-apps. De volgende opdracht toont een kloon van de bron-web-app in een nieuwe WebApp gemaakt tijdens het configureren van een nieuw Traffic Manager-profiel:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Toevoegen van nieuwe Web-App gekloond aan een bestaand Traffic Manager-profiel
Scenario: U al een Azure Resource Manager traffic manager-profiel hebt en wilt zowel web-apps als eindpunten toevoegen. Om dit te doen, moet u eerst het bestaande verkeer samenstellen manager-profiel-id. U moet de abonnements-ID, naam van de resourcegroep en de bestaande profielnaam voor traffic manager.

```PowerShell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Na de traffic Manager-ID hebt, ziet u de volgende opdracht maakt een kloon van de bron-web-app in een nieuwe WebApp terwijl ze toe te voegen aan een bestaand Traffic Manager-profiel:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Huidige beperkingen
Deze functie is momenteel in preview en nieuwe mogelijkheden worden toegevoegd na verloop van tijd. Hier volgen de bekende beperkingen van de huidige versie van het klonen van de app:

* Instellingen voor automatisch schalen worden niet gekloond.
* Back-upschema instellingen worden niet gekloond.
* VNET-instellingen worden niet gekloond.
* App Insights worden niet automatisch ingesteld op de doel-web-app
* Eenvoudige verificatie-instellingen worden niet gekloond.
* Kudu extensie worden niet gekloond.
* TiP regels worden niet gekloond.
* Database-inhoud is niet gekloond
* Uitgaande IP-adressen verandert als aan een andere schaaleenheid klonen

### <a name="references"></a>Verwijzingen
* [Klonen van de Web-App](app-service-web-app-cloning.md)
* [Back-up van een web-app in Azure App Service](web-sites-backup.md)
* [Azure Resource Manager-ondersteuning voor Azure Traffic Manager-Preview](../traffic-manager/traffic-manager-powershell-arm.md)
* [Inleiding tot de App Service-omgeving](environment/intro.md)
* [Azure PowerShell gebruiken met Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

