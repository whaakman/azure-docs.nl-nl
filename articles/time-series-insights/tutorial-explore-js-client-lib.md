---
title: 'Zelfstudie: De JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken | Microsoft Docs'
description: Informatie over de JavaScript-clientbibliotheek en het bijbehorende programmeermodel van Azure Time Series Insights.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 07/29/2019
ms.author: dpalled
ms.custom: seodec18
ms.openlocfilehash: 1ae37636a2caf108221be2240a9517a547945096
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638844"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Zelfstudie: De JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken

De Azure Time Series Insights-client bibliotheek van Java script D3 is ontwikkeld om webontwikkelaars in te helpen bij het opvragen en visualiseren van gegevens die zijn opgeslagen in Time Series Insights. In deze zelf studie wordt u begeleid bij de Time Series Insights-client bibliotheek en het programmeer model met behulp van een gehoste voor beeld-app.

In de zelf studie wordt beschreven hoe u met de bibliotheek werkt, hoe u toegang krijgt tot Time Series Insights gegevens en hoe u grafiek besturings elementen kunt gebruiken om gegevens weer te geven en te visualiseren. U leert ook hoe u kunt experimenteren met verschillende soorten grafieken om gegevens te visualiseren. Aan het einde van de zelf studie kunt u de client bibliotheek gebruiken om Time Series Insights-functies in uw eigen web-app op te nemen.

Meer informatie over:

> [!div class="checklist"]
> * De voorbeeld toepassing Time Series Insights
> * De Time Series Insights java script-client bibliotheek
> * Hoe de voorbeeld toepassing de bibliotheek gebruikt om Time Series Insights gegevens te visualiseren

> [!NOTE]
> * De zelf studie maakt gebruik van een gratis, gehoste [Time Series Insights Webdemo](https://insights.timeseries.azure.com/clientsample).
> * De bron bestanden voor de Time Series Insights-voor beeld-app zijn opgenomen in de [github-voorbeeld opslagplaats](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Lees de [documentatie over de time series Insights-client](https://github.com/microsoft/tsiclient/blob/master/docs/API.md).

## <a name="prerequisites"></a>Vereisten

* Meld u aan voor een [gratis Azure-abonnement](https://azure.microsoft.com/free/) als u er nog geen hebt.

* In deze zelf studie wordt de functie **Ontwikkelhulpprogramma's** van uw browser gebruikt. Moderne webbrowsers ([micro soft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [Firefox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/)en anderen) bieden doorgaans toegang tot de **weer gave weboverzicht** via de F12-sneltoets op het toetsen bord. Een andere manier om de weer gave te openen, is door met de rechter muisknop op een webpagina te klikken en vervolgens **element controleren**te selecteren.

## <a name="video"></a>Video

In deze video introduceren we de open source Time Series Insights java script SDK:
<br /><br />

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]



## <a name="time-series-insights-sample-application"></a>Time Series Insights-voorbeeldtoepassing

In deze zelf studie gebruiken we een gratis, gehoste Time Series Insights voor beeld-app om de bron code achter de toepassing te verkennen en de Time Series Insights java script-client bibliotheek te verkennen. Als u de voor beeld-app gebruikt, leert u hoe u kunt communiceren met Time Series Insights in Java script en gegevens kunt visualiseren via grafieken en grafieken.

1. Ga naar de [voorbeeld toepassing time series Insights](https://insights.timeseries.azure.com/clientsample). De volgende aanmeldings prompt wordt weer gegeven:

   [![Aanmeldings prompt voor client voorbeeld Time Series Insights](media/tutorial-explore-js-client-lib/tcs-sign-in.png)](media/tutorial-explore-js-client-lib/tcs-sign-in.png#lightbox)

1. Selecteer **Aanmelden** om uw referenties in te voeren of te selecteren. Gebruik een organisatie account (Azure Active Directory) voor ondernemingen of een persoonlijk account (Microsoft-account).

   [![Vragen om Time Series Insights client voorbeeld referenties](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png#lightbox)

1. Nadat u zich hebt aangemeld, wordt een pagina weer gegeven met grafieken die zijn gevuld met Time Series Insights gegevens. Uw gebruikersaccount en de optie **Afmelden** worden in de rechterbovenhoek weergegeven:

   [![Time Series Insights client voor beeld van hoofd pagina na aanmelden](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png#lightbox)

### <a name="page-source-and-structure"></a>Paginabron en -structuur

Eerst gaan we de [HTML-en Java script-bron code](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) van de gerenderde webpagina bekijken:

1. Open **Ontwikkelhulpprogramma's** in uw browser. Bekijk de HTML-elementen waaruit de huidige pagina bestaat, ook wel de HTML- of DOM-structuur genoemd.

1. Vouw de `<head>` elementen `<body>` en uit en houd rekening met de volgende secties:

   * Onder het `<head>` -element vindt u meta gegevens en afhankelijkheden van de pagina waarmee de app kan worden uitgevoerd:
     * Een `<script>` element dat wordt gebruikt om te verwijzen naar het ADAL-bestand (Azure Active Directory Authentication Library) *ADAL. min. js*. ADAL is een JavaScript-bibliotheek die zorgt voor OAuth 2.0-verificatie (aanmelding) en het verkrijgen van een token voor toegang tot API's.
     * Meerdere `<link>` elementen voor Style Sheets (ook wel *CSS*genoemd), zoals *sampleStyles. CSS* en *tsiclient. CSS*. Het opmaak model bepaalt de opmaak Details van de visuele pagina, zoals kleuren, letter typen en afstand.
     * Een `<script>` element dat wordt gebruikt om te verwijzen naar de time series Insights java script-client bibliotheek *tsiclient. js*. De pagina gebruikt de-bibliotheek voor het aanroepen van Time Series Insights-service-Api's en voor het weer geven van grafiek besturings elementen op de pagina.

     >[!NOTE]
     > * De bron code voor de Java script-bibliotheek ADAL is beschikbaar in de [opslag plaats Azure-ActiveDirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js).
     > * De bron code voor de Time Series Insights java script-client bibliotheek is beschikbaar in de [tsiclient-opslag plaats](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).

   * Onder het `<body>` -element vindt `<div>` u elementen, waarmee u de indeling van items op de pagina en een ander `<script>` element kunt definiëren:
     * Met het `<div>` eerste element wordt het dialoog venster **Aanmelden** (`id="loginModal"`) opgegeven.
     * Het tweede `<div>`-element fungeert als een bovenliggend item voor:
       * Een koptekst-`<div>`-element, dat wordt gebruikt voor statusberichten en aanmeldingsgegevens aan de bovenkant van de pagina (`class="header"`).
       * Een `<div>` element voor de rest van de hoofd elementen van de pagina, met inbegrip van de grafieken (`class="chartsWrapper"`).
       * Een `<script>` sectie met het Java script dat wordt gebruikt voor het beheren van de pagina.

   [![Time Series Insights client-voor beeld met Ontwikkelhulpprogramma's](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

1. Vouw het `<div class="chartsWrapper">` element uit en u vindt meer onderliggende `<div>` elementen. Deze elementen worden gebruikt voor het plaatsen van alle voorbeelden van grafiekbesturingselementen. Er zijn verschillende paren `<div>` elementen, één voor elk grafiek voorbeeld:

   * Het eerste element (`class="rowOfCardsTitle"`) bevat een beschrijvende titel om samen te vatten wat de grafiek(en) laten zien. Bijvoorbeeld: `Static Line Charts With Full-Size Legends.`
   * Het tweede element`class="rowOfCards"`() is een bovenliggend item dat aanvullende onderliggende `<div>` elementen bevat die de werkelijke grafiek besturings elementen binnen een rij positioneren.

   [![Div-elementen van Body](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png#lightbox)

1. Vouw het `<script type="text/javascript">` element uit dat direct onder het `<div class="chartsWrapper">` -element ligt. Het begin van de Java script-sectie op pagina niveau wordt gebruikt voor het verwerken van alle pagina logica (verificatie, het aanroepen van Time Series Insights service-Api's, het samen stellen van de grafiek besturings elementen en meer):

   [![Hoofd script](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png#lightbox)

## <a name="time-series-insights-javascript-client-library-concepts"></a>Concepten van Time Series Insights java script-client bibliotheek

De Time Series Insights-client bibliotheek (*tsclient. js*) biedt abstracties voor twee belang rijke java script-functies:

* **Wrapper-methoden voor het aanroepen van de time series Insights query-api's**: REST-Api's die u kunt gebruiken om Time Series Insights gegevens op te vragen met behulp van statistische expressies. De methoden zijn ingedeeld onder de TsiClient. server-naam ruimte van de bibliotheek.

* **Methoden voor het maken en vullen van verschillende typen grafiekbesturingselementen**: Methoden die u kunt gebruiken om de Time Series Insights statistische gegevens op een webpagina weer te geven. De methoden zijn ingedeeld onder de naam ruimte TsiClient. UX van de-bibliotheek.

Dankzij deze vereenvoudiging kunnen ontwikkel aars eenvoudiger gebruikers interface grafieken en-grafiek onderdelen bouwen die worden ingeschakeld met Time Series Insights gegevens.

### <a name="authentication"></a>Authentication

De [voorbeeld toepassing time series Insights](https://insights.timeseries.azure.com/clientsample) is een app met één pagina die ADAL OAuth 2,0-gebruikers verificatie ondersteunt:

1. Wanneer u ADAL gebruikt voor verificatie, moet de client-app zijn geregistreerd in Azure Active Directory (Azure AD). De app met één pagina is in feite geregistreerd voor gebruik van de [OAuth 2,0 impliciete toekennings stroom](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-implicit-grant-flow).
1. De toepassing moet sommige registratie-eigenschappen tijdens runtime opgeven. De eigenschappen omvatten de client-GUID`clientId`() en de omleidings-URI (`postLogoutRedirectUri`).
1. Later vraagt de app een *toegangs token* aan bij Azure AD. Het toegangs token wordt uitgegeven voor een eindige set machtigingen voor een specifieke service of API-id (https\/:/API.timeseries.Azure.com). De tokenmachtigingen worden namens de aangemelde gebruiker uitgegeven. De id voor de service of API is een andere eigenschap die deel uitmaakt van de Azure AD-registratie van de app.
1. Nadat ADAL het toegangs token naar de app heeft geretourneerd, wordt het door gegeven als Bearer- *token* wanneer het toegang heeft tot de time series Insights service-api's.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=3-7,34-37)]

> [!TIP]
> Voor meer informatie over Azure AD-verificatie bibliotheken die door micro soft worden ondersteund, raadpleegt u de documentatie over de [Azure Active Directory-verificatie bibliotheek](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries#microsoft-supported-client-libraries).

### <a name="control-identification"></a>Identificatie van besturingselementen

In het gegeven voor beeld `<div>` worden elementen gerangschikt in het bovenliggende `<body>` element om een overzichtelijke indeling te bieden voor de grafiek besturings elementen die op de pagina worden weer gegeven.

Elk `<div>` element bevat eigenschappen voor de plaatsing en visuele kenmerken van grafiek besturings elementen. Eigenschappen van `id` HTML-elementen fungeren als unieke id's om te verbinden met specifieke besturings elementen om gevisualiseerde gegevens weer te geven en bij te werken.

### <a name="aggregate-expressions"></a>Samenvoegingsexpressies

De Time Series Insights-client bibliotheek-Api's gebruiken geaggregeerde expressies:

* Een statistische expressie biedt de mogelijkheid om een of meer *zoek termen*samen te stellen.

* De client-api's zijn ontworpen om Vergelijk bare functionaliteit te bieden aan een andere demo-app (de [Time Series Insights Explorer](https://insights.timeseries.azure.com/demo)), `where` die gebruikmaakt van een zoek reeks `splitBy` , predikaten, metingen en waarden.

* De meeste Api's van de client bibliotheek maken gebruik van een matrix met statistische expressies die de service gebruikt om een Time Series Insights gegevens query samen te stellen.

### <a name="call-pattern"></a>Aanroeppatroon

Het vullen en rendering van grafiek besturings elementen volgt een algemeen patroon. U kunt het algemene patroon in de voor beeld-app bekijken en u kan u helpen bij het gebruik van de client bibliotheek:

1. Declareer `array` een om een of meer time series Insights statistische expressies te bevatten:

   ```javascript
   var aes =  [];
   ```

1. Build *1* tot *n* samengevoegde expressie-objecten. Voeg ze vervolgens toe aan de matrix voor samengevoegde expressies:

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **aggregateExpression-parameters**

   | Parameter | Description | Voorbeeld |
   | --------- | ----------- | ------- |
   | `predicateObject` | De expressie voor gegevens filtering |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | De naam van de eigenschap van de meting die wordt gebruikt | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | De aggregaties van de eigenschap maat eenheid die u wilt | `['avg', 'min']` |
   | `searchSpan`      | De duur en interval grootte van de statistische expressie | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | De eigenschap van de teken reeks die u wilt splitsen (optioneel: kan null zijn) | `{property: 'Station', type: 'String'}` |
   | `color`         | De kleur van de objecten die u wilt weer geven | `'pink'` |
   | `alias`           | Een beschrijvende naam voor de statistische expressie | `'Factory3Temperature'` |
   | `contextMenuActions` | Een matrix met acties die moeten worden gebonden aan de tijd reeks objecten in een visualisatie (optioneel) | Zie [pop-upcontext menu's](#pop-up-context-menus)voor meer informatie. |

1. Roep een Time Series Insights query aan met behulp van de TsiClient. server-Api's om de cumulatieve gegevens aan te vragen:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates parameters**

   | Parameter | Description | Voorbeeld |
   | --------- | ----------- | ------- |
   | `token`     | Het toegangs token voor de Time Series Insights-API |  `authContext.getTsiToken()`<br />Zie [Verificatie](#authentication) voor meer informatie. |
   | `envFQDN`   | De Fully Qualified Domain Name (FQDN) voor de Time Series Insights omgeving | Van de Azure Portal. Bijvoorbeeld: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | Een matrix met Time Series Insights query-expressies | Gebruik de `aes` variabele zoals eerder beschreven: `aes.map(function(ae){return ae.toTsx()}`. |

1. Het gecomprimeerde resultaat dat wordt geretourneerd van de Time Series Insights query transformeren naar JSON voor visualisatie:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

1. Een grafiek besturings element maken met behulp van de TsiClient. UX-Api's. Koppel deze aan een van de `<div>` elementen op de pagina:

   ```javascript
   var barChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

1. Vul het besturings element grafiek met de getransformeerde JSON-gegevens objecten in en Genereer het besturings element op de pagina:

   ```javascript
   barChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="render-controls"></a>Besturings elementen weer geven

De Time Series Insights-client bibliotheek bevat acht unieke, out-of-the-box Analytics-besturings elementen:

* **lijn diagram**
* **cirkel diagram**
* **staaf diagram**
* **heatmap**
* **Besturings elementen voor hiërarchie**
* **toegankelijk raster**
* **afzonderlijke gebeurtenis tijdlijnen**
* **status overgang tijd lijnen**

### <a name="line-chart-bar-chart-and-pie-chart-examples"></a>Voor beelden van lijn diagram, staaf diagram en cirkel diagram

Bekijk de demo code die wordt gebruikt voor het weer geven van enkele van de standaard besturings elementen voor grafieken. Let op het programmeer model en de patronen voor het maken van deze besturings elementen. Bekijk in het bijzonder de HTML onder `// Example 3/4/5` de opmerking, waarmee besturings elementen worden weer gegeven `id` met de `chart4`HTML- `chart5`waarden `chart3`, en.

Terughalen uit stap 3 van de [sectie pagina bron en-structuur](#page-source-and-structure) , waarmee grafiek besturings elementen in rijen op de pagina worden gerangschikt. Elk grafiek besturings element heeft een beschrijvende titel rij. In dit voor beeld worden de drie grafieken gevuld onder het `Multiple Chart Types From the Same Data` element title `<div>` en zijn ze gebonden aan de drie `<div>` elementen onder de titel:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

De volgende sectie van Java script-code maakt gebruik van patronen die eerder zijn beschreven: bouw Time Series Insights samengestelde expressies, gebruik ze om te zoeken naar Time Series Insights gegevens en vervolgens de drie grafieken weer te geven. Er worden drie grafiek typen gebruikt uit de tsiClient. UX- `LineChart`naam `BarChart`ruimte: `PieChart`, en. De grafiek typen worden gebruikt om de respectieve grafieken te maken en weer te geven. Alle drie de grafieken kunnen dezelfde statistische-expressie gegevens `transformedResult`gebruiken:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

De drie grafieken worden als volgt weergegeven:

[![Multiple Chart Types From The Same Data](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Geavanceerde functies

De Time Series Insights-client bibliotheek heeft verschillende aanvullende functies die u kunt gebruiken om gegevens visualisaties op een creatieve wijze te implementeren.

### <a name="states-and-events"></a>Statussen en gebeurtenissen

Een geavanceerde functionaliteit is de mogelijkheid om status overgangen en discrete gebeurtenissen toe te voegen aan grafieken. Deze functie is handig voor het markeren van incidenten, waarschuwingen en het maken van status switches (bijvoorbeeld aan/uit-switches).

Bekijk de code rond de `// Example 10` opmerking. De code geeft een regel besturings element weer onder de `Line Charts with Multiple Series Types` titel en verbindt dit `<div>` met het-element met de `id` HTML `chart10`-waarde.

In de volgende stappen wordt het proces beschreven:

1. Een structuur met `events4` de naam is gedefinieerd voor de status-wijzigings elementen die moeten worden gevolgd. De structuur bevat:

   * Een tekenreekssleutel met de naam `Component States`.
   * Een matrix van waardeobjecten die de statussen voorstellen. Elk object omvat:
     * Een tekenreekssleutel die een ISO-tijdstempel in JavaScript bevat.
     * Een matrix die de kenmerken van de status bevat: een kleur en een beschrijving.

1. De `events5` structuur is gedefinieerd voor `Incidents`, die een matrix bevat met de gebeurtenis elementen die moeten worden gevolgd. De matrixstructuur heeft dezelfde vorm als de structuur die is beschreven voor `events4`.

1. Het lijn diagram wordt gerenderd en wordt door gegeven in de twee structuren met de para `events:` meters van de grafiek opties: en. `states:` Let op de andere optie parameters voor het `tooltip:`opgeven `theme:`van een `grid:`, of.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

De ruit vormige markeringen/pop-upvensters die worden gebruikt om incidenten op te geven en de gekleurde balken/pop-upvensters op de tijd schaal geven de status wijzigingen aan:

[![Lijn diagrammen met meerdere reeks typen](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Pop-upcontextmenu's

Een andere geavanceerde functionaliteit is de mogelijkheid om aangepaste context menu's te maken (Klik met de rechter muisknop op pop-upmenu's). Aangepaste contextmenu's zijn handig voor het inschakelen van acties en logische vervolgstappen binnen het bereik van uw toepassing.

Bekijk de code rondom de `// Example 13/14/15` opmerking. Deze code geeft in eerste instantie een lijn diagram weer onder `Line Chart with Context Menu to Create Pie/Bar Chart`de titel. De grafiek is gebonden aan het `<div>` element met de HTML `id` - `chart13`waarde.

Met behulp van contextmenu's biedt het lijndiagram de mogelijkheid om dynamisch een cirkel- en staafdiagram te maken, die worden gekoppeld aan `<div>`-elementen met de id's `chart14` en `chart15`. Zowel in het cirkel diagram als in het staaf diagram worden ook context menu's gebruikt om hun eigen functies in te scha kelen: de mogelijkheid om gegevens van het cirkel diagram naar het staaf diagram te kopiëren en de staaf diagram gegevens af te drukken naar het browser console venster.

In de volgende stappen wordt het proces beschreven:

1. Er wordt een reeks aangepaste acties gedefinieerd. Elke actie bevat een matrix met een of meer elementen. Elk element definieert één contextmenuopdracht:

   * `barChartActions`: Deze actie definieert het contextmenu voor het cirkeldiagram, met één element voor het definiëren van één menuopdracht:
     * `name`: De tekst die wordt gebruikt voor de menuopdracht: Parameters afdrukken naar console.
     * `action`: De actie die aan de menuopdracht is gekoppeld. De actie is altijd een anonieme functie die drie argumenten accepteert. Deze zijn gebaseerd op de samenvoegingsexpressie die is gebruikt voor het maken van de grafiek. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
       * `ae`: De matrix met de samenvoegingsexpressie.
       * `splitBy`: De `splitBy` waarde.
       * `timestamp`: Het tijdstempel.

   * `pieChartActions`: Deze actie definieert het contextmenu voor het staafdiagram, dat één element bevat voor het definiëren van één menuopdracht. De vorm en het schema zijn hetzelfde als die `barChartActions` van het element dat eerder is `action` beschreven, maar de functie is aanzienlijk anders: er wordt een instantie gemaakt en weer gegeven in het staaf diagram. Het `ae` argument wordt gebruikt om de samengevoegde expressie matrix op te geven die tijdens runtime wordt door gegeven wanneer de menu opdracht wordt geopend. Met de functie wordt ook de eigenschap `ae.contextMenu` ingesteld met het contextmenu `barChartActions`.
   * `contextMenuActions`: Deze actie definieert het contextmenu voor het lijndiagram, dat drie elementen bevat voor het definiëren van drie menuopdrachten. De vorm en het schema voor elk element zijn hetzelfde als de elementen die eerder zijn beschreven. Net als bij het `barChartActions`-element, worden met de eerste opdracht drie functieargumenten weggeschreven naar het consolevenster van de browser. Net als bij `pieChartActions` het element worden de tweede twee items geïnstantieerd en wordt respectievelijk het cirkel diagram en staaf diagram weer gegeven. Met de volgende twee items worden ook hun eigenschappen `ae.contextMenu` ingesteld met respectievelijk de contextmenu's `pieChartActions` en `barChartActions`.

1. Twee geaggregeerde expressies worden gepusht `aes` naar de geaggregeerde Expression-matrix. Hiermee wordt de `contextMenuActions` matrix voor elk item opgegeven. Deze expressies worden gebruikt met het besturingselement voor het lijndiagram.

1. Alleen het lijn diagram wordt in eerste instantie weer gegeven, waarbij zowel het cirkel diagram als het staaf diagram tijdens runtime kunnen worden weer gegeven.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

De volgende scherm afbeelding toont de grafieken met hun respectievelijke pop-upcontext menu's. Het cirkel diagram en het staaf diagram zijn dynamisch gemaakt met behulp van de opties in het context menu van het lijn diagram.

[![Lijn diagram met context menu voor het maken van een cirkel diagram en staaf diagram](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Kwasten

U kunt penselen gebruiken om een tijds bereik te bereiken voor het definiëren van acties zoals zoomen en verkennen.

De code die wordt gebruikt voor het illustreren van penselen wordt weer `Line Chart with Context Menu to Create Pie/Bar Chart` gegeven in het voor beeld waarin pop-upcontext menu's worden beschreven.

* Kwastacties lijken erg op een contextmenu, in zoverre dat hiermee een reeks aangepaste acties voor de kwast kan worden gedefinieerd. Elke actie bevat een matrix met een of meer elementen. Elk element definieert één contextmenuopdracht:
   * `name`: De tekst die wordt gebruikt voor de menuopdracht: Parameters afdrukken naar console.
   * `action`: De actie die is gekoppeld aan de menuopdracht, die altijd een anonieme functie is die twee argumenten accepteert. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
     * `fromTime`: De `from` tijds tempel van de kwast selectie.
     * `toTime`: De `to` tijds tempel van de kwast selectie.

* Kwastacties worden toegevoegd als een eigenschap voor grafiekopties. De `brushContextMenuActions: brushActions` eigenschap wordt door gegeven aan `linechart.Render` de aanroep.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

[![Lijn diagram met context menu om een cirkel diagram en een staaf diagram te maken met behulp van penselen](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png#lightbox)

## <a name="clean-up-resources"></a>Resources opschonen

Nu u de zelf studie hebt voltooid, kunt u de resources die u hebt gemaakt opschonen:

1. Selecteer in het menu links in het [Azure Portal](https://portal.azure.com) **alle resources**, zoek de Azure time series Insights resource groep.
1. Verwijder de hele resource groep (en alle resources erin) door elke resource afzonderlijk **verwijderen** of verwijderen te selecteren.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Meld u aan en verken de Time Series Insights voorbeeld toepassing en de bijbehorende bron
> * Api's gebruiken in de Time Series Insights java script-client bibliotheek
> * Java script gebruiken om grafiek besturings elementen te maken en vullen met Time Series Insights gegevens

De voorbeeld toepassing Time Series Insights maakt gebruik van een demo-gegevensset. Lees het volgende artikel voor meer informatie over het maken van uw eigen Time Series Insights omgeving en gegevensset:

> [!div class="nextstepaction"]
> [Zelfstudie: Een Azure Time Series Insights-omgeving maken](tutorial-create-populate-tsi-environment.md)

U kunt ook de bron bestanden van de Time Series Insights-voorbeeld toepassing bekijken:

> [!div class="nextstepaction"]
> [Opslag plaats van Time Series Insights voor beeld-app](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)

Lees de documentatie voor de Time Series Insights-client-API-referentie:

> [!div class="nextstepaction"]
> [Referentie documentatie voor Time Series Insights-API](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)
