---
title: Web-app-analyse voor ASP.NET instellen met Azure Application Insights | Microsoft Docs
description: Configureer prestaties, beschikbaarheid en gebruiksanalyse voor uw ASP.NET-website die on-premises of in Azure wordt gehost.
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 05b642949205d7698fbcf791a5d2f06528ff239e
ms.openlocfilehash: e827bddfa0bcc1c1e46d06c6856033e77859c7fb
ms.lasthandoff: 03/01/2017


---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>Application Insights instellen voor uw ASP.NET-website
Met [Azure Application Insights](app-insights-overview.md) wordt uw live-toepassing bewaakt om u te helpen bij het [detecteren en onderzoeken van prestatieproblemen en -uitzonderingen](app-insights-detect-triage-diagnose.md). Ook kunt u [controleren hoe uw app wordt gebruikt](app-insights-overview-usage.md). De tool werkt voor de Web Apps-functie van Azure App Service, en voor apps die worden gehost op uw eigen on-premises IIS-servers of op VM's in de cloud.

## <a name="before-you-start"></a>Voordat u begint
U hebt de volgende zaken nodig:

* Visual Studio 2013 update 3 of later. Later is beter.
* Een abonnement op [Microsoft Azure](http://azure.com). Als uw team of organisatie een Azure-abonnement heeft, kan de eigenaar u toevoegen met behulp van uw [Microsoft-account](http://live.com).

Er zijn ook andere onderwerpen die u kunt bekijken als u geïnteresseerd bent in:

* [Een web-app instrumenteren tijdens runtime](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="a-nameidea-step-1-add-the-application-insights-sdk"></a><a name="ide"></a> Stap 1: de Application Insights-SDK toevoegen

Klik in Solution Explorer met de rechtermuisknop op het web-app-project. Klik op **Toevoegen** en daarna op **Application Insights Telemetry** of **Application Insights configureren**.

![Schermopname van Solution Explorer waarin Application Insights Telemetry toevoegen is gemarkeerd](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(In Visual Studio 2015 is er ook een optie om Application Insights toe te voegen aan het dialoogvenster Nieuw project.)

Ga door naar de pagina voor Application Insights-configuratie:

![Schermopname van de pagina Uw app registreren bij Application Insights](./media/app-insights-asp-net/visual-studio-register-dialog.png)

1. Selecteer het account en het abonnement die u gebruikt voor toegang tot Azure.
2. Selecteer de resource in Azure die u wilt gebruiken om de gegevens van uw app te bekijken. Meestal maakt u voor elke app een afzonderlijke resource. Als u een resourcegroep of locatie wilt instellen voor het opslaan van uw gegevens, klikt u op **Instellingen configureren**. Resourcegroepen worden gebruikt om toegang tot gegevens te beheren. Als u verschillende apps hebt die deel uitmaken van hetzelfde systeem, kunt u hun Application Insights-gegevens in dezelfde resourcegroep plaatsen.
3. Application Insights is gratis te gebruiken, tot een bepaald telemetrievolume. U kunt een limiet instellen om kosten te voorkomen. Wanneer de resource is gemaakt, kunt u de selectie in de portal wijzigen door naar **Kenmerken + prijzen**, **Gegevensvolumebeheer**, **Dagelijks volumelimiet** te gaan.
4. Klik op **Registreren** om Application Insights te configureren voor uw web-app. Er wordt telemetrie verzonden naar [Azure Portal](https://portal.azure.com), zowel tijdens de foutopsporing als na het publiceren van de app.
5. U kunt ook alleen de Application Insights-SDK toevoegen aan uw app. In dit geval kunt u de telemetrie in Visual Studio bekijken tijdens de foutopsporing. U kunt later terugkeren naar deze configuratiepagina, of u kunt wachten tot de app is geïmplementeerd en [telemetrie inschakelen tijdens runtime](app-insights-monitor-performance-live-website-now.md).


## <a name="a-nameruna-step-2-run-your-app"></a><a name="run"></a> Stap 2: uw app uitvoeren
Voer uw app uit met F5. Open verschillende pagina's om telemetrie te genereren.

In Visual Studio ziet u het aantal gebeurtenissen dat is geregistreerd.

![Schermopname van Visual Studio. Tijdens het opsporen van fouten wordt de knop Application Insights weergegeven.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry-in-visual-studio-or-application-insights"></a>Stap 3: uw telemetrie bekijken in Visual Studio of Application Insights
U ziet uw telemetrie in Visual Studio of in de Application Insights-webportal.

Open het venster Application Insights **in Visual Studio**. Klik op de knop **Application Insights** of klik met de rechtermuisknop op uw project in Solution Explorer, selecteer vervolgens **Application Insights** en klik daarna op **Search Live Telemetry** (Live telemetrie zoeken).

Ga in het venster Visual Studio Application Insights naar de weergave **Gegevens van foutopsporingssessie** voor telemetrie die is gegenereerd aan de serverzijde van uw app. Experimenteer met de filters en klik op een gebeurtenis voor meer details.

![Schermopname van de weergave Gegevens van foutopsporingssessie in het venster Application Insights.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Als u geen gegevens ziet, controleert u of het tijdsbereik correct is en klikt u op het pictogram Zoeken.

[Meer informatie over Application Insights-hulpprogramma's in Visual Studio](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="the-application-insights-web-portal"></a>De Application Insights-webportal
Tenzij u ervoor hebt gekozen alleen de SDK te installeren, kunt u de telemetrie ook **in de Application Insights-webportal** bekijken. De portal biedt meer grafieken, hulpprogramma's voor analyse en dashboards dan Visual Studio.

Open uw Application Insights-resource. Meld u aan bij [Azure Portal](https://portal.azure.com/) en zoek de resource daar of klik met de rechtermuisknop op het project in Visual Studio en laat u daarheen brengen.

![Schermopname van Visual Studio die toont hoe u de Application Insights-portal kunt openen](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Als u een toegangsfout ontvangt, hebt u mogelijk meer dan één set Microsoft-referenties en bent u aangemeld met de verkeerde set. Meld u af en opnieuw aan in de portal.

De portal wordt geopend met een weergave van de telemetrie van uw app.
![Schermopname van Application Insights-overzichtspagina](./media/app-insights-asp-net/66.png)

Klik op een tegel of grafiek om meer details te zien.

### <a name="more-details-in-the-application-insights-web-portal"></a>Meer details in de Application Insights-webportal
Hier volgen enkele voorbeelden van hoe de portal u meer details kan geven.

* [**Live Metrics Stream**](app-insights-live-stream.md) toont vrijwel onmiddellijk de telemetrie.

    ![Schermopname van de portal. Klik op de blade Overzicht op Live Stream.](./media/app-insights-asp-net/livestream.png)

    Open Live Metrics Stream terwijl uw app wordt uitgevoerd, zodat ze verbinding kunnen maken.

    Live Metrics Stream toont alleen telemetrie gedurende één minuut nadat deze is verzonden. Voor meer historisch onderzoek gebruikt u Zoeken, Metrics Explorer en Analytics. Het kan een paar minuten duren voordat er op deze locaties gegevens worden weergegeven.

* [**Zoeken**](app-insights-diagnostic-search.md) toont individuele gebeurtenissen, zoals aanvragen, uitzonderingen en paginaweergaven. U kunt filteren op gebeurtenistype, overeenkomende termen en eigenschapswaarden. Klik op een gebeurtenis om de eigenschappen en verwante gebeurtenissen ervan weer te geven.

    ![Schermopname van de portal. Klik op de blade Overzicht op Zoeken.](./media/app-insights-asp-net/search.png)

 * In de ontwikkelingsmodus ziet u mogelijk veel afhankelijkheidsgebeurtenissen (AJAX). Dit zijn synchronisaties tussen de browser en de serveremulator. Als u deze wilt verbergen, klikt u op het filter **Afhankelijkheid**.
* In de grafieken worden [**samengevoegde metrische gegevens** ](app-insights-metrics-explorer.md), zoals aanvraag- en foutpercentages, weergegeven. Klik op een grafiek om een blade te openen met gedetailleerdere informatie. Klik op de tag **Bewerken** van een grafiek om filters en grootte in te stellen.

    ![Schermopname van blade met samengevoegde meetgegevens die beschikbaar is in de portal](./media/app-insights-asp-net/metrics.png)

[Meer informatie over het gebruik van Application Insights in Azure Portal](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>Stap 4: uw app publiceren
Publiceer uw app op de IIS-server of op Azure. Bekijk de livestream met metrische gegevens in [Live Metrics Stream](app-insights-metrics-explorer.md#live-metrics-stream) om te controleren of alles goed werkt.

Uw telemetrie wordt opgebouwd in de Application Insights-portal, waar u metrische gegevens kunt controleren, uw telemetrie kunt doorzoeken en [dashboards](app-insights-dashboards.md) kunt instellen. U kunt ook de krachtige [querytaal van Analytics](app-insights-analytics.md) gebruiken om gebruik en prestaties te analyseren of om specifieke gebeurtenissen te zoeken.

U kunt uw telemetrie ook blijven analyseren in [Visual Studio](app-insights-visual-studio.md) met hulpprogramma's voor diagnostisch zoeken en [Trends](app-insights-visual-studio-trends.md).

> [!NOTE]
> Als uw app zoveel telemetrie verzendt dat de[beperkingslimieten](app-insights-pricing.md#limits-summary) worden benaderd, worden automatisch [steekproeven](app-insights-sampling.md) ingeschakeld. Met steekproeven vermindert u de hoeveelheid telemetrie die vanuit uw app wordt verzonden, maar behoudt u gecorreleerde gegevens voor diagnostische doeleinden.
>
>

## <a name="a-namelanda-what-does-the-add-application-insights-command-do"></a><a name="land"></a>Wat doet de opdracht Application Insights toevoegen?
Application Insights verzendt telemetrie van uw app naar de Application Insights-portal (die wordt gehost in Azure).

![Diagram van verplaatsing van telemetrie](./media/app-insights-asp-net/01-scheme.png)

De opdracht doet dus drie dingen:

1. Het NuGet-pakket uit de Application Insights Web-SDK wordt toegevoegd aan uw project. Klik met de rechtermuisknop op het project en kies **NuGet-pakketten beheren** om het pakket weer te geven in Visual Studio.
2. Er wordt een Application Insights-resource gemaakt in [Azure Portal](https://portal.azure.com/). Hier ziet u de gegevens. De *instrumentatiesleutel*, die de resource definieert, wordt opgehaald.
3. De instrumentatiesleutel wordt in `ApplicationInsights.config` ingevoegd, zodat de SDK telemetrie naar de portal kan verzenden.

Als u wilt, kunt u deze stappen handmatig uitvoeren voor [ASP.NET 4](app-insights-windows-services.md) of [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started).

### <a name="upgrade-to-future-sdk-versions"></a>Upgraden naar toekomstige SDK-versies
Als u wilt upgraden naar een [nieuwe release van de SDK](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases), opent u **NuGet-pakketbeheer** opnieuw en filtert u op geïnstalleerde pakketten. Selecteer **Microsoft.ApplicationInsights.Web** en kies **Upgraden**.

Als u aanpassingen in ApplicationInsights.config hebt aangebracht, slaat u hiervan een kopie op voordat u de upgrade uitvoert. Voeg de wijzigingen vervolgens samen in de nieuwe versie.

## <a name="add-more-telemetry"></a>Meer telemetrie toevoegen
Hier volgen andere typen telemetrie die u kunt toevoegen.
### <a name="dependencies-exceptions-and-performance-counters"></a>Afhankelijkheden, uitzonderingen en prestatiemeteritems

[Installeer Status Monitor](http://go.microsoft.com/fwlink/?LinkId=506648) op elke IIS-servermachine om extra telemetrie over uw web-apps te verkrijgen. Als het programma al is geïnstalleerd, hoeft u verder niets te doen. (Het kan zijn dat u Status Monitor al eerder hebt gebruikt, voor het bewaken van een app tijdens runtime.)

Als u Status Monitor gebruikt naast de buildtime-SDK, krijgt u meer volledige telemetrie, met:

* [Prestatiemeteritems](app-insights-performance-counters.md): tellers voor CPU, geheugen, schijf en andere prestaties met betrekking tot uw app.
* [Uitzonderingen](app-insights-asp-net-exceptions.md): gedetailleerdere telemetrie voor bepaalde uitzonderingen.
* [Afhankelijkheden](app-insights-asp-net-dependencies.md): inclusief retourwaarden.

### <a name="webpages-and-single-page-apps"></a>Webpagina's en apps van één pagina
1. [Voeg het JavaScript-fragment toe](app-insights-javascript.md) aan uw webpagina's om gegevens weer te geven over paginaweergaven, laadtijden, browseruitzonderingen, prestaties van AJAX-aanroepen en gebruikers- en sessieaantallen. Deze worden weergegeven op de blades Browser en Gebruik.
2. [Codeer aangepaste gebeurtenissen](app-insights-api-custom-events-metrics.md) om gebruikersacties te tellen of (de tijdsduur ervan) te meten.


### <a name="diagnostic-code"></a>Diagnostische code
Hebt u een probleem? Als u code in uw app wilt invoegen om de app beter te diagnosticeren, hebt u verschillende opties:

* [Logboektraceringen vastleggen](app-insights-asp-net-trace-logs.md): als u al met Log4N, NLog of System.Diagnostics.Trace werkt om traceringsgebeurtenissen te registreren, kan de uitvoer worden verzonden naar Application Insights. U kunt deze uitvoer correleren met aanvragen, doorzoeken en analyseren.
* [Aangepaste gebeurtenissen en prestatiegegevens](app-insights-api-custom-events-metrics.md): gebruik TrackEvent() en TrackMetric() in server- of webpaginacode.
* [Telemetrie labelen met extra eigenschappen](app-insights-api-filtering-sampling.md#add-properties).

Gebruik [Zoeken](app-insights-diagnostic-search.md) om specifieke gebeurtenissen te zoeken en te correleren, en [Analyse](app-insights-analytics.md) om krachtigere query's uit te voeren.

## <a name="alerts"></a>Waarschuwingen
Wees als eerste op de hoogte als er problemen zijn met uw app.

* [Beschikbaarheidstests](app-insights-monitor-web-app-availability.md): maak tests om ervoor te zorgen dat uw site zichtbaar is op internet.
* [Slimme diagnostische gegevens](app-insights-proactive-diagnostics.md): deze tests worden automatisch uitgevoerd, zodat u niets hoeft te doen om ze in te stellen. Deze geeft aan of een app een ongebruikelijk aantal mislukte aanvragen heeft.
* [Metrische waarschuwingen](app-insights-alerts.md): stel deze in om u te waarschuwen als een metriek een drempelwaarde overschrijdt. U kunt deze instellen op aangepaste metrische gegevens die u in uw app codeert.

Standaard worden er waarschuwingen naar de eigenaar van het Azure-abonnement verstuurd.

![Schermopname van voorbeeld van waarschuwings-e-mail](./media/app-insights-asp-net/alert-email.png)

## <a name="version-and-release-tracking"></a>Versie en release bijhouden
Als u de toepassingsversie wilt bijhouden, zorgt u ervoor dat `buildinfo.config` wordt gegenereerd door het Microsoft Build Engine-proces. Voeg het volgende toe in uw .csproj-bestand:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Wanneer de buildgegevens beschikbaar zijn, voegt de Application Insights-webmodule automatisch **Toepassingsversie** als eigenschap toe aan elk telemetrie-item. Dit biedt de mogelijkheid om op versie te filteren wanneer u [diagnostische zoekopdrachten](app-insights-diagnostic-search.md) uitvoert of [metrische gegevens verkent](app-insights-metrics-explorer.md).

Let er echter op dat het buildversienummer alleen wordt gegenereerd door Microsoft Build Engine en niet door de ontwikkelaarsbuild in Visual Studio.

### <a name="release-annotations"></a>Release-aantekeningen
Als u Visual Studio Team Services gebruikt, kunt u [een aantekeningenmarkering ophalen](app-insights-annotations.md) die aan uw grafieken wordt toegevoegd wanneer u een nieuwe versie uitgeeft. De volgende afbeelding toont hoe deze markering wordt weergegeven.

![Schermopname van aantekening bij voorbeeldrelease in een grafiek](./media/app-insights-asp-net/release-annotation.png)

## <a name="next-steps"></a>Volgende stappen
**[Met Application Insights werken in Visual Studio](app-insights-visual-studio.md)**<br/>Bevat informatie over foutopsporing met telemetrie, het doorzoeken van diagnostische gegevens en het in detail analyseren van code.

**[Werken met de Application Insights-portal](app-insights-dashboards.md)**<br/> Bevat informatie over dashboards, krachtige hulpprogramma's voor diagnose en analyse, waarschuwingen, een live afhankelijkheidskaart van uw toepassing en exportmogelijkheden voor telemetrie.

