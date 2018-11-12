---
title: Synthetische partitiesleutels in Azure Cosmos DB
description: Informatie over het gebruik van synthetische partitiesleutels in uw Azure Cosmos DB-containers
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: f6323549c0ecf8f0196d327779f7f7cb67c6e03f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263510"
---
# <a name="create-a-synthetic-partition-key"></a>Maakt een synthetische partitiesleutel

Het is een aanbevolen procedure om een partitiesleutel met veel verschillende waarden, zoals honderden of duizenden. Het doel is om uw gegevens en de belasting gelijkmatig verdelen over de items die zijn gekoppeld aan deze partitie-sleutelwaarden. Als een dergelijke eigenschap niet in uw gegevens bestaat, kan een synthetische partitiesleutel worden samengesteld. De volgende secties beschrijven enkele basistechnieken voor het genereren van een synthetische partitiesleutel voor de container.

## <a name="concatenating-multiple-properties-of-an-item"></a>Samenvoegen van meerdere eigenschappen van een item

U kunt een partitiesleutel vormen door het samenvoegen van meerdere eigenschapswaarden naar één kunstmatige `partitionKey` eigenschap. Deze sleutels worden aangeduid als synthetische sleutels. Neem bijvoorbeeld het volgende voorbeelddocument:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Een optie is voor de vorige document, of in te stellen/DeviceID /date als de partitiesleutel, als u wilt dat voor het partitioneren van de container op basis van apparaat-ID of datum. Een andere optie is om deze twee waarden in een synthetische samen te voegen `partitionKey` eigenschap die wordt gebruikt als de partitiesleutel.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

In realtime scenario's, hebt u duizenden documenten in een database, zodat in plaats van de synthetische sleutel handmatig toe te voegen, moet u client-side-logica voor de waarden samenvoegen en de synthetische sleutel invoegen in de documenten definiëren.

## <a name="using-a-partition-key-with-random-suffix"></a>Met behulp van een partitiesleutel met willekeurig achtervoegsel

Er is een andere mogelijke strategie voor de belasting meer gelijkmatig verdelen toe te voegen een willekeurig getal aan het einde van de waarde voor de partitiesleutel. Naar items in deze manier kunt u parallel uitvoeren van schrijfbewerkingen over meerdere partities.

Bijvoorbeeld, als een partitiesleutel een datum vertegenwoordigt, u mogelijk kiest u een willekeurig getal tussen 1 en 400 en deze samen als achtervoegsel aan de datum. Deze methode resulteert in partitiesleutelwaarden die zijn, zoals 2018-08-09.1 2018-08-09.2, enzovoort, via 2018-08-09.400. Omdat u het uitvoeren van de partitiesleutel zijn willekeurige, worden de schrijfbewerkingen op de container op elke dag gelijkmatig verdeeld over meerdere partities. Deze methode resulteert in betere parallelle uitvoering en de totale hogere doorvoer.

## <a name="using-a-partition-key-with-pre-calculated-suffixes"></a>Met behulp van een partitiesleutel met vooraf berekende achtervoegsels 

Hoewel het uitvoeren van willekeurige strategie kan aanzienlijk verbeteren door de doorvoer van schrijfbewerkingen, is het moeilijk om een specifiek item niet lezen omdat u niet weet het Achtervoegselwaarde die wordt gebruikt wat bij het schrijven van het item. Als u wilt maken het gemakkelijker om afzonderlijke items te lezen, kunt u de strategie vooraf berekende achtervoegsels. Gebruik in plaats van een willekeurig getal voor het distribueren van de items tussen partities, een getal berekenen op basis van iets dat u wilt zoeken.

Houd rekening met het vorige voorbeeld, waarbij een container maakt gebruik van een datum in de partitiesleutel. Nu Stel dat elk item een toegankelijk VIN (Vehicle-id-nummer)-kenmerk heeft en als u vaak query's om items te vinden door VIN, ook op datum uitvoert. Voordat u uw toepassing schrijft het item naar de container, kunt het berekenen van een hash-achtervoegsel op basis van het Chassisnummer en voegt deze toe aan de partitie sleutel datum. De berekening genereren een getal tussen 1 en 400 die gelijkmatig gedistribueerd, vergelijkbaar met de resultaten van de methode willekeurige strategie is. De waarde voor de partitiesleutel is de datum die wordt samengevoegd met het berekende resultaat vervolgens.

Met deze strategie worden schrijfbewerkingen gelijkmatig verdeeld tussen de partitiesleutelwaarden die zijn, en de partities. U kunt eenvoudig een bepaald artikel en de datum, lezen, omdat u de waarde voor de partitiesleutel kunt berekenen voor een specifieke Vehicle-id-nummer in. Het voordeel van deze methode is dat kunt u voorkomen dat het maken van één hot partitiesleutel (de partitiesleutel die alle van de werkbelasting). 

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over het maken van partities concept in de volgende artikelen:

* Meer informatie over [logische partities](partition-data.md)
* Meer informatie over [inrichting doorvoer voor Cosmos-containers en -databases](set-throughput.md)
* Informatie over [over het inrichten van doorvoer voor een Cosmos-container](how-to-provision-container-throughput.md)
* Informatie over [hoe u de doorvoer voor een Cosmos-database inrichten](how-to-provision-database-throughput.md)
