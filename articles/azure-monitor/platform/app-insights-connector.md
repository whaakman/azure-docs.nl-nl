---
title: Azure Application Insights-app-gegevens weergeven | Microsoft Docs
description: Prestatieproblemen analyseren en inzicht krijgen wat gebruikers doen met uw app bij bewaakt met Application Insights kunt u de oplossing Application Insights-Connector.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 49280cad-3526-43e1-a365-c6a3bf66db52
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: magoedte
ms.openlocfilehash: 5e6d750d62ccd31a85cbf47e13c12be078bcb2c4
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023121"
---
# <a name="application-insights-connector-management-solution-preview"></a>Application Insights-Connector-beheeroplossing (Preview)

![Application Insights-symbool](./media/app-insights-connector/app-insights-connector-symbol.png)

>[!NOTE]
> Met de ondersteuning van [query's voor meerdere bronnen](../../azure-monitor/log-query/cross-workspace-query.md), de oplossing Application Insights-connector is niet langer vereist en wordt afgeschaft. Vanaf juli kunt pas u weer nieuwe Application Insights-resources koppelen aan Log Analytics-werkruimten. Bestaande koppelingen en dashboards blijft functioneren tot November 2018. Zie voor meer informatie, [OMS-portal naar Azure verplaatst,](../../azure-monitor/platform/oms-portal-transition.md).

De Applications Insights-Connector-oplossing helpt u bij het vaststellen van prestatieproblemen en inzicht krijgen wat gebruikers doen met uw app wanneer deze wordt bewaakt met [Application Insights](../../application-insights/app-insights-overview.md). Weergaven van dezelfde toepassingstelemetrie die ontwikkelaars wordt weergegeven in Application Insights zijn beschikbaar in Log Analytics. Wanneer u uw Application Insights-apps met Log Analytics integreert, is echter zichtbaarheid van uw toepassingen met operationele gegevens en uw toepassing op één plek met verhoogd. Met de dezelfde weergaven, kunt u samenwerken met uw app-ontwikkelaars. De algemene weergaven kunt de tijd om te detecteren en oplossen van toepassings- en platformproblemen te beperken.

Wanneer u de oplossing gebruikt, kunt u het volgende doen:

- Al uw Application Insights-apps op één plek, weergeven, zelfs als ze zich in verschillende Azure-abonnementen
- Correleren van gegevens van de infrastructuur met toepassingsgegevens
- Toepassingsgegevens visualiseren met perspectieven in zoeken in Logboeken
- Schakel over naar uw Application Insights-app in Azure portal van Log Analytics-gegevens

## <a name="connected-sources"></a>Verbonden bronnen

Gegevens niet in tegenstelling tot de meeste andere Log Analytics-oplossingen, door agents verzameld voor de Application Insights-Connector. Alle gegevens die worden gebruikt door de oplossing wordt geleverd rechtstreeks vanuit Azure.

| Verbonden bron | Ondersteund | Description |
| --- | --- | --- |
| [Windows-agents](../../azure-monitor/platform/agent-windows.md) | Nee | De oplossing verzamelt geen informatie van Windows-agents. |
| [Linux-agents](../../azure-monitor/learn/quick-collect-linux-computer.md) | Nee | De oplossing worden geen gegevens verzameld van Linux-agents. |
| [SCOM-beheergroep](../../azure-monitor/platform/om-agents.md) | Nee | De oplossing worden geen gegevens verzameld van agents in een verbonden SCOM-beheergroep. |
| [Azure Storage-account](collect-azure-metrics-logs.md) | Nee | De oplossing doet niet Verzamelingsgegevens van Azure storage. |

## <a name="prerequisites"></a>Vereisten

- Voor toegang tot gegevens van Application Insights-Connector, moet u een Azure-abonnement hebt
- U moet ten minste één geconfigureerde Application Insights-resource hebben.
- U moet de eigenaar of bijdrager van de Application Insights-resource.

## <a name="configuration"></a>Configuratie

1. Inschakelen van de oplossing Azure Web Apps-analyse van de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ApplicationInsights?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen uit de galerie van oplossingen](../../azure-monitor/insights/solutions.md).
2. Blader naar de [Azure-portal](https://portal.azure.com). Selecteer **alle services** Application Insights openen. Zoek vervolgens naar Application Insights. 
3. Onder **abonnementen**, selecteert u een abonnement met Application Insights-resources en klik vervolgens onder **naam**, selecteer een of meer toepassingen.
4. Klik op **Opslaan**.

Gegevens beschikbaar in ongeveer 30 minuten, en de Application Insights-tegel wordt bijgewerkt met gegevens, zoals de volgende afbeelding:

![Application Insights-tegel](./media/app-insights-connector/app-insights-tile.png)

Andere punten moet rekening houden met:

- U kunt alleen Application Insights-apps koppelen aan een Log Analytics-werkruimte.
- U kunt alleen een koppeling [Basic of Enterprise Application Insights-resources](https://azure.microsoft.com/pricing/details/application-insights) naar Log Analytics. U kunt echter de gratis laag van Log Analytics gebruiken.

## <a name="management-packs"></a>Management packs

Deze oplossing kan niet alle management packs geïnstalleerd in de verbonden beheergroepen.

## <a name="use-the-solution"></a>De oplossing gebruiken

De volgende secties wordt beschreven hoe u de blades wordt weergegeven in de Application Insights-dashboard kunnen zien en gebruiken met gegevens van uw apps kunt gebruiken.

### <a name="view-application-insights-connector-information"></a>Application Insights-Connector-informatie weergeven

Klik op de **Application Insights** tegel om te openen de **Application Insights** dashboard om te zien van de volgende blades.

![Application Insights-dashboard](./media/app-insights-connector/app-insights-dash01.png)

![Application Insights-dashboard](./media/app-insights-connector/app-insights-dash02.png)

Het dashboard bevat de blades weergegeven in de tabel. Elke blade bevat maximaal tien items die overeenkomen met de criteria voor het opgegeven bereik en de duur van die blade. U kunt een logboekzoekopdracht die alle records retourneert wanneer u klikt op uitvoeren **alle** aan de onderkant van de blade of wanneer u klikt op de blade-header.


| **Kolom** | **Beschrijving** |
| --- | --- |
| Toepassingen - aantal toepassingen | Geeft het aantal toepassingen in Application-resources. Ook een lijst met toepassingsnamen en voor elk, het aantal toepassingsrecords. Klik op het aantal voor het uitvoeren van een logboekzoekopdracht voor <code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName</code> <br><br>  Klik op de toepassingsnaam van een om uit te voeren een zoeken in Logboeken voor de toepassing die toepassingsrecords per host, de records op telemetrietype en alle gegevens op type wordt (op basis van de laatste dag). |
| Gegevensvolume – als host fungeert voor verzenden van gegevens | Geeft het aantal computer fungeert als host die gegevens worden verzonden. Bevat ook computer fungeert als host en het aantal records voor elke host. Klik op het aantal voor het uitvoeren van een logboekzoekopdracht voor <code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by Host</code> <br><br> Klik op de naam van een computer om uit te voeren een zoeken in Logboeken voor de host waarin toepassingsrecords per host, de records op telemetrietype en alle gegevens op type (op basis van de laatste dag). |
| Beschikbaarheid – webtestresultaten | Geeft een ringdiagram voor de testresultaten web, waarmee wordt aangegeven geslaagd of mislukt. Klik op de grafiek voor het uitvoeren van een logboekzoekopdracht voor <code>ApplicationInsights &#124; where TelemetryType == "Availability" &#124; summarize AggregatedValue = sum(SampledCount) by AvailabilityResult</code> <br><br> Resultaten staat dat het aantal fasen en fouten voor alle tests. Hierin staan alle Web-Apps met verkeer voor de laatste minuut. Klik op de toepassingsnaam van een om een van de details van mislukte webtests zoeken in logboeken weer te geven. |
| Serveraanvragen – aanvragen per uur | Geeft een lijndiagram van de serveraanvragen per uur voor verschillende toepassingen. Beweeg de muisaanwijzer over een regel in de grafiek voor het ontvangen van aanvragen voor een punt in tijd top 3-toepassingen. Ook wordt een lijst van de toepassingen die zijn ontvangen van aanvragen en het aantal aanvragen voor de geselecteerde periode weergegeven. <br><br>Klik op de grafiek voor het uitvoeren van een logboekzoekopdracht voor <code>ApplicationInsights &#124; where TelemetryType == "Request" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> die ziet u een meer gedetailleerde lijndiagram van de serveraanvragen per uur voor verschillende toepassingen. <br><br> Klik op een toepassing in de lijst om uit te voeren van een logboekzoekopdracht voor <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code> die toont een lijst met aanvragen, grafieken voor aanvragen gedurende de duur van de tijd en de aanvraag en een lijst met aanvraag-responscodes.   |
| Fouten: mislukte aanvragen per uur | Geeft een lijndiagram van mislukte aanvragen per uur. Beweeg de muisaanwijzer over de grafiek om te zien van de eerste 3 toepassingen met mislukte aanvragen voor een punt in tijd. Ook ziet u een lijst met toepassingen met het aantal mislukte aanvragen voor elk. Klik op de grafiek voor het uitvoeren van een logboekzoekopdracht voor <code>ApplicationInsights &#124; where TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> die ziet u een meer gedetailleerde lijndiagram van mislukte aanvragen. <br><br>Klik op een item in de lijst om uit te voeren van een logboekzoekopdracht voor <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code> dat geeft mislukte aanvragen, grafieken voor mislukte aanvragen via de duur van de tijd en de aanvraag en een lijst van mislukte aanvragen responscodes. |
| Uitzonderingen: uitzonderingen per uur | Geeft een lijndiagram van uitzonderingen per uur. Beweeg de muisaanwijzer over de grafiek om te zien van de eerste 3 toepassingen met uitzonderingen voor een punt in tijd. Ook ziet u een lijst met toepassingen met het aantal uitzonderingen voor elk. Klik op de grafiek voor het uitvoeren van een logboekzoekopdracht voor <code>ApplicationInsights &#124; where TelemetryType == "Exception" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> die een meer gedetailleerde koppeling grafiek van uitzonderingen weergeeft. <br><br>Klik op een item in de lijst om uit te voeren van een logboekzoekopdracht voor <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Exception"</code> die toont een lijst met uitzonderingen, grafieken voor uitzonderingen ten opzichte van tijd en mislukte aanvragen en een lijst met uitzonderingstypen.  |

### <a name="view-the-application-insights-perspective-with-log-search"></a>Het perspectief van de Application Insights met zoeken in Logboeken weergeven

Wanneer u een item in het dashboard klikt, ziet u een Application Insights-perspectief wordt weergegeven in het zoekvak. Het perspectief biedt een uitgebreide visualisatie, op basis van het telemetrietype die geselecteerd. Dit het geval is, visualisatie inhoudswijzigingen voor verschillende telemetrietypen.

Wanneer u een willekeurige plaats in de blade toepassingen klikt, ziet u de standaard **toepassingen** perspectief.

![Application Insights toepassingen perspectief](./media/app-insights-connector/applications-blade-drill-search.png)

Het perspectief bevat een overzicht van de toepassing die u hebt geselecteerd.

De **beschikbaarheid** blade ziet u een ander perspectief-overzicht waarin u de resultaten van web- en verwante mislukte aanvragen kunt bekijken.

![Application Insights-beschikbaarheid perspectief](./media/app-insights-connector/availability-blade-drill-search.png)

Wanneer u klikt op een willekeurige plaats de **serveraanvragen** of **fouten** blades, de onderdelen perspectief wijzigen zodat u een visualisatie die betrekking op aanvragen hebben.

![Blade voor Application Insights-fouten](./media/app-insights-connector/server-requests-failures-drill-search.png)

Wanneer u klikt op een willekeurige plaats de **uitzonderingen** blade ziet u een visualisatie die afgestemd op uitzonderingen.

![Blade voor Application Insights-uitzonderingen](./media/app-insights-connector/exceptions-blade-drill-search.png)

Ongeacht of u klikken op iets een de **Application Insights-Connector** dashboard in de **zoeken** pagina zelf, query's retourneren van Application Insights-gegevens wordt de toepassing weergegeven. Insights-perspectief. Bijvoorbeeld, als u Application Insights-gegevens bekijkt een **&#42;** query geeft ook het tabblad perspectief, zoals de volgende afbeelding:

![Application Insights ](./media/app-insights-connector/app-insights-search.png)

Perspectief onderdelen zijn bijgewerkt, afhankelijk van de zoekquery. Dit betekent dat u kunt de resultaten filteren met behulp van een zoekveld waarmee u beschikt over de mogelijkheid om te zien van de gegevens uit:

- Al uw toepassingen
- Een enkele geselecteerde toepassing
- Een groep van toepassingen

### <a name="pivot-to-an-app-in-the-azure-portal"></a>Schakel over naar een app in Azure portal

Application Insights-Connector blades zijn ontworpen om te kunnen Schakel over naar de geselecteerde Application Insights-app *bij het gebruik van de Azure-portal*. U kunt de oplossing gebruiken als een platform voor het controleren van hoog niveau waarmee u een app op te lossen. Als u een potentieel probleem in een van uw verbonden toepassingen ziet, kunt u een van beide Zoom in op deze in Log Analytics search of u kunt draaien rechtstreeks naar de Application Insights-app.

Draaitabel, klikt u op de weglatingstekens (**...** ) die aan het einde van elke regel wordt weergegeven, en selecteer **openen in Application Insights**.

>[!NOTE]
>**Openen in Application Insights** is niet beschikbaar in de Azure-portal.

![Openen in Application Insights](./media/app-insights-connector/open-in-app-insights.png)

### <a name="sample-corrected-data"></a>Voorbeeld gecorrigeerde gegevens

Application Insights biedt *[steekproeven correctie](../../azure-monitor/app/sampling.md)* om te helpen Telemetrisch verkeer te verminderen. Als u sampling voor uw app in Application Insights inschakelt, krijgt u een kleiner aantal vermeldingen die zijn opgeslagen in Application Insights en Log Analytics. Terwijl de consistentie van gegevens wordt behouden in de **Application Insights-Connector** pagina's en perspectieven, u moet handmatig Corrigeer sample-gegevens voor uw aangepaste query's.

Hier volgt een voorbeeld van de correctie van steekproeven in een logboek zoekquery:

```
ApplicationInsights | summarize AggregatedValue = sum(SampledCount) by TelemetryType
```

De **steekproef aantal** veld aanwezig is in alle vermeldingen en toont het aantal gegevenspunten dat het item vertegenwoordigt. Als u densitysampling voor uw app in Application Insights inschakelen, **steekproef aantal** groter is dan 1. Som te tellen van het werkelijke aantal vermeldingen dat uw toepassing genereert, de **steekproef aantal** velden.

Steekproeven is van invloed op het totale aantal vermeldingen dat uw toepassing genereert. U hoeft niet te corrigeren sampling voor metrische velden als **RequestDuration** of **AvailabilityDuration** omdat het gemiddelde voor weergegeven vermeldingen in de velden weergegeven.

## <a name="input-data"></a>Invoergegevens

De oplossing ontvangt de volgende telemetrietypen van de gegevens van uw verbonden apps met Application Insights:

- Beschikbaarheid
- Uitzonderingen
- Aanvragen
- Paginaweergaven – voor uw werkruimte voor het ontvangen van paginaweergaven, moet u uw apps voor het verzamelen van gegevens configureren. Zie voor meer informatie, [PageViews](../../azure-monitor/app/api-custom-events-metrics.md#page-views).
- Aangepaste gebeurtenissen – voor uw werkruimte voor het ontvangen van aangepaste gebeurtenissen, moet u uw apps voor het verzamelen van gegevens configureren. Zie voor meer informatie, [TrackEvent](../../azure-monitor/app/api-custom-events-metrics.md#trackevent).

Gegevens worden ontvangen door Log Analytics van Application Insights zodra deze beschikbaar is.

## <a name="output-data"></a>Uitvoergegevens

Een record met een *type* van *ApplicationInsights* is gemaakt voor elk type invoergegevens. Application Insights-records hebben eigenschappen die worden weergegeven in de volgende secties:

### <a name="generic-fields"></a>Algemene velden

| Eigenschap | Description |
| --- | --- |
| Type | ApplicationInsights |
| ClientIP |   |
| TimeGenerated | Tijd van de record |
| ApplicationId | De instrumentatiesleutel van de Application Insights-app |
| ApplicationName | Naam van de Application Insights app |
| RoleInstance | ID van server-host |
| Apparaattype | Client-apparaat |
| ScreenResolution |   |
| Continent | Continent waarvan de aanvraag afkomstig is |
| Land | Land waar de aanvraag afkomstig is |
| Provincie | Provincie, provincie of land waar de aanvraag afkomstig is |
| Plaats | Stad of plaats waar de aanvraag afkomstig is |
| isSynthetic | Geeft aan of de aanvraag is gemaakt door een gebruiker of door de geautomatiseerde methode. True = gebruiker gegenereerde of = false geautomatiseerde methode |
| SamplingRate | Percentage van de telemetrie die is gegenereerd door de SDK die wordt verzonden naar de portal. Het bereik 0,0 100,0. |
| SampledCount | 100/(SamplingRate). Bijvoorbeeld, 4 =&gt; 25% |
| IsAuthenticated | Waar of onwaar |
| Bewerkings-id | Items die dezelfde bewerking ID als betrokken Items worden weergegeven in de portal. Meestal de aanvraag-ID |
| ParentOperationID | ID van de bovenliggende bewerking |
| OperationName |   |
| sessie-id | GUID voor het aanduiden van de sessie waarin de aanvraag is gemaakt |
| SourceSystem | ApplicationInsights |

### <a name="availability-specific-fields"></a>Beschikbaarheid-specifieke velden

| Eigenschap | Description |
| --- | --- |
| TelemetryType | Beschikbaarheid |
| AvailabilityTestName | Naam van de WebTest |
| AvailabilityRunLocation | Geografische bron van http-aanvraag |
| AvailabilityResult | Geeft aan dat het resultaat van het succes van de WebTest |
| AvailabilityMessage | Het bericht dat is gekoppeld aan de WebTest |
| AvailabilityCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| DataSizeMetricValue | 1.0 of 0,0 |
| DataSizeMetricCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| AvailabilityDuration | Tijd in milliseconden, van de duur van de website |
| AvailabilityDurationCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| AvailabilityValue |   |
| AvailabilityMetricCount |   |
| AvailabilityTestId | De unieke GUID voor de WebTest |
| AvailabilityTimestamp | Exacte tijdstempel van de beschikbaarheidstest |
| AvailabilityDurationMin | Dit veld geeft voor sample records weer van de minimale web testduur (in milliseconden) voor de gegevenspunten weergegeven |
| AvailabilityDurationMax | Voor sample records bevat dit veld maximale duur van de (in milliseconden) voor de gegevenspunten weergegeven |
| AvailabilityDurationStdDev | Voor sample records wordt in dit veld de standaarddeviatie tussen alle web test duur (in milliseconden) voor de gegevenspunten weergegeven |
| AvailabilityMin |   |
| AvailabilityMax |   |
| AvailabilityStdDev | &nbsp;  |

### <a name="exception-specific-fields"></a>Uitzondering-specifieke velden

| Type | ApplicationInsights |
| --- | --- |
| TelemetryType | Uitzondering |
| ExceptionType | Type van de uitzondering |
| ExceptionMethod | De methode die wordt gemaakt van de uitzondering |
| ExceptionAssembly | Assembly bevat het framework en versie, evenals het openbare-sleuteltoken |
| ExceptionGroup | Type van de uitzondering |
| ExceptionHandledAt | Geeft het niveau aan dat de uitzondering wordt verwerkt |
| ExceptionCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| ExceptionMessage | Bericht van uitzondering |
| ExceptionStack | Volledige stack van uitzondering |
| ExceptionHasStack | Waar, als uitzondering een stack heeft |



### <a name="request-specific-fields"></a>Aanvraag-specifieke velden

| Eigenschap | Description |
| --- | --- |
| Type | ApplicationInsights |
| TelemetryType | Aanvraag |
| ResponseCode | HTTP-antwoord verzonden naar client |
| RequestSuccess | Geeft aan of geslaagd of mislukt. Waar of ONWAAR. |
| Aanvraag-id | ID voor het aanduiden van de aanvraag |
| RequestName | GET/POST + URL-basis |
| RequestDuration | Tijd in seconden van de duur van de aanvraag |
| URL | URL van de aanvraag niet met inbegrip van host |
| Host | Web server-host |
| URLBase | Volledige URL van de aanvraag |
| ApplicationProtocol | Het type protocol dat wordt gebruikt door de toepassing |
| RequestCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| RequestDurationCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| RequestDurationMin | Dit veld geeft de duur van de minimale aanvraag (in milliseconden) voor sample records weer voor de gegevenspunten weergegeven. |
| RequestDurationMax | Dit veld geeft voor sample records, de duur van de maximale aanvraag (in milliseconden) weer voor de gegevenspunten weergegeven |
| RequestDurationStdDev | Dit veld geeft voor sample records, de standaardafwijking tussen alle aanvraag de duur (in milliseconden) weer voor de gegevenspunten weergegeven |

## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken

Deze oplossing beschikt niet over een aantal voorbeelden van zoekopdrachten op het dashboard weergegeven. Echter, voorbeeld log zoekquery's met beschrijvingen worden weergegeven in de [weergave Application Insights-Connector informatie](#view-application-insights-connector-information) sectie.

## <a name="next-steps"></a>Volgende stappen

- Gebruik [zoeken in logboeken](../../azure-monitor/log-query/log-query-overview.md) om gedetailleerde informatie voor uw Application Insights-apps weer te geven.
