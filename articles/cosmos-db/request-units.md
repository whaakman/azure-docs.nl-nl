---
title: Aanvraageenheden en doorvoer in Azure Cosmos DB
description: Meer informatie over hoe u om op te geven en een schatting maken van aanvraag eenheid vereisten in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: rimman
ms.openlocfilehash: c43b718002267cd33135c130c6c41cabd8ee9c3b
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52310032"
---
# <a name="request-units-in-azure-cosmos-db"></a>Aanvraageenheden in Azure Cosmos DB

Met Azure Cosmos DB betaalt u voor de doorvoer u inrichten en de opslag die u op uurbasis verbruikt. Doorvoer moet worden ingericht om ervoor te zorgen dat voldoende systeembronnen beschikbaar voor uw Azure Cosmos-database altijd zijn te voldoen of de SERVICEOVEREENKOMST van Azure Cosmos DB.

Azure Cosmos DB biedt ondersteuning voor tal van API's (SQL, MongoDB, Cassandra, Gremlin en tabel). Elke API heeft een eigen set databasebewerkingen, variërend van eenvoudige punt leest en schrijft naar complexe query's. Elke databasebewerking verbruikt, afhankelijk van de complexiteit van de bewerking systeembronnen.  De kosten van alle databasebewerkingen is genormaliseerd door Azure Cosmos DB en wordt uitgedrukt in termen van Aanvraageenheden (ru's). De kosten voor het lezen van een item van 1 KB is 1 Aanvraageenheid (1 RU) en minimale ru's nodig om te gebruiken van 1 GB aan opslagruimte is 40. Alle andere databasebewerkingen worden op dezelfde manier kosten in termen van ru's toegewezen. Kosten worden altijd, ongeacht de API die u gebruikt om te communiceren met uw Azure Cosmos-container en de databasebewerking (schrijven, lezen, query's), gemeten in termen van ru's.

U kunt RU/s beschouwen als de valuta die voor doorvoer. RU/s is een valuta op basis van een tarief, dat de systeembronnen, zoals CPU, IOP's en geheugen die nodig zijn isoleert voor het uitvoeren van de databasebewerkingen die worden ondersteund door Azure Cosmos DB. De volgende afbeelding ziet u de basis van aanvraageenheden gebruikt door verschillende databasebewerkingen:

![Databasebewerkingen aanvraageenheden gebruiken](./media/request-units/request-units.png)

Als u wilt beheren en plannen van capaciteit, Azure Cosmos DB zorgt ervoor dat het aantal ru's voor een bepaalde database gedurende een bepaalde gegevensset deterministisch. U kunt het aantal ru's die worden verbruikt door elke databasebewerking die door te controleren van de reactieheader bijhouden. Wanneer u bekend bent met de factoren die invloed hebben op aanvraag eenheid kosten in rekening gebracht en vereisten van de doorvoer van uw toepassing, kunt u uw toepassing voordelig uitvoeren.

Wanneer u wordt gefactureerd op uurbasis, kunt u het aantal ru's inrichten voor uw toepassing op basis van per seconde in stappen van 100 RU/s. Als u wilt de ingerichte doorvoer voor uw toepassing schalen, kunt u vergroten of verkleinen het aantal ru's (in de stappen of afloopt in stappen van 100 ru's) op elk gewenst moment programmatisch of via de Azure-portal.

U kunt de doorvoer op twee verschillende granulaties inrichten: 

* **Containers**. Zie voor meer informatie, [over het inrichten van doorvoer voor een Azure Cosmos-container.](how-to-provision-container-throughput.md)
* **Databases**. Zie voor meer informatie, [over het inrichten van doorvoer voor een Azure Cosmos-database.](how-to-provision-database-throughput.md)

## <a name="request-unit-considerations"></a>Overwegingen voor aanvraag-eenheid

Tijdens het schatten van het aantal RU/s om in te richten, is het belangrijk dat u rekening houden met de volgende factoren:

* **Item grootte** - zoals de grootte van een item wordt verhoogd, het aantal om te lezen of schrijven van het item ook toeneemt verbruikte ru's.

* **Item indexeren** -standaard elk item automatisch is geïndexeerd. Minder aanvraageenheden worden gebruikt als u ervoor kiest niet te indexeren aantal objecten in een container.

* **De eigenschap count-item** -ervan uitgaande dat de standaardbeleidsregels voor indexering voor alle eigenschappen, het aantal ru's gebruikt voor het schrijven van een item wordt verhoogd wanneer het item eigenschap aantal toeneemt.

* **Geïndexeerde eigenschappen** -beleid voor een index voor elke container bepaalt welke eigenschappen standaard worden geïndexeerd. U kunt het verbruik van de aanvraag-eenheid voor schrijfbewerkingen beperken door het beperken van het aantal geïndexeerde eigenschappen.

* **Gegevensconsistentie** -de sterke en gebonden veroudering consistentieniveaus ongeveer 2 X meer ru's verbruiken tijdens het uitvoeren van leesbewerkingen in vergelijking met die van andere soepele consistentieniveaus.

* **Query uitvoeren op patronen** -de complexiteit van een query is van invloed op het aantal aanvraageenheden voor een bewerking worden verbruikt. Het aantal queryresultaten, het aantal predikaten, de aard van de predikaten, het nummer van de gebruiker gedefinieerde functies, de grootte van de brongegevens bevinden, de grootte van het resultaat ingesteld en projecties van invloed zijn op de kosten van querybewerkingen. Azure Cosmos DB garandeert dat dezelfde query op de dezelfde gegevens altijd hetzelfde aantal ru's op herhalen uitvoeringen kosten.

* **Gebruik een script** - zoals met query's, opgeslagen procedures en triggers verbruiken ru's op basis van de complexiteit van de bewerkingen die worden uitgevoerd. Inspecteer de aanvraagheader van de kosten in rekening gebracht voor een beter begrip van hoeveel aanvraag eenheid capaciteit van elke bewerking verbruikt tijdens het ontwikkelen van uw toepassing.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [inrichting doorvoer voor Azure Cosmos-containers en -databases](set-throughput.md)
* Meer informatie over [logische partities](partition-data.md)
* Meer informatie over [wereldwijd schalen ingerichte doorvoer](scaling-throughput.md)
* Informatie over [over het inrichten van doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md)
* Informatie over [hoe u de doorvoer voor een Azure Cosmos-database inrichten](how-to-provision-database-throughput.md)
