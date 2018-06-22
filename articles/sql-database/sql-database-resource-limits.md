---
title: Azure SQL Database-resource beperkt overzicht | Microsoft Docs
description: Deze pagina beschrijft enkele algemene resource op basis van het DTU-limieten voor individuele databases in Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 3cf78369a7177eb3b2e384af0cc81563f6806137
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309986"
---
# <a name="overview-azure-sql-database-resource-limits"></a>Overzicht van Azure SQL Database limieten 

In dit artikel biedt een overzicht van de Azure SQL Database-resource beperkt en bevat informatie over wat er gebeurt wanneer deze limieten zijn bereikt of overschreden.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Wat is het maximum aantal servers en databases?

| Maximum | Waarde |
| :--- | :--- |
| Databases per server | 5000 |
| Standaardaantal servers per abonnement in elke regio | 20 |
| Maximumaantal servers per abonnement in elke regio | 200 |
|||

> [!NOTE]
> Als u meer serverquota dan de standaardhoeveelheid, kan een nieuw ondersteuningsverzoek in de Azure portal voor het abonnement met type probleem 'Target' worden verzonden.

> [!IMPORTANT]
> Als het aantal databases de limiet is per server nadert, kan het volgende gebeuren:
> - Latentie in met het uitvoeren van query's in de database master te vergroten.  Het gaat hierbij om weergaven van de gebruiksstatistieken resource zoals sys.resource_stats.
> - Latentie in beheerbewerkingen vergroten en opbouwen van de portal standpunten die betrekking hebben op inventariseren van de server-databases.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Wat gebeurt er wanneer database limieten zijn bereikt?

### <a name="compute-dtus-and-edtus--vcores"></a>Berekenen (dtu's en edtu's / vCores)

Wanneer database compute-gebruik (gemeten door dtu's en edtu's of vCores) toeneemt, query uitvoeren op de toename in latentie en kan zelfs time-out. In deze omstandigheden query's kunnen in de wachtrij geplaatst door de service en vindt u bronnen voor uitvoering als resource komen.
Wanneer zich voordoen tijdens een hoge compute-gebruik, wordt een risicobeperking opties zijn onder andere:

- Uitbreiding van het prestatieniveau van de database of de elastische groep om de database met meer computerresources. Zie [resources voor één database schalen](sql-database-single-database-scale.md) en [schalen resources van elastische pool](sql-database-elastic-pool-scale.md).
- Query's te verlagen van het Resourcegebruik van elke query te optimaliseren. Zie voor meer informatie [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Storage

Wanneer databaseruimte gebruikt de maximale limiet bereikt, database worden ingevoegd en updates die Verhoog de gegevensgrootte niet als clients ontvangen een [foutbericht](sql-database-develop-error-messages.md). Database selecteert en VERWIJDERINGEN blijven slagen.

Wanneer zich voordoen tijdens een hoge ruimtegebruik, wordt een risicobeperking opties zijn onder andere:

- Verhogen van de maximale grootte van de database of de elastische groep of Voeg meer opslag toe. Zie [resources voor één database schalen](sql-database-single-database-scale.md) en [schalen resources van elastische pool](sql-database-elastic-pool-scale.md).
- Als de database zich in een elastische groep, kan klikt u vervolgens ook de database worden verplaatst buiten de groep zodat de opslagruimte wordt niet gedeeld met andere databases.

### <a name="sessions-and-workers-requests"></a>Sessies en werknemers (aanvragen) 

Het maximum aantal sessies en werknemers worden bepaald door de prijscategorie en prestatieniveau serviceniveau (dtu's en edtu's). Nieuwe aanvragen worden geweigerd als sessie- of worker limieten zijn bereikt en clients een foutbericht weergegeven wordt. Terwijl het aantal verbindingen beschikbaar kan worden beheerd door de toepassing, is het aantal gelijktijdige werknemers vaak moeilijker te schatten en te beheren. Dit is vooral van toepassing tijdens de piekuren van load wanneer database limieten zijn bereikt en werknemers opstapelen vanwege langer actieve query's. 

Wanneer zich voordoen tijdens een hoog gebruik van sessie- of worker, wordt een risicobeperking opties zijn onder andere:
- Het serviceniveau voor laag of de prestaties van de database of elastische pool te vergroten. Zie [resources voor één database schalen](sql-database-single-database-scale.md) en [schalen resources van elastische pool](sql-database-elastic-pool-scale.md).
- Optimaliseren query's te verlagen van het Resourcegebruik van elke query als de oorzaak van het gebruik van verhoogde worker vanwege conflicten over berekenen bronnen. Zie voor meer informatie [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

Wanneer zich voordoen tijdens een hoog gebruik van sessie- of worker, wordt een risicobeperking opties zijn onder andere:
- Het serviceniveau voor laag of de prestaties van de database te vergroten. Zie [resources voor één database schalen](sql-database-single-database-scale.md) en [schalen resources van elastische pool](sql-database-elastic-pool-scale.md).
- Optimaliseren query's te verlagen van het Resourcegebruik van elke query als de oorzaak van het gebruik van verhoogde worker vanwege conflicten over berekenen bronnen. Zie voor meer informatie [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Database Veelgestelde vragen over](sql-database-faq.md) voor antwoorden op veelgestelde vragen.
- Zie voor meer informatie over de algemene Azure limieten [Azure-abonnement en Servicelimieten, quota's en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over dtu's en edtu's [dtu's en edtu's](sql-database-what-is-a-dtu.md).
- Zie voor meer informatie over de maximale grootte tempdb https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
