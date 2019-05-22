---
title: 'Zelfstudie: De JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken | Microsoft Docs'
description: Informatie over de JavaScript-clientbibliotheek en het bijbehorende programmeermodel van Azure Time Series Insights.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 7208c0d42cba0e7f04fc6876bf3ada9fa65a00d9
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991455"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Zelfstudie: De JavaScript-clientbibliotheek van Azure Time Series Insights ontdekken

De clientbibliotheek voor JavaScript D3 op basis van Azure Time Series Insights is ontwikkeld om te helpen web ontwikkelaars query en visualiseren van gegevens die zijn opgeslagen in Time Series Insights. In deze zelfstudie leidt u door de Time Series Insights-clientbibliotheek en programmeermodel met behulp van een gehoste voorbeeld-app.

De zelfstudie wordt uitgelegd hoe u werkt met de bibliotheek, toegang krijgen tot Time Series Insights-gegevens en hoe u besturingselementen kolomdiagram gebruiken om te genereren en visualiseren van gegevens. U leert ook hoe u om te experimenteren met verschillende soorten grafieken om gegevens te visualiseren. Aan het einde van de zelfstudie, kunt u zult de clientbibliotheek gebruiken om op te nemen van Time Series Insights-functies in uw eigen web-app.

Specifiek, vindt u meer over:

> [!div class="checklist"]
> * De Time Series Insights-voorbeeldtoepassing
> * De Time Series Insights JavaScript-clientbibliotheek
> * Hoe de voorbeeldtoepassing maakt gebruik van de bibliotheek om Time Series Insights-gegevens te visualiseren

> [!NOTE]
> * De zelfstudie wordt gebruikgemaakt van een gratis, gehoste [Time Series Insights web demo](https://insights.timeseries.azure.com/clientsample).
> * De bronbestanden van de Time Series Insights-voorbeeld-app vindt u in de [voorbeeldopslagplaats in GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Lees de [Time Series Insights-client-referentiedocumentatie](https://github.com/microsoft/tsiclient/blob/master/docs/API.md).

## <a name="video"></a>Video

In deze video stellen we de open-source Time Series Insights JavaScript SDK:
<br /><br />

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie wordt gebruikgemaakt van uw browser **hulpprogramma's voor ontwikkelaars** functie. Moderne webbrowsers ([Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [Firefox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/), enzovoort) worden meestal aangeboden aan toegang tot de **Inspector Webweergave** via de sneltoets F12 op het toetsenbord. Een andere manier toegang tot de weergave is met de rechtermuisknop op een webpagina en selecteer vervolgens **inspecteren Element**.

## <a name="time-series-insights-sample-application"></a>Time Series Insights-voorbeeldtoepassing

In deze zelfstudie gebruiken we een gratis, gehoste voorbeeldapp van Time Series Insights verkennen van de broncode achter de toepassing en de Time Series Insights JavaScript-clientbibliotheek verkennen. Met behulp van de voorbeeld-app, leert u hoe u werken met Time Series Insights in JavaScript en visualiseren van gegevens via de diagrammen en grafieken.

1. Ga naar de [Time Series Insights-voorbeeldtoepassing](https://insights.timeseries.azure.com/clientsample). Het volgende bericht aanmelden wordt weergegeven:

   [![Time Series Insights client voorbeeld aanmeldingsprompt](media/tutorial-explore-js-client-lib/tcs-sign-in.png)](media/tutorial-explore-js-client-lib/tcs-sign-in.png#lightbox)

1. Selecteer **aanmelden** invoeren of selecteren van uw referenties. Gebruik een enterprise-organisatie-account (Azure Active Directory) of een persoonlijk account (Microsoft-account).

   [![Time Series Insights-clientreferenties voorbeeld vragen](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png#lightbox)

1. Nadat u zich hebt aangemeld, wordt een pagina met grafieken gevuld met Time Series Insights-gegevens weergegeven. Uw gebruikersaccount en de optie **Afmelden** worden in de rechterbovenhoek weergegeven:

   [![Time Series Insights client voorbeeld hoofdpagina na het aanmelden](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png#lightbox)

### <a name="page-source-and-structure"></a>Paginabron en -structuur

Eerst gaan we bekijken de [HTML en JavaScript-broncode](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) van de webpagina van de weergegeven:

1. Open **Ontwikkelhulpprogramma's** in uw browser. Bekijk de HTML-elementen waaruit de huidige pagina bestaat, ook wel de HTML- of DOM-structuur genoemd.

1. Vouw de `<head>` en `<body>` elementen en houd rekening met de volgende secties:

   * Onder de `<head>` -element, vindt u de metagegevens van de pagina en afhankelijkheden waarmee de app wordt uitgevoerd:
     * Een `<script>` element dat wordt gebruikt om te verwijzen naar de Azure Active Directory Authentication Library (ADAL)-bestand *adal.min.js*. ADAL is een JavaScript-bibliotheek die zorgt voor OAuth 2.0-verificatie (aanmelding) en het verkrijgen van een token voor toegang tot API's.
     * Meerdere `<link>` -elementen voor opmaakmodellen (ook wel bekend als *CSS*), zoals *sampleStyles.css* en *tsiclient.css*. De opmaakmodellen beheren pagina visuele stijl details, zoals kleuren, lettertypen en afstand.
     * Een `<script>` element dat wordt gebruikt om te verwijzen naar de Time Series Insights JavaScript-clientbibliotheek *tsiclient.js*. De pagina maakt gebruik van de bibliotheek om aan te roepen van Time Series Insights service-API's en grafiekbesturingselementen op de pagina weergeven.

     >[!NOTE]
     > * De broncode voor de ADAL JavaScript-bibliotheek is beschikbaar in de [azure-activedirectory-bibliotheek-voor-js-opslagplaats](https://github.com/AzureAD/azure-activedirectory-library-for-js).
     > * De broncode voor de Time Series Insights JavaScript-clientbibliotheek is beschikbaar in de [tsiclient opslagplaats](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).

   * Onder de `<body>` -element, vindt u `<div>` elementen, die het definiëren van de indeling van de items op de pagina, en een andere `<script>` element:
     * De eerste `<div>` element Hiermee geeft u de **aanmelden** in het dialoogvenster (`id="loginModal"`).
     * Het tweede `<div>`-element fungeert als een bovenliggend item voor:
       * Een koptekst-`<div>`-element, dat wordt gebruikt voor statusberichten en aanmeldingsgegevens aan de bovenkant van de pagina (`class="header"`).
       * Een `<div>` -element voor de rest van de pagina instantie-elementen, met inbegrip van de grafieken (`class="chartsWrapper"`).
       * Een `<script>` sectie waarin het JavaScript dat wordt gebruikt voor het beheren van de pagina.

   [![Time Series Insights Client voorbeeld met hulpprogramma's voor ontwikkelaars](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

1. Vouw de `<div class="chartsWrapper">` -element en u vindt meer onderliggende `<div>` elementen. Deze elementen worden gebruikt voor het plaatsen van alle voorbeelden van grafiekbesturingselementen. Er zijn verschillende sets `<div>` elementen, één voor elke grafiek-voorbeeld:

   * Het eerste element (`class="rowOfCardsTitle"`) bevat een beschrijvende titel om samen te vatten wat de grafiek(en) laten zien. Bijvoorbeeld: `Static Line Charts With Full-Size Legends.`
   * De tweede (`class="rowOfCards"`)-element is een bovenliggende met aanvullende onderliggende `<div>` elementen die de werkelijke grafiekbesturingselementen in een rij te plaatsen.

   [![Div-berichttekstonderdelen](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png#lightbox)

1. Vouw de `<script type="text/javascript">` element dat direct onder de `<div class="chartsWrapper">` element. Het begin van de JavaScript-sectie op paginaniveau wordt gebruikt voor het afhandelen van alle van de pagina logica (verificatie, aanroepen van API's, het weergeven van de besturingselementen kolomdiagram en meer van de Time Series Insights-service):

   [![Hoofdtekst script](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png#lightbox)

## <a name="time-series-insights-javascript-client-library-concepts"></a>Time Series Insights JavaScript-client-bibliotheek concepten

De Time Series Insights-clientbibliotheek (*tsclient.js*) abstracties voor twee belangrijke JavaScript-functies biedt:

* **Wrapper methoden voor het aanroepen van de API's van tijd Series Insights Query**: REST API's die kunt u gegevens op te vragen voor Time Series Insights met behulp van statistische expressies. De methoden zijn ingedeeld in de naamruimte TsiClient.Server van de bibliotheek.

* **Methoden voor het maken en vullen van verschillende typen grafiekbesturingselementen**: Methoden om weer te geven van de Time Series Insights kunt u de gegevens in een webpagina samenvoegen. De methoden zijn ingedeeld in de naamruimte TsiClient.UX van de bibliotheek.

Ontwikkelaars kunnen via deze vereenvoudigen UI graph en grafiek-onderdelen die zijn uitgerust met Time Series Insights-gegevens eenvoudiger bouwen.

### <a name="authentication"></a>Verificatie

De [Time Series Insights-voorbeeldtoepassing](https://insights.timeseries.azure.com/clientsample) is een app met één pagina met ondersteuning voor verificatie van ADAL OAuth 2.0 gebruiker:

1. Wanneer u ADAL voor verificatie gebruikt, moet u de client-app geregistreerd in Azure Active Directory (Azure AD). In feite de app met één pagina is geregistreerd voor het gebruik van de [OAuth 2.0-impliciete stroom verlenen](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-implicit-grant-flow).
1. De toepassing moet opgeven dat een aantal van de registratie-eigenschappen tijdens runtime. De eigenschappen omvatten het client-GUID (`clientId`) en de omleidings-URI (`postLogoutRedirectUri`).
1. Later, de app verzoekt een *toegangstoken* van Azure AD. Het toegangstoken is uitgegeven voor een beperkte set machtigingen voor een bepaalde service of de API-id (https:\//api.timeseries.azure.com). De tokenmachtigingen worden namens de aangemelde gebruiker uitgegeven. De id voor de service of de API is een andere eigenschap die opgenomen in de Azure AD-registratie van de app.
1. Het toegangstoken ADAL stuurt naar de app, wordt doorgegeven als een *bearer-token* wanneer deze toegang heeft tot de API's van de Time Series Insights-service.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=3-7,34-37)]

> [!TIP]
> Zie voor meer informatie over ondersteunde Microsoft Azure AD-verificatiebibliotheken, de [naslagdocumentatie voor Azure Active Directory Authentication Library](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries#microsoft-supported-client-libraries).

### <a name="control-identification"></a>Identificatie van besturingselementen

In het opgegeven voorbeeld `<div>` elementen zijn gerangschikt in de bovenliggende `<body>` element voor een praktische indeling voor de grafiek dat render op de pagina bepaalt.

Elke `<div>` element Hiermee geeft u de eigenschappen voor de plaatsing en visuele kenmerken van besturingselementen kolomdiagram. HTML-element `id` eigenschappen dienen als unieke id's verbinding maken met specifieke eisen voor weergeven en bijwerken van de gegevens gevisualiseerd.

### <a name="aggregate-expressions"></a>Samenvoegingsexpressies

De Time Series Insights-clientbibliotheek API's statistische expressies gebruiken:

* Een statistische expressie biedt de mogelijkheid om te maken van een of meer *zoektermen*.

* De client API's zijn ontworpen om te bieden vergelijkbare functionaliteit als een andere demo-app (de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com/demo)), die maakt gebruik van bereik, zoeken naar `where` predikaten, metingen, en `splitBy` waarden.

* De meeste clientbibliotheek API's krijgen een matrix van statistische expressies die de service gebruikt voor het maken van een query op een Time Series Insights.

### <a name="call-pattern"></a>Aanroeppatroon

Invullen en rendering van besturingselementen kolomdiagram kunt u een algemeen patroon. U ziet het algemene patroon in de voorbeeld-app en u kan helpen bij het gebruik van de clientbibliotheek:

1. Declareer een `array` voor het opslaan van een of meer Time Series Insights statistische expressies:

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
   | `predicateObject` | De filterexpressie van gegevens |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | De naam van de eigenschap van de meting die wordt gebruikt | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | De aggregaties van de meting-eigenschap die u wilt | `['avg', 'min']` |
   | `searchSpan`      | De duur en het interval grootte van de statistische expressie | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | De tekenreekseigenschap die u splitsen wilt op (optioneel: kan niet null zijn) | `{property: 'Station', type: 'String'}` |
   | `color`         | De kleur van de objecten die u wilt renderen | `'pink'` |
   | `alias`           | Een beschrijvende naam voor de statistische expressie | `'Factory3Temperature'` |
   | `contextMenuActions` | Een matrix van acties die moeten worden gekoppeld aan de time series-objecten in een visualisatie (optioneel) | Zie voor meer informatie, [pop-contextmenu](#pop-up-context-menus). |

1. Een Time Series Insights-query aanroepen met behulp van de APIs TsiClient.Server om aan te vragen van de verzamelde gegevens:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates parameters**

   | Parameter | Description | Voorbeeld |
   | --------- | ----------- | ------- |
   | `token`     | Het toegangstoken voor de Time Series Insights-API |  `authContext.getTsiToken()`<br />Zie [Verificatie](#authentication) voor meer informatie. |
   | `envFQDN`   | De volledig gekwalificeerde domeinnaam (FQDN) voor de Time Series Insights-omgeving | Vanuit de Azure-portal. Bijvoorbeeld: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | Een matrix van Time Series Insights-query-expressies | Gebruik de `aes` variabele, zoals eerder beschreven: `aes.map(function(ae){return ae.toTsx()}`. |

1. Het gecomprimeerde resultaat dat voor visualisatie van de Time Series Insights-query wordt geretourneerd in JSON transformeren:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

1. Een grafiekbesturingselement maken met behulp van de APIs TsiClient.UX. Binden aan een van de `<div>` elementen op de pagina:

   ```javascript
   var barChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

1. Vullen van het grafiekbesturingselement met de JSON-objecten voor getransformeerde gegevens en het besturingselement op de pagina weer te geven:

   ```javascript
   barChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="render-controls"></a>Besturingselementen weergeven

De Time Series Insights-clientbibliotheek biedt acht unieke, out-of-the-box analytics besturingselementen:

* **Lijndiagram**
* **Cirkeldiagram**
* **Staafdiagram**
* **heatmap**
* **hiërarchie van besturingselementen**
* **toegankelijk raster**
* **discrete gebeurtenis tijdlijnen**
* **status overgang tijdlijnen**

### <a name="line-chart-bar-chart-and-pie-chart-examples"></a>Lijndiagram en staafdiagram cirkeldiagram-voorbeelden

Bekijk de demo-code die wordt gebruikt om enkele van de grafiekbesturingselementen standaard weer te geven. Houd er rekening mee de programmeermodel en patronen voor het maken van deze besturingselementen. Specifiek, onderzoekt de HTML-code onder de `// Example 3/4/5` opmerking die besturingselementen met de HTML-code weergegeven `id` waarden `chart3`, `chart4`, en `chart5`.

Intrekken van stap 3 van de [pagina van de bron- en structuur sectie](#page-source-and-structure) dat grafiekbesturingselementen zijn gerangschikt in rijen op de pagina. Elk grafiekbesturingselement heeft een rij beschrijvende titel. In dit voorbeeld wordt de drie grafieken worden ingevuld onder de `Multiple Chart Types From the Same Data` titel `<div>` -element en zijn afhankelijk van de drie `<div>` elementen die onder de titel:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

De volgende sectie van de JavaScript-code maakt gebruik van patronen die eerder zijn beschreven: Time Series Insights statistische expressies bouwen, gebruiken ze om te vragen voor Time Series Insights-gegevens en vervolgens de drie grafieken weergegeven. Drie grafiektypen worden gebruikt vanuit de naamruimte tsiClient.ux: `LineChart`, `BarChart`, en `PieChart`. De grafiektypen worden gebruikt voor het maken en de respectieve grafieken weergegeven. Alle drie grafieken kunt dezelfde statistische expressiegegevens `transformedResult`:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

De drie grafieken worden als volgt weergegeven:

[![Multiple Chart Types From The Same Data](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Geavanceerde functies

De Time Series Insights-clientbibliotheek bevat verschillende aanvullende functies die u gebruiken kunt voor het implementeren van visualisaties van zoekgegevens creatief.

### <a name="states-and-events"></a>Statussen en gebeurtenissen

Een geavanceerde functies is de mogelijkheid om toe te voegen statusovergangen en afzonderlijke gebeurtenissen aan grafieken. Deze functie is handig voor het markeren van incidenten, waarschuwingen en het maken van status-switches (in/uit-switches, bijvoorbeeld).

Bekijk de code in rond de `// Example 10` opmerking. De code wordt weergegeven onder de titel lijnbesturingselement `Line Charts with Multiple Series Types` en bindt aan de `<div>` element met de HTML-code `id` waarde `chart10`.

De volgende stappen beschrijven het proces:

1. Een structuur met de naam `events4` is gedefinieerd voor het opslaan van de statuswijziging elementen om bij te houden. De structuur bevat:

   * Een tekenreekssleutel met de naam `Component States`.
   * Een matrix van waardeobjecten die de statussen voorstellen. Elk object omvat:
     * Een tekenreekssleutel die een ISO-tijdstempel in JavaScript bevat.
     * Een matrix die de kenmerken van de status bevat: een kleur en een beschrijving.

1. De `events5` structuur wordt gedefinieerd voor `Incidents`, dat bevat een matrix van de gebeurteniselementen om bij te houden. De matrixstructuur heeft dezelfde vorm als de structuur die is beschreven voor `events4`.

1. Het lijndiagram wordt weergegeven en wordt doorgegeven in de twee structuren met de grafiekopties parameters: `events:` en `states:`. Houd er rekening mee de andere optie-parameters voor het opgeven van een `tooltip:`, `theme:`, of `grid:`.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

De reeks met een ruit markeringen/pop-van windows die worden gebruikt om aan te geven van incidenten en de gekleurde balken/pop-van windows langs de tijdschaal geven aan de statuswijzigingen:

[![Lijndiagrammen met meerdere reeksen](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Pop-upcontextmenu's

Een andere geavanceerde functies is de mogelijkheid om te maken van aangepaste contextmenu (Klik met de rechtermuisknop pop-upmenu's). Aangepaste contextmenu's zijn handig voor het inschakelen van acties en logische vervolgstappen binnen het bereik van uw toepassing.

Bekijk de code in rond de `// Example 13/14/15` opmerking. Deze code geeft in eerste instantie een lijndiagram onder de titel `Line Chart with Context Menu to Create Pie/Bar Chart`. De grafiek is gebonden aan de `<div>` element met de HTML-code `id` waarde `chart13`.

Met behulp van contextmenu's biedt het lijndiagram de mogelijkheid om dynamisch een cirkel- en staafdiagram te maken, die worden gekoppeld aan `<div>`-elementen met de id's `chart14` en `chart15`. De cirkeldiagram in zowel het staafdiagram ook contextmenu's gebruiken om in te schakelen van hun eigen functies: de mogelijkheid om gegevens te kopiëren van de cirkeldiagram in het staafdiagram en om respectievelijk de staafdiagramgegevens naar het browservenster console af te drukken.

De volgende stappen beschrijven het proces:

1. Een reeks van aangepaste acties worden gedefinieerd. Elke actie bevat een matrix met een of meer elementen. Elk element definieert één contextmenuopdracht:

   * `barChartActions`: Deze actie definieert het contextmenu voor het cirkeldiagram, met één element voor het definiëren van één menuopdracht:
     * `name`: De tekst die wordt gebruikt voor de menuopdracht: Parameters afdrukken naar console.
     * `action`: De actie die aan de menuopdracht is gekoppeld. De actie is altijd een anonieme functie die drie argumenten accepteert. Deze zijn gebaseerd op de samenvoegingsexpressie die is gebruikt voor het maken van de grafiek. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
       * `ae`: De matrix met de samenvoegingsexpressie.
       * `splitBy`: De `splitBy` waarde.
       * `timestamp`: Het tijdstempel.

   * `pieChartActions`: Deze actie definieert het contextmenu voor het staafdiagram, dat één element bevat voor het definiëren van één menuopdracht. De vorm en het schema is hetzelfde als de `barChartActions` element eerder beschreven, maar de `action` functie aanzienlijk verschilt: het instantieert en het staafdiagram wordt weergegeven. De `ae` argument wordt gebruikt om op te geven van de matrix statistische expressie die wordt doorgegeven tijdens runtime als in het menu-item wordt geopend. Met de functie wordt ook de eigenschap `ae.contextMenu` ingesteld met het contextmenu `barChartActions`.
   * `contextMenuActions`: Deze actie definieert het contextmenu voor het lijndiagram, dat drie elementen bevat voor het definiëren van drie menuopdrachten. De vorm en het schema voor elk element is hetzelfde als de elementen die eerder zijn beschreven. Net als bij het `barChartActions`-element, worden met de eerste opdracht drie functieargumenten weggeschreven naar het consolevenster van de browser. Vergelijkbaar met de `pieChartActions` -element de volgende twee items instantiëren en respectievelijk de cirkeldiagram en staafdiagram weer te geven. Met de volgende twee items worden ook hun eigenschappen `ae.contextMenu` ingesteld met respectievelijk de contextmenu's `pieChartActions` en `barChartActions`.

1. Twee statistische expressies worden gepusht naar de `aes` statistische expressie matrix. Ze geven de `contextMenuActions` matrix voor elk item. Deze expressies worden gebruikt met het besturingselement voor het lijndiagram.

1. Alleen het lijndiagram wordt in eerste instantie weergegeven, waarin de cirkeldiagram in zowel het staafdiagram tijdens runtime kunnen worden gerenderd.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

De volgende schermafbeelding ziet u de grafieken met hun respectieve pop-contextmenu's. De cirkeldiagram en staafdiagram zijn dynamisch gemaakt met behulp van de regel grafiek context menu-opties.

[![Lijndiagram met contextmenu cirkeldiagram en staafdiagram maken](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Kwasten

U kunt penselen gebruiken als bereik voor een tijdsbereik voor het definiëren van acties zoals zoomen en verkennen.

De code die wordt gebruikt om te illustreren penselen wordt weergegeven in de `Line Chart with Context Menu to Create Pie/Bar Chart` voorbeeld waarin wordt beschreven pop-contextmenu's.

* Kwastacties lijken erg op een contextmenu, in zoverre dat hiermee een reeks aangepaste acties voor de kwast kan worden gedefinieerd. Elke actie bevat een matrix met een of meer elementen. Elk element definieert één contextmenuopdracht:
   * `name`: De tekst die wordt gebruikt voor de menuopdracht: Parameters afdrukken naar console.
   * `action`: De actie die is gekoppeld aan de menuopdracht, die altijd een anonieme functie is die twee argumenten accepteert. In dit geval worden de argumenten weggeschreven naar het consolevenster van de browser:
     * `fromTime`: De `from` tijdstempel van de kwast selectie.
     * `toTime`: De `to` tijdstempel van de kwast selectie.

* Kwastacties worden toegevoegd als een eigenschap voor grafiekopties. De `brushContextMenuActions: brushActions` eigenschap wordt doorgegeven aan de `linechart.Render` aanroepen.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

[![Lijndiagram met contextmenu cirkeldiagram en staafdiagram maken met behulp van penselen](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png#lightbox)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Meld u aan en bekijk de Time Series Insights-voorbeeldtoepassing en de bron
> * API's in de Time Series Insights JavaScript-clientbibliotheek gebruiken
> * JavaScript gebruiken om te maken en vullen grafiekbesturingselementen met Time Series Insights-gegevens

De voorbeeldtoepassing Time Series Insights maakt gebruik van een demo-gegevensset. Als u wilt weten hoe u uw eigen Time Series Insights-omgeving en de gegevensset kunt maken, lees het volgende artikel:

> [!div class="nextstepaction"]
> [Zelfstudie: Een Azure Time Series Insights-omgeving maken](tutorial-create-populate-tsi-environment.md)

Of weergeven van de bronbestanden van de Time Series Insights-voorbeeld-toepassing:

> [!div class="nextstepaction"]
> [Opslagplaats voor Time Series Insights-voorbeeld-app](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)

Lees de Time Series Insights-API-referentiedocumentatie voor client:

> [!div class="nextstepaction"]
> [Time Series Insights-API-referentiedocumentatie](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)
