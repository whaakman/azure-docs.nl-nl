---
title: Voorbeeld-REST API voor Azure Search-2017-11-11-Preview - Azure Search
description: Azure Search Service REST API-versie 2017-11-11-Preview bevat experimentele functies zoals synoniemen en moreLikeThis zoekopdrachten.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 06/28/2018
ms.author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 524c1a6d083db02349c7dae9a0131228613dc170
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997607"
---
# <a name="azure-search-service-rest-api-version-2017-11-11-preview"></a>Azure Search Service REST-api-versie 2017-11-11-Preview
In dit artikel beschrijft de `api-version=2017-11-11-Preview` versie van Azure Search service REST API, biedt experimentele functies die nog niet algemeen beschikbaar.

> [!NOTE]
> Preview-functies zijn beschikbaar voor test- en experimenteren met het doel van het verzamelen van feedback en zijn onderhevig aan wijzigingen. We raden u ten zeerste aan met behulp van de preview-API's in productietoepassingen.


## <a name="new-in-2017-11-11-preview"></a>Nieuw in 2017-11-11-Preview

[**automatisch aanvullen** ](search-autocomplete-tutorial.md) lid wordt van de bestaande [suggesties API](https://docs.microsoft.com/rest/api/searchservice/suggestions) om toe te voegen aanvullende, automatisch aangevulde optreedt op de zoekbalk. automatisch aanvullen retourneert candidate querytermen die een gebruiker als de query-tekenreeks voor een toekomstige zoekopdracht kiezen kan. Suggesties retourneert documenten in reactie op gedeeltelijke invoer: zoekresultaten zijn direct en dynamisch worden gewijzigd wanneer de invoer van de term zoeken in de lengte en het specifieke karakter groeit.

[**Cognitieve zoekopdrachten**](cognitive-search-concept-intro.md), een nieuwe verrijking-mogelijkheid in Azure Search latente informatie gevonden in niet-tekstuele bronnen en niet-gedifferentieerde tekst omzetten ervan in volledige tekst doorzoekbare inhoud in Azure Search. De volgende bronnen zijn ingevoerd of gewijzigd in de preview-REST-API. Alle andere REST-API's zijn hetzelfde, ongeacht of u de algemeen beschikbare worden aangeroepen of een preview-versie.

+ [Vaardigheden operations(api-version=2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/skillset-operations)

+ [Indexeerfunctie maken (api-version = 2017-11-11-Preview)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)

Alle andere REST-API's zijn hetzelfde, ongeacht de manier waarop u de api-versie instellen. Bijvoorbeeld, `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11-Preview` en `GET https://[service name].search.windows.net/indexes/hotels?api-version=2017-11-11` (zonder `Preview`) zijn functioneel equivalent.

## <a name="other-preview-features"></a>Andere preview-functies

Functies die in eerdere previews is aangekondigd zijn nog steeds in openbare preview. Als u een API met een eerdere voorbeeld-api-versie bellen, kunt u blijven gebruiken die versie of schakel over naar `2017-11-11-Preview` zonder wijzigingen in het verwachte gedrag.

+ [CSV-bestanden in Azure Blob-indexering](search-howto-index-csv-blobs.md), geïntroduceerd in `api-version=2015-02-28-Preview`, blijft de preview-functie. Deze functie maakt deel uit van Azure Blob-indexering en via een instelling van de parameter wordt aangeroepen. Elke regel in een CSV-bestand wordt geïndexeerd als een afzonderlijke document.

+ [JSON-matrices in Azure Blob-indexering](search-howto-index-json-blobs.md), geïntroduceerd in `api-version=2015-02-28-Preview`, blijft de preview-functie. Deze functie maakt deel uit van Azure Blob-indexering en via een instelling van de parameter wordt aangeroepen. waarbij elk element in de matrix wordt geïndexeerd als een afzonderlijke document.

+ [queryparameter moreLikeThis](search-more-like-this.md) worden documenten gevonden die relevant voor een bepaald document zijn. Deze functie is al in de eerdere voorbeelden. 


## <a name="how-to-call-a-preview-api"></a>Over het aanroepen van een preview-API

Voorbeelden van oudere nog steeds operationeel zijn, maar deze laatste dus verouderd na verloop van tijd. Als uw code wordt de `api-version=2016-09-01-Preview` of `api-version=2015-02-28-Preview`, deze aanroepen nog steeds geldig zijn. Echter is alleen de nieuwste preview-versie bijgewerkt met verbeteringen. 

De voorbeeldsyntaxis van de volgende ziet u een aanroep naar de preview-API-versie.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2017-11-11-Preview

Azure Search-service is beschikbaar in meerdere versies. Zie voor meer informatie, [API-versies](search-api-versions.md).
