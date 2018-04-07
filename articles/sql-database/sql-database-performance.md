---
title: Bewaken en verbeterde prestaties - Azure SQL Database | Microsoft Docs
description: De Azure SQL Database biedt prestatiehulpprogramma's om te identificeren gebieden die u kunnen de huidige queryprestaties verbeteren.
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: c97aa3aba9b705eeb0e3f40f289af389e6f380b0
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="monitor-and-improve-performance"></a>Prestaties bewaken en verbeteren
Azure SQL Database identificeert potentiële problemen in uw database en raadt aan om de acties die de prestaties van uw werkbelasting verbeteren kunnen door intelligent afstemmen acties en aanbevelingen.

Als u wilt controleren van de databaseprestaties van uw, gebruikt u de **prestaties** tegel op de pagina overzicht of navigeer naar 'Ondersteuning + probleemoplossing' sectie:

   ![Weergaveprestaties](./media/sql-database-performance/entries.png)

In de 'Ondersteuning + probleemoplossing' sectie, kunt u de volgende pagina's:


1. [Prestatieoverzicht van de](#performance-overview) voor het bewaken van de prestaties van uw database. 
2. [Prestaties aanbevelingen](#performance-recommendations) prestaties aanbevelingen waarmee de prestaties van uw workload kunnen vinden.
3. [Query Performance Insight](#query-performance-insight) bovenste resource verbruikt query's te vinden.
4. [Automatische afstemming](#automatic-tuning) te laten Azure SQL Database de database automatisch te optimaliseren.

## <a name="performance-overview"></a>Prestatieoverzicht van de
Deze weergave bevat een samenvatting van de databaseprestaties van uw en helpt u bij de prestaties afstemmen en probleemoplossing. 

![Prestaties](./media/sql-database-performance/performance.png)

* De **aanbevelingen** tegel bevat een verdeling van aanbevelingen voor uw database afstemmen (drie belangrijkste aanbevelingen worden weergegeven als er meer zijn). Op deze tegel klikt, gaat u naar  **[prestaties aanbevelingen](#performance-recommendations)**. 
* De **afstemmen activiteit** tegel bevat een samenvatting van de lopende en voltooide acties voor uw database afstemmen, zodat u snel inzicht in de geschiedenis van het afstemmen van de activiteit. Deze tegel te klikken gaat u naar de volledige geschiedenisweergave afstemmen voor uw database.
* De **automatische afstemming** tegel toont de [automatische afstemming configuratie](sql-database-automatic-tuning-enable.md) voor uw database (tuning opties die automatisch worden toegepast op uw database). Op deze tegel klikt, opent het dialoogvenster van de configuratie van automation.
* De **databasequery's** tegel ziet u de samenvatting van de prestaties van query's voor uw database (algehele DTU-gebruik en de bovenkant resource verbruikt query's). Op deze tegel klikt, gaat u naar  **[Query Performance Insight](#query-performance-insight)**.

## <a name="performance-recommendations"></a>Prestatieaanbevelingen
Deze pagina bevat intelligent [afstemmen aanbevelingen](sql-database-advisor.md) die de prestaties van uw database kunt verbeteren. De volgende soorten aanbevelingen worden weergegeven op deze pagina:

* Aanbevelingen over welke indexen te maken of verwijderen.
* Aanbevelingen wanneer schema problemen zijn geïdentificeerd in de database.
* Aanbevelingen wanneer query's van query's profiteren kunnen.

![Prestaties](./media/sql-database-performance/recommendations.png)

U kunt ook volledige geschiedenis van het afstemmen van de acties die zijn toegepast in het verleden vinden.

Meer informatie over het vinden van een apply prestaties aanbevelingen in [zoeken en toepassen van aanbevelingen voor prestaties](sql-database-advisor-portal.md) artikel.

## <a name="automatic-tuning"></a>Automatisch instellen
Azure SQL-Databases kunt automatisch afstemmen van de databaseprestaties door toe te passen [prestaties aanbevelingen](sql-database-advisor.md). Voor meer informatie lezen [automatische afstemmen artikel](sql-database-automatic-tuning.md). Als u wilt inschakelen, lezen [het inschakelen van automatische afstemming](sql-database-automatic-tuning-enable.md).

## <a name="query-performance-insight"></a>Inzicht in queryprestaties
[Query Performance Insight](sql-database-query-performance.md) kunt u minder tijd databaseprestaties oplossen door op te geven:

* Dieper inzicht in uw databases brongebruik (DTU). 
* De bovenste CPU verbruikt query's mogelijk voor verbeterde prestaties kunnen worden afgestemd. 
* De mogelijkheid om Inzoomen op de details van een query. 

  ![Prestatiedashboard](./media/sql-database-query-performance/performance.png)

Meer informatie over deze pagina niet vinden in het artikel  **[het gebruik van de Query Performance Insight](sql-database-query-performance.md)**.

## <a name="additional-resources"></a>Aanvullende resources
* [Azure SQL Database-prestaties richtlijnen voor individuele databases](sql-database-performance-guidance.md)
* [Wanneer een elastische groep moet worden gebruikt?](sql-database-elastic-pool-guidance.md)

