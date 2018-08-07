---
title: Prestaties - Azure SQL Database bewaken en verbeteren | Microsoft Docs
description: De Azure SQL Database biedt prestaties hulpprogramma's waarmee u gebieden geïdentificeerd die u kunnen de huidige queryprestaties verbeteren.
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: 8a458af27bd517be7c3ce0b5ad30c6d575d494c0
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522438"
---
# <a name="monitor-and-improve-performance"></a>Prestaties bewaken en verbeteren
Azure SQL Database geeft mogelijke problemen in uw database en wordt aanbevolen acties die prestaties van uw workload verbeteren kunnen door intelligente acties en aanbevelingen te geven.

Als u wilt controleren van de databaseprestaties van uw, gebruikt u de **prestaties** tegel op de pagina overzicht en navigeer naar "Ondersteuning en probleemoplossing" sectie:

   ![Prestaties weergeven](./media/sql-database-performance/entries.png)

In de "Ondersteuning en probleemoplossing" sectie, kunt u de volgende pagina's:


1. [Prestatieoverzicht](#performance-overview) voor het bewaken van prestaties van uw database. 
2. [Aanbevelingen voor prestaties](#performance-recommendations) vinden aanbevelingen voor prestaties die prestaties van uw workload kunnen verbeteren.
3. [Query Performance Insight](#query-performance-insight) bovenste die veel resources verbruiken van query's te vinden.
4. [Automatisch afstemmen](#automatic-tuning) zodat Azure SQL Database de database automatisch te optimaliseren.

## <a name="performance-overview"></a>Prestatieoverzicht
In deze weergave bevat een samenvatting van de databaseprestaties van uw, en helpt u bij het afstemmen van prestaties en oplossen van problemen. 

![Prestaties](./media/sql-database-performance/performance.png)

* De **aanbevelingen** tegel bevat een verdeling van de aanbevelingen voor uw database voor het afstemmen (drie belangrijkste aanbevelingen worden weergegeven of er meer zijn). Op deze tegel klikt, gaat u naar  **[aanbevelingen voor prestaties](#performance-recommendations)**. 
* De **Afstemmingsactiviteit** tegel bevat een samenvatting van de actieve en voltooide acties voor uw database afstemmen, zodat u snel inzicht krijgt in de geschiedenis van het afstemmen van de activiteit. Op deze tegel klikt, wordt u gaat naar de volledige geschiedenis-weergave afstemmen voor uw database.
* De **automatisch afstemmen** tegel toont de [configuratie voor automatisch afstemmen](sql-database-automatic-tuning-enable.md) voor uw database (de opties die automatisch worden toegepast op uw database afstemmen). Op deze tegel klikt, opent u het dialoogvenster van de configuratie van automation.
* De **databasequery's** tegel staat een overzicht van de prestaties van query's voor uw database (totale DTU-gebruik en boven die veel resources verbruiken query's). Op deze tegel klikt, gaat u naar  **[Query Performance Insight](#query-performance-insight)**.

## <a name="performance-recommendations"></a>Aanbevelingen voor prestaties
Deze pagina vindt u intelligente [aanbevelingen voor het afstemmen](sql-database-advisor.md) die prestaties van uw database kunt verbeteren. De volgende typen aanbevelingen worden weergegeven op deze pagina:

* Aanbevelingen over welke indexen te maken of te verwijderen.
* Aanbevelingen als schema problemen worden geïdentificeerd in de database.
* Aanbevelingen wanneer query's van geparameteriseerde query's profiteren kunnen.

![Prestaties](./media/sql-database-performance/recommendations.png)

U vindt hier ook volledige geschiedenis van het afstemmen van de acties die zijn toegepast in het verleden.

Meer informatie over het vinden van een item voor toepassen aanbevelingen voor prestaties in [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md) artikel.

## <a name="automatic-tuning"></a>Automatisch instellen
Azure SQL-Databases kan automatisch afstemmen van prestaties van de database door toe te passen [aanbevelingen voor prestaties](sql-database-advisor.md). Voor meer informatie lezen [automatisch afstemmen artikel](sql-database-automatic-tuning.md). Als u wilt inschakelen, lezen [het inschakelen van automatisch afstemmen](sql-database-automatic-tuning-enable.md).

## <a name="query-performance-insight"></a>Inzicht in queryprestaties
[Query Performance Insight](sql-database-query-performance.md) kunt u minder tijd door het oplossen van prestaties van de database door te geven:

* Beter inzicht in het gebruik van de resources (DTU) databases. 
* De hoogste CPU-verbruik van query's die mogelijk kunnen worden afgestemd voor betere prestaties. 
* De mogelijkheid om in te zoomen op de details van een query. 

  ![van Prestatiedashboard](./media/sql-database-query-performance/performance.png)

Meer informatie over deze pagina vinden in het artikel  **[Query Performance Insight gebruiken](sql-database-query-performance.md)**.

## <a name="additional-resources"></a>Aanvullende resources
* [Richtlijnen voor prestaties van Azure SQL-Database voor individuele databases](sql-database-performance-guidance.md)
* [Wanneer moet een elastische pool worden gebruikt?](sql-database-elastic-pool-guidance.md)

