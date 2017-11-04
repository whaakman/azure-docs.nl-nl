---
title: Query performance insights voor Azure SQL Database | Microsoft Docs
description: Query-prestaties bewaken, identificeert de meeste CPU verbruikt query's voor een Azure SQL Database.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: c2f580b2-3835-453f-89f5-140e02dd2ea7
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 015f3255d0ff9691168899df1e983e70f35215b1
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="azure-sql-database-query-performance-insight"></a>Azure SQL Database Query Performance Insight
Is een uitdaging waarvoor aanzienlijke expertise en investeringen tijd beheren en het afstemmen van de prestaties van relationele databases. Inzicht in queryprestaties kunt u minder tijd databaseprestaties oplossen door het volgende nodig:

* Dieper inzicht in uw databases brongebruik (DTU). 
* De top-query's op het aantal CPU/duur/uitvoering, die mogelijk voor verbeterde prestaties kunnen worden afgestemd.
* De mogelijkheid om in te zoomen naar de details van een query de tekst en de geschiedenis van Resourcegebruik weergeven. 
* Prestaties afstemmen aantekeningen die acties uitgevoerd door weergeven [SQL Azure Database Advisor](sql-database-advisor.md)  



## <a name="prerequisites"></a>Vereisten
* Query Performance Insight vereist dat [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) actief is op uw database. Als de Query Store niet wordt uitgevoerd, wordt de portal vraagt u te schakelen.

## <a name="permissions"></a>Machtigingen
De volgende [toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md) machtigingen zijn vereist voor Query Performance Insight gebruiken: 

* **Lezer**, **eigenaar**, **Inzender**, **SQL DB Contributor**, of **SQL Server Inzender** machtigingen zijn vereist de bovenste bron verbruikt query's en grafieken weer te geven. 
* **Eigenaar**, **Inzender**, **SQL DB Contributor**, of **SQL Server Inzender** machtigingen zijn vereist om querytekst weer te geven.

## <a name="using-query-performance-insight"></a>Met behulp van de Query Performance Insight
Inzicht in queryprestaties is eenvoudig te gebruiken:

* Open [Azure-portal](https://portal.azure.com/) en zoeken naar database die u wilt onderzoeken. 
  * Selecteer in de linkerkant menu onder ondersteuning en het oplossen van problemen 'Query Performance Insight'.
* Bekijk de lijst met de top-resource verbruikende query's op het eerste tabblad.
* Selecteer een afzonderlijke query om de details ervan weer te geven.
* Open [SQL Azure Database Advisor](sql-database-advisor.md) en controleer of er geen aanbevelingen beschikbaar zijn.
* Schuifregelaars gebruiken of zoomen pictogrammen om geobserveerde interval te wijzigen.
  
    ![Prestatiedashboard](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Een paar uur van gegevens moet worden vastgelegd door het Queryarchief van de SQL Database een inzichten voor queryprestaties. Als de database geen activiteiten heeft of Query Store gedurende een bepaalde periode niet actief was, de grafieken worden leeg om deze periode weer te geven. U kunt Query Store op elk gewenst moment inschakelen als deze niet wordt uitgevoerd.   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a>Bekijk de hoogste CPU verbruikt query 's
In de [portal](http://portal.azure.com) het volgende doen:

1. Blader naar een SQL-database en klik op **alle instellingen** > **ondersteuning + probleemoplossing** > **Query performance insight**. 
   
    ![Inzicht in queryprestaties][1]
   
    De weergave van de meest gebruikte query's wordt geopend en de hoogste CPU in beslag neemt query's worden vermeld.
2. Klik op rond de grafiek voor meer informatie.<br>De bovenste regel bevat algemene DTU % voor de database, terwijl de balken die door de geselecteerde query's worden gebruikt tijdens de geselecteerde interval CPU-percentage weergeven (bijvoorbeeld als **afgelopen week** is geselecteerd elke balk vertegenwoordigt één dag).
   
    ![Top-query 's][2]
   
    Het raster onder vertegenwoordigt verzamelde informatie voor de zichtbare query's.
   
   * Query-ID - de unieke id van de query in de database.
   * CPU per query tijdens waarneembare interval (afhankelijk van statistische functie).
   * Duur per query (afhankelijk van statistische functie).
   * Totaal aantal uitvoeringen voor een bepaalde query.
     
     Schakel of afzonderlijke query's wilt opnemen of uitsluiten van de grafiek met behulp van de selectievakjes uit.
3. Als uw gegevens verlopen is, klikt u op de **vernieuwen** knop.
4. U kunt gebruiken schuifregelaars en knoppen observatie-interval wijzigen en onderzoeken pieken inzoomen en uitzoomen: ![instellingen](./media/sql-database-query-performance/zoom.png)
5. Als u een andere weergave wilt, u kunt eventueel selecteren **aangepaste** tabblad en ingesteld:
   
   * Metrische gegevens (CPU, duur, het aantal keer uitgevoerd)
   * Tijdsinterval (afgelopen week afgelopen maand afgelopen 24 uur). 
   * Aantal query's.
   * Statistische functie.
     
     ![instellingen](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Details van de afzonderlijke query's weergeven
Om Querydetails te bekijken:

1. Klik op een query in de lijst met de meest gebruikte query's.
   
    ![Meer informatie](./media/sql-database-query-performance/details.png)
2. De detailweergave wordt geopend en het aantal query's CPU-verbruik/duur/uitvoering is onderverdeeld gedurende een bepaalde periode.
3. Klik op rond de grafiek voor meer informatie.
   
   * Bovenste diagram toont de regel met algemene database DTU % en de balken zijn CPU-percentage gebruikt door de geselecteerde query.
   * Tweede diagram toont de totale duur door de geselecteerde query.
   * Onder diagram toont het aantal uitvoeringen door de geselecteerde query.
     
     ![Querydetails][3]
4. Optioneel gebruik schuifregelaars, knoppen Inzoomen en uitzoomen of klik op **instellingen** aanpassen hoe querygegevens worden weergegeven, of kies een andere periode.

## <a name="review-top-queries-per-duration"></a>Bekijk meest gebruikte query's per duur
In de recente update van de Query Performance Insight, er zijn twee nieuwe metrische gegevens kunt u potentiële knelpunten geïntroduceerd: aantal duur en uitvoering.<br>

Langlopende query's hebben de grootste kans dat langer resources vergrendelen, andere gebruikers worden geblokkeerd en schaalbaarheid te beperken. Ze zijn ook het meest in aanmerking voor optimalisatie.<br>

Langdurige query's identificeren:

1. Open **aangepaste** tabblad in de Query Performance Insight voor de geselecteerde database
2. Metrische gegevens moet wijzigen **duur**
3. Selecteer het aantal query's en observatie-interval
4. Selecteer de statistische functie
   
   * **Som** tijdens de gehele observatie-interval is de som van alle uitvoeringstijd van de query.
   * **Maximale** vindt u query's welke uitvoeringstijd maximale was op de hele observatie-interval.
   * **Gem.** gezocht gemiddelde uitvoeringstijd van alle query uitvoeringen en ziet u boven buiten deze gemiddelden. 
     
     ![duur van de query][4]

## <a name="review-top-queries-per-execution-count"></a>Bekijk meest gebruikte query's per aantal keer uitgevoerd
Hoog aantal uitvoeringen mogelijk niet beïnvloeden database zelf en gebruik van bronnen kan lage, maar algemene toepassing, haalt traag.

In sommige gevallen zeer hoge uitvoering count kan leiden tot het vergroten van retouren netwerk. Retouren aanzienlijk beïnvloeden. Ze zijn onderworpen aan netwerklatentie en de latentie downstream-server. 

Bijvoorbeeld: veel gegevensgestuurde websites sterk toegang krijgen tot de database voor elke aanvraag van gebruiker. Tijdens het verbinding helpt, het toegenomen verkeer groeperen en verwerken van de belasting van de database-server kan de prestaties nadelig beïnvloeden.  Algemeen advies is om retouren tot een absoluut minimum te beperken.

Uitgevoerd om te identificeren vaak query's ('chatty') query's:

1. Open **aangepaste** tabblad in de Query Performance Insight voor de geselecteerde database
2. Metrische gegevens moet wijzigen **aantal keer uitgevoerd**
3. Selecteer het aantal query's en observatie-interval
   
    ![uitvoering van het aantal query 's][5]

## <a name="understanding-performance-tuning-annotations"></a>Inzicht in prestaties afstemmen aantekeningen
Tijdens het verkennen van de werkbelasting in Query Performance Insight, ziet u mogelijk pictogrammen met verticale lijn boven op de grafiek.<br>

Deze pictogrammen zijn aantekeningen; ze hebben betrekking op prestaties beïnvloeden bewerkingen worden uitgevoerd door [SQL Azure Database Advisor](sql-database-advisor.md). Door zwevende aantekening krijgen u algemene informatie over de actie:

![query aantekening][6]

Als u wilt meer weten of de aanbeveling advisor toepassen, klikt u op het pictogram. Details van de actie wordt geopend. Als een actieve bevelen kunt u meteen met behulp van opdracht toepassen.

![details van de aantekening query][7]

### <a name="multiple-annotations"></a>Meerdere aantekeningen.
Het is mogelijk dat vanwege zoomniveau, aantekeningen die zich dicht bij elkaar ophalen tot één samengevouwen zullen. Hiermee wordt aangegeven door speciale pictogram, erop te klikken wordt geopend nieuwe blade waarin de lijst met aantekeningen gegroepeerd worden weergegeven.
Correleren van query's en -prestaties afstemmen acties kan helpen om uw werkbelasting beter te begrijpen. 

## <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>De Query Store-configuratie voor Query Performance Insight optimaliseren
U kunt de volgende Query Store-berichten tegenkomen tijdens het gebruik van de Query Performance Insight:

* 'Query Store is niet goed geconfigureerd op deze database. Klik hier voor meer informatie."
* 'Query Store is niet goed geconfigureerd op deze database. Klik hier om instellingen te wijzigen." 

Deze berichten worden doorgaans weergegeven wanneer het Queryarchief kan geen nieuwe gegevens verzamelen. 

Eerste geval gebeurt wanneer Query Store in alleen-lezen status en parameters optimaal zijn ingesteld. U kunt dit oplossen door het vergroten van de Query Store of Query Store uit te schakelen.

![knop qds][8]

Tweede geval gebeurt wanneer de Query Store is uitgeschakeld of de parameters zijn niet optimaal ingesteld. <br>U kunt het beleid bewaren en vastleggen wijzigen en Query Store inschakelen door het uitvoeren van de onderstaande opdrachten of rechtstreeks vanuit de portal:

![knop qds][9]

### <a name="recommended-retention-and-capture-policy"></a>Aanbevolen bewaren en vastleggen beleid
Er zijn twee soorten bewaarbeleidsregels:

* Op de basis - grootte als ingesteld op AUTO deze worden schoongemaakt gegevens automatisch wanneer bijna de maximale grootte is bereikt.
* Op basis van tijd - standaard die we deze wordt ingesteld op 30 dagen, wat betekent dat, als de Query Store is geen ruimte meer wordt uitgevoerd, querygegevens ouder dan 30 dagen wordt verwijderd

Vastleggen van beleid kan worden ingesteld op:

* **Alle** -alle query's worden vastgelegd.
* **Automatische** -incidentele's en query's met verwaarlozen compileren en uitvoering duur worden genegeerd. Drempelwaarden voor de duur voor het aantal, compileren en runtime uitvoeren worden intern bepaald. Dit is de standaardoptie.
* **Geen** -Query Store stopt u het vastleggen nieuwe query's, maar de runtime-statistieken voor al vastgelegde query's worden nog steeds verzameld.

U wordt aangeraden alle beleidsregels instellen op automatisch en schone beleid tot 30 dagen:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Verhoog de grootte van de Query Store. Dit kan worden uitgevoerd door verbinding te maken met een database en het uitgeven van volgende query:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Deze instellingen zijn toegepast brengt uiteindelijk Query Store verzamelen van de nieuwe query's, maar als u niet wilt wachten voordat u Query Store kunt wissen. 

> [!NOTE]
> Uitvoeren van de volgende query worden alle huidige gegevens in het Queryarchief verwijderd. 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Samenvatting
Inzicht in queryprestaties vindt u informatie over het effect van uw workload van query's en hoe deze zich verhoudt tot brongebruik van de database. Met deze functie wordt u meer informatie over het grootste verbruik van query's en gemakkelijk herkennen die om op te lossen voordat ze een probleem.

## <a name="next-steps"></a>Volgende stappen
Extra aanbevelingen over het verbeteren van de prestaties van uw SQL-database, klikt u op [aanbevelingen](sql-database-advisor.md) op de **Query Performance Insight** blade.

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

