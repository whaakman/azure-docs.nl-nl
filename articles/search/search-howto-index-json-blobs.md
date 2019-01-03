---
title: JSON-blobs indexeren van Azure Blob-indexering voor zoeken in volledige tekst - Azure Search
description: Verken Azure JSON-blobs voor tekstinhoud met behulp van de indexeerfunctie Azure Search Blob. Indexeerfuncties automatiseren opname van gegevens voor bepaalde gegevensbronnen, zoals Azure Blob-opslag.
ms.date: 12/21/2018
author: HeidiSteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 6df8d9a5c1ca1e587834ea08f73b3dd9498f8537
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753146"
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexeren van JSON-blobs met de indexeerfunctie Azure Search Blob
In dit artikel leest u hoe het configureren van een indexeerfunctie Azure Search blob om uit te pakken gestructureerde inhoud uit JSON-blobs in Azure Blob-opslag.

U kunt de [portal](#json-indexer-portal), [REST-API's](#json-indexer-rest), of [.NET SDK](#json-indexer-dotnet) naar JSON-inhoud indexeren. Gemeenschappelijke alle methoden is JSON-documenten bevinden in een blobcontainer in een Azure Storage-account. Zie voor instructies over het pushen van JSON-documenten in andere niet-Azure-platforms, [gegevens importeren in Azure Search](search-what-is-data-import.md).

JSON-blobs in Azure Blob-opslag zijn doorgaans een enkele JSON-document of een JSON-matrix. De bouw, afhankelijk van hoe u ingesteld kan worden geparseerd in de blob-indexeerfunctie in Azure Search de **parsingMode** parameter voor de aanvraag.

<a name="json-indexer-portal"></a>

## <a name="use-the-portal"></a>Gebruik de portal

De eenvoudigste methode voor het indexeren van JSON-documenten, is het gebruik van een wizard in de [Azure-portal](https://portal.azure.com/). Tijdens het parseren van metagegevens in de Azure blob-container, de [ **gegevens importeren** ](search-import-data-portal.md) wizard kunt maken van een standaardindex, wijzen bronvelden toe aan indexvelden doel en laden van de index in één bewerking. U kunt een operationele volledige tekst search-index in minuten hebben, afhankelijk van de grootte en complexiteit van de brongegevens.

### <a name="1---prepare-source-data"></a>1 - brongegevens voorbereiden

U hebt een Azure storage-account, met Blob-opslag, en een container van JSON-documenten. Als u niet bekend met een van deze taken bent, controleert u de vereiste 'Set up Azure Blob-service en de belasting voorbeeldgegevens' de [cognitive search-quickstart](cognitive-search-quickstart-blob.md#set-up-azure-blob-service-and-load-sample-data).

### <a name="2---start-import-data-wizard"></a>2 - de wizard gegevens importeren starten

U kunt [start de wizard](search-import-data-portal.md) vanuit de opdrachtbalk op de pagina met Azure Search-service of door te klikken op **Azure Search toevoegen** in de **Blob-service** sectie van uw storage-account navigatiedeelvenster links.

### <a name="3---set-the-data-source"></a>3 - de gegevensbron instellen

In de **gegevensbron** pagina, de bron moet **Azure Blob Storage**, met de volgende specificaties:

+ **Gegevens uitpakken** moet *inhoud en metagegevens*. Als u deze optie kiest, kan de wizard een indexschema afleiden en wijs de velden voor het importeren.
   
+ **Bij het parseren modus** moet worden ingesteld op *JSON* of *JSON-matrix*. 

  *JSON* articulates van elke blob als één zoekopdracht documenten, weergegeven als een onafhankelijke item in de zoekresultaten. 

  *JSON-matrix* is voor blobs bestaan uit meerdere elementen, waar u voor elk element in om te worden gelede als een zelfstandige, document onafhankelijke zoeken. Als u blobs zijn complex en u niet kiezen *JSON-matrix* de gehele blob wordt opgenomen als één document.
   
+ **Storage-container** moet uw opslagaccount en container of een verbindingsreeks die wordt omgezet naar de container opgeven. Op de portalpagina van Blob service krijgt u verbindingsreeksen.

   ![BLOB-gegevensbrondefinitie](media/search-howto-index-json/import-wizard-json-data-source.png)

### <a name="4---skip-the-add-cognitive-search-page-in-the-wizard"></a>4 - slaat u de pagina 'Cognitief zoeken toevoegen' in de wizard

Cognitieve vaardigheden toevoegen is niet nodig voor het importeren van JSON-document. Tenzij u specifiek behoefte aan hebt [zijn onder andere Cognitive Services API's en transformaties](cognitive-search-concept-intro.md) aan uw pijplijn indexering, moet u deze stap overslaan.

Als u wilt overslaan, klikt u op de volgende pagina **doelindex aanpassen**.

### <a name="5---set-index-attributes"></a>5 - set indexkenmerken

In de **Index** pagina, ziet u een lijst met velden met een gegevenstype en een reeks selectievakjes in voor het instellen van indexkenmerken. De wizard kan een standaardindex op basis van metagegevens en door steekproeven van de brongegevens te genereren. 

De standaardinstellingen produceren vaak een werkbare oplossing: als u een veld (cast als tekenreeks) om te fungeren als de sleutel of het document-ID als unieke identificatie van elk document, evenals de velden die goede kandidaten zijn voor zoeken in volledige tekst en worden opgehaald in een resultatenset. Voor blobs, de `content` veld is de beste kandidaat voor doorzoekbare inhoud.

U kunt de standaardwaarden accepteren of lees de beschrijving van [indexkenmerken](https://docs.microsoft.com/rest/api/searchservice/create-index#bkmk_indexAttrib) en [taalanalyse](https://docs.microsoft.com/rest/api/searchservice/language-support) overschrijven of vormen een aanvulling op de beginwaarden. 

Neem even de tijd om te controleren van uw selecties. Zodra u de wizard uitvoert, fysieke gegevensstructuren worden gemaakt en kunt u zich niet aan het bewerken van deze velden zonder te verwijderen en opnieuw maken van alle objecten.

   ![De definitie van de blob-index](media/search-howto-index-json/import-wizard-json-index.png)

### <a name="6---create-indexer"></a>6 - indexeerfunctie maken

De wizard maakt volledig is opgegeven, drie afzonderlijke objecten in uw zoekservice. Een gegevensbron en index-object worden opgeslagen als benoemde resources in uw Azure Search-service. De laatste stap maakt u een indexeerfunctie-object. Naamgeving van de indexeerfunctie kan bestaan als een zelfstandige-resource die u kunt plannen en beheren onafhankelijk van het bronobject index en gegevens, die zijn gemaakt in dezelfde volgorde van de wizard.

Als u niet bekend met indexeerfuncties bent, een *indexeerfunctie* is een resource in Azure Search, een externe gegevensbron voor doorzoekbare inhoud te verkennen. De uitvoer van de **gegevens importeren** wizard wordt een indexeerfunctie die uw JSON-gegevensbron te verkennen, doorzoekbare inhoud worden uitgepakt en in een Azure Search-index importeert.

   ![De definitie van de BLOB-indexeerfunctie](media/search-howto-index-json/import-wizard-json-indexer.png)

Klik op **OK** uitvoeren van de wizard en alle objecten maken. Indexeren begint onmiddellijk.

U kunt gegevens importeren in de portal-pagina's bewaken. Voortgang van meldingen geven indexering status en het aantal documenten worden geüpload. Wanneer het indexeren is voltooid, kunt u [Search explorer](search-explorer.md) query uitvoeren op uw index.

<a name="json-indexer-rest"></a>

## <a name="use-rest-apis"></a>REST-API's gebruiken

Indexeren van JSON-blobs is vergelijkbaar met de extractie gewoon document in een werkstroom driedelige gemeenschappelijk voor alle indexeerfuncties in Azure Search: maken van een gegevensbron, een index maken, een indexeerfunctie maken.

Voor de code op basis van JSON indexeren, kunt u de REST-API gebruiken met API's voor [indexeerfuncties](https://docs.microsoft.com/rest/api/searchservice/create-indexer), [gegevensbronnen](https://docs.microsoft.com/rest/api/searchservice/create-data-source), en [indexen](https://docs.microsoft.com/rest/api/searchservice/create-index). In tegenstelling tot de wizard beheerportal een code-benadering vereist dat u een index hebt in-place, klaar om te accepteren van de JSON-documenten bij het verzenden van de **indexeerfunctie maken** aanvraag.

JSON-blobs in Azure Blob-opslag zijn doorgaans een enkele JSON-document of een JSON-matrix. De bouw, afhankelijk van hoe u ingesteld kan worden geparseerd in de blob-indexeerfunctie in Azure Search de **parsingMode** parameter voor de aanvraag.

| JSON-document | parsingMode | Description | Beschikbaarheid |
|--------------|-------------|--------------|--------------|
| Een per-blob | `json` | Geparseerd JSON-blobs als één segment van de tekst. Elk JSON-blob wordt één Azure Search-document. | Algemeen beschikbaar in zowel [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) en [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) API's. |
| Meerdere per blob | `jsonArray` | Een JSON-matrix in de blob, waarbij elk element van de matrix een afzonderlijke Azure Search-document wordt geparseerd.  | Algemeen beschikbaar in zowel [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) en [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) API's. |


### <a name="step-1-create-a-data-source"></a>Stap 1: Een gegevensbron maken

De eerste stap is het bron-verbindingsgegevens voor de gegevens die worden gebruikt door de indexeerfunctie opgeven. Het type, dat is opgegeven voor de gegevensbron als `azureblob`, bepaalt welke gegevens extractie gedrag worden aangeroepen door de indexeerfunctie. Voor JSON blob-indexering is definitie van de gegevensbron hetzelfde voor JSON-documenten en -matrices. 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="step-2-create-a-target-search-index"></a>Stap 2: Een doel search-index maken 

Indexeerfuncties zijn gekoppeld aan een indexschema. Als u gebruikmaakt van de API (in plaats van de portal), bereid een index op voorhand zodat u kunt deze op de indexbewerking opgeven.

De index slaat doorzoekbare inhoud samenvoegt in Azure Search. Geef voor het maken van een index, een schema dat Hiermee geeft u de velden in een document, kenmerken en andere constructies die de zoekervaring vorm. Als u een index met de namen van dezelfde velden en gegevenstypen als de bron maakt, wordt de bron- en velden, zodat u het werk om toe te wijzen expliciet de velden die overeenkomen met de indexeerfunctie.

Het volgende voorbeeld wordt een [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) aanvraag. De index heeft een doorzoekbare `content` veld voor het opslaan van de tekst die is geëxtraheerd uit blobs:   

    POST https://[service name].search.windows.net/indexes?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
          "name" : "my-target-index",
          "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
          ]
    }


### <a name="step-3-configure-and-run-the-indexer"></a>Stap 3: Configureren en de indexeerfunctie uitvoeren

Tot nu toe zijn definities voor de gegevensbron en index parsingMode neutraal. Echter, in stap 3 voor configuratie van de indexeerfunctie, het pad afwijkt, afhankelijk van hoe u wilt dat de JSON-blob inhoud zijn geparseerd en structuur in een Azure Search-index. U kunt kiezen uit `json` of `jsonArray`:

+ Stel **parsingMode** naar `json` indexeren van elke blob als één document.

+ Stel **parsingMode** naar `jsonArray` als uw blobs bestaan uit JSON-matrices en moet u elk element van de matrix om te worden van een afzonderlijke document in Azure Search. U kunt een document zien als één item in de zoekresultaten. Als u wilt dat elk element in de matrix worden weergegeven in zoekresultaten als onafhankelijk item, gebruikt u de `jsonArray` optie.

In het definitie van de indexeerfunctie, kunt u eventueel gebruiken **veldtoewijzingen** om te kiezen welke eigenschappen van de bron-JSON-document worden gebruikt om uw doel search-index te vullen. JSON-matrices, kunt u een document-basis waarmee wordt aangegeven waar de matrix wordt geplaatst in de blob instellen als de matrix als een eigenschap op lagere niveaus bestaat.

> [!IMPORTANT]
> Bij het gebruik `json` of `jsonArray` parseermodus, Azure Search wordt ervan uitgegaan dat alle blobs in uw gegevensbron JSON bevatten. Als u nodig hebt ter ondersteuning van een combinatie van JSON en niet-JSON-blobs in de dezelfde gegevensbron, laat het ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).


### <a name="how-to-parse-single-json-blobs"></a>Het parseren van één JSON-blobs

Standaard [indexeerfunctie voor Azure Search blob](search-howto-indexing-azure-blob-storage.md) JSON-blobs worden geparseerd als een enkel segment van de tekst. Vaak wilt u de structuur van uw JSON-documenten behouden. Stel dat u hebt de volgende JSON-document in Azure Blob-opslag:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

De blob-indexeerfunctie parseert het JSON-document in een Azure Search-document. De indexeerfunctie wordt een index geladen door die overeenkomen met 'tekst', 'datePublished' en 'tags' van de bron op basis van de index met dezelfde naam en getypte doelvelden.

Configuratie is opgegeven in de hoofdtekst van een indexeerfunctie-bewerking. Intrekken van het gegevensbronobject eerder zijn gedefinieerd, Hiermee geeft u het type en gegevensbroninformatie. De doelindex moet bovendien ook bestaan als een lege container in uw service. Planning en parameters zijn optioneel, maar als u ze niet opgeeft, de indexeerfunctie wordt uitgevoerd onmiddellijk, met behulp van `json` als de modus voor het parseren.

Een volledig opgegeven aanvraag kan er als volgt uitzien:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Veldtoewijzingen zijn niet vereist, zoals is vermeld. Gegeven van een index met 'tekst', ' datePublished en 'tags' velden en de blob indexeerfunctie kunt afleiden van de juiste toewijzing zonder een veldtoewijzing aanwezig zijn in de aanvraag.

### <a name="how-to-parse-json-arrays"></a>Het parseren van JSON-matrices

U kunt ook kiezen voor de functie voor JSON-matrix. Deze mogelijkheid is nuttig wanneer blobs bevatten een *matrix met JSON-objecten*, en u wilt dat elk element om te worden van een afzonderlijke Azure Search-document. Bijvoorbeeld, kunt uitgaande van de volgende JSON-blob, u vullen uw Azure Search-index met drie afzonderlijke documenten, elk met velden 'id' en 'tekst'.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Voor een JSON-matrix, moet definitie van de indexeerfunctie lijken op het volgende voorbeeld. U ziet dat de parameter parsingMode geeft u de `jsonArray` parser. De juiste parser op te geven en met de juiste gegevens zijn invoer de slechts twee matrix-specifieke vereisten voor het indexeren van JSON-blobs.

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Nogmaals, zoals u ziet dat veldtoewijzingen kunnen worden weggelaten. Ervan uitgaande dat een index met dezelfde naam 'id' en 'tekst' velden, kan de blob-indexeerfunctie de juiste toewijzing zonder een expliciete toewijzing van de veldenlijst afleiden.

<a name="nested-json-arrays"></a>

### <a name="nested-json-arrays"></a>Geneste JSON-matrices
Wat gebeurt er als u wilt een matrix met JSON-objecten, maar die matrix indexeren ergens is ingesloten in het document? U kunt kiezen welke eigenschap bevat de matrix via de `documentRoot` configuratie-eigenschap. Bijvoorbeeld, als uw blobs als volgt uitzien:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

Deze configuratie gebruiken om te indexeren van de matrix die is opgenomen in de `level2` eigenschap:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

### <a name="using-field-mappings-to-build-search-documents"></a>U veldtoewijzingen voor het bouwen van documenten zoeken

Als bron en doel-velden zijn niet perfect uitgelijnd, kunt u een sectie met toewijzing in de hoofdtekst van de aanvraag voor koppelingen met expliciete veld te definiëren.

Op dit moment Azure Search kan niet worden geïndexeerd willekeurige JSON-documenten rechtstreeks, omdat het ondersteunt alleen primitieve gegevenstypen, tekenreeksmatrices en GeoJSON-punten. U kunt echter **veldtoewijzingen** te selecteren van onderdelen van uw JSON-document 'lift"deze in op het hoogste niveau van de velden van het zoekdocument. Zie voor meer informatie over de basisprincipes van veld toewijzingen, [veldtoewijzingen in Azure Search-indexeerfuncties](search-indexer-field-mappings.md).

Opnieuw bezoeken onze voorbeeld-JSON-document:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Wordt ervan uitgegaan dat een search-index met de volgende velden: `text` van het type `Edm.String`, `date` van het type `Edm.DateTimeOffset`, en `tags` van het type `Collection(Edm.String)`. U ziet het verschil tussen 'datePublished' in de bron en `date` veld in de index. Om toe te wijzen uw JSON de gewenste vorm te geven, gebruikt u de volgende veldtoewijzingen:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

De bron-veldnamen in de toewijzingen zijn opgegeven met behulp van de [JSON-aanwijzer](https://tools.ietf.org/html/rfc6901) notatie. U start met een slash om te verwijzen naar de hoofdmap van uw JSON-document en vervolgens de gewenste eigenschap (op het niveau van het nesten van willekeurige) kiezen met behulp van forward slash gescheiden pad.

U kunt ook afzonderlijke matrixelementen verwijzen met behulp van een op nul gebaseerde index. Bijvoorbeeld, zodat het eerste element van de matrix 'tags' uit het bovenstaande voorbeeld, gebruikt u een veldtoewijzing als volgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Als naam van een bron in het pad naar een veld toewijzing naar een eigenschap die niet bestaat in JSON verwijst, wordt deze toewijzing overgeslagen zonder fouten. Dit wordt gedaan zodat we kunnen documenten met een ander schema (dit is een gebruikelijk) ondersteunen. Omdat er geen validatie is, moet u voorzichtig zijn om te voorkomen dat typfouten in het veld toewijzing-specificatie.
>
>

### <a name="rest-example-indexer-request-with-field-mappings"></a>REST-voorbeeld: Indexeerfunctie aanvraag met veldtoewijzingen

Het volgende voorbeeld is de nettolading van een volledig opgegeven indexer, met inbegrip van veldtoewijzingen:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

<a name="json-indexer-dotnet"></a>

## <a name="use-net-sdk"></a>.NET SDK gebruiken

De .NET SDK is volledig pariteit met de REST-API. Het is raadzaam om de vorige sectie van de REST-API om te leren van concepten, werkstromen en vereisten te controleren. U kunt vervolgens verwijzen naar de volgende .NET API-referentiedocumentatie voor het implementeren van een JSON-indexeerfunctie in beheerde code.

+ [Microsoft.Azure.Search.models.DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet)
+ [Microsoft.Azure.Search.models.DataSourceType](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype?view=azure-dotnet) 
+ [Microsoft.Azure.Search.models.index](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index?view=azure-dotnet) 
+ [Microsoft.Azure.Search.models.indexer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer?view=azure-dotnet)

## <a name="see-also"></a>Zie ook

+ [Indexeerfuncties in Azure Search](search-indexer-overview.md)
+ [Azure Blob Storage met Azure Search indexeren](search-howto-index-json-blobs.md)
+ [Indexeren van CSV-blobs met de indexeerfunctie voor Azure Search blob](search-howto-index-csv-blobs.md)
+ [Zelfstudie: Semi-gestructureerde gegevens uit Azure Blob-opslag doorzoeken](search-semi-structured-data.md)
