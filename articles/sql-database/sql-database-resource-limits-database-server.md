---
title: Azure SQL Database server-bronlimieten | Microsoft Docs
description: In dit artikel biedt een overzicht van de Azure SQL Database-server resourcelimieten voor individuele databases en elastische pools. Het bevat ook informatie over wat er gebeurt wanneer deze resourcelimieten zijn bereikt of overschreden.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan,moslake,josack
manager: craigg
ms.date: 03/01/2019
ms.openlocfilehash: 801b7de4b82c37503f2a14619112cbf46ca60a43
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57447078"
---
# <a name="sql-database-resource-limits-for-azure-sql-database-server"></a>SQL Database-resourcebeperkingen voor Azure SQL Database-server

In dit artikel biedt een overzicht van de SQL Database-resourcebeperkingen voor een SQL Database-server die wordt beheerd van individuele databases en elastische pools. Het bevat ook informatie over wat er gebeurt wanneer deze resourcelimieten zijn bereikt of overschreden.

> [!NOTE]
> Zie voor de grenzen van beheerde exemplaren, [SQL Database-resourcebeperkingen voor beheerde exemplaren](sql-database-managed-instance-resource-limits.md).

## <a name="maximum-resource-limits"></a>Maximale resourcebeperkingen

| Resource | Limiet |
| :--- | :--- |
| Databases per server | 5000 |
| Standaardaantal servers per abonnement in andere regio 's | 20 |
| Maximumaantal servers per abonnement in andere regio 's | 200 |  
| DTU / eDTU-quotum per server | 54,000 |  
| vCore quotum per server-exemplaar | 540 |
| Maximum aantal toepassingen per server | Beperkt door het aantal dtu's of vCores. Bijvoorbeeld, als elke groep 1000 dtu's is, klikt u vervolgens een server kan ondersteunen 54 pools.|
||||

> [!NOTE]
> Als u wilt meer DTU-quotum voor /eDTU, vCore quotum of meer servers dan de standaardinstelling, kan een nieuwe ondersteuningsaanvraag in de Azure-portal voor het abonnement met probleemtype 'Target' worden verzonden. De DTU / eDTU-quota en database-limiet per server beperkt het aantal elastische pools per server.
> [!IMPORTANT]
> Als het aantal databases de limiet is per SQL Database-server nadert, kan het volgende gebeuren:
> - Latentie in het uitvoeren van query's op de hoofddatabase te vergroten.  Dit omvat de weergave van gebruiksstatistieken resource, zoals sys.resource_stats.
> - Latentie van bewerkingen vergroten en rendering portal standpunten die betrekking hebben op het inventariseren van databases op de server.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Wat gebeurt er na het verstrijken van database-resourcebeperkingen

### <a name="compute-dtus-and-edtus--vcores"></a>COMPUTE (dtu's en Edtu / vCores)

Wanneer Databasegebruik voor rekenkracht (gemeten door het aantal dtu's en edtu's of vCores) toeneemt, wordt latentie van query vergroot en kan zelfs time-out. In deze omstandigheden vindt query's kunnen in de wachtrij geplaatst door de service en vindt u bronnen voor uitvoering als resource zijn gratis.
Als hoge compute-gebruik worden aangetroffen, wordt een risicobeperking opties zijn onder andere:

- Het compute vergroten van de database of elastische pool voor de database met meer rekenresources. Zie [schalen van één database-resources](sql-database-single-database-scale.md) en [resources voor elastische pool schalen](sql-database-elastic-pool-scale.md).
- Query's te verminderen van het Resourcegebruik van elke query te optimaliseren. Zie voor meer informatie, [Query afstemmen/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Opslag

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

## <a name="transaction-log-rate-governance"></a>Transaction Log tarief Governance 
Transaction log tarief governance is een proces in Azure SQL Database gebruikt voor het beperken van hoge opname-tarieven voor workloads zoals bulksgewijs invoegen, SELECT INTO en indexbuilds. Deze limieten worden bijgehouden en worden afgedwongen op het niveau dan een seconde voor het aantal records genereren van logboekbestanden, beperkende doorvoer, ongeacht hoeveel IOs kan worden uitgegeven voor de gegevensbestanden.  Transactietarieven log generatie op dit moment worden lineair geschaald tot een tijdstip dat is afhankelijk van de hardware, snelheid toegestaan met de maximumgrootte van het logboek wordt 48 MB/s met de vCore model kopen. 

> [!NOTE]
> De daadwerkelijke fysieke IOs aan logbestanden van transacties worden niet geregeld of die beperkt zijn. 

Logboek-tarieven zijn ingesteld, zodat ze kunnen worden bereikt en aanhoudend in tal van scenario's, terwijl het algehele systeem de functionaliteit ervan met minimale gevolgen voor de gebruikersbelasting onderhouden kunt. Logboek tarief bestuur zorgt ervoor dat back-ups binnen een gepubliceerde herstelmogelijkheden Sla's blijven transactielogboek.  Deze governance voorkomt ook dat voor een overmatige achterstand op secundaire replica's.

Als de logboekrecords worden gegenereerd, wordt elke bewerking geëvalueerd en beoordeeld op of moet worden vertraagd om te kunnen handhaven een maximale gewenste log-tarief (MB/s per seconde). De vertragingen zijn niet toegevoegd wanneer de records in logboek registreren in plaats daarvan worden weggeschreven naar de opslag, log tarief governance wordt toegepast tijdens het genereren van frequentie logboek zelf.

De werkelijke logboekgeneratie tarieven die zijn opgelegd tijdens runtime kunnen ook worden beïnvloed door mechanismen voor feedback, tijdelijk vermindering van de toegestane log-tarieven, zodat het systeem kunt stabiliseren. Ruimtebeheer van logboekbestanden, actieve naar buiten het logboek ruimte voorwaarden en beschikbaarheidsgroep replicatiemechanismen vermijden kan de grenzen van het hele systeem tijdelijk verlagen. 

Vormgeven van logboek tarief resourceregeling verkeer wordt opgehaald via de volgende wait-typen (waarbij wordt aangemeld met de [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) DMV):

| Type wachten | Opmerkingen |
| :--- | :--- |
| LOG_RATE_GOVERNOR | Database beperken |
| POOL_LOG_RATE_GOVERNOR | Groep van toepassingen beperken |
| INSTANCE_LOG_RATE_GOVERNOR | Exemplaar niveau beperken |  
| HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE | Besturingselement voor feedback, beschikbaarheid van groep fysieke replicatie in Premium/bedrijfskritiek niet kunnen |  
| HADR_THROTTLE_LOG_RATE_LOG_SIZE | Feedback-besturingselement, beperken tarieven om te voorkomen dat een out-of ruimte-logboekvoorwaarde |
||||

Als een frequentielimiet van logboekbestanden die de gewenste schaalbaarheid is die worden aangetroffen, houd rekening met de volgende opties:
- Schaal omhoog naar een grotere laag om op te halen van de maximale 48 MB/s log snelheid. 
- Als gegevens worden geladen tijdelijke, kan dat wil zeggen staging-gegevens in een ETL-proces, deze worden geladen in tempdb (die minimaal wordt geregistreerd). 
- Voor analytische scenario's door in een geclusterde columnstore gedekt-tabel te laden. Dit beperkt de vereiste log snelheid vanwege compressie. Deze techniek neemt CPU-gebruik en is alleen van toepassing op gegevenssets die baat bij een geclusterde columnstore-indexen hebben. 

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over algemene Azure-limieten [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md).
- Zie voor meer informatie over dtu's en edtu's [dtu's en edtu's](sql-database-purchase-models.md#dtu-based-purchasing-model).
- Zie voor meer informatie over de maximale grootte tempdb [TempDB in Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
