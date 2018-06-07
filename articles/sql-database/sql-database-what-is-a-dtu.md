---
title: 'SQL Database: wat is een DTU? | Microsoft Docs'
description: Inzicht in wat een Azure SQL Database-transactie-eenheid is.
keywords: databaseopties, prestaties van de database
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 50b12de2400f0db90108e3eeb158357039ac92ec
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649819"
---
# <a name="database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Database Transaction Units (dtu's) en elastische Database Transaction Units (edtu's)
In dit artikel wordt uitgelegd wat DTU's (Database Transaction Units) en eDTU's (elastische Database Transaction Units) zijn en wat er gebeurt als u het maximale aantal DTU's of eDTU's hebt bereikt. Zie voor specifieke informatie over de prijzen, [prijzen van Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/single/).

## <a name="what-are-database-transaction-units-dtus"></a>Wat zijn's (Database Transaction Units)?
Voor één Azure SQL database op een specifiek prestatieniveau binnen een [servicelaag](sql-database-single-database-resources.md), Microsoft garandeert een bepaalde mate van resources voor die database (onafhankelijk van een andere database in de Azure-cloud) bieden een voorspelbare niveau van de prestaties. De hoeveelheid resources wordt berekend als een aantal Database Transaction Units of dtu's en is een gebundelde meting van de berekenings-, opslag- en i/o-resources. De verhouding tussen deze resources oorspronkelijk is bepaald door een [benchmark-OLTP-werkbelasting](sql-database-benchmark-overview.md), leidt tot die kenmerkend zijn voor echte OLTP-werkbelastingen. Wanneer uw werkbelasting het bedrag van elk van deze resources overschrijdt, is uw doorvoer beperkte - resulterende in tragere prestaties en time-outs. De beschikbare bronnen voor andere SQL-databases in de Azure-cloud hebben geen invloed op de bronnen die worden gebruikt door de werkbelasting en de resources die worden gebruikt door andere werkbelastingen hebben geen invloed op de beschikbare bronnen voor de SQL-database.

![selectiekader](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu's zijn vooral handig zijn om de relatieve hoeveelheid resources tussen Azure SQL-Databases op verschillende prestatieniveaus en Servicelagen te begrijpen. Bijvoorbeeld, verdubbeling van het aantal dtu's door het verhogen van het prestatieniveau van een database is gelijk aan de set beschikbare bronnen voor die database verdubbelen. Zo biedt een Premium P11-database met 1750 DTU's 350 keer meer DTU aan rekenvermogen dan een Basic-database met 5 DTU's.  

Gebruik voor het verkrijgen van meer inzicht in het brongebruik (DTU) van uw werkbelasting [Azure SQL Database Query Performance Insight](sql-database-query-performance.md) naar:

- De top-query's op het aantal CPU/duur/uitvoering die mogelijk kan worden afgestemd voor verbeterde prestaties identificeren. Bijvoorbeeld, een i/o-intensieve query kan profiteren van het gebruik van [in het geheugen optimalisatietechnieken](sql-database-in-memory.md) beter gebruikmaken van het beschikbare geheugen op een bepaalde prijscategorie en prestatieniveau serviceniveau.
- Inzoomen op de details van een query, geven de tekst en de geschiedenis van bronnen beter worden benut.
- Access-prestaties afstemmen van de aanbevelingen die acties uitgevoerd door weergeven [SQL Database Advisor](sql-database-advisor.md).

U kunt wijzigen [DTU Servicelagen](sql-database-service-tiers-dtu.md) op elk gewenst moment met minimale downtime voor uw toepassing (gemiddeld over het algemeen binnen vier seconden). Voor veel bedrijven en apps is het kunnen maken van databases en het naar wens omhoog of omlaag schalen van de prestaties al voldoende, vooral als de gebruikspatronen redelijk voorspelbaar zijn. Bij onvoorspelbare gebruikspatronen kan het echter lastig zijn uw kosten en bedrijfsmodel effectief te beheren. In dit scenario gebruikt u een elastische pool met een bepaald aantal edtu's die worden gedeeld door meerdere databases in de pool.

![Inleiding tot SQL Database: DTU's van individuele database per laag en niveau](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Wat zijn de elastische Database Transaction Units (edtu's)?
In plaats daarvan dan bieden een specifieke set resources (dtu's) die mogelijk niet altijd nodig, voor een SQL-Database die altijd beschikbaar is, u kunt plaatsen databases in een [elastische pool](sql-database-elastic-pool.md) op een SQL-Database-server die een pool van resources tussen deelt Deze databases. De gedeelde bronnen in een elastische pool worden gemeten met behulp van de elastische Database Transaction Units of edtu's. Elastische pools bieden een eenvoudige, rendabele oplossing voor het beheren van de prestatiedoelstellingen voor meerdere databases met breed uiteenlopende en onvoorspelbare gebruikspatronen. Een elastische pool wordt gegarandeerd dat resources kunnen niet worden gebruikt door een database in de groep, terwijl elke database in de groep altijd gezorgd een minimale hoeveelheid benodigde resources beschikbaar heeft. 

![Inleiding tot SQL Database: eDTU's per laag en niveau](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Een pool krijgt een bepaald aantal edtu's voor de prijs van een set. Binnen de elastische pool hebben afzonderlijke databases de flexibiliteit om de schaal automatisch aan te passen binnen de geconfigureerde grenzen. Een database zwaarder belast verbruikt meer edtu's om vraag te voldoen. Databases onder lichter belasting verbruikt minder edtu's. Databases zonder belasting verbruiken geen edtu's. Door het inrichten van resources voor de hele pool zijn plaats per database beheertaken vereenvoudigd, een voorspelbare budget voorzien in de groep.

Er kunnen extra eDTU's aan een bestaande pool worden toegevoegd zonder uitvaltijd van de database en zonder gevolgen voor de databases in de pool. En als de extra eDTU's niet meer nodig zijn, kunnen ze op elk moment uit een bestaande pool worden verwijderd. U kunt toevoegen of databases aan de pool afgetrokken of limiet voor de hoeveelheid edtu's dat een database onder een zware belasting kunt gebruiken om te reserveren edtu's voor andere databases. Als een database is zoals verwacht onder-gebruik van bronnen die u kunt verplaatsen buiten de toepassingen en configureren als een individuele database met een voorspelbare hoeveelheid vereiste resources.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Hoe bepaal ik hoeveel DTU's ik met mijn workload nodig heb?
Als u een bestaande workload voor virtuele on-premises of SQL Server-machines wilt migreren naar Azure SQL Database, kunt u de [DTU-rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken om een schatting te maken van het aantal benodigde DTU's. Voor de werkbelasting van een bestaande Azure SQL Database, kunt u [SQL Database Query Performance Insight](sql-database-query-performance.md) verbruik (Dtu) diepere inzichten voor het optimaliseren van uw werkbelasting van uw database begrijpen. U kunt ook de [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV om brongebruik voor het afgelopen uur weer te geven. U kunt ook de catalogusweergave [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) verbruik van de laatste 14 dagen, maar in een lagere betrouwbaarheid van vijf minuten gemiddelden wordt weergegeven.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Hoe weet ik of een elastische pool met resources voor mij voordeliger is?
Pools zijn geschikt voor een groot aantal databases met specifieke gebruikspatronen. Voor een bepaalde database wordt dit patroon gekenmerkt door een laag gebruik gemiddelde met relatief incidentele gebruik pieken. SQL Database evalueert automatisch het historisch resourcegebruik van databases in een bestaande SQL Database-server en op basis daarvan wordt de juiste poolconfiguratie in de Azure-portal aanbevolen. Zie [Wanneer moet een elastische pool worden gebruikt?](sql-database-elastic-pool.md) voor meer informatie.

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Wat gebeurt er wanneer ik mijn maximale aantal dtu's bereikt?
Prestaties zijn kalibreren en om de resources die nodig zijn voor het uitvoeren van de werkbelasting van uw database tot het toegestane maximum voor de geselecteerde service laag/prestatieniveau geregeld. Als uw werkbelasting is kunt u door een van de CPU/gegevens i/o/logboek-i/o-limieten, u ontvangt nog altijd het maximumniveau van toegestane bronnen, maar u zult waarschijnlijk ook toegenomen query latenties ervaren. Deze limieten resulteren niet in fouten, maar wel in een vertraging van de workload, tenzij de vertraging zo ernstig wordt dat query's een time-out geven. Als u de maximale toegestane gelijktijdige sessies/gebruikersaanvragen (werkthreads) bereikt, ziet u expliciete fouten. Zie [limieten voor Azure SQL Database]( sql-database-dtu-resource-limits.md#what-happens-when-database-and-elastic-pool-resource-limits-are-reached) voor meer informatie over limieten niet gerelateerd aan CPU, geheugen, i/o gegevens of transactie logboek IO.

## <a name="next-steps"></a>Volgende stappen
* Zie [aankoopmodel DTU gebaseerde](sql-database-service-tiers-dtu.md) Meld i/o voor informatie over het dtu's en beschikbaar voor individuele databases en elastische groepen met edtu's, evenals beperkingen met betrekking tot bronnen dan CPU, geheugen, i/o gegevens en de transactie.
* Zie [vCore gebaseerde aankoopmodel (preview)](sql-database-service-tiers-vcore.md) voor meer informatie over de toewijzing en service lagen vCore op basis van een resource. 
* Zie [SQL Database Query Performance Insight](sql-database-query-performance.md) als u meer wilt weten over uw verbruik (DTU's).
* Zie [SQL Database benchmark overview](sql-database-benchmark-overview.md) (Overzicht van SQL Database-benchmarks) voor meer informatie over de methodologie achter de OLTP-benchmarkworkload waarmee de DTU-combinatie wordt bepaald.
