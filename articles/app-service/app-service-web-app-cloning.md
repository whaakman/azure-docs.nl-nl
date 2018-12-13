---
title: App klonen met PowerShell - Azure App Service
description: Informatie over het klonen van uw Web-Apps naar de nieuwe Web-Apps met behulp van PowerShell.
services: app-service\web
documentationcenter: ''
author: ahmedelnably
manager: stefsch
editor: ''
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2016
ms.author: aelnably
ms.custom: seodec18
ms.openlocfilehash: 87bae4db64c0a22790b7f52f919601f82aa548df
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53261864"
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service-App klonen met behulp van PowerShell
Met de release van Microsoft Azure PowerShell versie 1.1.0 is een nieuwe optie is toegevoegd aan `New-AzureRMWebApp` waarmee u een bestaande Web-App om een nieuwe app in een andere regio of in dezelfde regio te klonen. Deze optie kan klanten snel en eenvoudig een aantal apps implementeren in verschillende regio's.

App klonen is momenteel alleen ondersteund voor premium-laag app service-abonnementen. De nieuwe functie maakt gebruik van dezelfde beperkingen als de functie Web Apps-back-up, Zie [maakt u een Back-up van een web-app in Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Een bestaande App klonen
Scenario: Een bestaande web-app in de regio Zuid-centraal VS, en u wilt klonen van de inhoud naar een nieuwe web-app in de regio Noord-centraal VS. Dit kan worden bereikt met behulp van de Azure Resource Manager-versie van de PowerShell-cmdlet om te maken van een nieuwe web-app met de `-SourceWebApp` optie.

Naam van de resourcegroep met de bron-web-app weet, kunt u de volgende PowerShell-opdracht kunt gebruiken om op te halen van de bron web-app-gegevens (in dit geval met de naam `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Voor het maken van een nieuwe App Service-Plan, kunt u `New-AzureRmAppServicePlan` opdracht zoals in het volgende voorbeeld

```PowerShell
New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

Met behulp van de `New-AzureRmWebApp` opdracht, u kunt de nieuwe web-app maken in de regio Noord-centraal VS, en deze koppelen aan een bestaande premium-laag van App Service-Plan. U kunt bovendien gebruik dezelfde resourcegroep bevinden als de bron-web-app of een nieuwe resourcegroep definiëren, zoals wordt weergegeven in de volgende opdracht uit:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

Als u wilt een bestaande web-app, met inbegrip van alle bijbehorende implementatiesites klonen, moet u gebruiken de `IncludeSourceWebAppSlots` parameter. De volgende PowerShell-opdracht ziet u het gebruik van deze parameter met de `New-AzureRmWebApp` opdracht:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Als u wilt klonen van een bestaande web-app binnen dezelfde regio, moet u een nieuwe resourcegroep maken en een nieuwe appservice plannen in dezelfde regio en vervolgens de volgende PowerShell-opdracht gebruiken om te klonen van de web-app

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen van een bestaande App naar een App Service Environment
Scenario: Een bestaande web-app in de regio Zuid-centraal VS, en u wilt klonen van de inhoud naar een nieuwe web-app aan een bestaande App Service Environment (ASE).

Naam van de resourcegroep met de bron-web-app weet, kunt u de volgende PowerShell-opdracht kunt gebruiken om op te halen van de bron web-app-gegevens (in dit geval met de naam `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

De naam van de as-omgeving en de naam van de resourcegroep die deel uitmaakt van de as-omgeving te weten, kunt u de nieuwe web-app in de bestaande as-omgeving, zoals wordt weergegeven in de volgende opdracht uit:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

De `Location` parameter is vereist vanwege de verouderde reden, maar wordt dit genegeerd wanneer u de app in een as-omgeving maakt. 

## <a name="cloning-an-existing-app-slot"></a>Klonen van een bestaande App-sleuf
Scenario: Wilt u een bestaande Web-App-sleuf aan zowel een nieuwe Web-App of een nieuwe sleuf van de Web-App klonen. Nieuwe Web-App kan zich in dezelfde regio als de oorspronkelijke Web-App-site of in een andere regio.

Naam van de resourcegroep met de bron-web-app weet, kunt u de volgende PowerShell-opdracht kunt gebruiken voor de bron web app sleuf informatie (in dit geval met de naam `source-webappslot`) gekoppeld aan de Web-App `source-webapp`:

```PowerShell
$srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot
```

De volgende opdracht ziet u het maken van een kloon van de bron-web-app in een nieuwe WebApp:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Traffic Manager tijdens het klonen van een app configureren
Meerdere regio's, web-apps maken en configureren van Azure Traffic Manager om te routeren van verkeer naar alle deze WebApps, is een belangrijk scenario om ervoor te zorgen dat klanten apps maximaal beschikbaar zijn. Bij het klonen van een bestaande web-app, hebt u de mogelijkheid om zowel web-apps naar een nieuw traffic manager-profiel of een bestaande verbinding te maken. Alleen Azure Resource Manager-versie van Traffic Manager wordt ondersteund.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Het maken van een nieuw Traffic Manager-profiel terwijl het klonen van een app
Scenario: Wilt u een WebApp klonen naar een andere regio tijdens het configureren van een Azure Resource Manager traffic manager-profiel met zowel web-apps. De volgende opdracht ziet u het maken van een kloon van de bron-web-app in een nieuwe WebApp tijdens het configureren van een nieuw Traffic Manager-profiel:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Toevoegen van nieuwe Web-App gekloond naar een bestaand Traffic Manager-profiel
Scenario: U al een Azure Resource Manager traffic manager-profiel hebt en u wilt beide web-apps als eindpunten toevoegen. Om dit te doen, moet u eerst voor het samenstellen van de bestaande traffic manager-profiel-id. U moet de abonnements-ID, naam van de resourcegroep en de bestaande profielnaam voor traffic manager.

```PowerShell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Nadat er de traffic Manager-ID, de volgende opdracht wordt uitgelegd hoe u een kloon van de bron-web-app in een nieuwe WebApp terwijl u ze toevoegt aan een bestaand Traffic Manager-profiel:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Huidige beperkingen
Deze functie is momenteel in preview en nieuwe mogelijkheden worden toegevoegd na verloop van tijd. Hier volgen de bekende beperkingen met betrekking tot de huidige versie van het klonen van de app:

* Instellingen voor automatisch schalen worden niet gekloond
* Back-upschema instellingen worden niet gekloond
* VNET-instellingen worden niet gekloond
* App Insights worden niet automatisch ingesteld op de doel-web-app
* Eenvoudige verificatie-instellingen worden niet gekloond
* Kudu-extensie worden niet gekloond
* TiP regels worden niet gekloond
* Inhoud van de database is niet gekloond
* Uitgaande IP-adressen gewijzigd als er aan een andere schaaleenheid klonen

### <a name="references"></a>Verwijzingen
* [Web-App klonen](app-service-web-app-cloning.md)
* [Back-up van een web-app in Azure App Service](web-sites-backup.md)
* [Azure Resource Manager-ondersteuning voor Azure Traffic Manager-Preview](../traffic-manager/traffic-manager-powershell-arm.md)
* [Inleiding tot de App Service-omgeving](environment/intro.md)
* [Azure PowerShell gebruiken met Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

