---
title: High-density hosten op Azure App Service met behulp van per-app schalen | Microsoft Docs
description: High-density hosten op Azure App Service
author: btardif
manager: erikre
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/22/2018
ms.author: byvinyal
ms.openlocfilehash: 97e1efe34417c3bf2f23801b2112b718f55d3416
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961925"
---
# <a name="high-density-hosting-on-azure-app-service-using-per-app-scaling"></a>High-density hosten op Azure App Service met behulp van per-app schalen
Standaard u App Service-apps schalen door het schalen van de [App Service-abonnement](azure-web-sites-web-hosting-plans-in-depth-overview.md) ze op worden uitgevoerd. Wanneer meerdere apps worden uitgevoerd in dezelfde App Service-abonnement, wordt elke instantie schaalvergroting alle apps uitgevoerd in het plan.

U kunt inschakelen *per app schalen* op App Service plan niveau. Het schalen een app onafhankelijk van de App Service-abonnement die als host fungeert. Op deze manier een App Service-abonnement kan worden geschaald tot 10 exemplaren, maar een app kan worden ingesteld op slechts vijf gebruiken.

> [!NOTE]
> Per app schalen is alleen beschikbaar voor **standaard**, **Premium**, **Premium V2** en **geïsoleerd** Prijscategorieën.
>

## <a name="per-app-scaling-using-powershell"></a>Per app schalen met behulp van PowerShell

Maken van een plan met per app schalen door te geven in de ```-perSiteScaling $true``` kenmerk de ```New-AzureRmAppServicePlan``` commandlet

```powershell
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Het bijwerken van een bestaand App Service-abonnement met per app schalen: 

- het doel-plan ophalen ```Get-AzureRmAppServicePlan```
- de eigenschap lokaal wijzigen ```$newASP.PerSiteScaling = $true```
- publiceren van uw wijzigingen naar azure ```Set-AzureRmAppServicePlan``` 

```powershell
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

Configureer op het niveau van de app het aantal exemplaren die de app in de App Service-abonnement gebruiken kunt.

In het onderstaande voorbeeld is de app beperkt tot twee instanties, ongeacht hoe vaak het onderliggende app service-abonnement uitgeschaald aan.

```powershell
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> `$newapp.SiteConfig.NumberOfWorkers` verschilt van `$newapp.MaxNumberOfWorkers`. Per app schalen gebruikt `$newapp.SiteConfig.NumberOfWorkers` om te bepalen van de kenmerken van de schaal van de app.

## <a name="per-app-scaling-using-azure-resource-manager"></a>Per app schalen met Azure Resource Manager

De volgende Azure Resource Manager-sjabloon wordt gemaakt:

- Een App Service-abonnement wordt uitgebreid naar 10 exemplaren
- een app die geconfigureerd voor het schalen van maximaal vijf exemplaren.

Het instellen van de App Service-abonnement de **PerSiteScaling** eigenschap op true `"perSiteScaling": true`. Het instellen van de app de **aantal werknemers** met 5 `"properties": { "numberOfWorkers": "5" }`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "appServicePlanName": { "type": "string" },
        "appName": { "type": "string" }
        },
    "resources": [
    {
        "comments": "App Service Plan with per site perSiteScaling = true",
        "type": "Microsoft.Web/serverFarms",
        "sku": {
            "name": "P1",
            "tier": "Premium",
            "size": "P1",
            "family": "P",
            "capacity": 10
            },
        "name": "[parameters('appServicePlanName')]",
        "apiVersion": "2015-08-01",
        "location": "West US",
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "perSiteScaling": true
        }
    },
    {
        "type": "Microsoft.Web/sites",
        "name": "[parameters('appName')]",
        "apiVersion": "2015-08-01-preview",
        "location": "West US",
        "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
        "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
        "resources": [ {
                "comments": "",
                "type": "config",
                "name": "web",
                "apiVersion": "2015-08-01",
                "location": "West US",
                "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                "properties": { "numberOfWorkers": "5" }
            } ]
        }]
}
```

## <a name="recommended-configuration-for-high-density-hosting"></a>Aanbevolen configuratie voor het hosten van high-density
Per app schalen is een functie die is ingeschakeld in beide globale Azure-regio's en [App Service-omgevingen](environment/app-service-app-service-environment-intro.md). De aanbevolen strategie is echter het gebruik van App Service-omgevingen om te profiteren van de geavanceerde functies en de grotere groepen van capaciteit.  

Volg deze stappen voor het configureren van high-density die als host fungeert voor uw apps:

1. De App-serviceomgeving configureren en kies een werknemersgroep die is aan het high-density hosting scenario toegewezen.
1. Een enkele App Service-abonnement maken en schalen voor gebruik van de beschikbare capaciteit op de worker-groep.
1. Stel de `PerSiteScaling` vlag op true in de App Service-plan.
1. Nieuwe apps zijn gemaakt en toegewezen aan deze App Service-abonnement met de **numberOfWorkers** eigenschap ingesteld op **1**. Met deze configuratie levert de hoogst mogelijke dichtheid in deze worker-groep.
1. Het aantal werknemers kan afzonderlijk worden geconfigureerd per app verlenen aanvullende resources naar behoefte. Bijvoorbeeld:
    - Een app intensief gebruik kunt stellen **numberOfWorkers** naar **3** hebben meer verwerkingscapaciteit voor die app. 
    - Laag gebruik apps stelt **numberOfWorkers** naar **1**.

## <a name="next-steps"></a>Volgende stappen

- [Gedetailleerd overzicht van Azure App Service-plannen](azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [Inleiding tot de App Service-omgeving](environment/app-service-app-service-environment-intro.md)
