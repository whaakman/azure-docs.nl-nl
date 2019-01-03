---
title: Profileer live Azure App Service-apps met Application Insights | Microsoft Docs
description: Profileer live-apps in Azure App Service met Application Insights Profiler.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: bc551d99528218f258acfd5d860795aae498d58f
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53653574"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Profileer live Azure App Service-apps met Application Insights

Profiler werkt momenteel voor ASP.NET en ASP.NET Core-apps die worden uitgevoerd op Azure App Service. De basis-servicelaag of hoger is vereist voor het gebruik van Profiler. Inschakelen van Profiler in Linux is momenteel alleen mogelijk via [deze methode](app-insights-profiler-aspnetcore-linux.md).

## <a id="installation"></a> Profiler inschakelen voor uw app
Als u wilt Profiler inschakelen voor een app, volg de onderstaande instructies. Als u een ander type Azure-service uitvoert, vindt hier u instructies voor het inschakelen van Profiler op andere ondersteunde platforms:
* [Cloud Services](app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-toepassingen](app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtuele machines](app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler is vooraf geïnstalleerd als onderdeel van de runtime van de App-Services, maar u moet inschakelen om get-profielen voor uw App Service-app. Nadat u een app hebt geïmplementeerd, zelfs als u hebt de App Insights-SDK opgenomen in de broncode, volgt u de stappen hieronder om de profiler inschakelen.

1. Ga naar de **App Services** deelvenster in de Azure-portal.
2. Navigeer naar **instellingen > Application Insights** deelvenster.

   ![App Insights inschakelen in App Services-portal](./media/app-insights-profiler/AppInsights-AppServices.png)

3. Ofwel de instructies in het deelvenster te maken van een nieuwe resource of Selecteer een bestaande App Insights-resource voor het bewaken van uw app. Controleer ook of de Profiler is **op**.

   ![App Insights-site-extensie toevoegen][Enablement UI]

4. Profiler is nu ingeschakeld met behulp van een App-instelling van de App-Services.

    ![App-instelling voor Profiler][profiler-app-setting]

## <a name="disable-profiler"></a>Profiler uitschakelen

Profiler voor een individuele app-exemplaar, onder opnieuw te starten of stoppen **-webtaken**, gaat u naar de app-resource. Als u wilt verwijderen van Profiler, gaat u naar **extensies**.

![Profiler uitschakelen voor een webtaak][disable-profiler-webjob]

Het is raadzaam dat u hebt ingeschakeld op al uw apps voor het detecteren van eventuele prestatieproblemen zo vroeg mogelijk Profiler.

Als u wijzigingen aan uw webtoepassing implementeren met web Deploy, zorg er dan voor dat u de map App_Data uitsluiten van wordt verwijderd tijdens de implementatie. De Profiler-extensie bestanden verwijderd anders de volgende keer dat u de web-App in Azure implementeren.



## <a name="next-steps"></a>Volgende stappen

* [Werken met Application Insights in Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)

[appinsights-in-appservices]:./media/app-insights-profiler/AppInsights-AppServices.png
[Enablement UI]: ./media/app-insights-profiler/Enablement_UI.png
[profiler-app-setting]:./media/app-insights-profiler/profiler-app-setting.png
[performance-blade]: ./media/app-insights-profiler/performance-blade.png
[performance-blade-examples]: ./media/app-insights-profiler/performance-blade-examples.png
[performance-blade-v2-examples]:./media/app-insights-profiler/performance-blade-v2-examples.png
[trace-explorer]: ./media/app-insights-profiler/trace-explorer.png
[trace-explorer-toolbar]: ./media/app-insights-profiler/trace-explorer-toolbar.png
[trace-explorer-hint-tip]: ./media/app-insights-profiler/trace-explorer-hint-tip.png
[trace-explorer-hot-path]: ./media/app-insights-profiler/trace-explorer-hot-path.png
[enable-profiler-banner]: ./media/app-insights-profiler/enable-profiler-banner.png
[disable-profiler-webjob]: ./media/app-insights-profiler/disable-profiler-webjob.png
[linked app services]: ./media/app-insights-profiler/linked-app-services.png

