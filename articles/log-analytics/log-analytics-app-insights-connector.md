---
title: Azure Application Insights-app-gegevens weergeven | Microsoft Docs
description: De oplossing Application Insights-Connector kunt u vaststellen van prestatieproblemen en u begrijpt wat gebruikers doen met uw app wanneer bewaakt met Application Insights.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 49280cad-3526-43e1-a365-c6a3bf66db52
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: banders
ms.openlocfilehash: fe6c003e095b25cf3ec3430fc68dcd399150b3ed
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="application-insights-connector-solution-preview-in-operations-management-suite-oms"></a>Application Insights-Connector oplossing (Preview) in de Operations Management Suite (OMS)

![Application Insights symbool](./media/log-analytics-app-insights-connector/app-insights-connector-symbol.png)

De toepassingen Insights-Connector-oplossing helpt u bij het vaststellen van prestatieproblemen en inzicht in wat gebruikers doen met uw app wanneer deze wordt bewaakt met [Application Insights](../application-insights/app-insights-overview.md). Weergaven van dezelfde toepassingstelemetrie die ontwikkelaars in Application Insights weergegeven zijn beschikbaar in OMS. Wanneer u uw Application Insights-apps met OMS integreert, wordt echter zichtbaarheid van uw toepassingen verhoogd met als bewerking en toepassingsgegevens op één plek. Met de dezelfde weergaven, kunt u samenwerken met uw app-ontwikkelaars. De algemene weergaven kunt minder tijd om te detecteren en oplossen van zowel de toepassing en de problemen met het platform.

Wanneer u de oplossing gebruikt, kunt u het volgende doen:

- Alle apps in uw Application Insights weergeven in één aanwezig is, zelfs wanneer ze zich in verschillende Azure-abonnementen
- Correleren van gegevens van de infrastructuur met toepassingsgegevens
- Toepassingsgegevens visualiseren met perspectieven in logboek zoeken
- Log Analytics-gegevens naar uw Application Insights-app in de OMS en Azure portals draaipunt

## <a name="connected-sources"></a>Verbonden bronnen

In tegenstelling tot de meeste andere Log Analytics-oplossingen, is niet gegevens voor de Application Insights-Connector verzameld door agents. Alle gegevens die worden gebruikt door de oplossing wordt geleverd rechtstreeks uit Azure.

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| [Windows-agents](log-analytics-windows-agents.md) | Nee | De oplossing verzamelt geen informatie van Windows-agents. |
| [Linux-agents](log-analytics-linux-agents.md) | Nee | De oplossing worden geen gegevens verzameld van Linux-agents. |
| [SCOM-beheergroep](log-analytics-om-agents.md) | Nee | De oplossing worden geen gegevens verzameld van agents in een verbonden SCOM-beheergroep. |
| [Azure Storage-account](log-analytics-azure-storage.md) | Nee | De oplossing biedt geen gegevens naar Azure storage. |

## <a name="prerequisites"></a>Vereisten

- Voor toegang tot informatie van de Application Insights-Connector, moet u een Azure-abonnement hebben
- U moet ten minste één geconfigureerde Application Insights-resource hebben.
- U moet de eigenaar of bijdrager van de Application Insights-resource.

## <a name="configuration"></a>Configuratie

1. Inschakelen van de Azure Web Apps Analytics-oplossing van de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ApplicationInsights?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).
2. Klik in de OMS-portal op **instellingen** &gt; **gegevens** &gt; **Application Insights**.
3. Onder **Selecteer een abonnement**, selecteer een abonnement met Application Insights-resources en klik vervolgens onder **toepassingsnaam**, selecteer een of meer toepassingen.
4. Klik op **Opslaan**.

Gegevens beschikbaar in ongeveer 30 minuten en de Application Insights-tegel wordt bijgewerkt met gegevens, zoals de volgende afbeelding:

![Application Insights-tegel](./media/log-analytics-app-insights-connector/app-insights-tile.png)

Andere punten rekening moet houden:

- U kunt alleen Application Insights-apps koppelen aan een OMS-werkruimte.
- U kunt alleen koppelen [Standard of Premium Application Insights-resources](https://azure.microsoft.com/pricing/details/application-insights) met OMS-logboekanalyse. U kunt echter de laag gratis van logboekanalyse.

## <a name="management-packs"></a>Management packs

Deze oplossing wordt niet geïnstalleerd voor alle management packs in de verbonden beheergroepen.

## <a name="use-the-solution"></a>De oplossing gebruiken

De volgende secties wordt beschreven hoe u kunt de blades weergegeven in de Application Insights-dashboard weergeven en interactie met gegevens van uw apps.

### <a name="view-application-insights-connector-information"></a>Informatie van de Application Insights-Connector weergeven

Klik op de **Application Insights** tegel openen de **Application Insights** dashboard om te zien van de volgende blades.

![Application Insights dashboard](./media/log-analytics-app-insights-connector/app-insights-dash01.png)

![Application Insights dashboard](./media/log-analytics-app-insights-connector/app-insights-dash02.png)

Het dashboard bevat de blades weergegeven in de tabel. Elke blade bevat maximaal 10 items die overeenkomen met de criteria die blade voor het opgegeven bereik en tijdsbereik. U kunt een zoekopdracht logboek waarmee alle records geretourneerd wanneer u klikt op uitvoeren **alle** aan de onderkant van de blade of wanneer u klikt op de blade-header.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

| **Kolom** | **Beschrijving** |
| --- | --- |
| Toepassingen - aantal toepassingen | Geeft het aantal toepassingen in de toepassingsbronnen. Een lijst met toepassingsnamen en voor elk, het aantal records van de toepassing. Klik op het uitvoeren van een logboek zoekt<code>Type=ApplicationInsights &#124; measure sum(SampledCount) by ApplicationName</code> <br><br>  Klik op de naam van een toepassing om uit te voeren een zoekopdracht logboek voor de toepassing waarin de records van de toepassing per host, records op type Telemetrie en alle gegevens op type (op basis van de laatste dag). |
| Gegevensvolume – als host fungeert voor verzenden van gegevens | Toont het aantal computer fungeert als host die van gegevens verzenden. Een lijst met computer fungeert als host en het aantal records voor elke host. Klik op het uitvoeren van een logboek zoekt<code>Type=ApplicationInsights &#124; measure sum(SampledCount) by Host</code> <br><br> Klik op de naam van een computer om uit te voeren een zoekopdracht logboek voor de host die records van de toepassing per host, records op type Telemetrie en alle gegevens op type (op basis van de laatste dag) worden weergegeven. |
| Beschikbaarheid – Webtest resultaten | Toont een ringdiagram voor web-testresultaten, waarmee wordt aangegeven op te geven of mislukken. Klik op de grafiek om uit te voeren een zoekopdracht logboek voor<code>Type=ApplicationInsights TelemetryType=Availability &#124; measure sum(SampledCount) by AvailabilityResult</code> <br><br> Het aantal fasen en fouten voor alle tests weergeven resultaten Deze bevat alle Web-Apps met verkeer voor de laatste minuut. Klik op een toepassingsnaam in een logboek zoekopdracht met details van de mislukte webtests weergeven. |
| Serveraanvragen – aanvragen per uur | Toont een lijndiagram van de serveraanvragen per uur voor verschillende toepassingen. Beweeg de muisaanwijzer over een regel in de grafiek voor de ontvangst van aanvragen voor een punt in tijd top-3-toepassingen. Ook wordt een lijst van de toepassingen die zijn ontvangen van aanvragen en het aantal aanvragen voor de geselecteerde periode. <br><br>Klik op de grafiek om uit te voeren een zoekopdracht logboek voor <code>Type=ApplicationInsights TelemetryType=Request &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> die ziet u een meer gedetailleerde lijndiagram van de serveraanvragen per uur voor verschillende toepassingen. <br><br> Klik op een toepassing in de lijst om uit te voeren een zoekopdracht logboek voor <code>Type=ApplicationInsights  ApplicationName=yourapplicationname  TelemetryType=Request</code> die bevat een overzicht van aanvragen, grafieken voor aanvragen gedurende de duur van de tijd en de aanvraag en een lijst van aanvraag responscodes.   |
| Fouten – mislukte aanvragen per uur | Toont een lijndiagram van mislukte aanvragen per uur. Beweeg de muisaanwijzer over de grafiek voor de eerste 3 toepassingen met mislukte aanvragen voor een punt in tijd. Ook wordt een lijst van toepassingen met het aantal mislukte aanvragen voor elk weergegeven. Klik op de grafiek om uit te voeren een zoekopdracht logboek voor <code>Type=ApplicationInsights TelemetryType=Request  RequestSuccess = false &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> die ziet u een meer gedetailleerde lijndiagram van mislukte aanvragen. <br><br>Klik op een item in de lijst om uit te voeren een zoekopdracht logboek voor <code>Type=ApplicationInsights ApplicationName=yourapplicationname TelemetryType=Request  RequestSuccess=false</code> dat toont mislukte aanvragen, grafieken voor mislukte aanvragen via de duur van de tijd en de aanvraag en een lijst met reactiecodes van mislukte aanvragen. |
| Uitzonderingen – uitzonderingen per uur | Toont een lijndiagram van uitzonderingen per uur. Beweeg de muisaanwijzer over de grafiek voor de eerste 3 toepassingen met uitzonderingen voor een punt in tijd. Ook wordt een lijst van toepassingen met het aantal uitzonderingen voor elk weergegeven. Klik op de grafiek om uit te voeren een zoekopdracht logboek voor <code>Type=ApplicationInsights TelemetryType=Exception &#124; measure sum(SampledCount) by ApplicationName interval 1hour</code> die bevat een meer gedetailleerde koppeling-grafiek met uitzonderingen. <br><br>Klik op een item in de lijst om uit te voeren een zoekopdracht logboek voor <code>Type=ApplicationInsights  ApplicationName=yourapplicationname TelemetryType=Exception</code> die wordt een lijst met uitzonderingen, diagrammen voor uitzonderingen via tijd en mislukte aanvragen en een lijst met typen.  |

### <a name="view-the-application-insights-perspective-with-log-search"></a>De Application Insights-perspectief met search logboek weergeven

Wanneer u een item in het dashboard klikt, ziet u een Application Insights-perspectief wordt weergegeven in de zoekopdracht. Het perspectief biedt een uitgebreide visualisatie, op basis van het type telemetrie dat geselecteerd. In dat geval visualisatie inhoudswijzigingen voor verschillende telemetrie typen.

Wanneer u een willekeurige plaats in de blade toepassingen klikt, ziet u de standaard **toepassingen** perspectief.

![Application Insights toepassingen perspectief](./media/log-analytics-app-insights-connector/applications-blade-drill-search.png)

Het perspectief geeft een overzicht van de toepassing die u hebt geselecteerd.

De **beschikbaarheid** blade ziet u een ander perspectiefweergave waarin u web testresultaten en verwante mislukte aanvragen kunt bekijken.

![Application Insights beschikbaarheid perspectief](./media/log-analytics-app-insights-connector/availability-blade-drill-search.png)

Wanneer u klikt op een willekeurige plaats de **serveraanvragen** of **fouten** blades, de onderdelen perspectief wijzigen zodat u een visualisatie in verband met aanvragen.

![Application Insights fouten blade](./media/log-analytics-app-insights-connector/server-requests-failures-drill-search.png)

Wanneer u klikt op een willekeurige plaats de **uitzonderingen** blade ziet u een visualisatie die aangepast aan de uitzonderingen.

![Application Insights uitzonderingen blade](./media/log-analytics-app-insights-connector/exceptions-blade-drill-search.png)

Ongeacht of u iets op een klikt de **Application Insights-Connector** dashboard in de **Search** pagina zelf, elke query retourneert gegevens van Application Insights ziet u de toepassing Insights perspectief. Als u gegevens van de Application Insights, bekijkt u bijvoorbeeld een **&#42;** query geeft ook het tabblad perspectief zoals de volgende afbeelding:

![Application Insights ](./media/log-analytics-app-insights-connector/app-insights-search.png)

Perspectief onderdelen zijn bijgewerkt, afhankelijk van de zoekopdracht. Dit betekent dat u kunt de resultaten filteren met behulp van een zoekveld waarmee u de mogelijkheid om te zien van de gegevens van:

- Al uw toepassingen
- Een geselecteerde toepassing
- Een groep van toepassingen

### <a name="pivot-to-an-app-in-the-azure-portal"></a>Draait in een app in de Azure-portal

Application Insights-Connector blades zijn ontworpen om u aan de geselecteerde Application Insights-app van draaipunt *wanneer u de OMS-portal gebruiken*. U kunt de oplossing gebruiken als een controle platform van hoog niveau die u helpt een app oplossen. Wanneer u een potentieel probleem in een van uw verbonden toepassingen zien, kunt u beide inzoomen in deze in de OMS-zoekopdracht of u rechtstreeks naar de Application Insights-app kunt draaien.

Als u wilt draaien, klikt u op de weglatingstekens (**...** ) die wordt weergegeven aan het einde van elke regel en selecteer **geopend in Application Insights**.

>[!NOTE]
>**Open in Application Insights** is niet beschikbaar in de Azure-portal.

![Open in Application Insights](./media/log-analytics-app-insights-connector/open-in-app-insights.png)

### <a name="sample-corrected-data"></a>Voorbeeld gecorrigeerd gegevens

Application Insights biedt  *[steekproef nemen correctie](../application-insights/app-insights-sampling.md)*  om u te helpen verminderen het verkeer van telemetrie. Als u steekproeven op uw Application Insights-app inschakelt, krijgt u een kleiner aantal vermeldingen die zijn opgeslagen in de Application Insights en in OMS. Terwijl de consistentie van de gegevens blijft behouden in de **Application Insights-Connector** pagina en perspectieven, u moet handmatig steekproefgegevens corrigeren voor uw aangepaste query's.

Hier volgt een voorbeeld van steekproeven correctie in een logboek zoekquery:

```
Type=ApplicationInsights | measure sum(SampledCount) by TelemetryType
```

De **aantal actieve** veld aanwezig is in alle vermeldingen en toont het aantal gegevenspunten dat het item vertegenwoordigt. Als u steekproeven voor uw app Application Insights inschakelt **aantal actieve** groter is dan 1. Som is opgegeven voor het tellen van het werkelijke aantal vermeldingen die uw toepassing genereert, de **aantal actieve** velden.

Steekproeven is van invloed op het totale aantal vermeldingen die uw toepassing genereert. U hoeft niet te corrigeren steekproeven voor metrische velden als **RequestDuration** of **AvailabilityDuration** omdat het gemiddelde voor weergegeven vermeldingen in de velden weergegeven.

## <a name="input-data"></a>Invoergegevens

De oplossing ontvangt de volgende typen telemetrie van gegevens van uw verbonden apps met Application Insights:

- Beschikbaarheid
- Uitzonderingen
- Aanvragen
- Paginaweergaven – voor uw werkruimte voor het ontvangen van paginaweergaven, moet u uw apps voor het verzamelen van gegevens configureren. Zie voor meer informatie [PageViews](../application-insights/app-insights-api-custom-events-metrics.md#page-views).
- Aangepaste gebeurtenissen – voor uw werkruimte aangepaste gebeurtenissen ontvangen, moet u uw apps voor het verzamelen van gegevens configureren. Zie voor meer informatie [TrackEvent](../application-insights/app-insights-api-custom-events-metrics.md#trackevent).

Gegevens worden ontvangen door OMS van Application Insights zodra deze beschikbaar is.

## <a name="output-data"></a>uitvoergegevens

Een record met een *type* van *ApplicationInsights* is gemaakt voor elk type invoergegevens. ApplicationInsights records hebben eigenschappen die worden weergegeven in de volgende secties:

### <a name="generic-fields"></a>Algemene velden

| Eigenschap | Beschrijving |
| --- | --- |
| Type | ApplicationInsights |
| client-IP |   |
| TimeGenerated | Tijd van de record |
| ApplicationId | De instrumentatiesleutel van de Application Insights-app |
| ApplicationName | Naam van de Application Insights app |
| RoleInstance | ID van server-host |
| DeviceType | Client-apparaat |
| ScreenResolution |   |
| Continent | Continent waarvan de aanvraag afkomstig is |
| Land | Land waar de aanvraag afkomstig is |
| Provincie | Provincie, status- of landinstelling waarvan de aanvraag afkomstig is |
| Plaats | Stad of plaats waar de aanvraag afkomstig is |
| isSynthetic | Hiermee wordt aangegeven of de aanvraag is gemaakt door een gebruiker of door automatische methode. = True gebruiker gegenereerde of ONWAAR = automatische methode |
| SamplingRate | Percentage van de telemetrie die is gegenereerd door de SDK die wordt verzonden naar de portal. Het bereik 0,0 100,0. |
| SampledCount | 100/(SamplingRate). Bijvoorbeeld, 4 =&gt; 25% |
| IsAuthenticated | Waar of onwaar |
| OperationID | Items die dezelfde bewerking-ID in de portal weergegeven als betrokken Items hebben. Meestal de aanvraag-ID |
| ParentOperationID | ID van de bovenliggende bewerking |
| OperationName |   |
| sessie-id | GUID voor het aanduiden van de sessie waarin de aanvraag is gemaakt |
| SourceSystem | ApplicationInsights |

### <a name="availability-specific-fields"></a>Beschikbaarheid-specifieke velden

| Eigenschap | Beschrijving |
| --- | --- |
| TelemetryType | Beschikbaarheid |
| AvailabilityTestName | Naam van de WebTest |
| AvailabilityRunLocation | Geografische bron van de http-aanvraag |
| AvailabilityResult | Geeft het resultaat van het succes van de WebTest |
| AvailabilityMessage | Het bericht dat is gekoppeld aan de WebTest |
| AvailabilityCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| DataSizeMetricValue | 1.0 of 0,0 |
| DataSizeMetricCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| AvailabilityDuration | Tijd in milliseconden van de duur van de test web |
| AvailabilityDurationCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| AvailabilityValue |   |
| AvailabilityMetricCount |   |
| AvailabilityTestId | De unieke GUID voor de WebTest |
| AvailabilityTimestamp | Nauwkeurig tijdstempel van de beschikbaarheidstest |
| AvailabilityDurationMin | Voor opgevangen records wordt in dit veld de minimale web testduur (milliseconden) voor de vertegenwoordigde gegevenspunten |
| AvailabilityDurationMax | Voor opgevangen records wordt in dit veld de maximale web testduur (milliseconden) voor de vertegenwoordigde gegevenspunten |
| AvailabilityDurationStdDev | Voor opgevangen records wordt in dit veld de standaardafwijking tussen alle web test duur (in milliseconden) voor de vertegenwoordigde gegevenspunten |
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
| ExceptionHandledAt | Geeft het niveau dat de uitzondering wordt verwerkt |
| ExceptionCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| ExceptionMessage | Bericht van de uitzondering |
| ExceptionStack | Volledige stack van de uitzondering |
| ExceptionHasStack | True als uitzondering een stack heeft |



### <a name="request-specific-fields"></a>Aanvraag-specifieke velden

| Eigenschap | Beschrijving |
| --- | --- |
| Type | ApplicationInsights |
| TelemetryType | Aanvraag |
| ResponseCode | HTTP-antwoord verzonden naar client |
| RequestSuccess | Hiermee wordt aangegeven of geslaagd of mislukt. Waar of ONWAAR. |
| Aanvraag-id | ID voor het aanduiden van de aanvraag |
| RequestName | GET/POST + URL-basis |
| RequestDuration | Tijd in seconden van de duur van aanvraag |
| URL | URL van de aanvraag niet met inbegrip van host |
| Host | Web server-host |
| URLBase | Volledige URL van de aanvraag |
| ApplicationProtocol | Type protocol dat wordt gebruikt door de toepassing |
| requestCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| RequestDurationCount | 100 /(Sampling Rate). Bijvoorbeeld, 4 =&gt; 25% |
| RequestDurationMin | Voor opgevangen records bevat dit veld de minimale aanvragenset duur (in milliseconden) voor de gegevenspunten dat wordt vertegenwoordigd. |
| RequestDurationMax | Dit veld bevat opgevangen records, de maximale aanvraag duur (in milliseconden) voor de vertegenwoordigde gegevenspunten |
| RequestDurationStdDev | Dit veld bevat opgevangen records, de standaardafwijking tussen alle aanvraag duur (in milliseconden) voor de vertegenwoordigde gegevenspunten |

## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken

Deze oplossing heeft geen een reeks voorbeeld logboek zoekopdrachten weergegeven op het dashboard. Voorbeeld logboek zoekquery's met beschrijvingen zijn echter weergegeven in de [weergave Application Insights-Connector informatie](#view-application-insights-connector-information) sectie.

## <a name="next-steps"></a>Volgende stappen

- Gebruik [logboek zoeken](log-analytics-log-searches.md) om gedetailleerde informatie voor uw Application Insights-apps te bekijken.
