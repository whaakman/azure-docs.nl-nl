---
title: Bewaking en prestatieafstemming - Azure SQL Database | Microsoft Docs
description: Tips voor het afstemmen in Azure SQL Database via evaluatie en verbetering van de prestaties.
services: sql-database
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
keywords: prestaties van SQL databaseprestaties afstemmen, tips voor het afstemmen van de sql prestaties afstemmen prestatieafstemming voor sql-database
ms.assetid: eb7b3f66-3b33-4e1b-84fb-424a928a6672
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: v-shysun
ms.openlocfilehash: 1791c56f86ee1997177daa95638c4f14068f8115
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="monitoring-and-performance-tuning"></a>Bewaking en prestaties afstemmen

Azure SQL-Database wordt automatisch beheerd en flexibele gegevensservice waarin u kunt eenvoudig gebruik controleren, toevoegen of verwijderen resources (CPU, geheugen, i/o) aanbevelingen die kunnen verbeteren de prestaties van de database of database aanpassen aan uw werkbelasting laten vinden en automatisch optimaliseren.

In dit artikel biedt een overzicht van controle en prestaties afstemmen van de opties die beschikbaar in Azure SQL Database zijn.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>Bewaking en probleemoplossing van de prestaties van de database

Azure SQL Database kunt u gemakkelijk het Databasegebruik van uw bewaken en identificeren van query's die prestatieproblemen kunnen veroorzaken. U kunt met behulp van Azure portal of het systeem weergaven databaseprestaties bewaken. U hebt de volgende opties voor bewaking en probleemoplossing van de prestaties van de database:

1. In de [Azure-portal](https://portal.azure.com), klikt u op **SQL-databases**, selecteert u de database, en vervolgens de grafiek bewaking op zoek naar bronnen hun maximale nadert. DTU-verbruik wordt standaard weergegeven. Klik op **bewerken** wijzigen van het tijdsbereik en waarden die worden weergegeven.
2. Gebruik [Query Performance Insight](sql-database-query-performance.md) voor het identificeren van de query's die het meest van resources hoeven te besteden aan.
3. U kunt dynamische beheerweergaven (DMV's), Extended Events (`XEvents`), en de queryopslag in SSMS prestatieparameters in realtime ophalen.

Zie de [prestaties richtlijnen onderwerp](sql-database-performance-guidance.md) technieken waarmee u de prestaties van Azure SQL Database verbeteren kunt als u een probleem met deze rapporten of weergaven identificeren vinden.

> [!IMPORTANT] 
> Het wordt aanbevolen om altijd de nieuwste versie van Management Studio te gebruiken, zodat uw versie gesynchroniseerd blijft met updates voor Microsoft Azure en SQL Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-to-improve-performance"></a>Databases voor betere prestaties optimaliseren

Azure SQL Database kunt u Identificeer mogelijkheden te verbeteren en prestaties van query's optimaliseren zonder resources aan de hand van [prestaties afstemmen aanbevelingen](sql-database-advisor.md). Ontbrekende indexen en onvoldoende geoptimaliseerde query's zijn veelvoorkomende redenen voor slechte databaseprestaties. U kunt deze afstemmen aanbevelingen voor het verbeteren van de prestaties van uw workload toepassen.
U kunt ook laten Azure SQL database [automatisch de prestaties van uw query's optimaliseren](sql-database-automatic-tuning.md) door toe te passen alle aanbevelingen en verifiëren van de verbetering van prestaties van de database hebt geïdentificeerd. U kunt de volgende opties gebruiken om de prestaties van uw database te verbeteren:

1. Gebruik [SQL Database Advisor](sql-database-advisor-portal.md) om aanbevelingen voor het maken en verwijderen van indexen, parameters voorzien van query's en het oplossen van problemen met schema weer te geven.
2. [Schakel automatische afstemming](sql-database-automatic-tuning-enable.md) en laat Azure SQL database automatisch prestatieproblemen fix geïdentificeerd.

## <a name="improving-database-performance-with-more-resources"></a>Verbeterde databaseprestaties met meer bronnen

Ten slotte, als er geen bruikbare items die u de prestaties van uw database verbeteren zijn kunnen, kunt u de hoeveelheid beschikbare resources in Azure SQL Database. U kunt meer bronnen toewijzen door het wijzigen van de [servicelaag](sql-database-service-tiers.md) van een zelfstandige database of vergroot de edtu's van een elastische pool op elk gewenst moment.
1. Voor zelfstandige databases, kunt u [Servicelagen wijzigen](sql-database-service-tiers.md) op aanvraag voor het verbeteren van de prestaties van de database.
2. Voor meerdere databases kunt u overwegen [elastische pools](sql-database-elastic-pool-guidance.md) automatisch schalen.

## <a name="tune-and-refactor-application-or-database-code"></a>Afstemmen en verander toepassing of databasecode

U kunt de toepassingscode meer optimaal gebruik van de database, wijzigen van indexen, forceren plannen of hints gebruiken om aan te passen handmatig de database naar de werkbelasting van uw wijzigen. Vinden van sommige richtlijnen en tips voor handmatige afstemmen en herschrijven van de code in de [prestaties richtlijnen onderwerp](sql-database-performance-guidance.md) artikel.


## <a name="next-steps"></a>Volgende stappen

- Als u wilt inschakelen automatische afstemming in Azure SQL Database en laat functie voor automatisch afstemmen uw werkbelasting volledig te beheren, Zie [inschakelen automatische afstemming](sql-database-automatic-tuning-enable.md).
- Als u wilt gebruiken voor handmatige afstemmen, kunt u bekijken [afstemmen aanbevelingen in Azure-portal](sql-database-advisor-portal.md) en de waarden die de prestaties van uw query's verbeteren handmatig toepassen.
- Bronnen die beschikbaar in de database door te wijzigen zijn wijzigen [Azure SQL Database Servicelagen](sql-database-performance-guidance.md)
