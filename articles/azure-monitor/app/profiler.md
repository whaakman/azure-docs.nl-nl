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
ms.openlocfilehash: 1d902f8d87b81fda9edf2c1c37ad40416ac6baf2
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54028302"
---
# <a name="profile-live-azure-app-service-apps-with-application-insights"></a>Profileer live Azure App Service-apps met Application Insights

Profiler werkt momenteel voor ASP.NET en ASP.NET Core-apps die worden uitgevoerd op Azure App Service. De basis-servicelaag of hoger is vereist voor het gebruik van Profiler. Inschakelen van Profiler in Linux is momenteel alleen mogelijk via [deze methode](profiler-aspnetcore-linux.md).

## <a id="installation"></a> Profiler inschakelen voor uw app
Als u wilt Profiler inschakelen voor een app, volg de onderstaande instructies. Als u een ander type Azure-service uitvoert, vindt hier u instructies voor het inschakelen van Profiler op andere ondersteunde platforms:
* [Cloud Services](../../application-insights/app-insights-profiler-cloudservice.md?toc=/azure/azure-monitor/toc.json)
* [Service Fabric-toepassingen](../../application-insights/app-insights-profiler-servicefabric.md?toc=/azure/azure-monitor/toc.json)
* [Virtuele machines](../../application-insights/app-insights-profiler-vm.md?toc=/azure/azure-monitor/toc.json)

Application Insights Profiler is vooraf geïnstalleerd als onderdeel van de runtime van de App-Services, maar u moet inschakelen om get-profielen voor uw App Service-app. Nadat u een app hebt geïmplementeerd, zelfs als u hebt de App Insights-SDK opgenomen in de broncode, volgt u de stappen hieronder om de profiler inschakelen.

1. Ga naar de **App Services** deelvenster in de Azure-portal.
2. Navigeer naar **instellingen > Application Insights** deelvenster.

   ![App Insights inschakelen in App Services-portal](./media/profiler/AppInsights-AppServices.png)

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

[Enablement UI]: ./media/profiler/Enablement_UI.png
[profiler-app-setting]:./media/profiler/profiler-app-setting.png
[disable-profiler-webjob]: ./media/profiler/disable-profiler-webjob.png
