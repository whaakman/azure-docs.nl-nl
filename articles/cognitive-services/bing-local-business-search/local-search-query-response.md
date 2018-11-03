---
title: Verzenden en het gebruik van lokale bedrijven zoeken-API voor Bing-query's en antwoorden | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Gebruik dit artikel voor informatie over het verzenden en het gebruik van zoekquery's met de Bing API voor zoeken van lokale bedrijven.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh; v-gedod
ms.openlocfilehash: a86db516e76a91d6e4bec378c47baf6eec13d032
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959049"
---
# <a name="sending-and-using-bing-local-business-search-api-queries-and-responses"></a>Verzenden en het gebruik van lokale bedrijven zoeken-API voor Bing-query's en antwoorden

U kunt lokale resultaten van de Bing API voor zoeken van lokale bedrijven krijgen door zoekquery's verzenden naar het eindpunt en met inbegrip van de `Ocp-Apim-Subscription-Key` koptekst, die vereist is. Samen met beschikbaar [headers](local-search-reference.md#headers) en [parameters](local-search-reference.md#query-parameters), zoekopdrachten kunnen worden aangepast door op te geven [geografische grenzen](specify-geographic-search.md) voor de ruimte moet worden gezocht en de [categorieën](local-search-query-response.md) van de locaties die worden geretourneerd.

## <a name="creating-a-request"></a>Het maken van een aanvraag

Toevoegen voor het verzenden van een aanvraag naar de Bing API voor zoeken van lokale bedrijven, een zoekterm op die de `q=` parameter toe te voegen aan de API-eindpunt en met inbegrip van de `Ocp-Apim-Subscription-Key` header. Bijvoorbeeld:

`https://api.cognitive.microsoft.com/bing/localbusinesses/v7.0/search?q=restaurant+in+Bellevue`

Hieronder ziet u de volledige aanvraag-URL-syntaxis. Zie de Bing API voor zoeken van lokale bedrijven [snelstartgidsen](quickstarts/local-quickstart.md), en referentie-inhoud voor [headers](local-search-reference.md#headers) en [parameters](local-search-reference.md#query-parameters) voor meer informatie over het verzenden van aanvragen. 

Zie voor meer informatie over lokale zoekcategorieën [categorieën zoeken voor de Bing API voor zoeken van lokale bedrijven](local-categories.md).

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search[?q][&localCategories][&cc][&mkt][&safesearch][&setlang][&count][&first][&localCircularView][&localMapView]
```

## <a name="using-responses"></a>Met behulp van reacties

JSON-antwoorden van de Bing API voor zoeken van lokale bedrijven bevatten een `SearchResponse` object. De API retourneert relevante zoekresultaten in de `places` veld. Als er geen resultaten worden gevonden, de `places` veld wordt niet opgenomen in het antwoord.

```
{
   "_type": "SearchResponse",
   "queryContext": {
      "originalQuery": "restaurant in Bellevue"
   },
   "places": {
      "totalEstimatedMatches": 10,
. . . 
```

### <a name="search-result-attributes"></a>Search resultaat kenmerken

De JSON-resultaten geretourneerd door de API zijn onder andere de volgende kenmerken:

* _Type
* Adres
* entityPresentationInfo
* Geo
* id
* naam
* routeablePoint
* Telefoonnummer
* url

Voor algemene informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [lokale zoeken-API voor Bing versie 7](local-search-reference.md) verwijzing.

> [!NOTE]
> U of een derde partij namens u mogelijk niet gebruiken, behouden, opslaan, in de cache, delen, of distribueren van gegevens uit de lokale zoeken-API voor het testen, ontwikkelen, training, distribueren of het beschikbaar maken van een niet-Microsoft-service of functie. 


## <a name="example-json-response"></a>Voorbeeld van JSON-antwoord

De volgende JSON-antwoord bevat zoekresultaten opgegeven door de query `?q=restaurant+in+Bellevue`.

```json
Vary: Accept-Encoding
BingAPIs-TraceId: 5376FFEB65294E24BB9F91AD70545826
BingAPIs-SessionId: 06ED7CEC80F746AA892EDAAC97CB0CB4
X-MSEdge-ClientID: 112C391E72C0624204153594738C63DE
X-MSAPI-UserState: aeab
BingAPIs-Market: en-US
X-Search-ResponseInfo: InternalResponseTime=659,MSDatacenter=CO4
X-MSEdge-Ref: Ref A: 5376FFEB65294E24BB9F91AD70545826 Ref B: BY3EDGE0306 Ref C: 2018-10-16T16:26:15Z
apim-request-id: fe54f585-7c54-4bf5-8b92-b9bede2b710a
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Cache-Control: max-age=0, private
Date: Tue, 16 Oct 2018 16:26:15 GMT
P3P: CP="NON UNI COM NAV STA LOC CURa DEVa PSAa PSDa OUR IND"
Content-Length: 978
Content-Type: application/json; charset=utf-8
Expires: Tue, 16 Oct 2018 16:25:15 GMT

{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "restaurant Bellevue"
  },
  "places": {
    "totalEstimatedMatches": 50,
    "value": [{
      "_type": "LocalBusiness",
      "id": "https:\/\/cognitivegblppe.azure-api.net\/api\/v7\/#Places.0",
      "name": "Facing East Taiwanese Restaurant",
      "url": "http:\/\/litadesign.wix.com\/facingeastrestaurant",
      "entityPresentationInfo": {
        "entityScenario": "ListItem",
        "entityTypeHints": ["Place", "LocalBusiness", "Restaurant"]
      },
      "geo": {
        "latitude": 47.6199188232422,
        "longitude": -122.202796936035
      },
      "routablePoint": {
        "latitude": 47.6199188232422,
        "longitude": -122.201713562012
      },
      "address": {
        "streetAddress": "1075 Bellevue Way NE Ste B2",
        "addressLocality": "Bellevue",
        "addressRegion": "WA",
        "postalCode": "98004",
        "addressCountry": "US",
        "neighborhood": "Bellevue",
        "text": "1075 Bellevue Way NE Ste B2, Bellevue, WA 98004"
      },
      "telephone": "(425) 688-2986"
    }],
    "searchAction": {
      "location": [{
        "name": "Bellevue, Washington"
      }],
      "query": "restaurant"
    }
  }
}
 
```

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="next-steps"></a>Volgende stappen
- [Snelstartgids voor lokale bedrijven zoeken](quickstarts/local-quickstart.md)
- [Lokale bedrijven zoeken Java-snelstartgids](quickstarts/local-search-java-quickstart.md)
- [Lokale bedrijven zoeken Node-quickstart](quickstarts/local-search-node-quickstart.md)
- [Lokale bedrijven zoeken Python-snelstartgids](quickstarts/local-search-python-quickstart.md)