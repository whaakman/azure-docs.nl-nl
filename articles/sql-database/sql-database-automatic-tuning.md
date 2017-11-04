---
title: Azure SQL Database - automatische afstemming | Microsoft Docs
description: Azure SQL Database SQL-query analyseert en automatisch wordt aangepast aan de werkbelasting van de gebruiker.
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: On Demand
ms.date: 09/19/2017
ms.author: jovanpop
ms.openlocfilehash: 10f8cca8e519b28f0fe29605419b860f73e04a87
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="automatic-tuning-in-azure-sql-database"></a>Automatische afstemming in Azure SQL Database

Azure SQL-Database is een volledig beheerde gegevensservice waarin de query's die worden uitgevoerd op de database en automatisch bewaakt verbetert de prestaties van de werkbelasting van de database. Azure SQL Database beschikt over een ingebouwde [automatische afstemming](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) intelligence mechanisme waarmee automatisch kunt afstemmen en prestaties van uw query's verbeteren door dynamisch aanpassing van de database naar uw workload. Automatische afstemming in Azure SQL Database is mogelijk een van de belangrijkste functies dat u in Azure SQL-Database inschakelen kunt om de prestaties van uw query's te optimaliseren.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="automatic-tuning-options"></a>Opties voor automatische afstemmen

[Automatische afstemming](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) in Azure SQL Database beschikbare opties zijn:
 1. **CREATE INDEX** die identificeert de indexen die de prestaties van uw werkbelasting mogelijk verbeterd, maakt de indexen en controleert of dat ze de prestaties van de query's verbeteren.
 2. **DROP INDEX** die redundante en dubbele indexen en indexen die niet werden gebruikt in de lange periode identificeert.
 3. **PLAN REGRESSIE correctie** SQL-query's die gebruikmaken van uitvoeringsplan die lager is dan de vorige goed plan identificeert en maakt gebruik van de laatste bekende goede planning in plaats van het regressed plan.

Azure SQL Database identificeert **CREATE INDEX**, **DROP INDEX**, en **plannen REGRESSIE correctie** aanbevelingen die u kunnen de database te optimaliseren en wordt deze weergegeven in Azure Portal. Meer informatie over de identificatie van de indexen die moeten worden gewijzigd op [indexaanbevelingen vinden in Azure portal](sql-database-advisor-portal.md). Met behulp van de portal aanbevelingen handmatig toepassen of Azure SQL Database voor het automatisch toepassen van aanbevelingen, werkbelasting bewaken na herstart van de wijziging laten en verifiëren dat de aanbeveling de prestaties van uw werkbelasting verbeterd.

Automatische afstemming van opties kan worden afzonderlijk ingeschakeld of uitgeschakeld per database, of ze kunnen worden geconfigureerd op de logische server en toegepast op elke database die u neemt instellingen over van de server. Configureren van [automatische afstemming](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) opties op de server en de overgenomen instellingen van de databases op de server wordt aanbevolen methode voor het configureren van automatische afstemming omdat dit vereenvoudigt het beheer van de opties voor automatische afstemmen op een groot aantal databases.

Raadpleeg dit artikel voor de stappen voor het [inschakelen automatische afstemming](sql-database-automatic-tuning-enable.md) met de Azure portal.

## <a name="next-steps"></a>Volgende stappen

- Als u wilt inschakelen automatische afstemming in Azure SQL Database en laat functie voor automatisch afstemmen uw werkbelasting volledig te beheren, Zie [inschakelen automatische afstemming](sql-database-automatic-tuning-enable.md).
- Als u wilt gebruiken voor handmatige afstemmen, kunt u bekijken [afstemmen aanbevelingen in Azure-portal](sql-database-advisor-portal.md) en de waarden die de prestaties van uw query's verbeteren handmatig toepassen.
- Meer informatie over ingebouwde intelligentie die bepaalde verbetert de prestatie de [Azure SQL Database](https://azure.microsoft.com/blog/artificial-intelligence-tunes-azure-sql-databases/).
- Lees meer over [automatische afstemming](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) in Azure SQL Database en SQL Server 2017.
