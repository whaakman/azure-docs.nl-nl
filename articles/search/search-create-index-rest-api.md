---
title: Een index maken (REST-API - Azure Search) | Microsoft Docs
description: Een index in code maken met behulp van de Azure Search HTTP REST-API.
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: ac6c5fba-ad59-492d-b715-d25a7a7ae051
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: 9a64d1436471e406b7d9b700257d3dd96b5edcde
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-azure-search-index-using-the-rest-api"></a>Een index voor Azure Search maken met behulp van de REST-API.
> [!div class="op_single_selector"]
>
> * [Overzicht](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
>
>

Dit artikel begeleidt u stapsgewijs door het proces van het maken van een [index](https://docs.microsoft.com/rest/api/searchservice/Create-Index) voor Azure Search met behulp van de Azure Search REST-API.

Voordat u de stappen in dit artikel uitvoert en een index maakt, moet u eerst [een Azure Search-service](search-create-service-portal.md) hebben gemaakt.

Als u een index voor Azure Search met behulp van de REST-API wilt maken, verzendt u één HTTP POST-aanvraag via de URL van het Azure Search-service-eindpunt. De definitie van de index bevindt zich zowel in de aanvraagtekst als de juist-opgemaakte JSON-inhoud.

## <a name="identify-your-azure-search-services-admin-api-key"></a>De admin api-sleutel voor de Azure Search-service vaststellen
Nu u een Azure Search-service hebt ingericht, bent u bijna klaar om HTTP-aanvragen te verzenden voor het URL-eindpunt van uw service met de REST-API. *Alle* API-aanvragen moeten de API-sleutel bevatten die is gegenereerd voor de Search-service die u hebt ingericht. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

1. Als u de API-sleutels van uw service wilt opzoeken, moet u zich aanmelden bij [Azure Portal](https://portal.azure.com/)
2. Ga naar de blade van uw Azure Search-service
3. Klik op het pictogram Sleutels

Uw service heeft zowel *administratorsleutels* als *querysleutels*.

* De primaire en secundaire *administratorsleutels* verlenen volledige rechten voor alle bewerkingen, inclusief de mogelijkheid voor het beheren van de service, het maken en verwijderen van indexen, indexeerfuncties en gegevensbronnen. Er zijn twee sleutels, zodat u de secundaire sleutel kunt gebruiken als u de primaire sleutel opnieuw aan het genereren bent en vice versa.
* Uw *querysleutels* geven alleen-lezentoegang tot indexen en documenten. Deze sleutels worden doorgaans verleend aan clienttoepassingen die zoekaanvragen verlenen.

Als u een index wilt maken, kunt u de primaire of secundaire administratorsleutel gebruiken.

## <a name="define-your-azure-search-index-using-well-formed-json"></a>Een index voor Azure Search definiëren met een goed-opgemaakte JSON
Als u de index wilt maken, hoeft u maar één HTTP POST-aanvraag bij uw service te doen. De hoofdtekst van uw HTTP POST-aanvraag bevat een JSON-object dat uw Azure Search-index definieert.

1. De eerste eigenschap van dit JSON-object is de naam van de index.
2. De tweede eigenschap van dit JSON-object is een JSON-matrix met de naam `fields`, die een afzonderlijk JSON-object voor elk veld in de index bevat. Elk van deze JSON-objecten bevat meerdere naamwaardeparen voor elk veldkenmerken, zoals naam, type enzovoort.

Als u uw index opzet, is het belangrijk dat u in uw achterhoofd houdt wat de gebruiker en uw bedrijf nodig hebben, aangezien aan elk veld de [relevante kenmerken](https://docs.microsoft.com/rest/api/searchservice/Create-Index) moeten worden toegewezen. Deze kenmerken bepalen welke zoekfuncties (filteren, facetten, zoekacties in volledige tekst sorteren enzovoort) beschikbaar zijn in velden. Als u een eigenschap niet expliciet instelt, worden de bijbehorende zoekfuncties standaard ingeschakeld, tenzij u deze functies specifiek hebt uitgeschakeld.

In ons voorbeeld hebben we onze index hotels genoemd en de velden als volgt ingesteld:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

We hebben de waarde van de indexkenmerken zorgvuldig voor elk veld ingesteld op basis van onze verwachting van het gebruik ervan in een toepassing. `hotelId` is bijvoorbeeld een unieke sleutel die mensen die naar een hotel zoeken waarschijnlijk niet kennen. Daarom hebben voor dat veld het zoeken in volledige tekst uitgeschakeld door `searchable` in te stellen op `false`, waardoor ruimte in de index wordt bespaard.

In een index van het type `Edm.String` moet precies één veld zijn aangewezen als het sleutelveld.

Voor de bovenstaande indexdefinitie wordt gebruikgemaakt van een taalanalyse voor het veld `description_fr`, omdat dit veld is bedoeld voor het opslaan van Franse tekst. Zie het onderwerp [Language support](https://docs.microsoft.com/rest/api/searchservice/Language-support) (Taalondersteuning) en het bijbehorende [blogbericht](https://azure.microsoft.com/blog/language-support-in-azure-search/) voor meer informatie over taalanalyse.

## <a name="issue-the-http-request"></a>De HTTP-aanvraag verzenden
1. U kunt de indexdefinitie gebruiken als aanvraagtekst. Verzend een HTTP POST-aanvraag via de URL van het Azure Search-service-eindpunt. In de URL moet de naam van de service gebruiken en de hostnaam gebruiken. Bovendien moet u de juiste `api-version` als een queryreeksparameter opgeven (op het moment van publicatie van dit document is `2016-09-01` de API-versie).
2. Gebruik in de aanvraagheaders `Content-Type` voor `application/json`. U moet ook de administratorsleutel van de service opgeven, zoals die is gedefinieerd in Stap I in de `api-key`-header.

U moet uw eigen servicenaam en API-sleutel opgeven om de onderstaande aanvraag te kunnen doen:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [api-key]


Als een aanvraag is gelukt, ziet u de statuscode 201 (Gemaakt). Ga [hier](https://docs.microsoft.com/rest/api/searchservice/Create-Index) naar de API-verwijzing voor meer informatie over het maken van een index met de REST-API. Zie [HTTP-statuscodes (Azure Search)](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes) voor meer informatie over andere HTTP-statuscodes die kunnen worden geretourneerd in geval van storing.

Als u klaar bent met een index en deze weer wilt verwijderen, roept u de aanvraag HTTP DELETE aan De index hotels zou bijvoorbeeld als volgt kunnen worden verwijderd:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2016-09-01
    api-key: [api-key]


## <a name="next-steps"></a>Volgende stappen
Als u een index voor Azure Search hebt gemaakt, kunt u [de inhoud naar de index uploaden](search-what-is-data-import.md), zodat u kunt beginnen met het zoeken van gegevens.
