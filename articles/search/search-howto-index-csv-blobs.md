---
title: CSV-blobs indexeren met de indexeerfunctie Azure Search Blob - Azure Search
description: CSV-blobs in Azure Blob-opslag voor zoeken in volledige tekst met behulp van een Azure Search-index verkennen. Indexeerfuncties automatiseren opname van gegevens voor bepaalde gegevensbronnen, zoals Azure Blob-opslag.
ms.date: 10/17/2018
author: mgottein
manager: cgronlun
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: dce19e750c5546ddd5cac18411dcbb4a603e5c45
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57216098"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexeren van CSV-blobs met de indexeerfunctie voor Azure Search blob
Standaard [indexeerfunctie voor Azure Search blob](search-howto-indexing-azure-blob-storage.md) parseert gescheiden tekst blobs als één segment van de tekst. Echter met blobs met CSV-gegevens, wilt u meestal voor het behandelen van elke regel in de blob als een afzonderlijke document. Bijvoorbeeld, de volgende tekst met scheidingstekens worden gegeven, kunt u parseren in twee documenten, elk met de 'id', 'datePublished' en 'tags' velden: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

In dit artikel leert u hoe u parseren van CSV-blobs met een indexeerfunctie Azure Search blob. 

> [!NOTE]
> Volg de aanbevelingen van de configuratie van indexeerfunctie in [een-op-veel indexeren](search-howto-index-one-to-many-blobs.md) voor uitvoer van meerdere documenten zoeken van een Azure-blob.

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

Indexer:

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

