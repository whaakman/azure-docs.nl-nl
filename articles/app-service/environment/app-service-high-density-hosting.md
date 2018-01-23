---
title: High-density hosten op Azure App Service | Microsoft Docs
description: High-density hosten op Azure App Service
author: btardif
manager: erikre
editor: 
services: app-service\web
documentationcenter: 
ms.assetid: a903cb78-4927-47b0-8427-56412c4e3e64
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 22/01/2018
ms.author: byvinyal
ms.openlocfilehash: 2ffffd3cc9f5c59f74f71d6d7d31c5ea615d11f4
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="high-density-hosting-on-azure-app-service"></a>High-density hosten op Azure App Service
Wanneer u App Service, wordt uw toepassing losgekoppeld van de capaciteit toegewezen door twee concepten:

* **De toepassing:** vertegenwoordigt de app en de configuratie van de runtime. Bevat bijvoorbeeld de versie van .NET die de runtime moet worden geladen, wordt de app-instellingen.
* **De App Service-abonnement:** definieert de eigenschappen van de capaciteit, de beschikbare functieset en de plaats van de toepassing. Kenmerken zijn mogelijk grote (vier kernen)-machine, vier exemplaren, Premium-functies in VS-Oost.

Een app altijd aan een App Service-abonnement is gekoppeld, maar een App Service-plan capaciteit om een of meer apps te kan bieden.

Het platform biedt daardoor de flexibiliteit om te isoleren van een enkele app of meerdere apps die resources delen door het delen van een App Service-abonnement hebben.

Echter, wanneer meerdere apps een App Service-abonnement deelt, een exemplaar van die app wordt uitgevoerd op elke instantie van deze App Service-abonnement.

## <a name="per-app-scaling"></a>Per app schalen
*Per app schalen* is een functie die kan worden ingeschakeld op het niveau van de App Service-plan en vervolgens gebruikt per toepassing.

Per app schaalt schalen een app onafhankelijk van de App Service-abonnement die als host fungeert. Op deze manier een App Service-abonnement kan worden geschaald tot 10 exemplaren, maar een app kan worden ingesteld op slechts vijf gebruiken.

   >[!NOTE]
   >Per app-schalen, is alleen beschikbaar voor **standaard**, **Premium**, **Premium V2** en **geïsoleerd** SKU App Service-plannen
   >

### <a name="per-app-scaling-using-powershell"></a>Per app schalen met behulp van PowerShell

Maak een plan dat is geconfigureerd als een *Per app schalen* plan door door te geven in de ```-perSiteScaling $true``` kenmerk de ```New-AzureRmAppServicePlan``` commandlet

```
New-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan `
                            -Location $Location `
                            -Tier Premium -WorkerSize Small `
                            -NumberofWorkers 5 -PerSiteScaling $true
```

Als u bijwerken van een bestaand App Service-abonnement om deze functie te gebruiken wilt: 

- het doel-plan ophalen```Get-AzureRmAppServicePlan```
- de eigenschap lokaal wijzigen```$newASP.PerSiteScaling = $true```
- publiceren van uw wijzigingen naar azure```Set-AzureRmAppServicePlan``` 

```
# Get the new App Service Plan and modify the "PerSiteScaling" property.
$newASP = Get-AzureRmAppServicePlan -ResourceGroupName $ResourceGroup -Name $AppServicePlan
$newASP

#Modify the local copy to use "PerSiteScaling" property.
$newASP.PerSiteScaling = $true
$newASP
    
#Post updated app service plan back to azure
Set-AzureRmAppServicePlan $newASP
```

Configureer op het niveau van de app het aantal exemplaren die de app in de app service-abonnement gebruiken kunt.

In het onderstaande voorbeeld is de app beperkt tot twee instanties, ongeacht hoe vaak het onderliggende app service-abonnement uitgeschaald aan.

```
# Get the app we want to configure to use "PerSiteScaling"
$newapp = Get-AzureRmWebApp -ResourceGroupName $ResourceGroup -Name $webapp
    
# Modify the NumberOfWorkers setting to the desired value.
$newapp.SiteConfig.NumberOfWorkers = 2
    
# Post updated app back to azure
Set-AzureRmWebApp $newapp
```

> [!IMPORTANT]
> $newapp. SiteConfig.NumberOfWorkers verschilt van $newapp. MaxNumberOfWorkers. Per app schalen maakt gebruik van $newapp. SiteConfig.NumberOfWorkers om te bepalen van de kenmerken van de schaal van de app.

### <a name="per-app-scaling-using-azure-resource-manager"></a>Per app schalen met Azure Resource Manager

De volgende *Azure Resource Manager-sjabloon* maakt:

- Een App Service-abonnement wordt uitgebreid naar 10 exemplaren
- een app die geconfigureerd voor het schalen van maximaal vijf exemplaren.

Het instellen van de App Service-abonnement de **PerSiteScaling** eigenschap op true ```"perSiteScaling": true```. Het instellen van de app de **aantal werknemers** met 5 ```"properties": { "numberOfWorkers": "5" }```.

```
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
Per app schalen is een functie die in App Service-omgevingen en globale Azure-regio's is ingeschakeld. De aanbevolen strategie is echter het gebruik van App Service-omgevingen om te profiteren van de geavanceerde functies en de grotere groepen van capaciteit.  

Volg deze stappen voor het configureren van high-density die als host fungeert voor uw apps:

1. De App-serviceomgeving configureren en kies een werknemersgroep die is aan het high-density hosting scenario toegewezen.
1. Een enkele App Service-abonnement maken en schalen voor gebruik van de beschikbare capaciteit op de worker-groep.
1. De PerSiteScaling-vlag ingesteld op true in de App Service-plan.
1. Nieuwe apps zijn gemaakt en toegewezen aan deze App Service-abonnement met de **numberOfWorkers** eigenschap ingesteld op **1**. Met deze configuratie levert de hoogst mogelijke dichtheid in deze worker-groep.
1. Het aantal werknemers kan afzonderlijk worden geconfigureerd per app verlenen aanvullende resources naar behoefte. Bijvoorbeeld:
    - Een app intensief gebruik kunt stellen **numberOfWorkers** naar **3** hebben meer verwerkingscapaciteit voor die app. 
    - Laag gebruik apps stelt **numberOfWorkers** naar **1**.

## <a name="next-steps"></a>Volgende stappen

- [Gedetailleerd overzicht van Azure App Service-plannen](../azure-web-sites-web-hosting-plans-in-depth-overview.md)
- [Inleiding tot de App Service-omgeving](app-service-app-service-environment-intro.md)
