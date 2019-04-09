---
title: Typen van de index in Azure Cosmos DB
description: Overzicht van de typen index in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.openlocfilehash: 5e7ee7c0bdfd0cff6be182e6d087cc264910e440
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59271557"
---
# <a name="index-types-in-azure-cosmos-db"></a>Typen van de index in Azure Cosmos DB

Er zijn meerdere opties bij het configureren van het indexeringsbeleid voor een pad. U kunt een of meer indexering definities voor elk pad opgeven:

- **Gegevenstype:** Tekenreeks, getal, punt, Polygon of LineString (kan slechts één vermelding per gegevenstype per pad bevatten).

- **Index-type:** Bereik (voor gelijkheid, bereik of ORDER BY-query's), of Spatial (voor ruimtelijke query's).

- **Precisie:** Voor de index van een bereik is de maximale precisiewaarde -1, dit is de standaardinstelling.

## <a name="index-kind"></a>Index-type

Azure Cosmos DB ondersteunt bereik index voor elk pad die kan worden geconfigureerd voor tekenreeks of getal gegevenstypen, of beide.

- **Bereik index** ondersteunt gelijkheid efficiënt query's, de JOIN-query's, de bereik-query's (met behulp van >, <>, =, < =,! =), en ORDER BY-query's. ORDER BY-query's, vereist standaard ook maximale index precisie (-1). Het gegevenstype mag tekenreeks of getal.

- **Ruimtelijke index** ondersteunt efficiënte ruimtelijke (binnen en afstand) query's. Het gegevenstype mag punt, Polygon of LineString. Azure Cosmos DB ondersteunt ook het type van de ruimtelijke index voor elk pad dat kan worden opgegeven voor de gegevenstypen punt, Polygon of LineString. De waarde in het opgegeven pad moet een geldige GeoJSON-fragment, zoals {"type": "Point", "coordinates": [0.0, 10.0]}. Azure Cosmos DB biedt ondersteuning voor automatische indexering van punt veelhoek en LineString gegevenstypen.

Hier volgen enkele voorbeelden van query's die variëren en ruimtelijke indexen kunnen worden gebruikt voor het bieden van:

| **Index-type** | **Beschrijving/use-case** |
| ---------- | ---------------- |
| Bereik      | Bereik via/prop /? (of /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer uit verzameling c waar c.prop = "waarde"<br><br>Selecteer uit verzameling c waar c.prop > 5<br><br>Selecteer uit verzameling c ORDER BY c.prop<br><br>Bereik via/Eigenschappen / [] /? (of / of/eigenschappen /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer taggen van verzameling c JOIN-tag IN c.props waar tag = 5  |
| Ruimtelijk    | Bereik via/prop /? (of /) kan worden gebruikt om de volgende query's efficiënt fungeren:<br><br>Selecteer in de verzameling c waar ST_DISTANCE(c.prop, {"type": 'Point', "coördinaten": [0.0, 10.0]}) < 40<br><br>Selecteer in de verzameling c waar ST_WITHIN(c.prop, {"type": 'Point',...}) --met het indexeren van punten ingeschakeld<br><br>Selecteer in de verzameling c waar ST_WITHIN({"type": "Veelhoek',...}, c.prop)--met indexering voor veelhoeken ingeschakeld. |

## <a name="default-behavior-of-index-kinds"></a>Standaardgedrag index soorten

- Als er geen index van het bereik van elke precisie is om aan te geven dat een scan mogelijk nodig om de query in een dergelijk geval standaard een fout geretourneerd voor query's met bereik operators zoals > =.

- Bereik-query's kunnen worden uitgevoerd zonder een bereik-index met behulp van de **x-ms-documentdb-enable-scan** -header in de REST-API of de **EnableScanInQuery** optie vragen via de .NET SDK. Als er geen andere filters in de query die Azure Cosmos DB de index gebruiken kunt om te filteren op basis van, wordt geen fout geretourneerd.

- Standaard wordt een fout geretourneerd voor ruimtelijke query's als er geen een ruimtelijke index, of andere filters die kunnen worden weergegeven in de index. Dergelijke query's kunnen worden uitgevoerd als een scan met behulp van **x-ms-documentdb-enable-scan** of **EnableScanInQuery**.

## <a name="index-precision"></a>Index precisie

> [!NOTE]
> Azure Cosmos-containers ondersteunen een nieuwe indexindeling die niet langer een aangepaste index precisie dan de maximale precisiewaarde (-1 vereist). Met deze methode worden altijd paden geïndexeerd met de maximale precisie. Als u een precisiewaarde voor het indexeringsbeleid opgeeft, wordt de CRUD-aanvragen op een containers wordt het secondedeel zonder interactie te negeren en het antwoord van de container bevat alleen de maximale precisiewaarde (-1).  Alle nieuwe Cosmos-containers maken standaard gebruik van de nieuwe indexindeling.

- Index precisie kunt u een afweging maken tussen de opslagoverhead en de prestaties van query's. Voor getallen, wordt u aangeraden de standaardconfiguratie van de precisie van-1 (het maximum). Omdat de getallen 8 bytes in JSON, is dit gelijk aan een configuratie van 8 bytes. Het kiezen van een lagere waarde voor de precisie, zoals 1 tot en met 7, index betekent dat de waarden binnen bepaalde bereiken worden toegewezen aan dezelfde-vermelding. Daarom kunt u de opslagruimte index beperken, maar het uitvoeren van query's mogelijk voor het verwerken van meer items. Als gevolg daarvan kunnen verbruikt deze meer doorvoer ru's.

- Index precisie heeft meer praktische toepassingen met bereiken van de tekenreeks. Omdat tekenreeksen elke willekeurige lengte zijn kunnen, kan de prestaties van tekenreeks bereik-query's door de keuze van de precisie van de index beïnvloeden. Het ook mogelijk van invloed op de hoeveelheid opslagruimte index die is vereist. Tekenreeks bereik indexen kunnen worden geconfigureerd met een index precisie tussen 1 en 100 of -1 (maximum). Als u uitvoeren ORDER BY query's op de eigenschappen van een verbindingsreeks wilt, moet u een precisie van-1 voor de bijbehorende paden opgeven.

- Ruimtelijke indexen gebruik altijd de precisie van de index standaard voor alle typen (Point, LineString en Veelhoek). De precisie van de index standaard voor ruimtelijke indexen kan niet worden overschreven.

Azure Cosmos DB, wordt er een fout geretourneerd wanneer een query maakt gebruik van ORDER BY, maar beschikt niet over een bereik-index op basis van het aangevraagde pad met de maximale precisie.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het indexeren in Azure Cosmos DB, de volgende artikelen:

- [Overzicht van het indexeren](index-overview.md)
- [Indexeringsbeleid](indexing-policies.md)
- [Indexpaden](index-paths.md)

