---
title: Limieten in Azure Cosmos DB
description: Dit artikel wordt beschreven in de Azure Cosmos DB limieten.
author: arramac
ms.author: arramac
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2019
ms.openlocfilehash: 0086327661df637dc0ae60208ed9424b4610ef0e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65969490"
---
# <a name="limits-in-azure-cosmos-db"></a>Limieten in Azure Cosmos DB

Dit artikel bevat een overzicht van de limieten in de Azure Cosmos DB-service.

## <a name="storage-and-throughput"></a>Opslag en doorvoer

Nadat u een Azure Cosmos-account in uw abonnement maken, kunt u gegevens beheren in uw account door [het maken van databases, containers en objecten](databases-containers-items.md). U kunt de doorvoer op het niveau van een container of een op databaseniveau in termen van inrichten [aanvraageenheden (RU/s of ru's)](request-units.md). De volgende tabel bevat de limieten voor opslag en doorvoer per container/database.

| Resource | Standaardlimiet |
| --- | --- |
| Maximum aantal ru's per container ([speciaal toegewezen doorvoer ingerichte modus](databases-containers-items.md#azure-cosmos-containers)) | 1\.000.000 standaard. U kunt verhogen door [via een Azure-ondersteuningsticket](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) of contact opnemen met ons op via [vragen Cosmos DB](mailto:askcosmosdb@microsoft.com) |
| Maximum aantal ru's per database ([gedeelde ingerichte doorvoermodus](databases-containers-items.md#azure-cosmos-containers)) | 1\.000.000 standaard. U kunt verhogen door [via een Azure-ondersteuningsticket](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) of contact opnemen met ons op via [vragen Cosmos DB](mailto:askcosmosdb@microsoft.com) |
| Maximum aantal ru's per partitiesleutel (logische) | 10\.000 |
| Maximale opslag voor alle items per partitiesleutel (logische)| 10 GB |
| Maximum aantal afzonderlijke (logische) partitiesleutels | Onbeperkt |
| Maximale opslag per container | Onbeperkt |
| Maximale opslag per database | Onbeperkt |

> [!NOTE]
> Zie voor aanbevolen procedures voor het beheren van werkbelastingen met partitiesleutels die hogere limieten nodig voor opslag of doorvoer hebt [ontwerpen voor ' hot ' partitiesleutels](synthetic-partition-keys.md)
>

Een Cosmos-container (of doorvoer van de gedeelde database) moet een minimale doorvoercapaciteit van 400 ru's hebben. Als de container groeit, afhankelijk de minimaal ondersteunde doorvoer ook van de volgende factoren:

* De maximale opslag verbruikt in de container wordt gemeten in intervallen van 40 ru's per GB aan opslagruimte verbruikt. Bijvoorbeeld, als een container 100 GB aan gegevens bevat, moet klikt u vervolgens de doorvoer ten minste 4000 ru 's
* De maximale doorvoer die ooit is ingericht op de container. De service ondersteunt Breek de doorvoer van een container voor 10% van de ingerichte maximale. Bijvoorbeeld, als uw doorvoer is verhoogd tot 10000 ru's, is klikt u vervolgens de laagste mogelijke ingerichte doorvoer 1000 ru 's
* Het totale aantal containers dat u ooit hebt gemaakt in een database gedeelde doorvoer, gemeten op 100 ru's per container. Bijvoorbeeld, als u vijf containers in een gedeelde doorvoer-database hebt gemaakt, moet klikt u vervolgens de doorvoer ten minste 500 ru 's

De huidige en minimale doorvoer van een container of een database kan worden opgehaald uit Azure portal of de SDK's. Zie voor meer informatie, [inrichten doorvoer voor containers en databases](set-throughput.md). Kortom volgen hier de minimale ingerichte RU-limieten. 

| Resource | Standaardlimiet |
| --- | --- |
| Minimale ru's per container ([speciaal toegewezen doorvoer ingerichte modus](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| Minimale ru's per database ([gedeelde ingerichte doorvoermodus](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| Minimale ru's per container in een gedeelde doorvoer-database | 100 |
| Minimale ru's per GB aan verbruikte opslag | 40 |

Cosmos DB biedt ondersteuning voor elastisch schalen van doorvoer (ru's) per container of de database via de SDK's of de portal. Elke container kunt synchroon en direct schalen binnen een schaalbereik van 10 tot 100 keer, tussen de minimale en maximale waarden. Als de aangevraagde doorvoer waarde buiten het bereik ligt, wordt schalen asynchroon uitgevoerd. Asynchrone schalen kan minuten duren tot uren afhankelijk van de aangevraagde doorvoer en opslaggrootte van de gegevens in de container.  

## <a name="control-plane-operations"></a>Bewerkingen voor de controlelaag

U kunt [inrichten en beheren van uw Azure Cosmos-account](how-to-manage-database-account.md) met behulp van de Azure portal, Azure PowerShell, Azure CLI en Azure Resource Manager-sjablonen. De volgende tabel bevat de limieten per abonnement, -account en het aantal bewerkingen.

| Resource | Standaardlimiet |
| --- | --- |
| Maximale database-accounts per abonnement | 50 standaard. U kunt verhogen door [via een Azure-ondersteuningsticket](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) of contact opnemen met ons op via [vragen Cosmos DB](mailto:askcosmosdb@microsoft.com)|
| Maximum aantal regionale failovers | 1/uur standaard. U kunt verhogen door [via een Azure-ondersteuningsticket](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) of contact opnemen met ons op via [vragen Cosmos DB](mailto:askcosmosdb@microsoft.com)|

> [!NOTE]
> Regionale failovers zijn alleen van toepassing op één regio schrijfbewerkingen accounts. Accounts voor meerdere regio's schrijven geen vereisen en beperkingen met betrekking tot het wijzigen van de schrijfregio hebben.

Cosmos DB maakt back-ups van uw gegevens automatisch met regelmatige intervallen. Zie voor meer informatie over back-upretentie intervallen en windows, [Online back-up en gegevens op aanvraag voor herstellen in Azure Cosmos DB](online-backup-and-restore.md).

## <a name="per-container-limits"></a>De limieten per container

Afhankelijk van welke API die u gebruikt, een Azure Cosmos-container omvatten een verzameling, een tabel of grafiek. Containers ondersteunen configuraties voor [unique key-beperkingen](unique-keys.md), [opgeslagen procedures, triggers en UDF's](stored-procedures-triggers-udfs.md), en [indexeringsbeleid](how-to-manage-indexing-policy.md). De volgende tabel bevat de limieten die specifiek zijn voor configuraties binnen een container. 

| Resource | Standaardlimiet |
| --- | --- |
| Maximale lengte van de database-of containernaam | 255 |
| Maximale opgeslagen procedures per container | 100 <sup>*</sup>|
| Maximale UDF's per container | 25 <sup>*</sup>|
| Maximum aantal paden in het indexeringsbeleid| 100 <sup>*</sup>|
| Maximum aantal unieke sleutels per container|10 <sup>*</sup>|
| Maximum aantal paden per unique key-beperking|16 <sup>*</sup>|

<sup>*</sup> U kunt een van deze limieten per container verhogen door contact op met ondersteuning van Azure of contact opnemen met ons op via [vragen Cosmos DB](mailto:askcosmosdb@microsoft.com).

## <a name="per-item-limits"></a>Limieten per item

Afhankelijk van welke API die u gebruikt, kan een Azure Cosmos-item op een document in een verzameling, een rij in een tabel of een knooppunt of randtabel in een grafiek vertegenwoordigen. De volgende tabel bevat de limieten per item in Cosmos DB. 

| Resource | Standaardlimiet |
| --- | --- |
| Maximale grootte van een item | 2 MB (lengte van de JSON-weergave UTF-8) |
| Maximale lengte van de waarde voor de partitiesleutel | 2048 bytes |
| Maximale lengte van de id-waarde | 1024 bytes |
| Maximum aantal eigenschappen per item | Er is geen limiet |
| Maximale nestdiepte voor acties | Er is geen limiet |
| Maximale lengte van de naam van eigenschap | Er is geen limiet |
| Maximale lengte van de waarde van eigenschap | Er is geen limiet |
| Maximale lengte van de tekenreekswaarde voor eigenschap | Er is geen limiet |
| Maximale lengte van de waarde van de numerieke eigenschap | IEEE754 dubbele precisie 64-bits |

Er zijn geen beperkingen voor de item-nettoladingen, zoals het aantal eigenschappen en het nesten diepte, met uitzondering van de beperkingen voor de lengte op partitiesleutel en -id-waarden en de totale grootte van de beperking van 2 MB. U moet configureren indexeringsbeleid voor containers met groot of complex item structuren RU worden verkleind. Zie [artikelen in Cosmos DB modelleren](how-to-model-partition-example.md) voor een voorbeeld van een echte en patronen voor het beheren van grote objecten.

## <a name="per-request-limits"></a>Limieten per aanvraag

Cosmos DB biedt ondersteuning voor [CRUD-en query](https://docs.microsoft.com/rest/api/cosmos-db/) op basis van bronnen zoals containers, artikelen en databases.  

| Resource | Standaardlimiet |
| --- | --- |
| Maximale uitvoeringstijd voor een enkele bewerking (zoals een opgeslagen procedure kan worden uitgevoerd of het ophalen van een enkele query pagina)| 5 per seconde |
| Maximum request size (stored procedure, CRUD)| 2 MB |
| Maximale reactiegrootte (bijvoorbeeld gepagineerde query) | 4 MB |

Nadat een bewerking, zoals query van de uitvoering time-out of antwoord de limiet van de grootte bereikt, wordt een pagina met resultaten en een vervolgtoken voor het hervatten van de uitvoering naar de client. Er is geen limiet op de duur van die één query voor pagina's / doorloopt uitvoeren kunt.

HMAC cosmos DB gebruikt voor autorisatie. U kunt ofwel een hoofdsleutel gebruiken of een [brontokens](secure-access-to-data.md) voor nauwkeuriger toegangsbeheer tot resources, zoals containers, het partitioneren van sleutels of items. De volgende tabel bevat de limieten voor autorisatietokens in Cosmos DB.

| Resource | Standaardlimiet |
| --- | --- |
| De maximale master token vervaltijd | 15 min  |
| Tijd van de vervaldatum van token minimale resource | 10 min  |
| Maximaal benodigde token verlooptijd | 24 uur standaard. U kunt verhogen door [via een Azure-ondersteuningsticket](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) of contact opnemen met ons op via [vragen Cosmos DB](mailto:askcosmosdb@microsoft.com)|
| Maximale tijdsverschil voor token-autorisatie| 15 min |

Cosmos DB ondersteunt de uitvoering van triggers tijdens schrijfbewerkingen. De service ondersteunt maximaal één vooraf trigger en één na trigger per schrijfbewerking. 

## <a name="sql-query-limits"></a>Limieten voor SQL-query

Cosmos DB ondersteunt het opvragen van items met [SQL](how-to-sql-query.md). De volgende tabel beschrijft de beperkingen in queryinstructies, bijvoorbeeld in termen van aantal van de EU of lengte van de query.

| Resource | Standaardlimiet |
| --- | --- |
| Maximale lengte van de SQL-query| 256 KB <sup>*</sup>|
| Maximum aantal verbindingen per query| 5 <sup>*</sup>|
| Maximale and per query| 2000 <sup>*</sup>|
| Maximale or-items per query| 2000 <sup>*</sup>|
| Maximale UDF's per query| 10 <sup>*</sup>|
| Maximum aantal argumenten per expressie| 6000 <sup>*</sup>|
| Maximum aantal punten per veelhoek| 4096 <sup>*</sup>|

<sup>*</sup> U kunt een van deze SQL-query-limieten verhogen door contact op met ondersteuning van Azure of contact opnemen met ons op via [vragen Cosmos DB](mailto:askcosmosdb@microsoft.com).

## <a name="mongodb-api-specific-limits"></a>MongoDB-API-specifieke limieten

Cosmos DB biedt ondersteuning voor de MongoDB-wire-protocol voor toepassingen die zijn geschreven voor MongoDB. U vindt de ondersteunde opdrachten en protocol-versies op [ondersteund MongoDB-functies en -syntaxis](mongodb-feature-support.md).

De volgende tabel bevat de limieten die specifiek zijn voor ondersteuning van MongoDB-functies. Andere Servicelimieten voor de SQL-code (core) API vermeld, gelden ook voor de MongoDB-API.

| Resource | Standaardlimiet |
| --- | --- |
| Maximale geheugengrootte van de MongoDB-query | 40 MB |
| Maximale uitvoeringstijd voor MongoDB-bewerkingen| 30 s |

## <a name="try-cosmos-db-free-limits"></a>Limieten voor Cosmos DB gratis uitproberen

De volgende tabel bevat de limieten voor de [Azure Cosmosdb gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/) proefversie.

| Resource | Standaardlimiet |
| --- | --- |
| Duur van de proefversie | 30 dagen (kan een willekeurig aantal keren vernieuwd) |
| Maximum aantal containers per abonnement (SQL, Gremlin, tabel-API) | 1 |
| Maximum aantal containers per abonnement (MongoDB-API) | 3 |
| Maximale doorvoer per container | 5000 |
| Maximale doorvoer per database gedeeld-doorvoer | 20000 |
| Max. totale opslag per account | 10 GB |

Probeer Cosmos DB ondersteunt wereldwijde distributie in alleen de regio VS-midden, Noord-Europa en Zuidoost-Azië. Azure ondersteuningstickets kunnen niet worden gemaakt voor Try Azure Cosmos DB-accounts. Echter, wordt ondersteuning geboden voor abonnees met bestaande ondersteuningsabonnementen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Cosmos-DB-core, concepten [wereldwijde distributie](distribute-data-globally.md) en [partitioneren](partitioning-overview.md) en [ingerichte doorvoer](request-units.md).

Lees onze snelstartgidsen om snel aan de slag te gaan met Azure Cosmos DB:

* [Aan de slag met de SQL-API van Azure Cosmos DB](create-sql-api-dotnet.md)
* [Aan de slag met de API van Azure Cosmos DB voor MongoDB](create-mongodb-nodejs.md)
* [Aan de slag met de Cassandra-API van Azure Cosmos DB](create-cassandra-dotnet.md)
* [Aan de slag met de Gremlin-API van Azure Cosmos DB](create-graph-dotnet.md)
* [Aan de slag met de Table-API van Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/)
