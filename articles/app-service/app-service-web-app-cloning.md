---
title: App klonen met PowerShell - Azure App Service
description: Leer hoe u uw App Service klonen op een nieuwe app met behulp van PowerShell.
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
ms.openlocfilehash: 17ea8545855cd926a393e9e40d3eccaabd6dba53
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54886523"
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service-App klonen met behulp van PowerShell
Met de release van Microsoft Azure PowerShell versie 1.1.0 is een nieuwe optie is toegevoegd aan `New-AzureRMWebApp` waarmee u een bestaande App Service-app om een nieuwe app in een andere regio of in dezelfde regio te klonen. Deze optie kan klanten snel en eenvoudig een aantal apps implementeren in verschillende regio's.

App klonen is momenteel alleen ondersteund voor premium-laag app service-abonnementen. Dezelfde beperkingen als back-up van App Service-functie maakt gebruik van de nieuwe functie, Zie [maakt u een Back-up van een app in Azure App Service](manage-backup.md).

## <a name="cloning-an-existing-app"></a>Een bestaande app klonen
Scenario: Een bestaande app in de regio Zuid-centraal VS, en u wilt klonen van de inhoud naar een nieuwe app in de regio Noord-centraal VS. Dit kan worden bereikt met behulp van de Azure Resource Manager-versie van de PowerShell-cmdlet om te maken van een nieuwe app met de `-SourceWebApp` optie.

Naam van de resourcegroep met de bron-app weet, kunt u de volgende PowerShell-opdracht kunt gebruiken om op te halen van de bronapp-gegevens (in dit geval met de naam `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Voor het maken van een nieuwe App Service-Plan, kunt u `New-AzureRmAppServicePlan` opdracht zoals in het volgende voorbeeld

```PowerShell
New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

Met behulp van de `New-AzureRmWebApp` opdracht, kunt u de nieuwe app maken in de regio Noord-centraal VS, en deze koppelen aan een bestaande premium-laag van App Service-Plan. U kunt bovendien gebruik dezelfde resourcegroep bevinden als de bron-app of een nieuwe resourcegroep definiëren, zoals wordt weergegeven in de volgende opdracht uit:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

Als u wilt een bestaande app, met inbegrip van alle bijbehorende implementatiesites klonen, moet u gebruiken de `IncludeSourceWebAppSlots` parameter. De volgende PowerShell-opdracht ziet u het gebruik van deze parameter met de `New-AzureRmWebApp` opdracht:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Als u wilt klonen van een bestaande app binnen dezelfde regio, moet u een nieuwe resourcegroep maken en een nieuwe appservice plannen in dezelfde regio en vervolgens de volgende PowerShell-opdracht gebruiken om het te klonen:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen van een bestaande App naar een App Service Environment
Scenario: Een bestaande app in de regio Zuid-centraal VS, en u wilt klonen van de inhoud naar een nieuwe app op een bestaande App Service Environment (ASE).

Naam van de resourcegroep met de bron-app weet, kunt u de volgende PowerShell-opdracht kunt gebruiken om op te halen van de bronapp-gegevens (in dit geval met de naam `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

De naam van de as-omgeving en de naam van de resourcegroep die deel uitmaakt van de as-omgeving te weten, kunt u de nieuwe app in de bestaande as-omgeving, zoals wordt weergegeven in de volgende opdracht uit:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

De `Location` parameter is vereist vanwege de verouderde reden, maar wordt dit genegeerd wanneer u de app in een as-omgeving maakt. 

## <a name="cloning-an-existing-app-slot"></a>Klonen van een bestaande App-sleuf
Scenario: Wilt u een bestaande implementatiesleuf van een app in een nieuwe app of een nieuwe sleuf klonen. De nieuwe app kan zich in dezelfde regio als de oorspronkelijke app-site of in een andere regio.

Naam van de resourcegroep met de bron-app weet, kunt u de volgende PowerShell-opdracht kunt gebruiken voor de bron-app-site-informatie (in dit geval met de naam `source-appslot`) gekoppeld aan `source-app`:

```PowerShell
$srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-app -Slot source-appslot
```

De volgende opdracht ziet u een kloon van de bron-app in een nieuwe app maken:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-app -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Traffic Manager tijdens het klonen van een app configureren
Het maken van apps voor meerdere regio's en het configureren van Azure Traffic Manager om verkeer te routeren naar deze apps is een belangrijk scenario om ervoor te zorgen dat klanten apps maximaal beschikbaar zijn. Bij het klonen van een bestaande app, hebt u de optie om beide apps naar een nieuw traffic manager-profiel of een bestaande verbinding te maken. Alleen Azure Resource Manager-versie van Traffic Manager wordt ondersteund.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Het maken van een nieuw Traffic Manager-profiel terwijl het klonen van een app
Scenario: Wilt u een app naar een andere regio klonen tijdens het configureren van een Azure Resource Manager traffic manager-profiel dat beide apps bevat. De volgende opdracht ziet u het maken van een kloon van de bron-app in een nieuwe app tijdens het configureren van een nieuw Traffic Manager-profiel:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-app-to-an-existing-traffic-manager-profile"></a>App aan een bestaand Traffic Manager-profiel toe te voegen nieuwe gekloond
Scenario: U al een Azure Resource Manager traffic manager-profiel hebt en wilt toevoegen van zowel apps als eindpunten. Om dit te doen, moet u eerst voor het samenstellen van de bestaande traffic manager-profiel-id. U moet de abonnements-ID, naam van de resourcegroep en de bestaande profielnaam voor traffic manager.

```PowerShell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Nadat er de traffic Manager-ID, de volgende opdracht wordt uitgelegd hoe u een kloon van de bron-app in een nieuwe app terwijl u ze toevoegt aan een bestaand Traffic Manager-profiel:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Huidige beperkingen
Hier volgen de bekende beperkingen van de app klonen:

* Instellingen voor automatisch schalen worden niet gekloond
* Back-upschema instellingen worden niet gekloond
* VNET-instellingen worden niet gekloond
* App Insights worden niet automatisch ingesteld op de doel-app
* Eenvoudige verificatie-instellingen worden niet gekloond
* Kudu-extensie worden niet gekloond
* TiP regels worden niet gekloond
* Inhoud van de database is niet gekloond
* Uitgaande IP-adressen gewijzigd als er aan een andere schaaleenheid klonen

### <a name="references"></a>Verwijzingen
* [App Service klonen](app-service-web-app-cloning.md)
* [Back-up van een app in Azure App Service](manage-backup.md)
* [Azure Resource Manager-ondersteuning voor Azure Traffic Manager-Preview](../traffic-manager/traffic-manager-powershell-arm.md)
* [Inleiding tot de App Service-omgeving](environment/intro.md)
* [Azure PowerShell gebruiken met Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

