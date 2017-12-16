---
title: Beveiligingsfilters voor bijsnijden resulteert in Azure Search | Microsoft Docs
description: Toegangsbeheer voor Azure Search-inhoud met behulp van beveiligingsfilters en gebruikers-id's.
ms.custom: 
ms.date: 08/07/2017
ms.service: search
ms.reviewer: 
ms.suite: 
ms.tgt_pltfrm: 
caps.latest.revision: "26"
author: revitalbarletz
ms.author: revitalb
manager: jlembicz
ms.openlocfilehash: c829399f9c21846d8ee5b43945e2565565279820
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="security-filters-for-trimming-results-in-azure-search"></a>Beveiligingsfilters voor bijsnijden resulteert in Azure Search

U kunt de beveiligingsfilters zodat zoekresultaten in Azure Search op basis van gebruikersidentiteit trim toepassen. Deze zoekervaring vereist gewoonlijk de identiteit van degene die aanvragen zoeken op basis van een veld met de principes die gemachtigd zijn om het document te vergelijken. Heeft toegang tot het document dat wanneer een overeenkomst is gevonden, de gebruiker of de principal (zoals een groep of rol).

Eén manier om een beveiliging voor het filteren is via een gecompliceerde splitsing van gelijkheid expressies: bijvoorbeeld `Id eq 'id1' or Id eq 'id2'`, enzovoort. Deze aanpak is gevoelig voor fouten, moeilijk te onderhouden, en in gevallen waarbij de lijst honderden of duizenden waarden bevat vertraagt de reactietijd van de query door het aantal seconden. 

Een benadering eenvoudiger en sneller is via de `search.in` functie. Als u `search.in(Id, 'id1, id2, ...')` in plaats van een expressie gelijkheid kunt u onderliggende seconde reactie verwachten tijden.

In dit artikel wordt beschreven hoe u Beveiligingsfiltering met de volgende stappen uitvoeren:
> [!div class="checklist"]
> * Maken van een veld waarin de principal-id 's 
> * Push of bestaande documenten bijwerken met de relevante principal-id 's
> * Een aanvraag zoeken met `search.in``filter`

>[!NOTE]
> Het proces van het ophalen van de principal-id wordt niet in dit document besproken. U krijgt rechtstreeks via uw serviceprovider voor identiteit.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u hebt een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F), [Azure Search-service](https://docs.microsoft.com/azure/search/search-create-service-portal), en [Azure Search-Index](https://docs.microsoft.com/azure/search/search-create-index-portal).  

## <a name="create-security-field"></a>Beveiliging veld maken

Uw documenten vergezeld gaan van een veld opgeven welke groepen toegang hebben. Deze informatie wordt de filtercriteria op basis waarvan documenten zijn geselecteerd of geweigerd in de resultatenset geretourneerd naar de verlener.
Stel dat we een index van beveiligde bestanden hebben en elk bestand toegankelijk voor een andere set van gebruikers is.
1. Veld toevoegen `group_ids` (u kunt een willekeurige naam hier) als een `Collection(Edm.String)`. Controleer of het veld heeft een `filterable` -kenmerk ingesteld op `true` zodat de zoekresultaten worden gefilterd op basis van de toegang tot de gebruiker heeft. Als u bijvoorbeeld de `group_ids` veld `["group_id1, group_id2"]` voor het document met `file_name` 'secured_file_b', alleen gebruikers die deel uitmaken van groep-id's 'group_id1' of 'group_id2' hebben toegang tot het bestand lezen.
   Zorg ervoor dat het veld `retrievable` kenmerk is ingesteld op `false` zodat deze niet wordt geretourneerd als onderdeel van de zoekopdracht.
2. Ook toevoegen `file_id` en `file_name` velden in dit voorbeeld.  

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

## <a name="pushing-data-into-your-index-using-the-rest-api"></a>Gegevens worden gepusht in uw index met behulp van de REST-API
  
Een HTTP POST-aanvraag doet bij uw index URL-eindpunt. De hoofdtekst van de HTTP-aanvraag is een JSON-object met de documenten die moeten worden toegevoegd:

```
POST https://[search service].search.windows.net/indexes/securedfiles/docs/index?api-version=[api-version]  
Content-Type: application/json
api-key: [admin key]
```

Geef de inhoud van uw documenten in de aanvraagtekst:

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

Als u bijwerken van een bestaand document met de lijst met groepen wilt, kunt u de `merge` of `mergeOrUpload` actie:

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

Voor volledige informatie over het toevoegen of bijwerken van documenten, kunt u lezen [bewerken van documenten](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).
   
## <a name="apply-the-security-filter"></a>Het beveiligingsfilter toepassen

Om documenten op basis van trim `group_ids` toegang, moet u een zoekopdracht met de opdracht een `group_ids/any(g:search.in(g, 'group_id1, group_id2,...'))` filter, waarbij 'group_id1 group_id2...' zijn de groepen waartoe de verlener van de aanvraag zoeken behoort.
Dit filter komt overeen met alle documenten waarvoor de `group_ids` veld bevat een van de opgegeven id's.
Voor volledige informatie over het zoeken naar documenten met behulp van Azure Search kunt u lezen [documenten zoeken](https://docs.microsoft.com/rest/api/searchservice/search-documents).
Houd er rekening mee dat dit voorbeeld ziet u hoe u documenten met behulp van een POST-aanvraag zoeken.

Probleem met de HTTP POST-aanvraag:

```
POST https://[service name].search.windows.net/indexes/securedfiles/docs/search?api-version=[api-version]  
Content-Type: application/json  
api-key: [admin or query key]
```

Geef het filter in de aanvraagtekst:

```JSON
{
   "filter":"group_ids/any(g:search.in(g, 'group_id1, group_id2'))"  
}
```

Krijgt u de documenten terug wanneer `group_ids` 'group_id1' of 'group_id2' bevat. Met andere woorden, krijgt u de documenten waartoe de uitgever van de aanvraag leestoegang heeft.

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

Dit is hoe u resultaten op basis van gebruiker en de Azure Search kunt filteren `search.in()` functie. U kunt deze functie gebruiken om door te geven in de principal-id's voor de aanvragende gebruiker voor het vergelijken van de principal-id's die zijn gekoppeld aan elk doeldocument. Wanneer een zoekaanvraag is afgehandeld, de `search.in` functie gefilterd zoekresultaten waarvoor geen van de gebruiker principals leestoegang hebben. De principal-id kunnen bestaan uit items zoals beveiligingsgroepen, rollen of zelfs de identiteit van gebruiker.
 
## <a name="see-also"></a>Zie ook

+ [Active Directory-identiteit gebaseerd toegangsbeheer met Azure Search-filters](search-security-trimming-for-azure-search-with-aad.md)
+ [Filters in Azure Search](search-filters.md)
+ [Beveiliging en toegang beheren in Azure Search-bewerkingen](search-security-overview.md)