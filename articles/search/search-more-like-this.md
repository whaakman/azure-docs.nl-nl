---
title: moreLikeThis in Azure Search (preview) - Azure Search
description: Voorlopige documentatie voor de functie moreLikeThis (preview), die beschikbaar zijn in de Azure Search REST-API.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 10/27/2016
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 1b91fbbcc025456b48ac8fcfcb3f286ede893541
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314486"
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis in Azure Search (preview)

`moreLikeThis=[key]` is een queryparameter in de [zoeken-API](https://docs.microsoft.com/rest/api/searchservice/search-documents). Door op te geven de `moreLikeThis` -parameter in een zoekopdracht, u kunt zoeken naar documenten die vergelijkbaar met het document dat is opgegeven door de documentsleutel zijn. Wanneer een search-aanvraag wordt gedaan met `moreLikeThis`, een query met zoektermen geëxtraheerd uit het opgegeven document die dat document beste beschrijven wordt gegenereerd. De gegenereerde query wordt vervolgens gebruikt voor het maken van de zoekaanvraag. Standaard wordt de inhoud van alle `searchable` velden worden beschouwd als, tenzij de `searchFields` parameter wordt gebruikt om te beperken van de velden. De `moreLikeThis` parameter kan niet worden gebruikt met de zoekparameter `search=[string]`.

## <a name="examples"></a>Voorbeelden 

Hieronder volgt een voorbeeld van een query moreLikeThis. De query vindt u documenten waarvan beschrijving van de velden zijn meest vergelijkbaar met het veld van het brondocument zoals opgegeven door de `moreLikeThis` parameter.

```  
Get /indexes/hotels/docs?moreLikeThis=1002&searchFields=description&api-version=2016-09-01-Preview
```  

```  
POST /indexes/hotels/docs/search?api-version=2016-09-01-Preview
    {  
      "moreLikeThis": "1002",  
      "searchFields": "description"  
    }  
```  

## <a name="feature-availability"></a>Beschikbaarheid van functies

De functie moreLikeThis is momenteel in preview en wordt alleen ondersteund in de preview-api-versies, `2015-02-28-Preview` en `2016-09-01-Preview`. Omdat de API-versie op de aanvraag is opgegeven, is het mogelijk te combineren in het algemeen beschikbaar is (GA) en preview-API's in dezelfde app. Preview-API's niet onder een SLA en functies vallen kan echter worden gewijzigd, zodat we raden niet dat deze gebruikt in productie-Apps.