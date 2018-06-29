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
ms.openlocfilehash: 2e000628ebec393147328125a04d5d6f55afd3c6
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100484"
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Azure Search Service REST-api-versie 2017-11-11-Preview
In dit artikel beschrijft de `api-version=2017-11-11-Preview` versie van Azure Search service REST API, met experimentele functies nog niet over het algemeen beschikbaar.

> [!NOTE]
> Preview-functies zijn beschikbaar voor het testen en experimenteren met het doel van de feedback te verzamelen en nog worden gewijzigd. We raden u ten zeerste aan met behulp van de preview-API's in productietoepassingen.


## <a name="new-in-2017-11-11-preview"></a>Nieuw in 2017-11-11-Preview

[**Automatisch aanvullen** ](search-autocomplete-tutorial.md) lid wordt van de bestaande [suggesties API](https://docs.microsoft.com/rest/api/searchservice/suggestions) om toe te voegen aanvullende automatisch aangevulde optreedt voor de zoekbalk. Automatisch aanvullen retourneert kandidaat voor een gebruiker kan kiezen als de query-tekenreeks voor een zoekopdracht in de volgende querytermen. Suggesties werkelijke documenten retourneert in reactie op gedeeltelijke invoer: zoekresultaten zijn direct en dynamisch wijzigen wanneer de invoer van de term zoeken in de lengte en het specifieke karakter groeit.

[**Cognitieve zoeken**](cognitive-search-concept-intro.md), een nieuwe verrijking mogelijkheid in Azure Search latente informatie in niet-tekstwaarden bronnen en deze omzetten in volledige tekst doorzoekbare inhoud in Azure Search niet gedifferentieerde tekst wordt gevonden. De volgende resources zijn geïntroduceerd of gewijzigd in de preview REST-API. Alle andere REST-API's zijn hetzelfde, ongeacht of u algemeen beschikbaar aanroepen of preview-versie.

+ [Vaardigheden operations(api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/skillset-operations)

+ [Maak indexeerfunctie (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)

Alle andere REST-API's zijn hetzelfde, ongeacht hoe u de api-versie instellen. Bijvoorbeeld: `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` en `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (zonder `Preview`) functioneel equivalent zijn.

## <a name="other-preview-features"></a>Andere preview-functies

In eerdere previews aangekondigd functies zijn nog steeds in openbare preview. Als u een API met een eerdere preview-api-versie bellen, kunt u blijven gebruiken die versie of overschakelen naar `2017-11-11-Preview` zonder wijzigingen in het verwachte gedrag.

+ [CSV-bestanden in Azure Blob-indexering](search-howto-index-csv-blobs.md), geïntroduceerd in `api-version=2015-02-28-Preview`, blijft een preview-functie. Deze functie maakt deel uit van Azure Blob-indexering en via een instelling van de parameter wordt aangeroepen. Elke regel in een CSV-bestand wordt geïndexeerd als een afzonderlijk document.

+ [JSON-matrices in Azure Blob-indexering](search-howto-index-json-blobs.md), geïntroduceerd in `api-version=2015-02-28-Preview`, blijft een preview-functie. Deze functie maakt deel uit van Azure Blob-indexering en via een instelling van de parameter wordt aangeroepen. waarbij elk element in de matrix wordt geïndexeerd als een afzonderlijk document.

+ [queryparameter moreLikeThis](search-more-like-this.md) vindt documenten die relevant voor een bepaald document zijn. Deze functie is al in de eerdere previews. 


## <a name="how-to-call-a-preview-api"></a>Het aanroepen van een preview-API

Oudere previews nog steeds operationeel zijn, maar zijn verouderd gedurende een bepaalde periode. Als uw code roept `api-version=2016-09-01-Preview` of `api-version=2015-02-28-Preview`, deze aanroepen nog geldig zijn. Echter wordt alleen de meest recente preview-versie vernieuwd met verbeteringen. 

De voorbeeldsyntaxis van de volgende ziet u een aanroep van de preview-API-versie.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Azure Search-service is beschikbaar in meerdere versies. Zie voor meer informatie [API-versies](search-api-versions.md).
