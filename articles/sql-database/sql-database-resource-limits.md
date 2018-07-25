---
title: Overzicht van Azure SQL-Database-bronlimieten | Microsoft Docs
description: Deze pagina beschrijft enkele algemene DTU gebaseerde resourcelimieten voor individuele databases in Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/02/2018
ms.author: carlrab
ms.openlocfilehash: 62b0639f134a134739b09593a0b21b47d06699dc
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39236921"
---
# <a name="overview-azure-sql-database-resource-limits"></a>Overzicht van Azure SQL Database-resourcebeperkingen 

Dit artikel bevat een overzicht van de resource van Azure SQL Database beperkt en bevat informatie over wat er gebeurt wanneer deze resourcelimieten zijn bereikt of overschreden.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Wat is het maximum aantal servers en databases?

| Maximum | Waarde |
| :--- | :--- |
| Databases per server | 5000 |
| Standaardaantal servers per abonnement in andere regio 's | 20 |
| Maximumaantal servers per abonnement in andere regio 's | 200 |
| DTU / eDTU-quotum per server | 54,000 |
|||

> [!NOTE]
> Als u wilt meer DTU-quotum voor /eDTU of meer servers dan de standaardinstelling, kan een nieuwe ondersteuningsaanvraag in de Azure-portal voor het abonnement met probleemtype 'Target' worden verzonden. De DTU / eDTU-quota en database-limiet per server beperkt het aantal elastische pools per server. 

> [!IMPORTANT]
> Als het aantal databases de limiet is per server nadert, kan het volgende gebeuren:
> - Latentie in het uitvoeren van query's op de hoofddatabase te vergroten.  Dit omvat de weergave van gebruiksstatistieken resource, zoals sys.resource_stats.
> - Latentie van bewerkingen vergroten en rendering portal standpunten die betrekking hebben op het inventariseren van databases op de server.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Wat gebeurt er wanneer de database-resourcebeperkingen wordt bereikt?

### <a name="compute-dtus-and-edtus--vcores"></a>COMPUTE (dtu's en Edtu / vCores)

Wanneer Databasegebruik voor rekenkracht (gemeten door het aantal dtu's en edtu's of vCores) toeneemt, wordt latentie van query vergroot en kan zelfs time-out. In deze omstandigheden vindt query's kunnen in de wachtrij geplaatst door de service en vindt u bronnen voor uitvoering als resource zijn gratis.
Als hoge compute-gebruik worden aangetroffen, wordt een risicobeperking opties zijn onder andere:

- Het prestatieniveau van de database of elastische pool voor de database met meer rekenresources te vergroten. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Query's te verminderen van het Resourcegebruik van elke query te optimaliseren. Zie voor meer informatie, [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Wanneer ruimte in database gebruikt de maximaal toegestane grootte is bereikt, database ingevoegd en niet bijwerken waarmee u de grootte van de gegevens te verhogen en -clients ontvangen een [foutbericht](sql-database-develop-error-messages.md). Database selecteren en doorgaan met verwijderen te voltooien.

Als hoge ruimteverbruik worden aangetroffen, wordt een risicobeperking opties zijn onder andere:

- Vergroten van de maximale grootte van de database of elastische pool of Voeg meer opslag toe. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Als de database zich in een elastische pool, kan vervolgens ook de database worden verplaatst buiten de groep zodat de opslagruimte wordt niet gedeeld met andere databases.

### <a name="sessions-and-workers-requests"></a>Sessies en werknemers (aanvragen) 

Het maximum aantal sessies en -werkrollen worden bepaald door de servicelaag en het prestatieniveau serviceniveau (dtu's en edtu's). Nieuwe aanvragen zijn afgewezen als sessie- of werkrollen limieten zijn bereikt en clients een foutbericht weergegeven wordt. Terwijl het aantal verbindingen beschikbaar kan worden beheerd door de toepassing, is het aantal gelijktijdige werknemers vaak mogelijk moeilijker te schatten en beheren. Dit geldt met name tijdens perioden van piekbelasting wanneer database resourcelimieten zijn bereikt en werknemers zich opstapelen vanwege langer uitvoeren van query's. 

Als hoog gebruik van sessie- of werkrollen worden aangetroffen, wordt een risicobeperking opties zijn onder andere:
- Het serviceniveau voor servicelaag of het prestatieniveau van de database of elastische pool te vergroten. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Optimaliseren query's om te verlagen van het Resourcegebruik van elke query als de oorzaak van het toegenomen worker-gebruik is vanwege conflicten over rekenresources. Zie voor meer informatie, [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Volgende stappen

- Zie [Veelgestelde vragen over SQL-Database](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over dtu's en edtu's [dtu's en edtu's](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Zie voor meer informatie over de maximale grootte tempdb https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
