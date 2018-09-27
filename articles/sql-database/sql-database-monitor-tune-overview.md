---
title: Bewaking en het afstemmen van prestaties - Azure SQL Database | Microsoft Docs
description: Tips voor het afstemmen in Azure SQL Database via evaluatie en verbetering van de prestaties.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 07/16/2018
ms.openlocfilehash: c79285247950510791ede915fcf0e5373792044f
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165038"
---
# <a name="monitoring-and-performance-tuning"></a>Bewaking en prestatieafstemming

Azure SQL-Database wordt automatisch beheerd en flexibele gegevensservice waar kunt u eenvoudig gebruik te bewaken, toevoegen of verwijderen van resources (CPU, geheugen, i/o-), aanbevelingen die kunnen verbeteren de prestaties van uw database of database aan te passen aan uw workload kunt vinden en automatisch optimaliseren.

## <a name="overview-of-monitoring-database-performance-in-azure-sql-database"></a>Overzicht van de databaseprestaties bewaken in Azure SQL Database
Het bewaken van de prestaties van een SQL-database in Azure begint met het bewaken van het resourcegebruik ten opzichte van het gekozen niveau van databaseprestaties. Bewaking helpt u te bepalen of uw database overtollige capaciteit heeft of problemen heeft juist omdat resources volledig worden benut uit en klikt u vervolgens beslissen of is het tijd om de grootte van compute aanpassen en service-lagen van de database in de [kopen op basis van DTU model](sql-database-service-tiers-dtu.md) of [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md). U kunt uw database bewaken in de [Azure-portal](https://portal.azure.com) met behulp van de volgende grafische hulpprogramma's of SQL [dynamische beheerweergave (DMV's)](sql-database-monitoring-with-dmvs.md).

Azure SQL Database kunt u voor het identificeren van kansen te verbeteren en optimaliseren van prestaties van query's zonder te hoeven wijzigen van resources aan de hand [aanbevelingen voor afstemming](sql-database-advisor.md). Ontbrekende indexen en onvoldoende geoptimaliseerde query's zijn veelvoorkomende redenen voor slechte databaseprestaties. U kunt deze aanbevelingen voor afstemming voor het verbeteren van de prestaties van uw workload kunt toepassen.
U kunt ook laat Azure SQL database tot [automatisch de prestaties van uw query's optimaliseren](sql-database-automatic-tuning.md) door het toepassen van alle aanbevelingen en te controleren dat ze betere databaseprestaties geïdentificeerd. U hebt de volgende opties voor controle en probleemoplossing van de prestaties van de database:

- In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-databases**, selecteert u de database en gebruik vervolgens de controle grafiek om te zoeken naar resources hun maximale nadert. DTU-verbruik wordt standaard weergegeven. Klik op **bewerken** te wijzigen van het tijdsbereik en de waarden die worden weergegeven.
- Gebruik [Query Performance Insight](sql-database-query-performance.md) voor het identificeren van de query's die besteden aan het meest van resources.
- Gebruik [SQL Database Advisor](sql-database-advisor-portal.md) om aanbevelingen voor het maken en verwijderen van indexen, parametriseren query's en schema-problemen oplossen weer te geven.
- Gebruik [Intelligent Insights van Azure SQL](sql-database-intelligent-insights.md) voor de automatische bewaking van de databaseprestaties van uw. Zodra een prestatieprobleem wordt gedetecteerd, wordt een diagnostisch logboek gegenereerd met de details en Root oorzaak Analysis (RCA) van het probleem. Prestaties verbetering aanbeveling wordt geleverd, indien mogelijk.
- [Automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md) en laat Azure SQL database automatisch correctie geïdentificeerd prestatieproblemen kunnen voordoen.
- U kunt ook gebruiken [dynamische beheerweergave (DMV's)](sql-database-monitoring-with-dmvs.md), [uitgebreid gebeurtenissen (`XEvents`) (sql-database/sql-database-xevent-db-diff-from-svr.md), en de [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) om prestaties te verkrijgen de parameters in realtime. Zie [prestatierichtlijnen](sql-database-performance-guidance.md) technieken die u gebruiken kunt om prestaties te verbeteren van Azure SQL Database als u een probleem opgetreden met behulp van deze rapporten of weergaven gevonden.

## <a name="monitor-databases-using-the-azure-portal"></a>Databases bewaken via de Azure-portal
In de [Azure-portal](https://portal.azure.com/) kunt u het verbruik van een individuele database bewaken door de database te selecteren en op de grafiek **Bewaking** te klikken. Nu wordt een venster **Metrische gegevens** geopend, dat u kunt wijzigen door op de knop **Grafiek bewerken** te klikken. Voeg de volgende metrische gegevens toe:

* CPU-percentage
* DTU-percentage
* Gegevens-I/O-percentage
* Databaseomvangpercentage

Als u deze metrische gegevens hebt toegevoegd, kunt u doorgaan met deze bekijken in de **bewaking** grafiek met meer informatie over de **Metric** venster. De vier metrische gegevens tonen het gemiddelde gebruikspercentage ten opzichte van de **DTU** van uw database. Zie de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) artikelen voor meer informatie over Servicelagen.  

![Servicelaagbewaking van databaseprestaties.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

U kunt ook meldingen configureren voor prestatiewaarden. Klik op de knop **Melding toevoegen** in het venster **Metrische gegevens**. Volg de wizard om de melding te configureren. U hebt de keuze om een melding weer te geven als de metrische gegevens een bepaalde drempelwaarde overschrijden of als het meetpunt onder een bepaalde drempelwaarde komt.

Als u bijvoorbeeld verwacht dat de workload van de database zal toenemen, kunt u configureren dat er een e-mailmelding wordt verstuurd wanneer de database 80% van een van de prestatiewaarden heeft bereikt. U kunt dit gebruiken als een vroegtijdige waarschuwing te achterhalen wanneer u misschien moet overschakelen naar de volgende hogere compute-grootte.

De maatstaven voor prestaties kunt u bepalen of u moet downgraden naar een lagere compute-grootte. Stel dat u een Standard S2-database gebruikt en alle prestatiewaarden aangeven dat de database gemiddeld nooit meer dan 10% gebruikt. Het is dan waarschijnlijk dat de database in Standard S1 goed werkt. Echter rekening houden met workloads die pieken of fluctueren, voordat u de beslissing om te verplaatsen naar een lagere compute-grootte.

## <a name="improving-database-performance-with-more-resources"></a>Verbetering van de prestaties van de database met andere bronnen

Ten slotte, als er geen bruikbare items die de prestaties van uw database kunnen verbeteren, kunt u de hoeveelheid beschikbare resources in Azure SQL Database. U kunt meer resources toewijzen door het veranderen van de [DTU-servicelaag](sql-database-service-tiers-dtu.md) van een individuele database of een toename van het aantal edtu's van een elastische pool op elk gewenst moment. U kunt ook als u de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md), kunt u de servicelaag wijzigen of de resources die zijn toegewezen aan uw database te verhogen. 
1. Voor individuele databases, kunt u [Servicelagen](sql-database-service-tiers-dtu.md) of [rekenresources](sql-database-service-tiers-vcore.md) on-demand om betere databaseprestaties.
2. Overweeg het gebruik van meerdere databases, [elastische pools](sql-database-elastic-pool-guidance.md) bronnen automatisch schalen.

## <a name="tune-and-refactor-application-or-database-code"></a>Afstemmen en herstructureren toepassing of databasecode

U kunt de code van de toepassing als u wilt meer optimaal gebruik van de database, wijzigen van indexen, geforceerd plannen of hints gebruiken om handmatig de database aan uw workload kunt wijzigen. Vindt u enkele richtlijnen en tips voor handmatige afstemmen en voor het herschrijven van de code in de [prestaties richtlijnen onderwerp](sql-database-performance-guidance.md) artikel.


## <a name="next-steps"></a>Volgende stappen

- Als u wilt inschakelen automatisch afstemmen in Azure SQL Database en laat de functie voor automatisch afstemmen uw werkbelasting volledig te beheren, Zie [automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md).
- Voor het gebruik van handmatige afstemmen, kunt u bekijken [aanbevelingen in Azure-portal voor het afstemmen](sql-database-advisor-portal.md) en handmatig toe te passen degene die de prestaties van uw query's verbeteren.
- Wijzigen van de resources die beschikbaar in uw database door het veranderen van zijn [Azure SQL Database-Servicelagen](sql-database-performance-guidance.md)
