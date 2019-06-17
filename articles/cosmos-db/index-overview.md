---
title: Indexeren in Azure Cosmos DB
description: Begrijp hoe indexering werkt in Azure Cosmos DB.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: thweiss
ms.openlocfilehash: 633d0f619132ee93951cfe0dc329a7514a38ef57
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66240743"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Indexeren in Azure Cosmos DB - overzicht

Azure Cosmos DB is een schema-agnostische database waarmee u het herhalen voor uw toepassing zonder dat u hoeft te bekommeren om schema's of indexbeheer. Standaard Azure Cosmos DB worden automatisch geïndexeerd elke eigenschap voor alle items in uw [container](databases-containers-items.md#azure-cosmos-containers) zonder te definiëren van een schema of secundaire indexen te configureren.

Het doel van dit artikel is waarin wordt uitgelegd hoe gegevens in Azure Cosmos DB worden geïndexeerd en hoe indexen wordt gebruikt om queryprestaties te verbeteren. Het verdient aanbeveling om te gaan via deze sectie daarna wordt ingegaan op het aanpassen van [indexeringsbeleid](index-policy.md).

## <a name="from-items-to-trees"></a>Van items die moeten worden structuren

Telkens wanneer een item is opgeslagen in een container, is de inhoud ervan weergegeven als een JSON-document vervolgens geconverteerd naar een boomstructuur. Dat houdt in dat elke eigenschap van dat item wordt weergegeven als een knooppunt in een boomstructuur. Een pseudo-hoofdknooppunt wordt als een ouder voor alle eigenschappen van de eerste niveau van het item gemaakt. De bladknooppunten bevatten de werkelijke scalaire waarden die worden uitgevoerd door een item.

Houd rekening met dit item als een voorbeeld:

    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 },
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }

Deze zou worden vertegenwoordigd door de volgende structuur:

![Het vorige item weergegeven als een boomstructuur](./media/index-overview/item-as-tree.png)

Houd er rekening mee hoe matrices worden gecodeerd in de structuur: elke vermelding in een matrix een tussenliggende knooppunt met het label met de index van dat item binnen de matrix opgehaald (0, 1 enz.).

## <a name="from-trees-to-property-paths"></a>Van structuren naar eigenschappaden

De reden waarom transformeert van items in de structuren in Azure Cosmos DB is omdat deze eigenschappen worden verwezen door de paden binnen deze structuren toestaat. We kunnen als u het pad voor een eigenschap, de structuur van het hoofdknooppunt met die eigenschap passeren en de labels van elk knooppunt van de betreffende samenvoegen.

Hier volgen de paden voor elke eigenschap van de voorbeeld-item die hierboven worden beschreven:

    /locations/0/country: "Germany"
    /locations/0/city: "Berlin"
    /locations/1/country: "France"
    /locations/1/city: "Paris"
    /headquarters/country: "Belgium"
    /headquarters/employees: 250
    /exports/0/city: "Moscow"
    /exports/1/city: "Athens"

Wanneer een item wordt geschreven, indexeert Azure Cosmos DB effectief van elke eigenschap pad en de bijbehorende waarde.

## <a name="index-kinds"></a>Index-typen

Azure Cosmos DB ondersteunt momenteel twee soorten indexen:

De **bereik** index type wordt gebruikt voor:

- gelijkheid query's: 

   ```sql SELECT * FROM container c WHERE c.property = 'value'```

- Bereik-query's: 

   ```sql SELECT * FROM container c WHERE c.property > 'value'``` (werkt voor `>`, `<`, `>=`, `<=`, `!=`)

- `ORDER BY` query's:

   ```sql SELECT * FROM container c ORDER BY c.property```

- `JOIN` query's: 

   ```sql SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'```

Bereik indexen kunnen worden gebruikt voor scalaire waarden (tekenreeks of getal).

De **ruimtelijke** index type wordt gebruikt voor:

- georuimtelijke afstand query's: 

   ```sql SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40```

- georuimtelijke binnen query's: 

   ```sql SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] } })```

Ruimtelijke indexen kunnen worden gebruikt op de juiste indeling [GeoJSON](geospatial.md) objecten. Punten, LineStrings en polygonen worden momenteel ondersteund.

De **samengestelde** index type wordt gebruikt voor:

- `ORDER BY` query's op meerdere eigenschappen: 

   ```sql SELECT * FROM container c ORDER BY c.firstName, c.lastName```

## <a name="querying-with-indexes"></a>Query's uitvoeren met indexen

De paden die is opgehaald bij het indexeren van gegevens wordt het eenvoudiger voor het opzoeken van de index bij het verwerken van een query. Door die overeenkomt met de `WHERE` component van een query met de lijst met geïndexeerde paden, is het mogelijk om de items die aan het querypredicaat zeer snel te identificeren.

Neem bijvoorbeeld de volgende query: `SELECT location FROM location IN company.locations WHERE location.country = 'France'`. Het pad met rood gemarkeerde wordt er gezocht naar het querypredicaat (filteren op items, te waarbij elke locatie "Frankrijk" heeft als de land/regio):

![Die overeenkomt met een specifiek pad binnen een structuur](./media/index-overview/matching-path.png)

> [!NOTE]
> Een `ORDER BY` -component die door één eigenschap orders *altijd* moet een bereik te indexeren en zal mislukken als het pad wordt verwezen naar een niet heeft. Op deze manier een meervoudige `ORDER BY` query *altijd* moet een samengestelde index.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het indexeren in de volgende artikelen:

- [Indexeringsbeleid](index-policy.md)
- [Indexeringsbeleid beheren](how-to-manage-indexing-policy.md)
