---
title: Beveiligingsfilters voor bijsnijden resultaten - Azure Search
description: Toegangsbeheer voor Azure Search-inhoud met beveiligingsfilters en gebruikers-id's.
ms.service: search
ms.topic: conceptual
services: search
ms.date: 08/07/2017
author: brjohnstmsft
ms.author: brjohnst
manager: jlembicz
ms.custom: seodec2018
ms.openlocfilehash: 326a449d3992d22a4be2d365061c99ef8b13aef9
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453487"
---
# <a name="security-filters-for-trimming-results-in-azure-search"></a>Beveiligingsfilters voor trimming resulteert in Azure Search

U kunt de beveiligingsfilters zoekresultaten in Azure Search op basis van gebruikersidentiteit TRIM toepassen. Deze zoekervaring is algemeen vereist voor het vergelijken van de identiteit van degene die het zoeken op basis van een veld met de principes die machtigingen voor het document hebben aanvragen. Wanneer een overeenkomst wordt gevonden, heeft de gebruiker of de principal (zoals een groep of rol) toegang tot het document.

Eén manier om een beveiliging filteren is tot en met een gecompliceerde scheiding van gelijkheid expressies: bijvoorbeeld `Id eq 'id1' or Id eq 'id2'`, enzovoort. Deze aanpak is gevoelig voor fouten, moeilijk te onderhouden, en in gevallen waarin de lijst honderden of duizenden waarden bevat, vertraagt de reactietijd van de query door op hoeveel seconden. 

Een eenvoudiger en sneller aanpak is via de `search.in` functie. Als u `search.in(Id, 'id1, id2, ...')` in plaats van een expressie gelijkheid u dan een seconde reactie kan verwachten tijden.

In dit artikel laat zien hoe om uit te voeren Beveiligingsfiltering met behulp van de volgende stappen uit:
> [!div class="checklist"]
> * Maken van een veld met de principal-id 's 
> * Push- of bijwerken van bestaande documenten met de relevante principal-id 's
> * Een zoekaanvraag met uitgeven `search.in` `filter`

>[!NOTE]
> Het proces van het ophalen van de principal-id's wordt niet beschreven in dit document. U krijgt het van uw id-provider.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), [Azure Search-service](https://docs.microsoft.com/azure/search/search-create-service-portal), en [Azure Search-Index](https://docs.microsoft.com/azure/search/search-create-index-portal).  

## <a name="create-security-field"></a>Beveiliging veld maken

Uw documenten moeten een veld op te geven welke groepen hebben toegang tot bevatten. Deze informatie wordt de filtercriteria op basis waarvan de documenten worden geselecteerd of geweigerd worden van de resultatenset geretourneerd naar de verlener.
Stel dat we een overzicht van de beveiligde bestanden hebben, en elk bestand toegankelijk is door een andere set gebruikers.
1. Veld toevoegen `group_ids` (u kunt een willekeurige naam hier) als een `Collection(Edm.String)`. Zorg ervoor dat het veld heeft een `filterable` kenmerk ingesteld op `true` zodat zoekresultaten worden gefilterd op basis van de toegang tot de gebruiker heeft. Als u bijvoorbeeld de `group_ids` veld `["group_id1, group_id2"]` voor het document met `file_name` "secured_file_b", alleen gebruikers die deel uitmaken van groep-id's 'group_id1' of 'group_id2' hebben toegang tot het bestand lezen.
   Zorg ervoor dat van het veld `retrievable` kenmerk is ingesteld op `false` zodat deze niet wordt geretourneerd als onderdeel van de zoekopdracht.
2. Ook toevoegen `file_id` en `file_name` velden dit voorbeeldscenario.  

```JSON
{
    "name": "securedfiles",  
    "fields": [
        {"name": "file_id", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "file_name", "type": "Edm.String"},
        {"name": "group_ids", "type": "Collection(Edm.String)", "filterable": true, "retrievable": false}
    ]
}
```

## <a name="pushing-data-into-your-index-using-the-rest-api"></a>Gegevens pushen naar uw index met behulp van de REST-API
  
Geven een HTTP POST-aanvraag voor URL-eindpunt van uw index. De hoofdtekst van de HTTP-aanvraag is een JSON-object met de documenten die moeten worden toegevoegd:

```
POST https://[search service].search.windows.net/indexes/securedfiles/docs/index?api-version=[api-version]  
Content-Type: application/json
api-key: [admin key]
```

Geef de inhoud van uw documenten in de hoofdtekst van de aanvraag:

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "file_id": "1",
            "file_name": "secured_file_a",
            "group_ids": ["group_id1"]
        },
        {
            "@search.action": "upload",
            "file_id": "2",
            "file_name": "secured_file_b",
            "group_ids": ["group_id1", "group_id2"]
        },
        {
            "@search.action": "upload",
            "file_id": "3",
            "file_name": "secured_file_c",
            "group_ids": ["group_id5", "group_id6"]
        }
    ]
}
```

Als u een bestaand document bijwerken met de lijst van groepen wilt, kunt u de `merge` of `mergeOrUpload` actie:

```JSON
{
    "value": [
        {
            "@search.action": "mergeOrUpload",
            "file_id": "3",
            "group_ids": ["group_id7", "group_id8", "group_id9"]
        }
    ]
}
```

Voor volledige informatie over het toevoegen of bijwerken van documenten, u kunt lezen [bewerken van documenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).
   
## <a name="apply-the-security-filter"></a>Het beveiligingsfilter toepassen

Als u wilt documenten op basis van trim `group_ids` toegang, moet u een zoekopdracht met de opdracht een `group_ids/any(g:search.in(g, 'group_id1, group_id2,...'))` worden gefilterd, waarbij 'group_id1, group_id2...' zijn de groepen waartoe de verlener van de aanvraag zoeken behoort.
Dit filter komt overeen met alle documenten waarvoor de `group_ids` veld bevat een van de opgegeven id's.
Voor volledige informatie over het zoeken naar documenten met behulp van Azure Search, u kunt lezen [documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents).
Houd er rekening mee dat dit voorbeeld laat zien hoe u om te zoeken naar documenten met behulp van een POST-aanvraag.

Probleem met de HTTP POST-aanvraag:

```
POST https://[service name].search.windows.net/indexes/securedfiles/docs/search?api-version=[api-version]  
Content-Type: application/json  
api-key: [admin or query key]
```

Geef het filter in de hoofdtekst van de aanvraag:

```JSON
{
   "filter":"group_ids/any(g:search.in(g, 'group_id1, group_id2'))"  
}
```

Krijgt u de documenten terug wanneer `group_ids` "group_id1" of "group_id2" bevat. Met andere woorden, krijgt u de documenten die de aanvraagverlener lezen-toegang heeft.

```JSON
{
 [
   {
    "@search.score":1.0,
     "file_id":"1",
     "file_name":"secured_file_a",
   },
   {
     "@search.score":1.0,
     "file_id":"2",
     "file_name":"secured_file_b"
   }
 ]
}
```
## <a name="conclusion"></a>Conclusie

Dit is hoe u resultaten op basis van gebruikers-id en Azure Search kunt filteren `search.in()` functie. U kunt deze functie gebruiken om door te geven in principe-id's voor de aanvragende gebruiker om te vergelijken met de principal-id's die zijn gekoppeld aan elk doeldocument dat. Wanneer u een search-aanvraag wordt verwerkt, de `search.in` functie filtert zoekresultaten waarvoor geen van de beveiligings-principals van de gebruiker leestoegang hebben. De principal-id kunnen bestaan uit zoals beveiligingsgroepen, rollen of zelfs de identiteit van gebruikers.
 
## <a name="see-also"></a>Zie ook

+ [Active Directory-identiteit gebaseerd toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search-with-aad.md)
+ [Filters in Azure Search](search-filters.md)
+ [Beveiligings- en toegangsbeheer in Azure Search-bewerkingen](search-security-overview.md)