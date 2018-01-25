---
title: Application Insights voor Azure Cloud Services | Microsoft Docs
description: Controleer uw web- en werkrollen op een effectieve manier met Application Insights
services: application-insights
documentationcenter: 
keywords: WAD2AI, Azure Diagnostics
author: mrbullwinkle
manager: carmonm
editor: alancameronwills
ms.assetid: 5c7a5b34-329e-42b7-9330-9dcbb9ff1f88
ms.service: application-insights
ms.devlang: na
ms.tgt_pltfrm: ibiza
ms.topic: get-started-article
ms.workload: tbd
ms.date: 05/05/2017
ms.author: mbullwin
ms.openlocfilehash: dd35ef2239469be6ecf478f44a7e97389e5c5dd3
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="application-insights-for-azure-cloud-services"></a>Application Insights voor Azure Cloud Services
[Microsoft Azure Cloud-service-apps](https://azure.microsoft.com/services/cloud-services/) kunnen met [Application Insights][start] worden gecontroleerd op beschikbaarheid, prestaties, fouten en gebruik door gegevens uit de Application Insights-SDK's te combineren met [Azure Diagnotics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics)-gegevens uit uw cloudservices. Op basis van de feedback die u krijgt over de prestaties en de effectiviteit van uw app tijdens het gebruik, kunt u weldoordachte beslissingen nemen over de richting van het ontwerp in elke fase van de ontwikkelingslevenscyclus.

![Voorbeeld](./media/app-insights-cloudservices/sample.png)

## <a name="before-you-start"></a>Voordat u begint
U hebt het volgende nodig:

* Een abonnement op [Microsoft Azure](http://azure.com). Meld u aan met een Microsoft-account, dat u mogelijk al hebt voor Windows, XBox Live of andere Microsoft-cloudservices. 
* Hulpprogramma’s van Microsoft Azure 2.9 of hoger
* Developer Analytics Tools 7.10 of hoger

## <a name="quick-start"></a>Snel starten
Als u uw cloudservice wilt controleren met Application Insights, kunt u dat het snelst en gemakkelijkst doen door die optie te kiezen wanneer u uw service naar Azure publiceert.

![Voorbeeld](./media/app-insights-cloudservices/azure-cloud-application-insights.png)

Als u deze optie kiest, wordt uw app tijdens runtime geïnstrumenteerd, zodat u beschikt over alle telemetrie die u nodig hebt voor het controleren van aanvragen, uitzonderingen en afhankelijkheden in uw webrol en ook van de prestatiemeteritems van uw werkrollen. Ook diagnostische traceringen die worden gegenereerd door uw app, worden naar Application Insights verzonden.

Als dat alles is wat u nodig hebt, bent u klaar. De volgende stappen die u kunt nemen, zijn de [metrische gegevens van uw app bekijken](app-insights-metrics-explorer.md), [query’s uitvoeren op uw gegevens met Analytics](app-insights-analytics.md) en mogelijk een [dashboard](app-insights-dashboards.md) instellen. U kunt [beschikbaarheidstests](app-insights-monitor-web-app-availability.md) instellen en [code toevoegen aan uw webpagina’s](app-insights-javascript.md) om hun prestaties in de browser te controleren.

Maar u kunt desgewenst kiezen voor meer opties:

* U kunt gegevens verzenden vanuit verschillende onderdelen en buildconfiguraties om resources te scheiden.
* U kunt aangepaste telemetrie uit uw app toevoegen.

Bent u geïnteresseerd in deze opties, leest dan verder.

## <a name="sample-application-instrumented-with-application-insights"></a>Voorbeeldtoepassing die is geïnstrumenteerd met Application Insights
Kijk naar deze [voorbeeldtoepassing](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) waarin Application Insights is toegevoegd aan een cloudservice met twee werkrollen die worden gehost in Azure. 

Hierna wordt beschreven hoe u uw eigen cloudserviceproject op dezelfde manier kunt aanpassen.

## <a name="plan-resources-and-resource-groups"></a>Resources en resourcegroepen plannen
De telemetrie van uw app wordt opgeslagen, geanalyseerd en weergegeven in een Azure-resource van het Application Insights-type. 

Elke resource behoort tot een resourcegroep. Resourcegroepen worden gebruikt om de kosten te beheersen, teamleden toegang te geven en updates te implementeren in één gecoördineerde transactie. U kunt bijvoorbeeld in [een script schrijven voor het implementeren](../azure-resource-manager/resource-group-template-deploy.md) van zowel een Azure Cloud-service als de bijbehorende Application Insights-controleresources.

### <a name="resources-for-components"></a>Resources voor onderdelen
Het wordt aangeraden om een afzonderlijke resource te maken voor elk onderdeel van uw toepassing - dat wil zeggen voor elke webrol en werkrol. U kunt dan elk onderdeel afzonderlijk analyseren, maar een [dashboard](app-insights-dashboards.md) maken waarop u de belangrijkste grafieken van elk onderdeel samenbrengt, zodat u ze kunt vergelijken en ze in een oogopslag kunt controleren. 

Een alternatief is om de telemetrie van meerdere rollen naar dezelfde resource te verzenden, maar [een dimensie-eigenschap toe te voegen aan elk telemetrie-item](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer) die de oorspronkelijke rol aangeeft. Bij deze alternatieve werkwijze worden grafieken met metrische gegevens over bijvoorbeeld uitzonderingen weergegeven waarin de aantallen van de verschillende rollen zijn samengevoegd. U kunt deze grafiek echter segmenteren op basis van de rol-id, als dat nodig is. Resultaten van zoekacties kunnen ook worden gefilterd met behulp van dezelfde dimensie. Deze alternatieve werkwijze maakt het wat gemakkelijker om alles op hetzelfde moment weer te geven, maar kan ook leiden tot enige verwarring tussen de rollen.

Telemetrie van de browser wordt gewoonlijk opgenomen in dezelfde resource als de gegevens van de webrol op de server.

Plaats de Application Insights-resources voor de verschillende onderdelen in één resourcegroep. Zo kunt u ze gemakkelijk gezamenlijk beheren. 

### <a name="separating-development-test-and-production"></a>Ontwikkelings-, test- en productiegegevens scheiden
Als u aangepaste gebeurtenissen ontwikkelt voor uw volgende functie terwijl de vorige versie live is, kunt u de via telemetrie verzamelde ontwikkelingsgegevens het beste verzenden naar een afzonderlijke Application Insights-resource. Als u dat niet doet, is het lastig om de telemetrie van uw test terug te vinden in alle verkeer van de live site.

Om deze situatie te voorkomen, maakt u afzonderlijke resources voor elke buildconfiguratie of 'stempel' (ontwikkeling, test, productie,...) van uw systeem. Plaats de resources voor elke buildconfiguratie in een afzonderlijke resourcegroep. 

U kunt de telemetrie naar de juiste resources verzenden door de Application Insights-SDK zo in te stellen dat deze afhankelijk van de buildconfiguratie steeds een andere instrumentatiesleutel oppikt. 

## <a name="create-an-application-insights-resource-for-each-role"></a>Een Application Insights-resource maken voor elke rol
Als u hebt besloten om een afzonderlijke resource voor elke rol te maken - en misschien een afzonderlijke set voor elke buildconfiguratie - kunt u dat het gemakkelijkst doen door al deze resources in de Application Insights-portal te maken. (Als u vaak resources maakt, kunt u [het proces automatiseren](app-insights-powershell.md).

1. Maak in de [Azure Portal][portal] een nieuwe Application Insights-resource. Kies ASP.NET-app als het toepassingstype. 

    ![Klik op Nieuw > Application Insights](./media/app-insights-cloudservices/01-new.png)
2. U ziet dat elke resource een instrumentatiesleutel krijgt. Mogelijk hebt u deze sleutels later nodig als u de configuratie van de SDK handmatig wilt configureren of verifiëren.

    ![Op Eigenschappen klikken, de sleutel selecteren en op Ctrl + C drukken](./media/app-insights-cloudservices/02-props.png) 

## <a name="set-up-azure-diagnostics-for-each-role"></a>Azure Diagnostics instellen voor elke rol
Stel deze optie in om uw app te controleren met Application Insights. Voor webrollen zorgt u hiermee voor prestatiebewaking, waarschuwingen en diagnostische gegevens en een gebruiksanalyse. Voor andere rollen kunt u diagnostische gegevens van Azure doorzoeken en controleren, zoals gegevens over het opnieuw opstarten, gegevens van prestatiemeteritems en aanroepen van System.Diagnostics.Trace. 

1. Open in Visual Studio Solution Explorer onder &lt;UwCloudservice&gt;, Rollen de eigenschappen van elke rol.
2. Schakel in **Configuratie** de optie **Diagnostische gegevens naar Application Insights verzenden** in en selecteer de juiste Application Insights-resource die u eerder hebt gemaakt.

Als u hebt besloten om een afzonderlijke Application Insights-resource voor elke buildconfiguratie te gebruiken, selecteert u eerst de configuratie.

![Configureer Application Insights in de eigenschappen van elke Azure-rol.](./media/app-insights-cloudservices/configure-azure-diagnostics.png)

Hiermee voegt u uw Application Insights-instrumentatiesleutels in de bestanden met de naam `ServiceConfiguration.*.cscfg` in. ([Voorbeeldcode](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg)).

Als u het niveau van diagnostische gegevens die naar Application Insights worden verzonden, wilt variëren, kunt u dat doen [door de `.cscfg`-bestanden rechtstreeks](app-insights-azure-diagnostics.md) te bewerken.

## <a name="sdk"></a>De SDK installeren in elk project
Deze optie voegt de mogelijkheid toe om aangepaste zakelijke telemetrie toe te voegen aan elke gewenste rol, voor een nadere analyse van de manier waarop uw toepassing werkt en wordt gebruikt.

Gebruik Visual Studio om de Application Insights-SDK voor elk cloudtoepassingsproject te configureren.

1. **Webrollen**: Klik met de rechtermuisknop op het project en kies **Application Insights configureren** of **Toevoegen > Application Insights Telemetry**.

2. **Werkrollen**: 
 * Klik met de rechtermuisknop op het project en selecteer **NuGet-pakketten beheren**.
 * Voeg [Application Insights voor Windows Servers](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) toe.

    ![Naar Application Insights zoeken](./media/app-insights-cloudservices/04-ai-nuget.png)

3. Configureer de SDK om gegevens te verzenden naar de Application Insights-resource.

    Stel in een geschikte opstartfunctie de instrumentatiesleutel in vanuit de configuratie-instelling in het CSCFG-bestand:
 
    ```csharp
   
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```
   
    Doe dit voor elke rol in uw toepassing. Zie de voorbeelden:
   
   * [Webrol](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
   * [Werkrol](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
   * [Voor webpagina’s](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13) 
4. Stel het bestand ApplicationInsights.config zo in dat het altijd naar de uitvoermap wordt gekopieerd. 
   
    (In het CONFIG-bestand ziet u berichten waarin u wordt gevraagd de instrumentatiesleutel in dat bestand te plaatsen. Voor cloudtoepassingen is het echter beter om de sleutel in te stellen vanuit het CSCFG-bestand. Dit zorgt ervoor dat de rol correct wordt geïdentificeerd in de portal.)

#### <a name="run-and-publish-the-app"></a>De app uitvoeren en publiceren
Voer uw app uit en meld u aan bij Azure. Open de Application Insights-resources die u hebt gemaakt. U ziet dat in [Search](app-insights-diagnostic-search.md) de afzonderlijke gegevenspunten worden weergegeven en in [Metric Explorer](app-insights-metrics-explorer.md) de cumulatieve gegevens. 

Voeg meer telemetrie toe - zie de secties hieronder - en publiceer uw app vervolgens om live diagnostische gegevens en gebruiksfeedback te krijgen. 

#### <a name="no-data"></a>Zijn er geen gegevens?
* Open de [Search][diagnostic]-tegel om afzonderlijke gebeurtenissen te bekijken.
* Gebruik de toepassing om verschillende pagina's te openen, zodat er telemetrie wordt gegenereerd.
* Wacht een paar seconden en klik op Vernieuwen.
* Zie [Probleemoplossing][qna].

## <a name="view-azure-diagnostic-events"></a>Gebeurtenissen van Azure Diagnostics weergeven
Waar vind ik informatie van [Azure Diagnostics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics) in Application Insights:

* Prestatiemeteritems worden weergegeven als aangepaste functies voor het verzamelen van metrische gegevens. 
* Windows-gebeurtenislogboeken worden als traceringen en aangepaste gebeurtenissen weergegeven.
* Toepassinglogboeken, ETW-logboeken en logboeken met diagnostische gegevens over de infrastructuur worden weergegeven als traceringen.

Als u prestatiemeteritems en aantallen gebeurtenissen wilt zien, opent u [Metrics Explorer](app-insights-metrics-explorer.md) en voegt u een nieuwe grafiek toe:

![Gegevens van Azure Diagnostics](./media/app-insights-cloudservices/23-wad.png)

Gebruik [Search](app-insights-diagnostic-search.md) of een [Analytics-query](app-insights-analytics-tour.md) om te zoeken in de verschillende traceerlogboeken die door Azure Diagnostics zijn verzonden. Stel bijvoorbeeld dat u te maken krijgt met een onverwerkte uitzondering waardoor een rol is gecrasht en gerecycled. Deze informatie zou worden weergegeven in het toepassingskanaal van het Windows-gebeurtenislogboek. U kunt Search gebruiken om naar de fout in het Windows-gebeurtenislogboek te kijken en om de volledige stack-trace voor de uitzondering op te halen. Dit helpt u bij het vinden van de hoofdoorzaak van het probleem.

![Zoeken in Azure Diagnostics](./media/app-insights-cloudservices/25-wad.png)

## <a name="more-telemetry"></a>Meer telemetrie
In de onderstaande secties ziet u hoe u aanvullende telemetrie voor verschillende aspecten van uw toepassing kunt ophalen.

## <a name="track-requests-from-worker-roles"></a>Aanvragen van werkrollen bijhouden
In webrollen worden door de aanvraagmodule automatisch gegevens over HTTP-aanvragen verzameld. Zie de [voorbeeld-MVCWebRole](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole) voor manieren waarop u het standaardverzamelgedrag kunt overschrijven. 

U kunt de prestaties van aanroepen voor werkrollen vastleggen door ze op dezelfde manier bij te houden als HTTP-aanvragen. In Application Insights wordt met het telemetrietype Request een werkeenheid op een benoemde server gemeten die kan worden getimed en als onafhankelijk item kan slagen of mislukken. Hoewel HTTP-aanvragen automatisch worden vastgelegd door de SDK, kunt u uw eigen code invoegen voor het bijhouden van aanvragen voor werkrollen.

Zie de twee voorbeeldwerkrollen die zijn geïnstrumenteerd voor het rapporteren van aanvragen: [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA) en [WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

## <a name="exceptions"></a>Uitzonderingen
Zie [Uitzonderingen controleren in Application Insights](app-insights-asp-net-exceptions.md) als u wilt weten hoe u onverwerkte uitzonderingen kunt verzamelen van verschillende typen webtoepassingen.

De voorbeeldwebrol heeft MVC5- en Web API 2-controllers. De onverwerkte uitzonderingen van de twee werkrollen worden vastgelegd met de volgende handlers:

* [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs) [hier](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) instellen voor MVC5-controllers
* [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs) [hier](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) instellen voor Web API 2-controllers

Voor werkrollen zijn er twee manieren om uitzonderingen bij te houden:

* TrackException(ex)
* Als u het NuGet-pakket met traceringslisteners van Application Insights hebt toegevoegd, kunt u **System.Diagnostics.Trace** gebruiken om uitzonderingen in het logboek vast te leggen. [Codevoorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)

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

U kunt extra aangepaste prestatiemeteritems of andere Windows-prestatiemeteritems opgeven door ApplicationInsights.config te bewerken, [zoals in dit voorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14).

  ![Prestatiemeteritems](./media/app-insights-cloudservices/OLfMo2f.png)

## <a name="correlated-telemetry-for-worker-roles"></a>Gecorreleerde telemetrie voor werkrollen
Het is uiterst nuttig om bij het vaststellen van problemen precies te kunnen zien wat heeft geleid tot een mislukte aanvraag of een lange wachttijd na een aanvraag. Voor webrollen wordt door de SDK automatisch een correlatie ingesteld tussen verwante telemetrie. Voor werkrollen kunt u om dit te bereiken een aangepaste telemetrie-initializer gebruiken voor het instellen van een gemeenschappelijk Operation.Id-contextkenmerk voor alle telemetrie. Zo kunt u in een oogopslag zien of het probleem (de latentie of fout) is veroorzaakt door een afhankelijkheid of door uw code! 

Dit doet u al volgt:

* Stel de correlatie-id in een CallContext in, zoals [hier](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36) wordt weergegeven. In dit geval wordt de aanvraag-id gebruikt als de correlatie-id.
* Voeg een aangepaste TelemetryInitializer-implementatie toe om de Operation.Id in te stellen op de correlatie-id die u hiervoor hebt ingesteld. Hier vindt u een voorbeeld: [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13)
* Voeg de aangepaste telemetrie-initializer toe. U kunt dat doen in het bestand ApplicationInsights.config of in de code, zoals [hier](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233) wordt weergegeven

Dat is alles. De portal is zo ingericht dat u alle bijbehorende telemetrie in een oogopslag kunt zien:

![Gecorreleerde telemetrie](./media/app-insights-cloudservices/bHxuUhd.png)

## <a name="client-telemetry"></a>Telemetrie op de client
[Voeg de JavaScript-SDK toe aan uw webpagina’s][client] om telemetrie van de browser op te halen, zoals het aantal paginaweergaven, paginalaadtijden en scriptuitzonderingen, en om de mogelijkheid te krijgen om aangepaste telemetrie te schrijven in uw paginascripts.

## <a name="availability-tests"></a>Beschikbaarheidstests
[Stel webtests in][availability] om te controleren of de toepassing live en responsief blijft.

## <a name="display-everything-together"></a>Een totaaloverzicht weergeven
Als u een totaalbeeld van uw systeem wilt krijgen, kunt u de belangrijkste controlegrafieken samenbrengen op één [dashboard](app-insights-dashboards.md). U kunt bijvoorbeeld het aantal aanvragen en het aantal fouten voor elke rol aan het dashboard vastmaken. 

Als u in uw systeem andere Azure-services gebruikt, zoals Stream Analytics, kunt u ook de controlegrafieken daarvan weergeven. 

Als u een mobiele app voor clients hebt, voegt u code in om aangepaste gebeurtenissen te verzenden bij belangrijke gebruikersbewerkingen en maakt u een [HockeyApp Bridge](app-insights-hockeyapp-bridge-app.md). Maak query’s in [Analytics](app-insights-analytics.md) om de aantallen gebeurtenissen weer te geven en aan het dashboard vast te maken.

## <a name="example"></a>Voorbeeld
In [het voorbeeld](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) wordt een service gecontroleerd die een webrol en twee werkrollen heeft.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Uitzondering Methode niet gevonden bij het uitvoeren van uw app in Azure Cloud Services
Hebt u uw app ontwikkeld voor .NET 4.6? 4.6 wordt niet automatisch ondersteund in Azure Cloud Services-rollen. [Installeer 4.6 voor elke rol](../cloud-services/cloud-services-dotnet-install-dotnet.md) voordat u uw app uitvoert.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen
* [Het verzenden van gegevens van Azure Diagnostics naar Application Insights configureren](app-insights-azure-diagnostics.md)
* [Het maken van Application Insights-resources automatiseren](app-insights-powershell.md)
* [Azure Diagnostics automatiseren](app-insights-powershell-azure-diagnostics.md)
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample)

[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[azure]: app-insights-azure.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[netlogs]: app-insights-asp-net-trace-logs.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md 
