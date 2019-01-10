---
title: Search explorer in Azure portal voor het uitvoeren van query's indexen - Azure Search
description: Gebruik Azure portal-hulpprogramma's zoals Search explorer in query-indexen in Azure Search. Voer zoektermen of volledig gekwalificeerde zoekreeksen met geavanceerde syntaxis.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 2aa372d1f917608de753007cc75ab0d608cafbba
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188722"
---
# <a name="how-to-use-search-explorer-to-query-indexes-in-azure-search"></a>Over het gebruik Search explorer in query-indexen in Azure Search 

Dit artikel leest u hoe u query's een bestaande Azure Search index met **Search explorer** in Azure portal. U kunt Search explorer gebruiken om in te dienen eenvoudige of volledige Lucene-queryreeksen naar elke bestaande index in uw service.

## <a name="start-search-explorer"></a>Search explorer starten

1. In de [Azure-portal](https://portal.azure.com), de zoekpagina service openen vanuit het dashboard of [Zoek uw service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in de lijst met Services.

2. Klik op de overzichtspagina van service **Search explorer**.

   ![Verkenner-opdracht in de portal zoeken](./media/search-explorer/search-explorer-cmd2.png "explorer-opdracht in de portal zoeken")

3. Selecteer de index van de query.

   ![Selecteer de index van query](./media/search-explorer/search-explorer-changeindex-se2.png "selecteert u de index")

4. (Optioneel) de API-versie instellen. Standaard de algemeen beschikbare API-versie is geselecteerd, maar kunt u een Preview-versie of oudere API als de syntaxis die u wilt gebruiken is specifiek voor een versie.

5. Nadat de index en API-versie is geselecteerd, voert u zoektermen of volledig gekwalificeerde query-expressies in de zoekbalk en klikt u op **zoeken** om uit te voeren.

   ![Voer zoektermen in en klik op Zoeken](./media/search-explorer/search-explorer-query-string-example.png "Enter zoeken de voorwaarden en klik op zoeken.")

Tips voor zoeken **Search explorer**:

+ Resultaten worden geretourneerd als uitgebreide JSON-documenten, zodat u document bouw en inhoud, volledig kunt bekijken. U kunt query-expressies, die wordt weergegeven in de voorbeelden te beperken welke velden worden geretourneerd.

+ Documenten bestaan uit alle velden die zijn gemarkeerd als **ophalen mogelijk** in de index. Als u wilt weergeven indexkenmerken in de portal, klikt u op *realestate-us-sample* in de **indexen** lijst op de overzichtspagina.

+ Vrije query's, vergelijkbaar met wat u in een commerciële webbrowser invoeren kunt zijn handig voor het testen van een eindgebruiker. Bijvoorbeeld, ervan uitgaande dat de voorbeeldindex ingebouwde onroerend goed, kunt u "Seattle appartementen lake Groningen" opgeven en vervolgens kunt u Ctrl-F termen in de lijst met zoekresultaten vinden. 

+ Query's en filter expressies moeten worden gelede in een syntaxis die wordt ondersteund door Azure Search. De standaardwaarde is een [eenvoudige syntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search), maar u kunt eventueel [volledige Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) voor krachtige query's. [Filterexpressies](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) zijn van een OData-syntaxis.

## <a name="basic-search-strings"></a>Basic zoekreeksen

De volgende voorbeelden wordt ervan uitgegaan dat de voorbeeldindex ingebouwde onroerend goed. Zie voor meer informatie over het maken van deze index [Quick Start: Importeren, index en query's in Azure portal](search-get-started-portal.md).

### <a name="example-1---empty-search"></a>Voorbeeld 1: lege zoekopdracht

Voor een eerste blik op uw inhoud, uitvoeren van een lege zoekopdracht door te klikken op **zoeken** aan geen voorwaarden opgegeven. Een lege zoekopdracht is handig als een eerste query omdat het volledige documenten retourneert, zodat u kunt document samenstelling controleren. Op een lege zoekopdracht, er is geen positie in zoekresultaten en documenten in willekeurige volgorde worden geretourneerd (`"@search.score": 1` voor alle documenten). Standaard worden 50 documenten geretourneerd in een zoekopdracht.

Gelijkwaardige syntaxis voor een lege zoekopdracht is `*` of `search=*`.

   ```Input
   search=*
   ```

   **Results**
   
   ![Voorbeeld van de lege query](./media/search-explorer/search-explorer-example-empty.png "niet-gekwalificeerd of lege query-voorbeeld")

### <a name="example-2---free-text-search"></a>Voorbeeld 2: zoeken met vrije tekst

Vrije-query's, met of zonder operators, zijn nuttig voor het simuleren van de gebruiker gedefinieerde query's worden verzonden vanuit een aangepaste app naar Azure Search. U ziet dat wanneer u zoektermen of expressies opgeeft, positie in zoekresultaten wordt geleverd aan de orde. Het volgende voorbeeld wordt een zoeken met vrije tekst.

   ```Input
   Seattle apartment "Lake Washington" miele OR thermador appliance
   ```

   **Results**

   U kunt Ctrl-F gebruiken om te zoeken in resultaten voor specifieke termen van belang zijn.

   ![Voorbeeld van vrije tekst](./media/search-explorer/search-explorer-example-freetext.png "voorbeeld van vrije tekst")

### <a name="example-3---count-of-matching-documents"></a>Voorbeeld 3: aantal overeenkomende documenten 

Voeg **$count** om het aantal overeenkomsten gevonden in een index. Op een lege zoekopdracht is aantal het totale aantal documenten in de index. Op een gekwalificeerde zoeken is het aantal documenten dat overeenkomt met de invoer van de query.

   ```Input1
   $count=true
   ```
   **Results**

   ![Aantal documenten voorbeeld](./media/search-explorer/search-explorer-example-count.png "aantal overeenkomende documenten in de index")

### <a name="example-4---restrict-fields-in-search-results"></a>Voorbeeld 4: de velden in de zoekresultaten beperken

Voeg **$select** te beperken tot de expliciet benoemde velden voor een beter leesbare uitvoer in **Search explorer**. De zoekreeks houden en **$count = true**, argumenten met het voorvoegsel **&**. 

   ```Input
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true
   ```

   **Results**

   ![Voorbeeld van de velden limiet](./media/search-explorer/search-explorer-example-selectfield.png "velden in de zoekresultaten te beperken")

### <a name="example-5---return-next-batch-of-results"></a>Voorbeeld 5: geretourneerde volgende batch van resultaten

Azure Search retourneert de bovenste 50 overeenkomsten op basis van de positie in zoekresultaten. Toevoegen als u het volgende aantal overeenkomende documenten, **$top = 100 & $skip = 50** te verhogen van het resultaat ingesteld op 100 documenten (de standaardwaarde is 50, maximaal 1000 is), de eerste 50 documenten wordt overgeslagen. Intrekken die u nodig hebt voor de zoekcriteria, zoals een zoekterm of expressie, resultaten ophalen gerangschikt. U ziet dat search scores dieper verkleinen u bereiken in de lijst met zoekresultaten.

   ```Input
   search=seattle condo&$select=listingId,beds,baths,description,street,city,price&$count=true&$top=100,&$skip=50
   ```

   **Results**

   ![Batch-zoekresultaten](./media/search-explorer/search-explorer-example-topskip.png "geretourneerde volgende batch van zoekresultaten")

## <a name="filter-expressions-greater-than-less-than-equal-to"></a>Filterexpressies (groter dan, kleiner dan, gelijk aan)

Gebruik de **$filter** parameter als u wilt opgeven in plaats van zoeken met vrije tekst precieze criteria. In dit voorbeeld wordt gezocht naar groter is dan 3 slaapkamers: `search=seattle condo&$filter=beds gt 3&$count=true`

   ![Filterexpressie](./media/search-explorer/search-explorer-example-filter.png "filteren op criteria")

## <a name="order-by-expressions"></a>Order by-expressies

Voeg **$orderby** om resultaten door een ander veld naast zoekscore te sorteren. Een voorbeeldexpressie die u gebruiken kunt voor het testen van deze functionaliteit is `search=seattle condo&$select=listingId,beds,price&$filter=beds gt 3&$count=true&$orderby=price asc`

   ![OrderBy-expressie](./media/search-explorer/search-explorer-example-ordery.png "de sorteervolgorde wijzigen")

Beide **$filter** en **$orderby** expressies zijn OData-constructies. Zie [OData-syntaxis filteren](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

De volgende resources bieden extra informatie over querysyntaxis en voorbeelden.

 + [Vereenvoudigde querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Lucene-querysyntaxis](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Lucene-queryvoorbeelden](search-query-lucene-examples.md) 
 + [OData-filterexpressiesyntaxis](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 