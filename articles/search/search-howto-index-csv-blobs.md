---
title: Indexeren van CSV-blobs met de indexeerfunctie voor Azure Search blob | Microsoft Docs
description: Meer informatie over het indexeren van CSV-blobs met Azure Search
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.openlocfilehash: b1f97b5e9542e32096bb060bce40e7b9620d0f49
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406072"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexeren van CSV-blobs met de indexeerfunctie voor Azure Search blob
Standaard [indexeerfunctie voor Azure Search blob](search-howto-indexing-azure-blob-storage.md) parseert gescheiden tekst blobs als één segment van de tekst. Echter met blobs met CSV-gegevens, wilt u meestal voor het behandelen van elke regel in de blob als een afzonderlijke document. Bijvoorbeeld, de volgende tekst met scheidingstekens worden gegeven, kunt u parseren in twee documenten, elk met de 'id', 'datePublished' en 'tags' velden: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

In dit artikel leert u hoe u parseren van CSV-blobs met een indexeerfunctie Azure Search blob. 

> [!IMPORTANT]
> Deze functionaliteit is momenteel in openbare preview en mag niet worden gebruikt in een productieomgeving. Zie voor meer informatie, [REST api-version = 2017-11-11-Preview](search-api-2017-11-11-preview.md). 
> 

## <a name="setting-up-csv-indexing"></a>Instellen van het indexeren van CSV
Indexeren van CSV-blobs, maken of bijwerken van de definitie van een indexeerfunctie met de `delimitedText` parseermodus:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Bekijk voor meer informatie over de indexeerfunctie maken API [indexeerfunctie maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

`firstLineContainsHeaders` Geeft aan dat de eerste regel (niet-lege) van elke blob headers bevat.
Als blobs niet een initiële headerregel bevat, moeten de kolomkoppen in de configuratie van de indexeerfunctie worden opgegeven: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

U kunt het scheidingsteken teken met behulp van de `delimitedTextDelimiter` configuratie-instelling. Bijvoorbeeld:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Op dit moment wordt alleen de UTF-8-codering ondersteund. Als u ondersteuning nodig voor andere coderingen hebt, stemt u erop op [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Wanneer u de tekst met scheidingstekens parseermodus, Azure Search wordt ervan uitgegaan dat alle blobs in uw gegevensbron CSV zal zijn. Als u nodig hebt ter ondersteuning van een combinatie van CSV- en niet-CSV-blobs in de dezelfde gegevensbron, neem stemt u erop op [UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Voorbeelden van aanvraag
Als dit alle samen, zijn hier de volledige nettolading-voorbeelden. 

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

## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search verbeteren
Als u functieverzoeken heeft of suggesties voor verbeteringen hebt, uw gegevens over opgeven [UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

