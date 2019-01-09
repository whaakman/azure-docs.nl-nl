---
title: Snelstartzelfstudie indexeren en query's uitvoeren met Azure Portal - Azure Search
description: In deze snelstartzelfstudie gebruikt u Azure Portal en ingebouwde voorbeeldgegevens om een index te genereren in Azure Search. Probeer zoekopdrachten in volledige tekst, filters, facetten, fuzzy zoekopdrachten, geosearch en meer.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 3f75cd61d948f3f6df34124a9b16b333f6c5e6d5
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54001784"
---
# <a name="quickstart-use-built-in-portal-tools-for-azure-search-import-indexing-and-queries"></a>Quickstart: Ingebouwde portalprogramma's gebruiken voor importeren, indexeren en query's uitvoeren in Azure Search

Probeer de ingebouwde hulpprogramma's in Azure Portal als u snel vertrouwd wilt raken met de concepten van Azure Search. Wizards en editors bieden geen volledige pariteit met de .NET- en REST API's maar u kunt snel aan de slag met een inleiding die u laat zien hoe u zonder code binnen een paar minuten interessante query's kunt schrijven met behulp van voorbeeldgegevens.

> [!div class="checklist"]
> * Begin met een gratis openbare op Azure gehoste set voorbeeldgegevens
> * Voer de wizard **Gegevens importeren** in Azure Search uit om gegevens te laden en een index te genereren
> * Bewaak de voortgang van de indexering in de portal
> * Bekijk een bestaande index en opties om deze te wijzigen
> * Probeer zoekopdrachten in volledige tekst, filters, facetten, fuzzy zoekopdrachten en geosearch met **Search Explorer**

Als de hulpprogramma’s te beperkend zijn, kunt u een [op code gebaseerde inleiding tot het programmeren van Azure Search in .NET](search-howto-dotnet-sdk.md) of [Postman of Fiddler gebruiken voor het maken van REST-API-aanroepen](search-fiddler.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. U kunt ook een 6 minuten durende demonstratie bekijken van de stappen in deze zelfstudie. De demonstratie begint na ongeveer drie minuten in deze [Azure Search-overzichtsvideo](https://channel9.msdn.com/Events/Connect/2016/138).

## <a name="prerequisites"></a>Vereisten

[Maak een Azure Search-service](search-create-service-portal.md) of zoek een bestaande service onder uw huidige abonnement.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open het servicedashboard van uw Azure Search-service. Als u de servicetegel niet hebt vastgemaakt aan het dashboard, kunt u de service op de volgende manier vinden:

   * Klik in de snelbalk in het linkernavigatiedeelvenster op **Alle services**.
   * Typ in het zoekvak het woord *zoeken* voor een lijst met zoekservices voor uw abonnement. Klik op **Services zoeken**. Als het goed is, wordt de service nu weergegeven in de lijst.

### <a name="check-for-space"></a>Controleren of er voldoende ruimte is

Veel klanten beginnen met de gratis service. Deze versie is beperkt tot drie indexen drie gegevensbronnen en drie indexeerfuncties. Zorg ervoor dat er voldoende ruimte is voor extra items voordat u begint. In deze zelfstudie wordt van elk object één exemplaar gemaakt.

Op secties in het servicedashboard wordt weergegeven hoeveel indexen, indexeerfuncties en gegevensbronnen u al hebt. 

![Lijsten met indexen, indexeerfuncties en gegevensbronnen][media/search-get-started-portal/tiles-indexers-datasources2.png]

## <a name="create-index"></a> Een index maken en gegevens laden

Zoekopdrachten worden herhaald over een [*index*](search-what-is-an-index.md) die doorzoekbare gegevens, metagegevens en aanvullende constructies bevat, die worden gebruikt om bepaald zoekgedrag te optimaliseren.

Voor deze zelfstudie gebruiken we een ingebouwde gegevensset die met behulp van een [*indexeerfunctie*](search-indexer-overview.md) kan worden verkend via de wizard **Gegevens importeren**. Een indexeerfunctie is een bronspecifieke verkenner die metagegevens en inhoud uit ondersteunde Azure-gegevensbronnen kan lezen. Normaal gesproken worden indexeerfuncties via een programma gebruikt, maar in de portal zijn ze toegankelijk via de wizard **Gegevens importeren**. 

### <a name="step-1---start-the-import-data-wizard-and-create-a-data-source"></a>Stap 1: de wizard Gegevens importeren starten en een gegevensbron maken

1. Klik in het dashboard van de Azure Search-service op **Gegevens importeren** op de opdrachtbalk om een zoekindex te maken en deze te vullen met gegevens.

   ![Opdracht Gegevens importeren](media/search-get-started-portal/import-data-cmd2.png)

2. Klik in de wizard op **Verbinding maken met uw gegevens** > **Voorbeelden** > **realestate-us-sample**. Deze gegevensbron is ingebouwd. Als u uw eigen gegevensbron zou maken, zou u een naam, het type en de verbindingsgegevens moeten opgeven. Zodra de gegevensbron is gemaakt, wordt deze een bestaande gegevensbron genoemd die opnieuw kan worden gebruikt voor andere bewerkingen.

   ![Voorbeeldgegevensset selecteren](media/search-get-started-portal/import-datasource-sample2.png)

3. Ga door naar de volgende pagina.

   ![De knop volgende pagina voor Cognitive Search](media/search-get-started-portal/next-button-add-cog-search.png)

### <a name="step-2---skip-cognitive-skills"></a>Stap 2: cognitieve vaardigheden overslaan

De wizard ondersteunt het maken van een [pijplijn voor cognitieve vaardigheden](cognitive-search-concept-intro.md) voor het opnemen van de AI-algoritmen van Cognitive Services bij het indexeren. 

Deze stap slaan we nu even over en we gaan naar **Doelindex aanpassen**.

   ![Stap voor cognitieve vaardigheden overslaan](media/search-get-started-portal/skip-cog-skill-step.png)

> [!TIP]
> U kunt de nieuwe previewfunctie van cognitief zoeken voor Azure Search proberen via de [Snelstart](cognitive-search-quickstart-blob.md) of [zelfstudie over cognitief zoeken](cognitive-search-tutorial-blob.md).

### <a name="step-3---configure-index"></a>Stap 3: index configureren

Het maken van een index is meestal een op code gebaseerde oefening die is voltooid voordat gegevens worden geladen. Maar zoals deze zelfstudie laat zien, kan de wizard een basisindex genereren voor elke gegevensbron die kan worden verkend. Een index vereist minimaal een naam en een verzameling velden. Eén van de velden moet zijn gemarkeerd als de documentsleutel die elk document op unieke wijze identificeert. Bovendien kunt u taalanalyses of suggesters opgeven als u wilt dat automatisch aanvullen of het voorstellen van query's wordt gebruikt.

Velden bevatten gegevenstypen en kenmerken. De selectievakjes bovenaan zijn *indexkenmerken* die bepalen hoe het veld wordt gebruikt.

* **Ophalen mogelijk** betekent dat dit veld wordt weergegeven in de lijst met zoekresultaten. U kunt afzonderlijke velden markeren als ontoegankelijk voor zoekresultaten door dit selectievakje uit te schakelen, bijvoorbeeld voor velden die alleen in filterexpressies worden gebruikt.
* **Sleutel** is de unieke id van het document. Het is altijd een tekenreeks en deze is vereist.
* Met de kenmerken **Filterbaar**, **Sorteerbaar** en **Geschikt voor facetten** bepaalt u of velden in een filter, een sorteervolgorde of een facetnavigatiestructuur worden gebruikt.
* **Doorzoekbaar** betekent dat een veld is opgenomen in een zoekopdracht in volledige tekst. Tekenreeksen zijn doorzoekbaar. Numerieke velden en Booleaanse waarden zijn vaak gemarkeerd als niet doorzoekbaar.

Opslagvereisten verschillen niet als gevolg van uw selectie. Als u bijvoorbeeld het kenmerk **Ophalen mogelijk** instelt voor meerdere velden, nemen de opslagvereisten niet toe.

Standaard wordt met de wizard de gegevensbron gescand op unieke id's als basis voor het sleutelveld. Tekenreeksen hebben de kenmerken Ophaalbaar en Doorzoekbaar. Gehele getallen hebben de kenmerken Ophaalbaar, Filterbaar, Sorteerbaar en Geschikt voor facetten.

1. Accepteer alle standaardwaarden.

  ![Gegenereerde onroerend goed-index](media/search-get-started-portal/realestateindex2.png)

2. Ga door naar de volgende pagina.

  ![Volgende pagina Indexeerfunctie maken](media/search-get-started-portal/next-button-create-indexer.png)

### <a name="step-4---configure-indexer"></a>Stap 4: indexeerfunctie configureren

Klik in de wizard**Gegevens importeren** op **Indexeerfunctie** > **Naam** en typ een naam voor de indexeerfunctie.

Dit object definieert een uitvoerbaar proces. U kunt een terugkerend schema instellen, maar in dit geval gebruikt u de standaardoptie om de indexeerfunctie één keer meteen uit te voeren.

Klik op **Verzenden** om de indexeerfunctie te maken en tegelijkertijd uit te voeren.

  ![indexeerfunctie voor onroerend goed](media/search-get-started-portal/realestate-indexer2.png)

## <a name="monitor-progress"></a>Voortgang controleren

De wizard leidt u naar de lijst met indexeerfuncties waar u de voortgang kunt controleren. Voor zelfnavigatie gaat u naar de overzichtspagina en klikt u op **Indexeerfuncties**.

Het kan een paar minuten duren voordat in de portal de pagina is bijgewerkt, maar u zou de zojuist gemaakte indexeerfunctie in de lijst moeten zien met de status die aangeeft dat deze wordt uitgevoerd of is geslaagd en met het aantal geïndexeerde documenten.

   ![Voortgangsbericht voor de indexeerfunctie](media/search-get-started-portal/indexers-inprogress2.png)

## <a name="view-the-index"></a>De index bekijken

In de lijst **Indexen** zijn de bestaande indexen vermeld, waaronder de index *realestate-us-sample* die u zojuist met de wizard hebt gemaakt.

Vanuit deze lijst kunt u het indexschema weergeven en desgewenst nieuwe velden toevoegen, maar u kunt bestaande velden niet wijzigen. Bestaande velden hebben een fysieke weergave in Azure Search en kunnen daarom niet worden gewijzigd, zelfs niet in code. Als u een bestaand veld fundamenteel wilt wijzigen, maakt u een nieuwe index en verwijdert u het oorspronkelijke veld.

   ![voorbeeld van indexdefinitie](media/search-get-started-portal/sample-index-def.png)

Andere constructies, zoals scoreprofielen en CORS-opties, kunnen op elk gewenst moment worden toegevoegd.

Neem even de tijd voor het bekijken van de indexdefinitieopties om duidelijk te begrijpen wat u wel en niet kunt bewerken tijdens indexontwerp. Uitgegrijsde opties geven aan dat een waarde niet kan worden gewijzigd of verwijderd. 

## <a name="query-index"></a> Query die Search Explorer gebruikt

U zou nu een zoekindex moeten hebben die klaar is om query’s uit te voeren met de ingebouwde querypagina [**Search Explorer**](search-explorer.md). Deze pagina biedt een zoekvak waarmee u willekeurige queryreeksen kunt testen.

> [!TIP]
> De volgende stappen worden na 6 min 8 sec gedemonstreerd in de [Azure Search-overzichtsvideo](https://channel9.msdn.com/Events/Connect/2016/138).
>

1. Klik op **Search explorer** in de opdrachtbalk.

   ![Opdracht Search Explorer](media/search-get-started-portal/search-explorer-cmd2.png)

2. Klik in de opdrachtbalk op **Index wijzigen** om te schakelen naar *realestate-us-sample*. Klik in de opdrachtbalk op **API-versie instellen** om te zien welke REST API's beschikbaar zijn. Gebruik voor onderstaande query’s de algemeen beschikbare versie (11-11-2017).

   ![Index- en API-opdrachten](media/search-get-started-portal/search-explorer-changeindex-se2.png)

3. Voer in de zoekbalk de onderstaande querytekenreeksen in en klik op **Zoeken**.

    > [!NOTE]
    > **Search Explorer** is alleen uitgerust om [REST API-aanvragen](https://docs.microsoft.com/rest/api/searchservice/search-documents) te verwerken. Er wordt syntaxis geaccepteerd voor [eenvoudige querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) en [volledige Lucene-queryparser](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), plus alle zoekparameters die beschikbaar zijn in bewerkingen voor [Document doorzoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents).
    >

## <a name="example-queries"></a>Voorbeelden van query's

U kunt termen en zinnen invoeren, vergelijkbaar met wat u in een zoekopdracht in Bing of Google doet, of u kunt volledig opgegeven query-expressies invoeren. Resultaten worden geretourneerd als uitgebreide JSON-documenten.

### <a name="simple-query-with-top-n-results"></a>Eenvoudige query met de bovenste N resultaten

#### <a name="example-string-query-searchseattle"></a>Voorbeeld (tekenreeks): `search=seattle`

* De parameter **search** wordt gebruikt om een zoekopdracht voor zoeken in volledige tekst in te voeren waarmee, in dit geval, vermeldingen in King County, Washington state worden geretourneerd die *Seattle* bevatten in elk doorzoekbaar veld van het document.

* Met **Search Explorer** worden resultaten geretourneerd in JSON. Deze indeling is uitgebreid en moeilijk te lezen als documenten een compacte structuur hebben. Dit is opzettelijk; dat het hele document zichtbaar is, is belangrijk voor ontwikkeldoeleinden, vooral tijdens het testen. Voor een betere gebruikerservaring moet u code schrijven waarmee [zoekresultaten](search-pagination-page-layout.md) belangrijke elementen weergeven.

* Documenten bestaan uit alle velden die zijn gemarkeerd als ‘Ophalen mogelijk’ in de index. Als u in de portal indexkenmerken wilt weergeven, gaat u naar de lijst **Indexen** en klikt u op *realestate-us-sample*.

#### <a name="example-parameterized-query-searchseattlecounttruetop100"></a>Voorbeeld (geparameteriseerde query): `search=seattle&$count=true&$top=100`

* Het symbool **&** wordt gebruikt om zoekparameters toe te voegen. Deze kunnen in willekeurige volgorde worden opgegeven.

* De parameter **$count=true** retourneert het totale aantal geretourneerde documenten. Deze waarde verschijnt bovenaan de zoekresultaten. U kunt filterquery's controleren door de wijzigingen te controleren die door **$count=true** worden gerapporteerd. Lage aantallen geven aan dat uw filter werkt.

* De **$top=100** retourneert de 100 hoogst scorende documenten in dit totaal. Standaard retourneert Azure Search de 50 beste resultaten. U kunt dit aantal vergroten of verkleinen via **$top**.

### <a name="filter-query"></a>De query filteren

Filters zijn opgenomen in de zoekopdrachten wanneer u de parameter **$filter** toevoegt. 

#### <a name="example-filtered-searchseattlefilterbeds-gt-3"></a>Voorbeeld (gefilterd): `search=seattle&$filter=beds gt 3`

* De parameter **$filter** retourneert resultaten die voldoen aan de criteria die u hebt opgegeven. In dit geval: meer dan 3 slaapkamers.

* Filtersyntaxis is een OData-constructie. Zie [OData-syntaxis filteren](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) voor meer informatie.

### <a name="facet-query"></a>De query facetteren

Facetfilters zijn opgenomen in zoekopdrachten. U kunt de facetparameter gebruiken om een samengevoegd aantal documenten te retourneren die overeenkomen met een door u opgegeven facetwaarde.

#### <a name="example-faceted-with-scope-reduction-searchfacetcitytop2"></a>Voorbeeld (gefacetteerd met bereikvermindering): `search=*&facet=city&$top=2`

* **search=*** is een lege zoekopdracht. Met een lege zoekopdracht wordt naar alles gezocht. Eén reden om een lege query in te dienen, is om de hele set documenten te filteren of als facet te gebruiken. U wilt bijvoorbeeld een facetnavigatiestructuur die bestaat uit alle plaatsen in de index.

* **facet** retourneert een navigatiestructuur die u kunt doorgeven aan een UI-besturingselement. Deze retourneert categorieën en een aantal. In dit geval zijn categorieën gebaseerd op het aantal plaatsen. Er is geen aggregatie in Azure Search, maar u kunt een geschatte aggregatie bepalen via `facet`, dat het aantal documenten in elke categorie retourneert.

* **$top=2** retourneert twee documenten, om te illustreren dat u `top` kunt gebruiken om het aantal resultaten te verlagen of te verhogen.

#### <a name="example-facet-on-numeric-values-searchseattlefacetbeds"></a>Voorbeeld (gefacetteerd op numerieke waarden): `search=seattle&facet=beds`**

* Deze query gebruikt het facet bedden in een tekstuele zoekopdracht naar *Seattle*. De term *beds* kan worden opgegeven als een facet, omdat het veld in de index is gemarkeerd als ophaalbaar, filterbaar en bruikbaar als facet. En de waarden die het bevat (numeriek, 1 tot en met 5), zijn geschikt voor het categoriseren van vermeldingen in groepen (vermeldingen met 3 slaapkamers, 4 slaapkamers).

* Alleen filterbare velden kunnen als facet worden gebruikt. Alleen ophaalbare velden kunnen in de resultaten worden geretourneerd.

### <a name="highlight-query"></a> Zoekresultaten markeren

Markeren betekent het toevoegen van opmaak aan tekst die overeenkomt met het trefwoord, overeenkomsten die in een bepaald veld zijn aangetroffen. Als de zoekterm verborgen is in een beschrijving, kunt u de treffers markeren om deze makkelijker te vinden.

#### <a name="example-highlighter-searchgranite-countertopshighlightdescription"></a>Voorbeeld (gemarkeerd): `search=granite countertops&highlight=description`

* In dit voorbeeld is de opgemaakte woordgroep *granieten bladen* gemakkelijker te zien in het omschrijvingsveld.

#### <a name="example-linguistic-analysis-searchmicehighlightdescription"></a>Voorbeeld (taalkundige analyse): `search=mice&highlight=description`

* Met zoekopdrachten in de volledige tekst kunnen woorden met vergelijkbare semantiek worden gevonden. In dit geval bevatten zoekresultaten gemarkeerde tekst voor ‘muis’, voor woningen die een muizenplaag hebben, in reactie op een zoekopdracht naar ‘muis’. De resultaten kunnen verschillende vormen van hetzelfde woord bevatten vanwege taalkundige analyse.

* Azure Search ondersteunt 56 analyzers van Lucene en Microsoft. Standaard wordt Lucene Analyzer gebruikt voor Azure Search.

### <a name="fuzzy-search"></a> Fuzzy zoekopdrachten uitproberen

Voor verkeerd gespelde zoektermen, zoals *samamish* voor het Sammamish-plateau in de regio Seattle, worden bij standaardzoekopdrachten geen overeenkomsten geretourneerd. Het volgende voorbeeld retourneert geen resultaten.

#### <a name="example-misspelled-term-unhandled-searchsamamish"></a>Voorbeeld (verkeerd gespelde term, niet verwerkt): `search=samamish`

U kunt fuzzy zoekopdrachten gebruiken om spelfouten te omzeilen. Fuzzy zoekopdrachten worden ingeschakeld wanneer u de volledige Lucene-querysyntaxis gebruikt, wat gebeurt wanneer u **queryType=full** voor de query instelt en de **~** aan de zoekreeks toevoegt.

#### <a name="example-misspelled-term-handled-searchsamamishquerytypefull"></a>Voorbeeld (verkeerd gespelde term, verwerkt): `search=samamish~&queryType=full`

Dit voorbeeld retourneert nu documenten die overeenkomsten met ‘Sammamish’ bevatten.

Als **queryType** niet is opgegeven, wordt standaard de eenvoudige queryparser gebruikt. De eenvoudige queryparser is sneller, maar als u gebruik wilt maken van fuzzy zoeken, reguliere expressies, zoeken op nabijheid of andere geavanceerde typen query's, dan hebt u de volledige syntaxis nodig.

Fuzzy zoekopdrachten en zoekopdrachten met jokertekens hebben implicaties voor zoekresultaten. Taalkundige analyse wordt niet uitgevoerd op deze queryindelingen. Bekijk in [Hoe zoeken in volledige tekst werkt in Azure Search](search-lucene-query-architecture.md#stage-2-lexical-analysis) de sectie over uitzonderingen op lexicale analyse voordat u fuzzy zoekopdrachten en zoekopdrachten met jokertekens gebruikt.

Zie [Lucene-querysyntaxis in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) voor meer informatie over queryscenario's op basis van de volledige queryparser.

### <a name="geo-search"></a> Georuimtelijk zoeken uitproberen

Georuimtelijk zoeken wordt ondersteund door het [gegevenstype edm.GeographyPoint](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) voor een veld met coördinaten. Geosearch is een type filter dat wordt opgegeven bij [Filter OData-syntaxis](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-122121513-47673988-le-5"></a>Voorbeeld (filters voor geografische coördinaten): `search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`

Met dit voorbeeld worden alle resultaten voor positionele gegevens gefilterd die minder dan 5 kilometer zijn verwijderd van een opgegeven punt (opgegeven als coördinaten voor lengte- en breedtegraad). Door **$count** toe te voegen, kunt u zien hoeveel resultaten er worden geretourneerd als u de afstand of de coördinaten wijzigt.

Georuimtelijk zoeken is handig als uw zoektoepassing een functie 'in mijn buurt zoeken' heeft of gebruikmaakt van kaartnavigatie. Dit is echter niet een zoekopdracht in volledige tekst. Als uw gebruikers op naam naar steden of landen willen zoeken, voegt u, naast coördinaten, ook velden met namen van steden of landen toe.

## <a name="takeaways"></a>Opgedane kennis

Deze zelfstudie biedt een snelle introductie tot het gebruik van Azure Search via Azure Portal.

U hebt geleerd hoe u een zoekindex maakt met de wizard **Gegevens importeren**. U hebt geleerd over [indexeerfuncties](search-indexer-overview.md), en over de basiswerkstroom voor indexontwerp, waaronder [ondersteunde wijzigingen in een gepubliceerde index](https://docs.microsoft.com/rest/api/searchservice/update-index).

Met behulp van de **Search Explorer** in de Azure Portal hebt u wat querysyntaxis geleerd via praktijkvoorbeelden die belangrijke mogelijkheden hebben gedemonstreerd, zoals filters, resultaatmarkering, fuzzy zoekopdrachten en op geografische locaties zoeken.

U hebt ook geleerd hoe u in de portal kunt zoeken naar indexen, indexeerfuncties en gegevensbronnen. Met elke nieuwe gegevensbron in de toekomst kunt u de portal gebruiken om met een minimale inspanning snel de definities of de veldverzamelingen ervan te controleren.

## <a name="clean-up"></a>Opruimen

Als deze zelfstudie uw eerste gebruik van de Azure Search-service was, verwijdert u de resourcegroep die de Azure Search-service bevat. Als dit niet het geval is, zoekt u de naam van de juiste resourcegroep op in de lijst met services en verwijdert u de betreffende groep.

## <a name="next-steps"></a>Volgende stappen

U kunt meer aspecten van Azure Search verkennen met behulp van de programmatische hulpprogramma's:

* [Een index maken met behulp van de .NET-SDK](https://docs.microsoft.com/azure/search/search-create-index-dotnet)
* [Een index maken met behulp van REST API's](https://docs.microsoft.com/azure/search/search-create-index-rest-api)
* [Een index maken met behulp van Postman of Fiddler en de Azure Search REST API's](search-fiddler.md)