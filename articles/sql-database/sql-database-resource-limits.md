---
title: Overzicht van Azure SQL-Database-bronlimieten | Microsoft Docs
description: Dit artikel bevat een overzicht van de resource van Azure SQL Database beperkt en bevat informatie over wat er gebeurt wanneer deze resourcelimieten zijn bereikt of overschreden.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 09/19/2018
ms.author: carlrab
ms.openlocfilehash: fbb73e46ed3aa41b544e8fea5c6184675140e810
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46498146"
---
# <a name="overview-azure-sql-database-resource-limits"></a>Overzicht van Azure SQL Database-resourcebeperkingen 

Dit artikel bevat een overzicht van de resource van Azure SQL Database beperkt en bevat informatie over wat er gebeurt wanneer deze resourcelimieten zijn bereikt of overschreden.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Wat is het maximum aantal servers en databases?

| Maximum | Logische server | Beheerd exemplaar |
| :--- | :--- | :--- |
| Databases per server-exemplaar | 5000 | 100 |
| Standaardaantal servers per abonnement in andere regio 's | 20 | N/A |
| Maximumaantal servers per abonnement in andere regio 's | 200 | N/A | 
| DTU / eDTU-quotum per server | 54,000 | N/A |  
| vCore quotum per server-exemplaar | 540 | 80 |
| Maximum aantal toepassingen per server | afhankelijk van het aantal dtu's of vCores | N/A |
||||

> [!NOTE]
> Als u wilt meer DTU-quotum voor /eDTU, vCore quotum of meer servers dan de standaardinstelling, kan een nieuwe ondersteuningsaanvraag in de Azure-portal voor het abonnement met probleemtype 'Target' worden verzonden. De DTU / eDTU-quota en database-limiet per server beperkt het aantal elastische pools per server. 

> [!IMPORTANT]
> Als het aantal databases de limiet is per server nadert, kan het volgende gebeuren:
> - Latentie in het uitvoeren van query's op de hoofddatabase te vergroten.  Dit omvat de weergave van gebruiksstatistieken resource, zoals sys.resource_stats.
> - Latentie van bewerkingen vergroten en rendering portal standpunten die betrekking hebben op het inventariseren van databases op de server.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Wat gebeurt er wanneer de database-resourcebeperkingen wordt bereikt?

### <a name="compute-dtus-and-edtus--vcores"></a>COMPUTE (dtu's en Edtu / vCores)

Wanneer Databasegebruik voor rekenkracht (gemeten door het aantal dtu's en edtu's of vCores) toeneemt, wordt latentie van query vergroot en kan zelfs time-out. In deze omstandigheden vindt query's kunnen in de wachtrij geplaatst door de service en vindt u bronnen voor uitvoering als resource zijn gratis.
Als hoge compute-gebruik worden aangetroffen, wordt een risicobeperking opties zijn onder andere:

- Het compute vergroten van de database of elastische pool voor de database met meer rekenresources. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Query's te verminderen van het Resourcegebruik van elke query te optimaliseren. Zie voor meer informatie, [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Wanneer ruimte in database gebruikt de maximaal toegestane grootte is bereikt, database ingevoegd en niet bijwerken waarmee u de grootte van de gegevens te verhogen en -clients ontvangen een [foutbericht](sql-database-develop-error-messages.md). Database selecteren en doorgaan met verwijderen te voltooien.

Als hoge ruimteverbruik worden aangetroffen, wordt een risicobeperking opties zijn onder andere:

- Vergroten van de maximale grootte van de database of elastische pool of Voeg meer opslag toe. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Als de database zich in een elastische pool, kan vervolgens ook de database worden verplaatst buiten de groep zodat de opslagruimte wordt niet gedeeld met andere databases.
- Een database voor het vrijmaken van ongebruikte ruimte verkleinen. Zie voor meer informatie, [bestandsruimte in Azure SQL Database beheren](sql-database-file-space-management.md)

### <a name="sessions-and-workers-requests"></a>Sessies en werknemers (aanvragen) 

Het maximum aantal sessies en -werkrollen worden bepaald door de service tier en compute-grootte (dtu's en edtu's). Nieuwe aanvragen zijn afgewezen als sessie- of werkrollen limieten zijn bereikt en clients een foutbericht weergegeven wordt. Terwijl het aantal verbindingen beschikbaar kan worden beheerd door de toepassing, is het aantal gelijktijdige werknemers vaak mogelijk moeilijker te schatten en beheren. Dit geldt met name tijdens perioden van piekbelasting wanneer database resourcelimieten zijn bereikt en werknemers zich opstapelen vanwege langer uitvoeren van query's. 

Als hoog gebruik van sessie- of werkrollen worden aangetroffen, wordt een risicobeperking opties zijn onder andere:
- Vergroten van de service tier of compute-grootte van de database of elastische pool. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Optimaliseren query's om te verlagen van het Resourcegebruik van elke query als de oorzaak van het toegenomen worker-gebruik is vanwege conflicten over rekenresources. Zie voor meer informatie, [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Volgende stappen

- Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over dtu's en edtu's [dtu's en edtu's](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Zie voor meer informatie over de maximale grootte tempdb https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
