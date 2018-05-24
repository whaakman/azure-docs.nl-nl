---
title: JavaScript-clientbibliotheek van Time Series Insights ontdekken
description: Lees hier alles over de JavaScript-clientbibliotheek en het bijbehorende programmeermodel van Time Series Insights.
documentationcenter: ''
services: time-series-insights
author: BryanLa
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: time-series-insights
ms.workload: na
ms.tgt_pltfrm: ''
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2018
ms.author: bryanla
ms.openlocfilehash: 5b845f36dbb65b38d0e4ac2a118277027239b3d6
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="tutorial-explore-the-time-series-insights-javascript-client-library"></a>Zelfstudie: JavaScript-clientbibliotheek van Time Series Insights ontdekken

Om ontwikkelaars te helpen bij het benaderen en visualiseren van gegevens die zijn opgeslagen in Time Series Insights (TSI), hebben we een bibliotheek met besturingselementen op basis van JavaScript D3 gemaakt. Aan de hand van een voorbeeld van een webtoepassing maakt u in deze zelfstudie kennis met de JavaScript-clientbibliotheek van TSI en het bijbehorende programmeermodel. 

De behandelde onderwerpen bieden u de mogelijkheid om te experimenteren, een beter begrip te krijgen van hoe u TSI-gegevens benadert en grafiekbesturingselementen te gebruiken voor het weergeven en visualiseren van gegevens. Het doel is om u voldoende informatie te bieden, zodat u de bibliotheek in uw eigen webtoepassing kunt gebruiken.

In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> * De TSI-voorbeeldtoepassing 
> * De JavaScript-clientbibliotheek van TSI
> * Hoe de voorbeeldtoepassing de bibliotheek gebruikt om TSI gegevens te visualiseren

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt de functie 'Developer Tools' gebruikt (ook wel bekend als DevTools of F12), een functie die beschikbaar is in de meeste moderne webbrowsers, zoals [Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [FireFox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools) en [Safari](https://developer.apple.com/safari/tools/). Als u nog niet bekend bent met deze functie, is het een goed idee om wat te experimenteren met deze functie in uw browser voordat u verdergaat met deze zelfstudie. 

## <a name="the-time-series-insights-sample-application"></a>De Time Series Insights-voorbeeldtoepassing

In deze zelfstudie wordt de Time Series Insights-voorbeeldtoepassing met de naam 'Time Series Insights Sample App' gebruikt om meer te weten te komen over de broncode achter de toepassing, inclusief informatie over het gebruik van de JavaScript-clientbibliotheek van TSI. De toepassing is een webtoepassing die uit één pagina bestaat (SPA), waarin u ziet hoe de bibliotheek kan worden gebruikt voor het benaderen en visualiseren van gegevens uit een voorbeeld-TSI-omgeving. 

1. Ga naar de [Time Series Insights-voorbeeldtoepassing](https://insights.timeseries.azure.com/clientsample). U ziet een pagina zoals in de volgende schermafbeelding, waarin u wordt gevraagd u aan te melden: ![Prompt voor aanmelden bij TSI Sample App](media/tutorial-explore-js-client-lib/tcs-sign-in.png)

2. Klik op de knop Log in en typ of selecteer uw referenties. U kunt een account van uw onderneming of organisatie gebruiken (Azure Active Directory) of een persoonlijk account (Microsoft-account of MSA). 

   ![Prompt voor invoeren van referenties voor TSI Sample App](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)

3. Als u bent aangemeld, ziet u een pagina die vergelijkbaar is met de onderstaande schermafbeelding, met verschillende stijlen van voorbeeldgrafieken, gevuld met TSI-gegevens. Rechtsboven ziet u uw gebruikersaccount en een link om u af te melden (Log out): ![Hoofdpagina van TSI Sample App na aanmelding](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)

### <a name="page-source-and-structure"></a>Paginabron en -structuur

Laten we eerst eens kijken naar de HTML- en JavaScript-broncode achter de pagina die wordt weergegeven in uw browser. We gaan niet alle elementen bespreken, maar alleen de hoofdonderdelen, zodat u een goed beeld krijgt van de werking van de pagina:

1. Open 'Developer Tools' in uw browser en bekijk de HTML-elementen die de huidige pagina vormen, ook wel aangeduid als de HTML- of DOM-structuur.

2. Vouw de elementen `<head>` en `<body>` uit en let op de volgende secties:
   - Onder `<head>` ziet u elementen waarmee aanvullende bestanden worden opgehaald om te pagina op de gewenste manier te laten werken:
     - Een `<script>`-element om te verwijzen naar de verificatiebibliotheek van Azure Active Directory (adal.min.js), ook wel bekend als ADAL. Dit is een JavaScript-bibliotheek met functies voor OAuth 2.0-verificatie (aanmelding) en het ophalen van tokens voor toegang tot API's:
     - `<link>`-elementen voor opmaakmodellen (sampleStyles.css, tsiclient.css), ook wel bekend als CSS. Deze elementen worden gebruikt voor het beheren van de visuele opmaak van de pagina, zoals kleuren, lettertypen, afstand, enzovoort. 
     - Een `<script>`-element om te verwijzen naar de TSI clientbibliotheek (tsiclient.js), een JavaScript-bibliotheek die door de pagina wordt gebruikt voor het aanroepen van TSI-API's en het weergeven van grafiekbesturingselementen op de pagina.

     >[!NOTE]
     > De broncode voor de ADAL JavaScript-bibliotheek is beschikbaar in de [opslagplaats azure-activedirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  
     > De JavaScript-broncode voor de TSI-clientbibliotheek is beschikbaar in de [opslagplaats tsiclient](https://github.com/Microsoft/tsiclient).

   - Onder `<body>` vindt u `<div>`-elementen, die fungeren als containers voor het definiëren van de indeling van items op de pagina, plus een ander `<script>`-element:
     - De eerste `<div>` verwijst naar het dialoogvenster Log in (`id="loginModal"`).
     - De tweede `<div>` fungeert als een bovenliggend item voor:
       - een koptekst-`<div>`, die wordt gebruikt voor statusberichten en aanmeldingsgegevens aan de bovenkant van de pagina (`class="header"`).
       - een `<div>` voor de overige hoofdtekstelementen (body) van de pagina, inclusief alle grafieken (`class="chartsWrapper"`).
       - een sectie `<script>`, met daarin alle JavaScript-code die wordt gebruikt voor het beheren van de pagina.

   [![TSI Sample App met DevTools](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

3. Vouw het element `<div class="chartsWrapper">` uit en u ziet nog meer onderliggende `<div>`-elementen, die worden gebruikt om de verschillende grafiekvoorbeelden te positioneren. Er zijn verschillende paren met `<div>`-elementen, één voor elk grafiekvoorbeeld:
   - Het eerste element (`class="rowOfCardsTitle"`) bevat een beschrijvende titel om samen te vatten wat de grafiek(en) laten zien, bijvoorbeeld: 'Static Line Charts With Full Size Legends'
   - Het tweede element (`class="rowOfCards"`) is een bovenliggend item, met aanvullende onderliggende `<div>`-elementen die het feitelijke grafiekbesturingselement binnen een rij positioneren. 

  !['div'-elementen in hoofdtekst weergeven](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)

4. Vouw nu het element `<script type="text/javascript">` uit, direct onder het element `<div class="chartsWrapper">`. U ziet het begin van de sectie met JavaScript op paginaniveau, die wordt gebruikt voor het afhandelen van alle paginalogica voor zaken zoals verificatie, aanroepen van TSI-API's en weergave van de grafiekbesturingselementen:

  ![Script voor hoofdtekst weergeven](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)

## <a name="tsi-client-javascript-library-concepts"></a>Concepten van JavaScript-clientbibliotheek van TSI

Hoewel we de TSI-clientbibliotheek (tsclient.js) niet in detail gaan bekijken, biedt de bibliotheek in essentie een abstractie voor twee belangrijke categorieën:

- **Wrapper-methoden voor het aanroepen van de query-API's van TSI** - REST-API's die de mogelijkheid bieden query's uit te voeren op TSI-gegevens met behulp van samenvoegingsexpressies, en die geordend onder de naamruimte `TsiClient.Server` van de bibliotheek. 
- **Methoden voor het maken en vullen van verschillende typen grafiekbesturingselementen** - Deze methoden worden gebruikt voor het weergeven van de samengevoegde gegevens van TSI op een webpagina, en ze zijn geordend onder de naamruimte `TsiClient.UX` van de bibliotheek. 

De volgende concepten zijn universeel en van toepassing op de API's van de TSI-clientbibliotheek in het algemeen. 

### <a name="authentication"></a>Verificatie

Zoals eerder gezegd, is dit voorbeeld een toepassing met één pagina en wordt OAuth 2.0-ondersteuning in ADAL gebruikt voor verificatie van gebruikers. Hier volgen enkele aandachtspunten voor deze sectie van het script:

1. Om ADAL te gebruiken voor verificatie moet de clienttoepassing zichzelf registreren in het toepassingsregister van Azure Active Directory (Azure AD). Aangezien het een toepassing met één pagina is, wordt deze geregistreerd voor het gebruik van de 'impliciete' werkstroom voor autorisatieverlening van OAuth 2.0. Om die reden worden tijdens runtime enkele van de registratie-eigenschappen opgegeven door de toepassing, zoals de GUID van de client-id (`clientId`) en de omleidings-URI (`postLogoutRedirectUri`), die nodig zijn om deel te kunnen nemen aan de werkstroom.

2. Later vraagt de toepassing een 'toegangstoken' op bij Azure AD. Het toegangstoken wordt uitgegeven voor een beperkte set machtigingen, voor een specifieke service/API-id (https://insights.timeseries.azure.com), ook wel aangeduid als het token 'audience'. De tokenmachtigingen worden namens de aangemelde gebruiker uitgegeven. De id voor de service/API is een andere eigenschap die wordt opgenomen in de registratie van de toepassing bij Azure AD. Als ADAL het toegangstoken retourneert aan de toepassing, wordt dit doorgegeven als een 'bearer-token' bij toegang tot de API's van TSI. 

   [!code-javascript[head-sample](~/samples-javascript/pages/index.html?range=140-199&highlight=4-9,36-39)]

### <a name="control-identification"></a>Identificatie van besturingselementen

Zoals hierboven besproken, bieden de `<div>`-elementen in de `<body>` de indeling voor alle grafiekbesturingselementen die worden getoond op de pagina. Elk element bevat eigenschappen voor de plaatsing en visuele kenmerken van het grafiekbesturingselement, met inbegrip van de eigenschap `id`. De eigenschap `id` bevat een unieke id, die wordt gebruikt in de JavaScript-code om elk besturingselement te identificeren en te binden voor weergeven en bijwerken. 

### <a name="aggregate-expressions"></a>Samenvoegingsexpressies

De API's van de TSI-clientbibliotheek maken intensief gebruik van samenvoegingsexpressies. Een samenvoegingsexpressie biedt de mogelijkheid om een of meer 'zoektermen' samen te stellen. De API's zijn vergelijkbaar met de manier waarop in [Time Series Insights Explorer](https://insights.timeseries.azure.com/demo) een zoekbereik wordt gebruikt, bestaande uit waarden voor Predicate, Measures en Split By. De meeste bibliotheek API's nemen een matrix van samenvoegingsexpressies, die door de service worden gebruikt voor het samenstellen van een query op TSI-gegevens.

### <a name="call-pattern"></a>Aanroeppatroon

Voor het vullen en weergeven van grafiekbesturingselementen wordt een algemeen patroon gehanteerd. Dit patroon wordt overal in de JavaScript-code van de pagina gebruikt om de besturingselementen van de TSI-voorbeeldtoepassing te instantiëren en laden:

1. Declareer een matrix voor het opslaan van een of meer samenvoegingsexpressies voor TSI.  

   ```javascript
   var aes =  [];
   ```

2. Definieer 1 tot n objecten voor samenvoegingsexpressies en voeg deze toe aan de matrix van samenvoegingsexpressies.  

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```
   **aggregateExpression-parameters**

   | Parameter | Beschrijving | Voorbeeld |
   | --------- | ----------- | ------- |
   | predicateObject | De expressie voor het filteren van gegevens. |`{predicateString: "Factory = 'Factory3'"}` |
   | measureObject   | De eigenschapsnaam van de meting die wordt gebruikt. | `{property: 'Temperature', type: "Double"}` |
   | measureTypes    | De gewenste samenvoegingen van de eigenschap Measure. | `['avg', 'min']` |
   | searchSpan      | De duur en intervalgrootte van de samenvoegingsexpressie. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | splitByObject   | De tekenreekseigenschap die u wilt splitsen (optioneel; kan niet null zijn). | `{property: 'Station', type: 'String'}` |
   | color           | De kleur van de objecten die u wilt weergeven. | `'pink'` |
   | alias           | Een beschrijvende naam voor de samenvoegingsexpressie. | `'Factory3Temperature'` |
   | contextMenuActions | Een matrix van acties die moeten worden gebonden aan de tijdreeksobjecten in een visualisatie (optioneel). | Zie [Contextmenu's in de sectie Geavanceerde functies.](#popup-context-menus) |

3. Roep een TSI-query aan met API's van `TsiClient.Server` om de samengevoegde gegevens op te vragen.  

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```
   **getAggregates parameters**

   | Parameter | Beschrijving | Voorbeeld |
   | --------- | ----------- | ------- |
   | token     | Toegangstoken voor de TSI-API. | `authContext.getTsiToken()` Zie [de sectie Verificatie.](#authentication) |
   | envFQDN     | De Fully Qualified Domain Name voor de TSI-omgeving. | Vanuit Azure Portal is dat bijvoorbeeld `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com` |
   | aeTsxArray | Matrix van TSI-query-expressies. | Gebruik de variabele `aes` zoals eerder is beschreven: `aes.map(function(ae){return ae.toTsx()}` |

4. Transformeer het gecomprimeerde resultaat dat is geretourneerd door de TSI-query, naar JSON voor visualisatie.

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

5. Maak een grafiekbesturingselement met API's van `TsiClient.UX`, en bindt het element aan een van de `<div>`-elementen op de pagina.

   ```javascript
   var lineChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

6. Vul het grafiekbesturingselement met de getransformeerde JSON-gegevensobjecten en geef het element weer op de pagina.

   ```javascript
   lineChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Besturingselementen weergeven

De bibliotheek bevat momenteel acht unieke besturingselementen voor analysedoeleinden. Het betreft een lijndiagram, een cirkeldiagram, een staafdiagram, een heatmap, hiërarchische besturingselementen, een toegankelijk raster, tijdlijnen met discrete gebeurtenissen en tijdlijnen met statusovergangen.   

### <a name="line-bar-pie-chart-examples"></a>Voorbeelden van lijn-, staaf- en cirkeldiagram 

Laten we eerst eens kijken naar de code achter sommige standaardbesturingselementen voor grafieken die in de toepassing worden gebruikt. We bespreken ook het programmeermodel en de programmeerpatronen die nodig zijn voor het maken van deze besturingselementen. We gaan in het bijzonder kijken naar de sectie met HTML onder de opmerking `// Example 3/4/5`, die zorgt voor de weergave van besturingselementen met de id-waarden `chart3`, `chart4` en `chart5`. 

Zoals we hebben gezien in stap 3 in de sectie [Paginabron en -structuur](#page-source-and-structure), worden grafiekbesturingselementen in rijen gerangschikt op de pagina, waarbij elk element een beschrijvende titelrij heeft. In dit voorbeeld staan de drie grafieken die worden ingevuld allemaal onder de titel 'Multiple Chart Types From the Same Data' `<div>`, en worden ze gebonden aan de drie `<div>`-elementen eronder:

[!code-javascript[code-sample1-line-bar-pie](~/samples-javascript/pages/index.html?range=60-74&highlight=1,5,9,13)]

In de volgende sectie met JavaScript-code wordt het eerder beschreven patroon gebruik voor het bouwen van TSI-samenvoegingsexpressies, het uitvoeren van deze query's om te zoeken naar TSI gegevens en het weergeven van de drie grafieken. U ziet dat de drie typen uit de naamruimte `tsiClient.ux`, `LineChart`, `BarChart` en `PieChart`, worden gebruikt voor het maken en weergeven van de respectieve grafieken. Ook belangrijk om te weten is dat alle drie de grafieken dezelfde samengevoegde expressiegegevens kunnen gebruiken, namelijk `transformedResult`:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/index.html?range=236-257&highlight=13-14,16-17,19-20)]

De drie grafieken worden als volgt weergegeven:

[![Multiple Chart Types From the Same Data](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Geavanceerde functies

De bibliotheek biedt ook een paar optionele geavanceerde functies die goed van pas kunnen komen.  

### <a name="states-and-events"></a>Statussen en gebeurtenissen

Een voorbeeld van geavanceerde functionaliteit is de mogelijkheid om statusovergangen en discrete gebeurtenissen toe te voegen aan grafieken. Deze functie is handig voor het markeren van incidenten, waarschuwingen en statustoestanden zoals aan/uit. 

Hier ziet u de code achter de HTML-sectie onder de opmerking `// Example 10`. Met de code wordt een lijnbesturingselement weergegeven onder de titel 'Line Charts with Multiple Series Types', waarbij dit element wordt gebonden aan de `<div>` met de id-waarde `chart10`:

1. Eerst wordt er een structuur met de naam `events4` gedefinieerd, voor het opslaan van de elementen voor statuswijziging die moeten worden bijgehouden. Deze structuur bevat het volgende:
   - Een tekenreekssleutel met de naam `"Component States"` 
   - Een matrix van waardeobjecten die de statussen voorstellen, waarbij elk object het volgende omvat:
     - Een tekenreekssleutel met een ISO-tijdstempel in JavaScript
     - Een matrix met kenmerken van de status
       - een kleur
       - een beschrijving

2. Vervolgens wordt er een structuur `events5` gedefinieerd voor `"Incidents"`, met daarin een matrix van de gebeurteniselementen die moeten worden bijgehouden. De matrixstructuur heeft dezelfde vorm als de structuur voor `events4`. 

3. Ten slotte wordt het lijndiagram weergegeven, waarbij de twee structuren worden doorgeven met de parameters voor grafiekopties: `events:` en `states:`. Kijk ook naar de andere optieparameters, voor het opgeven van een `tooltip:`, `theme:` of `grid:`. 

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/index.html?range=332-384&highlight=5,26,51)]

Visueel gezien worden de ruitvormige markeringen/pop-ups gebruikt om incidenten aan te geven, terwijl de gekleurde balken/pop-ups langs de tijdschaal statuswijzigingen aangeven:

[![Line Charts with Multiple Series Types](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="popup-context-menus"></a>Contextmenu's

Een ander voorbeeld van geavanceerde functionaliteit zijn aangepaste contextmenu's of snelmenu's (menu's die verschijnen als u met de rechtermuisknop ergens op klikt). Deze zijn nuttig voor het inschakelen van acties en logische vervolgstappen binnen het bereik van uw toepassing.

Hier zien we de code achter de HTML onder `// Example 13/14/15`. Met deze code wordt in eerste instantie een lijndiagram weergegeven onder de titel 'Line Chart with Context Menu to Create Pie/Bar Chart', dat wordt gebonden aan het `<div>`-element met de id-waarde `chart13`. Met behulp van contextmenu's biedt het lijndiagram de mogelijkheid om dynamisch een cirkel- en staafdiagram te maken, die worden gebonden aan `<div>`-elementen met de id's `chart14` en `chart15`. Daarnaast gebruiken de cirkel- en staafdiagrammen contextmenu's om hun eigen functies aan te bieden: respectievelijk de mogelijkheid om gegevens te kopiëren van het cirkeldiagram naar het staafdiagram, en het weergeven van de gegevens uit het staafdiagram in het consolevenster van de browser.

1. Eerst wordt er een reeks aangepaste acties gedefinieerd. Elke actie bevat een matrix met een of meer elementen, waarbij elk element een bepaald menu-item van het contextmenu definieert: 
   - `barChartActions`: definieert het contextmenu voor het cirkeldiagram, met één element voor het definiëren van één menu-item:
     - `name`: de tekst die wordt gebruikt voor het menu-item: 'Print parameters to console'
     - `action`: de actie die is gekoppeld aan het menu-item, wat altijd een anonieme functie is die drie argumenten accepteert op basis van de samenvoegingsexpressie die is gebruikt voor het maken van de grafiek. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
       - `ae`: de matrix met de samenvoegingsexpressie
       - `splitBy`: de waarde voor splitBy
       - `timestamp`: het tijdstempel
   - `pieChartActions`: definieert het contextmenu voor het staafdiagram, met één element voor het definiëren van één menu-item. De vorm en het schema zijn hetzelfde als eerder beschreven bij `barChartActions`, maar u ziet dat de functie `action` er heel anders uitziet, aangezien deze het staafdiagram instantieert en weergeeft. Het argument `ae` wordt gebruikt voor het opgeven van de matrix met de samenvoegingsexpressie, die tijdens runtime kan worden opgegeven via een pop-up met het menu-item. Met de functie wordt ook de eigenschap `ae.contextMenu` ingesteld met het contextmenu `barChartActions`.
   - `contextMenuActions`: definieert het contextmenu voor het lijndiagram, dat drie elementen bevat om drie menu-items te definiëren. De vorm en het schema voor elk element zijn hetzelfde als hierboven. Net als bij `barChartActions`, worden met het eerste item drie functieargumenten weggeschreven naar het consolevenster van de browser. Net als bij `pieChartActions`, worden met de volgende twee items respectievelijk de cirkel- en staafdiagrammen geïnstantieerd en weergegeven. Met de volgende twee items worden ook hun eigenschappen `ae.contextMenu` ingesteld met respectievelijk de contextmenu's `pieChartActions` en `barChartActions`.

2. Vervolgens worden twee samenvoegingsexpressies naar de matrix met de samenvoegingsexpressie `aes` gepusht, die voor beide de matrix `contextMenuActions` opgeven. Deze worden gebruikt met het besturingselement voor het lijndiagram.

3. Ten slotte wordt in eerste instantie alleen het lijndiagram weergegeven, van waaruit zowel het cirkel- als het staafdiagram tijdens runtime kan worden weergegeven.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/index.html?range=456-535&highlight=7,16,29,61-64,78)]

In de schermafbeelding ziet u de grafieken, met hun respectieve contextmenu's. De cirkel- en staafdiagrammen zijn dynamisch gemaakt met behulp van de opties in het contextmenu van het lijndiagram:

[![Lijndiagram met contextmenu voor maken van cirkel- of staafdiagram](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Kwasten

Kwasten of penselen kunnen worden gebruikt om een tijdbereik te bepalen voor acties zoals zoomen en verkennen. 

De code die wordt gebruikt om het werken met kwasten te demonstreren, wordt ook toegepast in het vorige voorbeeld 'Line Chart with Context Menu to Create Pie/Bar Chart', in [Contextmenu's](#popup-context-menus-section). 

1. Kwastacties lijken erg op een contextmenu en definiëren , een reeks aangepaste acties voor de kwast. Elke actie bevat een matrix met een of meer elementen, waarbij elk element een bepaald menu-item van het contextmenu definieert:
   - `name`: de tekst die wordt gebruikt voor het menu-item: 'Print parameters to console'
   - `action`: de actie die is gekoppeld aan het menu-item, wat altijd een anonieme functie is die twee argumenten accepteert. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
      - `fromTime`: het 'van'-tijdstempel van de kwastselectie
      - `toTime`: het 'tot'-tijdstempel van de kwastselectie

2. Kwastacties worden toegevoegd als een eigenschap voor grafiekopties. Merk ook op dat de eigenschap `brushContextMenuActions: brushActions` wordt doorgegeven aan de aanroep `linechart.Render`.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/index.html?range=521-535&highlight=1,13)]

![Lijndiagram met contextmenu voor maken van cirkel- of staafdiagram met kwasten](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Aanmelden en de TSI-voorbeeldtoepassing en de bron ervan bekijken
> * API's in de JavaScript-clientbibliotheek van TSI gebruiken
> * JavaScript gebruiken om grafiekbesturingselementen te maken en vullen met TSI gegevens

Zoals besproken, wordt in de TSI-voorbeeldtoepassing een demo-gegevensset gebruikt. Ga naar het volgende artikel voor meer informatie over het maken van een eigen TSI-omgeving en gegevensset:

> [!div class="nextstepaction"]
> [Uw Azure Time Series Insights-omgeving plannen](time-series-insights-environment-planning.md)


