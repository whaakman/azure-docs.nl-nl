---
title: Profileer live Azure App Service-apps met Application Insights | Microsoft Docs
description: Profileer live-apps in Azure App Service met Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 71a8a0e268c1b264a0a1a7f955f310bfddc830d2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439942"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Profileer live Azure App Service-apps met Application Insights

U kunt uitvoeren Profiler op ASP.NET en ASP.NET Core-apps die worden uitgevoerd op Azure App Service met behulp van basis-servicelaag of hoger. Inschakelen van Profiler in Linux is momenteel alleen mogelijk via [deze methode](profiler-aspnetcore-linux.md).

## <a id="installation"></a> Profiler inschakelen voor uw app
Als u wilt Profiler inschakelen voor een app, volg de onderstaande instructies. Als u een ander type Azure-service uitvoert, vindt hier u instructies voor het inschakelen van Profiler op andere ondersteunde platforms:
* [Cloud Services](../../azure-monitor/app/profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-toepassingen](../../azure-monitor/app/profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtuele machines](../../azure-monitor/app/profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler is vooraf geïnstalleerd als onderdeel van de runtime van de App-Services. De onderstaande stappen wordt beschreven hoe inschakelt voor uw App Service. Volg deze stappen, zelfs als u de App Insights-SDK hebt opgenomen in uw toepassing tijdens het opbouwen.

1. Schakel 'Always On'-instelling voor uw appservice. U kunt de instelling op de pagina configuratie van uw App Service onder algemene instellingen bijwerken.
1. Ga naar de **App Services** deelvenster in de Azure-portal.
1. Navigeer naar **instellingen > Application Insights** deelvenster.

   ![App Insights inschakelen in App Services-portal](./media/profiler/AppInsights-AppServices.png)

1. Ofwel de instructies in het deelvenster te maken van een nieuwe resource of Selecteer een bestaande App Insights-resource voor het bewaken van uw app. Controleer ook of de Profiler is **op**. Als uw Application Insights-resource in een ander abonnement vanuit uw App Service is, kunt u deze pagina niet gebruiken voor het configureren van Application Insights. U kunt nog steeds dit handmatig doen dat door het maken van de vereiste app-instellingen handmatig. [De volgende sectie bevat instructies voor het handmatig inschakelen van Profiler.](#enable-profiler-manually-or-with-azure-resource-manager) 

   ![App Insights-site-extensie toevoegen][Enablement UI]

1. Profiler is nu ingeschakeld met behulp van een App-instelling van de App-Services.

    ![App-instelling voor Profiler][profiler-app-setting]

## <a name="enable-profiler-manually-or-with-azure-resource-manager"></a>Inschakelen van Profiler handmatig of met Azure Resource Manager
Application Insights Profiler kan worden ingeschakeld door het maken van appinstellingen voor uw Azure App Service. De pagina met de bovenstaande opties wordt deze appinstellingen voor u gemaakt. Maar u kunt het maken van deze instellingen met behulp van een sjabloon of op andere wijze automatiseren. Deze instellingen ook werken als uw Application Insights-resource in een ander abonnement van uw Azure App Service is.
Dit zijn de instellingen die nodig zijn voor de profiler inschakelen:

|App-instelling    | Value    |
|---------------|----------|
|APPINSIGHTS_INSTRUMENTATIONKEY         | iKey voor uw Application Insights-resource    |
|APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
|DiagnosticServices_EXTENSION_VERSION | ~3 |


U kunt deze waarden met behulp van instellen [Azure Resource Manager-sjablonen](../../azure-monitor/app/azure-web-apps.md#app-service-application-settings-with-azure-resource-manager), [Azure Powershell](https://docs.microsoft.com/powershell/module/az.websites/set-azwebapp), [Azure CLI](https://docs.microsoft.com/cli/azure/webapp/config/appsettings?view=azure-cli-latest).

### <a name="enabling-profiler-for-other-clouds-manually"></a>Profiler handmatig inschakelen voor andere clouds

Als u de profiler voor andere clouds inschakelen wilt, kunt u de onderstaande app-instellingen.

|App-instelling    | US Government waarden| China-cloud |   
|---------------|---------------------|-------------|
|ApplicationInsightsProfilerEndpoint         | https://agent.serviceprofiler.azure.us    | https://profiler.applicationinsights.azure.cn |
|ApplicationInsightsEndpoint | https://dc.applicationinsights.us | https://dc.applicationinsights.azure.cn |

## <a name="disable-profiler"></a>Profiler uitschakelen

Profiler voor een individuele app-exemplaar, onder opnieuw te starten of stoppen **-webtaken**, gaat u naar de app-resource. Als u wilt verwijderen van Profiler, gaat u naar **extensies**.

![Profiler uitschakelen voor een webtaak][disable-profiler-webjob]

Het is raadzaam dat u hebt ingeschakeld op al uw apps voor het detecteren van eventuele prestatieproblemen zo vroeg mogelijk Profiler.

De Profiler-bestanden kunnen worden verwijderd wanneer het gebruik van web Deploy voor het implementeren van wijzigingen naar uw webtoepassing. U kunt voorkomen dat de verwijdering door uit te sluiten van de map App_Data worden verwijderd tijdens de implementatie. 


## <a name="next-steps"></a>Volgende stappen

* [Werken met Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
