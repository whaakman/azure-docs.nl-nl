---
title: 'Zelfstudie: De JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken | Microsoft Docs'
description: Informatie over de JavaScript-clientbibliotheek en het bijbehorende programmeermodel van Azure Time Series Insights.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 04/23/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: a91afdbeaa2ced37b237b4f2b80a8dbbe2c4a05c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717222"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Zelfstudie: De JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken

Om webontwikkelaars te helpen bij het opvragen en visualiseren van gegevens die zijn opgeslagen in Time Series Insights (TSI), is de TSI-clientbibliotheek op basis van JavaScript D3 ontwikkeld. In deze zelfstudie leidt u door de TSI-clientbibliotheek en programmeermodel met behulp van een gehoste voorbeeld-app.

De zelfstudie over het werken met de bibliotheek wordt uitgelegd hoe u toegang tot gegevens van TSI en grafiekbesturingselementen gebruiken om te genereren en visualiseren van gegevens. U leert ook hoe u om te experimenteren met verschillende soorten grafieken om gegevens te visualiseren. Aan het einde van de zelfstudie, kunt u zult de clientbibliotheek gebruiken voor het opnemen van TSI-functies in uw eigen web-app.

Specifiek, vindt u meer over:

> [!div class="checklist"]
> * De TSI-voorbeeldtoepassing.
> * De JavaScript-clientbibliotheek van TSI.
> * Hoe de voorbeeldtoepassing de bibliotheek gebruikt om TSI gegevens te visualiseren.

> [!NOTE]
> * De zelfstudie wordt gebruikgemaakt van een gratis, gehoste, [Time Series Insights web demo](https://insights.timeseries.azure.com/clientsample).
> * De bronbestanden van de Time Series Insights-voorbeeld-app vindt u in de [voorbeeldopslagplaats in GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).

## <a name="video"></a>Video

### <a name="in-this-video-we-introduce-the-open-source-time-series-insights-javascript-sdkbr"></a>In deze video stellen we de open source-SDK Time Series Insights voor JavaScript aan u voor.</br>

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie wordt gebruikgemaakt van uw browser **hulpprogramma's voor ontwikkelaars** functie. Moderne webbrowsers ([Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [FireFox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/), enzovoort) worden meestal aangeboden aan toegang tot de **Inspector Webweergave** via de `F12` sneltoets. Anders kunnen worden geopend door met de rechtermuisknop op een webpagina te selecteren **inspecteren Element**.

## <a name="time-series-insights-sample-application"></a>Time Series Insights-voorbeeldtoepassing

In deze zelfstudie wordt een gratis, gehoste, Time Series Insights-voorbeeldapp gebruikt om de broncode van de toepassing en de TSI van JavaScript-clientbibliotheek verkennen. Via deze leert u hoe u werken met TSI in JavaScript en visualiseren van gegevens via de diagrammen en grafieken.

1. Ga naar de [Time Series Insights-voorbeeldtoepassing](https://insights.timeseries.azure.com/clientsample). De volgende aanmelding prompt wordt weergegeven:

   [![Voorbeeld aanmelden opdrachtprompt TSI-Client](media/tutorial-explore-js-client-lib/tcs-sign-in.png)](media/tutorial-explore-js-client-lib/tcs-sign-in.png#lightbox)

1. Selecteer **aanmelden** invoeren of selecteren van uw referenties. Een enterprise-organisatie-account (Azure Active Directory) of een persoonlijk account (Microsoft-Account of MSA) gebruiken.

   [![Voorbeeld van TSI clientreferenties vragen](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png#lightbox)

1. Na het aanmelden ziet u een pagina met verschillende soorten grafieken ingevuld met gegevens van TSI. Uw gebruikersaccount en de optie **Afmelden** worden in de rechterbovenhoek weergegeven:

   [![TSI Client voorbeeld hoofdpagina na het aanmelden](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png#lightbox)

### <a name="page-source-and-structure"></a>Paginabron en -structuur

<div id="page-source-and-structure"></div>

Eerst gaan we bekijken de [HTML en JavaScript-broncode](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) van de gegenereerde web-wisselbaar geheugen:

1. Open **Ontwikkelhulpprogramma's** in uw browser. Bekijk de HTML-elementen waaruit de huidige pagina bestaat, ook wel de HTML- of DOM-structuur genoemd.

1. Vouw de `<head>` en `<body>` elementen en houd rekening met de volgende secties:

   * Onder de `<head>` -element, vindt u pagina-metagegevens en afhankelijkheden waarmee de app wordt uitgevoerd:
     * Een `<script>`-element dat wordt gebruikt om te verwijzen naar de Azure Active Directory Authentication Library **adal.min.js** (ook wel ADAL genoemd). ADAL is een JavaScript-bibliotheek die zorgt voor OAuth 2.0-verificatie (aanmelding) en het verkrijgen van een token voor toegang tot API's.
     * Meerdere `<link>`-elementen voor opmaakmodellen (ook wel CSS genoemd) zoals **sampleStyles.css** en **tsiclient.css**. De opmaakmodellen worden gebruikt voor het instellen van visuele paginaopmaakdetails, zoals de kleuren, lettertypen en afstand.
     * Een `<script>`-element dat wordt gebruikt om te verwijzen naar de JavaScript-clientbibliotheek van TSI, **tsiclient.js**. De bibliotheek wordt door de pagina gebruikt om API's van de TSI-service aan te roepen en grafiekbesturingselementen op de pagina weer te geven.

     >[!NOTE]
     > * De broncode voor de ADAL JavaScript-bibliotheek is beschikbaar in de [opslagplaats azure-activedirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js).
     > * De JavaScript-broncode voor de TSI-clientbibliotheek is beschikbaar in de [opslagplaats tsiclient](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).

   * Onder de `<body>` -element, vindt u `<div>` elementen, die het definiëren van de indeling van de items op de pagina, en een andere `<script>` element:
     * Het eerste `<div>`-element verwijst naar het dialoogvenster **Log in** (`id="loginModal"`).
     * Het tweede `<div>`-element fungeert als een bovenliggend item voor:
       * Een koptekst-`<div>`-element, dat wordt gebruikt voor statusberichten en aanmeldingsgegevens aan de bovenkant van de pagina (`class="header"`).
       * Een `<div>`-element voor de overige hoofdtekstelementen (body) van de pagina, inclusief alle grafieken (`class="chartsWrapper"`).
       * Een `<script>`-sectie, met daarin alle JavaScript-code die wordt gebruikt voor het beheren van de pagina.

   [![TSI Sample App met Ontwikkelhulpprogramma's](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

1. Vouw de `<div class="chartsWrapper">` -element en u vindt meer onderliggende `<div>` elementen. Deze elementen worden gebruikt voor het plaatsen van alle voorbeelden van grafiekbesturingselementen. Er zijn verschillende paren van `<div>`-elementen, één voor elk grafiekvoorbeeld:

   * Het eerste element (`class="rowOfCardsTitle"`) bevat een beschrijvende titel om samen te vatten wat de grafiek(en) laten zien. Bijvoorbeeld: `Static Line Charts With Full-Size Legends.`
   * Het tweede element (`class="rowOfCards"`) is een bovenliggend item dat aanvullende onderliggende `<div>`-elementen bevat die de feitelijke grafiekbesturingselementen in een rij plaatsen.

   [![Div-berichttekstonderdelen](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png#lightbox)

1. Vouw nu het `<script type="text/javascript">`-element, direct onder het `<div class="chartsWrapper">`-element, uit. U ziet het begin van de pagina op serverniveau JavaScript-sectie die wordt gebruikt voor het afhandelen van alle van de pagina logica (verificatie, aanroepen van API's, het weergeven van de besturingselementen kolomdiagram en meer van de TSI-service):

   [![Hoofdtekst script](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png#lightbox)

## <a name="tsi-javascript-client-library-concepts"></a>Concepten van JavaScript-clientbibliotheek van TSI

De TSI-clientbibliotheek (**tsclient.js**) abstracties voor twee belangrijke JavaScript-functies biedt:

* **Wrapper-methoden voor het aanroepen van de Query-API's voor TSI**: REST-API's waarmee u met behulp van samenvoegingsexpressies query's kunt uitvoeren op TSI-gegevens. De methoden zijn georganiseerd onder de `TsiClient.Server`-naamruimte van de bibliotheek.

* **Methoden voor het maken en vullen van verschillende typen grafiekbesturingselementen**: Methoden die worden gebruikt om de TSI-gegevens te visualiseren op een webpagina. De methoden zijn georganiseerd onder de `TsiClient.UX`-naamruimte van de bibliotheek.

Via deze vereenvoudigen, kunnen ontwikkelaars UI graph en grafiek-onderdelen die worden aangestuurd door gegevens van TSI eenvoudiger.

### <a name="authentication"></a>Verificatie

De [Time Series Insights-voorbeeldtoepassing](https://insights.timeseries.azure.com/clientsample) is een app met één pagina met ondersteuning voor verificatie van ADAL OAuth 2.0-gebruiker:

1. Wanneer u ADAL voor verificatie, moet de client-app in Azure Active Directory zijn geregistreerd. In feite de app met één pagina is geregistreerd voor het gebruik van de [OAuth 2.0-impliciete stroom verlenen](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-implicit-grant-flow).
1. Daarom moet de toepassing enkele van de registratie-eigenschappen tijdens runtime opgeven. Deze omvatten de client-GUID (`clientId`) en de omleidings-URI (`postLogoutRedirectUri`).
1. Later, de app verzoekt een **toegangstoken** uit Azure Active Directory. Het toegangstoken is uitgegeven voor een beperkte set machtigingen voor een specifieke service/API-id (`https://api.timeseries.azure.com`). De tokenmachtigingen worden namens de aangemelde gebruiker uitgegeven. De id voor de service /-API is een andere eigenschap die opgenomen in de Azure Active Directory-registratie van de app.
1. Het toegangstoken ADAL stuurt naar de app, wordt doorgegeven als een **bearer-token** wanneer toegang tot de TSI-service API's.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=3-7,34-37)]

### <a name="control-identification"></a>Identificatie van besturingselementen

In het opgegeven voorbeeld `<div>` elementen zijn gerangschikt in de bovenliggende `<body>` element voor een praktische indeling voor alle grafiekbesturingselementen in de weergegeven op de pagina.

Elke `<div>` element Hiermee geeft u de eigenschappen voor de plaatsing en visuele kenmerken van besturingselementen kolomdiagram. HTML-element `id` eigenschappen fungeren als unieke id's verbinding maken met specifieke besturingselementen voor het weergeven en bijwerken van de gevisualiseerde gegevens.

### <a name="aggregate-expressions"></a>Samenvoegingsexpressies

De TSI-clientbibliotheek API's statistische expressies gebruiken:

* Een statistische expressie biedt de mogelijkheid om te maken van een of meer **zoektermen**.

* De Client-API's zijn ontworpen om te bieden vergelijkbare functionaliteit als een andere bieden demo-app (de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com/demo)), waarin de tijdspanne voor zoeken, where-predicaat, metingen en split-by-waarde.

* De meeste van de clientbibliotheek API's krijgen een matrix van statistische expressies die worden gebruikt door de service voor het bouwen van een query op een TSI.

### <a name="call-pattern"></a>Aanroeppatroon

Invullen en rendering van besturingselementen kolomdiagram kunt u een algemeen patroon. Dit algemene patroon kan worden waargenomen in de voorbeeld-app en helpt u bij het gebruik van de Client-bibliotheek:

1. Declareer een `array` voor de opslag van een of meer samenvoegingsexpressies voor TSI:

   ```javascript
   var aes =  [];
   ```

1. Bouw *1* naar *n* statistische expressie objecten. Ze vervolgens toevoegen aan de matrix statistische expressie:

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **aggregateExpression-parameters**

   | Parameter | Description | Voorbeeld |
   | --------- | ----------- | ------- |
   | `predicateObject` | De filterexpressie van gegevens. |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | De eigenschapsnaam van de meting die wordt gebruikt. | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | De gewenste samenvoegingen van de eigenschap Measure. | `['avg', 'min']` |
   | `searchSpan`      | De duur en intervalgrootte van de samenvoegingsexpressie. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | De tekenreekseigenschap die u wilt splitsen (optioneel; kan null zijn). | `{property: 'Station', type: 'String'}` |
   | `color`         | De kleur van de objecten die u wilt weergeven. | `'pink'` |
   | `alias`           | Een beschrijvende naam voor de samenvoegingsexpressie. | `'Factory3Temperature'` |
   | `contextMenuActions` | Een matrix van acties die moeten worden gebonden aan de tijdreeksobjecten in een visualisatie (optioneel). | Zie voor meer informatie de sectie [pop-contextmenu's](#contextMenu) |

1. Roep een TSI-query aan met de API's van `TsiClient.Server` om de samengevoegde gegevens op te vragen:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates parameters**

   | Parameter | Description | Voorbeeld |
   | --------- | ----------- | ------- |
   | `token`     | Het toegangstoken voor de TSI-API. |  `authContext.getTsiToken()` Zie de sectie [Verificatie](#authentication) voor meer informatie. |
   | `envFQDN`   | De FQDN (Fully Qualified Domain Name) voor de TSI-omgeving. | Vanuit Azure Portal is dat bijvoorbeeld: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | Een matrix van TSI-query-expressies. | Gebruik de variabele `aes` zoals eerder is beschreven: `aes.map(function(ae){return ae.toTsx()}`. |

1. Transformeer het gecomprimeerde resultaat dat is geretourneerd door de TSI-query, naar JSON voor visualisatie:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

1. Maak een grafiekbesturingselement met de API's van `TsiClient.UX` en bind het element aan een van de `<div>`-elementen op de pagina:

   ```javascript
   var barChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

1. Vul het grafiekbesturingselement met de getransformeerde JSON-gegevensobjecten en geef het besturingselement weer op de pagina:

   ```javascript
   barChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Besturingselementen weergeven

De TSI-clientbibliotheek biedt acht unieke, out-of-the-box, analytics besturingselementen:

* **Lijndiagram**
* **Cirkeldiagram**
* **Staafdiagram**
* **heatmap**
* **hiërarchie van besturingselementen**
* **toegankelijk raster**
* **discrete gebeurtenis tijdlijnen**
* **status overgang tijdlijnen**

### <a name="line-bar-pie-chart-examples"></a>Voorbeelden van lijn-, staaf- en cirkeldiagram

Bekijk de demo-code die wordt gebruikt om enkele van de standard-grafiekbesturingselement weer te geven. U ziet het programmeermodel en patronen voor het maken van deze besturingselementen. Specifiek, onderzoekt de sectie van HTML onder de `// Example 3/4/5` opmerking die besturingselementen met de HTML-code weergegeven `id` waarden `chart3`, `chart4`, en `chart5`.

Intrekken van **stap 3** van de [pagina van de bron- en structuur sectie](#page-source-and-structure) dat grafiekbesturingselementen zijn gerangschikt in rijen op de pagina die allemaal een beschrijvende titel. In dit voorbeeld wordt de drie grafieken worden ingevuld onder de titel `"Multiple Chart Types From the Same Data"` `<div>` element, en zijn gebonden aan de drie `<div>` elementen die onder de titel:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

In de volgende sectie met JavaScript-code worden patronen gebruikt die eerder zijn beschreven: maak TSI-samenvoegingsexpressies, voer ze uit om TSI-gegevens op te vragen en geef de drie grafieken weer. U ziet de drie typen die uit de naamruimte `tsiClient.ux` worden gebruikt: `LineChart`, `BarChart` en `PieChart`. Hiermee worden de respectieve grafieken gemaakt en weergegeven. Ook belangrijk om te weten is dat alle drie de grafieken dezelfde samengevoegde expressiegegevens kunnen gebruiken, namelijk `transformedResult`:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

De drie grafieken worden als volgt weergegeven:

[![Multiple Chart Types From The Same Data](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Geavanceerde functies

De TSI-clientbibliotheek heeft verschillende aanvullende functies die u gebruiken kunt voor het implementeren van visualisaties van zoekgegevens creatief.

### <a name="states-and-events"></a>Statussen en gebeurtenissen

Een geavanceerde functionaliteit is de mogelijkheid om toe te voegen statusovergangen en afzonderlijke gebeurtenissen aan grafieken. Deze functie is handig voor het markeren van incidenten, waarschuwingen en het maken van status-switches (in/uit-switches voor voorbeeld).

Bekijk de code in rond de `// Example 10` opmerking. De code wordt weergegeven onder de titel lijnbesturingselement `"Line Charts with Multiple Series Types"`, en bindt aan de `<div>` element met de HTML-code `id` waarde `chart10`.

1. Eerst een structuur met de naam `events4` is gedefinieerd voor het opslaan van de statuswijziging elementen om bij te houden. De structuur bevat:

   * Een tekenreekssleutel met de naam `Component States`.
   * Een matrix van waardeobjecten die de statussen voorstellen. Elk object omvat:
     * Een tekenreekssleutel die een ISO-tijdstempel in JavaScript bevat.
     * Een matrix die de kenmerken van de status bevat: een kleur en een beschrijving.

2. Vervolgens worden de `events5` structuur wordt gedefinieerd voor `Incidents`, dat bevat een matrix van de gebeurteniselementen om bij te houden. De matrixstructuur heeft dezelfde vorm als de structuur die is beschreven voor `events4`.

3. Ten slotte wordt het lijndiagram weergegeven, waarbij de twee structuren worden doorgeven met de parameters voor grafiekopties: `events:` en `states:`. Kijk ook naar de andere optieparameters, waarmee een `tooltip:`, `theme:` of `grid:` kan worden opgegeven.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

De ruitvormige markeringen/pop-upvensters, waarmee incidenten worden aangegeven, en de gekleurde balken/pop-upvensters langs de tijdschaal, geven op visuele wijze statuswijzigingen aan:

[![Line Charts with Multiple Series Types](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Pop-upcontextmenu's

<div id="contextMenu"></div>

Een andere geavanceerde functies is de mogelijkheid om te maken van aangepaste contextmenu (Klik met de rechtermuisknop pop-upmenu's). Aangepaste contextmenu's zijn handig voor het inschakelen van acties en logische vervolgstappen binnen het bereik van uw toepassing.

Code Bekijk rustig de `// Example 13/14/15` opmerking. Deze code geeft in eerste instantie een lijndiagram onder de titel `"Line Chart with Context Menu to Create Pie/Bar Chart"` en de grafiek is gebonden aan de `<div>` element met de HTML-code `id` waarde `chart13`.

Met behulp van contextmenu's biedt het lijndiagram de mogelijkheid om dynamisch een cirkel- en staafdiagram te maken, die worden gekoppeld aan `<div>`-elementen met de id's `chart14` en `chart15`. Daarnaast gebruiken de cirkel- en staafdiagrammen contextmenu's om hun eigen functies aan te bieden: respectievelijk de mogelijkheid om gegevens te kopiëren van het cirkeldiagram naar het staafdiagram, en het weergeven van de gegevens uit het staafdiagram in het consolevenster van de browser.

1. Eerst wordt er een reeks aangepaste acties gedefinieerd. Elke actie bevat een matrix met een of meer elementen. Elk element definieert één contextmenuopdracht:

   * `barChartActions`: Deze actie definieert het contextmenu voor het cirkeldiagram, met één element voor het definiëren van één menuopdracht:
     * `name`: De tekst die wordt gebruikt voor de menuopdracht: Parameters afdrukken naar console.
     * `action`: De actie die aan de menuopdracht is gekoppeld. De actie is altijd een anonieme functie die drie argumenten accepteert. Deze zijn gebaseerd op de samenvoegingsexpressie die is gebruikt voor het maken van de grafiek. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
       * `ae`: De matrix met de samenvoegingsexpressie.
       * `splitBy`: De `splitBy` waarde.
       * `timestamp`: Het tijdstempel.

   * `pieChartActions`: Deze actie definieert het contextmenu voor het staafdiagram, dat één element bevat voor het definiëren van één menuopdracht. De vorm en het schema zijn hetzelfde als van het vorige `barChartActions`-element, maar u ziet dat de functie `action` er heel anders uitziet: hiermee wordt het staafdiagram geïnstantieerd en weergegeven. Het argument `ae` wordt gebruikt om de matrix met de samenvoegingsexpressie op te geven die in runtime wordt doorgegeven wanneer de menuopdracht wordt geopend. Met de functie wordt ook de eigenschap `ae.contextMenu` ingesteld met het contextmenu `barChartActions`.
   * `contextMenuActions`: Deze actie definieert het contextmenu voor het lijndiagram, dat drie elementen bevat voor het definiëren van drie menuopdrachten. De vorm en het schema voor elk element zijn hetzelfde als voor de eerder beschreven elementen. Net als bij het `barChartActions`-element, worden met de eerste opdracht drie functieargumenten weggeschreven naar het consolevenster van de browser. Net als bij het `pieChartActions`-elementen, worden met de volgende twee items respectievelijk de cirkel- en staafdiagrammen geïnstantieerd en weergegeven. Met de volgende twee items worden ook hun eigenschappen `ae.contextMenu` ingesteld met respectievelijk de contextmenu's `pieChartActions` en `barChartActions`.

2. Daarna worden twee samenvoegingsexpressies naar de matrix met de samenvoegingsexpressie `aes` gepusht en wordt voor elk item de matrix `contextMenuActions` opgegeven. Deze expressies worden gebruikt met het besturingselement voor het lijndiagram.

3. Ten slotte wordt in eerste instantie alleen het lijndiagram weergegeven, van waaruit zowel het cirkel- als het staafdiagram tijdens runtime kan worden weergegeven.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

In de schermafbeelding ziet u de grafieken met hun respectieve pop-upcontextmenu's. De cirkel- en staafdiagrammen zijn dynamisch gemaakt met behulp van de opties in het contextmenu van het lijndiagram.

[![Lijndiagram met contextmenu voor maken van cirkel- of staafdiagram](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Kwasten

Kwasten of penselen worden gebruikt om een tijdbereik te bepalen voor acties zoals zoomen en verkennen.

De code die wordt gebruikt om het werken met kwasten te demonstreren, wordt weergegeven in het vorige voorbeeld (Lijndiagram met contextmenu voor maken van cirkel- of staafdiagram), waarin pop-upcontextmenu's worden beschreven.

1. Kwastacties lijken erg op een contextmenu, in zoverre dat hiermee een reeks aangepaste acties voor de kwast kan worden gedefinieerd. Elke actie bevat een matrix met een of meer elementen. Elk element definieert één contextmenuopdracht:
   * `name`: De tekst die wordt gebruikt voor de menuopdracht: Parameters afdrukken naar console.
   * `action`: De actie die is gekoppeld aan de menuopdracht, die altijd een anonieme functie is die twee argumenten accepteert. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
      * `fromTime`: De `from` tijdstempel van de kwast selectie.
      * `toTime`: De `to` tijdstempel van de kwast selectie.

2. Kwastacties worden toegevoegd als een eigenschap voor grafiekopties. Let op de eigenschap `brushContextMenuActions: brushActions` die wordt doorgegeven aan de aanroep `linechart.Render`.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

[![Lijndiagram met contextmenu pie/staafdiagram maken met penselen](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Aanmelden en de TSI-voorbeeldtoepassing en de bron ervan bekijken.
> * API's in de JavaScript-clientbibliotheek van TSI gebruiken.
> * JavaScript gebruiken om grafiekbesturingselementen te maken en vullen met TSI gegevens.

Zoals wordt weergegeven, wordt in de TSI-voorbeeldtoepassing maakt gebruik van een voorbeeldgegevensset. Ga naar het volgende artikel voor informatie over het maken van een eigen TSI-omgeving en gegevensset:

> [!div class="nextstepaction"]
> [Zelfstudie: Een Azure Time Series Insights-omgeving maken](tutorial-create-populate-tsi-environment.md)

Of de bronbestanden van TSI voorbeeld van toepassing op:

> [!div class="nextstepaction"]
> [Opslagplaats TSI voorbeeld-app](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)
