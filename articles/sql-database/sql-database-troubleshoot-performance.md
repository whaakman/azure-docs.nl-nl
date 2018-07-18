---
title: Bewaking en het afstemmen van prestaties - Azure SQL Database | Microsoft Docs
description: Tips voor het afstemmen in Azure SQL Database via evaluatie en verbetering van de prestaties.
services: sql-database
author: v-shysun
manager: craigg
editor: ''
keywords: SQL-prestaties afstemmen, prestaties van de database afstemmen, sql-prestaties afstemmen tips voor het afstemmen van prestaties van sql database
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: v-shysun
ms.openlocfilehash: 79f41ab133cba539e5f855b3ab8fa21723694acb
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092559"
---
# <a name="monitoring-and-performance-tuning"></a>Bewaking en prestatieafstemming

Azure SQL-Database wordt automatisch beheerd en flexibele gegevensservice waar kunt u eenvoudig gebruik te bewaken, toevoegen of verwijderen van resources (CPU, geheugen, i/o-), aanbevelingen die kunnen verbeteren de prestaties van uw database of database aan te passen aan uw workload kunt vinden en automatisch optimaliseren.

In dit artikel bevat een overzicht van bewaking en afstemming van opties die beschikbaar in Azure SQL Database zijn.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>Bewaking en probleemoplossing van de prestaties van de database

Azure SQL Database kunt u eenvoudig uw Databasegebruik controleren en identificeren van query's die de prestatieproblemen kunnen veroorzaken. U kunt de databaseprestaties met behulp van Azure portal of het systeem weergaven bewaken. U hebt de volgende opties voor controle en probleemoplossing van de prestaties van de database:

1. In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-databases**, selecteert u de database en gebruik vervolgens de controle grafiek om te zoeken naar resources hun maximale nadert. DTU-verbruik wordt standaard weergegeven. Klik op **bewerken** te wijzigen van het tijdsbereik en de waarden die worden weergegeven.
2. Gebruik [Query Performance Insight](sql-database-query-performance.md) voor het identificeren van de query's die besteden aan het meest van resources.
3. U kunt dynamische beheerweergave (DMV's), Extended Events (`XEvents`), en de Query Store in SSMS prestatieparameters in realtime ophalen.

Zie de [prestaties richtlijnen onderwerp](sql-database-performance-guidance.md) technieken die u gebruiken kunt om prestaties te verbeteren van Azure SQL Database als u een probleem opgetreden met behulp van deze rapporten of weergaven gevonden.

> [!IMPORTANT] 
> Het wordt aanbevolen om altijd de nieuwste versie van Management Studio te gebruiken, zodat uw versie gesynchroniseerd blijft met updates voor Microsoft Azure en SQL Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-to-improve-performance"></a>Database voor betere prestaties optimaliseren

Azure SQL Database kunt u voor het identificeren van kansen te verbeteren en optimaliseren van prestaties van query's zonder te hoeven wijzigen van resources aan de hand [aanbevelingen voor afstemming](sql-database-advisor.md). Ontbrekende indexen en onvoldoende geoptimaliseerde query's zijn veelvoorkomende redenen voor slechte databaseprestaties. U kunt deze aanbevelingen voor afstemming voor het verbeteren van de prestaties van uw workload kunt toepassen.
U kunt ook laat Azure SQL database tot [automatisch de prestaties van uw query's optimaliseren](sql-database-automatic-tuning.md) door het toepassen van alle aanbevelingen en te controleren dat ze betere databaseprestaties geïdentificeerd. U kunt de volgende opties gebruiken om de prestaties van uw database te verbeteren:

1. Gebruik [SQL Database Advisor](sql-database-advisor-portal.md) om aanbevelingen voor het maken en verwijderen van indexen, parametriseren query's en schema-problemen oplossen weer te geven.
2. [Automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md) en laat Azure SQL database automatisch correctie geïdentificeerd prestatieproblemen kunnen voordoen.

## <a name="improving-database-performance-with-more-resources"></a>Verbetering van de prestaties van de database met andere bronnen

Ten slotte, als er geen bruikbare items die de prestaties van uw database kunnen verbeteren, kunt u de hoeveelheid beschikbare resources in Azure SQL Database. U kunt meer resources toewijzen door het veranderen van de [DTU-servicelaag](sql-database-service-tiers-dtu.md) van een zelfstandige database of een toename van het aantal edtu's van een elastische pool op elk gewenst moment. U kunt ook als u de [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md), kunt u de servicelaag wijzigen of de resources die zijn toegewezen aan uw database te verhogen. 
1. Voor zelfstandige databases, kunt u [Servicelagen](sql-database-service-tiers-dtu.md) of [rekenresources](sql-database-service-tiers-vcore.md)on-demand om betere databaseprestaties.
2. Overweeg het gebruik van meerdere databases, [elastische pools](sql-database-elastic-pool-guidance.md) bronnen automatisch schalen.

## <a name="tune-and-refactor-application-or-database-code"></a>Afstemmen en herstructureren toepassing of databasecode

U kunt de code van de toepassing als u wilt meer optimaal gebruik van de database, wijzigen van indexen, geforceerd plannen of hints gebruiken om handmatig de database aan uw workload kunt wijzigen. Vindt u enkele richtlijnen en tips voor handmatige afstemmen en voor het herschrijven van de code in de [prestaties richtlijnen onderwerp](sql-database-performance-guidance.md) artikel.


## <a name="next-steps"></a>Volgende stappen

- Als u wilt inschakelen automatisch afstemmen in Azure SQL Database en laat de functie voor automatisch afstemmen uw werkbelasting volledig te beheren, Zie [automatisch instellen inschakelen](sql-database-automatic-tuning-enable.md).
- Voor het gebruik van handmatige afstemmen, kunt u bekijken [aanbevelingen in Azure-portal voor het afstemmen](sql-database-advisor-portal.md) en handmatig toe te passen degene die de prestaties van uw query's verbeteren.
- Wijzigen van de resources die beschikbaar in uw database door het veranderen van zijn [Azure SQL Database-Servicelagen](sql-database-performance-guidance.md)
