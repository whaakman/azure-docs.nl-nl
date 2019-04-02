---
title: Maak een synthetische partitiesleutel in Azure Cosmos DB aan uw gegevens en de belasting gelijkmatig verdelen.
description: Informatie over het gebruik van synthetische partitiesleutels in uw Azure Cosmos-containers
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.author: rimman
ms.openlocfilehash: 6b3499c36ae7abd03c4a1f1ca3a3a46e2c315120
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762732"
---
# <a name="create-a-synthetic-partition-key"></a>Een synthetische partitiesleutel maken

Het is de aanbevolen procedure voor een partitiesleutel met veel verschillende waarden, zoals honderden of duizenden hebben. Het doel is om uw gegevens en de belasting gelijkmatig verdelen over de items die zijn gekoppeld aan deze partitie-sleutelwaarden. Als een dergelijke eigenschap niet in uw gegevens bestaat, kunt u maken een *synthetische partitiesleutel*. Dit document beschrijft enkele basistechnieken voor het genereren van een synthetische partitiesleutel voor uw Cosmos-container.

## <a name="concatenate-multiple-properties-of-an-item"></a>Samenvoegen van meerdere eigenschappen van een item

U kunt een partitiesleutel vormen door het samenvoegen van meerdere eigenschapswaarden naar één kunstmatige `partitionKey` eigenschap. Deze sleutels worden aangeduid als synthetische sleutels. Neem bijvoorbeeld het volgende voorbeelddocument:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Voor de vorige document, wordt een optie/DeviceID of /date instellen als de partitiesleutel is. Gebruik deze optie als u wilt voor het partitioneren van de container op basis van apparaat-ID of datum. Een andere optie is om deze twee waarden in een synthetische samen te voegen `partitionKey` eigenschap die wordt gebruikt als de partitiesleutel.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

U kunt in realtime scenario's, duizenden items in een database hebben. In plaats van de synthetische sleutel handmatig toe te voegen, client-side-logica voor het samenvoegen van waarden en de synthetische sleutel invoegen in de items in uw Cosmos-containers te definiëren.

## <a name="use-a-partition-key-with-a-random-suffix"></a>Een partitiesleutel gebruiken met een willekeurig achtervoegsel

Er is een andere mogelijke strategie voor de belasting meer gelijkmatig verdelen toe te voegen een willekeurig getal aan het einde van de waarde voor de partitiesleutel. Wanneer u items op deze manier distribueert, kunt u parallelle schrijfbewerkingen uitvoeren over meerdere partities.

Een voorbeeld is als een partitiesleutel een datum voorstelt. U mogelijk een willekeurig getal tussen 1 en 400 kiezen en het samenvoegen als achtervoegsel aan de datum. Deze methode resulteert in partitiesleutelwaarden die zijn, zoals `2018-08-09.1`,`2018-08-09.2`, enzovoort, via `2018-08-09.400`. Omdat u een willekeurige kleur de partitiesleutel geven, worden de schrijfbewerkingen op de container op elke dag gelijkmatig verdeeld over meerdere partities. Deze methode resulteert in betere parallelle uitvoering en de totale hogere doorvoer.

## <a name="use-a-partition-key-with-pre-calculated-suffixes"></a>Een partitiesleutel gebruiken met vooraf berekende achtervoegsels 

De strategie willekeurig achtervoegsel kan schrijven-doorvoer aanzienlijk verbeteren, maar het is moeilijk te lezen van een specifiek item. U weet niet de waarde voor het domeinachtervoegsel dat werd gebruikt toen u het artikel is geschreven. Als u wilt maken het gemakkelijker om afzonderlijke items te lezen, gebruik van de vooraf berekende achtervoegsels-strategie. Gebruik in plaats van een willekeurig getal voor het distribueren van de items over de partities, een getal dat wordt berekend op basis van iets dat u wilt zoeken.

Houd rekening met het vorige voorbeeld, waarbij een container maakt gebruik van een datum als de partitiesleutel. Stel nu dat elk item heeft een `Vehicle-Identification-Number` (`VIN`) kenmerk die we willen openen. Verder toe, Stel dat u vaak query's uitvoeren om items zoeken op de `VIN`, naast de datum. Voordat u uw toepassing schrijft het item naar de container, kunt het berekenen van een hash-achtervoegsel op basis van het Chassisnummer en voegt deze toe aan de partitie sleutel datum. De berekening genereren een getal tussen 1 en 400 die gelijkmatig is verdeeld. Dit resultaat is vergelijkbaar met de resultaten van de methode van de strategie willekeurig achtervoegsel. De waarde voor de partitiesleutel is de datum die wordt samengevoegd met het berekende resultaat.

Met deze strategie worden schrijfbewerkingen gelijkmatig verdeeld tussen de partitiesleutelwaarden die zijn, en de partities. U kunt eenvoudig lezen een bepaald artikel en de datum, omdat u de waarde voor de partitiesleutel voor een specifieke kunt berekenen `Vehicle-Identification-Number`. Het voordeel van deze methode is dat u het maken van één hot partitiesleutel, dat wil zeggen, een partitiesleutel waarmee alle de werkbelasting kunt voorkomen. 

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over het maken van partities concept in de volgende artikelen:

* Meer informatie over [logische partities](partition-data.md).
* Meer informatie over het [doorvoer inrichten op Azure Cosmos-containers en databases](set-throughput.md).
* Meer informatie over het [inrichten doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).
* Meer informatie over het [doorvoer voor een Azure Cosmos-database inrichten](how-to-provision-database-throughput.md).
