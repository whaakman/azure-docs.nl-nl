---
title: Prestatie-inzichten op te vragen voor Azure SQL Database | Microsoft Docs
description: Query-prestaties controleren, geeft u de meeste CPU verbruikt query's voor een Azure SQL Database.
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
ms.date: 04/01/2018
ms.openlocfilehash: f46d5a31667f52f553bcfdbd20ff56439d526144
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163304"
---
# <a name="azure-sql-database-query-performance-insight"></a>Azure SQL Database Query Performance Insight
Beheer en de afstelling van de prestaties van relationele databases is een lastige taak waarvoor aanzienlijke expertise en tijd investering. Query Performance Insight kunt u minder tijd door het oplossen van databaseprestaties biedt de volgende:

* Beter inzicht in het gebruik van de resources (DTU) databases. 
* De populairste query's op telling van CPU/duur/uitvoering, die mogelijk kunnen worden afgestemd voor betere prestaties.
* De mogelijkheid om in te zoomen op de details van een query, bekijk de tekst en de geschiedenis van Resourcegebruik. 
* Prestaties afstemmen aantekeningen die laten zien van acties die worden uitgevoerd door [SQL Azure Database Advisor](sql-database-advisor.md)  



## <a name="prerequisites"></a>Vereisten
* Query Performance Insight vereist dat [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) actief is op uw database. Als Query Store wordt niet uitgevoerd, wordt de portal vraagt u deze in te schakelen.

## <a name="permissions"></a>Machtigingen
De volgende [op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md) machtigingen zijn vereist voor Query Performance Insight gebruiken: 

* **Lezer**, **eigenaar**, **Inzender**, **' SQL DB Contributor '**, of **Inzender voor SQL Server** machtigingen zijn vereist om te bekijken in de bovenste die veel resources verbruiken query's en -grafieken. 
* **De eigenaar van**, **Inzender**, **' SQL DB Contributor '**, of **Inzender voor SQL Server** machtigingen zijn vereist om querytekst weer te geven.

## <a name="using-query-performance-insight"></a>Query Performance Insight gebruiken
Query Performance Insight is eenvoudig te gebruiken:

* Open [Azure-portal](https://portal.azure.com/) en zoeken naar database die u wilt onderzoeken. 
  * Selecteer in het menu van de linkerkant, onder ondersteuning en probleemoplossing van 'Query Performance Insight'.
* Bekijk de lijst met de top-resource-intensieve query's op het eerste tabblad.
* Selecteer een afzonderlijke query's om de details ervan weer te geven.
* Open [SQL Azure Database Advisor](sql-database-advisor.md) en controleer of er geen aanbevelingen beschikbaar zijn.
* Schuifregelaars gebruiken of uitzoomen van pictogrammen als waargenomen interval wilt wijzigen.
  
    ![van Prestatiedashboard](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Een paar uur van de gegevens moeten worden vastgelegd door de Query Store voor SQL Database query performance inzicht te bieden. Als de database geen activiteit heeft of Query Store is niet actief geweest gedurende een bepaalde periode, wordt de grafieken niet leeg zijn om deze periode weer te geven. U kunt Query Store op elk gewenst moment inschakelen als deze niet wordt uitgevoerd.   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a>Hoogste CPU-verbruik van query's controleren
In de [portal](http://portal.azure.com) het volgende doen:

1. Blader naar een SQL-database en klik op **alle instellingen** > **ondersteuning + probleemoplossing** > **Query performance insight**. 
   
    ![Inzicht in queryprestaties][1]
   
    Hiermee opent u de weergave van de populairste query's en de hoogste CPU in beslag nemen query's worden vermeld.
2. Klik op rond de grafiek voor meer informatie.<br>De bovenste regel bevat algemene DTU % voor de database, terwijl de balken die worden gebruikt door de geselecteerde query's tijdens de geselecteerde interval CPU-percentage weergeven (bijvoorbeeld, als **afgelopen week** is geselecteerd elke balk vertegenwoordigt één dag).
   
    ![Top-query 's][2]
   
    Het raster onder vertegenwoordigt verzamelde informatie voor de weergegeven query's.
   
   * Query-ID - de unieke id van de query in de database.
   * CPU per query tijdens waarneembare interval (afhankelijk van statistische functie).
   * Duur per query (afhankelijk van statistische functie).
   * Totaal aantal uitvoeringen voor een bepaalde query.
     
     Schakelt u afzonderlijke query's op te nemen of ze uitsluiten van de grafiek met behulp van de selectievakjes in of uit.
3. Als uw gegevens verlopen is, klikt u op de **vernieuwen** knop.
4. U kunt gebruiken schuifregelaars en knoppen observatie van het interval wijzigen en onderzoeken van pieken inzoomen en uitzoomen: ![instellingen](./media/sql-database-query-performance/zoom.png)
5. (Optioneel) als u een andere weergave wilt, kunt u **aangepaste** tabblad en ingesteld:
   
   * Metrische gegevens (CPU, duur, aantal uitvoeringen)
   * Tijdsinterval (afgelopen week afgelopen maand afgelopen 24 uur). 
   * Aantal query's.
   * Statistische functie.
     
     ![instellingen](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Details van afzonderlijke query's weergeven
Om Querydetails te bekijken:

1. Klik op een query in de lijst van de populairste query's.
   
    ![details](./media/sql-database-query-performance/details.png)
2. De detailweergave wordt geopend en het aantal query's CPU-verbruik/duur/uitvoering is onderverdeeld na verloop van tijd.
3. Klik op rond de grafiek voor meer informatie.
   
   * Bovenste grafiek bevat een regel met de algemene database DTU % en zijn de balken CPU-percentage die worden gebruikt door de geselecteerde query.
   * Tweede grafiek toont de totale duur van de geselecteerde query.
   * Onderste grafiek toont de totale aantal uitvoeringen door de geselecteerde query.
     
     ![Querydetails][3]
4. (Optioneel) Gebruik de schuifregelaars, knoppen Inzoomen en uitzoomen of klik op **instellingen** aanpassen hoe querygegevens worden weergegeven, of kies een andere periode.

## <a name="review-top-queries-per-duration"></a>Controleer de populairste query's per duur
In de recente update van de Query Performance Insight, hebben we twee nieuwe metrische gegevens waarmee u mogelijke knelpunten te identificeren kunt geïntroduceerd: aantal duur en kan worden uitgevoerd.<br>

Langlopende query's hebben de grootste kans op meer resources vergrendelen, andere gebruikers blokkeren en schaalbaarheid te beperken. Ze zijn ook de beste kandidaten voor optimalisatie.<br>

Langlopende query's identificeren:

1. Open **aangepaste** tabblad in de Query Performance Insight voor de geselecteerde database
2. Metrische gegevens moet wijzigen **duur**
3. Selecteer het aantal query's en observatie van interval
4. Selecteer de statistische functie
   
   * **Som** is de som van alle uitvoeringstijd van de query tijdens het hele waarneming interval.
   * **Maximale** vindt u query's die uitvoeringstijd maximale was op basis van hele waarneming interval.
   * **Gem.** gezocht gemiddelde uitvoeringstijd van alle uitvoeringen van de query en ziet u de bovenkant buiten deze gemiddelden. 
     
     ![Queryduur van de][4]

## <a name="review-top-queries-per-execution-count"></a>Bekijk de populairste query's per aantal uitvoeringen
Hoog aantal uitvoeringen kan niet worden die betrekking hebben op database zelf en verbruik van resources laag kan zijn, maar algemene toepassing krijgt mogelijk traag.

In sommige gevallen, aantal zeer hoog uitvoeringen kan leiden tot het verhogen van het netwerk ' round trips '. Retouren aanzienlijk beïnvloeden. Ze zijn afhankelijk van de netwerklatentie en latentie voor downstream-server. 

Bijvoorbeeld veel op gegevens gebaseerde websites sterk toegang krijgen tot de database voor elke gebruikersaanvraag. Terwijl u verbinding kunt, het toegenomen verkeer groeperen en verwerkingsbelasting voor de database-server kan de prestaties nadelig beïnvloeden.  Algemeen advies is ' round trips ' tot een absoluut minimum te houden.

Voor het identificeren van vaak uitgevoerd ("intensieve") query's voor query's:

1. Open **aangepaste** tabblad in de Query Performance Insight voor de geselecteerde database
2. Metrische gegevens moet wijzigen **aantal uitvoeringen**
3. Selecteer het aantal query's en observatie van interval
   
    ![uitvoering van het aantal query 's][5]

## <a name="understanding-performance-tuning-annotations"></a>Inzicht in prestaties afstemmen aantekeningen
Tijdens het verkennen van uw workload in Query Performance Insight, ziet u mogelijk pictogrammen met verticale lijn boven op de grafiek.<br>

Deze pictogrammen zijn aantekeningen; ze hebben betrekking op prestaties die betrekking hebben op acties die worden uitgevoerd door [SQL Azure Database Advisor](sql-database-advisor.md). Door zwevende aantekening krijgt u algemene informatie over de actie:

![query-aantekening][6]

Als u wilt meer informatie of advisor aanbeveling toepast, klikt u op het pictogram. Details van de actie wordt geopend. Als het een actieve aanbeveling kunt u meteen met behulp van opdracht kunt toepassen.

![Querydetails voor de aantekening][7]

### <a name="multiple-annotations"></a>Meerdere aantekeningen.
Het is mogelijk dat vanwege zoomniveau, aantekeningen die zich dicht bij elkaar wordt ophalen samengevoegd tot één. Dit wordt vertegenwoordigd door een speciaal pictogram, te klikken op het open nieuwe blade waarin de lijst met aantekeningen gegroepeerd zal worden weergegeven.
Correleren van query's en afstemmen van de acties van de prestaties kan helpen uw workload beter te begrijpen. 

## <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>De Query Store-configuratie voor Query Performance Insight optimaliseren
U kunt de volgende Query Store-berichten tegenkomen tijdens het gebruik van de Query Performance Insight:

* "Query Store is niet juist geconfigureerd op deze database. Klik hier voor meer informatie."
* "Query Store is niet juist geconfigureerd op deze database. Klik hier om instellingen te wijzigen." 

Deze berichten worden gewoonlijk weergegeven wanneer Query Store kan geen nieuwe gegevens te verzamelen. 

Eerste geval gebeurt er wanneer Query Store heeft de status alleen-lezen en parameters optimaal zijn ingesteld. U kunt dit oplossen door het vergroten van de Query Store of Query Store uit te schakelen.

![qds-knop][8]

Tweede geval treedt op wanneer de Query Store is uitgeschakeld of de parameters zijn niet optimaal ingesteld. <br>U kunt de retentie en vastleggen beleid wijzigen en Query Store inschakelen door het uitvoeren van de onderstaande opdrachten of rechtstreeks vanuit de portal:

![qds-knop][9]

### <a name="recommended-retention-and-capture-policy"></a>Aanbevolen beleid voor retentie en vastleggen
Er zijn twee soorten beleid voor het bewaren:

* Op basis van - grootte indien ingesteld op automatisch het worden schoongemaakt gegevens automatisch wanneer bijna de maximale grootte is bereikt.
* Op basis van tijd - standaard die deze wordt ingesteld op 30 dagen, wat betekent dat, als Query Store wordt uitgevoerd geen schijfruimte meer, worden gegevens ouder dan 30 dagen wordt verwijderd

Vastleggen van beleid kan worden ingesteld op:

* **Alle** -alle query's worden vastgelegd.
* **Automatisch** -incidentele query's en query's met een extra compileren en uitvoeren duur worden genegeerd. Drempelwaarden voor het aantal, compileren en runtime uitvoeringsduur worden intern bepaald. Dit is de standaardoptie.
* **Geen** -Query Store stopt het vastleggen van nieuwe query's, maar de runtimestatistieken voor al vastgelegde query's worden nog steeds verzameld.

U wordt aangeraden alle beleidsregels instellen op automatisch en schoon beleid tot 30 dagen:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Verhoog de grootte van de Query Store. Dit kan worden uitgevoerd door verbinding te maken met een database en het uitgeven van de volgende query:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Toepassen van deze instellingen zorgt uiteindelijk Query Store verzamelen van nieuwe query's, maar als u niet wilt wachten voordat u Query Store kunt wissen. 

> [!NOTE]
> Uitvoeren van de volgende query worden alle huidige gegevens in de Query Store verwijderd. 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Samenvatting
Query Performance Insight kunt u het effect van de werkbelasting van uw query's en hoe deze zich verhoudt tot verbruik van databaseresources begrijpen. Met deze functie is, wordt u meer informatie over het grootste verbruik van query's en eenvoudig identificeren om op te lossen voordat ze een probleem.

## <a name="next-steps"></a>Volgende stappen
Voor extra aanbevelingen over het verbeteren van de prestaties van uw SQL-database, klikt u op [aanbevelingen](sql-database-advisor.md) op de **Query Performance Insight** blade.

![Performance Advisor](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

