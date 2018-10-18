---
title: Vraagprognose in energie technische handleiding
description: Een technische handleiding voor de sjabloon met Microsoft Cortana Intelligence-oplossing voor vraagprognose in energie.
services: machine-learning
author: garyericson
manager: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/16/2016
ms.author: garye
ms.openlocfilehash: 8ff5c52b324c95bb48de0f9bbb1011ede737efb0
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49387658"
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Technische handleiding aan de sjabloon Cortana Intelligence-oplossing voor vraagprognose in energie
## <a name="overview"></a>**Overzicht**
Sjablonen voor oplossingen zijn ontworpen om het proces van het bouwen van een demo E2E boven op Cortana Intelligence Suite te versnellen. Een geïmplementeerde sjabloon bepalingen van uw abonnement met de benodigde Cortana Intelligence-onderdeel en de relaties tussen bouwen. Deze ook de seeding van de gegevenspijplijn met voorbeeldgegevens ophalen gegenereerd op basis van een toepassing simuleren. De gegevenssimulator van de koppeling downloaden en installeren op uw lokale computer, Raadpleeg het Leesmij-bestand voor instructies over het gebruik van de simulator. Gegevens die zijn gegenereerd op basis van de simulator hydrates de gegevenspijplijn en start genereren voorspelling machine learning, die vervolgens kan worden gevisualiseerd in Power BI-dashboard.

Sjabloon van de oplossing kan worden gevonden [hier](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1)

Het implementatieproces leidt u door de verschillende stappen voor het instellen van de referenties van uw oplossing. Zorg ervoor dat u deze referenties, zoals de naam van oplossing, gebruikersnaam en wachtwoord die u tijdens de implementatie opgeeft vastleggen.

Het doel van dit document is om uit te leggen van de referentie-architectuur en de verschillende onderdelen die zijn ingericht in uw abonnement als onderdeel van deze oplossingssjabloon. Het document ook vertelt hoe u de voorbeeldgegevens, vervangen door echte gegevens van uw eigen insights/voorspellingen op basis van u gewonnen gegevens zien. Bovendien de presentaties document over de onderdelen van de sjabloon van de oplossing die worden gewijzigd moet als u wilt aanpassen van de oplossing met uw eigen gegevens. Instructies voor het bouwen van de Power BI-dashboard voor deze oplossingssjabloon worden geleverd aan het einde.

## <a name="details"></a>**Details**
![](media/cortana-analytics-technical-guide-demand-forecast/ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Architectuur uitgelegd
Wanneer de oplossing wordt geïmplementeerd, verschillende Azure-services binnen de Cortana Analytics Suite worden geactiveerd (dat wil zeggen, Event Hub, Stream Analytics, HDInsight, de Data Factory, Machine Learning, *enzovoort*). Het architectuurdiagram ziet, op hoog niveau, hoe de vraagprognose voor energie-oplossingssjabloon is samengesteld uit de end-to-end. U kunt deze services onderzoeken door erop te klikken op het oplossingsdiagram van de sjabloon die zijn gemaakt met de implementatie van de oplossing. De volgende secties beschrijven elk gegeven.

## <a name="data-source-and-ingestion"></a>**De gegevensbron en opnamesnelheid**
### <a name="synthetic-data-source"></a>Synthetische gegevensbron
De gegevensbron die wordt gebruikt wordt voor deze sjabloon wordt gegenereerd vanuit een bureaubladtoepassing die u downloaden en lokaal uitvoeren na een geslaagde implementatie. U vindt de instructies voor het downloaden en installeren van deze toepassing in de Eigenschappenbalk wanneer u het eerste knooppunt energie voorspellen Gegevenssimulator aangeroepen op het oplossingsdiagram van de sjabloon selecteert. Deze toepassing feeds de [Azure Event Hub](#azure-event-hub) service met gegevenspunten of gebeurtenissen die worden gebruikt in de rest van de oplossingsstroom.

De toepassing voor het genereren van gebeurtenis vult de Azure Event Hub alleen terwijl deze wordt uitgevoerd op uw computer.

### <a name="azure-event-hub"></a>Azure Event Hub
De [Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) -service is de ontvanger van de invoer die is opgegeven door de synthetische gegevensbron beschreven.

## <a name="data-preparation-and-analysis"></a>**Voorbereiden van gegevens en analyse**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
De [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) service wordt gebruikt voor bijna realtime analyses over de invoerstroom uit de [Azure Event Hub](#azure-event-hub) service en publiceren van resultaten op een [Power BI](https://powerbi.microsoft.com)dashboard, evenals het archiveren van alle ruwe inkomende gebeurtenissen naar de [Azure Storage](https://azure.microsoft.com/services/storage/) service later worden verwerkt door de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service.

### <a name="hdinsight-custom-aggregation"></a>Aangepaste HDInsight-aggregatie
De Azure HDInsight-service wordt gebruikt om uit te voeren [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts (gecoördineerd door Azure Data Factory) voor het leveren van aggregaties van de ruwe gebeurtenissen die zijn gearchiveerd met behulp van de Azure Stream Analytics-service.

### <a name="azure-machine-learning"></a>Azure Machine Learning
De [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -service (gecoördineerd door Azure Data Factory) wordt gebruikt om te maken van prognoses op toekomstig energieverbruik van een bepaalde regio op basis van de ontvangen invoer.

## <a name="data-publishing"></a>**Gegevens publiceren**
### <a name="azure-sql-database-service"></a>Azure SQL Database Service
De [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) service wordt gebruikt om op te slaan (beheerd door Azure Data Factory) de voorspellingen ontvangen door de Azure Machine Learning-service die wordt gebruikt in de [Power BI](https://powerbi.microsoft.com) dashboard.

## <a name="data-consumption"></a>**Gegevensverbruik**
### <a name="power-bi"></a>Power BI
De [Power BI](https://powerbi.microsoft.com) service wordt gebruikt om weer te geven van een dashboard met aggregaties geleverd door de [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) service, evenals vraag voorspellen resultaten die zijn opgeslagen in [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) die zijn gemaakt met behulp van de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) service. Raadpleeg de volgende sectie voor instructies over het bouwen van de Power BI-dashboard voor deze oplossingssjabloon.

## <a name="how-to-bring-in-your-own-data"></a>**Hoe u uw eigen gegevens**
In deze sectie wordt beschreven hoe u uw eigen gegevens overbrengen naar Azure, en welke gebieden voor de gegevens die u in deze architectuur brengt wijzigingen nodig.

Is het onwaarschijnlijk dat de gegevensset die wordt gebruikt voor deze oplossingssjabloon er naar een gegevensset die u levert gezocht wordt. Inzicht krijgen in uw gegevens en de vereisten zijn essentieel in hoe u deze sjabloon wilt werken met uw eigen gegevens wijzigen. Als nog niet bekend met Azure Machine Learning-service, krijgt u een inleiding tot het met behulp van het voorbeeld in [over het maken van uw eerste experiment](machine-learning/studio/create-experiment.md).

De volgende secties worden besproken in de secties van de sjabloon die wijzigingen vereist wanneer een nieuwe gegevensset wordt geïntroduceerd.

### <a name="azure-event-hub"></a>Azure Event Hub
De [Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) -service is algemeen, zodat gegevens kan op de hub in CSV of JSON-indeling worden geplaatst. Er is geen speciale verwerking vindt plaats in de Azure Event Hub, maar het is belangrijk dat u begrijpt dat de gegevens die is ingevoerd.

Dit document wordt niet beschreven hoe u uw gegevens opnemen, maar een gebeurtenissen of gegevens naar een Azure Event Hub, kunt u eenvoudig kunt verzenden met behulp van de [Event Hub-API](event-hubs/event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
De [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) service wordt gebruikt voor bijna realtime analyses door te lezen uit gegevensstromen en uitvoeren van gegevens naar een willekeurig aantal bronnen.

Voor de vraagprognose voor energie-oplossingssjabloon, wordt de Azure Stream Analytics-query bestaat uit twee subquery's, elk gebeurtenis van de Azure Event Hub-service gebruiken als invoer en uitvoer die naar twee verschillende locaties. Deze uitvoer bestaan uit een Power BI-gegevensset en een Azure-opslaglocatie.

De [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) query kan worden gevonden door:

* Meld u aan bij de [Azure-portal](https://portal.azure.com/)
* Zoeken naar de stream analytics-taken ![](media/cortana-analytics-technical-guide-demand-forecast/icon-stream-analytics.png) die zijn gegenereerd wanneer de oplossing is geïmplementeerd. Eén meter is voor het pushen van gegevens naar blob-opslag (bijvoorbeeld mytest1streaming432822asablob) en een andere voor het pushen van gegevens naar Power BI (bijvoorbeeld mytest1streaming432822asapbi).
* Selecteren

  * ***INVOER*** om weer te geven van de query invoeren
  * ***QUERY*** om de query zelf weer te geven
  * ***UITVOER*** om de verschillende uitvoer weer te geven

Informatie over het samenstellen van de Azure Stream Analytics-query kan worden gevonden de [Stream Analytics Query verwijzing](https://msdn.microsoft.com/library/azure/dn834998.aspx) op MSDN.

In deze oplossing krijgt u de Azure Stream Analytics-taak die gegevensset met bijna realtime analyses informatie over de gegevensstroom inkomende aan een Power BI-dashboard uitvoert als onderdeel van deze oplossingssjabloon. Omdat er impliciete kennis op over de indeling van de binnenkomende, moet deze query's worden gewijzigd op basis van de gegevensindeling van uw.

De andere Azure Stream Analytics-taak voert alle [Event Hub](https://azure.microsoft.com/services/event-hubs/) gebeurtenissen naar [Azure Storage](https://azure.microsoft.com/services/storage/) en kan daarom geen wijziging, ongeacht de gegevensindeling van uw is vereist omdat de informatie over de volledige gebeurtenis worden gestreamd naar opslag.

### <a name="azure-data-factory"></a>Azure Data Factory
De [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) coördineert service de verplaatsing en verwerking van gegevens. In de on-Demand prognose voor energie-oplossingssjabloon de data factory bestaat uit 12 [pijplijnen](data-factory/concepts-pipelines-activities.md) die verplaatsen en verwerken van de gegevens met behulp van verschillende technologieën.

  U kunt toegang tot uw data factory door het openen van de Data Factory-knooppunt aan de onderkant van het oplossingsdiagram van de sjabloon die zijn gemaakt met de implementatie van de oplossing. U ziet de data factory in Azure portal. Als u fouten onder uw gegevenssets ziet, kunt u deze negeren als ze het gevolg zijn van data factory worden geïmplementeerd voordat u de gegevensgenerator is gestart. Deze fouten doen niet voorkomen dat uw data factory werkt.

Deze sectie wordt beschreven voor de benodigde [pijplijnen](data-factory/concepts-pipelines-activities.md) en [activiteiten](data-factory/concepts-pipelines-activities.md) die deel uitmaken van de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Volgende afbeelding is de diagramweergave van de oplossing:

![](media/cortana-analytics-technical-guide-demand-forecast/ADF2.png)

Vijf van de pijplijnen van deze factory bevat [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts die worden gebruikt voor het partitioneren en de gegevens worden geaggregeerd. Wanneer die u hebt genoteerd, de scripts bevinden zich in de [Azure Storage](https://azure.microsoft.com/services/storage/) account is gemaakt tijdens de installatie. De locatie is: demandforecasting\\\\script\\\\hive\\ \\ (of https://[Your oplossing name].blob.core.windows.net/demandforecasting).

Vergelijkbaar met de [Azure Stream Analytics](#azure-stream-analytics-1) query's, de [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts impliciete kennis op over de indeling van de binnenkomende, deze query's zou moeten worden gewijzigd op basis van uw gegevens formatteren en [functie-engineering](machine-learning/team-data-science-process/create-features.md) vereisten.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat één activiteit: een [HDInsightHive](data-factory/transform-data-using-hadoop-hive.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die wordt uitgevoerd een [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) het script op geaggregeerde gestreamde in vraaggegevens elke 10 seconden in onderstation instellen op elk uur regioniveau en plaats in [Azure Storage](https://azure.microsoft.com/services/storage/) via de Azure Stream Analytics-taak.

De [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor deze taak partitioneren is ***AggregateDemandRegion1Hr.hql***

#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat twee activiteiten:

* [HDInsightHive](data-factory/transform-data-using-hadoop-hive.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die een Hive-script voor het per uur gegevens van geschiedenis van aanvraag in onderstation instellen op elk uur regioniveau aggregeren en plaatsen in Azure Storage tijdens de Azure Stream wordt uitgevoerd Analytics-taak
* [Kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die de samengevoegde gegevens uit Azure Storage-blob verplaatst naar de Azure SQL-Database die is ingericht als onderdeel van de installatie van de sjabloon oplossing.

De [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) voor deze taak is een script ***AggregateDemandHistoryRegion.hql***.

#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*
Deze [pijplijnen](data-factory/concepts-pipelines-activities.md) bevatten verschillende activiteiten en waarvan de beoordeelde voorspellingen op basis van de Azure Machine Learning-experiment die zijn gekoppeld aan deze oplossingssjabloon wordt. Ze zijn bijna identiek, behalve in elk van deze verwerkt alleen de andere regio, wordt die werd uitgevoerd door verschillende RegionID doorgegeven in de pijplijn ADF en het hive-script voor elke regio.  
De activiteiten die zijn opgenomen in deze pijplijn zijn:

* [HDInsightHive](data-factory/transform-data-using-hadoop-hive.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die een Hive-script uit te voeren aggregaties en functie-engineering die nodig zijn voor de Azure Machine Learning-experiment wordt uitgevoerd. Het Hive-scripts voor deze taak worden respectieve ***PrepareMLInputRegionX.hql***.
* [Kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die wordt verplaatst van de resultaten van de [HDInsightHive](data-factory/transform-data-using-hadoop-hive.md) activiteit aan een enkele Azure Storage-blob die toegang door de [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) activiteit.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) activiteit die de Azure Machine Learning-experiment, wat resulteert in de resultaten worden opgeslagen in een enkele Azure Storage-blob worden aangeroepen.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat één activiteit: een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die de resultaten van de Azure Machine Learning-experiment van de desbetreffende verplaatst ***MLScoringRegionXPipeline***met de Azure SQL-Database die is ingericht als onderdeel van de installatie van de sjabloon oplossing.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat één activiteit: een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die de gegevens van de samengevoegde doorlopende aanvraag van verplaatst ***LoadHistoryDemandDataPipeline*** tot de Azure SQL De database die is ingericht als onderdeel van de installatie van de sjabloon oplossing.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat één activiteit: een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die wordt verplaatst van de referentiegegevens van regio/onderstation/Topologygeo die zijn geüpload naar Azure Storage-blob als onderdeel van de oplossingssjabloon installatie met de Azure SQL-Database die is ingericht als onderdeel van de installatie van de sjabloon oplossing.

### <a name="azure-machine-learning"></a>Azure Machine Learning
De [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren gebruikt voor deze oplossingssjabloon de voorspelling van de vraag van de regio biedt. Het experiment is specifiek voor de gegevensset verbruikt en daarom vereist wijziging of vervanging die specifiek zijn voor de gegevens die wordt voorgelegd aan.

## <a name="monitor-progress"></a>**De voortgang van de**
Zodra de Gegevensgenerator wordt gestart, wordt de pijplijn wordt gehydrateerd ophalen en de verschillende onderdelen van uw oplossing start starten in actie volgende de opdrachten die zijn uitgegeven door de Data Factory. Er zijn twee manieren kunt u de pijplijn bewaken.

1. Controleer de gegevens uit Azure Blob Storage.

    Een van de Stream Analytics-taken schrijft de onbewerkte binnenkomende gegevens naar blob-opslag. Als u klikt op **Azure Blob Storage** onderdeel van uw oplossing van het scherm dat u de oplossing is geïmplementeerd en klik vervolgens op **Open** in het rechterdeelvenster het, gaat u naar de [Azure Portal](https://portal.azure.com). Klik op **Blobs**. In het volgende deelvenster ziet u een lijst met Containers. Klik op **"energysadata"**. In het volgende deelvenster ziet u de **"demandongoing"** map. In de map rawdata ziet u de mappen met namen, zoals datum = 28-01-2016 enzovoort. Als u deze mappen ziet, betekent dit dat de onbewerkte gegevens met succes is gegenereerd op uw computer en die zijn opgeslagen in blob-opslag. Hier ziet u bestanden die u beperkte grootte in MB in deze mappen hebt.
2. Controleer de gegevens uit een Azure SQL Database.

    De laatste stap van de pijplijn is het schrijven van gegevens (bijvoorbeeld voorspellingen op basis van machine learning) in SQL-Database. Mogelijk moet wachten maximaal twee uur de gegevens worden weergegeven in de SQL-Database. Eén manier om te controleren hoeveel gegevens zijn beschikbaar in uw SQL-Database is via [Azure-portal](https://portal.azure.com/). Zoek in het linkerdeelvenster, SQL-DATABASES![](media/cortana-analytics-technical-guide-demand-forecast/SQLicon2.png) en klik erop. Zoek uw database (dat wil zeggen demo123456db) en vervolgens klikt u op. Op de volgende pagina onder **'Verbinding maken met uw database'** sectie, klikt u op **"Run Transact-SQL-query's op basis van uw SQL-database"**.

    Hier kunt u klikken op nieuwe Query en query's uitvoeren voor het aantal rijen (bijvoorbeeld: ' select count(*) van DemandRealHourly) ' als uw database groeit, het aantal rijen in de tabel moet vergroten.)
3. Controleer de gegevens van Power BI-dashboard.

    U kunt Power BI-dashboard voor snelpad instellen voor het bewaken van de onbewerkte binnenkomende gegevens. Volg de instructies in de sectie 'Power BI-Dashboard'.

## <a name="power-bi-dashboard"></a>**Power BI-Dashboard**
### <a name="overview"></a>Overzicht
In deze sectie wordt beschreven hoe u Power BI-dashboard te visualiseren van uw realtime gegevens van Azure stream analytics (dynamisch pad), evenals resultaten van Azure machine learning-(koud pad) prognose instellen.

### <a name="setup-hot-path-dashboard"></a>Snelpad Dashboard instellen
De volgende stappen helpen u over het visualiseren van realtime-gegevens-uitvoer van Stream Analytics-taken die zijn gegenereerd op het moment van implementatie van de oplossing. Een [Power BI online](http://www.powerbi.com/) account is vereist om de volgende stappen uitvoeren. Als u geen account hebt, kunt u [maakt u er een](https://powerbi.microsoft.com/pricing).

1. Power BI-uitvoer in Azure Stream Analytics (ASA) toevoegen.

   * Moet u de instructies in [Azure Stream Analytics en Power BI: een realtime analytics-dashboard voor realtime zichtbaarheid van streaminggegevens](stream-analytics/stream-analytics-power-bi-dashboard.md) voor het instellen van de uitvoer van uw Azure Stream Analytics-taak als uw Power BI-dashboard .
   * Ga naar de stream analytics-taak in uw [Azure-portal](https://portal.azure.com). De naam van de taak moet zijn: YourSolutionName + "streamingjob" + willekeurig getal + "asapbi' (dat wil zeggen demostreamingjob123456asapbi).
   * Een Power BI-uitvoer voor de ASA-taak toevoegen. Stel de **uitvoer Alias** als **'PBIoutput'**. Stel uw **gegevenssetnaam** en **tabelnaam** als **'EnergyStreamData'**. Nadat u de uitvoer hebt toegevoegd, klikt u op **'Start'** aan de onderkant van de pagina om de Stream Analytics-taak te starten. U moet een bevestigingsbericht wordt weergegeven (bijvoorbeeld ' eerste stream analytics-taak myteststreamingjob12345asablob is voltooid').
2. Meld u aan bij [Power BI online](http://www.powerbi.com)

   * In het linkerdeelvenster, gegevenssets sectie in mijn werkruimte, moet u een nieuwe gegevensset weergegeven in het linkerdeelvenster van Power BI zien. Dit is de streaminggegevens die u van Azure Stream Analytics in de vorige stap hebt gepusht.
   * Zorg ervoor dat de ***visualisaties*** deelvenster is geopend en wordt weergegeven aan de rechterkant van het scherm.
3. Maak de tegel "Vraag door Timestamp":

   * Klik op de gegevensset **'EnergyStreamData'** in de sectie van de gegevenssets in het linkerdeelvenster.
   * Klik op **'Lijndiagram'** pictogram ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic8.png).
   * Klik op 'EnergyStreamData' in **velden** deelvenster.
   * Klik op **"Timestamp"** en controleer of deze wordt weergegeven onder 'As'. Klik op **"Laden"** en controleer of deze wordt weergegeven onder 'Values'.
   * Klik op **opslaan** op de boven- en de naam van het rapport als 'EnergyStreamDataReport'. Het rapport met de naam 'EnergyStreamDataReport' wordt weergegeven in de sectie rapporten in het navigatievenster links.
   * Klik op **"Pincode Visual"** ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) pictogram in de rechterbovenhoek van dit lijndiagram, een venster 'Vastmaken aan Dashboard' mogelijk weergegeven waarin u kiest een dashboard. Selecteer 'EnergyStreamDataReport' en klik vervolgens op 'Pincode'.
   * Beweeg de muisaanwijzer over deze tegel op het dashboard, klikt u op 'bewerken' pictogram in de rechterbovenhoek om te wijzigen van de titel ervan als "Vraag door Timestamp"
4. Andere dashboardtegels op basis van de juiste gegevenssets maken. De weergave van het laatste dashboard: ![](media/cortana-analytics-technical-guide-demand-forecast/PBIFullScreen.png)

### <a name="setup-cold-path-dashboard"></a>Setup van koude pad-Dashboard
In pijplijn koude pad, wordt het essentiële doel is om op te halen van de vraagprognose van elke regio. Power BI maakt verbinding met een Azure SQL database als de gegevensbron, waar de voorspellingsresultaten worden opgeslagen.

> [!NOTE]
> 1) Het duurt enkele uren voor het verzamelen van genoeg voorspelde resultaten voor het dashboard. U wordt aangeraden dat u start dit proces 2-3 uur nadat u de gegevensgenerator lunch. 2) in deze stap is het vereiste onderdeel is om te downloaden en installeren van de gratis software [Power BI desktop](https://powerbi.microsoft.com/desktop).
>
>

1. Referenties van de database ophalen.

   U moet **database-servernaam, databasenaam, gebruikersnaam en wachtwoord** voordat u doorgaat naar de volgende stappen. Hier volgen de stappen om u te begeleiden u deze kunt vinden.

   * Eenmaal **"Azure SQL Database"** op uw oplossingssjabloon diagram wordt groen, klikt u op deze en klik vervolgens op **'Geopend'**. U naar Azure-portal geleid en de pagina van uw database-informatie ook wordt geopend.
   * Klik op de pagina vindt u een sectie 'Database'. De tabel uit de database die u hebt gemaakt. De naam van uw database moet **'Uw oplossingsnaam willekeurig getal + 'db' '** (bijvoorbeeld ' mytest12345db').
   * Klik op uw database in de nieuwe pop-outmodus Configuratiescherm, vindt u de naam van uw database-server in de rechterbovenhoek. De naam van uw database-server moet `"Your Solution Name + Random Number + 'database.windows.net,1433'"` (bijvoorbeeld ' mytest12345.database.windows.net,1433').
   * Uw database **gebruikersnaam** en **wachtwoord** zijn hetzelfde als de gebruikersnaam en het wachtwoord eerder vastgelegd tijdens de implementatie van de oplossing.
2. De gegevensbron van het koude pad Power BI-bestand bijwerken

   * Zorg ervoor dat u hebt de nieuwste versie van geïnstalleerd [Power BI desktop](https://powerbi.microsoft.com/desktop).
   * In de **"DemandForecastingDataGeneratorv1.0"** map die u hebt gedownload, dubbelklikt u op de **'Power BI Template\DemandForecastPowerBI.pbix'** bestand. De eerste visualisaties zijn gebaseerd op dummy-gegevens. **Opmerking:** als u een foutbericht ziet, zorg ervoor dat u de meest recente versie van Power BI Desktop hebt geïnstalleerd.

     Nadat u deze, klikt u boven aan het bestand opent, klikt u op **query's bewerken**. Dubbelklik in het pop-outmodus venster, op **'Source'** in het rechterdeelvenster.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic1.png)
   * Vervang in het pop-outmodus venster, **'Server'** en **'Database'** met uw eigen namen van de server en database, en klik vervolgens op **'OK'**. Voor de servernaam van de, zorg ervoor dat u de poort 1433 opgeven (**YourSolutionName.database.windows.net, 1433**). Negeer de waarschuwingsberichten die worden weergegeven op het scherm.
   * In het volgende pop-outmodus venster, ziet u twee opties in het linkerdeelvenster (**Windows** en **Database**). Klik op **'Database'**, vult u uw **'Gebruikersnaam'** en **'Wachtwoord'** (dit is de gebruikersnaam en het wachtwoord dat u hebt ingevoerd wanneer u eerst de oplossing wordt geïmplementeerd en een Azure wordt gemaakt SQL database). In ***Selecteer welk gegevensniveau voor deze instellingen toepassen op***, een optie voor database controleren. Klik vervolgens op **'Verbinding maken'**.
   * Nadat u terug naar de vorige pagina Begeleide bent, sluit u het venster. Een bericht POP's uit - Klik **toepassen**. Klik ten slotte de **opslaan** knop de wijzigingen op te slaan. Uw Power BI-bestand is nu ingesteld voor verbinding met de server. Als uw visualisaties leeg zijn, zorg er dan voor dat u de gewenste opties op de visualisaties aan alle gegevens visualiseren door te klikken op het gumpictogram in de rechterbovenhoek van de legenda's uitschakelen. Gebruik de vernieuwknop om nieuwe gegevens voor de visualisaties weer te geven. In eerste instantie alleen ziet u de seedgegevens op uw visualisaties als de data factory is gepland om te vernieuwen elke drie uur. Na 3 uur ziet u de nieuwe voorspellingen doorgevoerd in uw visualisaties bij het vernieuwen van de gegevens.
3. (Optioneel) Het koude pad-dashboard om te publiceren [Power BI online](http://www.powerbi.com/). Houd er rekening mee dat deze stap moet een Power BI-account (of Office 365-account).

   * Klik op **'Publiceren'** en enkele seconden later er verschijnt een venster weergegeven "Publiceren naar Power BI succes!" met een groen vinkje. Klik op de volgende koppeling "Open demoprediction.pbix in Power BI". Gedetailleerde instructies, Zie [publiceren vanuit Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Een nieuw dashboard maken: klik op de **+** Meld u aan bij de **Dashboards** sectie in het linkerdeelvenster. Voer de naam 'Vraag voorspellen Demo' voor dit nieuwe dashboard.
   * Nadat u het rapport openen, klikt u op ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) om alle visualisaties aan uw dashboard vast te maken. Gedetailleerde instructies, Zie [een tegel vastmaken aan Power BI-dashboard vanuit een rapport](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Ga naar de dashboardpagina en de grootte en locatie van uw visualisaties aanpassen en bewerken van hun titels. Zie voor gedetailleerde instructies over hoe u uw tegels bewerken, [bewerken een tegel--formaat wijzigen, verplaatsen, naam wijzigen, vastmaken, verwijderen, hyperlink toevoegen](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Hier volgt een voorbeeld van dashboard met sommige koude pad visualisaties zijn vastgemaakt aan het.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic7.png)
4. (Optioneel) Planning vernieuwen van de gegevensbron.

   * Vernieuwing wilt plannen van de gegevens, de muisaanwijzer boven de **EnergyBPI definitief** gegevensset, klikt u op ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic3.png) en kies vervolgens **vernieuwen plannen**.
     **Opmerking:** als er een waarschuwing massage, klikt u op **referenties bewerken** en controleer of de referenties van uw database zijn hetzelfde als die wordt beschreven in stap 1.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic4.png)
   * Vouw de **vernieuwen plannen** sectie. Schakel 'uw gegevens actueel houden'.
   * Geen vernieuwing worden gepland op basis van uw behoeften. Meer informatie, Zie [gegevens vernieuwen in Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).

## <a name="how-to-delete-your-solution"></a>**Uw oplossing verwijderen**
Zorg ervoor dat u de gegevensgenerator stopt wanneer u de oplossing niet actief gebruikt als de gegevensgenerator leidt hogere kosten tot. De oplossing te verwijderen als u deze niet gebruikt. Uw oplossing verwijdert, worden de onderdelen die zijn ingericht in uw abonnement tijdens de implementatie van de oplossing. De oplossing klikt u op de oplossingsnaam van uw in het linkerdeelvenster van de oplossingssjabloon verwijderen en klik op verwijderen.

## <a name="cost-estimation-tools"></a>**Schatting van de extra kosten**
De volgende twee hulpprogramma's zijn beschikbaar waarmee u meer informatie over de totale kosten die betrokken zijn bij het uitvoeren van de on-Demand prognose voor energie-oplossingssjabloon in uw abonnement:

* [Microsoft Azure hulpprogramma Cost Estimator ontwikkeld (online)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure-hulpprogramma voor Cost Estimator ontwikkeld (desktop)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Bevestigingen**
In dit artikel is geschreven door gegevenswetenschapper Yijing Chen en software-engineer Qiu Min bij Microsoft.
