---
title: Query Performance Insight voor Azure SQL Database | Microsoft Docs
description: Query-prestaties controleren, identificeert de meeste CPU verbruikt query's voor een Azure SQL-database.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 5d892005881436dec89c0d0d010f7f02e7bdebf9
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54039194"
---
# <a name="query-performance-insight-for-azure-sql-database"></a>Query Performance Insight voor Azure SQL-Database

Beheer en de afstelling van de prestaties van relationele databases duurt expertise en -tijd. Query Performance Insight is een onderdeel van de Azure SQL Database intelligente prestaties-productlijn. Dit helpt u besteed minder tijd die het oplossen van prestaties van de database door te geven aan:

* Beter inzicht in het gebruik van de resources (DTU) databases.
* Als u meer informatie over de belangrijkste databasequery's op telling van CPU, de duur en de uitvoering (potentiële kandidaten voor verbeterde prestaties afstemmen).
* De mogelijkheid om in te zoomen op details van een query, om de tekst van deze query en de geschiedenis van Resourcegebruik weer te geven.
* Aantekeningen waarin de aanbevelingen voor prestaties van [SQL Database Advisor](sql-database-advisor.md).

![Inzicht in queryprestaties](./media/sql-database-query-performance/opening-title.png)

> [!TIP]
> Voor eenvoudige prestaties controleren met Azure SQL Database, raden wij Query Performance Insight. Houd er rekening mee de productbeperkingen in dit artikel is gepubliceerd. Voor geavanceerde controle van de prestaties van de database op schaal, wordt aangeraden [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md). Het beschikt over ingebouwde intelligentie voor het oplossen van prestaties automatisch. Voor sommige van de meest voorkomende prestatieproblemen met de database automatisch afstemmen wordt aangeraden [automatisch afstemmen](sql-database-automatic-tuning.md).

## <a name="prerequisites"></a>Vereisten

Query Performance Insight vereist dat [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) actief is op uw database. Het automatisch ingeschakeld voor alle Azure SQL-databases standaard. Als Query Store wordt niet uitgevoerd, wordt u zodat het door de Azure-portal gevraagd.

> [!NOTE]
> Als het bericht 'Query Store is niet juist geconfigureerd op deze database' wordt weergegeven in de portal, Zie [optimaliseren van de configuratie van de Query Store](#optimize-the-query-store-configuration-for-query-performance-insight).
>

## <a name="permissions"></a>Machtigingen

U hebt het volgende nodig [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) machtigingen Query Performance Insight gebruiken:

* **Lezer**, **eigenaar**, **Inzender**, **' SQL DB Contributor '**, of **Inzender voor SQL Server** machtigingen zijn vereist de top-resource-intensieve query's en grafieken bekijken.
* **De eigenaar van**, **Inzender**, **' SQL DB Contributor '**, of **Inzender voor SQL Server** machtigingen zijn vereist om querytekst weer te geven.

## <a name="use-query-performance-insight"></a>Query Performance Insight gebruiken

Query Performance Insight is eenvoudig te gebruiken:

1. Open de [Azure-portal](https://portal.azure.com/) en zoeken van een database die u wilt onderzoeken.
2. Open in het menu aan de linkerkant **intelligente prestaties** > **Query Performance Insight**.
  
   ![Query Performance Insight in het menu](./media/sql-database-query-performance/tile.png)

3. Bekijk de lijst met de top-resource-intensieve query's op het eerste tabblad.
4. Selecteer een afzonderlijke query's om de details ervan weer te geven.
5. Open **intelligente prestaties** > **aanbevelingen voor prestaties** en controleer of alle aanbevelingen voor prestaties beschikbaar zijn. Zie voor meer informatie over ingebouwde prestatieaanbevelingen [SQL Database Advisor](sql-database-advisor.md).
6. Schuifregelaars gebruiken of uitzoomen pictogrammen om de waargenomen interval te wijzigen.

   ![van Prestatiedashboard](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Voor SQL-Database om de informatie in de Query Performance Insight weer te geven, moet de Query Store om vast te leggen van een paar uur aan gegevens. Als de database geen activiteit heeft of als de Query Store is niet actief geweest gedurende een bepaalde periode, wordt de grafieken niet leeg zijn wanneer Query Performance Insight wordt weergegeven dat tijdsbereik. Als deze niet wordt uitgevoerd, kunt u de Query Store inschakelen op elk gewenst moment. Zie voor meer informatie, [aanbevolen procedures met Query Store](https://docs.microsoft.com/sql/relational-databases/performance/best-practice-with-the-query-store).

## <a name="review-top-cpu-consuming-queries"></a>Bovenste CPU verbruikt query's controleren

Query Performance Insight laat de top vijf CPU verbruikt-query's standaard wanneer u het eerst opent.

1. Schakelt u afzonderlijke query's op te nemen of ze uitsluiten van de grafiek met behulp van de selectievakjes in of uit.

    De bovenste regel bevat algemene DTU-percentage voor de database. CPU-percentage dat de geselecteerde query's die worden gebruikt tijdens de geselecteerde interval, geven de balken. Bijvoorbeeld, als **afgelopen week** is geselecteerd, elke balk vertegenwoordigt een enkele dag.

    ![Top-query 's](./media/sql-database-query-performance/top-queries.png)

   > [!IMPORTANT]
   > De DTU-regel weergegeven wordt geaggregeerd op een waarde voor maximale verbruik in perioden van één uur. Het bedoeld voor een vergelijking op hoog niveau alleen met statistieken voor query-uitvoering. In sommige gevallen DTU-gebruik lijkt het alsof te hoog ten opzichte van query's uitgevoerd, maar dit wordt mogelijk niet het geval is.
   >
   > Als een query maximum is overschreden DTU tot 100% slechts een paar minuten, wordt de regel DTU in Query Performance Insight bijvoorbeeld weergegeven in het hele uur van het verbruik als 100% (het gevolg is van de maximale geaggregeerde waarde).
   >
   > Voor een fijnere vergelijking (maximaal één minuut), houd rekening met het maken van een aangepaste grafiek met DTU-gebruik:
   >
   > 1. Selecteer in de Azure portal, **Azure SQL Database** > **bewaking**.
   > 2. Selecteer **Metrische gegevens**.
   > 3. Selecteer **+ toevoegen grafiek**.
   > 4. Selecteer de DTU-percentage van de grafiek.
   > 5. Selecteer daarnaast **afgelopen 24 uur** in het menu linksboven en wijzig deze in één minuut.
   >
   > Gebruik de aangepaste DTU-grafiek met een gedetailleerder niveau naar details om te vergelijken met het diagram van de uitvoering van query.

   Het raster onder toont verzamelde informatie voor de weergegeven query's:

   * Query-ID die een unieke id voor de query in de database is.
   * CPU per query tijdens een interval van waarneembare, afhankelijk van de statistische functie.
   * Duur per query, die ook afhankelijk van de statistische functie.
   * Totaal aantal uitvoeringen voor een specifieke query.

2. Als uw gegevens verlopen is, selecteert u de **vernieuwen** knop.

3. Schuifregelaars gebruiken en zoomknoppen observatie van het interval wijzigen en verbruik pieken onderzoeken:

   ![Schuifregelaars en inzoomen op knoppen voor het wijzigen van het interval](./media/sql-database-query-performance/zoom.png)

4. Desgewenst kunt u de **aangepaste** tabblad om aan te passen van de weergave voor:

   * Metrische gegevens (CPU, duur, aantal uitvoeringen).
   * Tijdsinterval (afgelopen 24 uur, afgelopen week of maand).
   * Aantal query's.
   * Statistische functie.
  
   ![Aangepast tabblad](./media/sql-database-query-performance/custom-tab.png)
  
5. Selecteer de **gaat >** knop om een aangepaste weergave.

   > [!IMPORTANT]
   > Query Performance Insight is beperkt tot het weergeven van de top 5-20 in beslag nemen query's, afhankelijk van uw selectie. Uw database kunt uitvoeren naast de top weergegeven die een veel meer query's en deze query's worden niet opgenomen in de grafiek.
   >
   > Er bestaat mogelijk een type database werkbelasting waarin veel kleinere query's, naast de top die wordt weergegeven, vaak worden uitgevoerd en het merendeel van de DTU gebruiken. Deze query's worden niet weergegeven in de Prestatiegrafiek.
   >
   > Bijvoorbeeld, een query mogelijk heeft verbruikt een aanzienlijke hoeveelheid DTU even, hoewel de totaal verbruik van de geobserveerde periode kleiner dan de andere boven verbruikt query's is. In dat geval weergegeven Resourcegebruik van deze query niet in de grafiek.
   >
   > Als u nodig hebt om te begrijpen van de belangrijkste query uitvoeringen de beperkingen van de Query Performance Insight, kunt u overwegen [Azure SQL Analytics](../azure-monitor/insights/azure-sql.md) voor geavanceerde databaseprestaties bewaken en problemen oplossen.
   >

## <a name="view-individual-query-details"></a>Details van afzonderlijke query's weergeven

Om Querydetails te bekijken:

1. Selecteer een query in de lijst van de populairste query's.

    ![Lijst van de populairste query 's](./media/sql-database-query-performance/details.png)

   Er wordt een gedetailleerde weergave geopend. Het bevat de CPU-verbruik, duur en aantal uitvoeringen na verloop van tijd.

2. Selecteer de grafiekfuncties voor meer informatie.

   * De bovenste grafiek bevat een regel met de totale DTU-percentage-database. De balken zijn het CPU-percentage die de geselecteerde query gebruikt.
   * Het tweede diagram toont de totale duur van de geselecteerde query.
   * De onderste grafiek geeft het totale aantal uitvoeringen door de geselecteerde query.

   ![Querydetails](./media/sql-database-query-performance/query-details.png)

3. (Optioneel) schuifregelaars gebruiken, gebruik de knoppen Inzoomen op of selecteer **instellingen** aanpassen hoe querygegevens worden weergegeven, of kies een ander tijdsbereik.

   > [!IMPORTANT]
   > Query Performance Insight wordt niet vastgelegd voor alle DDL-query's. In sommige gevallen kan deze niet alle ad-hocquery's worden vastgelegd.
   >

## <a name="review-top-queries-per-duration"></a>Controleer de populairste query's per duur

Twee metrische gegevens in Query Performance Insight kunt u mogelijke knelpunten vinden: aantal duur en kan worden uitgevoerd.

Langlopende query's hebben de grootste kans op meer resources vergrendelen, andere gebruikers blokkeren en schaalbaarheid te beperken. Ze zijn ook de beste kandidaten voor optimalisatie.

Langlopende query's identificeren:

1. Open de **aangepaste** tabblad in de Query Performance Insight voor de geselecteerde database.
2. Wijzigen van de metrische **duur**.
3. Selecteer het aantal query's en het interval waarneming.
4. Selecteer de statistische functie:

   * **Som** is de som van alle uitvoeringstijd van de query voor het hele waarneming-interval.
   * **Maximale** vindt u query's in welke uitvoeringstijd is maximum voor het hele waarneming-interval.
   * **Gem.** gevonden met de gemiddelde uitvoeringstijd van alle uitvoeringen van de query en toont u de belangrijkste die voor deze gemiddelden.

   ![Queryduur](./media/sql-database-query-performance/top-duration.png)

5. Selecteer de **gaat >** knop om een aangepaste weergave.

   > [!IMPORTANT]
   > De queryweergave aanpassen, wordt de DTU-regel niet bijgewerkt. De DTU-regel wordt altijd de maximale verbruik-waarde voor het interval.
   >
   > Voor meer informatie over database DTU-verbruik meer details (maximaal één minuut), houd rekening met het maken van een aangepaste grafiek in de Azure-portal:
   >
   > 1. Selecteer **Azure SQL Database** > **bewaking**.
   > 2. Selecteer **Metrische gegevens**.
   > 3. Selecteer **+ toevoegen grafiek**.
   > 4. Selecteer de DTU-percentage van de grafiek.
   > 5. Selecteer daarnaast **afgelopen 24 uur** in het menu linksboven en wijzig deze in één minuut.
   >
   > U wordt aangeraden dat u de aangepaste DTU-grafiek gebruiken om te vergelijken met het diagram van de query-prestaties.
   >

## <a name="review-top-queries-per-execution-count"></a>Bekijk de populairste query's per aantal uitvoeringen

Een Gebruikerstoepassing die gebruikmaakt van de database get vertragen, zelfs als een groot aantal uitvoeringen kan niet worden die betrekking hebben op de database zelf en verbruik van resources laag is.

In sommige gevallen kan een aantal hoge uitvoeringen leiden tot meer netwerkrondes. Retouren invloed hebben op prestaties. Ze zijn afhankelijk van de netwerklatentie en latentie voor downstream-server.

Bijvoorbeeld: veel gegevens gebaseerde websites sterk toegang krijgen tot de database voor elke gebruikersaanvraag. Hoewel Groepsgewijze verbinding helpt, het toegenomen verkeer en de verwerkingsbelasting op de database-server kunnen de prestaties negatief beïnvloeden. Houd ' round trips ' in het algemeen tot een minimum beperkt.

Voor het identificeren van vaak uitgevoerd ("intensieve") query's:

1. Open de **aangepaste** tabblad in de Query Performance Insight voor de geselecteerde database.
2. Wijzigen van de metrische **aantal uitvoeringen**.
3. Selecteer het aantal query's en het interval waarneming.
4. Selecteer de **gaat >** knop om een aangepaste weergave.

   ![uitvoering van het aantal query 's](./media/sql-database-query-performance/top-execution.png)

## <a name="understand-performance-tuning-annotations"></a>Inzicht in prestaties afstemmen aantekeningen

Tijdens het verkennen van uw workload in Query Performance Insight, ziet u mogelijk pictogrammen met een verticale lijn boven op de grafiek.

Deze pictogrammen zijn aantekeningen. Ze geven aanbevelingen voor prestaties van [SQL Database Advisor](sql-database-advisor.md). Door de muiswijzer op van een aantekening, krijgt u samenvattende informatie over aanbevelingen voor prestaties.

   ![query-aantekening](./media/sql-database-query-performance/annotation.png)

Als u wilt meer inzicht in of van de advisor-aanbeveling toepast, selecteer het pictogram details van de aanbevolen actie openen. Als dit een actieve aanbeveling, kunt u deze meteen toepassen vanuit de portal.

   ![Querydetails voor de aantekening](./media/sql-database-query-performance/annotation-details.png)

In sommige gevallen, vanwege het zoomniveau is het mogelijk dat aantekeningen dicht bij elkaar zijn samengevoegd tot een enkel aantekening. Query Performance Insight wordt dit weergegeven als een pictogram van de aantekening groep. Selecteer het pictogram van de aantekening groep, wordt een nieuwe blade met een lijst met de aantekeningen geopend.

Correlerende query's en -prestaties optimaliseren acties u kunnen helpen bij uw workload beter te begrijpen.

## <a name="optimize-the-query-store-configuration-for-query-performance-insight"></a>De Query Store-configuratie voor Query Performance Insight optimaliseren

Tijdens het gebruik van Query Performance Insight, ziet u mogelijk de volgende Query Store-foutberichten:

* "Query Store is niet juist geconfigureerd op deze database. Klik hier voor meer informatie."
* "Query Store is niet juist geconfigureerd op deze database. Klik hier om instellingen te wijzigen."

Deze berichten worden gewoonlijk weergegeven wanneer Query Store kan geen nieuwe gegevens verzamelen.

Het eerste geval gebeurt er wanneer Query Store in de status alleen-lezen is en parameters optimaal zijn ingesteld. U kunt dit oplossen door het vergroten van het gegevensarchief of door de Query Store uit te schakelen. (Als u Query Store uitschakelt, alle eerder verzamelde telemetrie, gaan verloren.)

   ![Details van de query Store](./media/sql-database-query-performance/qds-off.png)

Het tweede geval treedt op wanneer de Query Store niet is ingeschakeld, of parameters niet optimaal zijn ingesteld. U kunt de retentie en vastleggen beleid wijzigen en Query Store, ook inschakelen door het uitvoeren van de volgende opdrachten uit [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) of de Azure-portal.

### <a name="recommended-retention-and-capture-policy"></a>Aanbevolen beleid voor retentie en vastleggen

Er zijn twee soorten beleid voor het bewaren:

* **Op basis van grootte**: Als dit beleid is ingesteld op **automatisch**, deze wordt opgeschoond gegevens automatisch wanneer bijna de maximale grootte is bereikt.
* **Op basis van de**: Dit beleid is standaard ingesteld op 30 dagen. Als Query Store onvoldoende ruimte, worden deze querygegevens ouder dan 30 dagen verwijderd.

U kunt de capture-beleid instellen op:

* **Alle**: Query Store bevat alle query's.
* **Automatisch**: Query Store negeert incidentele query's en query's met een duur van onbelangrijke compileren en uitvoeren. Drempelwaarden voor het aantal uitvoeringen, Compileer duur en de duur van runtime intern worden bepaald. Dit is de standaardoptie.
* **Geen**: Query Store stopt u het vastleggen van nieuwe query's, maar de runtime-statistieken voor al vastgelegde query's worden nog steeds verzameld.

Het is raadzaam alle beleidsregels instellen op **automatisch** en opschonen van het beleid tot 30 dagen door het uitvoeren van de volgende opdrachten uit [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) of de Azure-portal. (Vervang `YourDB` met de naam van de database.)

```sql
    ALTER DATABASE [YourDB]
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);
```

Verhoog de grootte van de Query Store door verbinding te maken met een database via [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) of de Azure-portal en de volgende query uit te voeren. (Vervang `YourDB` met de naam van de database.)

```T-SQL
    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);
```

Deze instellingen worden toegepast, wordt uiteindelijk Query Store verzamelen van telemetrie voor nieuwe query's maken. Als u Query Store worden meteen operationele nodig hebt, kunt u eventueel Query Store wissen door te voeren van de volgende query via SSMS of de Azure-portal. (Vervang `YourDB` met de naam van de database.)

> [!NOTE]
> De volgende query uit te voeren, worden alle eerder verzamelde bewaakte telemetrie in Query Store verwijderd.

```SQL
    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;
```

## <a name="summary"></a>Samenvatting

Query Performance Insight kunt u het effect van de werkbelasting van query's en hoe deze zich verhoudt tot het verbruik van databaseresources begrijpen. Met deze functie vindt u meer over de query top verbruikt op uw database en vindt u query's te optimaliseren voordat ze een probleem.

## <a name="next-steps"></a>Volgende stappen

* Selecteer voor database-aanbevelingen voor prestaties, [aanbevelingen](sql-database-advisor.md) op de blade van de navigatie Query Performance Insight.

    ![Het tabblad aanbevelingen](./media/sql-database-query-performance/ia.png)

* Overweeg in te schakelen [automatisch afstemmen](sql-database-automatic-tuning.md) voor veelvoorkomende problemen met prestaties de database.
* Informatie over hoe [Intelligent Insights](sql-database-intelligent-insights.md) kunt u automatisch oplossen van problemen met prestaties van de database.
* Overweeg het gebruik van [Azure SQL Analytics]( ../azure-monitor/insights/azure-sql.md) voor geavanceerde prestatiebewaking van een grote reeks SQL-databases, elastische pools en beheerde instanties met ingebouwde intelligentie.
