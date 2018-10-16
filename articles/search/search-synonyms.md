---
pageTitle: Synonyms in Azure Search | Microsoft Docs
description: Synoniemen gebruiken om uit te breiden het bereik van een zoekopdracht
authors: mhko
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 04/20/2018
manager: jlembicz
ms.author: nateko
ms.openlocfilehash: 9f887b065cf4fcc295873ee969030c67d17d9e2f
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49318466"
---
# <a name="synonyms-in-azure-search"></a>Synoniemen Azure Search

Synoniemen in zoekmachines equivalente termen die impliciet uitbreiden van het bereik van een query, zonder dat de gebruiker deze op te geven daadwerkelijk de term worden gekoppeld. Bijvoorbeeld, valt gezien de term 'hond' als synoniem koppelingen van "canine" en 'puppy' alle documenten met "hond", "hondachtige" of "puppy" binnen het bereik van de query.

In Azure Search wordt synoniem uitbreiding uitgevoerd op moment dat de query. U kunt synoniementoewijzingen toevoegen aan een service met geen onderbreking van bestaande bewerkingen. U kunt toevoegen een **synonymMaps** eigenschap aan de velddefinitie van een zonder de index opnieuw opbouwen.

## <a name="feature-availability"></a>Beschikbaarheid van functies

De synoniemenfunctie wordt ondersteund in de meest recente api-versie (api-version = 2017-11-11). Azure Portal biedt er momenteel geen ondersteuning voor.

## <a name="how-to-use-synonyms-in-azure-search"></a>Over het gebruik van synoniemen in Azure search

In Azure Search is synoniem ondersteuning gebaseerd op synoniementoewijzingen die u definieert en uploadt naar uw service. Deze kaarten deel uitmaken van een onafhankelijke resource (zoals indexen of gegevensbronnen) en kunnen worden gebruikt door elk doorzoekbaar veld in een index in uw zoekservice.

Synoniem wordt toegewezen en indexen onafhankelijk van elkaar worden bijgehouden. Nadat u een synoniemtoewijzing definiëren en te naar uw service uploaden, kunt u de functie synoniem op een veld inschakelen door toe te voegen een nieuwe eigenschap, genaamd **synonymMaps** in de velddefinitie van het. Het maken, bijwerken en verwijderen van een synoniemtoewijzing is altijd een geheel document-bewerking, wat betekent dat u kan niet maken, bijwerken of verwijderen van onderdelen van de synoniemtoewijzing incrementeel. Zelfs een afzonderlijke vermelding wordt bijgewerkt, moet een opnieuw laden.

Synoniemen opnemen in uw zoektoepassing is een proces in twee stappen:

1.  Voeg een synoniemtoewijzing toe aan uw search-service via de onderstaande API's.  

2.  Configureer een doorzoekbaar veld voor het gebruik van de synoniemtoewijzing in de indexdefinitie.

### <a name="synonymmaps-resource-apis"></a>SynonymMaps Resource API 's

#### <a name="add-or-update-a-synonym-map-under-your-service-using-post-or-put"></a>Toevoegen of bijwerken van een synoniemtoewijzing onder uw service, met behulp van posten of te plaatsen.

Synoniementoewijzingen zijn geüpload naar de service via post- of PUT. Elke regel moet worden gescheiden door een teken voor de nieuwe regel ('\n'). U kunt maximaal 5000 regels per synoniemtoewijzing in een gratis service en 10.000 regels in alle andere SKU's definiëren. Elke regel kan maximaal 20 uitbreidingen hebben.

Synoniementoewijzingen moeten zich in de indeling van Apache Solr die hieronder wordt uitgelegd. Als u een bestaande synoniem woordenlijst in een andere indeling hebt en rechtstreeks gebruiken, laat het ons weten op [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

U kunt een nieuwe synoniemtoewijzing met behulp van HTTP POST, zoals in het volgende voorbeeld maken:

    POST https://[servicename].search.windows.net/synonymmaps?api-version=2017-11-11
    api-key: [admin key]

    {  
       "name":"mysynonymmap",
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

U kunt ook gebruik van PUT en geef de naam van de kaart synoniem op de URI. Als de synoniemtoewijzing niet bestaat, wordt deze gemaakt.

    PUT https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2017-11-11
    api-key: [admin key]

    {  
       "format":"solr",
       "synonyms": "
          USA, United States, United States of America\n
          Washington, Wash., WA => WA\n"
    }

##### <a name="apache-solr-synonym-format"></a>Apache Solr synoniem indeling

De indeling Solr ondersteunt gelijkwaardige en expliciete synoniementoewijzingen. Regels voor apparaatgroeptoewijzing voldoen aan de open-source synoniem filterspecificatie van Apache Solr, zoals beschreven in dit document: [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Hieronder volgt een van voorbeeldregel voor equivalente synoniemen.
```
USA, United States, United States of America
```

"VS" wordt aan de regel hierboven een zoekquery uitbreiden naar "VS" of "VS" of 'Verenigde Staten van Amerika'.

Expliciete toewijzing wordt aangeduid met een pijl "= > '. Als u opgeeft, een reeks term van een zoekopdracht die overeenkomt met de linkerzijde van ' = > ' wordt vervangen door de alternatieven aan de rechterkant. Vermeld hieronder de regel, zoekquery's 'Washington', 'Washington' of "WA" wordt alle worden herschreven voor "WA". Expliciete toewijzing is alleen van toepassing is in de opgegeven richting en Herschrijf de query "WA" naar "Groningen" niet in dit geval.
```
Washington, Wash., WA => WA
```

#### <a name="list-synonym-maps-under-your-service"></a>Lijst met synoniem toegewezen onder uw service.

    GET https://[servicename].search.windows.net/synonymmaps?api-version=2017-11-11
    api-key: [admin key]

#### <a name="get-a-synonym-map-under-your-service"></a>Een synoniemtoewijzing onder uw service ophalen.

    GET https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2017-11-11
    api-key: [admin key]

#### <a name="delete-a-synonyms-map-under-your-service"></a>Een kaart synoniemen onder uw service verwijderen.

    DELETE https://[servicename].search.windows.net/synonymmaps/mysynonymmap?api-version=2017-11-11
    api-key: [admin key]

### <a name="configure-a-searchable-field-to-use-the-synonym-map-in-the-index-definition"></a>Configureer een doorzoekbaar veld voor het gebruik van de synoniemtoewijzing in de indexdefinitie.

De eigenschap van een nieuw veld **synonymMaps** kan worden gebruikt om op te geven van een synoniemtoewijzing moet worden gebruikt voor een doorzoekbaar veld. Synoniementoewijzingen zijn service level resources en kunnen worden verwezen door een veld van een index van de service.

    POST https://[servicename].search.windows.net/indexes?api-version=2017-11-11
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

**synonymMaps** kan worden opgegeven voor de doorzoekbare velden van het type 'Edm.String' of 'Collection(Edm.String)'.

> [!NOTE]
> U kunt slechts één synoniem toewijzen per veld hebben. Als u gebruiken van meerdere synoniementoewijzingen wilt, laat het ons weten op [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="impact-of-synonyms-on-other-search-features"></a>Gevolgen van synoniemen voor andere functies voor het zoeken

De synoniemenfunctie herschrijft de oorspronkelijke query met synoniemen met de OR-operator. Om deze reden behandelen treffers markeren en scoreprofielen de oorspronkelijke termijn en synoniemen als gelijkwaardig.

Synoniem functie is van toepassing op zoekquery's en geldt niet voor filters of facetten. Op deze manier zijn suggesties gebaseerd op de oorspronkelijke termijn; synoniem overeenkomsten worden niet weergegeven in het antwoord.

Synoniem uitbreidingen niet van toepassing op jokertekens zoektermen; voorvoegsel, fuzzy, en de regex-voorwaarden niet worden uitgebreid.

Als u één query die van toepassing synoniem uitbreiding en jokertekens, reguliere expressie of fuzzy zoekopdrachten is uitvoeren wilt, kunt u de query's met de OR-syntaxis kunt combineren. Bijvoorbeeld, de term zou zijn als u wilt combineren synoniemen met jokertekens voor eenvoudige query-syntaxis, `<query> | <query>*`.

## <a name="tips-for-building-a-synonym-map"></a>Tips voor het bouwen van een synoniemtoewijzing

- Een beknopte, goed ontworpen synoniemtoewijzing is efficiënter dan een volledige lijst van mogelijke overeenkomsten. Uitzonderlijk groot of complex woordenlijsten duren parseren en van invloed zijn op de latentie van de query als de query veel synoniemen breidt langer. In plaats van de schatting waarmee voorwaarden kunnen worden gebruikt, krijgt u de werkelijke voorwaarden via een [verkeer analyserapport zoeken](search-traffic-analytics.md).

- Als een inleiding en de validatie oefenen, inschakelen en gebruik vervolgens dit rapport om precies te bepalen welke voorwaarden wordt profiteren van een overeenkomst synoniem, en ga vervolgens door deze te gebruiken als de validatie van de synoniemtoewijzing productie van een betere resultaten. In een vooraf gedefinieerd rapport geeft de tegels "meest voorkomende zoekquery's ' en 'nul resultaat zoekquery's ' u de benodigde informatie.

- U kunt meerdere synoniementoewijzingen maken voor uw zoektoepassing (bijvoorbeeld door de taal als de toepassing een meertalige klantenbestand ondersteunt). Een veld kan een van beide op dit moment alleen kunnen gebruiken. U kunt de eigenschap synonymMaps van een veld op elk gewenst moment bijwerken.

## <a name="next-steps"></a>Volgende stappen

- Als u een bestaande index in een ontwikkelomgeving (niet-productie) hebt, kunt u experimenteren met een klein dictionary treffers markeren om te zien hoe het toevoegen van synoniemen verandert de zoekfunctie, met inbegrip van invloed op scoreprofielen en suggesties.

- [Analyse zoekverkeer inschakelen](search-traffic-analytics.md) en gebruikt u het vooraf gedefinieerde Power BI-rapport voor meer informatie over welke voorwaarden voor de meeste worden gebruikt en welke nul documenten retourneren. Gewapend met deze inzichten, wijzig de woordenlijst om op te nemen van synoniemen voor niet-productieve query's die moeten worden omgezet naar documenten in uw index.
