---
title: Vraag prognose in technische handleiding energie | Microsoft Docs
description: Een technische handleiding voor de sjabloon oplossing met Microsoft Cortana Intelligence voor vraag prognose in energie.
services: cortana-analytics
documentationcenter: 
author: yijichen
manager: ilanr9
editor: yijichen
ms.assetid: 7f1a866b-79b7-4b97-ae3e-bc6bebe8c756
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: inqiu;yijichen;ilanr9
ms.openlocfilehash: bb3520d36e4c34c752fe388f3126da285e2161cd
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Technische handleiding voor de sjabloon Cortana Intelligence-oplossing voor vraag prognose in energie
## <a name="overview"></a>**Overzicht**
Oplossingssjablonen zijn ontworpen om het proces van het bouwen van een demo E2E boven op de Cortana Intelligence Suite versnellen. Een geïmplementeerde sjabloon bepalingen van uw abonnement met de benodigde Cortana Intelligence-onderdeel en de relaties tussen bouwen. Deze ook de seeding van de pijplijn gegevens met voorbeeldgegevens ophalen gegenereerd op basis van een toepassing simuleren. De simulator gegevens van de koppeling downloaden en installeren op uw lokale computer, Raadpleeg het Leesmij-bestand voor instructies over het gebruik van de simulator. Gegevens die zijn gegenereerd op basis van de simulator hydrates de gegevens pijplijn en start het genereren van machine learning voorspelling, die vervolgens kan worden weergegeven op het Power BI-dashboard.

De oplossingssjabloon vindt [hier](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1)

Het implementatieproces begeleidt u bij verschillende stappen voor het instellen van de referenties van uw oplossing. Zorg ervoor dat u deze referenties zoals oplossingsnaam, gebruikersnaam en wachtwoord dat u tijdens de implementatie opgeeft vastleggen.

Het doel van dit document is om uit te leggen van de referentiearchitectuur en de verschillende onderdelen die zijn ingericht in uw abonnement als onderdeel van de sjabloon voor deze oplossing. Het document is ook wordt gesproken over hoe u de voorbeeldgegevens vervangen door echte gegevens van uw eigen kunnen inzicht/voorspellingen van u gewonnen gegevens zien. Bovendien de spreekt document over de onderdelen van de oplossing-sjabloon die worden gewijzigd moet als u wilt aanpassen van de oplossing met uw eigen gegevens. Over het bouwen van de Power BI-dashboard voor de sjabloon voor deze oplossing worden instructies aan het einde.

## <a name="details"></a>**Details**
![](media/cortana-analytics-technical-guide-demand-forecast/ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Architectuur uitgelegd
Als u de oplossing is geïmplementeerd, verschillende Azure-services binnen Cortana-Analytics Suite zijn geactiveerd (dat wil zeggen, Event Hub, Stream Analytics, HDInsight, Data Factory, Machine Learning *enzovoort*). De Architectuurdiagram ziet u, op een hoog niveau, hoe het voorspellen van de vraag voor energie-oplossingssjabloon is samengesteld uit de end-to-end. U kunt deze services onderzoeken door erop te klikken op de oplossing sjabloon diagram gemaakt met de implementatie van de oplossing. De volgende secties beschrijven elk gegeven.

## <a name="data-source-and-ingestion"></a>**Gegevensbron en opnamesnelheid**
### <a name="synthetic-data-source"></a>Synthetische gegevensbron
Voor deze sjabloon wordt de gegevensbron waarmee gegenereerd vanuit een bureaubladtoepassing die u downloaden en lokaal uitvoeren na een geslaagde implementatie. U vinden de instructies voor het downloaden en installeren van deze toepassing op de Eigenschappenbalk wanneer u het eerste knooppunt energie prognose gegevens Simulator aangeroepen in het diagram van de sjabloon oplossing selecteert. Deze toepassing feeds de [Azure Event Hub](#azure-event-hub) service met gegevenspunten of gebeurtenissen die worden gebruikt in de rest van de stroom van de oplossing.

De toepassing voor het genereren van gebeurtenis vult de Azure Event Hub alleen terwijl deze wordt uitgevoerd op uw computer.

### <a name="azure-event-hub"></a>Azure Event Hub
De [Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) -service is de ontvanger van de ingevoerde door de synthetische gegevensbron beschreven.

## <a name="data-preparation-and-analysis"></a>**Voorbereiden van gegevens en analyse**
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
De [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) service wordt gebruikt voor bijna in realtime analyses op de invoerstroom van geven de [Azure Event Hub](#azure-event-hub) service en publiceren van de resultaten naar een [Power BI](https://powerbi.microsoft.com) dashboard, evenals alle onbewerkte binnenkomende gebeurtenissen te archiveren de [Azure Storage](https://azure.microsoft.com/services/storage/) service later worden verwerkt door de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service.

### <a name="hdinsight-custom-aggregation"></a>Aangepaste HDInsight-aggregatie
De Azure HDInsight-service wordt gebruikt om uit te voeren [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts (gedirigeerd door Azure Data Factory) voor aggregaties op de onbewerkte gebeurtenissen die zijn gearchiveerd met de Azure Stream Analytics-service.

### <a name="azure-machine-learning"></a>Azure Machine Learning
De [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -service wordt gebruikt (gedirigeerd door Azure Data Factory) aanbrengen prognose van toekomstige energieverbruik van een bepaald gebied gegeven van de invoer ontvangen.

## <a name="data-publishing"></a>**Gegevens publiceren**
### <a name="azure-sql-database-service"></a>Azure SQL Database-Service
De [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) service wordt gebruikt om op te slaan (beheerd door Azure Data Factory) de voorspellingen ontvangen door de Azure Machine Learning-service die wordt gebruikt in de [Power BI](https://powerbi.microsoft.com) dashboard.

## <a name="data-consumption"></a>**Gegevens verbruik**
### <a name="power-bi"></a>Power BI
De [Power BI](https://powerbi.microsoft.com) service wordt gebruikt voor het weergeven van een dashboard met aggregaties geleverd door de [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) service als vraag forecast resultaten opgeslagen in [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) die zijn geproduceerd met behulp van de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) service. Raadpleeg de volgende sectie voor instructies over het bouwen van de Power BI-dashboard voor deze oplossing-sjabloon.

## <a name="how-to-bring-in-your-own-data"></a>**Hoe op te zetten in uw eigen gegevens**
Deze sectie wordt beschreven hoe u uw eigen gegevens overbrengen naar Azure en welke gebieden voor de gegevens die u in deze architectuur brengt wijzigingen nodig.

Het lijkt onwaarschijnlijk dat een gegevensset die u brengt overeenkomen met de gegevensset die wordt gebruikt voor deze oplossingssjabloon. Inzicht in uw gegevens en de vereisten zijn van cruciaal belang bij hoe het wijzigen van deze sjabloon wilt werken met uw eigen gegevens. Als niet bekend bent met Azure Machine Learning-service, kunt u een inleiding tot het verkrijgen door middel van het voorbeeld in [het maken van uw eerste experiment](machine-learning/studio/create-experiment.md).

De volgende secties worden besproken in de secties van de sjabloon die te worden gewijzigd hoeven wanneer een nieuwe gegevensset wordt geïntroduceerd.

### <a name="azure-event-hub"></a>Azure Event Hub
De [Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) -service is algemeen, zodat gegevens naar de hub in CSV- of JSON-indeling kan worden geplaatst. Er is geen speciale verwerking optreedt in de Azure Event Hub, maar het is belangrijk dat u begrijpt dat de gegevens die is ingevoerd.

Dit document beschrijft niet hoe u uw gegevens opnemen, maar een kan eenvoudig gebeurtenissen of gegevens verzenden naar een Azure Event Hub, met behulp van de [Event Hub API](event-hubs/event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure Stream Analytics
De [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) service wordt gebruikt om uw bijna realtime analyses te lezen van gegevensstromen en uitvoeren van gegevens naar een willekeurig aantal bronnen.

Voor het voorspellen van de vraag voor oplossingssjabloon energieverbruik bestaat de Azure Stream Analytics query uit twee subquery's, elk gebeurtenis van de service Azure Event Hub gebruiken als invoer en uitvoer met naar twee verschillende locaties. Deze uitvoer bestaan uit één gegevensset met Power BI en een Azure-opslaglocatie.

De [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) query kan worden gevonden door:

* Aanmelden bij de [Azure-portal](https://portal.azure.com/)
* Zoeken naar de stream analytics-taken ![](media/cortana-analytics-technical-guide-demand-forecast/icon-stream-analytics.png) die zijn gegenereerd tijdens de oplossing is geïmplementeerd. Een is om de gegevens worden gepusht naar blob storage (bijvoorbeeld mytest1streaming432822asablob) en een andere voor Power BI (bijvoorbeeld mytest1streaming432822asapbi) gegevens worden gepusht.
* Selecteren

  * ***INVOER*** invoer als de query wilt weergeven
  * ***QUERY*** om de query zelf weer te geven
  * ***LEVERT*** om de verschillende uitvoer weer te geven

Informatie over het samenstellen van Azure Stream Analytics query vindt u in de [Stream Analytics Query verwijzing](https://msdn.microsoft.com/library/azure/dn834998.aspx) op MSDN.

In deze oplossing is de Azure Stream Analytics-taak die gegevensset met bijna realtime analyses informatie over de gegevensstroom inkomende aan een Power BI-dashboard levert die deel uitmaken van deze oplossingssjabloon. Omdat er impliciete kennis over de indeling van de binnenkomende, moet deze query's worden gewijzigd op basis van indeling van uw gegevens.

De andere Azure Stream Analytics-taak levert alle [Event Hub](https://azure.microsoft.com/services/event-hubs/) gebeurtenissen die moeten worden [Azure Storage](https://azure.microsoft.com/services/storage/) en daarom geen wijziging ongeacht de indeling van uw gegevens is vereist omdat de informatie over de volledige gebeurtenis gestreamd naar opslag.

### <a name="azure-data-factory"></a>Azure Data Factory
De [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) service ingedeeld het verkeer en de verwerking van gegevens. In het voorspellen van de vraag voor energie oplossingssjabloon de gegevensfactory is opgebouwd uit 12 [pijplijnen](data-factory/concepts-pipelines-activities.md) die verplaatsen en de gegevens met behulp van verschillende technologieën worden verwerkt.

  U kunt toegang tot uw data factory door het openen van het Data Factory-knooppunt onder aan de oplossing sjabloon diagram gemaakt met de implementatie van de oplossing. Ziet u de gegevensfactory in de Azure portal. Als u fouten onder uw gegevenssets ziet, kunt u die negeren omdat deze vanwege gegevensfactory worden geïmplementeerd voordat de gegevensgenerator werd gestart. Deze fouten doen niet voorkomen dat uw data factory werkt.

In deze sectie worden de benodigde [pijplijnen](data-factory/concepts-pipelines-activities.md) en [activiteiten](data-factory/concepts-pipelines-activities.md) opgenomen in de [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/). Volgende afbeelding is de diagramweergave van de oplossing:

![](media/cortana-analytics-technical-guide-demand-forecast/ADF2.png)

Vijf van de pijplijnen van deze factory bevatten [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts die worden gebruikt voor het partitioneren en de gegevens totaliseren. Wanneer u hebt genoteerd, de scripts bevinden zich in de [Azure Storage](https://azure.microsoft.com/services/storage/) account gemaakt tijdens de installatie. De locatie is: demandforecasting\\\\script\\\\hive\\ \\ (of https://[Your oplossing name].blob.core.windows.net/demandforecasting).

Net als bij de [Azure Stream Analytics](#azure-stream-analytics-1) query's de [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts impliciete kennis over de indeling van de binnenkomende, deze query's zou moeten worden gewijzigd op basis van indeling van uw gegevens en [functie-engineering](machine-learning/team-data-science-process/create-features.md) vereisten.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat een enkele activiteit - een [HDInsightHive](data-factory/transform-data-using-hadoop-hive.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die wordt uitgevoerd een [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) het script op geaggregeerde gestreamde in vraaggegevens om de tien seconden in onderstation instellen op elk uur regioniveau en plaatsen [Azure Storage](https://azure.microsoft.com/services/storage/) via de Azure Stream Analytics-taak.

De [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor deze taak partitionering is ***AggregateDemandRegion1Hr.hql***

#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat twee activiteiten:

* [HDInsightHive](data-factory/transform-data-using-hadoop-hive.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die een Hive-script voor de per uur geschiedenis vraaggegevens in onderstation instellen op elk uur regioniveau totaliseren en plaatsen in Azure Storage tijdens de Azure Stream Analytics-taak wordt uitgevoerd
* [Kopiëren](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die de geaggregeerde gegevens van Azure Storage-blob verplaatst naar de Azure SQL Database die als onderdeel van de installatie van de sjabloon oplossing is ingericht.

De [Hive](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script voor deze taak is ***AggregateDemandHistoryRegion.hql***.

#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*
Deze [pijplijnen](data-factory/concepts-pipelines-activities.md) bevatten verschillende activiteiten en waarvan eindresultaat is de scored voorspellingen van het Azure Machine Learning-experiment die zijn gekoppeld aan deze oplossingssjabloon. Ze zijn bijna identiek, met uitzondering van elk van deze verwerkt alleen de andere regio die wordt uitgevoerd door verschillende RegionID doorgegeven in de ADF-pijplijn en het hive-script voor elke regio.  
De activiteiten die zijn opgenomen in deze pipeline zijn:

* [HDInsightHive](data-factory/transform-data-using-hadoop-hive.md) activiteit met behulp van een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die wordt uitgevoerd een Hive-script uit te voeren aggregaties functie-engineering nodig zijn voor het Azure Machine Learning-experiment. De Hive-scripts voor deze taak zijn respectieve ***PrepareMLInputRegionX.hql***.
* [Kopiëren](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die wordt verplaatst van de resultaten van de [HDInsightHive](data-factory/transform-data-using-hadoop-hive.md) activiteit aan één Azure Storage blob die toegang op basis van worden kan de [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) activiteit.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) activiteit die de Azure Machine Learning-experiment, wat resulteert in de resultaten worden opgeslagen in een enkel Azure Storage-blob aanroept.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat een enkele activiteit - een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die de resultaten van het Azure Machine Learning-experiment van de respectieve verplaatst ***MLScoringRegionXPipeline***naar de Azure SQL-Database die is ingericht als onderdeel van de installatie van de sjabloon oplossing.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat een enkele activiteit - een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die de vraaggegevens geaggregeerde lopende uit verplaatst ***LoadHistoryDemandDataPipeline*** naar de SQL Azure De database die als onderdeel van de installatie van de sjabloon oplossing is ingericht.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Dit [pijplijn](data-factory/concepts-pipelines-activities.md) bevat een enkele activiteit - een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die de referentiegegevens voor onderstation-regio/Topologygeo dat als onderdeel van de oplossingssjabloon zijn geüpload naar Azure Storage-blob verplaatst installatie van de Azure SQL Database die als onderdeel van de installatie van de sjabloon oplossing is ingericht.

### <a name="azure-machine-learning"></a>Azure Machine Learning
De [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren gebruikt voor de oplossingssjabloon voor deze de voorspelling van de vraag van de regio biedt. Het experiment is specifiek voor de gegevensset die is verbruikt en daarom vereist wijziging of vervanging is specifiek voor de gegevens die in is gebracht.

## <a name="monitor-progress"></a>**Voortgang van de monitor**
Zodra de Generator van gegevens wordt gestart, de pijplijn begint met het ophalen van gehydrateerd en de verschillende onderdelen van uw oplossing beginnen bij de start in de actie volgende de opdrachten die zijn uitgegeven door de Data Factory. Er zijn twee manieren kunt u de pijplijn bewaken.

1. Controleer de gegevens uit Azure Blob Storage.

    Een van de Stream Analytics-taken schrijft de onbewerkte binnenkomende gegevens naar blob storage. Als u op klikt **Azure Blob Storage** onderdeel van uw oplossing van het scherm u de oplossing is geïmplementeerd en klik vervolgens op **Open** in het rechterpaneel u gaat naar de [Azure Portal](https://portal.azure.com). Klik op **Blobs**. In het volgende deelvenster ziet u een lijst van Containers. Klik op **'energysadata'**. In het volgende deelvenster ziet u de **'demandongoing'** map. In de map rawdata ziet u de mappen met namen zoals datum = 2016-01-28 enzovoort. Als u deze mappen ziet, betekent dit dat de onbewerkte gegevens voltooid is wordt op uw computer wordt gegenereerd en opgeslagen in blob storage. Hier ziet u bestanden met moeten eindig grootte in MB in deze mappen.
2. Controleer de gegevens van Azure SQL Database.

    De laatste stap van de pijplijn is om gegevens te schrijven (bijvoorbeeld de voorspellingen van machine learning) in SQL-Database. U moet wellicht wacht maximaal twee uur voor de gegevens worden weergegeven in de SQL-Database. Een manier om te controleren hoeveel gegevens zijn beschikbaar in de SQL-Database, is via [Azure-portal](https://portal.azure.com/). Zoek in het linkerdeelvenster SQL-DATABASES![](media/cortana-analytics-technical-guide-demand-forecast/SQLicon2.png) en klik erop. Zoek uw database (dat wil zeggen demo123456db) en vervolgens klikt u op. Op de volgende pagina onder **'Verbinding maken met uw database'** sectie, klikt u op **' Uitvoeren Transact-SQL-query's op uw SQL-database'**.

    Hier kunt u klikken op nieuwe Query en de query voor het aantal rijen (bijvoorbeeld ' select count(*) van DemandRealHourly) ' wanneer de database groeit, het aantal rijen in de tabel moet verhogen.)
3. Controleer de gegevens van Power BI-dashboard.

    U kunt Power BI-dashboard van hot pad instellen voor het bewaken van de onbewerkte binnenkomende gegevens. Volg de aanwijzingen in de sectie 'Power BI-Dashboard'.

## <a name="power-bi-dashboard"></a>**Power BI-Dashboard**
### <a name="overview"></a>Overzicht
Deze sectie beschrijft het instellen van Power BI-dashboard om uw realtime gegevens vanuit Azure stream analytics (hot pad) visualiseren, evenals forecast resultaten van Azure machine learning-(koude pad).

### <a name="setup-hot-path-dashboard"></a>Hot pad Dashboard instellen
De volgende stappen helpen u het visualiseren van real-time gegevensuitvoer van de Stream Analytics-taken die zijn gegenereerd op het moment van implementatie van de oplossing. Een [Power BI online](http://www.powerbi.com/) account is vereist de volgende stappen uit te voeren. Als u geen account hebt, kunt u [maken van een](https://powerbi.microsoft.com/pricing).

1. Power BI-uitvoer in Azure Stream Analytics (ASA) toevoegen.

   * U moet de instructies in [Azure Stream Analytics & Power BI: een dashboard realtime analyses voor realtime zichtbaarheid van gegevensstromen](stream-analytics/stream-analytics-power-bi-dashboard.md) voor het instellen van de uitvoer van uw Azure Stream Analytics-taak als uw Power BI-dashboard .
   * Zoek de stream analytics-taak in uw [Azure-portal](https://portal.azure.com). De naam van de taak moet: YourSolutionName + "streamingjob" + willekeurig getal + "asapbi' (dat wil zeggen demostreamingjob123456asapbi).
   * Voeg een Power BI-uitvoer voor de ASA-taak. Stel de **uitvoer Alias** als **'PBIoutput'**. Stel uw **gegevenssetnaam** en **tabelnaam** als **'EnergyStreamData'**. Nadat u de uitvoer hebt toegevoegd, klikt u op **'Start'** onder aan de pagina de Stream Analytics-taak starten. U moet een bevestigingsbericht wordt weergegeven (bijvoorbeeld ' starten stream analytics-taak myteststreamingjob12345asablob is voltooid').
2. Meld u aan bij [online in Power BI](http://www.powerbi.com)

   * In het linkerdeelvenster, gegevenssets sectie in mijn werkruimte, moet u een nieuwe gegevensset weergegeven in het linkerdeelvenster van Power BI zien. Dit is de streaminggegevens die u vanuit Azure Stream Analytics in de vorige stap gepusht.
   * Zorg ervoor dat de ***visualisaties*** deelvenster is geopend en aan de rechterkant van het scherm wordt weergegeven.
3. Maak de tegel 'Vraag door tijdstempel':

   * Klik op de gegevensset **'EnergyStreamData'** in het linkerdeelvenster gegevenssets sectie.
   * Klik op **'Lijndiagram'** pictogram ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic8.png).
   * Klik op 'EnergyStreamData' in **velden** Configuratiescherm.
   * Klik op **'Tijdstempel'** en zorg ervoor dat er onder 'As' wordt weergegeven. Klik op **'Load'** en zorg ervoor dat er wordt weergegeven onder "Waarden".
   * Klik op **opslaan** op de bovenkant en de naam van het rapport als 'EnergyStreamDataReport'. Het rapport met de naam 'EnergyStreamDataReport' wordt weergegeven in de sectie rapporten in het deelvenster Navigator aan de linkerkant.
   * Klik op **'Pincode Visual'** ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) pictogram in de rechterbovenhoek van dit lijndiagram een venster 'Vastmaken aan Dashboard' mogelijk weergegeven waarmee u kunt ervoor kiezen een dashboard. Selecteer 'EnergyStreamDataReport' en klik op 'Pincode'.
   * Beweeg de muisaanwijzer via deze tegel op het dashboard, klikt u op '' pictogram bewerken in de rechterbovenhoek te wijzigen van de titel als 'Vraag door tijdstempel'
4. Andere dashboard tegels op basis van de juiste gegevenssets maken. De laatste dashboardweergave:![](media/cortana-analytics-technical-guide-demand-forecast/PBIFullScreen.png)

### <a name="setup-cold-path-dashboard"></a>Setup koude pad Dashboard
In de pijplijn voor koude pad gegevens, is het essentieel doel is om op te halen van de vraagprognose van elke regio. Power BI maakt verbinding met een Azure SQL database als de gegevensbron, waar de voorspelling resultaten worden opgeslagen.

> [!NOTE]
> 1) Het duurt enkele uren voor het verzamelen van genoeg prognose resultaten voor het dashboard. U wordt aangeraden de start van dit proces 2-3 uur nadat u de gegevensgenerator lunch. 2) in deze stap wordt de vereiste is om te downloaden en installeren van de gratis software [van Power BI desktop](https://powerbi.microsoft.com/desktop).
>
>

1. De databasereferenties ophalen.

   U moet **servernaam, databasenaam, gebruikersnaam en wachtwoord van de database** voordat u doorgaat naar de volgende stappen. Hier volgen de stappen om u te helpen ze te zoeken.

   * Eenmaal **'Azure SQL Database'** op uw oplossingssjabloon diagram wordt groen, klik hierop en klik op **'Open'**. U geleid tot Azure-portal en de pagina van de database-informatie ook wordt geopend.
   * Klik op de pagina vindt u een sectie 'Database'. Hiermee geeft u de database die u hebt gemaakt. De naam van de database moet **'Uw oplossingsnaam willekeurig getal + 'database' '** (bijvoorbeeld ' mytest12345db').
   * Klik op de database in de nieuwe pop uit Configuratiescherm vindt u de naam van uw database-server op de voorgrond. De naam van uw database-server moet `"Your Solution Name + Random Number + 'database.windows.net,1433'"` (bijvoorbeeld ' mytest12345.database.windows.net,1433').
   * Uw database **gebruikersnaam** en **wachtwoord** zijn hetzelfde als de gebruikersnaam en wachtwoord eerder vastgelegd tijdens de implementatie van de oplossing.
2. De gegevensbron van het bestand van koude pad Power BI bijwerken

   * Zorg ervoor dat u hebt de nieuwste versie van geïnstalleerd [van Power BI desktop](https://powerbi.microsoft.com/desktop).
   * In de **'DemandForecastingDataGeneratorv1.0'** map die u hebt gedownload, dubbelklikt u op de **'Power BI Template\DemandForecastPowerBI.pbix'** bestand. De eerste visualisaties zijn gebaseerd op dummy-gegevens. **Opmerking:** als u een foutbericht ziet, controleert u of u de nieuwste versie van Power BI Desktop hebt geïnstalleerd.

     Nadat u deze boven aan het bestand openen, klikt u op **query's bewerken**. Dubbelklik in het pop uit venster **'Source'** in het rechterpaneel weergegeven.
     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic1.png)
   * Vervang in het pop uit venster **'Server'** en **'Database'** met uw eigen namen van de server en database en klik vervolgens op **'OK'**. Voor de servernaam van de, zorg ervoor dat u de poort 1433 opgeven (**YourSolutionName.database.windows.net, 1433**). Negeer de waarschuwingsberichten die worden weergegeven op het scherm.
   * In de volgende pop uit venster ziet u twee opties in het linkerdeelvenster (**Windows** en **Database**). Klik op **'Database'**, vul uw **'Gebruikersnaam'** en **'Password'** (dit is de gebruikersnaam en het wachtwoord dat u hebt ingevoerd wanneer u eerst de oplossing geïmplementeerd en een Azure SQL database gemaakt). In ***Selecteer welk gegevensniveau toe te passen van deze instellingen***, niveau databaseoptie controleren. Klik vervolgens op **'Connect'**.
   * Als u terug naar de vorige pagina bent geleid, sluit het venster. Een bericht POP's uit - Klik **toepassen**. Klik ten slotte de **opslaan** knop de wijzigingen wilt opslaan. Uw Power BI-bestand is nu ingesteld voor verbinding met de server. Als uw visualisaties leeg zijn, zorg er dan voor dat u de selecties op de visualisaties niet alle gegevens visualiseren door te klikken op het pictogram gum op de rechterbovenhoek van de legenda's wissen. Gebruik de knop Vernieuwen in overeenstemming met nieuwe gegevens op de visualisaties. Aanvankelijk ziet u alleen de seedgegevens op uw visualisaties omdat de gegevensfactory is gepland voor elke drie uur vernieuwen. Na drie uur ziet u de nieuwe voorspellingen doorgevoerd in uw visualisaties Wanneer u de gegevens vernieuwen.
3. (Optioneel) Publiceren van het dashboard koude pad om [Power BI online](http://www.powerbi.com/). Houd er rekening mee dat deze stap moet een Power BI-account (of Office 365-account).

   * Klik op **'Publiceren'** en later enkele seconden verschijnt er een venster weergeven 'Publiceren naar Power BI geslaagd'! met een groen vinkje. Klik op de volgende koppeling 'Open demoprediction.pbix in Power BI'. Gedetailleerde instructies vindt [publiceren vanuit Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Een nieuw dashboard maken: klik op de  **+**  Meld u aan bij de **Dashboards** sectie in het linkerdeelvenster. Voer de naam 'Vraag prognose Demo' voor dit nieuwe dashboard.
   * Nadat u het rapport openen, klikt u op ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic6.png) alle visualisaties aan uw dashboard vastmaken. Gedetailleerde instructies vindt [een tegel vastmaken aan Power BI-dashboard uit een rapport](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Ga naar de dashboardpagina en de grootte en locatie van uw visualisaties aanpassen en hun titels bewerken. Gedetailleerde instructies voor het bewerken van uw tegels Zie [bewerken een tegel--formaat, verplaatsen, wijzig de naam, pincode, verwijderen, voegt u hyperlink](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Hier volgt een voorbeeld van dashboard met sommige visualisaties koude pad is vastgemaakt aan het.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic7.png)
4. (Optioneel) Geplande vernieuwing van de gegevensbron.

   * Vernieuwen van de planning van de gegevens, de muisaanwijzer boven de **EnergyBPI Final** gegevensset, klikt u op ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic3.png) en kies vervolgens **schema vernieuwen**.
     **Opmerking:** als er een waarschuwing massage, klikt u op **referenties bewerken** en zorg ervoor dat uw databasereferenties zijn hetzelfde als die wordt beschreven in stap 1.

     ![](media/cortana-analytics-technical-guide-demand-forecast/PowerBIpic4.png)
   * Vouw de **schema vernieuwen** sectie. Schakel in 'uw gegevens up-to-date houden'.
   * De vernieuwing op basis van de behoeften van uw plannen. Ga voor meer informatie Zie [gegevens vernieuwen in Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).

## <a name="how-to-delete-your-solution"></a>**Het verwijderen van uw oplossing**
Zorg ervoor dat u de gegevensgenerator van stopt wanneer de oplossing niet actief wordt gebruikt als de hogere kosten leidt ertoe dat de gegevensgenerator uitgevoerd. Verwijder de oplossing als u dit niet gebruikt. Uw oplossing verwijdert, worden de onderdelen die zijn ingericht in uw abonnement wanneer u de oplossing hebt geïmplementeerd. Voor het verwijderen van de oplossing Klik op de oplossingsnaam van uw in het linkerpaneel van de oplossingssjabloon en klik op verwijderen.

## <a name="cost-estimation-tools"></a>**Schatting extra kosten**
De volgende twee hulpprogramma's zijn beschikbaar om te helpen u de totale kosten die betrokken zijn bij het voorspellen van de vraag voor oplossingssjabloon energie in uw abonnement beter te begrijpen:

* [Microsoft Azure kosten Estimator Tool (online)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure kosten Estimator Tool (desktop)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Bevestigingen**
In dit artikel is geschreven door gegevens wetenschappelijk Yijing Chen en software engineer Qiu Min bij Microsoft.
