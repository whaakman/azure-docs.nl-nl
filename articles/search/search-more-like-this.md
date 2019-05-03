---
title: moreLikeThis in Azure Search (preview) - Azure Search
description: Voorlopige documentatie voor de functie moreLikeThis (preview), die beschikbaar zijn in de Azure Search REST-API.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: d18069335bb20f78a5bcda85eb6fcb2a5abe75f7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024675"
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis in Azure Search (preview)

`moreLikeThis=[key]` is een queryparameter in de [zoeken-API voor documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents) waarmee wordt gezocht naar documenten die vergelijkbaar is met het document dat is opgegeven door de documentsleutel. Wanneer een search-aanvraag wordt gedaan met `moreLikeThis`, een query met zoektermen geëxtraheerd uit het opgegeven document die dat document beste beschrijven wordt gegenereerd. De gegenereerde query wordt vervolgens gebruikt voor het maken van de zoekaanvraag. Standaard de inhoud van alle doorzoekbare velden worden beschouwd, min de beperkte velden die u hebt opgegeven met behulp van de `searchFields` parameter. De `moreLikeThis` parameter kan niet worden gebruikt met de zoekparameter `search=[string]`.

De inhoud van het hoogste niveau de doorzoekbare velden worden standaard beschouwd als. Als u wilt dat bepaalde velden in plaats daarvan opgeven, kunt u de `searchFields` parameter. 

> [!NOTE]
> `moreLikeThis` Preview-versie werkt niet op doorzoekbare Subvelden in een [complexe type](search-howto-complex-data-types.md).

## <a name="examples"></a>Voorbeelden 

Hieronder volgt een voorbeeld van een query moreLikeThis. De query vindt u documenten waarvan beschrijving van de velden zijn meest vergelijkbaar met het veld van het brondocument zoals opgegeven door de `moreLikeThis` parameter.

```
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2019-05-06-Preview
```

```
POST /indexes/hotels/docs/search?api-version=2019-05-06-Preview
    {
      "moreLikeThis": "1002",
      "searchFields": "description"
    }
```

## <a name="feature-availability"></a>Beschikbaarheid van functies

De `moreLikeThis` parameter is beschikbaar in preview REST-API's alleen (`api-version=2019-05-06-Preview`).

## <a name="next-steps"></a>Volgende stappen

U kunt elk web testen hulpprogramma gebruiken om te experimenteren met deze functie.  We raden u aan met Postman voor deze oefening.

> [!div class="nextstepaction"]
> [Verken Azure Search REST API's met Postman](search-fiddler.md)