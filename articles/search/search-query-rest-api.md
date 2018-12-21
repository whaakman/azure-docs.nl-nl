---
title: In code een query uitvoeren voor een index met behulp van REST API - Azure Search
description: HTTP-opdrachten en REST API-code voor het samenstellen van een zoekquery in Azure Search. Voeg zoekparameters toe om de zoekresultaten te filteren en te sorteren.
author: brjohnstmsft
manager: jlembicz
ms.author: brjohnst
services: search
ms.service: search
ms.topic: quickstart
ms.date: 04/20/2018
ms.custom: seodec2018
ms.openlocfilehash: ccdb872646dded34cb50cd64f56733445132c4b7
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316919"
---
# <a name="query-your-azure-search-index-using-the-rest-api"></a>Een query uitvoeren in uw Azure Search-index met behulp van de REST-API
> [!div class="op_single_selector"]
>
> * [Overzicht](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
>
>

In dit artikel wordt beschreven hoe u een query uitvoert in uw index met behulp van de [Azure Search REST-API](https://docs.microsoft.com/rest/api/searchservice/).

Voordat u deze procedure begint, moet u al [een Azure Search-index hebben gemaakt](search-what-is-an-index.md) en moet deze index [gevuld zijn met gegevens](search-what-is-data-import.md). Zie [How full text search works in Azure Search](search-lucene-query-architecture.md) (Hoe zoeken in volledige tekst werkt in Azure Search) voor achtergrondinformatie.

## <a name="identify-your-azure-search-services-query-api-key"></a>De query api-sleutel voor de Azure Search-service vaststellen
Een belangrijk onderdeel van elke zoekbewerking in de REST-API van Azure Search is de *api-sleutel* die is gegenereerd voor de service die u hebt ingericht. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

1. Als u de API-sleutels van uw service wilt opzoeken, kunt u zich aanmelden bij [Azure Portal](https://portal.azure.com/)
2. Ga naar de blade van uw Azure Search-service
3. Klik op het pictogram Sleutels

Uw service heeft zowel *administratorsleutels* als *querysleutels*.

* De primaire en secundaire *administratorsleutels* verlenen volledige rechten voor alle bewerkingen, inclusief de mogelijkheid voor het beheren van de service, het maken en verwijderen van indexen, indexeerfuncties en gegevensbronnen. Er zijn twee sleutels, zodat u de secundaire sleutel kunt gebruiken als u de primaire sleutel opnieuw aan het genereren bent en vice versa.
* Uw *querysleutels* geven alleen-lezentoegang tot indexen en documenten. Deze sleutels worden doorgaans verleend aan clienttoepassingen die zoekaanvragen verlenen.

U kunt gebruikmaken van een van de query-sleutel om een query in een index uit te voeren. De administratorsleutels kunnen ook worden gebruikt voor query's, maar u moet gebruikmaken van een querysleutel in de toepassingscode, aangezien dit het [principe van minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) volgt.

## <a name="formulate-your-query"></a>Uw query formuleren
Er zijn twee manieren om [in de index te zoeken met behulp van de REST-API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents). De ene manier is om een HTTP POST-aanvraag uit te geven waarbij uw queryparameters worden gedefinieerd in een JSON-object in de aanvraagtekst. De andere manier is om een HTTP GET-aanvraag uit te geven waarbij uw queryparameters worden gedefinieerd in de aanvraag-URL. POST heeft [soepelere limieten](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) met betrekking tot de grootte van queryparameters dan GET. Daarom wordt u aangeraden POST te gebruiken, tenzij er speciale omstandigheden zijn waarin het gebruik van GET beter zou zijn.

Voor zowel POST als GET moet u in de aanvraag-URL de *servicenaam*, de *indexnaam* en de juiste *API-versie* (de huidige API-versie is `2017-11-11` op het moment van publicatie van dit document) opgeven. Voor GET geeft u in de *querytekenreeks* aan het einde van de URL de queryparameters op. Hieronder vindt u de URL-indeling:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2017-11-11

De indeling voor POST is hetzelfde, maar met alleen de api-versie in de queryreeksparameters.

#### <a name="example-queries"></a>Voorbeelden van query 's
Hier volgen een paar voorbeeldquery's op een index met de naam "hotels". Deze query's worden weergegeven in zowel de GET als POST-indeling.

Zoeken in de hele index op de term 'budget' en alleen het veld `hotelName` retourneren:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2017-11-11

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2017-11-11
{
    "search": "budget",
    "select": "hotelName"
}
```

Een filter toepassen op de index om te zoeken naar hotels goedkoper dan €150 per nachten de `hotelId` en `description` retourneren:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2017-11-11

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2017-11-11
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Zoeken in de gehele index, ordenen op een bepaald veld (`lastRenovationDate`) in aflopende volgorde, de twee bovenste resultaten selecteren en alleen `hotelName` and `lastRenovationDate` weergeven:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2017-11-11

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2017-11-11
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="submit-your-http-request"></a>De HTTP-aanvraag verzenden
Nu u uw query hebt geformuleerd als onderdeel van uw HTTP-aanvraag-URL (voor GET) of hoofdtekst (voor POST), kunt u uw aanvraagheaders definiëren en uw query verzenden.

#### <a name="request-and-request-headers"></a>Aanvragen en aanvraagheaders
U moet twee aanvraagheaders definiëren voor GET en drie voor POST:

1. De `api-key`-header moet worden ingesteld op de querysleutel uit stap I hierboven. U kunt ook een administratorsleutel gebruiken voor de header `api-key`, maar het wordt aanbevolen de querysleutel te gebruiken aangezien deze exclusieve alleen-lezentoegang biedt tot indexen en documenten.
2. De `Accept`-header moet worden ingesteld op `application/json`.
3. Alleen voor POST moet de `Content-Type`-header ook worden ingesteld op `application/json`.

Hieronder vindt u een HTTP GET-aanvraag voor de index "hotels" met behulp van de Azure Search REST-API met een eenvoudige query waarin wordt gezocht naar de term "motel":

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2017-11-11
Accept: application/json
api-key: [query key]
```

Dit is dezelfde voorbeeldquery voor HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2017-11-11
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Als een queryaanvraag is gelukt, wordt de statuscode `200 OK` weergegeven. De zoekresultaten worden geretourneerd als JSON in de hoofdtekst van het antwoord. Hier ziet u hoe de resultaten voor de bovenstaande query's eruit zien, ervan uitgaande dat de index "hotels" is gevuld met de voorbeeldgegevens in [Gegevens importeren in Azure Search met behulp van de REST-API](search-import-data-rest-api.md) (de JSON is voor de duidelijkheid geformatteerd).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Ga naar de sectie "Antwoord" van [Documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) voor meer informatie. Zie [HTTP-statuscodes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes) voor meer informatie over andere HTTP-statuscodes die kunnen worden geretourneerd in geval van storing.
