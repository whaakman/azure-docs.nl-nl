---
title: Azure Cosmos DB metrische gegevens ophalen uit Azure Monitor
description: ''
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 2eb61a6b9afa3cabf1733be120dfbdacb7de4534
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276793"
---
# <a name="monitor-and-debug-azure-cosmos-db-metrics-from-azure-monitor"></a>Bewaken en fouten opsporen in Azure Cosmos DB-metrische gegevens van Azure Monitor

U kunt Azure Cosmos DB-metrische gegevens van Azure Monitor API weergeven. Azure Monitor biedt verschillende manieren om te communiceren met metrische gegevens, met inbegrip van de Azure-portal of uitvoeren van query's ze toegang hebben tot deze via de REST-API met PowerShell of CLI. Metrische gegevens van Azure Cosmos DB met lage latentie numerieke waarden, die worden verzameld op één minuut frequentie standaard, u kunt ook deze metrische gegevens samenvoegen. Deze metrische gegevens zijn geschikt voor het ondersteunen van realtime scenario's.  

Dit artikel beschrijft de verschillende Azure Cosmos DB metrische gegevens vindt u van Azure Monitor met behulp van Azure portal. Als u geïnteresseerd in algemene bent gebruiksvoorbeelden en Zie hoe de metrische gegevens van Azure Cosmos DB opnieuw wordt gebruikt om te analyseren en foutopsporing van deze problemen [bewaken en fouten opsporen met metrische gegevens in Azure Cosmos DB](use-metrics.md) artikel. U gebruikt een van uw bestaande Azure-Cosmos-accounts en de verschillende metrische gegevens op de database, container, regio, aanvraag of bewerking niveaus weergeven. Zorg dus ervoor dat u hebt een Azure Cosmos-account met voorbeeldgegevens en CRUD-bewerkingen op gegevens uitvoeren.

## <a name="view-metrics-from-azure-portal"></a>Metrische gegevens weergeven vanuit Azure portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Selecteer **Monitor** uit de linker navigatiebalk, en selecteer **metrische gegevens**.

   ![Deelvenster van de metrische gegevens in Azure Monitor](./media/cosmos-db-azure-monitor-metrics/monitor-metrics-blade.png)

1. Uit de **metrische gegevens** deelvenster > **selecteert u een resource** > Kies de vereiste **abonnement**, en **resourcegroep**. Voor de **resourcetype**, selecteer **Azure Cosmos DB-accounts**, kies een van uw bestaande Azure-Cosmos-accounts en selecteer **toepassen**. 

   ![Kies een Cosmos DB-account om metrische gegevens weer te geven](./media/cosmos-db-azure-monitor-metrics/select-cosmosdb-account.png)

1. U kunt vervolgens een metrische waarde selecteren uit de lijst met beschikbare metrische gegevens. U kunt metrische gegevens die specifiek zijn voor aanvraageenheden, opslag, latentie, beschikbaarheid, Cassandra en anderen. Zie voor meer informatie over alle beschikbare metrische gegevens in deze lijst, de [metrische gegevens per categorie](#metrics-by-category) sectie van dit artikel. In dit voorbeeld gaan we selecteren **Aanvraageenheden** en **Avg** als de waarde voor Tijdaggregatie. 

   Naast deze gegevens verder kunt u de **tijdsbereik** en **tijdgranulatie** van de metrische gegevens. Op de limiet is bereikt, kunt u metrische gegevens weergeven voor de afgelopen 30 dagen.  Nadat u het filter toepast, wordt een grafiek weergegeven op basis van uw filter. Hier ziet u het gemiddelde aantal aanvraageenheden per minuut voor de geselecteerde periode verbruikt.  

   ![Kies een metrische waarde van de Azure-portal](./media/cosmos-db-azure-monitor-metrics/metric-types.png)

## <a name="add-filters-to-metrics"></a>Filters toevoegen aan de metrische gegevens

U kunt ook filteren metrische gegevens en de grafiek weergegeven met een specifieke **CollectionName**, **DatabaseName**, **OperationType**, **regio**, en **StatusCode**. Als u wilt de metrische gegevens filteren, selecteert u **filter toevoegen** en kiest u de vereiste eigenschap zoals **OperationType** en selecteer een waarde zoals **Query**. De grafiek geeft de basis van aanvraageenheden gebruikt voor de querybewerking voor de geselecteerde periode. De bewerkingen uitgevoerd via de opgeslagen procedure worden niet geregistreerd, zodat ze niet beschikbaar onder de OperationType metrische gegevens zijn.

![Een filter om te selecteren welke metrische gegevens toevoegen](./media/cosmos-db-azure-monitor-metrics/add-metrics-filter.png)

U kunt metrische gegevens groeperen met behulp van de **toepassen splitsen** optie. U kunt bijvoorbeeld de basis van aanvraageenheden per bewerkingstype groeperen en weergeven van de grafiek voor alle bewerkingen in één keer zoals wordt weergegeven in de volgende afbeelding: 

![Voeg splitsen filter toepassen](./media/cosmos-db-azure-monitor-metrics/apply-metrics-splitting.png)


## <a id="metrics-by-category"></a>Metrische gegevens per categorie

### <a name="request-metrics"></a>Aanvraag voor metrische gegevens
            
|Metrische gegevens (metrische weergavenaam)|Eenheid (samenvoegingstype) |Description|Dimensies| Tijd granulaties| Verouderde metrische toewijzing | Gebruik |
|---|---|---|---| ---| ---| ---|
| TotalRequests (totaal aantal aanvragen) | Count (aantal) | Aantal aanvragen| DatabaseName, CollectionName, Region, StatusCode| Alle | TotalRequests, Http 2xx, Http 3xx, Http-fout 400, 401 Http-, interne serverfout Service niet beschikbaar, aanvragen beperkt, Gemiddeld aantal aanvragen per seconde | Gebruikt voor het bewaken van aanvragen per statuscode, verzameling met een granulariteit van één minuut. Als u verzoeken per seconde, minuut aantal aggregatie gebruiken en delen door 60. |
| MetadataRequests (aanvragen voor metagegevens) |Count (aantal) | Het aantal aanvragen voor metagegevens. Azure Cosmos DB houdt verzameling systeemmetagegevens voor elk account, waarmee u het inventariseren van verzamelingen, databases, enzovoort, en de bijbehorende configuraties, gratis. | DatabaseName, CollectionName, Region, StatusCode| Alle| |Gebruikt voor het bewaken van vertragingen vanwege aanvragen voor metagegevens.|
| MongoRequests (Mongo-aanvragen) | Count (aantal) | Aantal aanvragen van Mongo | Databasenaam, CollectionName, regio, CommandName, foutcode| Alle |Snelheid van aanvragen voor mongo-Query, Mongo-Update aanvragen snelheid, Mongo verwijderen snelheid van aanvragen, Mongo invoegen snelheid van aanvragen, snelheid van aanvragen voor Mongo-aantal| Gebruikt voor het bewaken van Mongo aanvraag fouten, typt u het gebruik van per opdracht. |

### <a name="request-unit-metrics"></a>Aanvraag eenheid metrische gegevens

|Metrische gegevens (metrische weergavenaam)|Eenheid (samenvoegingstype)|Description|Dimensies| Tijd granulaties| Verouderde metrische toewijzing | Gebruik |
|---|---|---|---| ---| ---| ---|
| MongoRequestCharge (kosten Mongo aanvragen) | Aantal (totaal) |Mongo-Aanvraageenheden verbruikt| Databasenaam, CollectionName, regio, CommandName, foutcode| Alle |Mongo-Query aanvraag kosten in rekening gebracht, Mongo Update kosten in rekening gebracht, Mongo verwijderen aanvraag kosten in rekening gebracht, Mongo invoegen aanvraag kosten in rekening gebracht, Mongo aantal aanvraag kosten aanvragen| Gebruikt voor het bewaken van Mongo-resource ru's in een minuut.|
| TotalRequestUnits (totaal aantal aanvragen eenheden)| Aantal (totaal) | Aanvraag dat eenheden verbruikt| DatabaseName, CollectionName, Region, StatusCode |Alle| TotalRequestUnits| Gebruikt om het totaal aantal RU-gebruik op een minute granulariteit bewaken. Als u gemiddelde ru's per seconde worden verbruikt, totale aggregatie op minuut gebruiken en delen door 60.|
| ProvisionedThroughput (ingerichte doorvoer)| Count (Maximum) |Ingerichte doorvoer via de granulatie van de verzameling| DatabaseName, CollectionName| 5 MIN.| | Gebruikt voor het bewaken van de ingerichte doorvoer per verzameling.|

### <a name="storage-metrics"></a>Metrische gegevens over opslag

|Metrische gegevens (metrische weergavenaam)|Eenheid (samenvoegingstype)|Description|Dimensies| Tijd granulaties| Verouderde metrische toewijzing | Gebruik |
|---|---|---|---| ---| ---| ---|
| AvailableStorage (beschikbare opslag) |Bytes (totaal) | Totale beschikbare opslag gerapporteerd op 5 minuten granulatie per regio| DatabaseName, CollectionName, Region| 5 MIN.| Beschikbare opslag| Gebruikt voor het bewaken van beschikbare opslag (alleen van toepassing op vaste opslag verzamelingen) capaciteit minimale granulariteit moet 5 minuten.| 
| DataUsage (gegevensgebruik) |Bytes (totaal) |Gebruik van de totale hoeveelheid gegevens die zijn gerapporteerd op 5 minuten granulatie per regio| DatabaseName, CollectionName, Region| 5 MIN. |Gegevensgrootte | Wordt gebruikt om het gebruik van de totale hoeveelheid gegevens op de verzameling en de regio bewaken, moet minimale granulariteit 5 minuten.|
| IndexUsage (Index Usage) | Bytes (totaal) |Totaal gebruik van de Index wordt gerapporteerd op 5 minuten granulatie per regio| DatabaseName, CollectionName, Region| 5 MIN.| Indexgrootte| Wordt gebruikt om het gebruik van de totale hoeveelheid gegevens op de verzameling en de regio bewaken, moet minimale granulariteit 5 minuten. |
| DocumentQuota (quotum Document) | Bytes (totaal) | Totale opslagquotum gerapporteerd op 5 minuten granulatie per regio.| DatabaseName, CollectionName, Region| 5 MIN. |Opslagcapaciteit| Wordt gebruikt voor het bewaken van de totale quotum verzameling en regio, moet minimale granulariteit 5 minuten.|
| DocumentCount (aantal documenten) | Aantal (totaal) |Totale documentaantal die zijn gerapporteerd op 5 minuten granulatie per regio| DatabaseName, CollectionName, Region| 5 MIN. |Aantal documenten|Wordt gebruikt voor het bewaken van het documentaantal op de verzameling en regio, moet minimale granulariteit 5 minuten.|

### <a name="latency-metrics"></a>Metrieken voor latentie

|Metrische gegevens (metrische weergavenaam)|Eenheid (samenvoegingstype)|Description|Dimensies| Tijd granulaties| Gebruik |
|---|---|---|---| ---| ---|
| ReplicationLatency (replicatievertraging)| Milliseconden (Minimum, Maximum, gemiddelde) | Replicatievertraging P99 tussen de bron- en regio's voor geo-ingeschakelde account| SourceRegion, TargetRegion| Alle | Gebruikt voor het bewaken van de replicatielatentie P99 tussen elke twee regio's voor een geo-replicatie-account. |


### <a name="availability-metrics"></a>Metrische gegevens over beschikbaarheid

|Metrische gegevens (metrische weergavenaam) |Eenheid (samenvoegingstype)|Description| Tijd granulaties| Verouderde metrische toewijzing | Gebruik |
|---|---|---|---| ---| ---|
| ServiceAvailability (servicebeschikbaarheid)| Percentage (Minimum, Maximum) | Beschikbaarheid van aanvragen bij de granulariteit van één uur| 1 UUR | Beschikbaarheid van de service | Hiermee geeft u het percentage van totaal aantal aanvragen doorgegeven. Een aanvraag wordt beschouwd als worden is mislukt vanwege een systeemfout is als de statuscode 410 is, 500 of 503 wordt gebruikt om de beschikbaarheid van het account op uur granulariteit te controleren. |


### <a name="cassandra-api-metrics"></a>Metrische gegevens over Cassandra-API

|Metrische gegevens (metrische weergavenaam)|Eenheid (samenvoegingstype)|Description|Dimensies| Tijd granulaties| Gebruik |
|---|---|---|---| ---| ---|
| CassandraRequests (Cassandra-aanvragen) | Count (aantal) | Aantal aanvragen voor Cassandra-API| Databasenaam, CollectionName, ErrorCode, regio, OperationType, ResourceType| Alle| Gebruikt voor het bewaken van de Cassandra-aanvragen met een granulariteit van één minuut. Als u verzoeken per seconde, minuut aantal aggregatie gebruiken en delen door 60.|
| CassandraRequestCharges (Cassandra aanvraag kosten) | Count (Sum, Min, Max, Avg) | Aanvraageenheden gebruikt door de Cassandra-API-aanvragen| DatabaseName, CollectionName, Region, OperationType, ResourceType| Alle| Gebruikt voor het bewaken van ru's per minuut wordt gebruikt door een Cassandra-API-account.|
| CassandraConnectionClosures (Cassandra-verbinding is gesloten) |Count (aantal) |Aantal Cassandra verbindingen gesloten| ClosureReason, Region| Alle | Gebruikt voor het bewaken van de verbinding tussen clients en de Cassandra-API van Azure Cosmos DB.|

## <a name="next-steps"></a>Volgende stappen

* [Bekijken en controleren metrische gegevens van Azure Cosmos DB-account metrische gegevens deelvenster](use-metrics.md)

* [Registratie in diagnoselogboek in Azure Cosmos DB](logging.md)
