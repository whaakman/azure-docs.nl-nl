---
title: JSON-uitvoer voor Stream Analytics | Microsoft Docs
description: Meer informatie over hoe Stream Analytics doel Azure Cosmos DB voor de JSON-uitvoer, voor het archiveren van gegevens en lage latentie query's op niet-gestructureerde JSON-gegevens.
keywords: JSON-uitvoer
documentationcenter: 
services: stream-analytics,documentdb
author: jseb225
manager: jhubbard
editor: cgronlun
ms.assetid: 5d2a61a6-0dbf-4f1b-80af-60a80eb25dd1
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeanb
ms.openlocfilehash: 29be0f5100aabe8374a26e6548effe20ccb9ac86
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="target-azure-cosmos-db-for-json-output-from-stream-analytics"></a>Azure Cosmos Doeldatabase voor JSON-uitvoer van de Stream Analytics
Stream Analytics kunt richten [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) inschakelen voor JSON-uitvoer, lage latentie en archiveren gegevensquery op niet-gestructureerde JSON-gegevens. Dit document bevat informatie over enkele aanbevolen procedures voor het implementeren van deze configuratie.

Voor gebruikers die niet bekend met Cosmos DB bent, Bekijk [leertraject voor Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) aan de slag. 

> [!Note]
> Op dit moment Azure Stream Analytics biedt alleen ondersteuning voor verbinding met behulp van CosmosDB **SQL-API**.
> Andere Azure Cosmos DB-API's zijn nog niet ondersteund. Als u punt Azure Stream Analytics aan de Azure DB die Cosmos-accounts die worden gemaakt met andere API's, zijn de gegevens mogelijk niet juist opgeslagen. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Basisprincipes van Cosmos-database als een output-doel
De Azure DB die Cosmos-uitvoer in Stream Analytics kunt schrijven uw stream resultaten als JSON-uitvoer in uw verzameling(en) Cosmos DB verwerken. Stream Analytics maakt geen verzamelingen in uw database in plaats daarvan moeten ze tevoren maken. Dit is zodat de facturering kosten van de Cosmos-DB verzamelingen transparant voor u zijn, en zodat u kunt afstemmen prestatie, consistentie en capaciteit van uw verzamelingen direct met de [Cosmos DB-API's](https://msdn.microsoft.com/library/azure/dn781481.aspx). U kunt het beste een Cosmos-DB-Database per streaming-taak logische scheiding aanbrengen tussen uw verzamelingen voor streaming-taak gebruikt.

Sommige van de opties voor het verzamelen van Cosmos DB worden hieronder beschreven.

## <a name="tune-consistency-availability-and-latency"></a>Consistentie, beschikbaarheid en latentie afstemmen
Om te voldoen aan uw toepassing, kunt Cosmos DB u fijnmazig afstemmen van de database en verzamelingen en afweging tussen de consistentie, beschikbaarheid en latentie. Afhankelijk van welke mate van lezen van consistentie op basis van de behoeften van uw scenario lezen en schrijven latentie, dat kunt u een consistentiecontrole-niveau voor het databaseaccount van uw. Standaard kunnen Cosmos DB ook synchrone indexeren op elke bewerking CRUD aan uw verzameling. Dit is een andere nuttig optie om te bepalen van de prestaties schrijven leestijd in Cosmos-database. Raadpleeg voor meer informatie over dit onderwerp de [wijzigen van uw database en query consistentieniveaus](../cosmos-db/consistency-levels.md) artikel.

## <a name="upserts-from-stream-analytics"></a>Upserts vanuit Stream Analytics
Stream Analytics-integratie met Cosmos DB kunt u invoegen of bijwerken van de records in uw Cosmos-DB-verzameling op basis van een bepaalde Document-ID-kolom. Dit wordt ook wel een *Upsert*.

Stream Analytics maakt gebruik van een optimistische Upsert benadering, waarbij de updates worden alleen uitgevoerd als invoegen is mislukt vanwege een conflict Document-ID. Deze update wordt uitgevoerd door de Stream Analytics als een PATCH, zodat u kunt gedeeltelijke updates aan het document, dat wil zeggen het toevoegen van nieuwe eigenschappen of het vervangen van die een bestaande eigenschap incrementeel wordt uitgevoerd. Houd er rekening mee dat wijzigingen in de waarden van Matrixeigenschappen in uw JSON-document, resulteren in de gehele matrix ophalen overschreven, dat wil zeggen de matrix niet worden samengevoegd.

## <a name="data-partitioning-in-cosmos-db"></a>Partitioneren van gegevens in Cosmos-DB
Cosmos DB [gepartitioneerde verzamelingen](../cosmos-db/partition-data.md) zijn de aanbevolen aanpak voor het partitioneren van uw gegevens. 

Voor één Cosmos DB verzamelingen kunt Stream Analytics u nog steeds voor het partitioneren van uw gegevens op basis van de querypatronen en de prestatiebehoeften van uw toepassing. Elke verzameling maximaal 10GB aan gegevens (maximaal) kan bevatten en er is momenteel geen manier voor een verzameling omhoog schalen (of overloop). Voor het uitbreiden, Stream Analytics kunt u schrijven tot meerdere verzamelingen met het opgegeven voorvoegsel (Zie onderstaande details van gebruik). Stream Analytics maakt gebruik van de consistente [Hash partitie Resolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) strategie op basis van de gebruiker opgegeven PartitionKey kolom voor het partitioneren van de uitvoer-records. Het aantal verzamelingen met het opgegeven voorvoegsel tijdens het starten van de streaming-taak wordt gebruikt als het aantal uitvoer partities, waarop de taak naar parallel schrijft (Cosmos DB verzamelingen = uitvoer partities). Voor één verzameling met de vertraagde indexering doen alleen ingevoegd, over 0,4 schrijfbewerkingen MB/s kan worden verwacht. Met behulp van meerdere verzamelingen, kunt u een hogere doorvoer en een verbeterde capaciteit.

Als u van plan bent om te verhogen van het aantal partities in de toekomst, moet u mogelijk uw taak stoppen, partitioneren van de gegevens van uw bestaande verzamelingen in nieuwe verzamelingen en vervolgens de Stream Analytics-taak opnieuw te starten. Meer informatie over het gebruik van PartitionResolver en opnieuw partitioneren samen met voorbeeldcode, worden opgenomen in een vervolgzelfstudie post. Het artikel [partitionering en schalen in Cosmos DB](../cosmos-db/sql-api-partition-data.md) biedt ook informatie over dit.

## <a name="cosmos-db-settings-for-json-output"></a>Cosmos DB-instellingen voor JSON-uitvoer
Maken van de Cosmos-DB als uitvoer in Stream Analytics genereert een prompt voor meer informatie, zoals hieronder wordt weergegeven. Deze sectie bevat een uitleg van de eigenschappen van definitie.

Gepartitioneerde verzameling | Verzamelingen met meerdere 'Één partitie'
---|---
![documentdb stream analytics uitvoerscherm](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png) |  ![documentdb stream analytics uitvoerscherm](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)


  
> [!NOTE]
> De **meerdere verzamelingen met 'Één partitie'** scenario vereist een partitiesleutel en configuratie wel wordt ondersteund. 

* **Uitvoer Alias** – een alias voor het verwijzen deze uitvoer in uw query ASA  
* **De accountnaam** : de naam of de URI van de Cosmos-DB-account voor het eindpunt.  
* **Account van de sleutel** – de gedeelde toegangssleutel voor het account van de Cosmos-DB.  
* **Database** – naam van de Cosmos-DB-database.  
* **Verzameling naampatroon** – naam van de verzameling of het patroon voor de verzamelingen moet worden gebruikt. Indeling van de Collectienaam kan worden samengesteld met behulp van de optionele {partition}-token, waarbij partities beginnen bij 0. Hieronder vindt u voorbeeld geldige invoer:  
  1\) MyCollection: een verzameling met de naam 'MyCollection' moet aanwezig zijn.  
  2\) MyCollection {partition} – deze verzamelingen moeten bestaan – 'MyCollection0', 'MyCollection1', 'MyCollection2', enzovoort.  
* **Partitie-sleutel** : optioneel. Dit is alleen nodig als u een token {partition} in het patroon van de naam van verzameling. De naam van het veld in uitvoergebeurtenissen dat wordt gebruikt voor het opgeven van de sleutel voor het partitioneren van uitvoer in collecties. Voor één verzameling uitvoer, elke willekeurige uitvoerkolom kan worden gebruikt, bijvoorbeeld PartitionId.  
* **ID-document** : optioneel. De naam van het veld in uitvoergebeurtenissen dat wordt gebruikt voor de primaire sleutel opgeven in welke invoegen of updatebewerkingen zijn gebaseerd.  
