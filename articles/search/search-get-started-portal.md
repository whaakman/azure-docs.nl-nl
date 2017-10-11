---
title: 'Zelfstudie: uw eerste Azure Search-index maken via de portal | Microsoft Docs'
description: Gebruik in Azure Portal de vooraf gedefinieerde voorbeeldgegevens om een index te genereren. Probeer zoekopdrachten in volledige tekst, filters, facetten, fuzzy zoekopdrachten, geosearch en meer.
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 21adc351-69bb-4a39-bc59-598c60c8f958
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 06/26/2017
ms.author: heidist
ms.openlocfilehash: c49989058fdd98d623c5517060f725e5f7e436d8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-create-your-first-azure-search-index-in-the-portal"></a>Zelfstudie: uw eerste Azure Search-index maken via de portal

Begin in Azure Portal met een vooraf gedefinieerde set voorbeeldgegevens om snel een index te genereren met behulp van de wizard **Gegevens importeren**. Probeer zoekopdrachten in volledige tekst, filters, facetten, fuzzy zoekopdrachten en geosearch met **Search Explorer**.  

Met deze inleiding zonder code kunt u aan de slag met vooraf gedefinieerde gegevens, zodat u direct interessante query's kunt schrijven. Hoewel de hulpprogramma's van de portal geen vervanging zijn voor code, zijn ze handig voor deze taken:

+ Hands On leren met minimale ramp-up
+ Prototype van een index maken voordat u code schrijft in **Gegevens importeren**
+ Query's testen en syntaxis parseren in **Search Explorer**
+ Een bestaande index weergeven die is gepubliceerd in uw service, en de bijbehorende kenmerken zoeken

**Geschatte tijd:** ongeveer 15 minuten, maar het kan langer duren als ook is vereist dat u zich registreert bij het account of de service. 

U kunt ook aan de slag met een [op code gebaseerde inleiding tot het programmeren van Azure Search in .NET](search-howto-dotnet-sdk.md).

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u beschikt over een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) en [Azure-service](search-create-service-portal.md). 

Als u niet direct een service wilt inrichten, kunt u ook een zes minuten durende demonstratie bekijken van de stappen in deze zelfstudie. De demonstratie ziet u na ongeveer drie minuten in deze [Azure Search-overzichtsvideo](https://channel9.msdn.com/Events/Connect/2016/138).

## <a name="find-your-service"></a>Uw service vinden
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Open het servicedashboard van uw Azure Search-service. Als u de servicetegel niet hebt vastgemaakt aan het dashboard, kunt u de service op de volgende manier vinden: 
   
   * Klik in de Jumpbar onder aan het linkernavigatiedeelvenster op **Meer services**.
   * Typ in het zoekvak het woord *zoeken* voor een lijst met zoekservices voor uw abonnement. Als het goed is, wordt de service nu weergegeven in de lijst. 

## <a name="check-for-space"></a>Controleren of er voldoende ruimte is
Veel klanten beginnen met de gratis service. Deze versie is beperkt tot drie indexen drie gegevensbronnen en drie indexeerfuncties. Zorg ervoor dat er voldoende ruimte is voor extra items voordat u begint. In deze zelfstudie wordt van elk object één exemplaar gemaakt. 

> [!TIP] 
> Op tegels in het servicedashboard wordt weergegeven hoeveel indexen, indexeerfuncties en gegevensbronnen u al hebt. De tegel Indexeerfunctie bevat indicatoren voor slagen en voor mislukken. Klik op de tegel om het aantal indexeerfuncties te bekijken. 
>
> ![Tegels voor indexeerfuncties en gegevensbronnen][1]
>

## <a name="create-index"></a> Een index maken en gegevens laden
Zoekopdrachten worden over een *index* herhaald met doorzoekbare gegevens, metagegevens en constructies, die worden gebruikt om bepaald zoekgedrag te optimaliseren.

We gebruiken een ingebouwde voorbeeldgegevensset die met behulp van een indexeerfunctie kan worden verkend via de wizard **Gegevens importeren**, om deze taak portal-gebaseerd te houden. 

#### <a name="step-1-start-the-import-data-wizard"></a>Stap 1: Wizard Gegevens importeren starten
1. Klik op het dashboard van uw Azure Search-service op **Gegevens importeren** in de opdrachtbalk om een wizard te starten waarmee u een index maakt en deze vervolgens vult.
   
    ![Opdracht Gegevens importeren][2]

2. Klik in de wizard **Gegevensbron** > **Voorbeelden** > **realestate-us-sample**. Deze gegevensbron is vooraf geconfigureerd met informatie over een naam, type en verbinding. Zodra de gegevensbron is gemaakt, wordt deze een bestaande gegevensbron genoemd die opnieuw kan worden gebruikt voor andere bewerkingen.

    ![Voorbeeldgegevensset selecteren][9]

3. Klik op **OK** om deze te gebruiken.

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

## <a name="query-index"></a> Een query uitvoeren voor de index
U hebt nu een zoekindex die gereed is om op te vragen. **Search explorer** is een queryprogramma dat is ingebouwd in de portal. Er wordt een zoekvak geboden zodat u kunt controleren of de zoekresultaten aan de verwachting voldoen. 

> [!TIP]
> In de [Azure Search-overzichtsvideo](https://channel9.msdn.com/Events/Connect/2016/138) worden de volgende stappen na 6 min 8 sec gedemonstreerd.
>

1. Klik op **Search explorer** in de opdrachtbalk.

   ![Opdracht Search Explorer][5]

2. Klik in de opdrachtbalk op **Index wijzigen** om te schakelen naar *realestate-us-sample*.

   ![Index- en API-opdrachten][6]

3. Klik in de opdrachtbalk op **API-versie instellen** om te zien welke REST API's beschikbaar zijn. Met voorbeeld-API's hebt u toegang tot nieuwe functies die nog niet algemeen zijn uitgebracht. Gebruik voor onderstaande query’s de algemeen beschikbare versie (01-09-2016) tenzij u andere instructies krijgt. 

    > [!NOTE]
    > [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents) en de [.NET-bibliotheek](search-howto-dotnet-sdk.md#core-scenarios) zijn volledig equivalent, maar **Search Explorer** is alleen uitgerust om REST-oproepen te verwerken. Er wordt syntaxis geaccepteerd voor [eenvoudige querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) en [volledige Lucene-queryparser](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), plus alle zoekparameters die beschikbaar zijn in bewerkingen voor [Document doorzoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents).
    > 

4. Voer in de zoekbalk de onderstaande querytekenreeksen in en klik op **Zoeken**.

  ![Voorbeeld van zoekquery][7]

**`search=seattle`**

+ De parameter `search` wordt gebruikt om een zoekopdracht voor zoeken in volledige tekst in te voeren waarmee, in dit geval, vermeldingen in King County, Washington state worden geretourneerd die *Seattle* bevatten in elk doorzoekbaar veld van het document. 

+ Met **Search Explorer** worden resultaten geretourneerd in JSON. Deze indeling is uitgebreid en moeilijk te lezen als documenten een compacte structuur hebben. Afhankelijk van uw documenten moet u mogelijk code schrijven die zoekresultaten verwerkt, om belangrijke elementen uit te pakken. 

+ Documenten bestaan uit alle velden die in de index zijn gemarkeerd als 'Ophalen mogelijk'. Als u in de portal indexkenmerken wilt weergeven, gaat u naar de tegel **Indexen** en klikt u op *realestate-us-sample*.

**`search=seattle&$count=true&$top=100`**

+ Het symbool `&` wordt gebruikt om zoekparameters toe te voegen. Deze kunnen in willekeurige volgorde worden opgegeven. 

+  De parameter `$count=true` retourneert het totale aantal geretourneerde documenten. U kunt filterquery's controleren door de wijzigingen te controleren die door `$count=true` worden gerapporteerd. 

+ De `$top=100` retourneert de 100 hoogst scorende documenten in dit totaal. Standaard retourneert Azure Search de 50 beste resultaten. U kunt dit aantal vergroten of verkleinen via `$top`.

**`search=*&facet=city&$top=2`**

+ `search=*` is een lege zoekopdracht. Met een lege zoekopdracht wordt naar alles gezocht. Eén reden om een lege query in te dienen, is om de hele set documenten te filteren of als facet te gebruiken. U wilt bijvoorbeeld een facetnavigatiestructuur die bestaat uit alle plaatsen in de index.

+  `facet` retourneert een navigatiestructuur die u kunt doorgeven aan een UI-besturingselement. Deze retourneert categorieën en een aantal. In dit geval zijn categorieën gebaseerd op het aantal plaatsen. Er is geen aggregatie in Azure Search, maar u kunt een geschatte aggregatie bepalen via `facet`, dat het aantal documenten in elke categorie retourneert.

+ `$top=2` retourneert twee documenten, om te illustreren dat u `top` kunt gebruiken om het aantal resultaten te verlagen of te verhogen.

**`search=seattle&facet=beds`**

+ Deze query gebruikt het facet bedden in een tekstuele zoekopdracht naar *Seattle*. `"beds"` kan worden opgegeven als een facet, omdat het veld in de index is gemarkeerd als ophaalbaar, filterbaar en bruikbaar als facet. En de waarden die het bevat (numeriek, 1 tot en met 5), zijn geschikt voor het categoriseren van vermeldingen in groepen (vermeldingen met 3 slaapkamers, 4 slaapkamers). 

+ Alleen filterbare velden kunnen als facet worden gebruikt. Alleen ophaalbare velden kunnen in de resultaten worden geretourneerd.

**`search=seattle&$filter=beds gt 3`**

+ De parameter `filter` retourneert resultaten die voldoen aan de criteria die u hebt opgegeven. In dit geval: meer dan 3 slaapkamers. 

+ Filtersyntaxis is een OData-constructie. Zie [OData-syntaxis filteren](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) voor meer informatie.

**`search=granite countertops&highlight=description`**

+ Markeren betekent het toevoegen van opmaak aan tekst die overeenkomt met het trefwoord, overeenkomsten die in een bepaald veld zijn aangetroffen. Als de zoekterm verborgen is in een beschrijving, kunt u de treffers markeren om deze makkelijker te vinden. In dit geval is de opgemaakte woordgroep `"granite countertops"` gemakkelijker te zien in het omschrijvingsveld.

**`search=mice&highlight=description`**

+ Met zoekopdrachten in de volledige tekst kunnen woorden met vergelijkbare semantiek worden gevonden. In dit geval bevatten zoekresultaten gemarkeerde tekst voor ´muis´, bij een zoekopdracht naar ´muizen´ voor huizen die te maken hebben met een muizenplaag. De resultaten kunnen verschillende vormen van hetzelfde woord bevatten vanwege taalkundige analyse. 

+ Azure Search ondersteunt 56 analyzers van Lucene en Microsoft. Standaard wordt Lucene Analyzer gebruikt voor Azure Search. 

**`search=samamish`**

+ Voor verkeerd gespelde woorden (bijvoorbeeld 'samamish' voor het Samammish plateau in de regio Seattle) worden bij standaardzoekopdrachten geen overeenkomsten geretourneerd. U kunt fuzzy zoekopdrachten gebruiken om spelfouten te omzeilen. In het volgende voorbeeld ziet u hoe dit in zijn werk gaat.

**`search=samamish~&queryType=full`**

+ Fuzzy zoeken wordt ingeschakeld als u het symbool `~` opgeeft en de volledige queryparser gebruikt. Hiermee wordt de `~`-syntaxis geïnterpreteerd en juist geparseerd. 

+ Fuzzy zoeken is beschikbaar wanneer u voor de volledige queryparser kiest. Deze wordt weergegeven wanneer u `queryType=full` instelt. Zie [Lucene-querysyntaxis in Azure Search](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) voor meer informatie over queryscenario's op basis van de volledige queryparser.

+ Als `queryType` niet is opgegeven, wordt standaard de eenvoudige queryparser gebruikt. De eenvoudige queryparser is sneller, maar als u gebruik wilt maken van fuzzy zoeken, reguliere expressies, zoeken op nabijheid of andere geavanceerde typen query's, dan hebt u de volledige syntaxis nodig. 

**`search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`**

+ Georuimtelijk zoeken wordt ondersteund door het [gegevenstype edm.GeographyPoint](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) voor een veld met coördinaten. Geosearch is een type filter dat wordt opgegeven bij [Filter OData-syntaxis](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

+ Met dit voorbeeld worden alle resultaten voor positionele gegevens gefilterd die minder dan 5 kilometer zijn verwijderd van een opgegeven punt (opgegeven als coördinaten voor lengte- en breedtegraad). Door `$count` toe te voegen, kunt u zien hoeveel resultaten er worden geretourneerd als u de afstand of de coördinaten wijzigt. 

+ Georuimtelijk zoeken is handig als uw zoektoepassing een functie ´in mijn buurt zoeken´ heeft of gebruikmaakt van kaartnavigatie. Dit is echter niet een zoekopdracht in volledige tekst. Als uw gebruikers op naam naar steden of landen willen zoeken, voegt u, naast coördinaten, ook velden met namen van steden of landen toe.

## <a name="next-steps"></a>Volgende stappen

+ Wijzig de objecten die u zojuist hebt gemaakt. Nadat u de wizard één keer hebt uitgevoerd, kunt u teruggaan en de afzonderlijke onderdelen weergeven of wijzigen, zoals de index, indexeerfunctie of de gegevensbron. Bepaalde wijzigingen die u aanbrengt, zoals het wijzigen van het gegevensveldtype, zijn niet toegestaan in de index. De meeste eigenschappen en instellingen kunnen echter wel worden gewijzigd.

  Als u afzonderlijke onderdelen wilt weergeven, klikt u op **Index**, **Indexeerfunctie** of op de tegel **Gegevensbronnen** op uw dashboard om een lijst met bestaande objecten weer te geven. Zie [Index bijwerken (Azure Search REST API)](https://docs.microsoft.com/rest/api/searchservice/update-index) voor informatie over indexbewerkingen waarvoor opnieuw bouwen niet is vereist.

+ Probeer de hulpprogramma's en stappen met andere gegevensbronnen. De voorbeeldgegevensset (`realestate-us-sample`) komt uit een Azure SQL-database die kan worden verkend met Azure Search. Naast Azure SQL Database kan Azure Search ook een index uit platte gegevensstructuren in Azure Table Storage, Blob Storage of SQL Server op een Azure-VM en Azure Cosmos DB verkennen of afleiden. Al deze gegevensbronnen worden ondersteund in de wizard. In code kunt u een index gemakkelijk vullen met behulp van een *indexeerfunctie*.

+ Alle andere gegevensbronnen die geen indexeerfunctie zijn, worden ondersteund via een pushmodel, waarbij de code nieuwe en gewijzigde rijensets in JSON naar de index pusht. Zie [Documenten toevoegen, bijwerken of verwijderen in Azure Search](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) voor meer informatie.

Voor meer informatie over andere functies die in dit artikel worden vermeld, gaat u naar deze koppelingen:

* [Overzicht van indexeerfuncties](search-indexer-overview.md)
* [Een index maken (inclusief een gedetailleerde beschrijving van de indexkenmerken)](https://docs.microsoft.com/rest/api/searchservice/create-index)
* [Search Explorer](search-explorer.md)
* [Documenten zoeken (inclusief voorbeelden van een querysyntaxis)](https://docs.microsoft.com/rest/api/searchservice/search-documents)


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