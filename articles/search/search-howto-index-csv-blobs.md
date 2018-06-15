---
title: Indexeren van CSV-blobs met Azure Search-indexeerfunctie voor blob | Microsoft Docs
description: Meer informatie over het CSV-blobs met Azure Search-index
author: chaosrealm
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 12/28/2017
ms.author: eugenesh
ms.openlocfilehash: bf65ab7858ba792418e325e7a025ee1bd88bbb27
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34363033"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexeren van CSV-blobs met Azure Search blob indexeerfunctie
Standaard [Azure Search-indexeerfunctie voor blob](search-howto-indexing-azure-blob-storage.md) parseert gescheiden tekst blobs als een enkel deel van de tekst. Echter met blobs met CSV-gegevens, wilt u meestal op dezelfde manier behandelen elke regel in de blob als een afzonderlijk document. Bijvoorbeeld de volgende gescheiden tekst gezien, raadzaam parseren in twee documenten elk die 'id', 'datePublished' en 'labels' velden bevat: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

In dit artikel leert u hoe CSV BLOB's met een Azure Search-indexeerfunctie blob parseren. 

> [!IMPORTANT]
> Deze functionaliteit is momenteel in de openbare preview en mag niet worden gebruikt in een productieomgeving. Zie voor meer informatie [REST api-version = 2017-11-11-Preview](search-api-2017-11-11-preview.md). 
> 

## <a name="setting-up-csv-indexing"></a>Instellen van het CSV-indexeren
Indexeren van CSV-blobs, maken of bijwerken van de definitie van een indexeerfunctie met de `delimitedText` bij het parseren van modus:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Bekijk voor meer informatie over de indexeerfunctie maken API [indexeerfunctie maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

`firstLineContainsHeaders` Geeft aan dat de eerste regel (niet-lege) van elke blob kopteksten bevat.
Als blobs niet een initiële kopregel bevatten, moeten de headers worden opgegeven in de configuratie van de indexeerfunctie: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

U kunt het scheidingsteken teken met behulp van de `delimitedTextDelimiter` configuratie-instelling. Bijvoorbeeld:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Op dit moment wordt wordt alleen de UTF-8-codering ondersteund. Als u ondersteuning nodig voor andere coderingen hebt, stemmen voor op [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Wanneer u het tekstbestand met scheidingstekens bij het parseren van modus gebruikt, Azure Search wordt ervan uitgegaan dat alle blobs in de gegevensbron zal CSV. Als u een combinatie van CSV en niet-CSV blobs in dezelfde gegevensbron ondersteunen moet, moet stemmen voor op [UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Voorbeelden van aanvraag
Als dit alle samen Hier vindt u de volledige nettolading voorbeelden. 

Gegevensbron: 

    POST https://[service name].search.windows.net/datasources?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2017-11-11-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search te verbeteren
Als u functie-aanvragen of suggesties voor verbeteringen hebt, geeft u uw invoer op [UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

