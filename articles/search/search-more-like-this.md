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
ms.openlocfilehash: 5723f1ab7258a9e0d672b5c0fd9fd0b9c4dc8721
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65522929"
---
# <a name="morelikethis-in-azure-search"></a>moreLikeThis in Azure Search

> [!Note]
> moreLikeThis is in preview en niet bedoeld voor gebruik in productieomgevingen. De [2019 in de REST-API-versie-05-06-Preview](search-api-preview.md) biedt deze functie. Er is geen .NET SDK-ondersteuning op dit moment.

`moreLikeThis=[key]` is een queryparameter in de [zoeken-API voor documenten](https://docs.microsoft.com/rest/api/searchservice/search-documents) waarmee wordt gezocht naar documenten die vergelijkbaar is met het document dat is opgegeven door de documentsleutel. Wanneer een search-aanvraag wordt gedaan met `moreLikeThis`, een query met zoektermen geëxtraheerd uit het opgegeven document die dat document beste beschrijven wordt gegenereerd. De gegenereerde query wordt vervolgens gebruikt voor het maken van de zoekaanvraag. Standaard de inhoud van alle doorzoekbare velden worden beschouwd, min de beperkte velden die u hebt opgegeven met behulp van de `searchFields` parameter. De `moreLikeThis` parameter kan niet worden gebruikt met de zoekparameter `search=[string]`.

De inhoud van het hoogste niveau de doorzoekbare velden worden standaard beschouwd als. Als u wilt dat bepaalde velden in plaats daarvan opgeven, kunt u de `searchFields` parameter. 

U kunt moreLikeThis niet gebruiken op doorzoekbare onderliggende velden in een [complexe type](search-howto-complex-data-types.md).

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


## <a name="next-steps"></a>Volgende stappen

U kunt elk web testen hulpprogramma gebruiken om te experimenteren met deze functie.  We raden u aan met Postman voor deze oefening.

> [!div class="nextstepaction"]
> [Verken Azure Search REST API's met Postman](search-fiddler.md)