---
title: Zelfstudie over indexeren, query's uitvoeren en filteren in Azure Search via de portal | Microsoft Docs
description: In deze zelfstudie gebruikt u de Azure-portal en vooraf gedefinieerde voorbeeldgegevens om een index te genereren in Azure Search. Probeer zoekopdrachten in volledige tekst, filters, facetten, fuzzy zoekopdrachten, geosearch en meer.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 124963359d0b2d4050156958de195e47b9331c92
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007981"
---
# <a name="tutorial-use-built-in-tools-for-azure-search-indexing-and-queries"></a>Zelfstudie: Ingebouwde hulpprogramma’s gebruiken voor indexeren en query’s uitvoeren in Azure Search

Op een Azure Search-servicepagina in de Azure-portal kunt u ingebouwde hulpprogramma’s gebruiken om concepten te testen en praktijkervaring op te doen met minimale moeite. Portalhulpprogramma’s bieden geen volledige pariteit met .NET- en REST-API’s, maar de wizards en editors bieden een eenvoudige manier om proof-of-concepts snel te testen. Met deze inleiding zonder code kunt u aan de slag met een kleine verzameling gepubliceerde gegevens, zodat u direct interessante query's kunt schrijven. 

> [!div class="checklist"]
> * Begin met openbare voorbeeldgegevens en genereer automatisch een Azure Search-index met de wizard **Gegevens importeren**. 
> * Bekijk het schema en de kenmerken voor elke index die naar Azure Search wordt gepubliceerd.
> * Probeer zoekopdrachten in volledige tekst, filters, facetten, fuzzy zoekopdrachten en geosearch met **Search Explorer**.  

Portalhulpprogramma’s ondersteunen niet de volledige reeks Azure Search-mogelijkheden. Als de hulpprogramma’s te beperkend zijn, kunt u een [op code gebaseerde inleiding tot het programmeren van Azure Search in .NET](search-howto-dotnet-sdk.md) of [online testprogramma’s voor het maken van REST-API-aanroepen](search-fiddler.md) overwegen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. U kunt ook een 6 minuten durende demonstratie bekijken van de stappen in deze zelfstudie. De demonstratie begint na ongeveer drie minuten in deze [Azure Search-overzichtsvideo](https://channel9.msdn.com/Events/Connect/2016/138).

## <a name="prerequisites"></a>Vereisten

[Maak een Azure Search-service](search-create-service-portal.md) of zoek een bestaande service onder uw huidige abonnement. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open het servicedashboard van uw Azure Search-service. Als u de servicetegel niet hebt vastgemaakt aan het dashboard, kunt u de service op de volgende manier vinden: 
   
   * Klik in de snelbalk in het linkernavigatiedeelvenster op **Alle services**.
   * Typ in het zoekvak het woord *zoeken* voor een lijst met zoekservices voor uw abonnement. Klik op **Services zoeken**. Als het goed is, wordt de service nu weergegeven in de lijst. 

### <a name="check-for-space"></a>Controleren of er voldoende ruimte is
Veel klanten beginnen met de gratis service. Deze versie is beperkt tot drie indexen drie gegevensbronnen en drie indexeerfuncties. Zorg ervoor dat er voldoende ruimte is voor extra items voordat u begint. In deze zelfstudie wordt van elk object één exemplaar gemaakt. 

> [!TIP] 
> Op tegels in het servicedashboard wordt weergegeven hoeveel indexen, indexeerfuncties en gegevensbronnen u al hebt. De tegel Indexeerfunctie bevat indicatoren voor slagen en voor mislukken. Klik op de tegel om het aantal indexeerfuncties te bekijken. 
>
> ![Tegels voor indexeerfuncties en gegevensbronnen][1]
>

## <a name="create-index"></a> Een index maken en gegevens laden
Zoekopdrachten worden over een [*index*](search-what-is-an-index.md) herhaald met doorzoekbare gegevens, metagegevens en constructies, die worden gebruikt om bepaald zoekgedrag te optimaliseren.

We gebruiken een ingebouwde voorbeeldgegevensset die met behulp van een [*indexeerfunctie*](search-indexer-overview.md) kan worden verkend via de wizard **Gegevens importeren**, om deze taak portal-gebaseerd te houden. Een indexeerfunctie is een bronspecifieke verkenner die metagegevens en inhoud uit ondersteunde Azure-gegevensbronnen kan lezen. In code kunt u indexeerfuncties als onafhankelijke bronnen maken en beheren. In de portal worden indexeerfuncties beschikbaar gesteld via de wizard **Gegevens importeren**. 

#### <a name="step-1-start-the-import-data-wizard"></a>Stap 1: Wizard Gegevens importeren starten
1. Klik op het dashboard van uw Azure Search-service op **Gegevens importeren** in de opdrachtbalk om een wizard te starten waarmee u een index maakt en deze vervolgens vult.
   
    ![Opdracht Gegevens importeren][2]

2. Klik in de wizard op **Verbinding maken met uw gegevens** > **Voorbeelden** > **realestate-us-sample**. Deze gegevensbron is vooraf geconfigureerd met informatie over een naam, type en verbinding. Zodra de gegevensbron is gemaakt, wordt deze een bestaande gegevensbron genoemd die opnieuw kan worden gebruikt voor andere bewerkingen.

    ![Voorbeeldgegevensset selecteren][9]

3. Klik op **OK** om deze te gebruiken.

#### <a name="skip-cognitive-skills"></a>Cognitieve vaardigheden overslaan

**Gegevens importeren** bevat een optionele stap voor cognitieve vaardigheden waarmee AI-algoritmen worden toegevoegd aan indexering. Deze functie wordt niet besproken in deze zelfstudie, dus u moet verdergaan naar **Doelindex aanpassen**. Als u nieuwsgierig bent naar de nieuwe preview-functie Cognitief zoeken in Azure Search, kunt u de [quickstart](cognitive-search-quickstart-blob.md) of [zelfstudie over cognitief zoeken](cognitive-search-tutorial-blob.md) raadplegen.

   ![Stap voor cognitieve vaardigheden overslaan][11]

#### <a name="step-2-define-the-index"></a>Stap 2: De index definiëren
Het maken van een index gebeurt meestal handmatig en is gebaseerd op code, maar met de wizard kan een index worden gegenereerd voor elke gegevensbron die ermee kan worden verkend. In een index moeten minimaal een naam en een verzameling met velden zijn opgenomen, waarbij één veld is gemarkeerd als de unieke documentsleutel voor elk document.

Velden bevatten gegevenstypen en kenmerken. De selectievakjes bovenaan zijn *indexkenmerken* die bepalen hoe het veld wordt gebruikt. 

* **Ophalen mogelijk** betekent dat dit veld wordt weergegeven in de lijst met zoekresultaten. U kunt afzonderlijke velden markeren als ontoegankelijk voor zoekresultaten door dit selectievakje uit te schakelen, bijvoorbeeld voor velden die alleen in filterexpressies worden gebruikt. 
* De kenmerken **Filterbaar**, **Sorteerbaar** en **Geschikt voor facetten** bepalen of een veld in een filter, een sorteervolgorde of een facetnavigatiestructuur kan worden gebruikt. 
* **Doorzoekbaar** betekent dat een veld is opgenomen in een zoekopdracht in volledige tekst. Tekenreeksen zijn doorzoekbaar. Numerieke velden en Booleaanse waarden zijn vaak gemarkeerd als niet doorzoekbaar. 

Standaard wordt met de wizard de gegevensbron gescand op unieke id's als basis voor het sleutelveld. Tekenreeksen hebben de kenmerken Ophaalbaar en Doorzoekbaar. Gehele getallen hebben de kenmerken Ophaalbaar, Filterbaar, Sorteerbaar en Geschikt voor facetten.

  ![Gegenereerde onroerend goed-index][3]

Klik op **OK** om de index te maken.

#### <a name="step-3-define-the-indexer"></a>Stap 3: De indexeerfunctie definiëren
Klik in de wizard**Gegevens importeren** op **Indexeerfunctie** > **Naam** en typ een naam voor de indexeerfunctie. 

Dit object definieert een uitvoerbaar proces. U kunt een terugkerend schema instellen, maar in dit geval gebruikt u de standaardoptie om de indexeerfunctie één keer uit te voeren zodra u op **OK** klikt.  

  ![indexeerfunctie voor onroerend goed][8]

## <a name="check-progress"></a>Voortgang controleren
Ga terug naar het servicedashboard, schuif omlaag en dubbelklik op de tegel **Indexeerfuncties** om de lijst met indexeerfuncties te openen als u het importeren van gegevens wilt controleren. Als het goed is, ziet u de zojuist gemaakte indexeerfunctie in de lijst met de status Wordt uitgevoerd of Geslaagd, samen met het aantal geïndexeerde documenten.

   ![Voortgangsbericht voor de indexeerfunctie][4]

## <a name="view-the-index"></a>De index bekijken

Tegels in het servicedashboard bieden zowel samenvattende informatie als toegang tot gedetailleerde informatie. Op de tegel **Indexen** zou u bijvoorbeeld een lijst moeten zien met bestaande indexen, waaronder de index *realestate-us-sample* die u zojuist in de vorige stap hebt gemaakt.

Klik nu op de index *realestate-us-sample* om de portalopties voor de definitie ervan te bekijken. Met de optie **Velden toevoegen/bewerken** kunt u nieuwe velden maken en volledig van kenmerken voorzien. Bestaande velden hebben een fysieke weergave in Azure Search en kunnen daarom niet worden gewijzigd, zelfs niet in code. Als u een bestaand veld fundamenteel wilt wijzigen, maakt u een nieuw veld en verwijdert u het oorspronkelijke veld. 

   ![voorbeeld van indexdefinitie][10]

Andere constructies, zoals scoreprofielen en CORS-opties, kunnen op elk gewenst moment worden toegevoegd. 

Neem even de tijd voor het bekijken van de indexdefinitieopties om duidelijk te begrijpen wat u wel en niet kunt bewerken tijdens indexontwerp. Uitgegrijsde opties geven aan dat een waarde niet kan worden gewijzigd of verwijderd.

## <a name="query-index"></a> Een query uitvoeren voor de index
U zou nu een zoekindex moeten hebben die klaar is om query’s uit te voeren met de ingebouwde querypagina [**Search Explorer**](search-explorer.md). Deze pagina biedt een zoekvak waarmee u willekeurige queryreeksen kunt testen. 

> [!TIP]
> In de [Azure Search-overzichtsvideo](https://channel9.msdn.com/Events/Connect/2016/138) worden de volgende stappen na 6 min 8 sec gedemonstreerd.
>

1. Klik op **Search explorer** in de opdrachtbalk.

   ![Opdracht Search Explorer][5]

2. Klik in de opdrachtbalk op **Index wijzigen** om te schakelen naar *realestate-us-sample*. Klik in de opdrachtbalk op **API-versie instellen** om te zien welke REST API's beschikbaar zijn. Gebruik voor onderstaande query’s de algemeen beschikbare versie (11-11-2017). 

   ![Index- en API-opdrachten][6]

3. Voer in de zoekbalk de onderstaande querytekenreeksen in en klik op **Zoeken**.

    > [!NOTE]
    > **Search Explorer** is alleen uitgerust om [REST-API-aanvragen](https://docs.microsoft.com/rest/api/searchservice/search-documents) te verwerken. Er wordt syntaxis geaccepteerd voor [eenvoudige querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) en [volledige Lucene-queryparser](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), plus alle zoekparameters die beschikbaar zijn in bewerkingen voor [Document doorzoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents).
    > 


#### <a name="example-string-searchseattle"></a>Voorbeeld (tekenreeks): `search=seattle`

+ De parameter **search** wordt gebruikt om een zoekopdracht voor zoeken in volledige tekst in te voeren waarmee, in dit geval, vermeldingen in King County, Washington state worden geretourneerd die *Seattle* bevatten in elk doorzoekbaar veld van het document. 

+ Met **Search Explorer** worden resultaten geretourneerd in JSON. Deze indeling is uitgebreid en moeilijk te lezen als documenten een compacte structuur hebben. Dit is opzettelijk; de zichtbaarheid van het hele document is een belangrijke use case, vooral tijdens het testen. Voor een betere gebruikerservaring moet u code schrijven waarmee [zoekresultaten](search-pagination-page-layout.md) belangrijke elementen weergeven.

+ Documenten bestaan uit alle velden die zijn gemarkeerd als ‘Ophalen mogelijk’ in de index. Als u in de portal indexkenmerken wilt weergeven, gaat u naar de tegel **Indexen** en klikt u op *realestate-us-sample*.

#### <a name="example-parameterized-searchseattlecounttruetop100"></a>Voorbeeld (geparameteriseerd): `search=seattle&$count=true&$top=100`

+ Het symbool **&** wordt gebruikt om zoekparameters toe te voegen. Deze kunnen in willekeurige volgorde worden opgegeven. 

+  De parameter **$count=true** retourneert het totale aantal geretourneerde documenten. Deze waarde verschijnt bovenaan de zoekresultaten. U kunt filterquery's controleren door de wijzigingen te controleren die door **$count=true** worden gerapporteerd. Lage aantallen geven aan dat uw filter werkt.

+ De **$top=100** retourneert de 100 hoogst scorende documenten in dit totaal. Standaard retourneert Azure Search de 50 beste resultaten. U kunt dit aantal vergroten of verkleinen via **$top**.

## <a name="filter-query"></a>De query filteren

Filters zijn opgenomen in de zoekopdrachten wanneer u de parameter **$filter** toevoegt. 

#### <a name="example-filtered-searchseattlefilterbeds-gt-3"></a>Voorbeeld (gefilterd): `search=seattle&$filter=beds gt 3`

+ De parameter **$filter** retourneert resultaten die voldoen aan de criteria die u hebt opgegeven. In dit geval: meer dan 3 slaapkamers. 

+ Filtersyntaxis is een OData-constructie. Zie [OData-syntaxis filteren](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) voor meer informatie.

## <a name="facet-query"></a>De query facetteren

Facetfilters zijn opgenomen in zoekopdrachten. U kunt de facetparameter gebruiken om een samengevoegd aantal documenten te retourneren die overeenkomen met een door u opgegeven facetwaarde. 

#### <a name="example-faceted-with-scope-reduction-searchfacetcitytop2"></a>Voorbeeld (gefacetteerd met bereikvermindering): `search=*&facet=city&$top=2`

+ **search=*** is een lege zoekopdracht. Met een lege zoekopdracht wordt naar alles gezocht. Eén reden om een lege query in te dienen, is om de hele set documenten te filteren of als facet te gebruiken. U wilt bijvoorbeeld een facetnavigatiestructuur die bestaat uit alle plaatsen in de index.

+  **facet** retourneert een navigatiestructuur die u kunt doorgeven aan een UI-besturingselement. Deze retourneert categorieën en een aantal. In dit geval zijn categorieën gebaseerd op het aantal plaatsen. Er is geen aggregatie in Azure Search, maar u kunt een geschatte aggregatie bepalen via `facet`, dat het aantal documenten in elke categorie retourneert.

+ **$top=2** retourneert twee documenten, om te illustreren dat u `top` kunt gebruiken om het aantal resultaten te verlagen of te verhogen.

#### <a name="example-facet-on-numeric-values-searchseattlefacetbeds"></a>Voorbeeld (gefacetteerd op numerieke waarden): `search=seattle&facet=beds`**

+ Deze query gebruikt het facet bedden in een tekstuele zoekopdracht naar *Seattle*. De term *beds* kan worden opgegeven als een facet, omdat het veld in de index is gemarkeerd als ophaalbaar, filterbaar en bruikbaar als facet. En de waarden die het bevat (numeriek, 1 tot en met 5), zijn geschikt voor het categoriseren van vermeldingen in groepen (vermeldingen met 3 slaapkamers, 4 slaapkamers). 

+ Alleen filterbare velden kunnen als facet worden gebruikt. Alleen ophaalbare velden kunnen in de resultaten worden geretourneerd.

## <a name="highlight-query"></a> Markering toevoegen

Markeren betekent het toevoegen van opmaak aan tekst die overeenkomt met het trefwoord, overeenkomsten die in een bepaald veld zijn aangetroffen. Als de zoekterm verborgen is in een beschrijving, kunt u de treffers markeren om deze makkelijker te vinden. 

#### <a name="example-highlighter-searchgranite-countertopshighlightdescription"></a>Voorbeeld (gemarkeerd): `search=granite countertops&highlight=description`

+ In dit voorbeeld is de opgemaakte woordgroep *granieten bladen* gemakkelijker te zien in het omschrijvingsveld.

#### <a name="example-linguistic-analysis-searchmicehighlightdescription"></a>Voorbeeld (taalkundige analyse): `search=mice&highlight=description`

+ Met zoekopdrachten in de volledige tekst kunnen woorden met vergelijkbare semantiek worden gevonden. In dit geval bevatten zoekresultaten gemarkeerde tekst voor ‘muis’, voor woningen die een muizenplaag hebben, in reactie op een zoekopdracht naar ‘muis’. De resultaten kunnen verschillende vormen van hetzelfde woord bevatten vanwege taalkundige analyse. 

+ Azure Search ondersteunt 56 analyzers van Lucene en Microsoft. Standaard wordt Lucene Analyzer gebruikt voor Azure Search. 

## <a name="fuzzy-search"></a> Fuzzy zoekopdrachten uitproberen

Voor verkeerd gespelde zoektermen, zoals *samamish* voor het Sammamish-plateau in de regio Seattle, worden bij standaardzoekopdrachten geen overeenkomsten geretourneerd. Het volgende voorbeeld retourneert geen resultaten.

#### <a name="example-misspelled-term-unhandled-searchsamamish"></a>Voorbeeld (verkeerd gespelde term, niet verwerkt): `search=samamish`

U kunt fuzzy zoekopdrachten gebruiken om spelfouten te omzeilen. Fuzzy zoekopdrachten worden ingeschakeld wanneer u de volledige Lucene-querysyntaxis gebruikt, wat gebeurt wanneer u **queryType=full** voor de query instelt en de **~** aan de zoekreeks toevoegt. 

#### <a name="example-misspelled-term-handled-searchsamamishquerytypefull"></a>Voorbeeld (verkeerd gespelde term, verwerkt): `search=samamish~&queryType=full`

Dit voorbeeld retourneert nu documenten die overeenkomsten met ‘Sammamish’ bevatten.

Als **queryType** niet is opgegeven, wordt standaard de eenvoudige queryparser gebruikt. De eenvoudige queryparser is sneller, maar als u gebruik wilt maken van fuzzy zoeken, reguliere expressies, zoeken op nabijheid of andere geavanceerde typen query's, dan hebt u de volledige syntaxis nodig. 

Fuzzy zoekopdrachten en zoekopdrachten met jokertekens hebben implicaties voor zoekresultaten. Taalkundige analyse wordt niet uitgevoerd op deze queryindelingen. Bekijk in [Hoe zoeken in volledige tekst werkt in Azure Search](search-lucene-query-architecture.md#stage-2-lexical-analysis) de sectie over uitzonderingen op lexicale analyse voordat u fuzzy zoekopdrachten en zoekopdrachten met jokertekens gebruikt.

Zie [Lucene-querysyntaxis in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) voor meer informatie over queryscenario's op basis van de volledige queryparser.

## <a name="geo-search"></a> Georuimtelijk zoeken uitproberen

Georuimtelijk zoeken wordt ondersteund door het [gegevenstype edm.GeographyPoint](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) voor een veld met coördinaten. Geosearch is een type filter dat wordt opgegeven bij [Filter OData-syntaxis](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-122121513-47673988-le-5"></a>Voorbeeld (filters voor geografische coördinaten): `search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`

Met dit voorbeeld worden alle resultaten voor positionele gegevens gefilterd die minder dan 5 kilometer zijn verwijderd van een opgegeven punt (opgegeven als coördinaten voor lengte- en breedtegraad). Door **$count** toe te voegen, kunt u zien hoeveel resultaten er worden geretourneerd als u de afstand of de coördinaten wijzigt. 

Georuimtelijk zoeken is handig als uw zoektoepassing een functie 'in mijn buurt zoeken' heeft of gebruikmaakt van kaartnavigatie. Dit is echter niet een zoekopdracht in volledige tekst. Als uw gebruikers op naam naar steden of landen willen zoeken, voegt u, naast coördinaten, ook velden met namen van steden of landen toe.

## <a name="takeaways"></a>Opgedane kennis

Deze zelfstudie demonstreert de basisstappen voor het gebruik van de wizard **Gegevens importeren** en **Search Explorer** in de Azure-portal.

Als drijfveer van de wizard Gegevens importeren hebt u over [indexeerfuncties](search-indexer-overview.md) geleerd, evenals de basiswerkstroom voor indexontwerp, waaronder [ondersteunde wijzigingen in een gepubliceerde index](ttps://docs.microsoft.com/rest/api/searchservice/update-index). 

U hebt querysyntaxis geleerd via praktijkvoorbeelden die belangrijke mogelijkheden demonstreren zoals filters, resultaatmarkering, fuzzy zoekopdrachten en op geografische locaties zoeken.

Tot slot hebt u geleerd hoe u informatie kunt verkrijgen door op tegels in het dashboard te klikken voor elke index, indexeerfunctie of gegevensbron die u voor uw abonnement maakt. Later, wanneer u met uw eigen indexen of die van collega’s werkt, kunt u de portal gebruiken om snel een gegevensbrondefinitie te controleren, of de constructie van een verzameling velden, zonder onbekende code te hoeven doorzoeken.

## <a name="clean-up-resources"></a>Resources opschonen

De snelste manier om op te schonen na een zelfstudie is de resourcegroep met de Azure Search-service te verwijderen. U kunt de resourcegroep nu verwijderen om alles daarin permanent te verwijderen. De naam van de resourcegroep staat in de portal op de pagina Overzicht van de Azure Search-service.

## <a name="next-steps"></a>Volgende stappen

Als u Azure Search verder wilt verkennen met hulpprogramma’s, kunt u een REST-testprogramma zoals Postman of Fiddler gebruiken:

> [!div class="nextstepaction"]
> [Online testprogramma’s voor het aanroepen van de Azure Search REST-API’s](search-fiddler.md)


<!--Image references-->
[1]: ./media/search-get-started-portal/tiles-indexers-datasources2.png
[2]: ./media/search-get-started-portal/import-data-cmd2.png
[3]: ./media/search-get-started-portal/realestateindex2.png
[4]: ./media/search-get-started-portal/indexers-inprogress2.png
[5]: ./media/search-get-started-portal/search-explorer-cmd2.png
[6]: ./media/search-get-started-portal/search-explorer-changeindex-se2.png
[7]: ./media/search-get-started-portal/search-explorer-query2.png
[8]: ./media/search-get-started-portal/realestate-indexer2.png
[9]: ./media/search-get-started-portal/import-datasource-sample2.png
[10]: ./media/search-get-started-portal/sample-index-def.png
[11]: ./media/search-get-started-portal/skip-cog-skill-step.png