---
title: Optimaliseer opslagkosten in Azure Cosmos DB
description: In dit artikel wordt uitgelegd hoe u voor het beheren van kosten voor opslag voor de gegevens die zijn opgeslagen in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.openlocfilehash: 6a6f8b47044d36d767721e2d3503d11518dbf5a6
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54036117"
---
# <a name="optimize-storage-cost-in-azure-cosmos-db"></a>Optimaliseer opslagkosten in Azure Cosmos DB

Azure Cosmos DB biedt onbeperkte opslag en doorvoer. In tegenstelling tot doorvoer, wat u inrichten moet/configureren op uw Azure Cosmos-containers of -databases, wordt de opslag in rekening gebracht op basis van gebruik. Alleen voor de logische opslag die u verbruikt in rekening gebracht en u hoeft van tevoren geen opslag reserveren. Opslag automatisch schalen omhoog en omlaag op basis van de gegevens die u toevoegt of verwijdert aan een Azure Cosmos DB-container.

## <a name="storage-cost"></a>Opslagkosten

Opslag wordt in rekening gebracht met de eenheid van GB's. Lokale SSD-opslag is die wordt gebruikt door uw gegevens en indexering. De totale opslag die wordt gebruikt, is gelijk aan de opslag die is vereist door de gegevens en de indexen die worden gebruikt in alle regio's als u van Azure Cosmos DB gebruikmaakt. Als u een Azure Cosmos-account wereldwijd in drie regio's repliceren, betaalt u voor de totale opslagkosten in elk van deze drie regio's. Zie voor een schatting van uw opslagvereisten [Capaciteitsplanner](https://www.documentdb.com/capacityplanner) hulpprogramma. De kosten voor opslag in Azure Cosmos DB is $0,25 GB/maand, Zie [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor de meest recente updates. U kunt waarschuwingen instellen om te bepalen van opslag die wordt gebruikt door uw Azure Cosmos-container voor het bewaken van uw opslag, Zie [Monitor Azure Cosmos DB](monitor-accounts.md)) artikel.

## <a name="optimize-cost-with-item-size"></a>Kosten met de grootte van artikel optimaliseren

Azure Cosmos DB wordt verwacht dat de grootte van het artikel moet 2 MB of minder voor optimale prestaties en kostenbesparingen. Als u een item voor het opslaan van groter zijn dan 2 MB aan gegevens moet, houd rekening met het schema van het item opnieuw. In het zeldzame geval dat u het schema niet wijzigen, kunt u het item in de subitems splitsen en ze logisch te koppelen met een algemene identifier(ID). Alle functies van de Azure Cosmos DB consistent worden gewerkt door verankeren aan dat logische id.

## <a name="optimize-cost-with-indexing"></a>Kosten met indexeren optimaliseren

Standaard de gegevens automatisch geïndexeerd, die de totale hoeveelheid opslagruimte kunt verhogen. U kunt echter aangepaste index beleid om deze overhead verminderen toepassen. Automatisch indexeren die niet afgestemd is op door middel van beleid is ongeveer 10-20% van de itemgrootte aan. Door te verwijderen of aanpassen, index-beleid, betaalt u geen extra kosten voor schrijfbewerkingen en capaciteit voor extra doorvoer vereisen. Zie [indexeren in Azure Cosmos DB](indexing-policies.md) om aangepaste indexering beleid te configureren. Als u hebt gewerkt met relationele databases, mag u denkt dat 'Alles indexeren' betekent verdubbeling van opslag of hoger. In Azure Cosmos DB is in het geval is Mediaan, het echter veel lager. In Azure Cosmos DB is de opslagoverhead van de index doorgaans laag (10-20%) zelfs met automatische indexering, omdat het is ontworpen voor een lage opslagverbruik. U kunt de verhouding van de prestaties van de voetafdruk en de query van de index op een meer verfijnde manier beheren door het beheer van het indexeringsbeleid.

## <a name="optimize-cost-with-time-to-live-and-change-feed"></a>Kosten met de tijd op live en wijzigingenfeed optimaliseren

Nadat u de gegevens niet meer nodig hebt u kunt zonder problemen verwijderen uit uw Azure Cosmos-account met behulp van [time to live van](time-to-live.md), [wijzigingenfeed](change-feed.md) of u kunt de oude gegevens migreren naar een ander gegevensarchief, zoals Azure blob-opslag of Azure datawarehouse. Met time to live van of een TTL-waarde, biedt Azure Cosmos DB de mogelijkheid items om automatisch te verwijderen uit een container na een bepaalde periode. Standaard kunt u tijd te bevinden zich op het niveau van de container en de waarde op basis van de per-item overschrijven instellen. Nadat de TTL-waarde is ingesteld op een container of op het itemniveau van een, worden Azure Cosmos DB automatisch deze items verwijderd nadat de tijd is verstreken sinds de tijd dat ze het laatst zijn gewijzigd. Met behulp van feed wijzigen, kunt u gegevens migreren naar een van beide een andere container in Azure Cosmos DB of met een extern gegevensarchief. De migratie neemt nul uitvaltijd en wanneer u bent klaar migreert, kunt u verwijderen of time to live verwijderen van de bron-Azure Cosmos-container van configureren.

## <a name="optimize-cost-with-rich-media-data-types"></a>Kosten met uitgebreide media-gegevenstypen optimaliseren 

Als u wilt opslaan, uitgebreide mediatypen, bijvoorbeeld video's, afbeeldingen, enzovoort, hebt u een aantal opties in Azure Cosmos DB. Een optie is voor het opslaan van deze typen uitgebreide media als Azure Cosmos-items. Er is een limiet van 2 MB per item en kunt u deze limiet voorkomen door het koppelen van het gegevensitem in meerdere subitems. Of u kunt in Azure Blob-opslag opslaan en gebruiken van de metagegevens van de verwijzing naar uw Azure Cosmos-items. Er zijn een aantal voordelen en nadelen met deze methode. De eerste benadering kunt u de beste prestaties in termen van latentie, doorvoer Sla's plus kant en klare wereldwijde distributie mogelijkheden voor de uitgebreide media-gegevenstypen naast uw reguliere Azure Cosmos-items. De ondersteuning is echter beschikbaar op een hogere prijs. Door het opslaan van media in Azure Blob-opslag, kunt u uw totale kosten verlagen. Als de latentie van essentieel belang is, kunt u premium-opslag gebruiken voor uitgebreide media-bestanden die van Azure Cosmos-items wordt verwezen. Deze systeemeigen is geïntegreerd met CDN voor het leveren van afbeeldingen uit de edge-server op de lagere kosten voor geo-beperking te omzeilen. De omlaag aan clientzijde met dit scenario is dat u hebt te maken met twee services: Azure Cosmos DB en Azure Blob storage, kan de operationele kosten verhoogt. 

## <a name="check-storage-consumed"></a>Verbruikte opslag controleren

Om te controleren of het opslagverbruik van een Azure Cosmos-container, kunt u het uitvoeren van een hoofd- of GET-aanvraag voor de container en inspecteer de `x-ms-request-quota` en de `x-ms-request-usage` headers. U kunt ook als u werkt met de .net SDK, kunt u de [DocumentSizeQuota](https://docs.microsoft.com/previous-versions/azure/dn850325(v%3Dazure.100)), en [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) eigenschappen om op te halen van de opslag die wordt gebruikt.

## <a name="using-sdk"></a>Met behulp van SDK

```csharp
// Measure the item size usage (which includes the index size)
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));   

Console.WriteLine("Item size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
```

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u doorgaan naar meer informatie over kostenoptimalisatie in Azure Cosmos DB met de volgende artikelen:

* Meer informatie over [optimaliseren voor de ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over [informatie over uw factuur voor Azure Cosmos DB](understand-your-bill.md)
* Meer informatie over [doorvoer kosten optimaliseren](optimize-cost-throughput.md)
* Meer informatie over [optimaliseren van de kosten van lees- en schrijfbewerkingen](optimize-cost-reads-writes.md)
* Meer informatie over [de kosten van query's optimaliseren](optimize-cost-queries.md)
* Meer informatie over [optimaliseren van de kosten van Azure Cosmos-accounts voor meerdere regio's](optimize-cost-regions.md)

