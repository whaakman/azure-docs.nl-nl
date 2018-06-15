---
title: moreLikeThis in Azure Search (preview) | Microsoft Docs
description: Voorlopige documentatie voor de functie moreLikeThis (preview), worden weergegeven in de Azure Search REST-API.
authors: mhko
manager: jlembicz
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 10/27/2016
ms.author: nateko
ms.openlocfilehash: 29d9a478ca2e91e658d7d0f52e7a193ba694bc16
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31790726"
---
# <a name="morelikethis-in-azure-search-preview"></a>moreLikeThis in Azure Search (preview)

`moreLikeThis=[key]` is een queryparameter in de [Search API](https://docs.microsoft.com/rest/api/searchservice/search-documents). Door op te geven de `moreLikeThis` parameter in een zoekopdracht kunt u documenten die vergelijkbaar met het document dat is opgegeven door de documentsleutel zijn zoeken. Wanneer een search-aanvraag wordt gedaan met `moreLikeThis`, een query is gegenereerd met opgehaald uit het opgegeven document zoektermen die dat document beste beschrijven. De gegenereerde query wordt vervolgens gebruikt voor het maken van de zoekopdracht. Standaard wordt de inhoud van alle `searchable` velden worden beschouwd als tenzij de `searchFields` parameter wordt gebruikt voor het beperken van de velden. De `moreLikeThis` parameter kan niet worden gebruikt met de zoekparameter `search=[string]`.

## <a name="examples"></a>Voorbeelden 

Hieronder volgt een voorbeeld van een query moreLikeThis. De query vindt u documenten waarvan de velden beschrijving meest vergelijkbaar zijn met het veld van het brondocument zoals opgegeven door de `moreLikeThis` parameter.

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

De functie moreLikeThis is momenteel in preview en alleen ondersteund in de preview-api-versies, `2015-02-28-Preview` en `2016-09-01-Preview`. Omdat de API-versie van de aanvraag is opgegeven, is het mogelijk om te worden gecombineerd (GA) is algemeen beschikbaar en preview-API's in dezelfde app. Preview-API's niet onder de SERVICEOVEREENKOMST en de functies zijn kan echter wel wijzigen, zodat u kunt beter niet met deze in productietoepassingen.