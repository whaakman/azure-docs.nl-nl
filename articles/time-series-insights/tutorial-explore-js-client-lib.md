---
title: De JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken
description: Informatie over de JavaScript-clientbibliotheek en het bijbehorende programmeermodel van Azure Time Series Insights.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 06/05/2018
ms.author: anshan
ms.openlocfilehash: 445584ef8f9a2534d7f183a4452df19ee87d0025
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626991"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Zelfstudie: JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken

Om webontwikkelaars te helpen bij het opvragen en visualiseren van gegevens die zijn opgeslagen in Time Series Insights (TSI), is de TSI-clientbibliotheek op basis van JavaScript D3 ontwikkeld.  Aan de hand van een voorbeeld van een webtoepassing maakt u in deze zelfstudie kennis met de clientbibliotheek van TSI en het bijbehorende programmeermodel.

De onderwerpen in deze zelfstudie bieden u de mogelijkheid om te experimenteren met de bibliotheek, inzicht te krijgen in hoe u TSI-gegevens benadert en grafiekbesturingselementen te gebruiken om gegevens weer te geven en te visualiseren. Het doel is om u voldoende informatie te bieden, zodat u de bibliotheek in uw eigen webtoepassing kunt gebruiken.

In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> * De TSI-voorbeeldtoepassing.
> * De JavaScript-clientbibliotheek van TSI.
> * Hoe de voorbeeldtoepassing de bibliotheek gebruikt om TSI gegevens te visualiseren.

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt de functie Ontwikkelhulpprogramma's gebruikt (ook wel bekend als DevTools of F12), een functie die beschikbaar is in de meeste moderne webbrowsers, zoals [Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [FireFox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/) en andere. Als u nog niet bekend bent met deze functie, wilt u misschien eerst wat met deze functie experimenteren in uw browser voordat u verdergaat met deze zelfstudie.

## <a name="time-series-insights-sample-application"></a>Time Series Insights-voorbeeldtoepassing

In deze zelfstudie wordt de Time Series Insights-voorbeeldtoepassing gebruikt om meer te weten te komen over de broncode achter de toepassing, inclusief informatie over het gebruik van de JavaScript-clientbibliotheek van TSI. Het voorbeeld is een Single-Page Web Application (SPA) die laat zien hoe u de bibliotheek gebruikt. Het voorbeeld laat zien hoe u gegevens uit een voorbeeld-TSI-omgeving opvraagt en visualiseert.

1. Blader naar de [Time Series Insights-voorbeeldtoepassing](https://insights.timeseries.azure.com/clientsample). U ziet een pagina zoals in de volgende afbeelding, met een prompt om u aan te melden:

   ![Aanmeldingsprompt in TSI Sample App](media/tutorial-explore-js-client-lib/tcs-sign-in.png)

2. Selecteer **Log in** of typ of selecteer uw referenties. Gebruik een account van uw onderneming of organisatie (Azure Active Directory) of een persoonlijk account (Microsoft-account of MSA).

   ![Prompt voor invoeren van referenties in TSI Sample App](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)

3. Als de aanmelding is geslaagd, ziet u een pagina zoals in de volgende afbeelding. De pagina toont verschillende stijlen van voorbeelddiagrammen die zijn gevuld met TSI-gegevens. Uw gebruikersaccount en de optie **Afmelden** worden in de rechterbovenhoek weergegeven:

   ![Hoofdpagina van de TSI Sample App na aanmelding](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)

### <a name="page-source-and-structure"></a>Paginabron en -structuur

Laten we eerst eens kijken naar de HTML- en JavaScript-broncode achter de pagina die wordt weergegeven in uw browser. We gaan niet alle elementen bespreken, maar alleen de hoofdonderdelen, zodat u een goed beeld krijgt van de werking van de pagina:

1. Open **Ontwikkelhulpprogramma's** in uw browser. Bekijk de HTML-elementen waaruit de huidige pagina bestaat, ook wel de HTML- of DOM-structuur genoemd.

2. Vouw de elementen `<head>` en `<body>` uit en let op de volgende secties:

   - Onder het element `<head>` ziet u elementen waarmee aanvullende bestanden worden opgehaald om te pagina op de gewenste manier te laten werken:
     - Een `<script>`-element dat wordt gebruikt om te verwijzen naar de Azure Active Directory Authentication Library **adal.min.js** (ook wel ADAL genoemd). ADAL is een JavaScript-bibliotheek die zorgt voor OAuth 2.0-verificatie (aanmelding) en het verkrijgen van een token voor toegang tot API's.
     - Meerdere `<link>`-elementen voor opmaakmodellen (ook wel CSS genoemd) zoals **sampleStyles.css** en **tsiclient.css**. De opmaakmodellen worden gebruikt voor het instellen van visuele paginaopmaakdetails, zoals de kleuren, lettertypen en afstand.
     - Een `<script>`-element dat wordt gebruikt om te verwijzen naar de JavaScript-clientbibliotheek van TSI, **tsiclient.js**. De bibliotheek wordt door de pagina gebruikt om API's van de TSI-service aan te roepen en grafiekbesturingselementen op de pagina weer te geven.

     >[!NOTE]
     > De broncode voor de ADAL JavaScript-bibliotheek is beschikbaar in de [opslagplaats azure-activedirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js).
     > De JavaScript-broncode voor de TSI-clientbibliotheek is beschikbaar in de [opslagplaats tsiclient](https://github.com/Microsoft/tsiclient).

   - Onder het `<body>`-element vindt u `<div>`-elementen, die als containers voor het definiëren van de indeling van items op de pagina fungeren, plus nog een `<script>`-element:
     - Het eerste `<div>`-element verwijst naar het dialoogvenster **Log in** (`id="loginModal"`).
     - Het tweede `<div>`-element fungeert als een bovenliggend item voor:
       - Een koptekst-`<div>`-element, dat wordt gebruikt voor statusberichten en aanmeldingsgegevens aan de bovenkant van de pagina (`class="header"`).
       - Een `<div>`-element voor de overige hoofdtekstelementen (body) van de pagina, inclusief alle grafieken (`class="chartsWrapper"`).
       - Een `<script>`-sectie, met daarin alle JavaScript-code die wordt gebruikt voor het beheren van de pagina.

   [![TSI Sample App met Ontwikkelhulpprogramma's](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

3. Als u het `<div class="chartsWrapper">`-element uitvouwt, vindt u meer onderliggende `<div>`-elementen. Deze elementen worden gebruikt voor het plaatsen van alle voorbeelden van grafiekbesturingselementen. Er zijn verschillende paren van `<div>`-elementen, één voor elk grafiekvoorbeeld:

   - Het eerste element (`class="rowOfCardsTitle"`) bevat een beschrijvende titel om samen te vatten wat de grafiek(en) laten zien. Bijvoorbeeld: 'Static Line Charts With Full Size Legends'.
   - Het tweede element (`class="rowOfCards"`) is een bovenliggend item dat aanvullende onderliggende `<div>`-elementen bevat die de feitelijke grafiekbesturingselementen in een rij plaatsen.

   ![Verschillende berichttekstelementen](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)

4. Vouw nu het `<script type="text/javascript">`-element, direct onder het `<div class="chartsWrapper">`-element, uit. U ziet het begin van de JavaScript-sectie op paginaniveau. Deze wordt gebruikt voor het afhandelen van alle paginalogica: verificatie, het aanroepen van API's van de TSI-service, het weergeven van de grafiekbesturingselementen en nog veel meer:

   ![Hoofdtekst van script](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)

## <a name="tsi-javascript-client-library-concepts"></a>Concepten van JavaScript-clientbibliotheek van TSI

Hoewel we de TSI-clientbibliotheek **tsclient.js** niet in detail gaan bekijken, biedt de bibliotheek in essentie een abstractie voor twee belangrijke categorieën:

- **Wrapper-methoden voor het aanroepen van de query-API's van TSI** - REST-API's waarmee u met behulp van samenvoegingsexpressies query's kunt uitvoeren op TSI-gegevens. De methoden zijn georganiseerd onder de `TsiClient.Server`-naamruimte van de bibliotheek.
- **Methoden voor het maken en vullen van verschillende typen grafiekbesturingselementen**: methoden die worden gebruikt om de samengevoegde TSI-gegevens weer te geven op een webpagina. De methoden zijn georganiseerd onder de `TsiClient.UX`-naamruimte van de bibliotheek.

De volgende concepten zijn universeel en van toepassing op de API's van de TSI-clientbibliotheek in het algemeen.

### <a name="authentication"></a>Verificatie

Zoals eerder gezegd, is dit voorbeeld een SPA (single-page application) die voor de verificatie van gebruikers gebruik maakt van de OAuth 2.0-ondersteuning in ADAL. Hier volgen enkele aandachtspunten voor deze sectie van het script:

1. Als ADAL wordt gebruikt voor verificatie, moet de clienttoepassing zichzelf registreren in het toepassingsregister van Azure Active Directory (Azure AD). Aangezien het een toepassing met één pagina is, wordt deze geregistreerd voor het gebruik van de 'impliciete' werkstroom voor autorisatieverlening van OAuth 2.0. Om die reden worden tijdens runtime enkele van de registratie-eigenschappen opgegeven door de toepassing, zoals de GUID van de client-id (`clientId`) en de omleidings-URI (`postLogoutRedirectUri`), die nodig zijn om deel te kunnen nemen aan de werkstroom.

2. Later vraagt de toepassing een 'toegangstoken' op bij Azure AD. Het toegangstoken wordt uitgegeven voor een beperkte set machtigingen voor een specifieke service/API-id https://api.timeseries.azure.com. De service-/API-id wordt ook wel het token 'doelgroep' genoemd. De tokenmachtigingen worden namens de aangemelde gebruiker uitgegeven. De id voor de service/API is nog een andere eigenschap die wordt opgenomen in de registratie van de toepassing bij Azure AD. Wanneer ADAL het toegangstoken aan de toepassing heeft geretourneerd, wordt dit doorgegeven als een 'bearer-token' bij toegang tot de API's van TSI.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=145-204&highlight=4-9,36-39)]

### <a name="control-identification"></a>Identificatie van besturingselementen

Zoals hierboven besproken, bieden de `<div>`-elementen in het `<body>`-element de indeling voor alle grafiekbesturingselementen die op de pagina worden weergegeven. Elk `<div>`-element bevat eigenschappen voor de plaatsing en visuele kenmerken van het grafiekbesturingselement, met inbegrip van een eigenschap `id`. De eigenschap `id` bevat een unieke id, die wordt gebruikt in de JavaScript-code om elk besturingselement te identificeren en te binden voor weergeven en bijwerken.

### <a name="aggregate-expressions"></a>Samenvoegingsexpressies

De API's van de TSI-clientbibliotheek maken intensief gebruik van samenvoegingsexpressies. Een samenvoegingsexpressie biedt de mogelijkheid om een of meer 'zoektermen' samen te stellen. De API's zijn ongeveer net zo ontworpen als [Time Series Insights Explorer](https://insights.timeseries.azure.com/demo), dat een zoekbereik gebruikt, bestaande uit waarden voor Predicate, Measures en Split By. De meeste bibliotheek API's nemen een matrix van samenvoegingsexpressies, die door de service worden gebruikt voor het samenstellen van een query op TSI-gegevens.

### <a name="call-pattern"></a>Aanroeppatroon

Voor het vullen en weergeven van grafiekbesturingselementen wordt een algemeen patroon gehanteerd. Dit patroon wordt overal in de JavaScript-code van de pagina gebruikt om de besturingselementen van de TSI-voorbeeldtoepassing te instantiëren en laden:

1. Declareer een `array` voor de opslag van een of meer samenvoegingsexpressies voor TSI:

   ```javascript
   var aes =  [];
   ```

2. Maak 1 tot n objecten voor samenvoegingsexpressies en voeg deze toe aan de matrix van samenvoegingsexpressies:

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **aggregateExpression-parameters**

   | Parameter | Beschrijving | Voorbeeld |
   | --------- | ----------- | ------- |
   | `predicateObject` | De expressie voor het filteren van gegevens. |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | De eigenschapsnaam van de meting die wordt gebruikt. | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | De gewenste samenvoegingen van de eigenschap Measure. | `['avg', 'min']` |
   | `searchSpan`      | De duur en intervalgrootte van de samenvoegingsexpressie. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | De tekenreekseigenschap die u wilt splitsen (optioneel; kan null zijn). | `{property: 'Station', type: 'String'}` |
   | `color`         | De kleur van de objecten die u wilt weergeven. | `'pink'` |
   | `alias`           | Een beschrijvende naam voor de samenvoegingsexpressie. | `'Factory3Temperature'` |
   | `contextMenuActions` | Een matrix van acties die moeten worden gebonden aan de tijdreeksobjecten in een visualisatie (optioneel). | Zie [Contextmenu's in de sectie Geavanceerde functies](#popup-context-menus) voor meer informatie. |

3. Roep een TSI-query aan met de API's van `TsiClient.Server` om de samengevoegde gegevens op te vragen:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates parameters**

   | Parameter | Beschrijving | Voorbeeld |
   | --------- | ----------- | ------- |
   | `token`     | Het toegangstoken voor de TSI-API. |  `authContext.getTsiToken()` Zie de sectie [Verificatie](#authentication) voor meer informatie. |
   | `envFQDN`   | De FQDN (Fully Qualified Domain Name) voor de TSI-omgeving. | Vanuit Azure Portal is dat bijvoorbeeld: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | Een matrix van TSI-query-expressies. | Gebruik de variabele `aes` zoals eerder is beschreven: `aes.map(function(ae){return ae.toTsx()}`. |

4. Transformeer het gecomprimeerde resultaat dat is geretourneerd door de TSI-query, naar JSON voor visualisatie:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

5. Maak een grafiekbesturingselement met de API's van `TsiClient.UX` en bind het element aan een van de `<div>`-elementen op de pagina:

   ```javascript
   var lineChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

6. Vul het grafiekbesturingselement met de getransformeerde JSON-gegevensobjecten en geef het besturingselement weer op de pagina:

   ```javascript
   lineChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Besturingselementen weergeven

De TSI-clientbibliotheek bevat momenteel acht unieke besturingselementen voor analysedoeleinden: een lijndiagram, een cirkeldiagram, een staafdiagram, een heatmap, hiërarchische besturingselementen, een toegankelijk raster, tijdlijnen met discrete gebeurtenissen en tijdlijnen met statusovergangen.

### <a name="line-bar-pie-chart-examples"></a>Voorbeelden van lijn-, staaf- en cirkeldiagram

Bekijk de code achter sommige standaardbesturingselementen voor grafieken die in de toepassing worden gebruikt, en het programmeermodel en de programmeerpatronen die nodig zijn voor het maken van deze besturingselementen. Bestudeer met name de sectie met HTML onder de opmerking `// Example 3/4/5`. Deze zorgt voor de weergave van besturingselementen met de id-waarden `chart3`, `chart4` en `chart5`.

In stap 3 in de sectie [Paginabron en -structuur](#page-source-and-structure) hebt u gezien dat grafiekbesturingselementen in rijen op de pagina worden gerangschikt, waarbij elk element een beschrijvende titelrij heeft. In dit voorbeeld worden de drie grafieken gevuld onder de titel 'Multiple Chart Types From the Same Data' `<div>`. Ze worden gekoppeld aan de drie `<div>`-elementen onder de titel:

[!code-javascript[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

In de volgende sectie met JavaScript-code worden patronen gebruikt die eerder zijn beschreven: maak TSI-samenvoegingsexpressies, voer ze uit om TSI-gegevens op te vragen en geef de drie grafieken weer. U ziet de drie typen die uit de naamruimte `tsiClient.ux` worden gebruikt: `LineChart`, `BarChart` en `PieChart`. Hiermee worden de respectieve grafieken gemaakt en weergegeven. Ook belangrijk om te weten is dat alle drie de grafieken dezelfde samengevoegde expressiegegevens kunnen gebruiken, namelijk `transformedResult`:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

De drie grafieken worden als volgt weergegeven:

[![Multiple Chart Types From The Same Data](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Geavanceerde functies

De TSI-clientbibliotheek heeft ook een paar optionele geavanceerde functies die goed van pas kunnen komen.

### <a name="states-and-events"></a>Statussen en gebeurtenissen

Een voorbeeld van zo'n geavanceerde functionaliteit is de mogelijkheid om statusovergangen en discrete gebeurtenissen toe te voegen aan grafieken. Deze functie is handig voor het markeren van incidenten, waarschuwingen en statustoestanden zoals aan/uit.

Bekijk de code achter de HTML-sectie onder de opmerking `// Example 10`. Met de code wordt een lijnbesturingselement weergegeven onder de titel 'Line Charts with Multiple Series Types', waarbij dit element wordt gekoppeld aan het `<div>`-element met de id-waarde `chart10`.

1. Eerst wordt er een structuur met de naam `events4` gedefinieerd. Deze dient om de elementen voor statuswijzigingen op te slaan die moeten worden bijgehouden. De structuur bevat:

   - Een tekenreekssleutel met de naam `Component States`.
   - Een matrix van waardeobjecten die de statussen voorstellen. Elk object omvat:
     - Een tekenreekssleutel die een ISO-tijdstempel in JavaScript bevat.
     - Een matrix die de kenmerken van de status bevat: een kleur en een beschrijving.

2. Vervolgens wordt er een structuur `events5` gedefinieerd voor 'Incidents'. Deze bevat een matrix van de gebeurteniselementen die moeten worden bijgehouden. De matrixstructuur heeft dezelfde vorm als de structuur die is beschreven voor `events4`.

3. Ten slotte wordt het lijndiagram weergegeven, waarbij de twee structuren worden doorgeven met de parameters voor grafiekopties: `events:` en `states:`. Kijk ook naar de andere optieparameters, waarmee een `tooltip:`, `theme:` of `grid:` kan worden opgegeven.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

De ruitvormige markeringen/pop-upvensters, waarmee incidenten worden aangegeven, en de gekleurde balken/pop-upvensters langs de tijdschaal, geven op visuele wijze statuswijzigingen aan:

[![Line Charts with Multiple Series Types](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Pop-upcontextmenu's

Een ander voorbeeld van geavanceerde functionaliteit zijn aangepaste contextmenu's (rechtsklikmenu's). Aangepaste contextmenu's zijn handig voor het inschakelen van acties en logische vervolgstappen binnen het bereik van uw toepassing.

Bekijk de code achter de HTML-sectie onder de opmerking `// Example 13/14/15`. Met deze code wordt in eerste instantie een lijndiagram weergegeven onder de titel 'Line Chart with Context Menu to Create Pie/Bar Chart'. Het diagram wordt bovendien gekoppeld aan het `<div>`-element met de id-waarde `chart13`. Met behulp van contextmenu's biedt het lijndiagram de mogelijkheid om dynamisch een cirkel- en staafdiagram te maken, die worden gekoppeld aan `<div>`-elementen met de id's `chart14` en `chart15`. Daarnaast gebruiken de cirkel- en staafdiagrammen contextmenu's om hun eigen functies aan te bieden: respectievelijk de mogelijkheid om gegevens te kopiëren van het cirkeldiagram naar het staafdiagram, en het weergeven van de gegevens uit het staafdiagram in het consolevenster van de browser.

1. Eerst wordt er een reeks aangepaste acties gedefinieerd. Elke actie bevat een matrix met een of meer elementen. Elk element definieert één contextmenuopdracht:

   - `barChartActions`: deze actie definieert het contextmenu voor het cirkeldiagram, met één element voor het definiëren van één menuopdracht:
     - `name`: de tekst die wordt gebruikt voor de menuopdracht: 'Print parameters to console'.
     - `action`: de actie die aan de menuopdracht is gekoppeld. De actie is altijd een anonieme functie die drie argumenten accepteert. Deze zijn gebaseerd op de samenvoegingsexpressie die is gebruikt voor het maken van de grafiek. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
       - `ae`: de matrix met de samenvoegingsexpressie.
       - `splitBy`: de waarde voor splitBy.
       - `timestamp`: het tijdstempel.

   - `pieChartActions`: deze actie definieert het contextmenu voor het staafdiagram, dat één element bevat voor het definiëren van één menuopdracht. De vorm en het schema zijn hetzelfde als van het vorige `barChartActions`-element, maar u ziet dat de functie `action` er heel anders uitziet: hiermee wordt het staafdiagram geïnstantieerd en weergegeven. Het argument `ae` wordt gebruikt om de matrix met de samenvoegingsexpressie op te geven die in runtime wordt doorgegeven wanneer de menuopdracht wordt geopend. Met de functie wordt ook de eigenschap `ae.contextMenu` ingesteld met het contextmenu `barChartActions`.
   - `contextMenuActions`: deze actie definieert het contextmenu voor het lijndiagram, dat drie elementen bevat voor het definiëren van drie menuopdrachten. De vorm en het schema voor elk element zijn hetzelfde als voor de eerder beschreven elementen. Net als bij het `barChartActions`-element, worden met de eerste opdracht drie functieargumenten weggeschreven naar het consolevenster van de browser. Net als bij het `pieChartActions`-elementen, worden met de volgende twee items respectievelijk de cirkel- en staafdiagrammen geïnstantieerd en weergegeven. Met de volgende twee items worden ook hun eigenschappen `ae.contextMenu` ingesteld met respectievelijk de contextmenu's `pieChartActions` en `barChartActions`.

2. Daarna worden twee samenvoegingsexpressies naar de matrix met de samenvoegingsexpressie `aes` gepusht en wordt voor elk item de matrix `contextMenuActions` opgegeven. Deze expressies worden gebruikt met het besturingselement voor het lijndiagram.

3. Ten slotte wordt in eerste instantie alleen het lijndiagram weergegeven, van waaruit zowel het cirkel- als het staafdiagram tijdens runtime kan worden weergegeven.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

In de schermafbeelding ziet u de grafieken met hun respectieve pop-upcontextmenu's. De cirkel- en staafdiagrammen zijn dynamisch gemaakt met behulp van de opties in het contextmenu van het lijndiagram.

[![Lijndiagram met contextmenu voor maken van cirkel- of staafdiagram](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Kwasten

Kwasten of penselen worden gebruikt om een tijdbereik te bepalen voor acties zoals zoomen en verkennen.

De code die wordt gebruikt om het werken met kwasten te demonstreren, wordt weergegeven in het vorige voorbeeld ('Line Chart with Context Menu to Create Pie/Bar Chart'), waarin [Pop-upcontextmenu's](#popup-context-menus-section) worden beschreven.

1. Kwastacties lijken erg op een contextmenu, in zoverre dat hiermee een reeks aangepaste acties voor de kwast kan worden gedefinieerd. Elke actie bevat een matrix met een of meer elementen. Elk element definieert één contextmenuopdracht:
   - `name`: de tekst die wordt gebruikt voor de menuopdracht: 'Print parameters to console'.
   - `action`: de actie die is gekoppeld aan het de menuopdracht, die altijd een anonieme functie is die twee argumenten accepteert. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
      - `fromTime`: het 'van'-tijdstempel van de kwastselectie.
      - `toTime`: het 'tot'-tijdstempel van de kwastselectie.

2. Kwastacties worden toegevoegd als een eigenschap voor grafiekopties. Let op de eigenschap `brushContextMenuActions: brushActions` die wordt doorgegeven aan de aanroep `linechart.Render`.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

![Lijndiagram met contextmenu voor maken van cirkel- of staafdiagram met kwasten](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Aanmelden en de TSI-voorbeeldtoepassing en de bron ervan bekijken.
> * API's in de JavaScript-clientbibliotheek van TSI gebruiken.
> * JavaScript gebruiken om grafiekbesturingselementen te maken en vullen met TSI gegevens.

Zoals besproken, wordt in de TSI-voorbeeldtoepassing een demo-gegevensset gebruikt. Ga naar het volgende artikel voor informatie over het maken van een eigen TSI-omgeving en gegevensset:

> [!div class="nextstepaction"]
> [Zelfstudie: Een Azure Time Series Insights-omgeving maken](tutorial-create-populate-tsi-environment.md)


