---
title: Azure Search Service REST API-versie 2017 11 11-Preview-| Microsoft Docs
description: Azure Search Service REST API-versie 2017-11-11-Preview bevat experimentele functies zoals synoniemen en moreLikeThis zoekopdrachten.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/17/2018
ms.author: HeidiSteen
ms.openlocfilehash: afcc8ac31c45c1a43d3d759ef6f5a1cee8166c0a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Azure Search Service REST-API: Versie 2017-11-11-Preview
In dit artikel beschrijft de `api-version=2017-11-11-Preview` versie van Azure Search service REST API, met experimentele functies nog niet over het algemeen beschikbaar.

> [!NOTE]
> Preview-functies zijn beschikbaar voor het testen en experimenteren met het doel van de feedback te verzamelen en nog worden gewijzigd. We raden u ten zeerste aan met behulp van de preview-API's in productietoepassingen.


## <a name="new-in-this-preview"></a>Nieuw in deze preview

+ [Cognitieve zoeken](cognitive-search-concept-intro.md), een nieuwe verrijking mogelijkheid in Azure Search latente informatie in niet-tekstwaarden bronnen en deze omzetten in volledige tekst doorzoekbare inhoud in Azure Search niet gedifferentieerde tekst wordt gevonden.

De volgende twee bewerkingen zijn ingevoerd of gewijzigd in de preview REST-API. Alle andere REST-API's zijn hetzelfde, ongeacht of u algemeen beschikbaar aanroepen of preview-versie (bijvoorbeeld.

+ [Vaardigheden maken (api-version = 2017-11-11-Preview)](ref-create-skillset.md)

+ [Maak indexeerfunctie (api-version = 2017-11-11-Preview)](ref-create-indexer.md)

Alle andere REST-API's zijn hetzelfde, ongeacht of u algemeen beschikbaar aanroepen of preview-versie. Bijvoorbeeld: `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` en `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (zonder `Preview`) functioneel equivalent zijn.

## <a name="other-preview-features"></a>Andere preview-functies

Functies van eerdere previews zijn nog steeds in de openbare preview.

+ [CSV-bestanden in Azure Blob-indexering](search-howto-index-csv-blobs.md), waarbij elke regel in een CSV-bestand wordt geïndexeerd als een afzonderlijk document.

+ [JSON-matrices in Azure Blob-indexering](search-howto-index-json-blobs.md), waarbij elk element in de matrix wordt geïndexeerd als een afzonderlijk document.

+ [`moreLikeThis` queryparameter](search-more-like-this.md) om documenten die relevant voor een bepaald document zijn te vinden. Deze functie is al in de eerdere previews. Als u deze API met een eerdere api-versie bellen, kunt u blijven gebruiken die versie.


## <a name="how-to-call-a-preview-api"></a>Het aanroepen van een preview-API

Oudere previews nog steeds operationeel zijn, maar zijn verouderd gedurende een bepaalde periode. Als uw code roept `api-version=2016-09-01-Preview` of `api-version=2015-02-25-Preview`, deze aanroepen nog geldig zijn. Echter wordt alleen de meest recente preview-versie vernieuwd met verbeteringen. 

De voorbeeldsyntaxis van de volgende ziet u een aanroep van de preview-API-versie.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Azure Search-service is beschikbaar in meerdere versies. Zie voor meer informatie [API-versies](search-api-versions.md).
