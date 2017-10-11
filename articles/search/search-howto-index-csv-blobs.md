---
title: Indexeren van CSV-blobs met Azure Search-indexeerfunctie voor blob | Microsoft Docs
description: Meer informatie over het CSV-blobs met Azure Search-index
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: ed3c9cff-1946-4af2-a05a-5e0b3d61eb25
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/15/2016
ms.author: eugenesh
ms.openlocfilehash: af9da85c37211d2436c23cc05400031c661ef51e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexeren van CSV-blobs met Azure Search blob indexeerfunctie
Standaard [Azure Search-indexeerfunctie voor blob](search-howto-indexing-azure-blob-storage.md) parseert gescheiden tekst blobs als een enkel deel van de tekst. Echter met blobs met CSV-gegevens, wilt u meestal op dezelfde manier behandelen elke regel in de blob als een afzonderlijk document. Bijvoorbeeld, krijgt de volgende gescheiden tekst: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

u kunt deze parseren tot 2 documenten elk die 'id', 'datePublished' en 'labels' velden bevat.

In dit artikel leert u hoe CSV BLOB's met een Azure Search-indexeerfunctie blob parseren. 

> [!IMPORTANT]
> Deze functionaliteit is momenteel in preview. Dit is alleen beschikbaar in de REST-API met versie **2015-02-28-Preview**. Maak onthouden, preview-API's zijn bedoeld voor testen en evalueren en mag niet worden gebruikt in een productieomgeving. 
> 
> 

## <a name="setting-up-csv-indexing"></a>Instellen van het CSV-indexeren
Indexeren van CSV-blobs, maken of bijwerken van de definitie van een indexeerfunctie met de `delimitedText` bij het parseren van modus:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Bekijk voor meer informatie over de indexeerfunctie maken API [indexeerfunctie maken](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`Geeft aan dat de eerste regel (niet-lege) van elke blob kopteksten bevat.
Als blobs niet een initiële kopregel bevatten, moeten de headers worden opgegeven in de configuratie van de indexeerfunctie: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Op dit moment wordt wordt alleen de UTF-8-codering ondersteund. Ook de door komma's `','` teken wordt ondersteund als het scheidingsteken. Als u ondersteuning nodig voor andere coderingen of scheidingstekens, laat ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Wanneer u het tekstbestand met scheidingstekens bij het parseren van modus gebruikt, Azure Search wordt ervan uitgegaan dat alle blobs in de gegevensbron zal CSV. Als u nodig hebt voor de ondersteuning van een combinatie van CSV en niet-CSV blobs in dezelfde gegevensbron, laat ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Voorbeelden van aanvraag
Als dit alle samen Hier vindt u de volledige nettolading voorbeelden. 

Gegevensbron: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexeerfunctie:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Help ons Azure Search te verbeteren
Als u functieaanvragen of suggesties voor verbeteringen hebt, kunt contact met ons op onze [UserVoice site](https://feedback.azure.com/forums/263029-azure-search/).

