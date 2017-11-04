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
ms.openlocfilehash: bf4d3a517e1308a142d21cffff64f3c6e104eb62
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexeren van JSON-blobs met Azure Search blob indexeerfunctie
In dit artikel laat zien hoe Azure Search blob indexeerfunctie om uit te pakken gestructureerde inhoud uit blobs die JSON bevatten configureren.

## <a name="scenarios"></a>Scenario's
Standaard [Azure Search-indexeerfunctie voor blob](search-howto-indexing-azure-blob-storage.md) JSON-blobs worden geparseerd als een enkel deel van de tekst. Vaak wilt u de structuur van uw JSON-documenten behouden. Bijvoorbeeld, krijgt het JSON-document

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

u kunt in een Azure Search-document met 'text', 'datePublished' en 'labels' velden worden geparseerd.

U kunt ook wanneer uw blobs bevatten een **matrix met JSON-objecten**, kunt u elk element van de matrix om te worden van een afzonderlijk Azure Search-document. Bijvoorbeeld, krijgt een blob met deze JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

u kunt uw Azure Search-index met drie afzonderlijke documenten, elk met velden 'id' en 'text' vullen.

> [!IMPORTANT]
> De JSON-matrix parseren functionaliteit is momenteel in preview. Dit is alleen beschikbaar in de REST-API met versie **2016-09-01-Preview**. Denk erom, preview-API's zijn bedoeld voor testen en evalueren en mag niet worden gebruikt in een productieomgeving.
>
>

## <a name="setting-up-json-indexing"></a>Instellen van JSON indexeren
Indexeren van JSON-blobs is vergelijkbaar met de extractie standaarddocument. Maak eerst de datasource precies zoals u normaal doet: 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

De doel-zoekindex vervolgens maken als u er nog geen hebt. 

Ten slotte Maak een indexeerfunctie en stel de `parsingMode` -parameter voor `json` (om het indexeren van elke blob als één document) of `jsonArray` (als uw blobs JSON-matrices bevatten en moet u elk element van een matrix moet worden behandeld als een afzonderlijk document):

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

Gebruik indien noodzakelijk **veld toewijzingen** kiest u de eigenschappen van de bron-JSON-document gebruikt voor het vullen van uw doel search-index in de volgende sectie.

> [!IMPORTANT]
> Als u werkt met `json` of `jsonArray` parseren van de modus Azure Search wordt ervan uitgegaan dat alle blobs in uw gegevensbron JSON bevatten. Als u nodig hebt voor de ondersteuning van een combinatie van JSON en niet-JSON-blobs in dezelfde gegevensbron, laat ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).
>
>

## <a name="using-field-mappings-to-build-search-documents"></a>U veld-verwijzingen voor het bouwen van documenten zoeken
Op dit moment Azure Search kan niet worden geïndexeerd willekeurige JSON-documenten rechtstreeks, omdat het ondersteunt alleen primitieve gegevenstypen, tekenreeksmatrices en GeoJSON punten. U kunt echter **veld toewijzingen** moeten worden verzameld van de onderdelen van uw JSON-document en 'lift' deze in op het hoogste niveau van de velden van het zoekdocument. Zie voor meer informatie over de basisprincipes van veld toewijzingen, [veld-verwijzingen voor Azure Search-indexeerfunctie overbruggen de verschillen tussen de gegevensbronnen en zoekindexen](search-indexer-field-mappings.md).

Binnenkort terug naar onze voorbeeld JSON-document:

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Stel dat u hebt een search-index met de volgende velden: `text` van het type `Edm.String`, `date` van het type `Edm.DateTimeOffset`, en `tags` van het type `Collection(Edm.String)`. Als u wilt uw JSON toewijzen aan de gewenste vorm, gebruiken de veldtoewijzingen van de volgende:

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

Als uw JSON-documenten alleen eenvoudige op het hoogste niveau eigenschappen bevatten, moet u mogelijk geen veldtoewijzingen helemaal. Bijvoorbeeld, als uw JSON uitziet, de eigenschappen van het hoogste niveau 'text', 'datePublished' en 'tags' rechtstreeks wordt toegewezen aan de overeenkomende velden in de search-index:

    {
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13"
       "tags" : [ "search", "storage", "howto" ]    
     }

Hier volgt een volledige indexeerfunctie nettolading met veldtoewijzingen:

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

## <a name="indexing-nested-json-arrays"></a>Geneste matrices van JSON indexeren
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

## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search te verbeteren
Als u hebt het functieaanvragen of suggesties voor verbeteringen, bereiken ons op onze [UserVoice site](https://feedback.azure.com/forums/263029-azure-search/).
