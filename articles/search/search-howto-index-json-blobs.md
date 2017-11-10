---
title: Indexeren van JSON-blobs met Azure Search blob indexeerfunctie
description: Indexeren van JSON-blobs met Azure Search blob indexeerfunctie
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 09/07/2017
ms.author: eugenesh
ms.openlocfilehash: 2dac2c5980970946a6b9c26ee6ee8ac0f0344144
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexeren van JSON-blobs met Azure Search blob indexeerfunctie
In dit artikel laat zien hoe een Azure Search blob indexeerfunctie om uit te pakken gestructureerde inhoud uit JSON-blobs in Azure Blob-opslag configureren.

JSON-blobs in Azure Blob storage zijn meestal één JSON-document of een JSON-matrix. De indexeerfunctie blob in Azure Search beide bouw, afhankelijk van hoe u instellen kunt parseren de **parsingMode** parameter bij de aanvraag.

| JSON-document | parsingMode | Beschrijving | Beschikbaarheid |
|--------------|-------------|--------------|--------------|
| Een per blob | `json` | JSON-blobs worden geparseerd als een enkel deel van de tekst. Elke blob JSON wordt een Azure Search-document. | Algemeen beschikbaar zijn in zowel [REST](https://docs.microsoft.com/rest/api/searchservice/indexer-operations) en [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) API's. |
| Meerdere per blob | `jsonArray` | Parseert een JSON-matrix in de blob, waarbij elk element van de matrix een afzonderlijk Azure Search-document wordt.  | In het voorbeeld in [REST api-version =`2016-09-01-Preview` ](search-api-2016-09-01-preview.md) en [.NET SDK Preview](https://aka.ms/search-sdk-preview). |

> [!Note]
> Preview-API's zijn bedoeld voor testen en evalueren en mag niet worden gebruikt in een productieomgeving.
>

## <a name="setting-up-json-indexing"></a>Instellen van JSON indexeren
Indexeren van JSON-blobs is vergelijkbaar met de reguliere document extractie in een werkstroom driedelige algemene naar alle indexeerfuncties in Azure Search.

### <a name="step-1-create-a-data-source"></a>Stap 1: een gegevensbron maken

De eerste stap is het bieden van informatie van de gegevensbronverbinding die wordt gebruikt door de indexeerfunctie. Het opgegeven type voor de gegevensbron hier aangegeven als `azureblob`, bepaalt welke gegevens extractie gedrag worden aangeroepen door de indexeerfunctie. Voor JSON-blob indexeren is gegevensbron de definitie is hetzelfde voor zowel de JSON-documenten en de matrices. 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

### <a name="step-2-create-a-target-search-index"></a>Stap 2: Een doel search-index maken 

Indexeerfuncties zijn gekoppeld aan een indexschema. Als u gebruikmaakt van de API (in plaats van de portal), bereid een index op voorhand zodat u kunt dit op de indexbewerking opgeven. 

> [!Note]
> Indexeerfuncties beschikbaar worden gesteld in de portal via de **importeren** actie voor een beperkt aantal algemeen beschikbaar indexeerfuncties. De werkstroom importeren kunt vaak vaak opstellen voor een voorlopige index op basis van metagegevens in de bron. Zie voor meer informatie [gegevens importeren in Azure Search in de portal](search-import-data-portal.md).

### <a name="step-3-configure-and-run-the-indexer"></a>Stap 3: Configureren en uitvoeren van de indexeerfunctie

Tot op heden is de definities voor de gegevensbron en index parsingMode networkdirect. Echter, in stap 3 voor de configuratie van de indexeerfunctie het pad afwijken, afhankelijk van hoe u wilt de JSON-blob inhoud zijn geparseerd en worden onderverdeeld in een Azure Search-index.

Bij het aanroepen van de indexeerfunctie als volgt:

+ Stel de **parsingMode** -parameter voor `json` (om het indexeren van elke blob als één document) of `jsonArray` (als uw blobs JSON-matrices bevatten en moet u elk element van een matrix moet worden behandeld als een afzonderlijk document).

+ Gebruik desgewenst **veld toewijzingen** kiezen welke eigenschappen van het JSON-document van bron worden gebruikt voor het vullen van uw doel search-index. JSON-matrices, kunt u de documenthoofdmap van een geeft aan waar de matrix is geplaatst in de blob instellen als de matrix als een lager niveau eigenschap bestaat.

> [!IMPORTANT]
> Als u werkt met `json` of `jsonArray` parseren van de modus Azure Search wordt ervan uitgegaan dat alle blobs in uw gegevensbron JSON bevatten. Als u nodig hebt voor de ondersteuning van een combinatie van JSON en niet-JSON-blobs in dezelfde gegevensbron, laat ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).


## <a name="how-to-parse-single-json-blobs"></a>Het parseren van één JSON-blobs

Standaard [Azure Search-indexeerfunctie voor blob](search-howto-indexing-azure-blob-storage.md) JSON-blobs worden geparseerd als een enkel deel van de tekst. Vaak wilt u de structuur van uw JSON-documenten behouden. Stel bijvoorbeeld dat u hebt het volgende JSON-document in Azure Blob-opslag:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

### <a name="indexer-definition-for-single-json-blobs"></a>Definitie van de indexeerfunctie voor één JSON-blobs

Met behulp van de Azure Search-indexeerfunctie blob, wordt een JSON-document als in het vorige voorbeeld in een Azure Search document geparseerd. De indexeerfunctie wordt een index geladen door een overeenkomende 'text', 'datePublished' en 'tags' van de bron met dezelfde naam en getypeerde doelvelden.

Configuratie is opgegeven in de hoofdtekst van een indexeerfunctie-bewerking. Intrekken of het type en informatie van gegevensbron in het gegevensbronobject eerder is gedefinieerd, is opgegeven. De doelindex moet bovendien ook bestaan als een lege container in uw service. Planning en parameters zijn optioneel, maar als u deze weglaat, de indexeerfunctie wordt uitgevoerd onmiddellijk met `json` als de modus voor parseren.

Een volledig opgegeven aanvraag kan er als volgt uitzien:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Zoals vermeld, zijn veld-verwijzingen niet vereist. Opgegeven van een index met 'text', ' datePublished en 'tags'-velden kan de blob indexeerfunctie de juiste toewijzing zonder een veldtoewijzing aanwezig in de aanvraag kunt afleiden.

## <a name="how-to-parse-json-arrays-preview"></a>Het parseren van JSON-matrices (preview)

U kunt ook kiezen voor de preview-functie van de JSON-matrix. Deze functie is nuttig wanneer blobs bevatten een *matrix met JSON-objecten*, en u wilt dat voor elk element in een apart Azure Search-document wordt omgezet. Bijvoorbeeld kunt de volgende JSON-blob gezien, u vullen uw Azure Search-index met drie afzonderlijke documenten, elk met velden 'id' en 'text'.  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

### <a name="indexer-definition-for-a-json-array"></a>Indexeerfunctie definitie voor een JSON-matrix

Voor een JSON-matrix de indexeerfunctie aanvraag maakt gebruik van de preview-API en de `jsonArray` parser. Dit zijn de slechts twee matrix-specifieke vereisten voor indexering van JSON-blobs.

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "jsonArray" } }
    }

Let weer veld-verwijzingen zijn niet vereist. Uitgaande van een index met velden 'id' en 'text', kan de blob-indexeerfunctie de juiste toewijzing zonder een lijst met velden toewijzing afleiden.

### <a name="nested-json-arrays"></a>Geneste matrices van JSON
Wat gebeurt er als u wilt een matrix van JSON-objecten, maar dat matrix index ergens is genest binnen het document? U kunt kiezen welke eigenschap bevat de matrix met behulp van de `documentRoot` configuratie-eigenschap. Als bijvoorbeeld uw blobs moeten uitzien:

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

## <a name="using-field-mappings-to-build-search-documents"></a>U veld-verwijzingen voor het bouwen van documenten zoeken

Wanneer de bron en doel-velden zijn niet perfect uitgelijnd, kunt u een sectie toewijzing definiëren in de aanvraagtekst voor expliciete veld naar koppelingen.

Op dit moment Azure Search kan niet worden geïndexeerd willekeurige JSON-documenten rechtstreeks, omdat het ondersteunt alleen primitieve gegevenstypen, tekenreeksmatrices en GeoJSON punten. U kunt echter **veld toewijzingen** moeten worden verzameld van de onderdelen van uw JSON-document en 'lift' deze in op het hoogste niveau van de velden van het zoekdocument. Zie voor meer informatie over de basisprincipes van veld toewijzingen, [toewijzingen in Azure Search indexeerfuncties veld](search-indexer-field-mappings.md).

Opnieuw bezoeken onze voorbeeld JSON-document:

    {
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Stel een search-index met de volgende velden: `text` van het type `Edm.String`, `date` van het type `Edm.DateTimeOffset`, en `tags` van het type `Collection(Edm.String)`. U ziet het verschil tussen 'datePublished' in de bron en `date` veld in de index. Als u wilt uw JSON toewijzen aan de gewenste vorm, gebruiken de veldtoewijzingen van de volgende:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

De bron-veldnamen in de toewijzingen worden opgegeven met de [JSON aanwijzer](http://tools.ietf.org/html/rfc6901) notatie. U start met een slash om te verwijzen naar de hoofdmap van uw JSON-document en vervolgens de gewenste eigenschap (op een willekeurige niveau geneste) met behulp van doorsturen slash gescheiden pad kiezen.

U kunt ook afzonderlijke matrixelementen verwijzen met behulp van een op nul gebaseerde index. Bijvoorbeeld, om te selecteren in het eerste element van de matrix 'labels' uit het voorgaande voorbeeld, gebruikt u een veldtoewijzing als volgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Als de naam van een bron in een veld toewijzing pad naar een eigenschap die niet bestaat in de JSON verwijst, wordt er zonder fouten toewijzing overgeslagen. Dit wordt gedaan zodat we kunnen documenten met een ander schema (dit is een algemene gebruiksvoorbeeld) ondersteunen. Omdat er geen validatie, moet u behandelen om te voorkomen dat typefouten in uw toewijzing veld opgeven.
>
>

## <a name="example-indexer-request-with-field-mappings"></a>Voorbeeld: Indexeerfunctie aanvraag met veld-verwijzingen

Het volgende voorbeeld is een volledig opgegeven indexeerfunctie nettolading, met inbegrip van veldtoewijzingen:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
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


## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search te verbeteren
Als u hebt het functieaanvragen of suggesties voor verbeteringen, bereiken ons op onze [UserVoice site](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="see-also"></a>Zie ook

+ [Indexeerfuncties in Azure Search](search-indexer-overview.md)
+ [Azure Blob Storage met Azure Search indexeren](search-howto-index-json-blobs.md)
+ [Indexeren van CSV-blobs met Azure Search blob indexeerfunctie](search-howto-index-csv-blobs.md)
+ [Zelfstudie: Semi-gestructureerde gegevens uit Azure Blob storage zoeken](search-semi-structured-data.md)