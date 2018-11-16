---
title: Indexeren van JSON-blobs met de indexeerfunctie voor Azure Search blob
description: Indexeren van JSON-blobs met de indexeerfunctie voor Azure Search blob
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.openlocfilehash: 0dbf8a44007fbba39f6ac4c20e375a6d13ac9021
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51711069"
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexeren van JSON-blobs met de indexeerfunctie voor Azure Search blob
In dit artikel leest u hoe het configureren van een indexeerfunctie Azure Search blob om uit te pakken gestructureerde inhoud uit JSON-blobs in Azure Blob-opslag.

JSON-blobs in Azure Blob-opslag zijn doorgaans een enkele JSON-document of een JSON-matrix. De bouw, afhankelijk van hoe u ingesteld kan worden geparseerd in de blob-indexeerfunctie in Azure Search de **parsingMode** parameter voor de aanvraag.

| JSON-document | parsingMode | Beschrijving | Beschikbaarheid |
|--------------|-------------|--------------|--------------|
| Een per-blob | `json` | Geparseerd JSON-blobs als één segment van de tekst. Elk JSON-blob wordt één Azure Search-document. | Algemeen beschikbaar in zowel [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) en [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) API's. |
| Meerdere per blob | `jsonArray` | Een JSON-matrix in de blob, waarbij elk element van de matrix een afzonderlijke Azure Search-document wordt geparseerd.  | Algemeen beschikbaar in zowel [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) en [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) API's. |


## <a name="setting-up-json-indexing"></a>Instellen van het indexeren van JSON
Indexeren van JSON-blobs is vergelijkbaar met de extractie gewoon document in een werkstroom driedelige gemeenschappelijk voor alle indexeerfuncties in Azure Search.

### <a name="step-1-create-a-data-source"></a>Stap 1: een gegevensbron maken

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

> [!Note]
> Indexeerfuncties worden weergegeven in de portal via de **importeren** actie voor een beperkt aantal algemeen beschikbare indexeerfuncties. De werkstroom importeren kan vaak, vaak bouwen voor een voorlopige index op basis van metagegevens in de bron. Zie voor meer informatie, [gegevens importeren in Azure Search in de portal](search-import-data-portal.md).

### <a name="step-3-configure-and-run-the-indexer"></a>Stap 3: Configureren en de indexeerfunctie uitvoeren

Tot nu toe zijn definities voor de gegevensbron en index parsingMode neutraal. Echter, in stap 3 voor configuratie van de indexeerfunctie, het pad afwijkt, afhankelijk van hoe u wilt dat de JSON-blob inhoud zijn geparseerd en structuur in een Azure Search-index.

Bij het aanroepen van de indexeerfunctie, het volgende doen:

+ Stel de **parsingMode** parameter `json` (indexeren om elke blob als één document) of `jsonArray` (als uw blobs JSON-matrices bevatten en moet u elk element van een matrix moet worden behandeld als een afzonderlijke document).

+ Gebruik eventueel **veldtoewijzingen** om te kiezen welke eigenschappen van de bron-JSON-document worden gebruikt om uw doel search-index te vullen. Voor JSON-matrices, als de matrix als een eigenschap van het lagere niveau bestaat, kunt u instellen een basismap die aangeeft waar de matrix in de blob is geplaatst.

> [!IMPORTANT]
> Bij het gebruik `json` of `jsonArray` parseermodus, Azure Search wordt ervan uitgegaan dat alle blobs in uw gegevensbron JSON bevatten. Als u nodig hebt ter ondersteuning van een combinatie van JSON en niet-JSON-blobs in de dezelfde gegevensbron, laat het ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).


## <a name="how-to-parse-single-json-blobs"></a>Het parseren van één JSON-blobs

Standaard [indexeerfunctie voor Azure Search blob](search-howto-indexing-azure-blob-storage.md) JSON-blobs worden geparseerd als een enkel segment van de tekst. Vaak wilt u de structuur van uw JSON-documenten behouden. Stel dat u hebt de volgende JSON-document in Azure Blob-opslag:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13",
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

### <a name="indexer-definition-for-single-json-blobs"></a>De definitie van de indexeerfunctie voor één JSON-blobs

Met behulp van de indexeerfunctie Azure Search blob, is een JSON-document die vergelijkbaar is met het vorige voorbeeld geparseerd in een enkel Azure Search-document. De indexeerfunctie wordt een index geladen door die overeenkomen met 'tekst', 'datePublished' en 'tags' van de bron tegen dezelfde naam en getypte doelvelden.

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

## <a name="how-to-parse-json-arrays"></a>Het parseren van JSON-matrices

U kunt ook kiezen voor de functie voor JSON-matrix. Deze mogelijkheid is nuttig wanneer blobs bevatten een *matrix met JSON-objecten*, en u wilt dat elk element om te worden van een afzonderlijke Azure Search-document. Bijvoorbeeld, kunt uitgaande van de volgende JSON-blob, u vullen uw Azure Search-index met drie afzonderlijke documenten, elk met velden 'id' en 'tekst'.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

### <a name="indexer-definition-for-a-json-array"></a>De definitie voor een JSON-matrix van indexeerfunctie

Voor een JSON-matrix, het gebruik van de indexeerfunctie-aanvraag de `jsonArray` parser. Dit zijn de slechts twee matrix-specifieke vereisten voor het indexeren van JSON-blobs.

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

Opnieuw bericht veldtoewijzingen zijn niet vereist. Een index met 'id' en 'tekst' velden worden gegeven, afleiden de blob-indexeerfunctie de juiste toewijzing zonder een lijst met velden-toewijzing.

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

## <a name="using-field-mappings-to-build-search-documents"></a>U veldtoewijzingen voor het bouwen van documenten zoeken

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

De bron-veldnamen in de toewijzingen zijn opgegeven met behulp van de [JSON-aanwijzer](http://tools.ietf.org/html/rfc6901) notatie. U start met een slash om te verwijzen naar de hoofdmap van uw JSON-document en vervolgens de gewenste eigenschap (op het niveau van het nesten van willekeurige) kiezen met behulp van forward slash gescheiden pad.

U kunt ook afzonderlijke matrixelementen verwijzen met behulp van een op nul gebaseerde index. Bijvoorbeeld, zodat het eerste element van de matrix 'tags' uit het bovenstaande voorbeeld, gebruikt u een veldtoewijzing als volgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Als naam van een bron in het pad naar een veld toewijzing naar een eigenschap die niet bestaat in JSON verwijst, wordt deze toewijzing overgeslagen zonder fouten. Dit wordt gedaan zodat we kunnen documenten met een ander schema (dit is een gebruikelijk) ondersteunen. Omdat er geen validatie is, moet u voorzichtig zijn om te voorkomen dat typfouten in het veld toewijzing-specificatie.
>
>

## <a name="example-indexer-request-with-field-mappings"></a>Voorbeeld: Indexeerfunctie aanvraag met veldtoewijzingen

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


## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search verbeteren
Als u functieverzoeken heeft of suggesties voor verbeteringen hebt, contact met ons opnemen op onze [UserVoice-site](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="see-also"></a>Zie ook

+ [Indexeerfuncties in Azure Search](search-indexer-overview.md)
+ [Azure Blob Storage met Azure Search indexeren](search-howto-index-json-blobs.md)
+ [Indexeren van CSV-blobs met de indexeerfunctie voor Azure Search blob](search-howto-index-csv-blobs.md)
+ [Zelfstudie: Semi-gestructureerde gegevens uit Azure Blob-opslag doorzoeken ](search-semi-structured-data.md)
