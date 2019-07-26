---
title: Aanvraag eenheden en door Voer in Azure Cosmos DB
description: Meer informatie over het opgeven en ramen van vereisten voor aanvraag eenheden in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: rimman
ms.openlocfilehash: a1143f912d894c1219de05b03a2338dc4e5bdc5f
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467659"
---
# <a name="request-units-in-azure-cosmos-db"></a>Aanvraag eenheden in Azure Cosmos DB

Met Azure Cosmos DB betaalt u de door u ingerichte door Voer en de opslag die u per uur verbruikt. De door voer moet worden ingericht om ervoor te zorgen dat er te allen tijde voldoende systeem bronnen beschikbaar zijn voor uw Azure Cosmos-data base. U hebt voldoende resources nodig om te voldoen aan de [Azure Cosmos DB sla's](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

Azure Cosmos DB ondersteunt veel Api's, zoals SQL, MongoDB, Cassandra, Gremlin en Table. Elke API heeft een eigen set database bewerkingen. Deze bewerkingen variëren van eenvoudig punt Lees-en schrijf bewerkingen naar complexe query's. Bij elke database bewerking worden systeem resources verbruikt op basis van de complexiteit van de bewerking. 

De kosten van alle database bewerkingen worden genormaliseerd door Azure Cosmos DB en worden uitgedrukt in *aanvraag eenheden* (of RUs voor short). U kunt RUs per seconde beschouwen als de valuta voor door voer. RUs per seconde is een op tarieven gebaseerde valuta. De systeem bronnen zoals CPU, IOPS en geheugen die nodig zijn om de database bewerkingen uit te voeren die door Azure Cosmos DB worden ondersteund, worden abstract. 

De kosten voor het lezen van een 1 KB-item zijn 1 aanvraag eenheid (of 1 RU). Alle andere database bewerkingen worden op dezelfde manier toegewezen als kosten met behulp van RUs. Ongeacht welke API u gebruikt om te communiceren met uw Azure Cosmos-container, worden de kosten altijd gemeten door RUs. Of de database bewerking een schrijf-, lees-of query is, worden de kosten altijd gemeten in RUs.

In de volgende afbeelding ziet u het hoge idee van RUs:

![Database bewerkingen gebruiken aanvraag eenheden](./media/request-units/request-units.png)

Azure Cosmos DB zorgt ervoor dat het aantal RUs voor een bepaalde database bewerking in een bepaalde gegevensset deterministisch is om de capaciteit te beheren en te plannen. U kunt de reactie header controleren om het aantal RUs bij te houden dat door een database bewerking wordt gebruikt. Wanneer u de [factoren begrijpt die van invloed zijn op de ru-kosten](request-units.md#request-unit-considerations) en de doorvoer vereisten van uw toepassing, kunt u uw toepassing effectief uitvoeren.

U voorziet het aantal RUs voor uw toepassing per seconde op basis van een interval van 100 RUs per seconde. Als u de ingerichte door Voer voor uw toepassing wilt schalen, kunt u het aantal RUs op elk gewenst moment verg Roten of verkleinen. U kunt schalen in stappen of verlaagt 100 RUs. U kunt uw wijzigingen programmatisch of via de Azure Portal maken. U wordt per uur gefactureerd.

U kunt de door Voer op twee verschillende granulariteit inrichten: 

* **Containers**: Zie voor meer informatie [door Voer in te richten op een Azure Cosmos-container](how-to-provision-container-throughput.md).
* **Data bases**: Zie voor meer informatie [door Voer in te richten op een Azure Cosmos-data base](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>Overwegingen voor aanvraag eenheden

Bij het schatten van het aantal RUs per seconde voor het inrichten, moet u rekening houden met de volgende factoren:

* **Item grootte**: Naarmate de grootte van een item toeneemt, neemt het aantal dat wordt gebruikt voor het lezen of schrijven van het item ook toe.

* **Items indexeren**: Elk item wordt standaard automatisch geïndexeerd. Minder RUs wordt verbruikt als u ervoor kiest om een aantal van uw items in een container niet te indexeren.

* **Aantal item eigenschappen**: Ervan uitgaande dat de standaard indexering op alle eigenschappen is ingesteld, wordt het aantal verbruikte RUs voor het schrijven van een item verhoogd naarmate het aantal eigenschappen van het item toeneemt.

* **Geïndexeerde eigenschappen**: Een index beleid op elke container bepaalt welke eigenschappen standaard worden geïndexeerd. Als u het RU-verbruik voor schrijf bewerkingen wilt verminderen, beperkt u het aantal geïndexeerde eigenschappen.

* **Gegevens consistentie**: De sterke en gebonden consistentie niveaus voor veroudering worden ongeveer twee keer meer gebruikt tijdens het uitvoeren van Lees bewerkingen in vergelijking met die van andere beperkte consistentie niveaus.

* **Query patronen**: De complexiteit van een query is van invloed op het aantal verbruikte RUs voor een bewerking. Factoren die van invloed zijn op de kosten van query bewerkingen zijn onder andere: 
    
    - Het aantal query resultaten
    - Het aantal predikaten
    - De aard van de predikaten
    - Het aantal door de gebruiker gedefinieerde functies
    - De grootte van de bron gegevens
    - De grootte van de resultatenset
    - Projecties

  Azure Cosmos DB garandeert dat dezelfde query op dezelfde gegevens altijd hetzelfde aantal RUs voor herhaalde uitvoeringen kost.

* **Script gebruik**: Net als bij query's gebruiken opgeslagen procedures en triggers RUs op basis van de complexiteit van de bewerkingen die worden uitgevoerd. Controleer bij het ontwikkelen van uw toepassing de [koptekst van de aanvraag lading](optimize-cost-queries.md#evaluate-request-unit-charge-for-a-query) om meer inzicht te krijgen in de hoeveelheid ru-capaciteit die door elke bewerking wordt verbruikt.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [inrichten van de door Voer voor Azure Cosmos-containers en-data bases](set-throughput.md).
* Meer informatie over [logische partities](partition-data.md).
* Meer informatie over het [wereld wijd schalen van ingerichte door Voer](scaling-throughput.md).
* Meer informatie over het [inrichten van de door Voer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).
* Meer informatie over het [inrichten van de door Voer voor een Azure Cosmos-data base](how-to-provision-database-throughput.md).
* Meer informatie over [het vinden van de kosten voor aanvraag eenheden voor een bewerking](find-request-unit-charge.md).
* Meer informatie over het [optimaliseren van ingerichte doorvoer kosten in azure Cosmos DB](optimize-cost-throughput.md).
* Meer informatie over het [optimaliseren van de kosten voor lezen en schrijven in azure Cosmos DB](optimize-cost-reads-writes.md).
* Meer informatie over het [optimaliseren van query kosten in azure Cosmos DB](optimize-cost-queries.md).
* Meer informatie over het [gebruik van metrische gegevens voor](use-metrics.md)het bewaken van de door voer.
