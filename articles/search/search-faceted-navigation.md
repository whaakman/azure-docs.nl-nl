---
title: Facetnavigatie implementeren in een categoriehiërarchie - Azure Search
description: Facet navigatie toevoegen naar toepassingen die kunnen worden geïntegreerd met Azure Search, een search-service van de cloud worden gehost op Microsoft Azure.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: b5c7050ac006ea2500854f8f41b134895e5e0061
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541210"
---
# <a name="how-to-implement-faceted-navigation-in-azure-search"></a>Facetnavigatie implementeren in Azure Search
Facetnavigatie is een filteren mechanisme waarmee Self-directed drilldown navigatie in zoektoepassingen. De term 'meervoudige navigatie' kan niet bekend zijn, maar u hebt waarschijnlijk eerder gebruikt. Zoals in het volgende voorbeeld wordt weergegeven, is meervoudige navigatie niets meer dan de categorieën die worden gebruikt om resultaten te filteren.

 ![Azure Search taak Portal-Demo][1]

Facetnavigatie is een alternatieve toegangspunt om te zoeken. Het biedt een alternatief voor complexe zoekquery expressies handmatig te typen. Facetten kunt u vinden wat u zoekt, terwijl u ervoor te zorgen dat er geen nul resultaten. Als ontwikkelaar kunnen facetten u de handigste zoekcriteria voor het navigeren in uw zoekverzameling beschikbaar. In de detailhandel online toepassingen, is vaak facetnavigatie gebouwd merken, afdelingen (kinderen schoenen), grootte, prijs, populariteit en classificaties. 

Facetnavigatie implementeren verschilt via zoektechnologieën. In Azure Search is meervoudige navigatie op het moment dat de query, gebouwd met behulp van de velden die u eerder de kenmerken in uw schema.

-   In de query's die uw toepassing wordt gemaakt, een query moet verzenden *facet queryparameters* om op te halen van de beschikbare facet filterwaarden voor de resultatenset van dat document.

-   Daadwerkelijk waaruit het resultaat van het document ingesteld, wordt de toepassing moet worden toegepast een `$filter` expressie.

In de ontwikkeling van toepassingen vormt schrijven van code die wordt gemaakt van query's het grootste deel van het werk. Veel van het gedrag van toepassingen die u van meervoudige navigatie verwacht worden geleverd door de service, inclusief ingebouwde ondersteuning voor het definiëren van de bereiken en ophalen van de aantallen voor facet resultaten. De service omvat ook praktische standaardinstellingen die u helpen te voorkomen dat onhandig navigatiestructuur. 

## <a name="sample-code-and-demo"></a>Voorbeeldcode en demo
In dit artikel wordt een taak search-portal als voorbeeld gebruikt. Het voorbeeld wordt geïmplementeerd als een ASP.NET MVC-toepassing.

-   Bekijken en testen van de demo werkt online op [Azure Search taak Portal-Demo](http://azjobsdemo.azurewebsites.net/).

-   Download de code uit de [opslagplaats voor Azure-voorbeelden op GitHub](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

## <a name="get-started"></a>Aan de slag
Als u geen ervaring met ontwikkeling zoeken, is de beste manier om na te denken van meervoudige navigatie die deze de mogelijkheden voor self-directed search bevat. Er is een type inzoomen zoekervaring, op basis van vooraf gedefinieerde filters, die wordt gebruikt voor het technologiegebied snel zoekresultaten via aanwijzen en klikken acties. 

### <a name="interaction-model"></a>Interactie van model

De zoekervaring voor meervoudige navigatie is iteratieve, laten we beginnen met informatie over het als een reeks query's die in reactie op gebruikersacties uitvouwen.

Het beginpunt is een toepassingspagina waarmee facetnavigatie, meestal op de omtrek geplaatst. Facetnavigatie is vaak een boomstructuur met selectievakjes in voor elke waarde of geklikt tekst. 

1. Een query verzonden naar Azure Search Hiermee geeft u de structuur meervoudige navigatie via een of meer facet queryparameters. Bijvoorbeeld, de query kan bevatten `facet=Rating`, bijvoorbeeld met een `:values` of `:sort` optie om de presentatie verder te verfijnen.
2. Een pagina voor zoeken waarmee facetnavigatie, met behulp van de facetten die zijn opgegeven in de aanvraag wordt weergegeven in de presentatielaag.
3. Uitgaande van een facetnavigatiestructuur met classificatie, u klikken op "4" om aan te geven dat alleen producten met een classificatie van 4 of hoger moeten worden weergegeven. 
4. Als antwoord verzendt de toepassing een query uitvoert met `$filter=Rating ge 4` 
5. De presentatielaag de pagina, met een lagere resultatenset met alleen de items die voldoen aan de criteria van de nieuwe updates (in dit geval producten geclassificeerd 4 en hoger).

Een facet is een queryparameter, maar niet verwarren met invoer voor de query. Deze wordt nooit gebruikt als selectiecriteria in een query. In plaats daarvan zien van de queryparameters facet als invoer in de navigatiestructuur die in het antwoord terugkomen. Voor elke facet queryparameter die u opgeeft, met Azure Search evalueert het aantal documenten worden in het gedeeltelijke resultaten voor elke facetwaarde.

U ziet dat de `$filter` in stap 4. Het filter is een belangrijk aspect van meervoudige navigatie. Hoewel facetten en filters onafhankelijk van elkaar in de API zijn, moet u beide om de ervaring die u van plan bent te leveren. 

### <a name="app-design-pattern"></a>Ontwerppatroon voor een App

In de code van de toepassing is het patroon facet queryparameters gebruiken om te retourneren van de structuur facetnavigatie samen met facet resultaten, plus een $filter-expressie.  De filterexpressie verwerkt de gebeurtenis click van de facetwaarde. Met de `$filter` expressie als de code achter de werkelijke trimmen van zoekresultaten geretourneerd naar de presentatielaag. Een facet kleuren worden gegeven, te klikken op de kleur rood wordt geïmplementeerd via een `$filter` expressie die u selecteert alleen de items die u een kleur rood hebt. 

### <a name="query-basics"></a>Query-basisbeginselen

In Azure Search, een aanvraag via een of meer queryparameters is opgegeven (Zie [documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) voor een beschrijving van elk element). Geen van de queryparameters zijn vereist, maar u moet ten minste één in volgorde van een query om geldig te zijn.

Precisie, begrepen zoals de mogelijkheid om te filteren irrelevante treffers wordt bereikt door één of beide van deze expressies:

-   **Search =**  
    De waarde van deze parameter om te achterhalen van de zoekopdracht. Het is mogelijk een los stukje van tekst of een complexe zoekquery-expressie met meerdere voorwaarden en operators. Op de server, wordt een zoekopdracht voor zoeken in volledige tekst, uitvoeren van query's doorzoekbare velden in de index voor het afstemmen van de voorwaarden, retourneren van resultaten in de volgorde van de positie gebruikt. Als u instelt `search` op null, query wordt uitgevoerd gedurende de gehele index (dat wil zeggen, `search=*`). In dit geval andere elementen van de query, zoals een `$filter` of scoringprofiel, worden de primaire factoren die invloed hebben op welke documenten worden geretourneerd `($filter`) en in welke volgorde (`scoringProfile` of `$orderby`).

-   **$filter=**  
    Een filter is een krachtig mechanisme voor het beperken van de grootte van search-resultaten op basis van de waarden van specifieke documentkenmerken. Een `$filter` wordt eerst geëvalueerd, gevolgd door onderverdeling logica die de beschikbare waarden en de bijbehorende aantallen voor elke waarde genereert

Expressies voor complexe zoekquery verminderen de prestaties van de query. Waar mogelijk, gebruikt u goed Filterexpressies voor betere nauwkeurigheid en queryprestaties verbeteren.

Vergelijk om beter te begrijpen hoe een filter toegevoegd voor meer precisie, een complexe zoekquery-expressie die een filterexpressie bevat:

-   `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`
-   `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Beide query's zijn geldig, maar de tweede is superieure als u niet motels met vervangende domeinpagina in Seattle zoekt.
-   De eerste query is afhankelijk van de specifieke woorden worden vermeld of niet wordt vermeld in de tekenreeksvelden, zoals naam, beschrijving en andere velden die doorzoekbare gegevens bevat.
-   De tweede query zoekt naar exacte overeenkomsten op gestructureerde gegevens en is waarschijnlijk nog veel meer nauwkeurige.

Zorg ervoor dat elke actie van de gebruiker via een facetnavigatiestructuur is voorzien van een verkleinen van zoekresultaten in toepassingen die meervoudige navigatie. Als u wilt beperken resultaten, een filterexpressie te gebruiken.

<a name="howtobuildit"></a>

## <a name="build-a-faceted-navigation-app"></a>Een meervoudige navigatie-app bouwen
U implementeren meervoudige navigatie met Azure Search in uw toepassingscode die de zoekaanvraag. De meervoudige navigatie, is afhankelijk van elementen in uw schema dat u eerder hebt gedefinieerd.

Vooraf gedefinieerde in uw zoekopdracht index is de `Facetable [true|false]` indexkenmerk, in de geselecteerde velden inschakelen of uitschakelen van hun gebruik in een facetnavigatiestructuur instellen. Zonder `"Facetable" = true`, een veld kan niet worden gebruikt in de facet navigatie.

De presentatielaag in uw code biedt de gebruikerservaring. Het moet delen van de meervoudige navigatie, zoals het label, waarden, selectievakjes en het aantal weergeven. De Azure Search REST-API is een platform-agnostische, Gebruik daarom ongeacht taal en platform van uw keuze. Het belangrijkste is om op te nemen van UI-elementen die ondersteuning bieden voor incrementeel vernieuwen, met de bijgewerkte status van de gebruikersinterface als elke extra facet is geselecteerd. 

Op het moment dat de query, maakt de code van uw toepassing een aanvraag met `facet=[string]`, een aanvraagparameter waarmee het veld voor facet door. Een query kunt hebben meerdere facetten, zoals `&facet=color&facet=category&facet=rating`, elkaar gescheiden door een en-teken (&)-teken.

Toepassingscode moet ook opgeven, een `$filter` expressie voor het afhandelen van de klikgebeurtenissen in meervoudige navigatie. Een `$filter` vermindert de zoekresultaten, met behulp van de facetwaarde als filtercriteria.

Azure Search retourneert de lijst met zoekresultaten op basis van een of meer voorwaarden die u, samen met updates in de structuur meervoudige navigatie invoert. In Azure Search meervoudige navigatie is een constructie één niveau met facet waarden, statistieken en van hoeveel resultaten gevonden voor elk criterium.

In de volgende secties nemen we eens nader bekijken over het bouwen van elk onderdeel.

<a name="buildindex"></a>

## <a name="build-the-index"></a>Bouw de index
Onderverdeling is ingeschakeld op basis van veld in de index, via deze indexkenmerk: `"Facetable": true`.  
Alle veldtypen die mogelijk kunnen worden gebruikt in facetnavigatie `Facetable` standaard. Dergelijke veldtypen bevatten `Edm.String`, `Edm.DateTimeOffset`, en alle numeriek veldtypen (in principe alle veldtypen zijn geschikt voor facetten behalve `Edm.GeographyPoint`, die niet kan worden gebruikt in facetnavigatie). 

Bij het bouwen van een index, is een best practice voor meervoudige navigatie expliciet onderverdeling om uit te schakelen voor velden die moeten nooit worden gebruikt als een facet.  In het bijzonder tekenreeksvelden voor singleton-waarden, zoals een naam-ID of product moeten worden ingesteld op `"Facetable": false` om te voorkomen dat onbedoelde (en inefficiënte) gebruik in meervoudige navigatie. Schakelen onderverdeling uit waar u deze niet nodig helpt bij het beveiligen van de grootte van de index klein en meestal verbetert de prestaties.

Volgende maakt deel uit van het schema voor de taak Portal-Demo voorbeeld-app, ontdaan van bepaalde kenmerken om de grootte te beperken:

```json
{
  ...
  "name": "nycjobs",
  "fields": [
    { “name”: "id",                 "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "job_id",             "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "agency",              "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "posting_type",        "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "num_of_positions",    "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "business_title",      "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "civil_service_title", "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "title_code_no",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "level",               "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_from",   "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_to",     "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_frequency",    "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "work_location",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
…
    { “name”: "geo_location",        "type": "Edm.GeographyPoint",     "searchable": false, "filterable": true, ...  "facetable": false, ... },
    { “name”: "tags",                "type": "Collection(Edm.String)", "searchable": true,  "filterable": true, ...  "facetable": true, ...  }
  ],
…
}
```

Zoals u in het voorbeeldschema ziet `Facetable` is uitgeschakeld voor tekenreeksvelden die al dan niet mogen worden gebruikt als facetten, zoals id-waarden. Schakelen onderverdeling uit waar u deze niet nodig helpt bij het beveiligen van de grootte van de index klein en meestal verbetert de prestaties.

> [!TIP]
> Als een best practice, bevatten de volledige set indexkenmerken in voor elk veld. Hoewel `Facetable` is standaard ingeschakeld voor bijna alle velden, instellen van opzettelijk elk kenmerk kunt u de implicaties bespreken van elk besluit schema. 

<a name="checkdata"></a>

## <a name="check-the-data"></a>Controleer de gegevens
De kwaliteit van uw gegevens heeft een directe invloed op of de structuur facetnavigatie gebeurtenis zich voordoet als u verwacht het dat. Het is ook van invloed op het gemak van het maken van filters reduceren van de resultatenset.

Als u facet door merk of prijs wilt, elk document moet bevatten waarden voor *BrandName* en *ProductPrice* die geldig, consistente en een filteroptie productief zijn.

Hier volgen enkele herinneringen van wat u wilt verwijderen voor:

* Voor elk veld dat u wilt facet door, u zich afvragen of deze bevat waarden die geschikt is als filters in self-directed search. De waarden moeten worden korte, beschrijvende en voldoende onderscheidend om te wissen keuze tussen concurrerende opties bieden.
* Spelfouten of bijna overeenkomende waarden. Als u facet op kleuren en waarden in het veld zijn oranje en Ornage (een spelfout), een facet op basis van het veld kleur beide wilt ophalen.
* Tekst met gemengde case kunt functies in facetnavigatie met orange en oranje wordt weergegeven als twee verschillende waarden ook aanrichten. 
* Enkelvoudige en meervoudige versies van dezelfde waarde kunnen resulteren in een afzonderlijke facet voor elk.

Zoals u zien kunt, is de zorgvuldigheid bij het voorbereiden van de gegevens een essentieel aspect van een doeltreffende meervoudige navigatie.

<a name="presentationlayer"></a>

## <a name="build-the-ui"></a>De gebruikersinterface bouwen
Werken van de presentatielaag kunt u vereisten die anders mogelijk worden gemist en te begrijpen welke mogelijkheden essentieel voor de zoekfunctie zijn.

Invoer van de gebruiker op de pagina detecteert en voegt de gewijzigde onderdelen in termen van meervoudige navigatie pagina van uw web- of toepassing geeft de structuur meervoudige navigatie. 

Voor webtoepassingen, wordt AJAX doorgaans gebruikt in de presentatielaag omdat Hiermee kunt u incrementele wijzigingen vernieuwen. U kunt ook ASP.NET MVC- of een andere visualisatie-platform die verbinding met een Azure Search-service via HTTP maken kan. De voorbeeldtoepassing waarnaar wordt verwezen in dit artikel--de **Azure Search taak Portal-Demo** – gebeurt er met een ASP.NET MVC-toepassing.

Het voorbeeld meervoudige navigatie is gratis ingebouwd in de pagina met zoekresultaten. Het volgende voorbeeld wordt opgehaald uit de `index.cshtml` -bestand van de voorbeeldtoepassing, toont de statische HTML-structuur voor het weergeven van meervoudige navigatie op de zoekpagina resultaten pagina. De lijst met facetten is opgebouwd of dynamisch opnieuw opbouwen wanneer u een zoekterm of schakelt u uit een facet.

```html
<div class="widget sidebar-widget jobs-filter-widget">
  <h5 class="widget-title">Filter Results</h5>
    <p id="filterReset"></p>
    <div class="widget-content">

      <h6 id="businessTitleFacetTitle">Business Title</h6>
      <ul class="filter-list" id="business_title_facets">
      </ul>

      <h6>Location</h6>
      <ul class="filter-list" id="posting_type_facets">
      </ul>

      <h6>Posting Type</h6>
      <ul class="filter-list" id="posting_type_facets"></ul>

      <h6>Minimum Salary</h6>
      <ul class="filter-list" id="salary_range_facets">
      </ul>

  </div>
</div>
```

Het volgende codefragment uit de `index.cshtml` pagina bouwt dynamisch de HTML-code om het eerste facet, Business titel weer te geven. Soortgelijke functies bouwen dynamisch de HTML-code voor de andere facetten. Elke facet heeft een label en een telling, waarin het aantal items gevonden voor dat resultaat facet worden weergegeven.

```js
function UpdateBusinessTitleFacets(data) {
  var facetResultsHTML = '';
  for (var i = 0; i < data.length; i++) {
    facetResultsHTML += '<li><a href="javascript:void(0)" onclick="ChooseBusinessTitleFacet(\'' + data[i].Value + '\');">' + data[i].Value + ' (' + data[i].Count + ')</span></a></li>';
  }

  $("#business_title_facets").html(facetResultsHTML);
}
```

> [!TIP]
> Wanneer u de pagina met zoekresultaten ontwerpt, moet u een mechanisme voor het wissen van facetten toevoegen. Als u de selectievakjes toevoegt, kunt u eenvoudig zien hoe de filters wissen. Voor andere indelingen moet u mogelijk een breadcrumb-patroon of een andere creative benadering. Bijvoorbeeld, in de Portal voor zoekopdrachten in taak voorbeeldtoepassing, kunt u de `[X]` na een geselecteerde facet het facet wissen.

<a name="buildquery"></a>

## <a name="build-the-query"></a>Bouw de query
De code die u voor het bouwen van query's schrijven moet alle onderdelen van een geldige query, met inbegrip van uitdrukkingen zoeken, facetten, filters, score profielen – iets gebruikt voor het formuleren van een aanvraag opgeven. In deze sectie wordt toegelicht waar facetten past in een query, en hoe filters met facetten worden gebruikt voor het leveren van een lagere resultatenset.

U ziet dat facetten integraal in deze voorbeeldtoepassing. De zoekervaring in de taak Portal-Demo is ontworpen rond meervoudige navigatie en filters. De opvallende plaatsing van meervoudige navigatie op de pagina ziet u het belang. 

Een voorbeeld is vaak een goede plaats om te beginnen. Het volgende voorbeeld wordt opgehaald uit de `JobsSearch.cs` -bestand, een aanvraag die wordt gemaakt van facet navigatie op basis van functie, locatie, Type plaatsen en minimale salaris builds. 

```cs
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "business_title", "posting_type", "level", "salary_range_from,interval:50000" },
};
```

Een queryparameter facet is ingesteld op een veld en afhankelijk van het gegevenstype, kan worden verder als parameters gebruikt met door komma's gescheiden lijst met `count:<integer>`, `sort:<>`, `interval:<integer>`, en `values:<list>`. Een lijst met waarden wordt ondersteund voor numerieke gegevens bij het instellen van bereiken. Zie [documenten zoeken (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) voor informatie over het gebruik.

Samen met de facetten, moet de aanvraag die door uw toepassing ook filters om de set candidate documenten op basis van de selectie van een facet waarde vast te maken. Facetnavigatie biedt voor een fiets-store, of er aanwijzingen op vragen zoals *welke kleuren, fabrikanten en typen fietsen beschikbaar zijn?*. Filteren van antwoorden op vragen zoals *mountainbikes, welke exacte fietsen zijn rood, in dit bereik te prijs?*. Wanneer u klikt op "Rood" om aan te geven dat alleen de rode producten moeten worden weergegeven, de volgende query die de toepassing verzendt bevat `$filter=Color eq ‘Red’`.

Het volgende codefragment uit de `JobsSearch.cs` pagina wordt de geselecteerde functie aan het filter toegevoegd als u een waarde in het facet Business titel selecteert.

```cs
if (businessTitleFacet != "")
  filter = "business_title eq '" + businessTitleFacet + "'";
```

<a name="tips"></a> 

## <a name="tips-and-best-practices"></a>Tips en best practices

### <a name="indexing-tips"></a>Tips voor indexeren
**De index-efficiëntie verbeteren als u een zoekvak niet gebruikt**

Als uw toepassing gebruikmaakt van meervoudige navigatie uitsluitend (dat wil zeggen, er zijn geen zoekvak), kunt u het veld als markeren `searchable=false`, `facetable=true` voor het produceren van een compactere index. Bovendien treedt indexeren alleen op hele facet waarden, geen woordafbreking of van de onderdelen van een waarde met meerdere woorden te indexeren.

**Opgeven welke velden kunnen worden gebruikt als facetten**

Let erop dat het schema van de index welke velden beschikbaar zijn bepaalt voor gebruik als een facet. Ervan uitgaande dat een veld is geschikt voor facetten, de query geeft aan welke velden moeten worden facet door. Het veld waarop u op meerdere niveaus bent bevat de waarden die worden weergegeven onder het label. 

De waarden die worden weergegeven onder elk label worden opgehaald uit de index. Bijvoorbeeld, als het veld facet is *kleur*, de waarden die beschikbaar zijn voor het filteren van aanvullende zijn de waarden voor dat veld - rood, zwart, enzovoort.

Voor numerieke en datum/tijd-waarden alleen, kunt u waarden expliciet instellen op het veld facet (bijvoorbeeld `facet=Rating,values:1|2|3|4|5`). Een lijst met waarden is toegestaan voor deze typen voor het vereenvoudigen van de scheiding tussen facet resultaten in aaneengesloten bereiken (beide bereiken op basis van numerieke waarden of perioden). 

**Standaard kunt u slechts één niveau van meervoudige navigatie hebben** 

Zoals is vermeld, is er geen directe ondersteuning voor het nesten van facetten in een hiërarchie. Standaard ondersteunt facetnavigatie in Azure Search slechts één niveau van filters. Echter, tijdelijke oplossingen bestaan. U kunt coderen een facet hiërarchische structuur in een `Collection(Edm.String)` verwijzen met één vermelding per hiërarchie. Deze oplossing implementeren valt buiten het bereik van dit artikel. 

### <a name="querying-tips"></a>Een query uitvoeren op tips
**Velden valideren**

Als u de lijst met facetten dynamisch op basis van de invoer van de niet-vertrouwde gebruiker maakt, controleert u dat de namen van de basis van meervoudig velden geldig zijn. Of, als u de namen bij het bouwen van URL's met behulp van `Uri.EscapeDataString()` in .NET of het equivalent in de gewenste platform.

### <a name="filtering-tips"></a>Tips voor filteren
**De precisie van de zoekopdracht verhogen met filters**

Filters gebruiken. Als u zich op net uitdrukkingen zoeken alleen baseert, als gevolg kan leiden tot een document moet worden geretourneerd die niet de exacte facetwaarde hebben in de velden.

**Verbeteren van zoeken met filters**

Filters de set documenten kandidaat voor zoekopdracht verfijnen en rangschikking uitsluiten. Hebt u een groot aantal documenten, het gebruik van een selectief facet inzoomen vaak biedt u betere prestaties.
  
**Alleen de velden van de basis van meervoudig filteren**

In meervoudige Inzoomen wilt u meestal alleen documenten die de facetwaarde in een bepaald (beperkt) veld niet overal in alle doorzoekbare velden hebt bevatten. Toevoegen van een filter versterkt het doelveld door te leiden van de service om te zoeken naar alleen in de basis van meervoudig veld voor een overeenkomende waarde.

**Trim facet resultaten met meer filters**

Facet resultaten zijn gevonden in de lijst met zoekresultaten die overeenkomen met een termijn van facet documenten. In het volgende voorbeeld, in de zoekresultaten voor *cloudcomputing*, 254 items hebben ook *interne specificatie* als een inhoudstype. Items zijn niet noodzakelijkerwijs sluiten elkaar wederzijds uit. Als een item voldoet aan de criteria van beide filters, telt deze ook mee in elk. Deze duplicatie is mogelijk wanneer onderverdeling op `Collection(Edm.String)` velden, die vaak worden gebruikt voor het implementeren van document tagging.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

In het algemeen als u vindt dat facet resultaten zijn consistent te groot is, raden wij aan dat toevoegen u meer filters zodat gebruikers meer opties voor het verfijnen van de zoekopdracht.

### <a name="tips-about-result-count"></a>Tips voor het resultaattelling

**Beperk het aantal items in de navigatiebalk facet**

Er is een standaardlimiet van 10 waarden voor elk meervoudige veld in de navigatiestructuur. Deze standaard is logisch voor navigatie-structuren, want het zorgt ervoor dat de lijst met waarden aan een handelbare hoeveelheid. U kunt deze standaardinstelling negeren door een waarde voor het tellen van toe te wijzen.

* `&facet=city,count:5` Hiermee geeft u op dat alleen de eerste vijf steden gevonden in de rechterbovenhoek gerangschikt resultaten worden geretourneerd als een facet-resultaat. Bekijk een voorbeeldquery met een zoekterm 'luchthaven' en 32 overeenkomsten. Als de query is opgegeven `&facet=city,count:5`, alleen de eerste vijf unieke plaatsen met de meeste documenten in de lijst met zoekresultaten zijn opgenomen in de resultaten facet.

U ziet dat het verschil tussen facet resultaten en lijst met zoekresultaten. Zoekresultaten zijn alle documenten die overeenkomen met de query. Facet resultaten zijn de overeenkomsten voor elke facetwaarde. In het voorbeeld bevatten de zoekresultaten plaatsnamen die zich niet in de lijst met facet classificatie (5 in ons voorbeeld). De resultaten worden gefilterd via meervoudige navigatie zichtbaar wanneer u facetten wissen, of kies andere facetten naast plaats. 

> [!NOTE]
> Hierover te discussiëren `count` wanneer er meer dan één type verwarrend kan zijn. De volgende tabel biedt een kort overzicht van hoe de term wordt gebruikt in Azure Search-API, voorbeeldcode en documentatie. 

* `@colorFacet.count`<br/>
  In de presentatiecode ziet u een parameter van het aantal op het facet, die wordt gebruikt om het aantal facet resultaten weer te geven. Aantal in facet resultaten geeft aan dat het aantal documenten die overeenkomen met de term facet of het bereik.
* `&facet=City,count:12`<br/>
  U kunt in een query facet aantal instellen op een waarde.  De standaardwaarde is 10, maar u kunt dit hogere of lagere instellen. Instellen van `count:12` haalt de bovenste 12 komt overeen met in facet resultaten met documentaantal.
* "`@odata.count`"<br/>
  Deze waarde in de query-antwoord geeft aan dat het aantal overeenkomende items in de lijst met zoekresultaten. Gemiddeld groter is dan de som van alle facet resultaten gecombineerd, vanwege de aanwezigheid van de items die overeenkomen met de zoekterm is, maar hebt u geen waarde facet overeenkomsten.

**Telt het aantal in facet resultaten ophalen**

Wanneer u een filter aan een basis van meervoudig query toevoegen, kunt u de instructie facet behouden (bijvoorbeeld `facet=Rating&$filter=Rating ge 4`). In technisch opzicht kunnen facet = score is niet nodig, maar de aantallen van waarden voor beoordelingen facet zodat deze retourneert 4 en hoger. Bijvoorbeeld, als u klikt op "4" en de query een filter voor groter of gelijk aan "4 bevat", zijn telt het aantal geretourneerd voor elke beoordeling die is 4 en hoger.  

**Zorg ervoor dat u nauwkeurige facet aantallen ophalen**

Onder bepaalde omstandigheden, is het wellicht facet aantallen komen niet overeen met de resultatensets (Zie [facetnavigatie in Azure Search (forumbericht)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Facet tellingen kunnen zijn onjuist vanwege de sharding-architectuur. Elke search-index heeft meerdere shards en elke shard rapporteert de top N-facetten door het documentaantal, die vervolgens worden gecombineerd tot één resultaat. Als sommige shards veel overeenkomende waarden, zijn terwijl anderen er minder hebben, merkt u misschien dat sommige waarden facet ontbreken of onder-telling in de resultaten.

Hoewel dit gedrag op elk gewenst moment wijzigen kan als u momenteel dit probleem ondervindt, kunt u omzeilen het door het aantal kunstmatig verversen:<number> naar een groot aantal om af te dwingen volledige rapportages van elke shard. Als de waarde voor aantal: is groter dan of gelijk is aan het aantal unieke waarden in het veld, bent u nauwkeurige resultaten gegarandeerd. Echter, wanneer het aantal documenten hoog zijn, er is een op de prestaties, dus zorgvuldig gebruik deze optie.

### <a name="user-interface-tips"></a>Tips voor gebruikers-interface
**Labels voor elk veld in de facet navigatie toevoegen**

Labels worden gewoonlijk gedefinieerd in de HTML-code of het formulier (`index.cshtml` in de voorbeeldtoepassing). Er is geen API in Azure Search voor facet navigatielabels of andere metagegevens.

<a name="rangefacets"></a>

## <a name="filter-based-on-a-range"></a>Filteren op basis van een bereik
Onderverdeling via bereiken met waarden is een algemene vereiste voor search-toepassing. Bereiken worden ondersteund voor numerieke gegevens en datum/tijd-waarden. Meer informatie over elke methode in [documenten zoeken (Azure Search API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).

Azure Search vereenvoudigt bereik bouw twee benaderingen voor het berekenen van een bereik bieden. Voor beide benaderingen maakt Azure Search de juiste bereiken gegeven van de invoer die u hebt opgegeven. Bijvoorbeeld, als u de waarden van 10 opgeeft | 20 | 30, wordt kan bereiken van 0-10, 10-20, 20-30 automatisch gemaakt. Uw toepassing kunt (optioneel) verwijderen voor de tijdstippen die leeg zijn. 

**Methode 1: Gebruik de parameter interval**  
Om in te stellen prijs facetten $10 per, zou u het volgende opgeven: `&facet=price,interval:10`

**Methode 2: Gebruik een waardenlijst met**  
Voor numerieke gegevens, kunt u een lijst met waarden.  Houd rekening met het facet bereik voor een `listPrice` veld weergegeven als volgt:

  ![Lijst met waarden van voorbeeld][5]

Als u een bereik facet zoals die in de vorige schermafbeelding, gebruikt u een lijst met waarden:

    facet=listPrice,values:10|25|100|500|1000|2500

Elk bereik is gebouwd met behulp van 0 als uitgangspunt, een waarde in de lijst als een eindpunt, en vervolgens ontdaan van het vorige bereik discrete intervallen maken. Azure Search is deze zaken als onderdeel van meervoudige navigatie. U hoeft niet te schrijven van code voor het structureren van elk interval.

### <a name="build-a-filter-for-a-range"></a>Bouw een filter voor een bereik
Als u wilt filteren op basis van een bereik dat u selecteert, kunt u de `"ge"` en `"lt"` filteren operators in een tweedelige-expressie die de eindpunten van het bereik definieert. Bijvoorbeeld, als u het bereik van 10-25 voor kiezen een `listPrice` veld, het filter is `$filter=listPrice ge 10 and listPrice lt 25`. In de voorbeeldcode wordt het gebruik van de filterexpressie **priceFrom** en **priceTo** parameters om in te stellen van de eindpunten. 

  ![Query voor een bereik van waarden][6]

<a name="geofacets"></a> 

## <a name="filter-based-on-distance"></a>Filter gebaseerd op afstand
Algemene om te zien filters waarmee u kiest een store, een restaurant of een bestemming op basis van de nauwe binding met uw huidige locatie. Hoewel dit type filter als meervoudige navigatie eruitzien kan, is alleen een filter. We hier worden vermeld voor degenen die specifiek op zoek bent naar implementatie advies voor het specifieke ontwerpprobleem.

Er zijn twee georuimtelijke functies in Azure Search **geo.distance** en **geo.intersects**.

* De **geo.distance** functie wordt de afstand in kilometer zijn verwijderd tussen de twee punten. Een punt is een veld en andere is een constante doorgegeven als onderdeel van het filter. 
* De **geo.intersects** functie retourneert waar als een opgegeven punt zich binnen een bepaalde veelhoek. Het punt is een veld en de veelhoek is opgegeven als een constante lijst coördinaten doorgegeven als onderdeel van het filter.

Vindt u voorbeelden van het filter in [syntaxis voor OData-expressie (Azure Search)](query-odata-filter-orderby-syntax.md).

<a name="tryitout"></a>

## <a name="try-the-demo"></a>Probeer de demoversie
De Azure Search taak Portal-Demo bevat de voorbeelden in dit artikel wordt verwezen.

-   Bekijken en testen van de demo werkt online op [Azure Search taak Portal-Demo](https://azjobsdemo.azurewebsites.net/).

-   Download de code uit de [opslagplaats voor Azure-voorbeelden op GitHub](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

Als u met zoekresultaten werkt, bekijk de URL voor wijzigingen in de basisquery's bouwen. Deze toepassing gebeurt facetten toevoegen aan de URI bij het selecteren van elkaar.

1. De functionaliteit van de toewijzing van de demo-app wilt gebruiken, krijgen een Bing kaarten-sleutel van de [Bing Maps Dev Center](https://www.bingmapsportal.com/). Plak deze over de bestaande sleutel in de `index.cshtml` pagina. De `BingApiKey` instellen in de `Web.config` -bestand wordt niet gebruikt. 

2. Voer de toepassing uit. De optionele rondleiding of in het dialoogvenster te sluiten.
   
3. Voer een zoekterm, zoals 'analist', en klik op het zoekpictogram. De query snel wordt uitgevoerd.
   
   Een facetnavigatiestructuur geretourneerd ook met de lijst met zoekresultaten. In de pagina met zoekresultaten bevat de facetnavigatiestructuur aantallen voor elke facet resultaat. Er is geen facetten zijn ingeschakeld, zodat alle overeenkomende resultaten worden geretourneerd.
   
   ![Zoekresultaten voor facetten selecteren][11]

4. Klik op een functie, de locatie of de minimale salaris. Facetten zijn null voor de eerste zoekopdracht maar als ze op basis van waarden, de lijst met zoekresultaten moeten worden ontdaan van de items die niet meer overeen met.
   
   ![Zoekresultaten na het selecteren van facetten][12]

5. Schakel de basis van meervoudig query zodat u andere query gedrag kunt, klikt u op de `[X]` na de geselecteerde facetten om te wissen van de facetten.
   
<a name="nextstep"></a>

## <a name="learn-more"></a>Meer informatie
Bekijk [gedetailleerde informatie over Azure Search](https://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). Op 45:25 is er een demo over het implementeren van facetten.

Voor meer inzicht in ontwerpprincipes voor meervoudige navigatie raden we aan de volgende koppelingen:

* [Ontwerpen voor Facetzoekopdrachten](http://www.uie.com/articles/faceted_search/)
* [Ontwerppatronen: Facetnavigatie](https://alistapart.com/article/design-patterns-faceted-navigation)


<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/azure-search-faceting-example.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png
[11]: ./media/search-faceted-navigation/faceted-search-before-facets.png
[12]: ./media/search-faceted-navigation/faceted-search-after-facets.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: https://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[https://www.odata.org/documentation/odata-version-2-0/overview/]: https://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: https://docs.microsoft.com/rest/api/searchservice/Search-Documents

