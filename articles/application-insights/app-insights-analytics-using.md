---
title: Met Analytics - een krachtige zoekprogramma van Azure Application Insights | Microsoft Docs
description: 'Met behulp van de Analytics, het hulpprogramma krachtige diagnostische gegevens doorzoeken van de Application Insights. '
services: application-insights
documentationcenter: 
author: danhadari
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: danha; mbullwin
ms.openlocfilehash: 0ca5c8b19f4699548a8551ec673e4a067d4e5fad
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="using-analytics-in-application-insights"></a>Door middel van analyses in Application Insights
[Analytics](app-insights-analytics.md) is de functie krachtige zoeken van [Application Insights](app-insights-overview.md). Deze pagina's worden de Log Analytics query language beschreven.

* **[Bekijk de video inleidende](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Uitprobeert Analytics op onze gesimuleerde gegevens](https://analytics.applicationinsights.io/demo)**  als uw app wordt niet van gegevens naar Application Insights nog verzenden.

## <a name="open-analytics"></a>Open Analytics
Klik op Analytics van uw app thuis resource in Application Insights.

![Open portal.azure.com open uw Application Insights-resource en klik op Analytics.](./media/app-insights-analytics-using/001.png)

De inline-zelfstudie hebt u enkele ideeën over wat u kunt doen.

Er is een [hier uitgebreider rondleiding](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>Uw telemetrie query
### <a name="write-a-query"></a>Een query schrijven
![Schema weergeven](./media/app-insights-analytics-using/150.png)

Beginnen met de namen van elk van de tabellen die aan de linkerkant worden vermeld (of de [bereik](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/range-operator) of [union](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/union-operator) operators). Gebruik `|` voor het maken van een pijplijn van [operators](https://docs.loganalytics.io/docs/Learn/References/Useful-operators). 

IntelliSense vraagt u met de operators en de expressie-elementen die u kunt gebruiken. Klik op het informatiepictogram (of druk op CTRL + SPATIEBALK) om op te halen van een langere beschrijving en voorbeelden van het gebruik van elk element.

Zie de [Analytics taal rondleiding](app-insights-analytics-tour.md) en [taalreferentie](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>Een query uitvoeren
![Een query uit te voeren](./media/app-insights-analytics-using/130.png)

1. U kunt één regeleinden gebruiken in een query.
2. Plaats de cursor in- of aan het einde van de query die u wilt uitvoeren.
3. Controleer het tijdsbereik van uw query. (U kunt deze wijzigen of deze door uw eigen overschrijven [ `where...timestamp...` ](https://docs.loganalytics.io/docs/Learn/Tutorials/Date-and-time-operations) -component in uw query.)
3. Klik op naar de query wilt uitvoeren.
4. Lege regels niet in uw query worden geplaatst. U kunt meerdere gescheiden query's in één query tabblad houden door deze te scheiden met een lege regels. Alleen de query met de cursor wordt uitgevoerd.

### <a name="save-a-query"></a>Een query opslaan
![Opslaan van een query](./media/app-insights-analytics-using/140.png)

1. Sla het huidige querybestand.
2. Open een opgeslagen query-bestand.
3. Maak een nieuwe querybestand.

## <a name="see-the-details"></a>Bekijk de details
Vouw een rij in de resultaten om te zien van de volledige lijst met eigenschappen. U kunt verder uitbreiden in elke eigenschap die is bijvoorbeeld een gestructureerde waarde -, aangepaste dimensies of de stack aanbieding in een uitzondering.

![Vouw een rij](./media/app-insights-analytics-using/070.png)

## <a name="arrange-the-results"></a>De resultaten ordenen
U kunt sorteren, filteren, pagineren en de resultaten van uw query te groeperen.

> [!NOTE]
> Sorteren, groeperen en filteren in de browser niet Voer de query opnieuw uit. Ze alleen de resultaten die zijn geretourneerd door de laatste query opnieuw te rangschikken. 
> 
> Om deze taken in de server uitvoeren voordat u de resultaten worden geretourneerd, schrijft u uw query met de [sorteren](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator), [samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) en [waar](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) operators.
> 
> 

Kies de kolommen die u wilt bekijken, sleept u de kolomkoppen om te rangschikken en kolombreedtes te slepen.

![Kolommen rangschikken](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Items sorteren en filteren
Uw resultaten sorteren door te klikken op de kop van een kolom. Klik op opnieuw als u wilt sorteren van de andere manier en klikt u op een derde tijdstip terugkeren naar de oorspronkelijke ordening geretourneerd door de query.

Gebruik het filterpictogram in de zoekactie.

![Kolommen sorteren en filteren](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a>Items groeperen
Gebruik op meer dan een kolom wilt sorteren, groeperen. Deze eerst inschakelen en sleep de kolomkoppen in de ruimte boven de tabel.

![Groep](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a>Ontbreken er bepaalde resultaten?

Als u denkt dat u niet de verwachte resultaten ziet, moet u er een aantal mogelijke redenen zijn.

* **Filter tijdsbereik**. Standaard worden alleen resultaten weergegeven in de afgelopen 24 uur. Er is een automatische filter dat het bereik van de resultaten die zijn opgehaald uit de brontabellen beperkt. 

    U kunt echter het tijdsbereik wijzigen filteren met behulp van de vervolgkeuzelijst.

    Of u kunt het automatische bereik vervangen door uw eigen [ `where  ... timestamp ...` component](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) in uw query. Bijvoorbeeld:

    `requests | where timestamp > ago('2d')`

* **Resultaten limiet**. Er is een limiet van ongeveer 10 kB rijen op de resultaten van de portal. Ziet u een waarschuwing als u gaan dan de limiet. Als dat gebeurt, won't sorteren van de resultaten in de tabel altijd weergeven u alle werkelijke eerste of laatste resultaten. 

    Het is raadzaam om te voorkomen dat roept de limiet. Het filter tijdsbereik gebruiken, of gebruik operators, zoals:

  * [Top 100 volgens de timestamp](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator) 
  * [100 duren](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/take-operator)
  * [samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) 
  * [waar tijdstempel > ago(3d)](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator)

(Meer dan 10 k rijen wilt? Overweeg het gebruik van [continue Export](app-insights-export-telemetry.md) in plaats daarvan. Analytics is ontworpen voor analyse, in plaats van onbewerkte gegevens op te halen.)

## <a name="diagrams"></a>Diagrammen
Selecteer het type diagram dat u wilt dat:

![Selecteer een diagramtype](./media/app-insights-analytics-using/230.png)

Als u meerdere kolommen van de juiste typen hebt, kunt u de x en y-as en een kolom van dimensies scheiden van de resultaten op.

Standaard resultaten in eerste instantie worden weergegeven als een tabel, en u handmatig het diagram selecteren. Maar u kunt de [renderen richtlijn](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) aan het einde van een query naar een diagram selecteren.

### <a name="analytics-diagnostics"></a>Analytics diagnostische gegevens


Op een timechart als er een plotselinge piek of de stap in uw gegevens, ziet u mogelijk een gemarkeerd punt op de regel. Hiermee wordt aangegeven dat een combinatie van eigenschappen die de onverwachte wijziging uitfilteren is geïdentificeerd door Analytics diagnostische gegevens. Klik op het punt om meer details op het filter en de gefilterde versie te zien. Hiermee kunt u bepalen wat de oorzaak van de wijziging. 

[Meer informatie over diagnostische gegevens Analytics](app-insights-analytics-diagnostics.md)


![Analytics diagnostische gegevens](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-to-dashboard"></a>Vastmaken aan dashboard
U kunt vastmaken een diagram of de tabel op een van uw [dashboards gedeeld](app-insights-dashboards.md) -Klik op de pincode. 

![Klik op de pincode](./media/app-insights-analytics-using/pin-01.png)

Dit betekent dat, wanneer u een dashboard om te controleren van de prestaties of het gebruik van uw webservices samenstellen, u erg complexe analyse naast de andere metrische gegevens opnemen kunt. 

Als er vier of minder kolommen, kunt u een tabel aan het dashboard vastmaken. Alleen de bovenste zeven rijen worden weergegeven.

### <a name="dashboard-refresh"></a>Dashboard vernieuwen
De grafiek vastgemaakt aan het dashboard wordt automatisch vernieuwd door de query opnieuw ongeveer elk uur wordt uitgevoerd. U kunt ook klikken op de knop vernieuwen.

### <a name="automatic-simplifications"></a>Automatische vereenvoudigen

Bepaalde vereenvoudigen worden toegepast op een grafiek wanneer u het vastmaken aan een dashboard.

**Tijd van de beperking:** query's zijn automatisch beperkt tot de afgelopen 14 dagen. Het effect is hetzelfde als wanneer de query bevat `where timestamp > ago(14d)`.

**Aantal beperking bin:** als u een grafiek met een groot aantal afzonderlijke opslaglocaties (meestal een staafdiagram) wordt weergegeven, de minder ingevuld opslaglocaties automatisch gegroepeerd in één bin 'anderen'. Bijvoorbeeld: deze query:

    requests | summarize count_search = count() by client_CountryOrRegion

Analytics weergegeven als volgt:

![Grafiek met lange staart](./media/app-insights-analytics-using/pin-07.png)

maar wanneer u deze aan een dashboard vastmaken, als volgt uitziet:

![Grafiek met beperkte opslaglocaties](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-to-excel"></a>Exporteren naar Excel
Nadat u een query uitvoert hebt, kunt u een CSV-bestand downloaden. Klik op **exporteren, Excel**.

## <a name="export-to-power-bi"></a>Exporteren naar Power BI
Plaats de cursor in een query en kies **exporteren, Power BI**.

![Exporteren van analytische gegevens met Power BI](./media/app-insights-analytics-using/240.png)

U voert de query in Power BI. U kunt dit instellen als volgens een schema vernieuwen.

U kunt dashboards die samenbrengen van gegevens uit een groot aantal bronnen maken met Power BI.

[Meer informatie over het exporteren naar Power BI](app-insights-export-power-bi.md)

## <a name="deep-link"></a>Dieptekoppeling

Een koppeling onder **uitvoer, koppeling van de Share** die u kunt zenden naar een andere gebruiker. Mits de gebruiker heeft [toegang tot uw resourcegroep](app-insights-resources-roles-access-control.md), de query wordt geopend in de gebruikersinterface Analytics.

(In de koppeling tekst van de query wordt weergegeven na '? q = ' gzip gecomprimeerd en base 64-codering. U kunt de code voor het genereren van dieptekoppelingen die u aan gebruikers biedt schrijven. De aanbevolen manier om het uitvoeren van analyses van code is echter met behulp van de [REST-API](https://dev.applicationinsights.io/).)


## <a name="automation"></a>Automatisering

Gebruik de [REST-API van Data Access](https://dev.applicationinsights.io/) Analytics query's uitvoeren. [Bijvoorbeeld](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (met PowerShell):

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

In tegenstelling tot de gebruikersinterface van Analytics, wordt de REST-API niet automatisch toegevoegd een timestamp-beperking aan de query's. Vergeet niet uw eigen where-component, om te voorkomen dat grote antwoorden toevoegen.



## <a name="import-data"></a>Gegevens importeren

U kunt gegevens importeren uit een CSV-bestand. Een gemiddelde gebruiksduur is om statische gegevens die u kunt deelnemen aan met tabellen uit uw telemetrie te importeren. 

Als de geverifieerde gebruikers worden geïdentificeerd in uw telemetrie met een alias of een verborgen-id, kunt u bijvoorbeeld een tabel die is toegewezen aliassen aan echte namen importeren. Door het uitvoeren van een join voor de aanvraagtelemetrie, kunt u gebruikers identificeren met hun echte naam in de Analytics-rapporten.

### <a name="define-your-data-schema"></a>Het gegevensschema van uw definiëren

1. Klik op **instellingen** (op linksboven) en vervolgens **gegevensbronnen**. 
2. Toevoegen van een gegevensbron, de instructies te volgen. U wordt gevraagd om op te geven van een steekproef van de gegevens die moet ten minste tien rijen bevatten. U kunt het schema corrigeren.

Hiermee definieert u een gegevensbron, waarin u vervolgens kunt afzonderlijke tabellen importeren.

### <a name="import-a-table"></a>Een tabel importeren

1. Open de definitie van de gegevensbron uit de lijst.
2. Klik op 'Uploaden' en volg de instructies voor het uploaden van de tabel. Hiervoor moet een aanroep van een REST-API en het is daarom gemakkelijk te automatiseren. 

De tabel is nu beschikbaar voor gebruik in Analytics query's. Deze wordt weergegeven in Analytics 

### <a name="use-the-table"></a>Gebruik de tabel

Stel de definitie van de gegevensbron wordt aangeroepen `usermap`, en dat er twee velden `realName` en `user_AuthenticatedId`. De `requests` tabel heeft ook een veld met de naam `user_AuthenticatedId`, zodat u gemakkelijk te koppelen:

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
De resulterende tabel van aanvragen is een extra kolom `realName`.

### <a name="import-from-logstash"></a>Importeren uit LogStash

Als u [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), kunt u Analytics query uitvoeren op uw Logboeken. Gebruik de [invoegtoepassing die gegevens doorgesluisd naar Analytics](https://github.com/Microsoft/logstash-output-application-insights). 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

