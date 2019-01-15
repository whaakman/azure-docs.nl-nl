---
title: Application Insights voor Azure cloud services | Microsoft Docs
description: Controleer uw web- en werkrollen op een effectieve manier met Application Insights
services: application-insights
documentationcenter: ''
keywords: WAD2AI, Azure Diagnostics
author: mrbullwinkle
manager: carmonm
ms.assetid: 5c7a5b34-329e-42b7-9330-9dcbb9ff1f88
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.workload: tbd
ms.date: 09/05/2018
ms.author: mbullwin
ms.openlocfilehash: d27c0e9570959e01267d83a768ead45b48b7cea1
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54267229"
---
# <a name="application-insights-for-azure-cloud-services"></a>Application Insights voor Azure cloud services
[Application Insights] [ start] kunt bewaken [Azure cloud service-apps](https://azure.microsoft.com/services/cloud-services/) voor beschikbaarheid, prestaties, fouten en gebruik door het combineren van gegevens uit Application Insights-SDK's met [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) gegevens van uw cloudservices. Op basis van de feedback die u krijgt over de prestaties en de effectiviteit van uw app tijdens het gebruik, kunt u weldoordachte beslissingen nemen over de richting van het ontwerp in elke fase van de ontwikkelingslevenscyclus.

![Een dashboard](./media/cloudservices/overview-graphs.png)

## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u het volgende nodig:

* Een [Azure](https://azure.com) abonnement. Meld u aan met uw Microsoft-account voor Windows, Xbox Live of andere Microsoft-cloudservices. 
* Hulpprogramma's van Microsoft Azure 2.9 of hoger.
* Developer Analytics Tools 7.10 of hoger.

## <a name="get-started-quickly"></a>Snel aan de slag
Als u uw cloudservice wilt controleren met Application Insights, kunt u dat het snelst en gemakkelijkst doen door die optie te kiezen wanneer u uw service naar Azure publiceert.

![Voorbeeld van de diagnostische instellingen pagina](./media/cloudservices/azure-cloud-application-insights.png)

Deze optie op uw app tijdens runtime, zodat u de telemetrie die u nodig hebt voor het bewaken van aanvragen, uitzonderingen en afhankelijkheden in uw Webrol instrumenten. Deze bewaakt ook prestatiemeteritems van uw werkrollen. Een diagnostische traceringen die worden gegenereerd door uw app worden ook verzonden naar Application Insights.

Als deze optie Alles die wat u nodig is, bent u klaar. 

De volgende stappen zijn [weergeven van metrische gegevens van uw app](../../azure-monitor/app/metrics-explorer.md), [opvragen van uw gegevens met Analytics](../../azure-monitor/app/analytics.md), en misschien instellen van een [dashboard](../../azure-monitor/app/app-insights-dashboards.md). 

Voor het bewaken van prestaties in de browser, u kunt ook het instellen van [beschikbaarheidstests](../../azure-monitor/app/monitor-web-app-availability.md) en [Voeg code toe aan uw webpagina's](../../azure-monitor/app/javascript.md).

De volgende secties worden de volgende aanvullende opties besproken:

* Verzenden van gegevens uit verschillende onderdelen en buildconfiguraties om resources te scheiden.
* U kunt aangepaste telemetrie uit uw app toevoegen.

## <a name="sample-app-instrumented-with-application-insights"></a>Voorbeeld-app is geïnstrumenteerd met Application Insights
In deze [voorbeeldapp](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService), Application Insights is toegevoegd aan een cloudservice met twee werkrollen die worden gehost in Azure. 

In de volgende sectie leert u hoe u uw eigen cloudserviceproject op dezelfde manier aanpassen.

## <a name="plan-resources-and-resource-groups"></a>Resources en resourcegroepen plannen
De telemetrie van uw app is opgeslagen, geanalyseerd en weergegeven in een Azure-resource van het type Application Insights. 

Elke resource behoort tot een resourcegroep. Resourcegroepen worden gebruikt voor het beheren van kosten, om toegang te verlenen aan teamleden, en het implementeren van updates in één gecoördineerde transactie. Bijvoorbeeld, u kunt de onderstaande [voor het implementeren van een script schrijven](../../azure-resource-manager/resource-group-template-deploy.md) een Azure-cloud-service en de bijbehorende Application Insights-controleresources allemaal in één bewerking.

### <a name="resources-for-components"></a>Resources voor onderdelen
U wordt aangeraden dat u een afzonderlijke resource voor elk onderdeel van uw app maken. Dat wil zeggen, het maken van een resource voor elke Webrol en werkrol. U kunt elk onderdeel afzonderlijk analyseren, maar u maakt een [dashboard](../../azure-monitor/app/app-insights-dashboards.md) die combineert de belangrijkste grafieken van alle onderdelen, zodat u kunt vergelijken en ze samen in één weergave controleren. 

Een alternatieve methode is voor het verzenden van de telemetrie van meer dan één rol naar dezelfde resource, maar [een dimensie-eigenschap toevoegen aan elk telemetrie-item](../../azure-monitor/app/api-filtering-sampling.md#add-properties-itelemetryinitializer) die de oorspronkelijke rol aangeeft. In deze benadering grafieken met metrische gegevens, zoals uitzonderingen, weer normaal gesproken een aggregatie waarin de aantallen van de verschillende rollen, maar u kunt de grafiek segmenteren door de rol-id, indien nodig. U kunt zoekopdrachten ook filteren op dezelfde dimensie. Deze alternatieve werkwijze maakt het wat gemakkelijker om alles op hetzelfde moment weer te geven, maar kan ook leiden tot enige verwarring tussen de rollen.

Telemetrie van de browser wordt gewoonlijk opgenomen in dezelfde resource als de gegevens van de webrol op de server.

Plaats de Application Insights-resources voor de verschillende onderdelen in één resourcegroep. Deze aanpak kunt eenvoudig deze gezamenlijk beheren. 

### <a name="separate-development-test-and-production"></a>Ontwikkelings-, test- en productiegegevens scheiden
Als u aangepaste gebeurtenissen ontwikkelt voor uw volgende functie terwijl de vorige versie live is, kunt u de via telemetrie verzamelde ontwikkelingsgegevens het beste verzenden naar een afzonderlijke Application Insights-resource. Anders kan het moeilijk zijn te vinden van de telemetrie van uw test tussen al het verkeer van de live site.

Om te voorkomen dat deze situatie, maakt u afzonderlijke resources voor elke buildconfiguratie of 'stempel' (ontwikkeling, testen, productie, enzovoort) van uw systeem. Plaats de resources voor elke buildconfiguratie in een afzonderlijke resourcegroep. 

Voor het verzenden van de telemetrie naar de juiste resources, kunt u de Application Insights SDK instellen zodat deze een andere instrumentatiesleutel, afhankelijk van de buildconfiguratie neemt. 

## <a name="create-an-application-insights-resource-for-each-role"></a>Een Application Insights-resource maken voor elke rol

Als u hebt besloten om een afzonderlijke resource voor elke rol te maken en misschien een afzonderlijke voor elke buildconfiguratie set, is het gemakkelijkst te maken in de Application Insights-portal. Als u vaak resources maakt, kunt u [het proces automatiseren](../../azure-monitor/app/powershell.md).

1. In de [Azure-portal][portal], selecteer **nieuw** > **Ontwikkelaarsservices**  >   **Application Insights**.  

    ![Deelvenster voor Application Insights](./media/cloudservices/01-new.png)

1. In de **toepassingstype** vervolgkeuzelijst, selecteer **ASP.NET-webtoepassing**.  
    Elke resource wordt geïdentificeerd door een instrumentatiesleutel. Mogelijk moet u deze sleutel later als u handmatig wilt configureren of Controleer de configuratie van de SDK.


## <a name="set-up-azure-diagnostics-for-each-role"></a>Azure Diagnostics instellen voor elke rol
Stel deze optie in om uw app te controleren met Application Insights. Voor webrollen biedt deze optie prestatiebewaking, waarschuwingen, diagnostische gegevens en analyse van het Resourcegebruik. Voor andere rollen, kunt u zoeken naar en controleren van Azure Diagnostics zoals opnieuw opstarten, prestatiemeteritems en aanroepen van System.Diagnostics.Trace. 

1. In Visual Studio Solution Explorer, onder  **\<Uwcloudservice >** > **rollen**, opent u de eigenschappen van elke rol.

1. In **configuratie**, selecteer de **diagnostische gegevens verzenden naar Application Insights** selectievakje en selecteer vervolgens de Application Insights-resource die u eerder hebt gemaakt.

Als u hebt besloten om een afzonderlijke Application Insights-resource voor elke buildconfiguratie te gebruiken, selecteert u eerst de configuratie.

![Application Insights configureren](./media/cloudservices/configure-azure-diagnostics.png)

Dit is het effect van het invoegen van uw Application Insights-instrumentatiesleutels in de bestanden met de naam *ServiceConfiguration.\*. cscfg*. Hier volgt de [voorbeeldcode](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg).

Als u variëren van het niveau van diagnostische gegevens die worden verzonden naar Application Insights wilt, kunt u doen [door het bewerken van de *.cscfg* bestanden rechtstreeks](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md).

## <a name="sdk"></a>De SDK installeren in elk project
Met deze optie kunt u aangepaste zakelijke telemetrie toevoegen aan een rol. De optie biedt een nadere analyse van hoe uw app wordt gebruikt en wordt uitgevoerd.

Gebruik Visual Studio om de Application Insights-SDK voor elk cloudtoepassingsproject te configureren.

1. Het configureren van **webrollen**, met de rechtermuisknop op het project en selecteer vervolgens **Application Insights configureren** of **toevoegen > Application Insights telemetry**.

1. Het configureren van **werkrollen**: 

    a. Met de rechtermuisknop op het project en selecteer vervolgens **NuGet-pakketten beheren**.

    b. Voeg [Application Insights voor Windows Servers](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) toe.

    ![Naar Application Insights zoeken](./media/cloudservices/04-ai-nuget.png)

1. Voor het configureren van de SDK om gegevens te verzenden naar de Application Insights-resource:

    a. Stel in een geschikte opstartfunctie de instrumentatiesleutel van de configuratie-instelling in de *.cscfg* bestand:
 
    ```csharp
   
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```
   
    b. Herhaal ' stap een ' voor elke rol in uw app. Zie de voorbeelden:
   
    * [Webrol](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
    * [Werkrol](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
    * [Voor webpagina 's](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13) 

1. Stel de *ApplicationInsights.config* bestanden altijd worden gekopieerd naar de uitvoermap.  
    Een bericht in de *.config* bestand vraagt u de instrumentatiesleutel er plaatsen. Echter voor cloud-apps, is het beter om in te stellen via de *.cscfg* bestand. Deze aanpak zorgt ervoor dat de rol correct wordt geïdentificeerd in de portal.

#### <a name="run-and-publish-the-app"></a>De app uitvoeren en publiceren

1. Uitvoeren van uw app en zich aanmelden bij Azure. 

1. Open de Application Insights-resources die u hebt gemaakt.  
    Afzonderlijke gegevenspunten worden weergegeven in [zoeken](../../azure-monitor/app/diagnostic-search.md), en samengevoegde gegevens worden weergegeven in [Metric Explorer](../../azure-monitor/app/metrics-explorer.md). 

1. Meer telemetrie toevoegen (Zie de volgende secties) en publiceert u uw app om live diagnostische gegevens en gebruiksgegevens feedback ontvangen. 

Als er geen gegevens, het volgende doen:
1. Als u wilt weergeven van afzonderlijke gebeurtenissen, opent u de [zoeken] [ diagnostic] tegel.
1. Open verschillende pagina's in de app, zodat er telemetrie wordt gegenereerd.
1. Wacht een paar seconden en klik vervolgens op **vernieuwen**.  
    Zie voor meer informatie, [probleemoplossing][qna].

## <a name="view-azure-diagnostics-events"></a>Azure Diagnostics-gebeurtenissen weergeven
U vindt de [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) gegevens in Application Insights in de volgende locaties:

* Prestatiemeteritems worden weergegeven als aangepaste functies voor het verzamelen van metrische gegevens. 
* Windows-gebeurtenislogboeken worden als traceringen en aangepaste gebeurtenissen weergegeven.
* Toepassinglogboeken, ETW-logboeken en logboeken met diagnostische gegevens over de infrastructuur worden weergegeven als traceringen.

Voor prestatiemeteritems en aantallen gebeurtenissen opent [Metrics Explorer](../../azure-monitor/app/metrics-explorer.md) en voeg het volgende diagram:

![Azure Diagnostics-gegevens](./media/cloudservices/23-wad.png)

Als u wilt zoeken in de verschillende traceerlogboeken die door Azure Diagnostics zijn verzonden, gebruik [zoeken](../../azure-monitor/app/diagnostic-search.md) of een [analysequery](../../azure-monitor/log-query/get-started-portal.md). Stel bijvoorbeeld dat u hebt een niet-verwerkte uitzondering terechtgekomen, waardoor een rol is gecrasht en gerecycled. Deze informatie zou worden weergegeven in het toepassingskanaal van het Windows-gebeurtenislogboek. Search kunt u de Windows Event Log-fout weergeven en ophalen van de volledige stack-trace voor de uitzondering. In dat geval kunt u de hoofdoorzaak van het probleem.

![Zoeken in Azure Diagnostics](./media/cloudservices/25-wad.png)

## <a name="more-telemetry"></a>Meer telemetrie
In de volgende secties worden besproken hoe u aanvullende telemetrie ophalen uit verschillende aspecten van uw app.

## <a name="track-requests-from-worker-roles"></a>Aanvragen van werkrollen bijhouden
In webrollen worden door de aanvraagmodule automatisch gegevens over HTTP-aanvragen verzameld. Zie voor meer voorbeelden van hoe u het standaardverzamelgedrag kunt overschrijven de [voorbeeld-MVCWebRole](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole). 

U kunt de prestaties van aanroepen voor werkrollen vastleggen door ze op dezelfde manier bij te houden als HTTP-aanvragen. In Application Insights wordt met het telemetrietype Request een werkeenheid op een benoemde server gemeten die kan worden getimed en als onafhankelijk item kan slagen of mislukken. Hoewel HTTP-aanvragen automatisch worden vastgelegd door de SDK, kunt u uw eigen code voor het bijhouden van aanvragen voor werkrollen kunt invoegen.

Zie de twee voorbeeldwerkrollen geïnstrumenteerd voor het rapporteren van aanvragen: 
* [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA)
* [WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

## <a name="exceptions"></a>Uitzonderingen
Zie voor meer informatie over het verzamelen van onverwerkte uitzonderingen van verschillende web-app [bewaking van uitzonderingen in Application Insights](../../azure-monitor/app/asp-net-exceptions.md).

De voorbeeldwebrol heeft MVC5- en Web API 2-controllers. De onverwerkte uitzonderingen van de twee werkrollen worden vastgelegd met de volgende handlers:

* [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs) ingesteld voor MVC5-controllers [zoals wordt weergegeven in dit voorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) 
* [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs) ingesteld voor Web API 2-controllers [zoals wordt weergegeven in dit voorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) 

U kunt uitzonderingen op twee manieren kunt bijhouden voor werkrollen:

* Use TrackException(ex).
* Als u de Application Insights trace listener NuGet-pakket hebt toegevoegd, kunt u System.Diagnostics.Trace gebruiken om aan te melden uitzonderingen [zoals wordt weergegeven in dit voorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107).

## <a name="performance-counters"></a>Prestatiemeteritems
Voor de volgende prestatiemeteritems worden gegevens verzameld:

    * \Process(??APP_WIN32_PROC??)\% Processor Time
    * \Memory\Available Bytes
    * \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
    * \Process(??APP_WIN32_PROC??)\Private Bytes
    * \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
    * \Processor(_Total)\% Processor Time

Voor webrollen worden ook gegevens verzameld voor de volgende prestatiemeteritems:

    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue

U kunt extra aangepaste prestatiemeteritems of andere Windows-prestatiemeteritems opgeven door te bewerken *ApplicationInsights.config* [zoals wordt weergegeven in dit voorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14).

  ![Prestatiemeteritems](./media/cloudservices/002-servers.png)

## <a name="correlated-telemetry-for-worker-roles"></a>Gecorreleerde telemetrie voor werkrollen
U kunt bekijken wat heeft geleid tot een aanvraag is mislukt of een lange wachttijd voor een ervaring uitgebreide diagnostische gegevens. Voor webrollen, de SDK automatisch een een correlatie ingesteld tussen verwante telemetrie. 

Voor het bereiken van deze weergave voor werkrollen, kunt u een aangepaste telemetrie-initializer gebruiken om in te stellen van een gemeenschappelijk Operation.Id-contextkenmerk voor alle telemetrie. In dat geval kunt u weergeven in een oogopslag of de latentie of fout-probleem is veroorzaakt door een afhankelijkheid of uw code. 

Dit doet u al volgt:

* De correlationId instellen in een CallContext [zoals wordt weergegeven in dit voorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36). In dit geval gebruiken we de aanvraag-ID als de correlationId.
* Voeg een aangepaste TelemetryInitializer-implementatie, als u wilt de Operation.Id instellen op de correlatie dat eerder is ingesteld. Zie voor een voorbeeld [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13).
* Voeg de aangepaste telemetrie-initializer toe. U kunt hiervoor de *ApplicationInsights.config* bestand of in de code [zoals wordt weergegeven in dit voorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233).

## <a name="client-telemetry"></a>Telemetrie op de client
Zie om op te halen van de browser op basis van telemetriegegevens, zoals paginaweergaven, laadtijden voor pagina's of scriptuitzonderingen die, en voor aangepaste telemetrie schrijven in uw paginascripts [de JavaScript-SDK toevoegen aan uw webpagina's][client].

## <a name="availability-tests"></a>Beschikbaarheidstests
Om te controleren of uw app blijft live en responsief, [webtests instellen][availability].

## <a name="display-everything-together"></a>Een totaaloverzicht weergeven
Voor een algemeen beeld van uw systeem, kunt u de belangrijkste controlegrafieken samenbrengen op één weergeven [dashboard](../../azure-monitor/app/app-insights-dashboards.md). U kunt bijvoorbeeld het aantal aanvragen en het aantal fouten voor elke rol aan het dashboard vastmaken. 

Als uw systeem maakt gebruik van andere Azure-services zoals Stream Analytics, bevatten ook hun bewakingsgrafieken. 

Als u een mobiele client-app hebt, gebruikt u [App Center](../../azure-monitor/learn/mobile-center-quickstart.md). Maak query’s in [Analytics](../../azure-monitor/app/analytics.md) om de aantallen gebeurtenissen weer te geven en aan het dashboard vast te maken.

## <a name="example"></a>Voorbeeld
In [het voorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) wordt een service gecontroleerd die een webrol en twee werkrollen heeft.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Uitzondering "method is niet gevonden" over het uitvoeren in Azure cloudservices
Hebt u uw app ontwikkeld voor .NET 4.6? .NET 4.6 wordt niet automatisch ondersteund in Azure cloud services-rollen. [Installeer .NET 4.6 voor elke rol](../../cloud-services/cloud-services-dotnet-install-dotnet.md) voordat u uw app uitvoert.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen
* [Het verzenden van gegevens van Azure Diagnostics naar Application Insights configureren](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Application Insights-resources automatisch maken](../../azure-monitor/app/powershell.md)
* [Azure Diagnostics automatiseren](../../azure-monitor/app/powershell-azure-diagnostics.md)
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample)

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[azure]: ../../azure-monitor/app/app-insights-overview.md
[client]: ../../azure-monitor/app/javascript.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[netlogs]: ../../azure-monitor/app/asp-net-trace-logs.md
[portal]: https://portal.azure.com/
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md 
