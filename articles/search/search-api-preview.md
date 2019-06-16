---
title: Voorbeeld-REST API voor Azure Search 2019-05-06-Preview - Azure Search
description: Azure Search Service REST API-versie 2019-05-06-Preview bevat experimentele functies zoals kennis store en door de klant beheerde versleutelingssleutels.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/02/2019
ms.author: HeidiSteen
ms.custom: seodec2018
ms.openlocfilehash: 5374ff896613dd8f8563a2054be8a92103e63fbb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65523908"
---
# <a name="azure-search-service-rest-api-version-2019-05-06-preview"></a>Azure Search Service REST-api-versie 2019-05-06-Preview
In dit artikel beschrijft de `api-version=2019-05-06-Preview` versie van Azure Search service REST API, biedt experimentele functies die nog niet algemeen beschikbaar.

> [!NOTE]
> Preview-functies zijn beschikbaar voor test- en experimenteren met het doel van het verzamelen van feedback en zijn onderhevig aan wijzigingen. We raden u ten zeerste aan met behulp van de preview-API's in productietoepassingen.


## <a name="new-in-2019-05-06-preview"></a>Nieuw in 2019-05-06-Preview

[**Kennis store** ](knowledge-store-concept-intro.md) is een nieuwe bestemming van een pijplijn verrijking op basis van AI. Naast een index, kunt u nu ingevuld gegevensstructuren die zijn gemaakt tijdens het indexeren in Azure storage behouden. U bepaalt de fysieke structuur van uw gegevens via de elementen in een set vaardigheden, inclusief hoe gegevens worden vormgegeven, of gegevens worden opgeslagen in Table storage of Blob storage, en of er zijn meerdere weergaven.

[**Door de klant beheerde versleutelingssleutels** ](search-security-manage-encryption-keys.md) voor servicezijde versleuteling-at-rest ook een nieuwe preview-functie is. Naast de ingebouwde versleuteling-at-rest beheerd door Microsoft, kunt u een extra versleutelingslaag waar u de exclusieve eigenaar van de sleutels zijn toepassen.

## <a name="other-preview-features"></a>Andere preview-functies

Functies die in eerdere previews is aangekondigd zijn nog steeds in openbare preview. Als u een API met een eerdere voorbeeld-api-versie bellen, kunt u blijven gebruiken die versie of schakel over naar `2019-05-06-Preview` zonder wijzigingen in het verwachte gedrag.

+ [queryparameter moreLikeThis](search-more-like-this.md) worden documenten gevonden die relevant voor een bepaald document zijn. Deze functie is al in de eerdere voorbeelden. 
* [Indexeren van CSV-blob](search-howto-index-csv-blobs.md) maakt u een document per regel, in plaats van een document per blob tekst.
* [Ondersteuning voor MongoDB-API voor Cosmos DB-indexeerfuncties](search-howto-index-cosmosdb.md) is beschikbaar als preview.


## <a name="how-to-call-a-preview-api"></a>Over het aanroepen van een preview-API

Voorbeelden van oudere nog steeds operationeel zijn, maar deze laatste dus verouderd na verloop van tijd. Als uw code wordt de `api-version=2016-09-01-Preview` of `api-version=2017-11-11-Preview`, deze aanroepen nog steeds geldig zijn. Echter is alleen de nieuwste preview-versie bijgewerkt met verbeteringen. 

De voorbeeldsyntaxis van de volgende ziet u een aanroep naar de preview-API-versie.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure Search-service is beschikbaar in meerdere versies. Zie voor meer informatie, [API-versies](search-api-versions.md).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de documentatie van Azure Search Service REST API-verwijzing. Als u problemen ondervindt, ons om hulp vragen op [StackOverflow](https://stackoverflow.com/) of [contact op met ondersteuning](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Search service REST API-verwijzing](https://docs.microsoft.com/rest/api/searchservice/)