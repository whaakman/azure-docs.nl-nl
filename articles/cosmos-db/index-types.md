---
title: Typen van de index in Azure Cosmos DB
description: Overzicht van de typen index in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/5/2018
ms.author: rimman
ms.openlocfilehash: f3c5d7bc1907e94ff2e590fe77cc531ac4b01f4c
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628998"
---
# <a name="index-types-in-azure-cosmos-db"></a>Typen van de index in Azure Cosmos DB

Er zijn meerdere opties waar u het indexeringsbeleid voor een pad configureren. U kunt een of meer indexering definities voor elk pad opgeven:

- **Gegevenstype:** tekenreeks, getal, punt, Polygon of LineString (kan slechts één vermelding per gegevenstype per pad bevatten).

- **Index-type:** Hash (gelijkheid query's), bereik (gelijkheid, bereik of ORDER BY-query's) of Spatial (ruimtelijke query's).

- **Precisie:** voor een hashindex dit varieert van 1 tot en met 8 voor zowel tekenreeksen en getallen en de standaardwaarde is 3. Voor de index van een bereik is de maximale precisiewaarde -1. Dit kan verschillen tussen 1 en 100 (maximumprecisie) voor de tekenreeks of numerieke waarden.

## <a name="index-kind"></a>Index-type

Azure Cosmos DB biedt ondersteuning voor Hash-index en bereik index voor elk pad die kan worden geconfigureerd voor tekenreeks of getal gegevenstypen, of beide.

- **Hash-index** biedt ondersteuning voor efficiënte gelijkheid en JOIN-query's. Voor de meeste gevallen nodig geen Hash-indexen een grotere precisie dan de standaardwaarde van 3 bytes. Het gegevenstype mag tekenreeks of getal.

- **Bereik index** biedt ondersteuning voor efficiënte gelijkheid query's, bereik-query's (met behulp van >, <>, =, < =,! =), en ORDER BY-query's. ORDER By-query's standaard vereist ook maximale index precisie (-1). Het gegevenstype mag tekenreeks of getal.

- **Ruimtelijke index** ondersteunt efficiënte ruimtelijke (binnen en afstand) query's. Het gegevenstype mag punt, Polygon of LineString. Azure Cosmos DB ondersteunt ook het type van de ruimtelijke index voor elk pad dat kan worden opgegeven voor de gegevenstypen punt, Polygon of LineString. De waarde in het opgegeven pad moet een geldige GeoJSON-fragment, zoals {"type": "Punt", "coördinaten": [0.0, 10.0]}. Azure Cosmos DB biedt ondersteuning voor automatische indexering van punt veelhoek en LineString gegevenstypen.

Hier volgen enkele voorbeelden van query's die Hash, bereik en ruimtelijke indexen kunnen worden gebruikt voor het bieden van:

| **Index-type** | **Beschrijving/use-case** |
| ---------- | ---------------- |
| Hash  | Hash-via/prop /? (of /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer uit verzameling c waar c.prop = "waarde"<br><br>Hash voor/eigenschappen / [] /? (of / of/eigenschappen /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer taggen van verzameling c JOIN-tag IN c.props waar tag = 5  |
| Bereik  | Bereik via/prop /? (of /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer uit verzameling c waar c.prop = "waarde"<br><br>Selecteer uit verzameling c waar c.prop > 5<br><br>Selecteer uit verzameling c ORDER BY c.prop   |
| Ruimtelijk     | Bereik via/prop /? (of /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer uit de verzameling-c<br><br>WAAR ST_DISTANCE (c.prop, {"type": "Wijst", "coördinaten": [0.0, 10.0]}) < 40<br><br>Selecteer uit verzameling c waar ST_WITHIN(c.prop, {"type": "Polygon",...})--met het indexeren van punten ingeschakeld<br><br>Selecteer uit verzameling c waar ST_WITHIN({"type": "Point",...}, c.prop)--met indexering voor veelhoeken ingeschakeld.     |

## <a name="default-behavior-of-index-kinds"></a>Standaardgedrag index soorten

- Als er geen index van het bereik van elke precisie is om aan te geven dat een scan mogelijk nodig om de query in een dergelijk geval standaard een fout geretourneerd voor query's met bereik operators zoals > =.

- Bereik-query's kunnen worden uitgevoerd zonder een bereik-index met behulp van de header 'x-ms-documentdb-enable-scan' in de REST-API of de optie 'EnableScanInQuery'-aanvraag met behulp van de .NET SDK. Als er geen andere filters in de query die Azure Cosmos DB de index gebruiken kunt om te filteren op basis van, wordt geen fout geretourneerd.

- Standaard wordt een fout geretourneerd voor ruimtelijke query's als er geen een ruimtelijke index, of andere filters die kunnen worden weergegeven in de index. Dergelijke query's kunnen worden uitgevoerd als een scan met behulp van x-ms-documentdb-enable-scan of EnableScanInQuery.

## <a name="index-precision"></a>Index precisie

- Index precisie kunt u een afweging maken tussen de overhead van indexopslag en prestaties van query's. Voor getallen, wordt u aangeraden de standaardconfiguratie van de precisie van-1 (maximum). Omdat de getallen 8 bytes in JSON, is dit gelijk aan een configuratie van 8 bytes. Het kiezen van een lagere waarde voor de precisie, zoals 1 tot en met 7, index betekent dat de waarden binnen bepaalde bereiken worden toegewezen aan dezelfde-vermelding. Daarom kunt u opslagruimte in de index verminderen, maar het uitvoeren van query's mogelijk voor het verwerken van meer items. Als gevolg daarvan kunnen verbruikt deze meer doorvoer ru's.

- Index precisie heeft meer praktische toepassingen met bereiken van de tekenreeks. Omdat tekenreeksen elke willekeurige lengte zijn kunnen, kan de prestaties van tekenreeks bereik-query's door de keuze van de precisie van de index beïnvloeden. Het ook mogelijk van invloed op de hoeveelheid opslagruimte index die is vereist. Tekenreeks bereik indexen kunnen worden geconfigureerd met een index precisie tussen 1 en 100 of -1 (maximum). Als u uitvoeren ORDER BY query's op de eigenschappen van een verbindingsreeks wilt, moet u een precisie van-1 voor de bijbehorende paden opgeven.

- Ruimtelijke indexen gebruik altijd de precisie van de index standaard voor alle typen (Point, LineString en Veelhoek). De precisie van de index standaard voor ruimtelijke indexen kan niet worden overschreven.

Azure Cosmos DB, wordt er een fout geretourneerd wanneer een query maakt gebruik van ORDER BY, maar beschikt niet over een bereik-index op basis van het aangevraagde pad met de maximale precisie.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het indexeren in Azure Cosmos DB, de volgende artikelen:

- [Overzicht van het indexeren](index-overview.md)
- [Indexeringsbeleid](indexing-policies.md)
- [Index paden](index-paths.md)

