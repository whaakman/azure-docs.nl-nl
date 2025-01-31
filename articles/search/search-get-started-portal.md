---
title: 'Quickstart: Maken, laden en query uitvoeren in een index met behulp van Azure portal - Azure Search'
description: Gebruik de wizard gegevens importeren in Azure portal maken, laden en query uitvoeren op uw eerste Azure Search-index.
author: lobrien
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/01/2019
ms.author: laobri
ms.custom: seodec2018
ms.openlocfilehash: 2a4d7435383f740dc386a740062e66cd2d3585b0
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798919"
---
# <a name="quickstart-create-an-azure-search-index-using-the-azure-portal"></a>Quickstart: Een Azure Search-index met behulp van de Azure portal maken
> [!div class="op_single_selector"]
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-get-started-powershell.md)
> * [Postman](search-get-started-postman.md)
> * [Python](search-get-started-python.md)
> * [C#](search-get-started-dotnet.md)

Probeer de ingebouwde hulpprogramma's in Azure Portal als u snel vertrouwd wilt raken met de concepten van Azure Search. Wizards en editors bieden geen volledige pariteit met de .NET- en REST API's maar u kunt snel aan de slag met een inleiding die u laat zien hoe u zonder code binnen een paar minuten interessante query's kunt schrijven met behulp van voorbeeldgegevens.

> [!div class="checklist"]
> * Begin met een gratis openbare op Azure gehoste set voorbeeldgegevens
> * Voer de wizard **Gegevens importeren** in Azure Search uit om gegevens te laden en een index te genereren
> * Bewaak de voortgang van de indexering in de portal
> * Bekijk een bestaande index en opties om deze te wijzigen
> * Probeer zoekopdrachten in volledige tekst, filters, facetten, fuzzy zoekopdrachten en geosearch met **Search Explorer**

Als u de hulpprogramma's zijn te beperken, kunt u overwegen een [code gebaseerde Inleiding tot het programmeren van Azure Search in .NET](search-howto-dotnet-sdk.md) of gebruik [Postman voor het maken van de REST API-aanroepen](search-get-started-postman.md). U kunt ook een 6 minuten durende demonstratie bekijken van de stappen in deze zelfstudie. De demonstratie begint na ongeveer drie minuten in deze [Azure Search-overzichtsvideo](https://channel9.msdn.com/Events/Connect/2016/138).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten

[Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze Quick Start. 

### <a name="check-for-space"></a>Controleren of er voldoende ruimte is

Veel klanten beginnen met de gratis service. Deze versie is beperkt tot drie indexen drie gegevensbronnen en drie indexeerfuncties. Zorg ervoor dat er voldoende ruimte is voor extra items voordat u begint. In deze zelfstudie wordt van elk object één exemplaar gemaakt.

Op secties in het servicedashboard wordt weergegeven hoeveel indexen, indexeerfuncties en gegevensbronnen u al hebt. 

![Lijsten met indexen, indexeerfuncties en gegevensbronnen](media/search-get-started-portal/tiles-indexers-datasources.png)

## <a name="create-index"></a> Een index maken en gegevens laden

Zoekopdrachten worden herhaald over een [*index*](search-what-is-an-index.md) die doorzoekbare gegevens, metagegevens en aanvullende constructies bevat, die worden gebruikt om bepaald zoekgedrag te optimaliseren.

Voor deze zelfstudie gebruiken we een ingebouwde gegevensset die met behulp van een [*indexeerfunctie*](search-indexer-overview.md) kan worden verkend via de wizard **Gegevens importeren**. Een indexeerfunctie is een bronspecifieke verkenner die metagegevens en inhoud uit ondersteunde Azure-gegevensbronnen kan lezen. Normaal gesproken worden indexeerfuncties via een programma gebruikt, maar in de portal zijn ze toegankelijk via de wizard **Gegevens importeren**. 

### <a name="step-1---start-the-import-data-wizard-and-create-a-data-source"></a>Stap 1: de wizard Gegevens importeren starten en een gegevensbron maken

1. Klik in het dashboard van de Azure Search-service op **Gegevens importeren** op de opdrachtbalk om een zoekindex te maken en deze te vullen met gegevens.

   ![Opdracht Gegevens importeren](media/search-get-started-portal/import-data-cmd.png)

2. In de wizard, klikt u op **verbinding maken met uw gegevens** > **voorbeelden** > **hotels-sample**. Deze gegevensbron is ingebouwd. Als u uw eigen gegevensbron zou maken, zou u een naam, het type en de verbindingsgegevens moeten opgeven. Zodra de gegevensbron is gemaakt, wordt deze een bestaande gegevensbron genoemd die opnieuw kan worden gebruikt voor andere bewerkingen.

   ![Voorbeeldgegevensset selecteren](media/search-get-started-portal/import-datasource-sample.png)

3. Ga door naar de volgende pagina.

   ![De knop volgende pagina voor Cognitive Search](media/search-get-started-portal/next-button-add-cog-search.png)

### <a name="step-2---skip-cognitive-skills"></a>Stap 2: cognitieve vaardigheden overslaan

De wizard ondersteunt het maken van een [pijplijn voor cognitieve vaardigheden](cognitive-search-concept-intro.md) voor het opnemen van de AI-algoritmen van Cognitive Services bij het indexeren. 

Deze stap slaan we nu even over en we gaan naar **Doelindex aanpassen**.

   ![Stap voor cognitieve vaardigheden overslaan](media/search-get-started-portal/skip-cog-skill-step.png)

> [!TIP]
> U kunt stap voor stap door een voorbeeld van AI-indexering in een [snelstartgids](cognitive-search-quickstart-blob.md) of [zelfstudie](cognitive-search-tutorial-blob.md).

### <a name="step-3---configure-index"></a>Stap 3: index configureren

Het maken van een index is meestal een op code gebaseerde oefening die is voltooid voordat gegevens worden geladen. Maar zoals deze zelfstudie laat zien, kan de wizard een basisindex genereren voor elke gegevensbron die kan worden verkend. Een index vereist minimaal een naam en een verzameling velden. Eén van de velden moet zijn gemarkeerd als de documentsleutel die elk document op unieke wijze identificeert. Bovendien kunt u taalanalyses of suggesters opgeven als u wilt dat automatisch aanvullen of het voorstellen van query's wordt gebruikt.

Velden bevatten gegevenstypen en kenmerken. De selectievakjes bovenaan zijn *indexkenmerken* die bepalen hoe het veld wordt gebruikt.

* **Ophalen mogelijk** betekent dat dit veld wordt weergegeven in de lijst met zoekresultaten. U kunt afzonderlijke velden als ontoegankelijk voor zoekresultaten markeren door dit selectievakje in, bijvoorbeeld voor velden die worden gebruikt alleen in filterexpressies uit te schakelen.
* **Sleutel** is de unieke id van het document. Het is altijd een tekenreeks en deze is vereist.
* Met de kenmerken **Filterbaar**, **Sorteerbaar** en **Geschikt voor facetten** bepaalt u of velden in een filter, een sorteervolgorde of een facetnavigatiestructuur worden gebruikt.
* **Doorzoekbaar** betekent dat een veld is opgenomen in een zoekopdracht in volledige tekst. Tekenreeksen zijn doorzoekbaar. Numerieke velden en Booleaanse waarden zijn vaak gemarkeerd als niet doorzoekbaar.

Opslagvereisten verschillen niet als gevolg van uw selectie. Als u bijvoorbeeld het kenmerk **Ophalen mogelijk** instelt voor meerdere velden, nemen de opslagvereisten niet toe.

Standaard wordt met de wizard de gegevensbron gescand op unieke id's als basis voor het sleutelveld. *Tekenreeksen* hebben de kenmerken **Ophaalbaar** en **Doorzoekbaar**. *Gehele getallen* hebben de kenmerken **Ophaalbaar**, **Filterbaar**, **Sorteerbaar** en **Geschikt voor facetten**.

1. Accepteer de standaardwaarden. 

   Als u de wizard opnieuw met behulp van een bestaande gegevensbron van hotels nogmaals uitvoeren, kan de index wordt niet worden geconfigureerd met standaardkenmerken. Voor toekomstige importbewerkingen moet u de kenmerken handmatig selecteren. 

   ![Index hotels gegenereerde](media/search-get-started-portal/hotelsindex.png)

2. Ga door naar de volgende pagina.

   ![Volgende pagina Indexeerfunctie maken](media/search-get-started-portal/next-button-create-indexer.png)

### <a name="step-4---configure-indexer"></a>Stap 4: indexeerfunctie configureren

Klik in de wizard**Gegevens importeren** op **Indexeerfunctie** > **Naam** en typ een naam voor de indexeerfunctie.

Dit object definieert een uitvoerbaar proces. U kunt een terugkerend schema instellen, maar in dit geval gebruikt u de standaardoptie om de indexeerfunctie één keer meteen uit te voeren.

Klik op **Verzenden** om de indexeerfunctie te maken en tegelijkertijd uit te voeren.

  ![Hotels indexeerfunctie](media/search-get-started-portal/hotels-indexer.png)

## <a name="monitor-progress"></a>Voortgang controleren

De wizard leidt u naar de lijst met indexeerfuncties waar u de voortgang kunt controleren. Voor zelfnavigatie gaat u naar de overzichtspagina en klikt u op **Indexeerfuncties**.

Het kan een paar minuten duren voordat in de portal de pagina is bijgewerkt, maar u zou de zojuist gemaakte indexeerfunctie in de lijst moeten zien met de status die aangeeft dat deze wordt uitgevoerd of is geslaagd en met het aantal geïndexeerde documenten.

   ![Voortgangsbericht voor de indexeerfunctie](media/search-get-started-portal/indexers-inprogress.png)

## <a name="view-the-index"></a>De index bekijken

De belangrijkste servicepagina bevat koppelingen naar de resources die in uw Azure Search-service zijn gemaakt.  Als u de index wilt bekijken die u zojuist hebt gemaakt, klikt u op **Indexen** in de lijst met koppelingen. 

   ![Lijst met indexen op het servicedashboard](media/search-get-started-portal/indexes-list.png)

In deze lijst, kunt u klikken op de *hotels-sample* index die u zojuist hebt gemaakt, het indexschema weergeven. en eventueel nieuwe velden toevoegen. 

Op het tabblad **Velden** wordt het indexschema getoond. Scroll naar de onderkant van de lijst om een nieuw veld in te voeren. In de meeste gevallen kunt u geen bestaande velden wijzigen. Bestaande velden hebben een fysieke weergave in Azure Search en kunnen daarom niet worden gewijzigd, zelfs niet in code. Als u een bestaand veld fundamenteel wilt wijzigen, maakt u een nieuwe index en verwijdert u het oorspronkelijke veld.

   ![voorbeeld van indexdefinitie](media/search-get-started-portal/sample-index-def.png)

Andere constructies, zoals scoreprofielen en CORS-opties, kunnen op elk gewenst moment worden toegevoegd.

Neem even de tijd voor het bekijken van de indexdefinitieopties om duidelijk te begrijpen wat u wel en niet kunt bewerken tijdens indexontwerp. Uitgegrijsde opties geven aan dat een waarde niet kan worden gewijzigd of verwijderd. 

## <a name="query-index"></a> Query die Search Explorer gebruikt

U zou nu een zoekindex moeten hebben die klaar is om query’s uit te voeren met de ingebouwde querypagina [**Search Explorer**](search-explorer.md). Deze pagina biedt een zoekvak waarmee u willekeurige queryreeksen kunt testen.

**Search Explorer** kan alleen omgaan met [REST API-aanvragen](https://docs.microsoft.com/rest/api/searchservice/search-documents), maar er wordt syntaxis geaccepteerd voor [eenvoudige querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) en [volledige Lucene-queryparser](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), plus alle zoekparameters die beschikbaar zijn in bewerkingen voor [Search-document-REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples).

> [!TIP]
> De volgende stappen worden na 6 min 8 sec gedemonstreerd in de [Azure Search-overzichtsvideo](https://channel9.msdn.com/Events/Connect/2016/138).
>

1. Klik op **Search explorer** in de opdrachtbalk.

   ![Opdracht Search Explorer](media/search-get-started-portal/search-explorer-cmd.png)

2. Uit de **Index** vervolgkeuzelijst kiezen *hotels-sample*. Klik op de **API-versie** vervolgkeuzelijst, om te zien welke REST-API's beschikbaar zijn. Gebruik voor onderstaande query's de algemeen beschikbare versie (2019-05-06).

   ![Index- en API-opdrachten](media/search-get-started-portal/search-explorer-changeindex.png)

3. Plak in de zoekbalk de onderstaande querytekenreeksen en klik op **Zoeken**.

   ![Querytekenreeks en knop Zoeken](media/search-get-started-portal/search-explorer-query-string-example.png)

## <a name="example-queries"></a>Voorbeelden van query's

U kunt termen en zinnen invoeren, vergelijkbaar met wat u in een zoekopdracht in Bing of Google doet, of u kunt volledig opgegeven query-expressies invoeren. Resultaten worden geretourneerd als uitgebreide JSON-documenten.

### <a name="simple-query-with-top-n-results"></a>Eenvoudige query met de bovenste N resultaten

#### <a name="example-string-query-searchspa"></a>Voorbeeld (tekenreeks): `search=spa`

* De **zoeken** parameter wordt gebruikt voor het invoeren van een zoekopdracht voor zoeken in volledige tekst, in dit geval Hotelgegevens geretourneerd voor degenen met *beveiligd-wachtwoordverificatie* in elk doorzoekbaar veld van het document.

* Met **Search Explorer** worden resultaten geretourneerd in JSON. Deze indeling is uitgebreid en moeilijk te lezen als documenten een compacte structuur hebben. Dit is opzettelijk; dat het hele document zichtbaar is, is belangrijk voor ontwikkeldoeleinden, vooral tijdens het testen. Voor een betere gebruikerservaring moet u code schrijven waarmee [zoekresultaten](search-pagination-page-layout.md) belangrijke elementen weergeven.

* Documenten bestaan uit alle velden die zijn gemarkeerd als ‘Ophalen mogelijk’ in de index. Als u wilt weergeven indexkenmerken in de portal, klikt u op *hotels-sample* in de **indexen** lijst.

#### <a name="example-parameterized-query-searchspacounttruetop10"></a>Voorbeeld (geparameteriseerde query): `search=spa&$count=true&$top=10`

* Het symbool **&** wordt gebruikt om zoekparameters toe te voegen. Deze kunnen in willekeurige volgorde worden opgegeven.

* De **$count = true** parameter retourneert het totale aantal geretourneerde documenten. Deze waarde verschijnt bovenaan de zoekresultaten. U kunt filterquery's controleren door de wijzigingen te controleren die door **$count=true** worden gerapporteerd. Lage aantallen geven aan dat uw filter werkt.

* De **$top = 10** retourneert de hoogst scorende documenten in 10 totaal. Standaard retourneert Azure Search de 50 beste resultaten. U kunt dit aantal vergroten of verkleinen via **$top**.

### <a name="filter-query"></a>De query filteren

Filters zijn opgenomen in de zoekopdrachten wanneer u de parameter **$filter** toevoegt. 

#### <a name="example-filtered-searchbeachfilterrating-gt-4"></a>Voorbeeld (gefilterd): `search=beach&$filter=Rating gt 4`

* De parameter **$filter** retourneert resultaten die voldoen aan de criteria die u hebt opgegeven. In dit geval classificaties groter is dan 4.

* Filtersyntaxis is een OData-constructie. Zie [OData-syntaxis filteren](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) voor meer informatie.

### <a name="facet-query"></a>De query facetteren

Facetfilters zijn opgenomen in zoekopdrachten. U kunt de facetparameter gebruiken om een samengevoegd aantal documenten te retourneren die overeenkomen met een door u opgegeven facetwaarde.

#### <a name="example-faceted-with-scope-reduction-searchfacetcategorytop2"></a>Voorbeeld (gefacetteerd met bereikvermindering): `search=*&facet=Category&$top=2`

* **search=** * is een lege zoekopdracht. Met een lege zoekopdracht wordt naar alles gezocht. Eén reden om een lege query in te dienen, is om de hele set documenten te filteren of als facet te gebruiken. U wilt bijvoorbeeld een facetnavigatiestructuur die bestaat uit alle hotels in de index.
* **facet** retourneert een navigatiestructuur die u kunt doorgeven aan een UI-besturingselement. Deze retourneert categorieën en een aantal. In dit geval zijn categorieën gebaseerd op een veld met de naam gemakkelijk *categorie*. Er is geen aggregatie in Azure Search, maar u kunt een geschatte aggregatie bepalen via `facet`, dat het aantal documenten in elke categorie retourneert.

* **$top=2** retourneert twee documenten, om te illustreren dat u `top` kunt gebruiken om het aantal resultaten te verlagen of te verhogen.

#### <a name="example-facet-on-numeric-values-searchspafacetrating"></a>Voorbeeld (facet op basis van numerieke waarden): `search=spa&facet=Rating`

* Deze query is facet voor classificatie, op een tekstuele zoekopdracht naar *beveiligd-wachtwoordverificatie*. De term *waardering* kan worden opgegeven als een facet, omdat het veld is gemarkeerd als ophaalbaar, Filterbaar en geschikt voor facetten in de index en de waarden die het bevat (numeriek, 1 tot en met 5), zijn geschikt voor het categoriseren van vermeldingen in groepen.

* Alleen filterbare velden kunnen als facet worden gebruikt. Alleen ophaalbare velden kunnen in de resultaten worden geretourneerd.

* De *waardering* veld is de drijvende komma dubbele precisie en de groepering is door de exacte waarde. Voor meer informatie over groeperen per interval (bijvoorbeeld: "3 sterwaarderingen," '4 sterwaarderingen', enzovoort), Zie [facetnavigatie implementeren in Azure Search](https://docs.microsoft.com/azure/search/search-faceted-navigation#filter-based-on-a-range).


### <a name="highlight-query"></a> Zoekresultaten markeren

Markeren betekent het toevoegen van opmaak aan tekst die overeenkomt met het trefwoord, overeenkomsten die in een bepaald veld zijn aangetroffen. Als de zoekterm verborgen is in een beschrijving, kunt u de treffers markeren om deze makkelijker te vinden.

#### <a name="example-highlighter-searchbeachhighlightdescription"></a>Voorbeeld (gemarkeerd): `search=beach&highlight=Description`

* In dit voorbeeld wordt het opgemaakte woord *strand* gemakkelijker te zien is in het omschrijvingsveld.

#### <a name="example-linguistic-analysis-searchbeacheshighlightdescription"></a>Voorbeeld (taalkundige analyse): `search=beaches&highlight=Description`

* Zoeken in volledige tekst herkent basic variaties in woorden. In dit geval bevatten zoekresultaten gemarkeerde tekst 'strand"hotels die die dit woord in hun doorzoekbare velden, in reactie op een zoekopdracht op 'stranden hebben'. De resultaten kunnen verschillende vormen van hetzelfde woord bevatten vanwege taalkundige analyse. 

* Azure Search ondersteunt 56 analyzers van Lucene en Microsoft. Standaard wordt Lucene Analyzer gebruikt voor Azure Search.

### <a name="fuzzy-search"></a> Fuzzy zoekopdrachten uitproberen

Standaard gespeld querytermen, zoals *seatle* voor "Seattle", geen overeenkomsten in standaardzoekopdrachten retourneren. Het volgende voorbeeld retourneert geen resultaten.

#### <a name="example-misspelled-term-unhandled-searchseatle"></a>Voorbeeld (verkeerd gespelde term, niet verwerkt): `search=seatle`

U kunt fuzzy zoekopdrachten gebruiken om spelfouten te omzeilen. Fuzzy zoekopdrachten worden ingeschakeld wanneer u de volledige Lucene-querysyntaxis gebruikt, wat gebeurt wanneer u **queryType=full** voor de query instelt en de **~** aan de zoekreeks toevoegt.

#### <a name="example-misspelled-term-handled-searchseatlequerytypefull"></a>Voorbeeld (verkeerd gespelde term, verwerkt): `search=seatle~&queryType=full`

In dit voorbeeld retourneert nu documenten die overeenkomsten op "Seattle" bevatten.

Als **queryType** niet is opgegeven, wordt standaard de eenvoudige queryparser gebruikt. De eenvoudige queryparser is sneller, maar als u gebruik wilt maken van fuzzy zoeken, reguliere expressies, zoeken op nabijheid of andere geavanceerde typen query's, dan hebt u de volledige syntaxis nodig.

Fuzzy zoekopdrachten en zoekopdrachten met jokertekens hebben implicaties voor zoekresultaten. Taalkundige analyse wordt niet uitgevoerd op deze queryindelingen. Bekijk in [Hoe zoeken in volledige tekst werkt in Azure Search](search-lucene-query-architecture.md#stage-2-lexical-analysis) de sectie over uitzonderingen op lexicale analyse voordat u fuzzy zoekopdrachten en zoekopdrachten met jokertekens gebruikt.

Zie [Lucene-querysyntaxis in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) voor meer informatie over queryscenario's op basis van de volledige queryparser.

### <a name="geo-search"></a> Georuimtelijk zoeken uitproberen

Georuimtelijk zoeken wordt ondersteund door het [gegevenstype edm.GeographyPoint](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) voor een veld met coördinaten. Geosearch is een type filter dat wordt opgegeven bij [Filter OData-syntaxis](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-12212-4767-le-5"></a>Voorbeeld (filters voor geografische coördinaten): `search=*&$count=true&$filter=geo.distance(Location,geography'POINT(-122.12 47.67)') le 5`

Met dit voorbeeld worden alle resultaten voor positionele gegevens gefilterd die minder dan 5 kilometer zijn verwijderd van een opgegeven punt (opgegeven als coördinaten voor lengte- en breedtegraad). Door **$count** toe te voegen, kunt u zien hoeveel resultaten er worden geretourneerd als u de afstand of de coördinaten wijzigt.

Georuimtelijk zoeken is handig als uw zoektoepassing een functie 'in mijn buurt zoeken' heeft of gebruikmaakt van kaartnavigatie. Dit is echter niet een zoekopdracht in volledige tekst. Als u gebruikersvereisten voor zoeken op een plaats of een land/regio met de naam, voegt u velden met namen van stad of land/regio, naast coördinaten toe.

## <a name="takeaways"></a>Opgedane kennis

Deze zelfstudie biedt een snelle introductie tot het gebruik van Azure Search via Azure Portal.

U hebt geleerd hoe u een zoekindex maakt met de wizard **Gegevens importeren**. U hebt geleerd over [indexeerfuncties](search-indexer-overview.md), en over de basiswerkstroom voor indexontwerp, waaronder [ondersteunde wijzigingen in een gepubliceerde index](https://docs.microsoft.com/rest/api/searchservice/update-index).

Met behulp van de **Search Explorer** in de Azure Portal hebt u wat querysyntaxis geleerd via praktijkvoorbeelden die belangrijke mogelijkheden hebben gedemonstreerd, zoals filters, resultaatmarkering, fuzzy zoekopdrachten en op geografische locaties zoeken.

U hebt ook geleerd hoe u in de portal kunt zoeken naar indexen, indexeerfuncties en gegevensbronnen. Met elke nieuwe gegevensbron in de toekomst kunt u de portal gebruiken om met een minimale inspanning snel de definities of de veldverzamelingen ervan te controleren.

## <a name="clean-up"></a>Opruimen

Wanneer u in uw eigen abonnement werkt, is het een goed idee aan het einde van een project om te bepalen of u moet nog steeds de resources die dat u hebt gemaakt. Resources naar links wordt uitgevoerd kan kosten u geld. U kunt afzonderlijke resources verwijderen of verwijder de resourcegroep als u wilt verwijderen van de volledige set van resources.

U kunt zoeken en beheren van resources in de portal, met behulp van de **alle resources** of **resourcegroepen** koppeling in het deelvenster navigatie aan de linkerkant.

Als u van een gratis service gebruikmaakt, houd er rekening mee dat u beperkt tot drie indexen, Indexeerfuncties en gegevensbronnen bent. U kunt afzonderlijke items in de portal om te blijven onder de limiet verwijderen. 

## <a name="next-steps"></a>Volgende stappen

U kunt meer aspecten van Azure Search verkennen met behulp van de programmatische hulpprogramma's:

* [Een index maken met behulp van de .NET-SDK](https://docs.microsoft.com/azure/search/search-create-index-dotnet)
* [Een index maken met behulp van REST API's](https://docs.microsoft.com/azure/search/search-create-index-rest-api)
* [Een index maken met behulp van Postman of Fiddler en de Azure Search REST API's](search-get-started-postman.md)
