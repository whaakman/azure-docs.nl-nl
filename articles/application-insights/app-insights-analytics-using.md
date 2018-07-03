---
title: Met behulp van Analytics - de krachtige zoekfunctie van Azure Application Insights | Microsoft Docs
description: 'De analytische gegevens, het hulpprogramma krachtige diagnostische gegevens doorzoeken van Application Insights gebruiken. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 07/02/2018
ms.reviewer: danha
ms.author: mbullwin
ms.openlocfilehash: aa86e2f3b1fb147ab167c948475a5207693143c2
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341559"
---
# <a name="using-analytics-in-application-insights"></a>Met behulp van analyses in Application Insights
[Analytics](app-insights-analytics.md) is van de krachtige zoekfunctie van [Application Insights](app-insights-overview.md). Deze pagina's beschrijven de querytaal van Log Analytics.

* **[Bekijk de inleidende video](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Probeer de Analytics op onze gesimuleerde gegevens](https://analytics.applicationinsights.io/demo)**  als uw app wordt niet van gegevens naar Application Insights nog verzenden.

## <a name="open-analytics"></a>Analytics openen
Klik op Analytics van home resource in Application Insights van uw app.

![Portal.azure.com Open, open uw Application Insights-resource en klik op Analytics.](./media/app-insights-analytics-using/001.png)

De inline-zelfstudie hebt u enkele ideeën over wat u kunt doen.

Er is een [hier uitgebreider rondleiding](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>Query uitvoeren op uw telemetrie
### <a name="write-a-query"></a>Een query schrijven
![Schema weergeven](./media/app-insights-analytics-using/150.png)

Beginnen met de namen van een van de tabellen aan de linkerkant weergegeven (of de [bereik](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/range-operator) of [union](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/union-operator) operators). Gebruik `|` te maken van een pijplijn van [operators](https://docs.loganalytics.io/docs/Learn/References/Useful-operators). 

IntelliSense vraagt u met de operators en de expressie-elementen die u kunt gebruiken. Klik op het informatiepictogram (of druk op CTRL + spatie) om een langere beschrijving en voorbeelden over het gebruik van elk element te verkrijgen.

Zie de [Analytics taal rondleiding](app-insights-analytics-tour.md) en [taalverwijzing](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>Een query uitvoeren
![Een query uit te voeren](./media/app-insights-analytics-using/130.png)

1. U kunt één regeleinden gebruiken in een query.
2. Plaats de cursor binnen of aan het einde van de query die u wilt uitvoeren.
3. Controleer het tijdsbereik van de query. (U kunt deze wijzigen of deze overschrijven door te nemen van uw eigen [ `where...timestamp...` ](https://docs.loganalytics.io/docs/Learn/Tutorials/Date-and-time-operations) -component in uw query.)
3. Klik op Ga naar de query wilt uitvoeren.
4. Plaats geen lege regels in uw query. U kunt verschillende gescheiden query's in één query-tabblad houden door deze te scheiden met lege regels. Alleen de query waaraan de cursor wordt uitgevoerd.

### <a name="save-a-query"></a>Een query opslaan
![Een query opslaan](./media/app-insights-analytics-using/140.png)

1. Sla het huidige querybestand.
2. Open een opgeslagen query-bestand.
3. Maak een nieuwe querybestand.

## <a name="see-the-details"></a>Bekijk de details
Vouw een rij in de resultaten om te zien van de volledige lijst met eigenschappen. U kunt verder uitbreiden in elke eigenschap die is bijvoorbeeld een gestructureerde waarde -, aangepaste dimensies of de aanbieding in een uitzondering-stack.

![Een rij uitvouwen](./media/app-insights-analytics-using/070.png)

## <a name="arrange-the-results"></a>De resultaten ordenen
U kunt sorteren, filteren, pagineren en de resultaten geretourneerd door de query groeperen.

> [!NOTE]
> Sorteren, te groeperen en te filteren in de browser niet Voer uw query opnieuw uit. Ze alleen de resultaten die zijn geretourneerd door de laatste query opnieuw te rangschikken. 
> 
> Om uit te voeren deze taken op de server voordat de resultaten worden geretourneerd, schrijft u uw query's uitvoeren met de [sorteren](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator), [samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) en [waar](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) operators.
> 
> 

Kies de kolommen die u wilt zien, sleept u de kolomkoppen om te rangschikken en breedte van kolommen te slepen.

![Kolommen rangschikken](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Artikelen sorteren en filteren
Uw resultaten sorteren door te klikken op de kop van een kolom. Klik op opnieuw om te sorteren van de andere manier en klikt u op een derde tijd om terug te keren naar de oorspronkelijke volgorde geretourneerd door de query.

Gebruik het filterpictogram om uw zoekactie te beperken.

![Kolommen sorteren en filteren](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a>Items groeperen
Op meer dan één kolom wilt sorteren, groeperen te gebruiken. Het eerst inschakelen en sleep de kolomkoppen in de ruimte boven de tabel.

![Groep](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a>Ontbreken er bepaalde resultaten?

Als u denkt dat u niet de verwachte resultaten ziet dat, moet u er een aantal mogelijke redenen zijn.

* **Filter tijdsbereik**. Standaard worden alleen resultaten weergegeven in de afgelopen 24 uur. Er is een automatische filter dat u het bereik van de resultaten die worden opgehaald uit de brontabellen beperkt. 

    U kunt echter het tijdsbereik wijzigen filter met behulp van de vervolgkeuzelijst.

    Of u kunt de automatische bereik overschrijven door te nemen van uw eigen [ `where  ... timestamp ...` component](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) in uw query. Bijvoorbeeld:

    `requests | where timestamp > ago('2d')`

* **Resultaten limiet**. Er is een limiet van ongeveer 10 k rijen op de resultaten van de portal. Een waarschuwing laat zien als u de limiet overschreden gaat. Als dit gebeurt, sorteer de resultaten in de tabel wordt niet altijd u alle de werkelijke eerste of laatste resultaten weergegeven. 

    Het is verstandig om te voorkomen dat de limiet hebt bereikt. Gebruik het filter tijdsbereik of gebruik operators zoals:

  * [Top 100 op timestamp](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator) 
  * [100 nemen](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/take-operator)
  * [samenvatten ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) 
  * [waar tijdstempel > ago(3d)](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator)

(Meer dan 10 k rijen wilt? Overweeg het gebruik van [continue Export](app-insights-export-telemetry.md) in plaats daarvan. Analytics is ontworpen voor analyse, in plaats van tijdens het ophalen van onbewerkte gegevens.)

## <a name="diagrams"></a>Diagrammen
Selecteer het type diagram dat u wilt:

![Selecteer een diagramtype](./media/app-insights-analytics-using/230.png)

Als u meerdere kolommen van de juiste typen hebt, kunt u de x en y-as en een kolom met dimensies scheiden van de resultaten op.

Standaard resultaten worden in eerste instantie weergegeven als een tabel, en u het diagram handmatig selecteren. Maar u kunt de [renderen richtlijn](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) aan het einde van een query voor het selecteren van een diagram.

### <a name="analytics-diagnostics"></a>Diagnostische gegevens over Analytics


Op een tijdgrafiek als er een plotselinge piek of een stap in uw gegevens, ziet u mogelijk een gemarkeerde punt op de regel. Hiermee wordt aangegeven dat Analytics diagnostische gegevens over een combinatie van eigenschappen die de onverwachte wijziging filteren is geïdentificeerd. Klik op het punt voor meer informatie over het filter en de gefilterde versie te zien. Hiermee kunt u bepalen wat de oorzaak van de wijziging. 

[Meer informatie over diagnostische gegevens over Analytics](app-insights-analytics-diagnostics.md)


![Diagnostische gegevens over Analytics](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-to-dashboard"></a>Vastmaken aan dashboard
U kunt vastmaken een diagram of de tabel op een van uw [gedeelde dashboards](app-insights-dashboards.md) -Klik op de pincode. 

![Klik op de pincode](./media/app-insights-analytics-using/pin-01.png)

Dit betekent dat wanneer u een dashboard om u te helpen bij het controleren van de prestaties of het gebruik van uw web-services samengesteld, u erg complexe analyses naast de andere metrische gegevens opnemen kunt. 

Als er vier of minder kolommen, kunt u een tabel aan het dashboard vastmaken. Alleen de bovenste zeven rijen worden weergegeven.

### <a name="dashboard-refresh"></a>Dashboard vernieuwen
De grafiek is vastgemaakt aan het dashboard wordt automatisch vernieuwd door de query opnieuw ongeveer elk uur wordt uitgevoerd. U kunt ook klikken op de knop vernieuwen.

### <a name="automatic-simplifications"></a>Automatische vereenvoudigen

Bepaalde vereenvoudigen worden toegepast op een grafiek, wanneer u deze aan een dashboard vastmaken.

**Tijd van beperking:** query's automatisch beperkt tot de afgelopen 30 dagen zijn. Het effect is hetzelfde als wanneer de query bevat `where timestamp > ago(30d)`.

**Aantal beperking van de opslaglocatie:** als u een grafiek die een groot aantal afzonderlijke opslaglocaties (meestal een staafdiagram heeft) wordt weergegeven, de minder ingevuld opslaglocaties automatisch worden gegroepeerd in een enkele "anderen" opslaglocatie. Bijvoorbeeld: deze query:

    requests | summarize count_search = count() by client_CountryOrRegion

ziet er als volgt in Analytics:

![Grafiek met lange staart](./media/app-insights-analytics-using/pin-07.png)

maar wanneer u deze aan een dashboard vastmaken, deze ziet er als volgt:

![Grafiek met beperkte opslaglocaties](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-to-excel"></a>Exporteren naar Excel
Nadat u een query uitvoert hebt, kunt u een CSV-bestand downloaden. Klik op **exporteren, Excel**.

## <a name="export-to-power-bi"></a>Exporteren naar Power BI
Plaats de cursor in een query en kies **exporteren, Power BI**.

![Exporteren van Analytics naar Power BI](./media/app-insights-analytics-using/240.png)

U kunt de query uitvoeren in Power BI. U kunt dit worden vernieuwd volgens een schema instellen.

Met Power BI, kunt u dashboards die Combineer gegevens van een groot aantal bronnen.

[Meer informatie over het exporteren naar Power BI](app-insights-export-power-bi.md)

## <a name="deep-link"></a>Dieptekoppeling

Haal een koppeling onder **Export, koppeling delen** die u kunt verzenden naar een andere gebruiker. Mits de gebruiker heeft [toegang tot de resourcegroep](app-insights-resources-roles-access-control.md), de query wordt geopend in de gebruikersinterface van Analytics.

(In de koppeling tekst van de query wordt weergegeven na '? q = ' gzip gecomprimeerde en base-64 gecodeerd. U kunt de code voor het genereren van dieptekoppelingen die u aan gebruikers geven schrijven. De aanbevolen manier om analyses uitvoeren van code is echter met behulp van de [REST-API](https://dev.applicationinsights.io/).)


## <a name="automation"></a>Automation

Gebruik de [Data Access-REST API](https://dev.applicationinsights.io/) Analytics-query's uitvoeren. [Bijvoorbeeld](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (met behulp van PowerShell):

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

In tegenstelling tot de gebruikersinterface van Analytics, wordt de REST-API niet automatisch toegevoegd de beperking van een tijdstempel aan uw query's. Vergeet niet uw eigen where-component, om te voorkomen dat grote antwoorden toevoegen.



## <a name="import-data"></a>Gegevens importeren

U kunt gegevens importeren uit een CSV-bestand. Er is een typische gebruiksscenario om statische gegevens die u kunt tabellen bij aan uw telemetrie te importeren. 

Als de geverifieerde gebruikers worden geïdentificeerd in uw telemetrie door een alias of een verborgen-id, kunt u bijvoorbeeld een tabel die aliassen wordt toegewezen aan de namen van echte importeren. Door het uitvoeren van een join op de aanvraagtelemetrie, kunt u gebruikers identificeren met hun echte naam in de Analytics-rapporten.

### <a name="define-your-data-schema"></a>Het gegevensschema van uw definiëren

1. Klik op **instellingen** (bovenaan links) en vervolgens **gegevensbronnen**. 
2. Toevoegen van een gegevensbron, de instructies te volgen. U wordt gevraagd om op te geven een voorbeeld van de gegevens die moet ten minste 10 rijen bevatten. U kunt het schema corrigeren.

Hiermee definieert u een gegevensbron die u vervolgens gebruiken kunt om afzonderlijke tabellen te importeren.

### <a name="import-a-table"></a>Een tabel importeren

1. Open de definitie van de gegevensbron in de lijst.
2. Klik op 'Uploaden' en volg de instructies voor het uploaden van de tabel. Dit omvat een aanroep van een REST-API en het is dus eenvoudig automatiseren. 

De tabel is nu beschikbaar voor gebruik in Analytics-query's. Wordt deze weergegeven in Analytics 

### <a name="use-the-table"></a>Gebruik de tabel

Stel de definitie van de gegevensbron wordt aangeroepen `usermap`, en dat er twee velden, `realName` en `user_AuthenticatedId`. De `requests` tabel heeft ook een veld met de naam `user_AuthenticatedId`, zodat u gemakkelijk om lid te worden:

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
De resulterende tabel van aanvragen heeft een extra kolom `realName`.

### <a name="import-from-logstash"></a>Importeren vanuit LogStash

Als u [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), kunt u Analytics om op te vragen de logboeken. Gebruik de [pipes data Analytics-invoegtoepassing](https://github.com/Microsoft/logstash-output-application-insights). 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

