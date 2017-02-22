---
title: Web-app-analyse voor ASP.NET instellen met Application Insights | Microsoft Docs
description: Configureer prestaties, beschikbaarheid en gebruiksanalyse voor uw ASP.NET-website die on-premises of in Azure wordt gehost.
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: douge
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/13/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 919f67a422faad2ba4c19e1f11f8e873098e8bd1
ms.openlocfilehash: 9c27cfb674a7743c7cfe47b35b263da48c9c564e


---
# <a name="set-up-application-insights-for-aspnet"></a>Application Insights instellen voor ASP.NET
Met [Azure Application Insights](app-insights-overview.md) wordt uw live-toepassing bewaakt om u te helpen bij het [detecteren en onderzoeken van prestatieproblemen en -uitzonderingen](app-insights-detect-triage-diagnose.md) en om [na te gaan hoe uw app wordt gebruikt](app-insights-overview-usage.md).  De tool werkt voor apps die worden gehost op uw eigen on-premises IIS-servers of op virtuele machines in de cloud, en voor Azure-webtoepassingen.

## <a name="before-you-start"></a>Voordat u begint
U hebt de volgende zaken nodig:

* Visual Studio 2013 update 3 of later. Later is beter.
* Een abonnement op [Microsoft Azure](http://azure.com). Als uw team of organisatie een Azure-abonnement heeft, kan de eigenaar u toevoegen met behulp van uw [Microsoft-account](http://live.com). 

Er zijn ook andere artikelen die u kunt bekijken als u geïnteresseerd bent in:

* [Een web-app instrumenteren tijdens runtime](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="a-nameidea-1-add-application-insights-sdk"></a><a name="ide"></a> 1. De Application Insights-SDK toevoegen
### <a name="if-its-a-new-project"></a>Als het een nieuw project is...
Wanneer u in Visual Studio een nieuw project maakt, controleert u of Application Insights is geselecteerd. 

![Een ASP.NET-project maken](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)

### <a name="-or-if-its-an-existing-project"></a>...of als het een bestaand project is
Klik in Solution Explorer met de rechtermuisknop op het project. Klik vervolgens op **Application Insights Telemetry toevoegen** of op **Application Insights configureren**.

![Add Application Insights kiezen](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* ASP.NET Core-project? - [Volg deze instructies voor het corrigeren van enkele coderegels](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs). 

## <a name="a-nameruna-2-run-your-app"></a><a name="run"></a> 2. Uw app uitvoeren
Start uw toepassing met F5 en probeer deze uit. Open verschillende pagina’s om telemetrie te genereren.

In Visual Studio ziet u het aantal gebeurtenissen dat is geregistreerd. 

![Tijdens het opsporen van fouten wordt in Visual Studio de knop Application Insights weergegeven.](./media/app-insights-asp-net/54.png)

## <a name="3-see-your-telemetry"></a>3. Uw telemetrie weergeven...
### <a name="-in-visual-studio"></a>...in Visual Studio
Open het Application Insights-venster in Visual Studio: klik op de knop Application Insights of klik met de rechtermuisknop op uw project in Solution Explorer, selecteer `Application Insights` en klik op `Search Live Telemetry`:

![Tijdens het opsporen van fouten wordt in Visual Studio de knop Application Insights weergegeven.](./media/app-insights-asp-net/55.png)

Deze weergave (‘Gegevens van foutopsporingssessie’) toont telemetrie die is gegenereerd in het servergedeelte van uw app. Experimenteer met de filters en klik op een gebeurtenis voor meer details.

* *Zijn er geen gegevens? Controleer of het tijdbereik correct is en klik op het pictogram Zoeken.*

[Meer informatie over Application Insights-hulpprogramma's in Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a> 

### <a name="-in-the-portal"></a>...in de portal
Tenzij u *Alleen SDK installeren* hebt gekozen, ziet u de telemetrie ook in de Application Insights-webportal. 

De portal biedt meer grafieken, hulpprogramma's voor analyse en dashboards dan Visual Studio. 

Open uw Application Insights-resource. Meld u aan bij [Azure Portal](https://portal.azure.com/) en zoek de resource daar of klik met de rechtermuisknop op het project in Visual Studio en laat u daarheen brengen.

![Met de rechtermuisknop op het project klikken en Azure Portal openen](./media/app-insights-asp-net/appinsights-04-openPortal.png)

* *Toegangsfout? Als u meer dan één set Microsoft-referenties hebt, bent u mogelijk aangemeld met de verkeerde set. Meld u af en opnieuw aan in de portal.*

De portal opent in een weergave van de telemetrie van uw app: ![Overzichtspagina Application Insights](./media/app-insights-asp-net/66.png)

Klik op een tegel of grafiek om meer details te zien.

### <a name="more-detail-in-the-portal"></a>Meer details in de portal

* [**Live Metrics Stream**](app-insights-live-stream.md) toont vrijwel onmiddellijk de telemetrie.

    ![Klik op de blade Overzicht op Live stream](./media/app-insights-asp-net/livestream.png)

    Open Live stream terwijl uw app wordt uitgevoerd, zodat ze verbinding kunnen maken.

    Live stream toont alleen telemetrie gedurende één minuut nadat deze is verzonden. Voor meer historisch onderzoek gebruikt u Zoeken, Metrics Explorer en Analytics. Het kan een paar minuten duren voordat er op deze locaties gegevens worden weergegeven.

* [**Zoeken** ](app-insights-diagnostic-search.md) toont individuele gebeurtenissen, zoals aanvragen, uitzonderingen en paginaweergaven. U kunt filteren op gebeurtenistype, overeenkomende termen en eigenschapswaarden. Klik op een gebeurtenis om de eigenschappen en verwante gebeurtenissen ervan weer te geven. 

    ![Klik op de blade Overzicht op Zoeken](./media/app-insights-asp-net/search.png)

 * In de ontwikkelingsmodus ziet u mogelijk veel afhankelijkheidsgebeurtenissen (AJAX). Dit zijn synchronisaties tussen de browser en de serveremulator. Als u deze wilt verbergen, klikt u op het filter Afhankelijkheid.
* In de grafieken worden [**samengevoegde metrische gegevens** ](app-insights-metrics-explorer.md), zoals aanvraag- en foutpercentages, weergegeven. Klik op een grafiek om een blade te openen met gedetailleerdere informatie. Klik op de tag **Bewerken** van een grafiek om filters, grootte en meer in te stellen.
    
    ![Klik op de blade Overzicht op een grafiek](./media/app-insights-asp-net/metrics.png)

[Meer informatie over het gebruik van Application Insights in Azure Portal](app-insights-dashboards.md).

## <a name="4-publish-your-app"></a>4. Uw app publiceren
Publiceer uw app op de IIS-server of op Azure. Bekijk de livestream met metrische gegevens in [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) om te controleren of alles goed werkt.

U kunt de opbouw van uw telemetrie volgen in de Application Insights-portal, waar u metrische gegevens kunt controleren, uw telemetrie kunt zoeken en [dashboards](app-insights-dashboards.md) kunt instellen. U kunt ook de krachtige [querytaal van Analytics](app-insights-analytics.md) gebruiken om het gebruik en de prestaties te analyseren of om specifieke gebeurtenissen te zoeken. 

U kunt uw telemetrie ook blijven analyseren in [Visual Studio](app-insights-visual-studio.md) met hulpprogramma's voor diagnostisch zoeken en [Trends](app-insights-visual-studio-trends.md).

> [!NOTE]
> Als uw app zoveel telemetrie verzendt dat de[beperkingslimieten](app-insights-pricing.md#limits-summary) worden benaderd, worden automatisch [steekproeven](app-insights-sampling.md) ingeschakeld. Met steekproeven vermindert u de hoeveelheid telemetrie die vanuit uw app wordt verzonden, maar behoudt u gecorreleerde gegevens voor diagnostische doeleinden.
> 
> 

## <a name="a-namelanda-what-did-add-application-insights-do"></a><a name="land"></a> Wat deed de opdracht Application Insights toevoegen?
Application Insights verzendt telemetrie van uw app naar de Application Insights-portal (die wordt gehost in Microsoft Azure):

![](./media/app-insights-asp-net/01-scheme.png)

De opdracht doet dus drie dingen:

1. Het NuGet-pakket uit de Application Insights Web-SDK wordt toegevoegd aan uw project. Klik met de rechtermuisknop op het project en kies NuGet-pakketten beheren om het pakket weer te geven in Visual Studio.
2. Er wordt een Application Insights-resource gemaakt in de [Azure Portal](https://portal.azure.com/). Hier ziet u de gegevens. De *instrumentatiesleutel*, die de resource definieert, wordt opgehaald.
3. De instrumentatiesleutel wordt in `ApplicationInsights.config` ingevoegd, zodat de SDK telemetrie naar de portal kan verzenden.

Als u wilt, kunt u deze stappen handmatig uitvoeren voor [ASP.NET 4](app-insights-windows-services.md) of [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).

### <a name="to-upgrade-to-future-sdk-versions"></a>Upgraden naar toekomstige SDK-versies
Als u wilt upgraden naar een [nieuwe release van de SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), opent u NuGet-pakketbeheer opnieuw en filtert u op geïnstalleerde pakketten. Selecteer Microsoft.ApplicationInsights.Web en kies Upgraden.

Als u aanpassingen in ApplicationInsights.config hebt aangebracht, slaat u hiervan een kopie op voordat u de upgrade uitvoert. Voeg vervolgens uw wijzigingen samen in de nieuwe versie.

## <a name="add-more-telemetry"></a>Meer telemetrie toevoegen
### <a name="dependencies-exceptions-and-performance-counters"></a>Afhankelijkheden, uitzonderingen en prestatiemeteritems

[Installeer Status Monitor](http://go.microsoft.com/fwlink/?LinkId=506648) op elke IIS-servermachine om extra telemetrie over uw web-apps te verkrijgen.

Als het programma al is geïnstalleerd, hoeft u verder niets te doen. 

Het kan zijn dat u Status Monitor al eerder hebt gebruikt, voor het bewaken van een app tijdens runtime. 

Als u Status Monitor gebruikt naast de buildtime-SDK, krijgt u meer volledige telemetrie, met:

* [Prestatiemeteritems](app-insights-performance-counters.md) - 
  Tellers voor CPU, geheugen, schijf en andere prestaties met betrekking tot uw app. 
* [Uitzonderingen](app-insights-asp-net-exceptions.md): gedetailleerdere telemetrie voor bepaalde uitzonderingen.
* [Afhankelijkheden](app-insights-asp-net-dependencies.md): inclusief retourwaarden.

### <a name="web-pages-and-single-page-apps"></a>Webpagina's en apps van één pagina
1. [Voeg het JavaScript-fragment toe](app-insights-javascript.md) aan uw webpagina's om de blades Browser en Gebruik te markeren met gegevens over paginaweergaven, laadtijden, browseruitzonderingen, prestaties van AJAX-aanroepen en gebruikers- en sessieaantallen.
2. [Codeer aangepaste gebeurtenissen](app-insights-api-custom-events-metrics.md) om gebruikersacties te tellen of (de tijdsduur ervan) te meten.


### <a name="diagnostic-code"></a>Diagnostische code
Hebt u een probleem? Als u code in uw app wilt invoegen om de app beter te diagnosticeren, hebt u verschillende opties:

* [Logboektraceringen vastleggen](app-insights-asp-net-trace-logs.md): als u al met Log4N, NLog of System.Diagnostics.Trace werkt om traceringsgebeurtenissen te registreren, kan de uitvoer worden verzonden naar Application Insights. U kunt deze dan in verband brengen met aanvragen, en u kunt ze doorzoeken en analyseren. 
* [Aangepaste gebeurtenissen en prestatiegegevens](app-insights-api-custom-events-metrics.md): gebruik TrackEvent() en TrackMetric() in server- of webpaginacode.
* [Telemetrie labelen met extra eigenschappen](app-insights-api-filtering-sampling.md#add-properties)

Gebruik [Zoeken](app-insights-diagnostic-search.md) om specifieke gebeurtenissen te zoeken en te correleren, en [Analyse](app-insights-analytics.md) om krachtigere query's uit te voeren.

## <a name="alerts"></a>Waarschuwingen
Wees als eerste op de hoogte als er problemen zijn met uw app. (Wacht niet totdat u het van uw gebruikers moet horen!) 

* [Maak webtests](app-insights-monitor-web-app-availability.md) om ervoor te zorgen dat uw site zichtbaar is op internet.
* [Proactieve diagnose](app-insights-proactive-diagnostics.md) wordt automatisch uitgevoerd (als uw app een bepaalde minimale hoeveelheid verkeer heeft). U hoeft niets te doen om dit in te stellen. Deze geeft aan of een app een ongebruikelijk aantal mislukte aanvragen heeft.
* [Stel metrische waarschuwingen in](app-insights-alerts.md) om u te waarschuwen als een metriek een drempelwaarde overschrijdt. U kunt deze instellen op aangepaste metrische gegevens die u in uw app codeert.

Standaard worden er waarschuwingen naar de eigenaar van het Azure-abonnement verstuurd. 

![voorbeeld van e-mailwaarschuwing](./media/app-insights-asp-net/alert-email.png)

## <a name="version-and-release-tracking"></a>Versie en release bijhouden
### <a name="track-application-version"></a>Toepassingsversie bijhouden
Zorg ervoor dat `buildinfo.config` door het MSBuild-proces wordt gegenereerd. Voeg het volgende toe in uw .csproj-bestand:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Wanneer de buildgegevens beschikbaar zijn, voegt de Application Insights-webmodule automatisch **Toepassingsversie** als eigenschap toe aan elk telemetrie-item. Dit biedt de mogelijkheid om op versie te filteren wanneer u [diagnostische zoekopdrachten](app-insights-diagnostic-search.md) uitvoert of [metrische gegevens verkent](app-insights-metrics-explorer.md). 

Let er echter op dat het buildversienummer alleen wordt gegenereerd door MS Build en niet door de ontwikkelaarsbuild in Visual Studio.

### <a name="release-annotations"></a>Release-aantekeningen
Als u Visual Studio Team Services gebruikt, kunt u [een aantekeningenmarkering ophalen](app-insights-annotations.md) die aan uw grafieken wordt toegevoegd wanneer u een nieuwe versie uitgeeft.

![voorbeeld van de release-aantekening](./media/app-insights-asp-net/release-annotation.png)

## <a name="next-steps"></a>Volgende stappen
|  |
| --- | --- |
| **[Met Application Insights werken in Visual Studio](app-insights-visual-studio.md)**<br/>Foutopsporing met telemetrie, het doorzoeken van diagnostische gegevens, het in detail analyseren van code. |
| **[Werken met de Application Insights-portal](app-insights-dashboards.md)**<br/>Dashboards, krachtige hulpprogramma's voor diagnose en analyse, waarschuwingen, een live afhankelijkheidskaart van uw toepassing en exportmogelijkheden voor telemetrie. |




<!--HONumber=Feb17_HO3-->


