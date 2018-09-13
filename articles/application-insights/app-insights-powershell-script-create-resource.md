---
title: PowerShell-script voor het maken van een Application Insights-resource | Microsoft Docs
description: Maken van Application Insights-resources automatiseren.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/19/2016
ms.author: mbullwin
ms.openlocfilehash: c6ef869bd211b156ba17d2aa8f384d15269b6d59
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35643638"
---
# <a name="powershell-script-to-create-an-application-insights-resource"></a>PowerShell-script om een Application Insights-resource te maken


Indien u wenst te bewaken van een nieuwe toepassing- of een nieuwe versie van een toepassing - met [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), instellen van een nieuwe resource in Microsoft Azure. Deze resource is waar de telemetriegegevens van uw app is geanalyseerd en weergegeven. 

U kunt het maken van een nieuwe resource automatiseren met behulp van PowerShell.

Bijvoorbeeld, als u een mobiel apparaat-app ontwikkelt, waarschijnlijk dat op elk gewenst moment, zullen er verschillende gepubliceerde versies van uw app wordt gebruikt door uw klanten. U wilt niet dat de resultaten van de telemetrie van verschillende versies verward ophalen. Je krijgt dus uw buildproces voor het maken van een nieuwe resource voor elke build.

> [!NOTE]
> Als u maken van een set met resources allemaal op hetzelfde moment wilt, kunt u overwegen [maken van de resources met behulp van een Azure-sjabloon](app-insights-powershell.md).
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a>Script voor het maken van een Application Insights-resource
Zie de specificaties van de desbetreffende cmdlet:

* [New-AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*PowerShell-Script*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Connect-AzureRmAccount / Connect-AzureRmAccount

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

## <a name="what-to-do-with-the-ikey"></a>Wat te doen met de iKey
Elke resource wordt geïdentificeerd door de instrumentatiesleutel (iKey). De iKey is een uitvoer van een script voor het maken van de resource. Uw build-script moet bieden dat de iKey naar de Application Insights SDK ingesloten in uw app.

Er zijn twee manieren om de iKey beschikbaar te maken met de SDK:

* In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*iKey*`</instrumentationkey>`
* Of in [initialisatiecode](app-insights-api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Zie ook
* [Application Insights en test bronnen op het web maken met sjablonen](app-insights-powershell.md)
* [Controle van Azure diagnostics met PowerShell instellen](app-insights-powershell-azure-diagnostics.md) 
* [Waarschuwingen instellen met behulp van PowerShell](app-insights-powershell-alerts.md)

