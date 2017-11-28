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
ms.date: 03/15/2017
ms.author: fboylu
ms.openlocfilehash: 080618b844669cbea29a6a48c32e937705b06e3f
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Technische handleiding voor de sjabloon Cortana Intelligence-oplossing voor voorspeld onderhoud in ruimtevaart en andere bedrijven

>[!Important]
In dit artikel is afgeschaft. De discussie over voorspeld onderhoud momenteel is nog steeds relevant zijn, maar voor actuele informatie, raadpleegt u [overzicht van de oplossing voor zakelijke doelgroepen](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace).


Oplossingssjablonen zijn ontworpen om het proces van het bouwen van een demo E2E boven op de Cortana Intelligence Suite versnellen. Een geïmplementeerde sjabloon voorziet in uw abonnement met vereiste onderdelen van de Cortana Intelligence en vervolgens de relaties tussen deze twee wordt gemaakt. Deze ook seeding van de pijplijn gegevens met voorbeeldgegevens van een toepassing van de generator gegevens, die u downloadt en installeert op uw lokale computer nadat u de oplossingssjabloon implementeert. De gegevens van de generator hydrates de gegevens pijplijn en start het genereren van machine learning voorspellingen, die vervolgens kunnen worden weergegeven op het Power BI-dashboard.

Het implementatieproces begeleidt u bij verschillende stappen voor het instellen van de referenties van uw oplossing. Zorg ervoor dat u de referenties zoals de naam van de oplossing, gebruikersnaam en wachtwoord die u tijdens de implementatie opgeeft vastleggen. 


De doelstellingen van dit artikel zijn naar:
- Beschrijf de referentiearchitectuur en onderdelen die zijn ingericht in uw abonnement.
- Laten zien hoe de voorbeeldgegevens vervangen door uw eigen gegevens. 
- Hoe de oplossingssjabloon te wijzigen.  

> [!TIP]
> U kunt downloaden en afdrukken een [PDF-versie van dit artikel](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).
> 
> 

## <a name="overview"></a>Overzicht
![Architectuur voor voorspeld onderhoud](./media/cortana-analytics-technical-guide-predictive-maintenance/predictive-maintenance-architecture.png)

Wanneer u de oplossing implementeert, wordt deze geactiveerd Azure-services binnen de Cortana-Analytics Suite (inclusief Event Hub, Stream Analytics, HDInsight, Data Factory en Machine Learning). Het architectuurdiagram toont hoe de voorspeld onderhoud voor lucht oplossingssjabloon is samengesteld. U kunt deze services in de Azure-portal door erop te klikken in het oplossing sjabloon diagram gemaakt met de implementatie van de oplossing (met uitzondering van HDInsight, die is ingericht op aanvraag wanneer de bijbehorende pipeline-activiteiten vereist zijn voor het uitvoeren en onderzoeken verwijderd daarna).
Download een [volledige versie van het diagram](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

De volgende secties worden de onderdelen van de oplossing.

## <a name="data-source-and-ingestion"></a>Gegevensbron en opnamesnelheid
### <a name="synthetic-data-source"></a>Synthetische gegevensbron
Voor deze sjabloon wordt de gegevensbron waarmee gegenereerd vanuit een bureaubladtoepassing die u downloaden en lokaal uitvoeren na een geslaagde implementatie.

Selecteer het eerste knooppunt voorspeld onderhoud Gegevensgenerator, in het diagram van de sjabloon oplossing de instructies voor het downloaden en installeren van deze toepassing. De instructies hiervoor vindt u in de balk eigenschappen. Deze toepassing feeds de [Azure Event Hub](#azure-event-hub) service met gegevenspunten of gebeurtenissen, die worden gebruikt in de rest van de stroom van de oplossing. Deze gegevensbron is afgeleid van openbaar beschikbare gegevens uit de [NASA gegevensopslagplaats](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) met behulp van de [Turbofan Engine Degradation Simulation Data Set](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

De toepassing voor het genereren van gebeurtenis vult de Azure Event Hub alleen terwijl deze wordt uitgevoerd op uw computer.

### <a name="azure-event-hub"></a>Azure Event Hub
De [Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) -service is de ontvanger van de ingevoerde door de synthetische gegevensbron.

## <a name="data-preparation-and-analysis"></a>Voorbereiden van gegevens en analyse
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
Gebruik [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) voor near realtime analyses op de invoerstroom van het [Azure Event Hub](#azure-event-hub) service. U publiceert resultaten naar een [Power BI](https://powerbi.microsoft.com) dashboard en archiveren van alle onbewerkte binnenkomende gebeurtenissen naar de [Azure Storage](https://azure.microsoft.com/services/storage/) service later worden verwerkt door de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)service.

### <a name="hdinsight-custom-aggregation"></a>Aangepaste HDInsight-aggregatie
Voer [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts (gedirigeerd door Azure Data Factory) met HDInsight voor aggregaties op de onbewerkte gebeurtenissen gearchiveerd met de Azure Stream Analytics-service.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Maken van voorspellingen op de resterende levensduur (resterende Levensduur) van een bepaalde vliegtuigmotor met behulp van de invoer ontvangen met [Azure Machine Learning Service](https://azure.microsoft.com/services/machine-learning/) (gedirigeerd door Azure Data Factory). 

## <a name="data-publishing"></a>Gegevens publiceren
### <a name="azure-sql-database"></a>Azure SQL Database
Gebruik [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) voor het opslaan van de voorspellingen ontvangen door de service Azure Machine Learning, die vervolgens worden gebruikt in de [Power BI](https://powerbi.microsoft.com) dashboard.

## <a name="data-consumption"></a>Gegevensverbruik
### <a name="power-bi"></a>Power BI
Gebruik [Power BI](https://powerbi.microsoft.com) om weer te geven van een dashboard met aggregaties en waarschuwingen die worden geleverd door [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/), evenals resterende Levensduur voorspellingen opgeslagen in [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) die zijn geproduceerd met [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/).

## <a name="how-to-bring-in-your-own-data"></a>Hoe op te zetten in uw eigen gegevens
Deze sectie wordt beschreven hoe u uw eigen gegevens overbrengen naar Azure en welke gebieden moeten worden gewijzigd voor de gegevens die u in deze architectuur brengt.

Het lijkt onwaarschijnlijk dat uw gegevensset overeenkomt met de gegevensset die wordt gebruikt door de [Turbofan Engine Degradation Simulation Data Set](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) voor deze oplossingssjabloon gebruikt. Inzicht in uw gegevens en de vereisten zijn van cruciaal belang bij hoe het wijzigen van deze sjabloon wilt werken met uw eigen gegevens. 

De volgende secties worden de onderdelen van de sjabloon die moeten worden aangepast wanneer een nieuwe gegevensset wordt geïntroduceerd.

### <a name="azure-event-hub"></a>Azure Event Hub
Azure Event Hub is algemeen; gegevens kan worden geplaatst op de hub in CSV- of JSON-indeling. Geen speciale verwerking optreedt in de Azure Event Hub, maar het is belangrijk dat u begrijpt dat de gegevens die wordt ingevoerd.

Dit document beschrijft niet hoe u uw gegevens opnemen, maar u kunt eenvoudig verzenden gebeurtenissen of gegevens naar een Azure Event Hub met de Event Hub-API's.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
De Azure Stream Analytics-service gebruiken om uw bijna realtime analyses te lezen van gegevensstromen en uitvoeren van gegevens naar een willekeurig aantal bronnen.

Voor voorspeld onderhoud voor lucht oplossingssjabloon, de Azure Stream Analytics query bestaat uit vier sub-query's, elke query gebeurtenissen van de service Azure Event Hub met externe uitvoer naar vier verschillende locaties verbruikt. Deze uitvoer bestaan uit drie Power BI-gegevenssets en een Azure-opslaglocatie.

De Azure Stream Analytics query kan worden gevonden door:

* Verbinding maken met de Azure-portal
* Zoeken naar de Stream Analytics-taken ![Stream Analytics-pictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-stream-analytics.png) die zijn gegenereerd tijdens de oplossing is geïmplementeerd (*bijvoorbeeld*, **maintenancesa02asapbi** en **maintenancesa02asablob** voor de oplossing voor voorspeld onderhoud)
* Selecteren
  
  * ***INVOER*** invoer als de query wilt weergeven
  * ***QUERY*** om de query zelf weer te geven
  * ***LEVERT*** om de verschillende uitvoer weer te geven

Informatie over het samenstellen van Azure Stream Analytics query vindt u in de [Stream Analytics Query verwijzing](https://msdn.microsoft.com/library/azure/dn834998.aspx) op MSDN.

In deze oplossing uitvoergegevenssets de query's drie met bijna realtime analyses informatie over de gegevensstroom inkomende aan een Power BI-dashboard geleverd als onderdeel van deze oplossingssjabloon. Omdat er impliciete kennis over de indeling van de binnenkomende, moeten deze query's op basis van indeling van uw gegevens worden gewijzigd.

De query in de tweede Stream Analytics-taak **maintenancesa02asablob** levert gewoon alle [Event Hub](https://azure.microsoft.com/services/event-hubs/) gebeurtenissen die moeten worden [Azure Storage](https://azure.microsoft.com/services/storage/) en daarom geen wijziging ongeacht de indeling van uw gegevens is vereist omdat de informatie over de volledige gebeurtenis gestreamd naar opslag.

### <a name="azure-data-factory"></a>Azure Data Factory
De [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service ingedeeld het verkeer en de verwerking van gegevens. In de voorspeld onderhoud voor lucht oplossingssjabloon de gegevensfactory bestaat uit drie [pijplijnen](../../data-factory/v1/data-factory-create-pipelines.md) die verplaatsen en de gegevens met behulp van verschillende technologieën worden verwerkt.  Toegang tot uw data factory door het openen van het Data Factory-knooppunt onder aan de oplossing sjabloon diagram gemaakt met de implementatie van de oplossing. Fouten in de gegevenssets worden vanwege gegevensfactory geïmplementeerd voordat de gegevensgenerator werd gestart. Deze fouten kunnen worden genegeerd en niet voorkomen dat uw data factory werkt

![Data Factory-gegevensset fouten](./media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

In deze sectie worden de benodigde [pijplijnen](../../data-factory/v1/data-factory-create-pipelines.md) en [activiteiten](../../data-factory/v1/data-factory-create-pipelines.md) opgenomen in de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Hier volgt een diagramweergave van de oplossing.

![Azure Data Factory](./media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Twee van de pijplijnen van deze factory bevatten [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts die worden gebruikt voor het partitioneren en de gegevens totaliseren. Wanneer u hebt genoteerd, de scripts bevinden zich in de [Azure Storage](https://azure.microsoft.com/services/storage/) account gemaakt tijdens de installatie. De locatie is: maintenancesascript\\\\script\\\\hive\\ \\ (of https://[Your oplossing name].blob.core.windows.net/maintenancesascript).

Net als bij [Azure Stream Analytics](#azure-stream-analytics-1) query's de [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts impliciete kennis over de indeling van de binnenkomende hebben en moet worden gewijzigd op basis van indeling van uw gegevens.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*
Dit [pijplijn](../../data-factory/v1/data-factory-create-pipelines.md) bevat een enkele activiteit - een [HDInsightHive](../../data-factory/v1/data-factory-hive-activity.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die wordt uitgevoerd een [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor het partitioneren van de gegevens in plaatsen [Azure Storage](https://azure.microsoft.com/services/storage/) tijdens de [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) taak.

De [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor deze taak partitionering is ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
Dit [pijplijn](../../data-factory/v1/data-factory-create-pipelines.md) bevat meerdere activiteiten waarvan eindresultaat de scored voorspellingen is van het [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenten die zijn gekoppeld aan deze oplossingssjabloon.

Activiteiten die zijn:

* [HDInsightHive](../../data-factory/v1/data-factory-hive-activity.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die wordt uitgevoerd een [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor het uitvoeren van de aggregaties en functie-engineering nodig zijn voor de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren.
  De [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor deze taak partitionering is ***PrepareMLInput.hql***.
* [Kopiëren](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die wordt verplaatst van de resultaten van de [HDInsightHive](../../data-factory/v1/data-factory-hive-activity.md) activiteit aan een enkele [Azure Storage](https://azure.microsoft.com/services/storage/) blob toegankelijk is voor de [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) de activiteit.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) activiteit aanroepen de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experiment met resultaten in een enkel plaatsen [Azure Storage](https://azure.microsoft.com/services/storage/) blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
Dit [pijplijn](../../data-factory/v1/data-factory-create-pipelines.md) bevat een enkele activiteit - een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die wordt verplaatst van de resultaten van de [Azure Machine Learning](#azure-machine-learning) experimenteren uit de  ***MLScoringPipeline*** naar de [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) ingericht als onderdeel van de installatie van de sjabloon oplossing.

### <a name="azure-machine-learning"></a>Azure Machine Learning
De [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren gebruikt voor deze oplossingssjabloon biedt de resterende nuttig Levensduur van een vliegtuigmotor. Het experiment is specifiek voor de gegevensset die is verbruikt en wijziging vereist of vervanging die specifiek zijn voor de gegevens die zijn ingediend.

Zie voor meer informatie over hoe het Azure Machine Learning-experiment is gemaakt, [voorspeld onderhoud: stap 1 van 3, het voorbereiden van gegevens en functie-engineering](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>Voortgang van de monitor
Zodra de Generator van gegevens wordt gestart, de pijplijn begint met het dehydrate en de verschillende onderdelen van uw oplossing start bij de start in de actie volgende de opdrachten die zijn uitgegeven door de data factory. Er zijn twee manieren voor het bewaken van de pijplijn.

1. Een van de Stream Analytics-taken schrijft de onbewerkte binnenkomende gegevens naar blob storage. Als u op voor Blob Storage-onderdeel van uw oplossing van het scherm u is de oplossing geïmplementeerd en klik vervolgens op openen in het rechterpaneel, duurt het voor de [Azure-portal](https://portal.azure.com/). Klik op Blobs. In het volgende deelvenster ziet u een lijst van Containers. Klik op **maintenancesadata**. In de volgende deelvenster is de **rawdata** map. In de map rawdata zijn mappen met namen zoals uur = 17 en uur = 18. De aanwezigheid van deze mappen geeft de onbewerkte gegevens worden gegenereerd op uw computer en opgeslagen in blob storage. U ziet de csv-bestanden met beperkte grootte in MB in deze mappen.
2. De laatste stap van de pijplijn is om gegevens te schrijven (bijvoorbeeld de voorspellingen van machine learning) in SQL-Database. U moet wellicht wacht maximaal drie uur voor de gegevens worden weergegeven in de SQL-Database. Een manier om te controleren hoeveel gegevens zijn beschikbaar in de SQL-Database, is via de [Azure-portal](https://portal.azure.com/). Zoek in het linkerdeelvenster SQL-DATABASES ![SQL pictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-SQL-databases.png) en klik erop. Ga vervolgens naar de database **pmaintenancedb** en klik erop. Klik in de volgende pagina aan de onderkant op beheren
   
    ![Pictogram beheren](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-manage.png).
   
    U kunt hier, klik op nieuwe Query en de query voor het aantal rijen (bijvoorbeeld select count(*) van PMResult). Als uw database groeit, wordt het aantal rijen in de tabel moet verhogen.

## <a name="power-bi-dashboard"></a>Power BI Dashboard

Een Power BI-dashboard instellen voor het visualiseren van uw Azure Stream Analytics-gegevens (hot pad) en de resultaten van de batch voorspelling van Azure machine learning-(koude pad).

### <a name="set-up-the-cold-path-dashboard"></a>Instellen van het dashboard koude pad
Het doel is in de pijplijn koude pad-gegevens ophalen van de voorspellende resterende Levensduur (resterende levensduur) van elke vliegtuigmotor zodra een vlucht (cyclus) is voltooid. Het resultaat van de voorspelling wordt elke drie uur voor het voorspellen van de vliegtuigmotoren die een vlucht voltooid tijdens de afgelopen drie uur bijgewerkt.

Power BI maakt verbinding met een Azure SQL database als de gegevensbron, waar de voorspelling resultaten worden opgeslagen. Opmerking: 1) op voor het implementeren van uw oplossing een voorspelling verschijnt in de database binnen drie uur.
De pbix-bestand met het downloaden van de codegenerator bevat enkele seedgegevens, zodat u meteen de Power BI-dashboard kunt maken. 2) in deze stap wordt de vereiste is om te downloaden en installeren van de gratis software [van Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

De volgende stappen helpen u op verbinding maken tussen de pbix-bestand en de SQL-Database die op het moment van implementatie van de oplossing met gegevens (bijvoorbeeld voorspelling resultaten) voor visualisatie is geactiveerd.

1. De databasereferenties ophalen.
   
   U moet **servernaam, databasenaam, gebruikersnaam en wachtwoord van de database** voordat u doorgaat naar de volgende stappen. Hier volgen de stappen om u te helpen ze te zoeken.
   
   * Eenmaal **'Azure SQL Database'** op uw oplossingssjabloon diagram wordt groen, klik hierop en klik op **'Openen'**.
   * Hier ziet u een nieuw tabblad/browservenster waarin de Azure portal-pagina. Klik op **'Resourcegroepen'** in het linkerdeelvenster.
   * Selecteer het abonnement dat u gebruikt voor het implementeren van de oplossing en selecteer vervolgens **' YourSolutionName\_ResourceGroup'**.
   * Klik in de nieuwe pop uit het Configuratiescherm op de ![SQL pictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-sql.png) pictogram voor toegang tot uw database. De naam van uw database is naast dit pictogram (bijvoorbeeld **'pmaintenancedb'**), en de **databaseservernaam** wordt vermeld onder de naameigenschap van de Server en moet er ongeveer uitzien  **YourSoutionName.database.windows.net**.
   * Uw database **gebruikersnaam** en **wachtwoord** zijn hetzelfde als de gebruikersnaam en wachtwoord eerder vastgelegd tijdens de implementatie van de oplossing.
2. De gegevensbron van het rapportbestand koude pad bijwerken met Power BI Desktop.
   
   * In de map waar u hebt gedownload en het bestand Generator zijn uitgepakt, dubbelklikt u op de **PowerBI\\PredictiveMaintenanceAerospace.pbix** bestand. Als u eventuele waarschuwingsberichten ziet wanneer u het bestand opent, negeert u deze. Klik boven aan het bestand **query's bewerken**.
     
     ![Query's bewerken](./media/cortana-analytics-technical-guide-predictive-maintenance/edit-queries.png)
   * Ziet u twee tabellen **RemainingUsefulLife** en **PMResult**. Selecteer de eerste tabel en klik op ![Query Instellingenpictogram](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-query-settings.png) naast **'Source'** onder **'Toegepast stappen'** aan de rechterkant **Query-instellingen** Configuratiescherm. Negeer eventuele waarschuwingsberichten die worden weergegeven.
   * Vervang in het pop uit venster **'Server'** en **'Database'** met uw eigen namen van de server en database en klik vervolgens op **'OK'**. Voor de servernaam van de, zorg ervoor dat u de poort 1433 opgeven (**YourSoutionName.database.windows.net, 1433**). Laat het veld Database als **pmaintenancedb**. Negeer de waarschuwingsberichten die worden weergegeven op het scherm.
   * In de volgende pop uit venster ziet u twee opties in het linkerdeelvenster (**Windows** en **Database**). Klik op **'Database'**, vul uw **'Username'** en **'Password'** (dit is de gebruikersnaam en het wachtwoord dat u hebt ingevoerd wanneer u eerst de oplossing geïmplementeerd en een Azure SQL database gemaakt). In ***Selecteer welk gegevensniveau toe te passen van deze instellingen***, niveau databaseoptie controleren. Klik vervolgens op **'Connect'**.
   * Klik op de tweede tabel **PMResult** klikt u vervolgens op ![pictogram Navigatie](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-navigation.png) naast **'Source'** onder **'Toegepast stappen'** aan de rechterkant **Query-instellingen** deelvenster, en de namen van de server en database zoals in de bovenstaande stappen bijwerken en klik op OK.
   * Als u terug naar de vorige pagina bent geleid, sluit het venster. Er wordt een bericht weergegeven: klik **toepassen**. Klik ten slotte de **opslaan** knop de wijzigingen wilt opslaan. Uw Power BI-bestand is nu ingesteld voor verbinding met de server. Als uw visualisaties leeg zijn, zorg er dan voor dat u de selecties op de visualisaties niet alle gegevens visualiseren door te klikken op het pictogram gum op de rechterbovenhoek van de legenda's wissen. Gebruik de knop Vernieuwen in overeenstemming met nieuwe gegevens op de visualisaties. Aanvankelijk ziet u alleen de seedgegevens op uw visualisaties omdat de gegevensfactory is gepland voor elke drie uur vernieuwen. U ziet na drie uur nieuwe voorspellingen doorgevoerd in uw visualisaties Wanneer u de gegevens vernieuwen.
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
De volgende stappen helpen u hoe u de gegevensuitvoer van de Stream Analytics-taken die zijn gegenereerd op het moment van implementatie van de oplossing te visualiseren. Een [Power BI online](http://www.powerbi.com/) account is vereist de volgende stappen uit te voeren. Als u geen account hebt, kunt u [maken van een](https://powerbi.microsoft.com/pricing).

1. Power BI-uitvoer in Azure Stream Analytics (ASA) toevoegen.
   
   * U moet de instructies in [Azure Stream Analytics & Power BI: een dashboard met analytische voor realtime zichtbaarheid van gegevensstromen](../../stream-analytics/stream-analytics-power-bi-dashboard.md) voor het instellen van de uitvoer van uw Azure Stream Analytics-taak als uw Power BI-dashboard.
   * De ASA-query heeft drie uitvoer die **aircraftmonitor**, **aircraftalert**, en **flightsbyhour**. U kunt de query weergeven door te klikken op het tabblad query. Overeenkomt met elk van deze tabellen, moet u een uitvoer toevoegen aan ASA. Wanneer u de uitvoer van de eerste toevoegt (**aircraftmonitor**) Zorg ervoor dat de **Uitvoeraliassen**, **gegevenssetnaam** en **tabelnaam** zijn dezelfde (**aircraftmonitor**). Herhaal de stappen voor het toevoegen van outputs voor **aircraftalert**, en **flightsbyhour**. Nadat u alle drie uitvoertabellen en de ASA-taak gestart hebt toegevoegd, krijgt u een bevestigingsbericht ('vanaf Stream Analytics-taak maintenancesa02asapbi is voltooid').
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
   * Klik op **opslaan** op de bovenkant en de naam van het rapport 'aircraftmonitor'. Het rapport met de naam 'aircraftmonitor' wordt weergegeven de **rapporten** sectie het **Navigator** in het linkerdeelvenster.
   * Klik op de **pincode Visual** pictogram in de rechterbovenhoek van dit lijndiagram. Een venster 'Vastmaken aan Dashboard' kan weergegeven voor u een dashboard te kiezen. Selecteer 'Voorspeld onderhoud Demo' en klik op 'Pincode'.
   * Beweeg de muisaanwijzer over deze tegel op het dashboard, klikt u op het pictogram "edit" in de rechterbovenhoek te wijzigen van de titel 'vloot weergave van Sensor 11 vs. Drempelwaarde 48,26" en subtitel van de"Gemiddelde via wagenpark gedurende een bepaalde periode."

## <a name="delete-your-solution"></a>Uw oplossing verwijderen
Zorg ervoor dat u de gegevensgenerator van stopt wanneer de oplossing niet actief wordt gebruikt als de gegevensgenerator uitgevoerd wordt hogere kosten in rekening worden. Verwijder de oplossing als u dit niet gebruikt. Uw oplossing verwijdert, worden de onderdelen die zijn ingericht in uw abonnement wanneer u de oplossing hebt geïmplementeerd. Klik op de oplossingsnaam van uw in het linkerdeelvenster van de oplossingssjabloon voor het verwijderen van de oplossing en klik vervolgens op **verwijderen**.

## <a name="cost-estimation-tools"></a>Schatting extra kosten
De volgende twee hulpprogramma's zijn beschikbaar om te helpen u de totale kosten die betrokken zijn bij de voorspeld onderhoud in uw abonnement voor lucht oplossingssjabloon beter te begrijpen:

* [Microsoft Azure kosten Estimator Tool (online)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure kosten Estimator Tool (desktop)](http://www.microsoft.com/download/details.aspx?id=43376)

