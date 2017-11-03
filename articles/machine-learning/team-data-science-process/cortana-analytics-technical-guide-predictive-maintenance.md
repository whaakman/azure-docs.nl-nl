---
title: Momenteel met Azure - Cortana Intelligence technische handleiding voor voorspeld onderhoud | Microsoft Docs
description: Een technische handleiding voor de sjabloon oplossing met Microsoft Cortana Intelligence voor voorspeld onderhoud in ruimtevaart, hulpprogramma's en transport.
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2c4d2147-0f05-4705-8748-9527c2c1f033
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: a6d9cd05eb370399fc95cc64bae892e8b5a73fe2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Technische handleiding voor de sjabloon Cortana Intelligence-oplossing voor voorspeld onderhoud in ruimtevaart en andere bedrijven

## <a name="important"></a>**Belangrijk**
In dit artikel is afgeschaft. De informatie die relevant is voor bij de hand, het probleem dat wil zeggen voorspeld onderhoud momenteel, maar het meest recente artikel met de meest recente informatie kan niet worden gevonden [hier](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace). 

## <a name="acknowledgements"></a>**Bevestigingen**
In dit artikel is geschreven door gegevenswetenschappers Yan Zhang, Gauher Shaheen, Fidan Boylu Uz en software engineer Dan Grecoe bij Microsoft.

## <a name="overview"></a>**Overzicht**
Oplossingssjablonen zijn ontworpen om het proces van het bouwen van een demo E2E boven op de Cortana Intelligence Suite versnellen. Een geïmplementeerde sjabloon wordt uw abonnement met vereiste onderdelen van de Cortana Intelligence inrichten en hun onderlinge relaties bouwen. Deze ook de seeding van de pijplijn gegevens met voorbeeldgegevens die zijn gegenereerd op basis van een toepassing voor de generator van gegevens die u downloadt en installeert op uw lokale computer nadat u de oplossingssjabloon implementeert. De gegevens die zijn gegenereerd op basis van de generator wordt hydrate de gegevens pijplijn en start het genereren van machine learning voorspellingen die vervolgens kunnen worden weergegeven op het Power BI-dashboard. Het implementatieproces leidt u door de verschillende stappen voor het instellen van de referenties van uw oplossing. Zorg ervoor dat u deze referenties zoals oplossingsnaam, gebruikersnaam en wachtwoord dat u tijdens de implementatie opgeeft vastleggen.  

Het doel van dit document is om uit te leggen van de referentiearchitectuur en de verschillende onderdelen die zijn ingericht in uw abonnement als onderdeel van de oplossingssjabloon voor deze. Het document is ook wordt gesproken over hoe u de voorbeeldgegevens vervangen door echte gegevens van uw eigen kunnen inzicht en voorspellingen van uw eigen gegevens te zien. Bovendien wordt het document beschreven voor de onderdelen van de oplossing-sjabloon die worden gewijzigd moet als u wilt aanpassen van de oplossing met uw eigen gegevens. Over het bouwen van de Power BI-dashboard voor de sjabloon voor deze oplossing worden instructies aan het einde.

> [!TIP]
> U kunt downloaden en afdrukken een [PDF-versie van dit document](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).
> 
> 

## <a name="big-picture"></a>**Grote afbeelding**
![Architectuur voor voorspeld onderhoud](./media/cortana-analytics-technical-guide-predictive-maintenance/predictive-maintenance-architecture.png)

Als u de oplossing is geïmplementeerd, verschillende Azure-services binnen Cortana-Analytics Suite zijn geactiveerd (*dat wil zeggen* Event Hub, Stream Analytics, HDInsight, Data Factory Machine Learning, *enzovoort*). Het bovenstaande Architectuurdiagram toont, op een hoog niveau hoe voorspeld onderhoud voor lucht oplossingssjabloon is samengesteld uit de end-to-end. U mag deze services in de azure-portal door erop te klikken op de oplossing sjabloon diagram gemaakt met de implementatie van de oplossing met uitzondering van HDInsight als deze service is ingericht op aanvraag wanneer de gerelateerde pipeline-activiteiten vereist voor het uitvoeren en verwijderde daarna zijn onderzoeken.
U kunt downloaden via een [volledige versie van het diagram](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

De volgende secties beschrijven elk gegeven.

## <a name="data-source-and-ingestion"></a>**Gegevensbron en opnamesnelheid**
### <a name="synthetic-data-source"></a>Synthetische gegevensbron
De gegevensbron die gebruikt wordt voor deze sjabloon gegenereerd vanuit een bureaubladtoepassing die u wilt downloaden en lokaal uitvoeren na een geslaagde implementatie. Vindt u de instructies voor het downloaden en installeren van deze toepassing op de Eigenschappenbalk wanneer u het eerste knooppunt voorspeld onderhoud Gegevensgenerator aangeroepen in het diagram van de sjabloon oplossing selecteert. Deze toepassing feeds de [Azure Event Hub](#azure-event-hub) service met gegevenspunten of gebeurtenissen die worden gebruikt in de rest van de stroom van de oplossing. Deze gegevensbron bestaat uit of afgeleid uit openbaar beschikbare gegevens uit de [NASA gegevensopslagplaats](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) met behulp van de [Turbofan Engine Degradation Simulation Data Set](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

De toepassing voor het genereren van gebeurtenis wordt de Azure Event Hub gevuld alleen terwijl deze wordt uitgevoerd op uw computer.

### <a name="azure-event-hub"></a>Azure event hub
De [Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) -service is de ontvanger van de ingevoerde door de synthetische gegevensbron die hierboven worden beschreven.

## <a name="data-preparation-and-analysis"></a>**Voorbereiden van gegevens en analyse**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
De [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) service wordt gebruikt voor bijna in realtime analyses op de invoerstroom van geven de [Azure Event Hub](#azure-event-hub) service en publiceren van de resultaten naar een [Power BI](https://powerbi.microsoft.com) dashboard, evenals alle onbewerkte binnenkomende gebeurtenissen te archiveren de [Azure Storage](https://azure.microsoft.com/services/storage/) service later worden verwerkt door de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service.

### <a name="hd-insights-custom-aggregation"></a>HD Insights aangepaste aggregatie
De service Azure HD inzicht wordt gebruikt om uit te voeren [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts (gedirigeerd door Azure Data Factory) voor aggregaties op de onbewerkte gebeurtenissen die zijn gearchiveerd met de Azure Stream Analytics-service.

### <a name="azure-machine-learning"></a>Azure Machine Learning
De [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) service (gedirigeerd door Azure Data Factory) wordt gebruikt voor het maken van voorspellingen op de resterende levensduur (resterende Levensduur) van een bepaalde vliegtuigmotor gegeven van de invoer ontvangen.

## <a name="data-publishing"></a>**Gegevens publiceren**
### <a name="azure-sql-database-service"></a>Azure SQL Database-Service
De [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) service wordt gebruikt om op te slaan (beheerd door Azure Data Factory) de voorspellingen ontvangen door de Azure Machine Learning-service die worden gebruikt in de [Power BI](https://powerbi.microsoft.com) dashboard.

## <a name="data-consumption"></a>**Gegevens verbruik**
### <a name="power-bi"></a>Power BI
De [Power BI](https://powerbi.microsoft.com) service wordt gebruikt voor het weergeven van een dashboard met aggregaties en waarschuwingen die worden geleverd door de [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) service en de resterende Levensduur voorspellingen opgeslagen in [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) die zijn geproduceerd met behulp van de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) service. Raadpleeg de sectie hieronder voor instructies over het bouwen van de Power BI-dashboard voor deze oplossing-sjabloon.

## <a name="how-to-bring-in-your-own-data"></a>**Hoe op te zetten in uw eigen gegevens**
Deze sectie wordt beschreven hoe u uw eigen gegevens overbrengen naar Azure en welke gebieden voor de gegevens die u in deze architectuur brengt wijzigingen nodig.

Het lijkt onwaarschijnlijk dat een gegevensset die u brengt overeenkomen met de gegevensset die wordt gebruikt door de [Turbofan Engine Degradation Simulation Data Set](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) voor deze oplossingssjabloon gebruikt. Inzicht in uw gegevens en de vereisten is cruciaal belang bij hoe het wijzigen van deze sjabloon wilt werken met uw eigen gegevens. Als dit uw eerste blootstelling aan de service Azure Machine Learning is, kunt u een inleiding tot het krijgen met behulp van het voorbeeld in [het maken van uw eerste experiment](../studio/create-experiment.md).

De volgende secties wordt besproken in de secties van de sjabloon die wijzigingen is vereist wanneer een nieuwe gegevensset wordt geïntroduceerd.

### <a name="azure-event-hub"></a>Azure Event Hub
De service Azure Event Hub is zeer algemeen, zodat gegevens naar de hub in CSV- of JSON-indeling kan worden geplaatst. Geen speciale verwerking optreedt in de Azure Event Hub, maar het is belangrijk dat u begrijpt dat de gegevens die wordt ingevoerd.

Dit document beschrijft niet hoe u uw gegevens opnemen, maar u kunt eenvoudig verzenden gebeurtenissen of gegevens naar een Azure Event Hub met behulp van de Event Hub-API.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
De Azure Stream Analytics-service wordt gebruikt om uw bijna realtime analyses te lezen van gegevensstromen en uitvoeren van gegevens naar een willekeurig aantal bronnen.

Voor voorspeld onderhoud voor lucht oplossingssjabloon bestaat de Azure Stream Analytics query uit vier subquery's, elk consumerende gebeurtenissen van de service Azure Event Hub en hoeven uitvoer vier verschillende locaties. Deze uitvoer bestaan uit drie Power BI-gegevenssets en een Azure-opslaglocatie.

De Azure Stream Analytics query kan worden gevonden door:

* Aanmelden bij de Azure-portal
* Zoeken naar de Stream Analytics-taken ![Stream Analytics-pictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-stream-analytics.png) die zijn gegenereerd tijdens de oplossing is geïmplementeerd (*bijvoorbeeld*, **maintenancesa02asapbi** en **maintenancesa02asablob** voor de oplossing voor voorspeld onderhoud)
* Selecteren
  
  * ***INVOER*** invoer als de query wilt weergeven
  * ***QUERY*** om de query zelf weer te geven
  * ***LEVERT*** om de verschillende uitvoer weer te geven

Informatie over het samenstellen van Azure Stream Analytics query vindt u in de [Stream Analytics Query verwijzing](https://msdn.microsoft.com/library/azure/dn834998.aspx) op MSDN.

In deze oplossing uitvoergegevenssets de query's drie met bijna realtime analyses informatie over de gegevensstroom inkomende aan een Power BI-dashboard die wordt geleverd als onderdeel van de oplossingssjabloon voor deze. Omdat er impliciete kennis over de indeling van de binnenkomende, moet deze query's worden gewijzigd op basis van indeling van uw gegevens.

De query in de tweede Stream Analytics-taak **maintenancesa02asablob** levert gewoon alle [Event Hub](https://azure.microsoft.com/services/event-hubs/) gebeurtenissen die moeten worden [Azure Storage](https://azure.microsoft.com/services/storage/) en daarom geen wijziging ongeacht de indeling van uw gegevens is vereist omdat de informatie over de volledige gebeurtenis gestreamd naar opslag.

### <a name="azure-data-factory"></a>Azure Data Factory
De [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service ingedeeld het verkeer en de verwerking van gegevens. In de voorspeld onderhoud voor lucht oplossingssjabloon de gegevensfactory bestaat uit drie [pijplijnen](../../data-factory/v1/data-factory-create-pipelines.md) die verplaatsen en de gegevens met behulp van verschillende technologieën worden verwerkt.  U kunt uw gegevensfactory openen door het openen van de het knooppunt Data Factory onderaan in het diagram van de sjabloon oplossing gemaakt met de implementatie van de oplossing. Hiermee gaat u aan de gegevensfactory in uw Azure-portal. Als u fouten onder uw gegevenssets ziet, kunt u die negeren omdat deze vanwege gegevensfactory worden geïmplementeerd voordat de gegevensgenerator werd gestart. Deze fouten doen niet voorkomen dat uw data factory werkt.

![Data Factory-gegevensset fouten](./media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

In deze sectie worden de benodigde [pijplijnen](../../data-factory/v1/data-factory-create-pipelines.md) en [activiteiten](../../data-factory/v1/data-factory-create-pipelines.md) opgenomen in de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Hieronder vindt u de diagramweergave van de oplossing.

![Azure Data Factory](./media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Twee van de pijplijnen van deze factory bevatten [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts die worden gebruikt voor het partitioneren en de gegevens totaliseren. Wanneer u hebt genoteerd, de scripts bevindt zich de [Azure Storage](https://azure.microsoft.com/services/storage/) account gemaakt tijdens de installatie. De locatie zijn: maintenancesascript\\\\script\\\\hive\\ \\ (of https://[Your oplossing name].blob.core.windows.net/maintenancesascript).

Net als bij de [Azure Stream Analytics](#azure-stream-analytics-1) query's de [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts impliciete kennis over de indeling van de binnenkomende, deze query's zou moeten worden gewijzigd op basis van indeling van uw gegevens.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*
Dit [pijplijn](../../data-factory/v1/data-factory-create-pipelines.md) bevat een enkele activiteit - een [HDInsightHive](../../data-factory/v1/data-factory-hive-activity.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die wordt uitgevoerd een [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor het partitioneren van de gegevens in plaatsen [Azure Storage](https://azure.microsoft.com/services/storage/) tijdens de [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) taak.

De [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor deze taak partitionering is ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
Dit [pijplijn](../../data-factory/v1/data-factory-create-pipelines.md) bevat meerdere activiteiten en waarvan eindresultaat is de scored voorspellingen van het [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenten die zijn gekoppeld aan deze oplossingssjabloon.

De opgenomen in deze activiteiten zijn:

* [HDInsightHive](../../data-factory/v1/data-factory-hive-activity.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die wordt uitgevoerd een [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor het uitvoeren van de aggregaties en functie-engineering nodig zijn voor de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren.
  De [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor deze taak partitionering is ***PrepareMLInput.hql***.
* [Kopiëren](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die wordt verplaatst van de resultaten van de [HDInsightHive](../../data-factory/v1/data-factory-hive-activity.md) activiteit aan een enkele [Azure Storage](https://azure.microsoft.com/services/storage/) blob die toegang op basis van worden kan de [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) activiteit.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) activiteit die roept de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment, wat resulteert in de resultaten worden opgeslagen in één [Azure Storage](https://azure.microsoft.com/services/storage/) blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
Dit [pijplijn](../../data-factory/v1/data-factory-create-pipelines.md) bevat een enkele activiteit - een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die wordt verplaatst van de resultaten van de [Azure Machine Learning](#azure-machine-learning) experimenteren uit de ***MLScoringPipeline*** naar de [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) die als onderdeel van de installatie van de sjabloon oplossing is ingericht.

### <a name="azure-machine-learning"></a>Azure Machine Learning
De [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren gebruikt voor deze oplossingssjabloon biedt de resterende nuttig Levensduur van een vliegtuigmotor. Het experiment is specifiek voor de gegevensset die is verbruikt en daarom moet wijziging of vervanging is specifiek voor de gegevens die in gebracht.

Zie voor meer informatie over hoe het Azure Machine Learning-experiment is gemaakt, [voorspeld onderhoud: stap 1 van 3, het voorbereiden van gegevens en functie-engineering](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Voortgang van de monitor**
 Zodra de Generator van gegevens wordt gestart, de pijplijn begint met het ophalen van gehydrateerd en de verschillende onderdelen van uw oplossing beginnen bij de start in de actie volgende de opdrachten die zijn uitgegeven door de Data Factory. Er zijn twee manieren kunt u de pijplijn bewaken.

1. Een van de Stream Analytics-taak schrijft de onbewerkte binnenkomende gegevens naar blob storage. Als u op voor Blob Storage-onderdeel van uw oplossing van het scherm u is de oplossing geïmplementeerd en klik vervolgens in het rechterdeelvenster op openen, gaat u naar de [beheerportal](https://portal.azure.com/). Klik op Blobs. In het volgende deelvenster ziet u een lijst van Containers. Klik op **maintenancesadata**. In het volgende deelvenster ziet u hoe de **rawdata** map. In de map rawdata ziet u de mappen met de namen bijvoorbeeld uur = 17, uur = 18 enzovoort. Als u deze mappen ziet, betekent dit dat de onbewerkte gegevens voltooid is wordt op uw computer wordt gegenereerd en opgeslagen in blob storage. U ziet de csv-bestanden die eindig grootte in MB in deze mappen moeten hebben.
2. De laatste stap van de pijplijn is om gegevens te schrijven (bijvoorbeeld de voorspellingen van machine learning) in SQL-Database. U moet wellicht wacht maximaal drie uur voor de gegevens worden weergegeven in de SQL-Database. Een manier om te controleren hoeveel gegevens zijn beschikbaar in de SQL-Database, is via [azure-portal](https://manage.windowsazure.com/). Zoek in het linkerdeelvenster SQL-DATABASES ![SQL pictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-SQL-databases.png) en klik erop. Ga vervolgens naar de database **pmaintenancedb** en klik erop. Klik in de volgende pagina aan de onderkant op beheren
   
    ![Pictogram beheren](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-manage.png).
   
    U kunt hier, klik op nieuwe Query en de query voor het aantal rijen (bv. Selecteer count(*) van PMResult). Als uw database groeit, wordt het aantal rijen in de tabel moet verhogen.

## <a name="power-bi-dashboard"></a>**Power BI-Dashboard**
### <a name="overview"></a>Overzicht
Deze sectie beschrijft hoe u Power BI-dashboard instelt voor het visualiseren van realtime gegevens uit Azure Stream Analytics (hot pad) en de resultaten van de batch voorspelling van Azure machine learning-(koude pad).

### <a name="setup-cold-path-dashboard"></a>Setup koude pad dashboard
In de pijplijn voor koude pad gegevens, is het essentieel doel is om op te halen van de voorspellende resterende Levensduur (resterende levensduur) van elke vliegtuigmotor zodra een vlucht (cyclus) is voltooid. Het resultaat van de voorspelling wordt elke drie uur voor het voorspellen van de vliegtuigmotoren die een vlucht voltooid tijdens de afgelopen drie uur bijgewerkt.

Power BI maakt verbinding met een Azure SQL database als de gegevensbron, waar de voorspelling resultaten worden opgeslagen. Opmerking: 1) bij het implementeren van uw oplossing een echte voorspelling wordt weergegeven in de database binnen drie uur.
De pbix-bestand met het downloaden van de codegenerator bevat enkele seedgegevens, zodat u meteen de Power BI-dashboard kunt maken. 2) in deze stap wordt de vereiste is om te downloaden en installeren van de gratis software [van Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

De volgende stappen helpen u op verbinding maken tussen de pbix-bestand en de SQL-Database die is geactiveerd op het moment van implementatie van de oplossing met gegevens (*bijvoorbeeld*. resultaten van de voorspelling) voor visualisatie.

1. De databasereferenties ophalen.
   
   U moet **servernaam, databasenaam, gebruikersnaam en wachtwoord van de database** voordat u doorgaat naar de volgende stappen. Hier volgen de stappen om u te helpen ze te zoeken.
   
   * Eenmaal **'Azure SQL Database'** op uw oplossingssjabloon diagram wordt groen, klik hierop en klik op **'Openen'**.
   * Hier ziet u een nieuw tabblad/browservenster waarin de Azure portal-pagina. Klik op **'Resourcegroepen'** in het linkerdeelvenster.
   * Selecteer het abonnement dat u gebruikt voor het implementeren van de oplossing en selecteer vervolgens **' YourSolutionName\_ResourceGroup'**.
   * Klik in de nieuwe pop uit het Configuratiescherm op de ![SQL pictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-sql.png) pictogram voor toegang tot uw database. De naam van uw database is naast het pictogram (*bijvoorbeeld*, **'pmaintenancedb'**), en de **databaseservernaam** wordt vermeld onder de naameigenschap van de Server en moet er ongeveer uitzien **YourSoutionName.database.windows.net**.
   * Uw database **gebruikersnaam** en **wachtwoord** zijn hetzelfde als de gebruikersnaam en wachtwoord eerder vastgelegd tijdens de implementatie van de oplossing.
2. De gegevensbron van het rapportbestand koude pad bijwerken met Power BI Desktop.
   
   * In de map op uw PC waarop u hebt gedownload en het bestand Generator zijn uitgepakt, dubbelklikt u op de **PowerBI\\PredictiveMaintenanceAerospace.pbix** bestand. Als u eventuele waarschuwingsberichten ziet wanneer u het bestand opent, negeert u deze. Klik boven aan het bestand **query's bewerken**.
     
     ![Query's bewerken](./media/cortana-analytics-technical-guide-predictive-maintenance/edit-queries.png)
   * Ziet u twee tabellen **RemainingUsefulLife** en **PMResult**. Selecteer de eerste tabel en klik op ![Query Instellingenpictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-query-settings.png) naast **'Source'** onder **'Toegepast stappen'** aan de rechterkant **Query-instellingen** Configuratiescherm. Negeer eventuele waarschuwingsberichten die worden weergegeven.
   * Vervang in het pop uit venster **'Server'** en **'Database'** met uw eigen namen van de server en database en klik vervolgens op **'OK'**. Voor de servernaam van de, zorg ervoor dat u de poort 1433 opgeven (**YourSoutionName.database.windows.net, 1433**). Laat het veld Database als **pmaintenancedb**. Negeer de waarschuwingsberichten die worden weergegeven op het scherm.
   * In de volgende pop uit venster ziet u twee opties in het linkerdeelvenster (**Windows** en **Database**). Klik op **'Database'**, vul uw **'Username'** en **'Password'** (dit is de gebruikersnaam en het wachtwoord dat u hebt ingevoerd wanneer u eerst de oplossing geïmplementeerd en een Azure SQL database gemaakt). In ***Selecteer welk gegevensniveau toe te passen van deze instellingen***, niveau databaseoptie controleren. Klik vervolgens op **'Connect'**.
   * Klik op de tweede tabel **PMResult** klikt u vervolgens op ![pictogram Navigatie](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-navigation.png) naast **'Source'** onder **'Toegepast stappen'** aan de rechterkant **Query-instellingen** deelvenster, en de namen van de server en database zoals in de bovenstaande stappen bijwerken en klik op OK.
   * Als u terug naar de vorige pagina bent geleid, sluit het venster. Een bericht verschijnt buiten: klik **toepassen**. Klik ten slotte de **opslaan** knop de wijzigingen wilt opslaan. Uw Power BI-bestand is nu ingesteld voor verbinding met de server. Als uw visualisaties leeg zijn, zorg er dan voor dat u de selecties op de visualisaties niet alle gegevens visualiseren door te klikken op het pictogram gum op de rechterbovenhoek van de legenda's wissen. Gebruik de knop Vernieuwen in overeenstemming met nieuwe gegevens op de visualisaties. In eerste instantie alleen ziet u de seedgegevens op uw visualisaties tijdens de gegevensfactory is gepland voor elke drie uur vernieuwen. U ziet na drie uur nieuwe voorspellingen doorgevoerd in uw visualisaties Wanneer u de gegevens vernieuwen.
3. (Optioneel) Publiceren van het dashboard koude pad om [Power BI online](http://www.powerbi.com/). Houd er rekening mee dat deze stap moet een Power BI-account (of Office 365-account).
   
   * Klik op **'Publiceren'** en later enkele seconden verschijnt er een venster weergeven 'Publiceren naar Power BI geslaagd'! met een groen vinkje. Klik op de koppeling hieronder 'Open PredictiveMaintenanceAerospace.pbix in Power BI'. Gedetailleerde instructies vindt [publiceren vanuit Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Een nieuw dashboard maken: klik op de  **+**  Meld u aan bij de **Dashboards** sectie in het linkerdeelvenster. Voer de naam 'Voorspeld onderhoud Demo' voor dit nieuwe dashboard.
   * Nadat u het rapport openen, klikt u op ![Punaisepictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-pin.png) alle visualisaties aan uw dashboard vastmaken. Gedetailleerde instructies vindt [een tegel vastmaken aan Power BI-dashboard uit een rapport](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Ga naar de dashboardpagina en de grootte en locatie van uw visualisaties aanpassen en hun titels bewerken. Gedetailleerde instructies voor het bewerken van uw tegels Zie [bewerken een tegel--formaat, verplaatsen, wijzig de naam, pincode, verwijderen, voegt u hyperlink](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Hier volgt een voorbeeld van dashboard met sommige visualisaties koude pad is vastgemaakt aan het.  Afhankelijk van hoe lang het uitvoeren van de gegevensgenerator van uw, kan de nummers op de visualisaties afwijken.
     <br/>
     ![Laatste weergeven](./media/cortana-analytics-technical-guide-predictive-maintenance/final-view.png)
     <br/>
   * Vernieuwen van de planning van de gegevens, de muisaanwijzer boven de **PredictiveMaintenanceAerospace** gegevensset, klikt u op ![beletselteken pictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-elipsis.png) en kies vervolgens **schema vernieuwen**.
     <br/>
     **Opmerking:** als er een waarschuwing massage, klikt u op **referenties bewerken** en zorg ervoor dat uw databasereferenties zijn hetzelfde als die wordt beschreven in stap 1.
     <br/>
     ![Geplande vernieuwing](./media/cortana-analytics-technical-guide-predictive-maintenance/schedule-refresh.png)
     <br/>
   * Vouw de **schema vernieuwen** sectie. Schakel in 'uw gegevens up-to-date houden'.
     <br/>
   * De vernieuwing op basis van de behoeften van uw plannen. Ga voor meer informatie Zie [gegevens vernieuwen in Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Hot pad dashboard instellen
De volgende stappen leidt het visualiseren van realtime gegevensuitvoer van de Stream Analytics-taken die zijn gegenereerd op het moment van implementatie van de oplossing. Een [Power BI online](http://www.powerbi.com/) account is vereist de volgende stappen uit te voeren. Als u geen account hebt, kunt u [maken van een](https://powerbi.microsoft.com/pricing).

1. Power BI-uitvoer in Azure Stream Analytics (ASA) toevoegen.
   
   * U moet de instructies in [Azure Stream Analytics & Power BI: een dashboard realtime analyses voor realtime zichtbaarheid van gegevensstromen](../../stream-analytics/stream-analytics-power-bi-dashboard.md) voor het instellen van de uitvoer van uw Azure Stream Analytics-taak als uw Power BI-dashboard.
   * De ASA-query heeft drie uitvoer die **aircraftmonitor**, **aircraftalert**, en **flightsbyhour**. U kunt de query weergeven door te klikken op het tabblad query. Overeenkomt met elk van deze tabellen, moet u een uitvoer toevoegen aan ASA. Wanneer u de uitvoer van de eerste toevoegt (*bijvoorbeeld* **aircraftmonitor**) Zorg ervoor dat de **Uitvoeraliassen**, **gegevenssetnaam** en **tabelnaam** zijn hetzelfde (**aircraftmonitor**). Herhaal de stappen voor het toevoegen van outputs voor **aircraftalert**, en **flightsbyhour**. Nadat u alle drie uitvoertabellen en de ASA-taak gestart hebt toegevoegd, krijgt u een bevestigingsbericht (*bijvoorbeeld*, 'Starten Stream Analytics-taak maintenancesa02asapbi is voltooid').
2. Meld u aan bij [online in Power BI](http://www.powerbi.com)
   
   * In de sectie gegevenssets in mijn werkruimte in het linkerdeelvenster de ***GEGEVENSSET*** namen **aircraftmonitor**, **aircraftalert**, en **flightsbyhour** moet worden weergegeven. Dit is de streaminggegevens die u vanuit Azure Stream Analytics in de vorige stap gepusht. De gegevensset **flightsbyhour** wellicht niet op hetzelfde moment als de andere twee gegevenssets vanwege de aard van de SQL-query erachter weergegeven. Echter, het weergegeven na een uur.
   * Zorg ervoor dat de ***visualisaties*** deelvenster is geopend en aan de rechterkant van het scherm wordt weergegeven.
3. Wanneer u de gegevens die binnenkomen in Power BI hebt, kunt u beginnen de streaming-gegevens te visualiseren. Hieronder wordt een voorbeeld-dashboard met visualisaties die bepaalde hot pad vastgemaakt aan. U kunt andere dashboard tegels op basis van de juiste gegevenssets maken. Afhankelijk van hoe lang het uitvoeren van de gegevensgenerator van uw, kan de nummers op de visualisaties afwijken.

    ![Dashboardweergave](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

1. Hier volgen een aantal stappen voor het maken van een van de bovenstaande – tegels de 'vloot weergave van de vs Sensor 11. Drempelwaarde 48,26" tegel:
   
   * Klik op de gegevensset **aircraftmonitor** in het linkerdeelvenster gegevenssets sectie.
   * Klik op de **lijndiagram** pictogram.
   * Klik op **verwerkte** in de **velden** deelvenster zodat deze worden weergegeven onder 'As' de **visualisaties** deelvenster.
   * Klik op 's11' en ' s11\_waarschuwing ' zodat deze beide worden weergegeven onder "Waarden". Klik op het pijltje naast **s11** en **s11\_waarschuwing**, 'Sum' op 'Gemiddeld' wijzigen.
   * Klik op **opslaan** op de bovenkant en de naam van het rapport 'aircraftmonitor'. Het rapport met de naam 'aircraftmonitor' wordt weergegeven in de **rapporten** sectie het **Navigator** in het linkerdeelvenster.
   * Klik op de **pincode Visual** pictogram in de rechterbovenhoek van dit lijndiagram. Een venster 'Vastmaken aan Dashboard' kan weergegeven voor u een dashboard te kiezen. Selecteer 'Voorspeld onderhoud Demo' en klik op 'Pincode'.
   * Beweeg de muisaanwijzer over deze tegel op het dashboard, klikt u op het pictogram "edit" in de rechterbovenhoek te wijzigen van de titel 'vloot weergave van Sensor 11 vs. Drempelwaarde 48,26" en subtitel van de 'Gemiddelde via wagenpark na verloop van tijd'.

## <a name="how-to-delete-your-solution"></a>**Het verwijderen van uw oplossing**
Zorg ervoor dat u de gegevensgenerator van stopt wanneer de oplossing niet actief wordt gebruikt als de gegevensgenerator uitgevoerd wordt hogere kosten in rekening worden. Verwijder de oplossing als u dit niet gebruikt. Uw oplossing verwijdert, worden de onderdelen die zijn ingericht in uw abonnement wanneer u de oplossing hebt geïmplementeerd. Voor het verwijderen van de oplossing Klik op de oplossingsnaam van uw in het linkerpaneel van de oplossingssjabloon en klik op verwijderen.

## <a name="cost-estimation-tools"></a>**Schatting extra kosten**
De volgende twee hulpprogramma's zijn beschikbaar om te helpen u de totale kosten die betrokken zijn bij de voorspeld onderhoud in uw abonnement voor lucht oplossingssjabloon beter te begrijpen:

* [Microsoft Azure kosten Estimator Tool (online)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure kosten Estimator Tool (desktop)](http://www.microsoft.com/download/details.aspx?id=43376)

