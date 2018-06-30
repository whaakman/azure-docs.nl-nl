---
title: Azure Stream Analytics-uitvoer naar een Cosmos-DB
description: Dit artikel wordt beschreven hoe u met Azure Stream Analytics uitvoer opslaan bij Azure Cosmos DB voor JSON-uitvoer, voor het archiveren van gegevens en lage latentie query's op niet-gestructureerde JSON-gegevens.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: ff2071a703b0b5e94cd68122a878b51e9d97669a
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37112748"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Stream Analytics-uitvoer naar Azure Cosmos-DB  
Stream Analytics kunt richten [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) inschakelen voor JSON-uitvoer, lage latentie en archiveren gegevensquery op niet-gestructureerde JSON-gegevens. Dit document bevat informatie over enkele aanbevolen procedures voor het implementeren van deze configuratie.

Voor gebruikers die niet bekend met Cosmos DB bent, Bekijk [leertraject voor Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) aan de slag. 

> [!Note]
> Op dit moment Azure Stream Analytics biedt alleen ondersteuning voor verbinding met behulp van Azure DB die Cosmos **SQL-API**.
> Andere Azure Cosmos DB-API's zijn nog niet ondersteund. Als u punt Azure Stream Analytics aan de Azure DB die Cosmos-accounts die worden gemaakt met andere API's, zijn de gegevens mogelijk niet juist opgeslagen. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Basisprincipes van Cosmos-database als een output-doel
De Azure DB die Cosmos-uitvoer in Stream Analytics kunt schrijven uw stream resultaten als JSON-uitvoer in uw verzameling(en) Cosmos DB verwerken. Stream Analytics biedt geen verzamelingen maken in uw database in plaats daarvan moeten ze tevoren maken. Dit is zodat de facturering kosten van de Cosmos-DB verzamelingen worden beheerd door u en zodat u kunt afstemmen, de prestaties, consistentie en capaciteit van uw verzamelingen direct met de [Cosmos DB-API's](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

Sommige van de opties voor het verzamelen van Cosmos DB worden hieronder beschreven.

## <a name="tune-consistency-availability-and-latency"></a>Consistentie, beschikbaarheid en latentie afstemmen
Om te voldoen aan uw toepassing, kunt Azure Cosmos DB u fijnmazig afstemmen van de database en verzamelingen en afweging tussen de consistentie, beschikbaarheid en latentie. Afhankelijk van welke mate van lezen van consistentie op basis van de behoeften van uw scenario lezen en schrijven latentie, dat kunt u een consistentiecontrole-niveau voor het databaseaccount van uw. Standaard kunnen Azure Cosmos DB ook synchrone indexeren op elke bewerking CRUD aan uw verzameling. Dit is een andere nuttige optie om te bepalen van de prestaties voor schrijven/lezen in Azure Cosmos DB. Raadpleeg voor meer informatie de [wijzigen van uw database en query consistentieniveaus](../cosmos-db/consistency-levels.md) artikel.

## <a name="upserts-from-stream-analytics"></a>Upserts vanuit Stream Analytics
Stream Analytics-integratie met Azure Cosmos DB kunt u invoegen of bijwerken van de records in de verzameling op basis van een bepaalde Document-ID-kolom. Dit wordt ook wel een *Upsert*.

Stream Analytics maakt gebruik van een benadering optimistische upsert waarin updates worden alleen uitgevoerd als invoegen is mislukt met een conflict Document-ID. Deze update wordt uitgevoerd als een PATCH, zodat gedeeltelijke updates aan het document, dat wil kunnen, het toevoegen van nieuwe eigenschappen of het vervangen van die een bestaande eigenschap incrementeel wordt uitgevoerd. Echter, wijzigingen in de waarden van Matrixeigenschappen in uw JSON-document, resulteren in de gehele matrix ophalen overschreven, dat wil zeggen, de matrix is niet samengevoegd.

## <a name="data-partitioning-in-cosmos-db"></a>Partitioneren van gegevens in Cosmos-DB
Azure Cosmos DB [onbeperkte](../cosmos-db/partition-data.md) zijn de aanbevolen aanpak voor partitionering van uw gegevens als Azure Cosmos DB automatisch wordt geschaald partities op basis van uw werkbelasting. Bij het schrijven naar een onbeperkt aantal containers, Stream Analytics maakt gebruik van zo veel parallelle schrijvers als de vorige stap van de query of partitieschema invoer.

Stream Analytics kunt omhoog of omlaag schalen zodra ze zijn volledig op geen enkele manier voor vaste Azure DB die Cosmos-verzamelingen. Ze hebben een bovenlimiet van 10 GB en de doorvoer van 10.000 RU/s.  Als u wilt de gegevens migreren vanaf een vaste container aan een onbeperkte container (bijvoorbeeld een met ten minste 1000 RU/s en een partitiesleutel), moet u gebruiken de [hulpprogramma voor gegevensmigratie](../cosmos-db/import-data.md) of de [wijziging feed bibliotheek](../cosmos-db/change-feed.md).

Schrijven naar meerdere vaste containers wordt afgeschaft en is niet de aanbevolen aanpak voor het schalen van uw Stream Analytics-taak. Het artikel [partitionering en schalen in Cosmos DB](../cosmos-db/sql-api-partition-data.md) biedt meer details.

## <a name="cosmos-db-settings-for-json-output"></a>Cosmos DB-instellingen voor JSON-uitvoer
Maken van de Cosmos-DB als uitvoer in Stream Analytics genereert een prompt voor meer informatie, zoals hieronder wordt weergegeven. Deze sectie bevat een uitleg van de eigenschappen van definitie.


![documentdb stream analytics uitvoerscherm](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

Veld           | Beschrijving 
-------------   | -------------
Uitvoeraliassen    | Een alias voor deze uitvoer in uw query ASA verwijzen   
Accountnaam    | De naam of het eindpunt-URI van de Azure DB die Cosmos-account 
Accountsleutel     | De gedeelde toegangssleutel voor het account voor Azure Cosmos-DB
Database        | De naam van de Azure DB die Cosmos-database
Verzamelingnaam | De naam van de verzameling voor de verzameling moet worden gebruikt. `MyCollection` is een voorbeeld geldige invoer - een verzameling met de naam `MyCollection` moet bestaan.  
Document-id     | Optioneel. De kolomnaam in uitvoergebeurtenissen dat wordt gebruikt als de unieke sleutel op welke insert of update-bewerkingen moeten worden gebaseerd. Als leeg wordt gelaten, wordt alle gebeurtenissen ingevoegd, zonder optie voor update.
