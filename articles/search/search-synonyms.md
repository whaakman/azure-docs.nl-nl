---
pageTitle: Synonyms in Azure Search (preview) | Microsoft Docs
description: Voorlopige documentatie voor de functie synoniemen (preview), worden weergegeven in de Azure Search REST-API.
services: search
documentationCenter: 
authors: mhko
manager: pablocas
editor: 
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/07/2016
ms.author: nateko
ms.openlocfilehash: 447abc48cca3dee398e641f8458e52a5b2cb8e42
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="synonyms-in-azure-search-preview"></a>Synoniemen in Azure Search (preview)

Synoniemen in zoekmachines koppelen vergelijkbare termen die impliciet uitbreiden van het bereik van een query, zonder dat de gebruiker naar de term daadwerkelijk te bieden. Bijvoorbeeld, vallen gezien de term 'aquaduct' en synoniem koppelingen van 'canine' en 'puppy' alle documenten met 'aquaduct', 'hondachtige' of 'puppy' binnen het bereik van de query.

In Azure Search wordt synoniem uitbreiding uitgevoerd op moment dat de query. U kunt synoniem maps toevoegen aan een service met niets onderbreking van bewerkingen voor bestaande. U kunt toevoegen een **synonymMaps** eigenschap in op de velddefinitie van een zonder dat de index opnieuw maken.

## <a name="feature-availability"></a>Beschikbaarheid van functies

De functie synoniemen is momenteel in preview en alleen ondersteund in de meest recente preview-api-versie (api-version = 2016-09-01-Preview). Azure Portal biedt er momenteel geen ondersteuning voor. Omdat de API-versie van de aanvraag is opgegeven, is het mogelijk om te worden gecombineerd (GA) is algemeen beschikbaar en preview-API's in dezelfde app. Preview-API's niet onder de SERVICEOVEREENKOMST en de functies zijn kan echter wel wijzigen, zodat u kunt beter niet met deze in productietoepassingen.

## <a name="how-to-use-synonyms-in-azure-search"></a>Het gebruik van synoniemen in Azure search

In Azure Search is synoniem ondersteuning gebaseerd op synoniem kaarten die u definieert en uploaden naar uw service. Deze toewijzingen deel uitmaken van een onafhankelijke resource (zoals indexen of gegevensbronnen) en kunnen worden gebruikt door een doorzoekbaar veld in een index in uw zoekservice.

Synoniem toegewezen en indexen onafhankelijk worden bewaard. Zodra u een kaart synoniem definiëren en dit naar uw service uploaden, kunt u de functie synoniem op een veld inschakelen door een nieuwe eigenschap toe te voegen **synonymMaps** in de velddefinitie van het. Maken, bijwerken en verwijderen van een synoniem-kaart is altijd een hele document-bewerking, wat betekent dat u niet kunt maken, bijwerken of onderdelen van de kaart synoniem incrementeel verwijderen. Zelfs een afzonderlijke vermelding bijwerken vereist een opnieuw laden.

Synoniemen opnemen in uw toepassing search is een proces in twee stappen:

1.  Een kaart synoniem toevoegen aan uw search-service via de onderstaande API's.  

2.  Configureer een doorzoekbaar veld voor het gebruik van de kaart synoniem in de definitie van de index.

### <a name="synonymmaps-resource-apis"></a>SynonymMaps Resource API 's

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>Toevoegen of bijwerken van een kaart synoniem onder uw service en met behulp van posten of te plaatsen.

Synoniem maps worden geüpload naar de service via POST of PUT. Elke regel moet worden gescheiden door het nieuwe regelteken (\n). U kunt maximaal 5000 regels per synoniem kaart in een gratis service en 10.000 regels in andere SKU's definiëren. Elke regel kan maximaal 20 uitbreidingen hebben.

In dit voorbeeld, moeten synoniem maps zich in de indeling van Apache Solr die hieronder wordt beschreven. Als u een bestaande synoniem woordenlijst in een andere indeling hebt en wilt rechtstreeks gebruiken, laat ons weten op [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

U kunt een nieuwe synoniem-toewijzing met behulp van HTTP POST, zoals in het volgende voorbeeld maken:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

U kunt ook gebruik van opslag en geef de naam van de kaart synoniem op de URI. Als de kaart synoniem niet bestaat, wordt deze gemaakt.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr synoniem indeling

De indeling Solr ondersteunt gelijkwaardige en expliciete synoniem toewijzingen. Regels voor apparaatgroeptoewijzing voldoen aan de open-source synoniem filterspecificatie van Apache Solr, in dit document beschreven: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Hieronder volgt een voorbeeldregel voor de equivalente synoniemen.
```
USA, United States, United States of America
```

Met de regel boven een zoekopdracht wilt uitbreiden, 'VS' naar 'VS' of 'Verenigde Staten' of 'Verenigde Staten van Amerika'.

Expliciete toewijzing wordt aangeduid met een pijl ' = > '. Als u opgeeft, een reeks termijn van een zoekopdracht die overeenkomt met de linkerkant van ' = > ' wordt vervangen door de alternatieven aan de rechterkant. De onderstaande regel opgegeven, zoekquery's 'Washington', 'Staat Washington' of "WA" alle herschreven "WA". Expliciete toewijzing is alleen van toepassing is in de opgegeven richting en Herschrijf de query "WA" naar 'Washington' niet in dit geval.
```
Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Lijst synoniem maps onder uw service.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Een kaart synoniem onder uw service ophalen.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Verwijderen van een kaart synoniemen onder uw service.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2016-09-01-Preview
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a>Configureer een doorzoekbaar veld voor het gebruik van de kaart synoniem in de definitie van de index.

De eigenschap van een nieuw veld **synonymMaps** kan worden gebruikt om op te geven van een synoniem kaart moet worden gebruikt voor een doorzoekbaar veld. Synoniem maps service level bronnen en kunnen worden verwezen vanuit elk veld van een index van de service.

    POST https://[servicename].search.windows.net/indexes?api-version=2016-09-01-Preview
    api-key: [admin key]

    {
       "name":"myindex",
       "fields":[
          {
             "name":"id",
             "type":"Edm.String",
             "key":true
          },
          {
             "name":"name",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"en.lucene",
             "synonymMaps":[
                "mysynonymmap"
             ]
          },
          {
             "name":"name_jp",
             "type":"Edm.String",
             "searchable":true,
             "analyzer":"ja.microsoft",
             "synonymMaps":[
                "japanesesynonymmap"
             ]
          }
       ]
    }

**synonymMaps** kan worden opgegeven voor de doorzoekbare velden van het type 'Edm.String' of 'Verzameling (EDM.String)'.

> [!NOTE]
> In dit voorbeeld, kunt u slechts één synoniem toewijzen per veld hebben. Als u gebruiken van meerdere synoniem maps wilt, laat ons weten op [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="impact-of-synonyms-on-other-search-features"></a>Gevolgen van synoniemen voor andere zoekfuncties

De functie synoniemen herschrijft de oorspronkelijke query met synoniemen met de operator OR. Om deze reden behandelen treffers syntaxismarkering en -profielen scoren de oorspronkelijke termijn en synoniemen als equivalent.

Synoniem functie is van toepassing op zoekquery's en geldt niet voor filters of facetten. Op deze manier worden suggesties alleen gebaseerd op de oorspronkelijke termijn; synoniem overeenkomsten worden niet weergegeven in het antwoord.

Synoniem uitbreidingen zijn niet van toepassing op jokertekens zoektermen; voorvoegsel, bij benadering, en de regex-voorwaarden worden niet aangevuld.

## <a name="tips-for-building-a-synonym-map"></a>Tips voor het bouwen van een synoniem-kaart

- Een beknopte, goed ontworpen synoniem-kaart is efficiënter dan een volledige lijst van mogelijke overeenkomsten. Uitzonderlijk groot of complex woordenlijsten duren langer om te parseren en van invloed zijn op de latentie van de query als de query wordt uitgebreid naar veel synoniemen. In plaats van de schatting waarmee voorwaarden kunnen worden gebruikt, kunt u de werkelijke voorwaarden via krijgen een [verkeer analyserapport zoeken](search-traffic-analytics.md).

- Als u een inleiding en de validatie uitoefenen, inschakelen en vervolgens wordt gebruikt in dit rapport nauwkeurig bepalen welke voorwaarden wordt profiteren van een overeenkomst synoniem en ga verder als validatie van de kaart synoniem opstellen van een beter resultaat te gebruiken. In een vooraf gedefinieerd rapport, de tegels 'meest algemene zoekquery's ' en 'nul resultaat zoekquery's ', krijgt u de benodigde informatie.

- U kunt meerdere synoniem toewijzingen maken voor uw zoektoepassing (bijvoorbeeld per taal als de toepassing meerdere talen van de klant base ondersteunt). Een veld kunt een van beide op dit moment alleen kunnen gebruiken. U kunt de eigenschap synonymMaps van een veld op elk gewenst moment bijwerken.

## <a name="next-steps"></a>Volgende stappen

- Als u een bestaande index in een ontwikkelomgeving (niet-productieve) hebt, kunt u experimenteren met een kleine woordenlijst om te zien hoe de toevoeging van synoniemen verandert de zoekervaring, met inbegrip van de gevolgen voor score berekenen voor profielen, te markeren en suggesties.

- [Inschakelen van search traffic analytics](search-traffic-analytics.md) en gebruikt u het vooraf gedefinieerde Power BI-rapport voor meer informatie over welke termen het meest worden gebruikt en welke nul documenten retourneren. Het woordenboek om op te nemen synoniemen voor niet-productieve query's die moeten worden omgezet in documenten in uw index met deze inzichten worden worden herzien.
