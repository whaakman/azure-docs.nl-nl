---
title: Meervoudige navigatie implementeren in Azure Search | Microsoft Docs
description: "Meervoudige navigatie toevoegen aan toepassingen die zijn geïntegreerd met Azure Search, een zoekservice in de cloud gehoste op Microsoft Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: cdf98fd4-63fd-4b50-b0b0-835cb08ad4d3
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 3/10/2017
ms.author: heidist
ms.openlocfilehash: 413f498eeb0bbc9a971c7a65200ed2fd8caa9aaf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-implement-faceted-navigation-in-azure-search"></a>Facetnavigatie implementeren in Azure Search
Meervoudige navigatie is een filtermechanisme waarmee zelf gerichte analyse navigatie in toepassingen. De term 'facetnavigatie' niet bekend zijn, maar u hebt waarschijnlijk het eerder gebruikt. Zoals in het volgende voorbeeld toont, wordt er meervoudige navigatie is niets meer dan de categorieën die wordt gebruikt voor het filteren van resultaten.

 ![Azure Search taak Portal-Demo][1]

Meervoudige navigatie is een alternatieve toegangspunt om te zoeken. Biedt een alternatief voor het zoeken van complexe expressies handmatig te typen. Facetten kunt u vinden wat u zoekt, terwijl u ervoor zorgt dat er geen nul resultaten. Als een ontwikkelaar kunnen facetten u de nuttigste zoekcriteria voor uw zoekopdracht corpus navigeren blootstellen. In online retail-toepassingen is vaak facetnavigatie gebouwd via merken, afdelingen (kinderen om), grootte, prijs, populariteit en classificaties. 

Implementatie van meervoudige navigatie verschilt voor elk voor zoekopdracht technologieën. In Azure Search is meervoudige navigatie op moment van de query wordt gebouwd met velden die u eerder in uw schema toegeschreven.

-   In de query's die uw toepassing wordt gemaakt, een query moet verzenden *facet queryparameters* ophalen van de beschikbare facet filterwaarden voor de resultatenset van dat document.

-   Het resultaat van het document daadwerkelijk knippen ingesteld, wordt de toepassing moet worden toegepast een `$filter` expressie.

In de toepassingsontwikkeling van uw vormt schrijven van code die wordt gemaakt van query's het grootste deel van het werk. Veel van het gedrag van toepassingen die u van meervoudige navigatie verwacht worden geleverd door de service, inclusief de ingebouwde ondersteuning voor het definiëren van de bereiken en ophalen van tellingen voor facet resultaten. De service omvat tevens logisch standaardwaarden die u helpen te voorkomen dat onhandig navigatiestructuur. 

## <a name="sample-code-and-demo"></a>Voorbeeldcode en demo
In dit artikel wordt een taak zoeken portal als voorbeeld. In het voorbeeld is geïmplementeerd als een ASP.NET MVC-toepassing.

-   Zie en testen van de werkende demo online op [Azure Search taak Portal-Demo](http://azjobsdemo.azurewebsites.net/).

-   Download de code uit de [Azure-Samples-opslagplaats op GitHub](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

## <a name="get-started"></a>Aan de slag
Als u nieuwe ontwikkeling zoeken, is het beste denken aan facetnavigatie dat de mogelijkheden voor zelf gerichte zoekopdracht te zien. Het is een soort inzoomen zoekervaring, op basis van vooraf gedefinieerde filters, die worden gebruikt voor het verfijnen van snel omlaag zoekresultaten via point-and-click acties. 

### <a name="interaction-model"></a>Interactie model

De zoekfunctie voor meervoudige navigatie is herhaald, dus laten we eerst het als een reeks van query's die in reactie op gebruikersacties uitgevouwen begrijpen.

Het beginpunt is een toepassingspagina waarmee facetnavigatie doorgaans op de omtrek geplaatst. Meervoudige navigatie is vaak een boomstructuur met selectievakjes uit voor elke waarde of overeen. 

1. Een query verzonden naar Azure Search Hiermee geeft u de structuur facetnavigatie via een of meer facet queryparameters. Bijvoorbeeld: de query kan onder andere `facet=Rating`, mogelijk met een `:values` of `:sort` optie om de presentatie verder te verfijnen.
2. De laag voor presentatie geeft een zoekpagina waarmee facetnavigatie met de facetten die zijn opgegeven voor de aanvraag.
3. Uitgaande van een meervoudige navigatie-structuur met de classificatie, u klikt op '4' om aan te geven dat alleen producten met een classificatie van 4 of hoger moeten worden weergegeven. 
4. Als antwoord verzendt de toepassing een query met`$filter=Rating ge 4` 
5. De laag voor presentatie updates de pagina, met een verminderde resultatenset met alleen de items die voldoen aan de nieuwe criteria (in dit geval producten beoordeeld 4 en hoger).

Een facet is een queryparameter, maar niet verwarren met invoer voor de query. Dit wordt nooit gebruikt als selectiecriteria in een query. In plaats daarvan zien facet queryparameters als invoer voor de navigatiestructuur die weer in het antwoord. Voor elke facet queryparameter die u opgeeft, evalueert Azure Search hoeveel documenten zijn in de gedeeltelijke resultaten voor elke facetwaarde.

U ziet de `$filter` in stap 4. Het filter is een belangrijk aspect van meervoudige navigatie. Hoewel de facetten en filters onafhankelijk van elkaar in de API zijn, moet u zowel voor die u van plan bent gebruikerservaring. 

### <a name="app-design-pattern"></a>Het ontwerppatroon App

In de toepassingscode is het patroon facet queryparameters gebruiken om te retourneren van de structuur facetnavigatie samen met facet resultaten, plus een $filter-expressie.  De filterexpressie verwerkt de gebeurtenis click van de facetwaarde van. Vergelijken met de `$filter` expressie als de code achter de werkelijke bijsnijden zoekresultaten geretourneerd naar de laag voor presentatie. Gezien een facet kleuren, te klikken op de kleur rood wordt geïmplementeerd via een `$filter` expressie die u selecteert alleen de items die een kleur van rood hebben. 

### <a name="query-basics"></a>Query-basisbeginselen

In Azure Search een aanvraag via een of meer queryparameters is opgegeven (Zie [documenten zoeken](http://msdn.microsoft.com/library/azure/dn798927.aspx) voor een beschrijving van elk criterium). Geen van de query-parameters zijn vereist, maar u moet ten minste één om een query naar een geldig hebben.

Precisie, zoals de mogelijkheid om te filteren irrelevante treffers wordt bereikt door een of beide van deze expressies te begrijpen:

-   **zoeken =**  
    De waarde van deze parameter wordt verstaan onder de zoekopdracht. Een los stukje van tekst of een complexe zoekopdracht-expressie met meerdere voorwaarden en operators mogelijk. De expressie voor een zoekopdracht wordt op de server gebruikt voor zoeken in volledige tekst, uitvoeren van query's doorzoekbare velden in de index voor die overeenkomt met de voorwaarden, retourneren resultaten in de volgorde van de waarden van positie. Als u instelt `search` op null, query wordt via de gehele index (dat wil zeggen, `search=*`). In dit geval andere elementen van de query, zoals een `$filter` of score berekenen profiel, zijn de primaire factoren die invloed hebben op welke documenten worden geretourneerd `($filter`) en in welke volgorde (`scoringProfile` of `$orderby`).

-   **$filter =**  
    Een filter is een krachtig mechanisme voor het beperken van de grootte van de zoekresultaten op basis van de waarden van specifieke documentkenmerken. Een `$filter` wordt eerst geëvalueerd, gevolgd door de facetten logica waarmee de beschikbare waarden en de bijbehorende tellingen voor elke waarde wordt gegenereerd

Complexe zoekopdracht expressies verminderen de prestaties van de query. Waar mogelijk, gebruikt u goed opgebouwde Filterexpressies voor betere nauwkeurigheid en queryprestaties verbeteren.

Vergelijk om beter te begrijpen hoe nauwkeuriger in een filter wordt toegevoegd, een complexe zoekexpressie dat een filterexpressie bevat:

-   `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`
-   `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Beide query's zijn geldig, maar de tweede beter als u niet motels met parkeerplaatsen in Haarlem zoekt is.
-   De eerste query, is afhankelijk van deze specifieke woorden wordt vermeld of niet in tekenreeksvelden zoals naam, beschrijving en andere velden die doorzoekbare gegevens genoemd.
-   De tweede query zoekt nauwkeurige overeenkomsten op gestructureerde gegevens en is waarschijnlijk nauwkeuriger.

In toepassingen die facetnavigatie omvatten, maar zorg ervoor dat wordt elke actie van de gebruiker over de structuur van een meervoudige navigatie vergezeld van een beperking van de zoekresultaten. Gebruiken om de resultaten beperken, een filterexpressie.

<a name="howtobuildit"></a>

## <a name="build-a-faceted-navigation-app"></a>Een meervoudige navigatie-app bouwen
U implementeren meervoudige navigatie met Azure Search in uw toepassingscode die de zoekopdracht voortbouwt. Er is een meervoudige navigatie afhankelijk van de elementen in het schema dat u eerder hebt gedefinieerd.

Vooraf gedefinieerd in de zoekopdracht index is de `Facetable [true|false]` indexkenmerk, ingesteld op de geselecteerde velden of het gebruik ervan in een structuur facetnavigatie uit te schakelen. Zonder `"Facetable" = true`, een veld in de navigatiebalk facet kan niet worden gebruikt.

De laag voor presentatie in uw code biedt de gebruikerservaring. Het moet delen van de facetnavigatie, zoals het label, waarden, selectievakjes en het aantal aanbieden. De Azure Search REST-API is platform networkdirect, dus elke taal en het platform dat u wilt gebruiken. Het belangrijkste is om op te nemen van UI-elementen die ondersteuning bieden voor incrementele vernieuwen met bijgewerkte gebruikersinterfacestatus omdat elke extra facet is geselecteerd. 

Op het moment dat de query, maakt de toepassingscode een aanvraag met `facet=[string]`, een aanvraagparameter waarmee het veld facet door. Een query kunt beschikt over meerdere facetten zoals `&facet=color&facet=category&facet=rating`, elkaar gescheiden door een ampersand (&)-teken.

Ook moet opgeven, toepassingscode een `$filter` expressie voor het afhandelen van de gebeurtenissen klikken in meervoudige navigatie. Een `$filter` minder zoekresultaten wilt weergeven, met behulp van de facetwaarde als filtercriteria.

Azure Search retourneert de zoekresultaten op basis van een of meer voorwaarden die u, samen met updates voor de structuur meervoudige navigatie invoert. In Azure Search meervoudige navigatie is één niveau gebouwd facet waarden, statistieken en hoeveel resultaten gevonden voor elk criterium.

In de volgende secties nemen we het bouwen van elk onderdeel nader bekijken.

<a name="buildindex"></a>

## <a name="build-the-index"></a>Bouw de index
Facetten is ingeschakeld op basis van door veld in de index, via deze indexkenmerk: `"Facetable": true`.  
Alle veldtypen die mogelijk kunnen worden gebruikt in facetnavigatie `Facetable` standaard. Dergelijke veldtypen omvatten `Edm.String`, `Edm.DateTimeOffset`, en de numerieke veldtypen (in wezen alle veldtypen zijn geschikt voor facetten behalve `Edm.GeographyPoint`, die niet worden gebruikt in facetnavigatie). 

Als u een index maakt, is een best practice voor meervoudige navigatie expliciet facetten om uit te schakelen voor velden die nooit mogen worden gebruikt als een beperkingsfacet.  In het bijzonder tekenreeksvelden voor singleton-waarden, zoals een naam-ID of product moeten worden ingesteld op `"Facetable": false` om te voorkomen dat het gebruik ervan onbedoeld (en niet effectief) in meervoudige navigatie. Facetten uitschakelen wanneer u deze niet nodig schakelen helpt u de grootte van de index klein en meestal verbetert de prestaties.

Volgende maakt deel uit van het schema voor de taak Portal-Demo voorbeeld-app afgekapte van enkele kenmerken om de grootte te beperken:

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

Zoals u in het voorbeeldschema ziet `Facetable` is uitgeschakeld voor tekenreeksvelden als facetten, zoals id-waarden mag niet worden gebruikt. Facetten uitschakelen wanneer u deze niet nodig schakelen helpt u de grootte van de index klein en meestal verbetert de prestaties.

> [!TIP]
> Als een best practice, omvatten de volledige set van indexkenmerken voor elk veld. Hoewel `Facetable` is standaard ingeschakeld voor bijna alle velden, opzettelijk instelling elk kenmerk kunt u denkt dat via de implicaties van elke beslissing schema. 

<a name="checkdata"></a>

## <a name="check-the-data"></a>Controleer de gegevens
De kwaliteit van uw gegevens heeft een directe invloed op of de structuur facetnavigatie gebeurtenis zich voordoet dan u verwacht. Het is ook van invloed op het gemak van het construeren van filters zodat de resultatenset.

Als u wilt facet door merk of prijs, elk document moet bevatten waarden voor *BrandName* en *ProductPrice* die geldig, consistente en productief zijn als een filteroptie zijn.

Hier volgen enkele herinneringen van wat u wilt verwijderen voor:

* Voor elk veld dat u wilt facet door, afvragen of deze bevat waarden die geschikt is als filters in zelf gerichte zoeken. De waarden moeten worden korte, beschrijvende en voldoende onderscheidende bieden een duidelijke keuze tussen concurrerende opties.
* De spelling of bijna overeenkomende waarden. Als u facet op kleur en veldwaarden oranje en Ornage (onjuist gespeld) omvatten, maar een facet op basis van het veld kleur zou kunnen worden opgepikt beide.
* Gemengde case tekst kan ook aanrichten functies in facetnavigatie met oranje en oranje verschijnen als twee verschillende waarden. 
* Enkelvoudige en meervoudige versies van dezelfde waarde kunnen resulteren in een afzonderlijke facet voor elk.

Als u zich kunt voorstellen is zorgvuldigheid voorbereiden van de gegevens in een essentiële aspect van effectieve meervoudige navigatie.

<a name="presentationlayer"></a>

## <a name="build-the-ui"></a>De gebruikersinterface bouwen
Werken terug vanaf de laag voor presentatie kunt u de vereisten die mogelijk anders worden overgeslagen, en u begrijpt welke mogelijkheden essentieel voor de zoekfunctie zijn onthullen.

Detecteert de invoer van de gebruiker op de pagina en de gewijzigde elementen invoegt in termen van meervoudige navigatie uw pagina website of toepassing geeft de structuur meervoudige navigatie. 

Voor webtoepassingen, wordt AJAX meestal gebruikt in de laag voor presentatie omdat Hiermee kunt u incrementele wijzigingen vernieuwen. U kunt ook ASP.NET MVC- of een andere visualisatie-platform die verbinding met een Azure Search-service via HTTP maken kan. De voorbeeldtoepassing waarnaar wordt verwezen in dit artikel--de **Azure Search taak Portal-Demo** – gebeurt er met een ASP.NET MVC-toepassing.

In het voorbeeld is meervoudige navigatie ingebouwd in de pagina met zoekresultaten. Het volgende voorbeeld wordt opgehaald uit de `index.cshtml` resultatenpagina-bestand van de voorbeeldtoepassing, toont de statische HTML-structuur voor het weergeven van meervoudige navigatie op de zoekopdracht. De lijst met facetten is gebouwd of dynamisch opnieuw opbouwen wanneer u een zoekterm of schakelt u uit een beperkingsfacet.

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

Het volgende codefragment uit de `index.cshtml` pagina dynamisch de HTML-code om het eerste facet, zakelijke titel weer te geven. Soortgelijke functies bouwen dynamisch de HTML-code voor de andere facetten. Elke facet heeft een label en een telling waarin het aantal items gevonden voor die resulteren facet.

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
> Wanneer u de pagina met zoekresultaten ontwerpt, moet u een mechanisme voor het wissen van facetten toevoegen. Als u de selectievakjes toevoegt, kunt u eenvoudig zien hoe de filters wissen. Voor andere indelingen moet u mogelijk een patroon breadcrumb of een andere creative benadering. Bijvoorbeeld, in de voorbeeldtoepassing taak zoeken Portal kunt u de `[X]` na een geselecteerde beperkingsfacet het facet wissen.

<a name="buildquery"></a>

## <a name="build-the-query"></a>De query bouwen
De code die u voor het bouwen van query's schrijven moet alle onderdelen van een geldige query, zoals zoeken expressies, facetten, filters, score berekenen profielen – iets gebruikt voor het opstellen van een aanvraag opgeven. In deze sectie besproken waar facetten past in een query en hoe filters met facetten worden gebruikt voor het leveren van een verminderde resultatenset.

U ziet dat facetten integraal in deze voorbeeldtoepassing. De zoekfunctie in de taak Portal-Demo is ontworpen om meervoudige navigatie en filters. De opvallende plaatsing van meervoudige navigatie op de pagina ziet u het belang. 

Een voorbeeld is vaak een goede plaats om te beginnen. Het volgende voorbeeld wordt opgehaald uit de `JobsSearch.cs` builds die een aanvraag die facet navigatie worden gemaakt op basis van functie, locatie, soort boeken en minimale salaris-bestand. 

```cs
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "business_title", "posting_type", "level", "salary_range_from,interval:50000" },
};
```

Een queryparameter facet is ingesteld op een veld en afhankelijk van het gegevenstype, kan worden verder als parameters gebruikt door komma's gescheiden lijst met `count:<integer>`, `sort:<>`, `interval:<integer>`, en `values:<list>`. Een lijst met waarden wordt ondersteund voor numerieke gegevens bij het instellen van bereiken. Zie [documenten zoeken (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) voor informatie over het gebruik.

De aanvraag geformuleerd door uw toepassing moet samen met de facetten, ook bouwen van filters om de set candidate documenten op basis van een waarde facet selectie vast te stellen. Voor een winkel fiets facetnavigatie biedt aanwijzingen op vragen zoals *welke kleuren, fabrikanten en typen bikes beschikbaar zijn?*. Filteren van antwoorden op vragen zoals *mountainbikes, welke exacte bikes rood zijn, in dit bereik te prijs?*. Als u op "Red" om aan te geven dat alleen rode producten moeten worden weergegeven, de volgende query de toepassing verzendt bevat een `$filter=Color eq ‘Red’`.

Het volgende codefragment uit de `JobsSearch.cs` pagina wordt de geselecteerde functie aan het filter toegevoegd als u een waarde uit het zakelijke titel facet selecteert.

```cs
if (businessTitleFacet != "")
  filter = "business_title eq '" + businessTitleFacet + "'";
```

<a name="tips"></a> 

## <a name="tips-and-best-practices"></a>Tips en best practices

### <a name="indexing-tips"></a>Tips indexeren
**Index-efficiëntie te verbeteren, als u een zoekvak niet gebruikt**

Als uw toepassing gebruikmaakt van meervoudige navigatie uitsluitend (dat wil zeggen, geen zoekvak), kunt u het veld als markeren `searchable=false`, `facetable=true` voor het produceren van een meer compacte index. Bovendien indexeren doet zich alleen op de hele facet waarden, met geen woordafbreking of van de onderdelen van een waarde met meerdere word te indexeren.

**Opgeven welke velden kunnen worden gebruikt als facetten**

Let erop dat het schema van de index welke velden beschikbaar zijn bepaalt voor gebruik als een beperkingsfacet. Ervan uitgaande dat een veld is geschikt voor facetten, geeft de query aan welke velden moeten worden facet door. Het veld waarop u facetten zijn bevat de waarden die worden weergegeven onder het label. 

De waarden die worden vermeld onder elk label worden opgehaald uit de index. Bijvoorbeeld, als het veld facet *kleur*, de waarden die beschikbaar zijn voor het filteren van aanvullende zijn de waarden voor dat veld - rood en zwart.

Voor numerieke en datum-/ alleen waarden, kunt u waarden expliciet instellen op het veld facet (bijvoorbeeld `facet=Rating,values:1|2|3|4|5`). Een lijst met waarden is toegestaan voor deze veldtypen voor het vereenvoudigen van de scheiding tussen facet resultaten in aaneengesloten bereiken (ofwel bereiken op basis van numerieke waarden of perioden). 

**Standaard kunt u slechts één niveau van meervoudige navigatie hebt** 

Zoals is aangegeven, is er geen rechtstreekse ondersteuning voor het nesten van facetten in een hiërarchie. Standaard ondersteunt meervoudige navigatie in Azure Search slechts één niveau van filters. Tijdelijke oplossingen echter bestaan. U kunt coderen een facet hiërarchische structuur in een `Collection(Edm.String)` verwijzen met één vermelding per hiërarchie. Deze oplossing implementeren valt buiten het bereik van dit artikel. 

### <a name="querying-tips"></a>Tips opvragen
**Velden valideren**

Als u de lijst met facetten dynamisch op basis van niet-vertrouwde gebruikersinvoer bouwt, controleert u de namen van de meervoudige velden zijn geldig. Of de namen escape tijdens het bouwen van URL's met behulp van `Uri.EscapeDataString()` in .NET of de overeenkomstige waarde in uw favoriete platform.

### <a name="filtering-tips"></a>Tips voor het filteren
**De precisie van de zoekopdracht verhogen met filters**

Filters gebruiken. Als u zich op net zoeken expressies alleen gegevens als gevolg kan leiden tot een document baseert moet worden geretourneerd die niet de facetwaarde van de exacte hebben met een van de velden.

**Het verhogen van de prestaties van de zoekopdracht met filters**

Filters beperken de set candidate documenten voor zoeken en ranking uitsluiten. Als er een groot aantal documenten, kunt met behulp van een selectief beperkingsfacet inzoomen vaak u betere prestaties.
  
**Alleen de meervoudige velden filteren**

In meervoudige Inzoomen wilt u waarschijnlijk alleen documenten met de facetwaarde in een specifieke (beperkt) veld niet overal in de doorzoekbare velden opnemen. Toevoegen van een filter versterkt het doelveld door de service om alleen in het veld meervoudige naar een overeenkomende waarde te zoeken.

**Trim facet resultaten met meer filters**

Facet resultaten worden gevonden in de zoekresultaten die overeenkomen met een term facet documenten. In het volgende voorbeeld wordt in de zoekresultaten voor *cloudcomputing*, 254 items hebben ook *interne specificatie* als een inhoudstype. Items zijn niet noodzakelijkerwijs sluiten elkaar wederzijds uit. Als een item voldoet aan de criteria van beide filters, wordt het beschouwd in elk criterium. Deze duplicaten is mogelijk wanneer facetten op `Collection(Edm.String)` velden, die vaak worden gebruikt voor het implementeren van document labels.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

In het algemeen als u vindt dat facet resultaten consistent is te groot, raden we toevoegen u meer filters zodat gebruikers op meer opties voor het verfijnen van de zoekopdracht.

### <a name="tips-about-result-count"></a>Tips voor het aantal resultaten

**Het aantal items in de navigatiebalk facet beperken**

Er is een standaardlimiet van 10 waarden voor elk meervoudige veld in de navigatiestructuur. Deze standaardinstelling zinvolle voor navigatiestructuur omdat het zorgt ervoor dat de waardelijst beheerbare grootte. U kunt deze standaardinstelling negeren door het toewijzen van een waarde op te tellen.

* `&facet=city,count:5`Hiermee geeft u op dat alleen de eerste vijf steden gevonden in de rechterbovenhoek gerangschikt resultaten worden geretourneerd als gevolg hiervan facet. U kunt een voorbeeldquery met een zoekterm van 'luchthaven' en 32 overeenkomsten. Als de query geeft `&facet=city,count:5`, alleen de eerste vijf unieke steden met de meeste documenten in de zoekresultaten zijn opgenomen in de resultaten facet.

U ziet het onderscheid tussen facet resultaten en zoekresultaten. Zoekresultaten zijn alle documenten die overeenkomen met de query. Facet resultaten worden de overeenkomsten voor elke facetwaarde. In het voorbeeld bevatten de zoekresultaten stad namen die zich niet in de lijst met facet classificatie (5 in ons voorbeeld). De resultaten worden gefilterd via facetnavigatie zichtbaar wanneer u facetten wissen, of kies andere facetten naast plaats. 

> [!NOTE]
> Bespreken `count` wanneer er meer dan één type kan verwarrend zijn. De volgende tabel biedt een kort overzicht van hoe de term wordt gebruikt in Azure Search API, voorbeeldcode en documentatie. 

* `@colorFacet.count`<br/>
  In de presentatiecode ziet u een parameter van het aantal op het facet, gebruikt om het aantal facet resultaten weer te geven. Aantal geeft in facet resultaten, het aantal documenten die overeenkomen met het facet term of -bereik.
* `&facet=City,count:12`<br/>
  U kunt in een query facet aantal instellen op een waarde.  De standaardwaarde is 10, maar u kunt deze hogere of lagere instellen. Instelling `count:12` boven 12 komt overeen met in facet resultaten op het aantal document opgehaald.
* "`@odata.count`"<br/>
  Deze waarde geeft in het queryantwoord, het aantal overeenkomende items in de zoekresultaten. Gemiddeld het groter is dan de som van alle facet resultaten gecombineerd, vanwege de aanwezigheid van items die overeenkomen met de zoekterm, maar geen overeenkomsten facet-waarde hebben.

**Telt het aantal in facet resultaten ophalen**

Wanneer u een filter aan een meervoudige query toevoegt, wilt u mogelijk de instructie facet bewaren (bijvoorbeeld `facet=Rating&$filter=Rating ge 4`). Technisch gezien facet = waardering is niet nodig, maar deze te houden retourneert het aantal waarden voor beoordelingen facet 4 en hoger. Bijvoorbeeld, als u op "4" en de query een filter voor groter of gelijk aan "4 bevat", zijn aantallen geretourneerd voor elke beoordeling die is 4 en hoger.  

**Zorg ervoor dat u nauwkeurige facet aantallen ophalen**

Onder bepaalde omstandigheden, ziet u wellicht facet aantallen komen niet overeen met de resultatensets (Zie [facetnavigatie in Azure Search (forum post)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Facet tellingen kunnen onnauwkeurig zijn vanwege de sharding-architectuur. Elke search-index heeft meerdere shards en elke shard rapporteert de bovenste N facetten door het aantal documenten, die vervolgens worden gecombineerd tot een enkelvoudig resultaat wordt verkregen. Als sommige shards veel overeenkomende waarden, hebt terwijl anderen minder hebben, merkt u dat sommige waarden facet ontbreken of onder-telling in de resultaten.

Hoewel dit gedrag op elk gewenst moment wijzigen kan als u vandaag de dag dit probleem ondervindt, kunt u omzeilen het door het aantal kunstmatig verversen:<number> tot een groot aantal af te dwingen volledige rapportages van elke shard. Als de waarde van het aantal: is groter dan of gelijk is aan het aantal unieke waarden in het veld, wordt gegarandeerd nauwkeurige resultaten. Echter, wanneer het aantal documenten, wat hoog zijn, er is een prestatiestraf, dus zorgvuldig gebruik deze optie.

### <a name="user-interface-tips"></a>Tips voor gebruiker-interface
**Labels voor elk veld in de navigatiebalk facet toevoegen**

Labels worden gewoonlijk gedefinieerd in het HTML-code of het formulier (`index.cshtml` in de voorbeeldtoepassing). Er is geen API in Azure Search voor facet navigatielabels of andere metagegevens.

<a name="rangefacets"></a>

## <a name="filter-based-on-a-range"></a>Filteren op basis van een bereik
Facetten via bereiken met waarden is een algemene zoekopdracht toepassing vereiste. Bereiken worden ondersteund voor numerieke gegevens en DateTime-waarden. Meer informatie over elke methode in [documenten zoeken (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Azure Search vereenvoudigt bereik constructie dankzij de twee methoden voor het berekenen van een bereik. Voor beide benaderingen maakt Azure Search de juiste bereiken gegeven van de invoerwaarden die u hebt opgegeven. Bijvoorbeeld, als u de waarden van 10 opgeeft | 20 | 30, wordt kan bereiken van 0-10, 10-20, 20-30 automatisch gemaakt. Uw toepassing kunt desgewenst verwijderen voor de tijdstippen die leeg zijn. 

**Methode 1: Gebruik de parameter interval**  
Prijs facetten in $10 stappen stelt opgeven:`&facet=price,interval:10`

**Methode 2: Een lijst met waarden gebruiken**  
U kunt een lijst met waarden gebruiken voor numerieke gegevens.  Houd rekening met het facet bereik voor een `listPrice` veld als volgt weergegeven:

  ![Lijst met waarden van voorbeeld][5]

Als u wilt een bereik facet zoals in de vorige schermafbeelding opgeeft, gebruikt u een lijst met waarden:

    facet=listPrice,values:10|25|100|500|1000|2500

Elk bereik is 0 als uitgangspunt, een waarde uit de lijst als een eindpunt gebruikt en vervolgens verwijderd van het vorige bereik maken discrete intervallen gebouwd. Azure Search biedt deze dingen als onderdeel van meervoudige navigatie. U beschikt niet over code schrijven voor elke periode structureren.

### <a name="build-a-filter-for-a-range"></a>Een filter voor een bereik bouwen
Als u wilt filteren documenten op basis van een bereik dat u selecteert, kunt u de `"ge"` en `"lt"` filteren operators in een tweedelige-expressie die u de eindpunten van het bereik definieert. Bijvoorbeeld, als u het bereik van 10-25 voor kiezen een `listPrice` veld het filter is `$filter=listPrice ge 10 and listPrice lt 25`. In de voorbeeldcode gebruikmaakt van de filterexpressie **priceFrom** en **priceTo** parameters voor het instellen van de eindpunten. 

  ![Query voor een bereik van waarden][6]

<a name="geofacets"></a> 

## <a name="filter-based-on-distance"></a>Filter gebaseerd op afstand
Algemene om te zien, filtert waarmee u kiest een store, een restaurant of een bestemming op basis van de nabijheid tot uw huidige locatie. Hoewel dit type filter als facetnavigatie eruitzien kan, is alleen een filter. We vermeld hier voor mensen die specifiek implementatie advies voor het probleem bepaald ontwerp zoekt.

Er zijn twee georuimtelijke functies in Azure Search **geo.distance** en **geo.intersects**.

* De **geo.distance** functie wordt de afstand in kilometer tussen twee punten. Één punt is een veld en andere een constante doorgegeven als onderdeel van het filter. 
* De **geo.intersects** functie retourneert true als er een opgegeven punt zich binnen een bepaalde veelhoek. Het punt een veld is en de veelhoek is opgegeven als een constante lijst coördinaten doorgegeven als onderdeel van het filter.

Vindt u voorbeelden van filter in [OData-expressiesyntaxis (Azure Search)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>

## <a name="try-the-demo"></a>Probeer de demoversie
De Azure Search taak Portal-Demo bevat de voorbeelden in dit artikel wordt verwezen.

-   Zie en testen van de werkende demo online op [Azure Search taak Portal-Demo](http://azjobsdemo.azurewebsites.net/).

-   Download de code uit de [Azure-Samples-opslagplaats op GitHub](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

Als u met zoekresultaten werkt, bekijkt u de URL voor wijzigingen in het samenstellen van query. Deze toepassing gebeurt facetten toevoegen aan de URI terwijl u elk item selecteert.

1. Voor het gebruik van de functionaliteit van de toewijzing van de demo-app moet u een Bing Maps-sleutel van de [Bing Maps Dev Center](https://www.bingmapsportal.com/). Plak deze via de bestaande sleutel in de `index.cshtml` pagina. De `BingApiKey` instellen in de `Web.config` -bestand wordt niet gebruikt. 

2. Voer de toepassing uit. De optionele rondleiding of het dialoogvenster sluiten.
   
3. Geef een zoekterm, zoals 'analist', en klik op het zoekpictogram. Snel de query wordt uitgevoerd.
   
   De structuur van een meervoudige navigatie wordt ook geretourneerd met de zoekresultaten. In de pagina met zoekresultaten bevat de structuur facetnavigatie tellingen voor elk resultaat facet. Er is geen facetten zijn geselecteerd, zodat alle overeenkomende resultaten worden geretourneerd.
   
   ![Zoekresultaten voordat u selecteert facetten][11]

4. Klik op een functie, de locatie of de minimale salaris. Facetten null in de eerste zoekopdracht zijn, maar als deze waarden ondernemen, de zoekresultaten zijn afgekapt tot van items die niet langer overeenkomen.
   
   ![Zoekresultaten na het selecteren van facetten][12]

5. Schakel de meervoudige query zodat u andere query gedrag kunt, klikt u op de `[X]` na de geselecteerde facetten te wissen van de facetten.
   
<a name="nextstep"></a>

## <a name="learn-more"></a>Meer informatie
Bekijk [Azure Search diepgaand](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). 45:25 is er een demo voor het implementeren van facetten.

Om meer inzicht te krijgen over ontwerp beginselen voor meervoudige navigatie raden we aan de volgende koppelingen:

* [Ontwerpen voor meervoudige zoeken](http://www.uie.com/articles/faceted_search/)
* [Ontwerppatronen: Meervoudige navigatie](http://alistapart.com/article/design-patterns-faceted-navigation)


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
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

