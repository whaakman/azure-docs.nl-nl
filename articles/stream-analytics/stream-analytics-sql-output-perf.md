---
title: Azure Stream Analytics-uitvoer naar Azure SQL Database
description: Meer informatie over het uitvoeren van gegevens naar SQL Azure en Azure Stream Analytics en bereikt hogere schrijven doorvoersnelheden.
services: stream-analytics
author: chetang
ms.author: chetang
manager: katicad
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/21/2018
ms.openlocfilehash: 794e2f3db44c29707400f96970159578d9e83f2d
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54303272"
---
# <a name="azure-stream-analytics-output-to-azure-sql-database"></a>Azure Stream Analytics-uitvoer naar Azure SQL Database

Dit artikel worden besproken tips voor betere prestaties voor schrijven-doorvoer bereiken wanneer u het laden van gegevens in SQL Azure-Database met behulp van Azure Stream Analytics.

SQL-uitvoer in Azure Stream Analytics biedt ondersteuning voor schrijven parallel als een optie. Deze optie [volledig parallelle](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) topologieën, waarbij meerdere partities voor uitvoer naar de doeltabel parallel schrijft taak. Als u deze optie in Azure Stream Analytics inschakelt echter mogelijk niet voldoende voor een hogere doorvoercapaciteit, omdat deze afhankelijk aanzienlijk uw configuratie voor SQL Azure-database en de tabelschema. De keuze van de indexen, clustering sleutel, index-opvulfactor en compressie wordt een invloed hebben op de tijd voor het laden van tabellen. Zie voor meer informatie over het optimaliseren van uw SQL Azure database voor het verbeteren van query en prestaties op basis van interne benchmarks laden [prestatierichtlijnen voor SQL database](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance). Ordening van schrijfbewerkingen is niet noodzakelijkerwijs bij het schrijven van parallel met SQL Azure-Database.

Hier volgen enkele configuraties binnen elke service die u kan helpen verbeteren de algehele doorvoer van uw oplossing.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

- **Partitioneren overnemen** : deze SQL-uitvoer configuratie optie kunnen overnemen van het partitieschema van de vorige querystap of invoer van. Met deze optie is ingeschakeld, schrijven naar een op schijf gebaseerde tabel en hebben een [volledig parallelle](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) topologie voor uw project, verwacht te zien van betere doorvoer. Deze partitioneren al automatisch gebeurt voor tal van andere [levert](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#partitions-in-sources-and-sinks). Bulksgewijs invoegen is gemaakt met deze optie is ook vergrendeling (TABLOCK) van de tabel uitgeschakeld.

> [!NOTE] 
> Wanneer er meer dan 8 invoer partities, overgenomen van de invoer partitieschema mogelijk niet de juiste keuze. Deze limiet is waargenomen in een tabel met een kolom met één identiteit en een geclusterde index. Op basis van uw schema en de keuze van indexen, kunnen uw opmerkingen variëren.

- **Batchgrootte** -SQL output-configuratie kunt u de maximale batchgrootte opgeven in de uitvoer van een Azure Stream Analytics SQL op basis van de aard van de doel-tabel/werkbelasting. Batchgrootte is het maximum aantal records dat met elke bulksgewijs verzonden invoegen transactie. In de geclusterde columnstore-indexen, batch-grootten rond [100 KB](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-data-loading-guidance) toestaan voor meer parallelle uitvoering, minimale logboekregistratie en optimalisaties vergrendelen. In schijfgebaseerde tabellen, 10K (standaard) of een lagere mogelijk optimaal is voor uw oplossing, zoals hogere batch grootten vergrendelingsescalatie tijdens bulksgewijs invoegen kunnen activeren.

- **Voer bericht afstemmen** : als u bent geoptimaliseerd met behulp van partitionering en batch grootte overnemen, waardoor het aantal invoergebeurtenissen per bericht per partitie zorgt verder pushen van de doorvoer van schrijfbewerkingen. Invoerbericht afstemmen, kunt batch-grootten in Azure Stream Analytics worden tot aan de opgegeven Batch-grootte, waardoor de doorvoer. Dit kan worden bereikt met behulp van [compressie](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-inputs) of bericht grotere beschikbaar zijn in Premium EventHub-SKU.

## <a name="sql-azure"></a>SQL Azure

- **Gepartitioneerde tabellen en indexen** – met een [gepartitioneerde](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes?view=sql-server-2017) SQL-tabel en gepartitioneerde indexen voor de tabel met dezelfde kolom als de partitiesleutel (bijvoorbeeld PartitionId) contentions tussen aanzienlijk kunnen verminderen partities tijdens schrijfbewerkingen. Voor een gepartitioneerde tabel moet u maakt een [partitiefunctie](https://docs.microsoft.com/sql/t-sql/statements/create-partition-function-transact-sql?view=sql-server-2017) en een [partitieschema](https://docs.microsoft.com/sql/t-sql/statements/create-partition-scheme-transact-sql?view=sql-server-2017) op de primaire bestandsgroep. Dit wordt ook de beschikbaarheid van de bestaande gegevens verhogen bij het laden van nieuwe gegevens. Logboek i/o-limiet kan worden bereikt op basis van het aantal partities, die kan worden verhoogd met de upgrade voor de SKU.

- **Unieke sleutel schendingen te voorkomen dat** : als u een [meerdere sleutelconflict waarschuwingsberichten](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-common-troubleshooting-issues#handle-duplicate-records-in-azure-sql-database-output) Zorg ervoor dat de taak wordt niet beïnvloed door de unique-beperking-schendingen die zijn waarschijnlijk, in het activiteitenlogboek voor Azure Stream Analytics tijdens het herstel gevallen. Dit kan worden vermeden door in te stellen de [negeren\_dubbele\_sleutel](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-common-troubleshooting-issues#handle-duplicate-records-in-azure-sql-database-output) optie op uw indexen.

## <a name="azure-data-factory-and-in-memory-tables"></a>Azure Data Factory en In-Memory-tabellen

- **De tabel in het geheugen als tijdelijke tabel** – [geheugentabellen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) toestaan voor laden van gegevens van zeer hoge snelheid, maar de gegevens moeten passen in het geheugen. Benchmarks show bulksgewijs laden uit een tabel in het geheugen aan een tabel op basis van een schijf is ongeveer 10 keer sneller dan rechtstreeks bulksgewijs invoegen in de tabel op basis van een schijf met een id-kolom en een geclusterde index met behulp van één schrijver. Als u wilt gebruikmaken van de prestaties van deze bulksgewijs invoegen, instellen van een [taak voor het kopiëren met behulp van Azure Data Factory](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database) die gegevens uit de tabel in het geheugen worden gekopieerd naar de tabel op basis van een schijf.

## <a name="summary"></a>Samenvatting

Kortom, met de functie gepartitioneerde uitvoer in Azure Stream Analytics voor SQL-uitvoer geeft uitgelijnde parallellisering van uw taak met een gepartitioneerde tabel in SQL Azure doorvoer aanzienlijke verbeteringen. Gebruik te maken van Azure Data Factory voor het indelen van verplaatsing van gegevens uit een In-Memory-tabel in schijfgebaseerde tabellen kunt factor doorvoer winsten geven. Zo mogelijk kan verbeteren dichtheid bericht ook worden belangrijke factor bij het verbeteren van de algemene doorvoer.
