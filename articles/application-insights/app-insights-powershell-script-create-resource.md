---
title: PowerShell-script voor het maken van een Application Insights-resource | Microsoft Docs
description: Maken van de Application Insights-resources automatiseren.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/19/2016
ms.author: mbullwin
ms.openlocfilehash: 376bcb542e4e83c2464d9f3f53ea71965ce79c33
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script-to-create-an-application-insights-resource"></a>PowerShell-script om een Application Insights-resource te maken


Als u wilt bewaken van een nieuwe toepassing- of een nieuwe versie van een toepassing - met [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), instellen van een nieuwe resource in Microsoft Azure. Deze resource wordt waarbij de telemetriegegevens vanuit uw app geanalyseerd en weergegeven. 

U kunt het maken van een nieuwe resource automatiseren met behulp van PowerShell.

Als u een mobiel apparaat-app ontwikkelt, is het bijvoorbeeld waarschijnlijk dat op elk gewenst moment zal er verschillende versies van uw app wordt gebruikt door uw klanten zijn gepubliceerd. U wilt niet dat de resultaten van de telemetrie van verschillende versies verward ophalen. Zo kunt u uw buildproces voor het maken van een nieuwe bron voor elk build ophalen.

> [!NOTE]
> Als u maken van een set resources allemaal op hetzelfde moment wilt, overweeg dan [maken van de resources met een Azure-sjabloon](app-insights-powershell.md).
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a>Script voor het maken van een Application Insights-resource
Zie de specificaties van de relevante cmdlet:

* [Nieuwe AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*PowerShell-Script*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount / Login-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

# Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource


$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Wat te doen met de sleutel
Elke bron wordt geïdentificeerd door de instrumentatiesleutel (iKey). De sleutel is de uitvoer van een script voor het maken van de resource. Uw script build leveren dat de iKey naar Application Insights-SDK is ingesloten in uw app.

Er zijn twee manieren om de iKey beschikbaar te maken met de SDK:

* In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*iKey*`</instrumentationkey>`
* Of in [initialisatiecode](app-insights-api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Zie ook
* [Application Insights en test webbronnen van sjablonen maken](app-insights-powershell.md)
* [Instellen van de bewaking van Azure diagnostics met PowerShell](app-insights-powershell-azure-diagnostics.md) 
* [Waarschuwingen instellen met behulp van PowerShell](app-insights-powershell-alerts.md)

