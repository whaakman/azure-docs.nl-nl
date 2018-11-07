---
title: Nauwkeuriger te voorspellen en rijgewoonten - Azure | Microsoft Docs
description: De mogelijkheden van Cortana Intelligence gebruiken om te krijgen van realtime en voorspellende inzichten op en winkelgedrag.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: deguhath
ms.openlocfilehash: fc4b2d0beef1c91cd937ba901974796441657441
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234762"
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Vehicle Telemetry Analytics-oplossing playbook: ontdeek de oplossing
Dit bevat menukoppelingen naar de secties van dit playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

In dit document zoomt in op in elk van de fasen weergegeven in de architectuur van de oplossing. Instructies en aanwijzingen voor aanpassing worden opgenomen. 

## <a name="data-sources"></a>Gegevensbronnen
De oplossing maakt gebruik van twee verschillende gegevensbronnen:

* Gesimuleerde vehicle signalen en diagnostische gegevens
* Vehicle catalogus

Een vehicle telematicssimulator is opgenomen als onderdeel van deze oplossing, zoals wordt weergegeven in de volgende schermafbeelding. Het verzendt de diagnostische gegevens en signalen die overeenkomen met de status van het voertuig en het sparen patroon op een bepaald tijdstip.  De catalogus vehicle bevat een referentiegegevensset die vehicle identificatiecodes (VINs) wordt toegewezen aan modellen. Opmerking: De gegevensset Vehicle telematica Simulator Visual Studio-oplossing is niet meer beschikbaar. 

![Vehicle telematicssimulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)


Deze gegevensset JSON-indeling bevat de volgende schema.

| Kolom | Beschrijving | Waarden |
| --- | --- | --- |
| VIN |Willekeurig gegenereerde VIN |Verkregen van een master lijst van 10.000 willekeurig gegenereerde VINs |
| Externe temperatuur |De externe temperatuur waarbij het voertuig vraagt om |Willekeurig gegenereerd getal tussen 0 en 100 |
| Engine temperatuur |De engine temperatuur van het voertuig |Willekeurig gegenereerd getal tussen 0 en 500 |
| Snelheid |De snelheid van de engine waarmee het voertuig vraagt om |Willekeurig gegenereerd getal tussen 0 en 100 |
| Brandstof |De brandstof niveau van het voertuig |Gegenereerde willekeurig getal tussen 0 en 100 (geeft brandstof niveau percentage) |
| EngineOil |De engine olie-niveau van het voertuig |Gegenereerde willekeurig getal tussen 0 en 100 (geeft engine olie niveau percentage) |
| Druk te verlichten band |Druk op de band van het voertuig |Gegenereerde willekeurig getal van 0 tot 50 (geeft band zware belasting op het niveau percentage) |
| Odometer |De kilometerstand van het voertuig |Willekeurig gegenereerd getal tussen 0 en 200.000 |
| Accelerator_pedal_position |De accelerator vorm positie van het voertuig |Gegenereerde willekeurig getal tussen 0 en 100 (geeft accelerator niveau percentage) |
| Parking_brake_status |Geeft aan of het voertuig geparkeerd of niet |De waarde True of False |
| Headlamp_status |Geeft aan of het licht op of niet |De waarde True of False |
| Brake_pedal_status |Geeft aan of het rempedaal wordt ingedrukt of niet |De waarde True of False |
| Transmission_gear_position |De positie van de tandwiel transmissie van het voertuig |Statussen: eerste, tweede, derde, vierde vijfde, zesde, zevende, achtste |
| Ignition_status |Geeft aan of het voertuig uitgevoerd of gestopt wordt |De waarde True of False |
| Windshield_wiper_status |Geeft aan of de combinatie voorruit is ingeschakeld of niet |De waarde True of False |
| ABS |Geeft aan of ABS of niet is ingeschakeld |De waarde True of False |
| Tijdstempel |De tijdstempel wanneer het gegevenspunt wordt gemaakt |Date |
| Plaats |De locatie van het voertuig |Vier steden in deze oplossing: Bellevue, Redmond, Sammamish, Seattle |

De referentiegegevensset model vehicle kaarten VINs modellen. 

| VIN | Model |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Sedan |
| 8J0U8XCPRGW4Z3NQE |Hybride |
| WORG68Z2PLTNZDBI7 |Familie sedan |
| JTHMYHQTEPP4WBMRN |Sedan |
| W9FTHG27LZN1YWO0Y |Hybride |
| MHTP9N792PHK08WJM |Familie sedan |
| EI4QXI2AXVQQING4I |Sedan |
| 5KKR2VB4WHQH97PF8 |Hybride |
| W9NSZ423XZHAONYXB |Familie sedan |
| 26WJSGHX4MA5ROHNL |Geconverteerd |
| GHLUB6ONKMOSI7E77 |Station wagon |
| 9C2RHVRVLMEJDBXLP |Compacte auto |
| BRNHVMZOUJ6EOCP32 |Kleine standaard |
| VCYVW0WUZNBTM594J |Sport auto 's |
| HNVCE6YFZSA5M82NY |Middelgrote standaard |
| 4R30FOR7NUOBL05GJ |Station wagon |
| WYNIIY42VKV6OQS1J |Grote standaard |
| 8Y5QKG27QET1RBK7I |Grote standaard |
| DF6OX2WSRA6511BVG |Coupé |
| Z2EOZWZBXAEW3E60T |Sedan |
| M4TV6IEALD5QDS3IR |Hybride |
| VHRA1Y2TGTA84F00H |Familie sedan |
| R0JAUHT1L1R3BIKI0 |Sedan |
| 9230C202Z60XX84AU |Hybride |
| T8DNDN5UDCWL7M72H |Familie sedan |
| 4WPYRUZII5YV7YA42 |Sedan |
| D1ZVY26UV2BFGHZNO |Hybride |
| XUF99EW9OIQOMV7Q7 |Familie sedan |
| 8OMCL3LGI7XNCC21U |Geconverteerd |
| ……. | |

## <a name="ingestion"></a>Gegevensopname
Combinaties van Azure Event Hubs, Azure Stream Analytics en Azure Data Factory worden gebruikt om op te nemen van het voertuig signalen, de diagnostische gebeurtenissen in realtime en batch-analyses. Al deze onderdelen worden gemaakt en geconfigureerd als onderdeel van de implementatie van de oplossing. 

### <a name="real-time-analysis"></a>Realtime analyse
De gebeurtenissen die worden gegenereerd door de vehicle telematicssimulator worden gepubliceerd naar de event hub met behulp van de event hub SDK.  

![Event hub-dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

De Stream Analytics-taak neemt deze gebeurtenissen uit de event hub en verwerkt de gegevens in realtime te analyseren van de voertuigstatus.

![Stream Analytics-taak verwerken van gegevens](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 


De Stream Analytics-taak:

* Neemt gegevens op uit de event hub.
* Voert een koppeling met de referentiegegevens het voertuig VIN toewijzen aan het bijbehorende model. 
* Persistente ze in Azure Blob-opslag voor uitgebreide batchanalyse. 

De volgende Stream Analytics-query wordt gebruikt om vast te leggen van de gegevens in Blob-opslag: 

![Query voor de Stream Analytics-taak voor opname van gegevens](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 


### <a name="batch-analysis"></a>Batchanalyse
Een extra volume van gesimuleerde vehicle signalen en diagnostische gegevens wordt ook gegenereerd voor rijkere batchanalyse. Deze extra volume is vereist om te controleren of een volume goede representatieve gegevens voor batchverwerking. Voor dit doel PrepareSampleDataPipeline gebruikt in de Data Factory-werkstroom voor het genereren van één jaar aan gesimuleerde vehicle signalen en diagnostische gegevens. Als u wilt de Data Factory aangepaste .NET-activiteit Visual Studio-oplossing voor aanpassingen op basis van uw vereisten hebt gedownload, gaat u naar de [aangepaste activiteit van Data Factory](https://go.microsoft.com/fwlink/?LinkId=717077) webpagina. 

Deze werkstroom bevat voorbeeldgegevens voor batchverwerking wordt voorbereid.

![Voorbeeldgegevens die zijn voorbereid voor de werkstroom voor batch](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 


De pijplijn bestaat uit een aangepaste Data Factory .NET-activiteit.

![PrepareSampleDataPipeline activiteit](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

Nadat de pijplijn correct wordt uitgevoerd en de gegevensset RawCarEventsTable is gemarkeerd als 'Gereed', is één jaar aan gesimuleerde vehicle signalen en diagnostische gegevens worden geproduceerd. U ziet de volgende map en bestand dat is gemaakt in uw storage-account onder de container connectedcar:

![PrepareSampleDataPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

## <a name="partition-the-data-set"></a>Partitioneren van de gegevensset
De onbewerkte semi-gestructureerde vehicle signalen en diagnostische gegevens in de voorbereiding van gegevens moet worden gepartitioneerd in de indeling van een jaar/maand. Deze partitioneren verhogen efficiënter uitvoeren van query's en schaalbare opslag op lange termijn doordat fouten over. Bijvoorbeeld, als het eerste account van de blob is vol, bedrijfsstoringen het naar het volgende account. 

>[!NOTE] 
>Deze stap in de oplossing geldt alleen voor batchverwerking.

Invoer en uitvoer gegevensbeheer:

* **Uitvoergegevens** (gelabelde PartitionedCarEventsTable) voor een lange periode wordt bewaard als de fundamentele / "rawest" vorm van gegevens in van de klant data lake. 
* **Invoergegevens** aan deze pijplijn wordt doorgaans verwijderd omdat er voor de uitvoergegevens vol vertrouwen aan de invoerpoort. Deze wordt opgeslagen (gepartitioneerd) beter voor later gebruik.

De werkstroom van de gebeurtenissen in de partitie auto.

![Partitie auto gebeurtenissen werkstroom](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)


De onbewerkte gegevens zijn gepartitioneerd met behulp van een Azure HDInsight Hive-activiteit in PartitionCarEventsPipeline, zoals wordt weergegeven in de volgende schermafbeelding. De voorbeeldgegevens gegenereerd voor een jaar in de stap van de voorbereiding van gegevens wordt geregeld aan de hand jaar/maand. De partities worden gebruikt voor het genereren van vehicle signalen en diagnostische gegevens voor elke maand (totaal van 12 partities) van een jaar. 

![PartitionCarEventsPipeline activiteit](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)


**PartitionConnectedCarEvents Hive-script**

Het Hive-script partitioncarevents.hql wordt gebruikt voor het partitioneren van. Deze bevindt zich in de map \demo\src\connectedcar\scripts van het gedownloade zip-bestand. 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

Nadat de pijplijn correct wordt uitgevoerd, ziet u de volgende partities in uw storage-account onder de container connectedcar gegenereerd:

![Gepartitioneerde uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

De gegevens is nu geoptimaliseerd, beter beheersbare en klaar voor verdere verwerking om uitgebreide batch inzichten te verkrijgen. 

## <a name="data-analysis"></a>Gegevensanalyse
In deze sectie ziet u hoe u Stream Analytics, Azure Machine Learning, Data Factory en HDInsight voor veel geavanceerde analyses in voertuigstatus en stimuleren gewoonten combineert.

### <a name="machine-learning"></a>Machine learning
Het doel hier is het voorspellen van de voertuigen waarvoor onderhoud of intrekken op basis van bepaalde Health-statistieken, op basis van de volgende veronderstellingen:

* Als een van de volgende drie voorwaarden voldaan wordt, wordt de voertuigen vereisen onderhoud onderhoud:
  
  * De band druk is laag.
  * Het niveau van de engine olie is laag.
  * De temperatuur van de motor is hoog.

* Als een van de volgende voorwaarden voldaan wordt, kunnen de voertuigen hebt een probleem met de veiligheid en vereisen intrekken:
  
  * De temperatuur engine hoog is, maar de externe temperatuur hoger is laag.
  * De engine temperatuur laag is, maar de externe temperatuur is hoog.

Op basis van de vorige vereisten, afwijkingen twee afzonderlijke modellen. Één model is voor de detectie van vehicle onderhoud en één model is voor de detectie van vehicle intrekken. In beide modellen, wordt de ingebouwde principal onderdeel analysis (Pso)-algoritme gebruikt voor detectie van afwijkingen. 

#### <a name="maintenance-detection-model"></a>**Model voor onderhoud**

Als een van drie indicatoren--band druk te verlichten, engine olie of engine temperatuur--voldoet aan de respectieve voorwaarde, de detectie van onderhoud model een anomalie rapporten. Als gevolg hiervan moeten alleen deze drie variabelen worden rekening houden met het bouwen van het model. In het experiment in machine learning, de **Select Columns in Dataset** module wordt gebruikt om op te halen van deze drie variabelen. De detectiemodule PCA-gebaseerd anomaliedetectie wordt vervolgens gebruikt om het model van de detectie van afwijkingen maken. 

PCA is een vastgestelde techniek in machine learning die kan worden toegepast op Functieselectie, classificatie en detectie van afwijkingen. PCA zet een set van aanvragen die mogelijk gecorreleerde variabelen in een set waarden principal onderdelen genoemd bevatten. Het belangrijkste idee van modellen PCA-gebaseerd is om gegevens naar een lagere-dimensionale ruimte aan functies en afwijkingen gemakkelijker kunt identificeren.

Voor elke nieuwe invoer voor de detectie-model berekent de detectie van afwijkingen eerst de projectie van het eigenvectors. Vervolgens de fout genormaliseerde reconstructie worden berekend. Deze fout genormaliseerde is de anomaliedetectie-score: hoe hoger de fout en de afwijkende het exemplaar. 

Elke record wordt beschouwd als zoals een punt in een driedimensionale ruimte gedefinieerd door band druk-, engine olie- en engine-temperatuur coördinaten in het probleem van de detectie van onderhoud. Voor het vastleggen van deze afwijkingen wordt PCA gebruikt om de oorspronkelijke gegevens in de driedimensionale ruimte projecteren op een tweedimensionale ruimte. De parameter aantal onderdelen moet ik gebruiken in PCA is dus ingesteld op twee. Deze parameter speelt een belangrijke rol bij het toepassen van PCA-gebaseerd anomaliedetectie. Nadat u PCA om gegevens, worden deze afwijkingen eenvoudiger aangeduid.

#### <a name="recall-anomaly-detection-model"></a>**Model voor anomaliedetectie intrekken**

In het intrekken van afwijkingen detectie model, de **Select Columns in Dataset** en PCA-gebaseerd anomaliedetectie detectiemodules worden gebruikt op een soortgelijke manier. Specifiek, engine drie variabelen--temperatuur, externe temperatuur en snelheid--zijn uitgepakt eerst met behulp van de **Select Columns in Dataset** module. De variabele snelheid is ook opgenomen, omdat de engine temperatuur doorgaans met de snelheid overeenkomt. De detectiemodule PCA-gebaseerd anomaliedetectie wordt vervolgens gebruikt om de gegevens uit de driedimensionale ruimte projecteren op een tweedimensionale ruimte. De intrekken criteria is voldaan. Het voertuig moet intrekken wanneer engine temperatuur- en externe temperatuur zeer negatief worden gecorreleerd. Nadat de PCA is uitgevoerd, wordt de PCA-gebaseerd anomaliedetectie-algoritme wordt gebruikt om vast te leggen van de afwijkingen. 

Bij het trainen van een model, wordt normale gegevens gebruikt als de ingevoerde gegevens op het PCA-gebaseerd anomaliedetectie-detectie-model te trainen. (Normale gegevens geen vereist onderhoud of intrekken.) In het scoring-experiment, wordt het model voor getrainde anomaliedetectie gebruikt om te detecteren of het voertuig onderhoud of intrekken vereist. 

### <a name="real-time-analysis"></a>Realtime analyse
De volgende Stream Analytics-SQL-query wordt gebruikt om op te halen van het gemiddelde van alle belangrijke vehicle-parameters. Deze parameters bevatten snelheid van het voertuig, brandstof niveau, engine temperatuur, kilometerstand, band druk te verlichten, engine olie niveau en anderen. De gemiddelden worden gebruikt voor het detecteren van afwijkingen, waarschuwingen geven en te bepalen van de algehele de gezondheid van voorwaarden van voertuigen uitgevoerd in een bepaalde regio. De gemiddelden zijn vervolgens gecorreleerd met demografische gegevens. 

![Stream Analytics-query voor verwerking in realtime](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Alle gemiddelden worden berekend over een tumblingvenster drie seconden. Een tumblingvenster wordt gebruikt omdat niet-overlappende en aaneengesloten tijdsintervallen vereist zijn. 

Zie voor meer informatie over de mogelijkheden van windowing in Stream Analytics, [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

#### <a name="real-time-prediction"></a>**Realtime voorspelling**

Een toepassing wordt opgenomen als onderdeel van de oplossing voor het operationeel maken van de machine learning-model in realtime. De toepassing RealTimeDashboardApp is gemaakt en geconfigureerd als onderdeel van de implementatie van de oplossing. De toepassing:

* Luistert naar een event hub-instantie waar Stream Analytics de gebeurtenissen in een patroon continu publiceert.

    ![Stream Analytics-query voor het publiceren van de gegevens](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) 

* Gebeurtenissen ontvangt. Voor elke gebeurtenis die deze toepassing ontvangt: 
   
   * De gegevens worden verwerkt met behulp van een machine learning-request response scoring (RRS)-eindpunt. De RRS-eindpunt wordt automatisch gepubliceerd als onderdeel van de implementatie.
   * De RRS-uitvoer wordt gepubliceerd naar een Power BI-gegevensset met behulp van de push API's.

Dit patroon is ook van toepassing zijn op scenario's waarin u wilt integreren van een line-of-business-toepassing in de werkstroom realtime analyses. Deze scenario's omvatten waarschuwingen, meldingen en berichten.

Opmerking: de gegevens voor de RealtimeDashboardApp Visual Studio-oplossing is niet meer beschikbaar.

#### <a name="execute-the-real-time-dashboard-application"></a>**De dashboardtoepassing realtime uitvoeren**
1. Pak de RealtimeDashboardApp en lokaal opslaan.

    ![RealTimeDashboardApp map](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) 

2. De toepassing RealtimeDashboardApp.exe uitvoeren.

3. Voer uw geldige Power BI-referenties in en selecteer **aanmelden**.  

    ![App-aanmelden venster realtime dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 
    
4. Selecteer **Accepteren**.

    ![Realtime dashboard app laatste aanmelding venster](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

>[!NOTE] 
>Als u leegmaken van de Power BI-gegevensset wilt, worden de RealtimeDashboardApp met de parameter "flushdata" uitgevoerd. 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Batchanalyse
Het doel hier is om weer te geven hoe Contoso Motors maakt gebruik van de Azure compute-mogelijkheden voor het gebruik van big data. Deze gegevens blijkt duidelijk inzicht op sparen patronen en het gebruiksgedrag voertuigstatus. Deze informatie kunt u:

* Verbetering van de gebruikerservaring en goedkoper maken door op te geven van inzichten op gewoonten en fuel-efficient sparen gedrag te stimuleren.
* Ontdek proactief klanten en hun sparen patronen om te bepalen van zakelijke beslissingen te nemen en bieden de beste mogelijke producten en services.

In deze oplossing maakt zijn de volgende metrische gegevens gericht:

* **Agressief gedrag te stimuleren**: identificeert de trend van de modellen, locaties, aangedreven voorwaarden en tijd van het jaar agressief sparen patronen inzichten kunnen krijgen. Contoso Motors kunt deze inzichten gebruiken voor marketingcampagnes te introduceren nieuwe functies van de persoonlijke en verzekering op basis van gebruik.
* **Fuel-Efficient sparen gedrag**: identificeert de trend van de modellen, locaties, aangedreven voorwaarden en tijd van het jaar fuel-efficient sparen patronen inzichten kunnen krijgen. Contoso Motors kunt deze inzichten voor marketingcampagnes gebruiken om nieuwe functies en proactieve rapporteren aan de stuurprogramma's voor kosteneffectieve en omgeving-vriendelijk sparen gewoonten te introduceren.
* **Intrekken van modellen**: modellen waarvoor u door tot het operationaliseren van de anomaliedetectie-detectie van machine learning-experiment identificeert.

Laten we bekijken de details van elk van deze metrische gegevens.

#### <a name="aggressive-driving-behavior-patterns"></a>**Agressief sparen gedragspatronen**

De gepartitioneerde vehicle signalen en diagnostische gegevens worden verwerkt in AggresiveDrivingPatternPipeline, zoals wordt weergegeven in de volgende werkstroom. Hive wordt gebruikt om te bepalen van de modellen, locatie, voertuig, aangedreven voorwaarden en andere parameters die agressief sparen patronen vertonen.

![Agressief sparen patroon-werkstroom](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 

***Agressief sparen patroon Hive-query***

Het Hive-script aggresivedriving.hql wordt gebruikt om het agressief sparen voorwaarde patronen analyseren. Deze bevindt zich in de map \demo\src\connectedcar\scripts van het gedownloade zip-bestand. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


Het script maakt gebruik van de combinatie van verzending gear positie, bedient vorm status en snelheid van het voertuig om roekeloze/agressief sparen gedrag op basis van patronen met hoge snelheid remmen te detecteren. 

Nadat de pijplijn correct wordt uitgevoerd, ziet u de volgende partities in uw storage-account onder de container connectedcar gegenereerd:

![AggressiveDrivingPatternPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 


#### <a name="fuel-efficient-driving-behavior-patterns"></a>**Fuel-Efficient sparen gedragspatronen**

De gepartitioneerde vehicle signalen en diagnostische gegevens worden verwerkt in FuelEfficientDrivingPatternPipeline, zoals wordt weergegeven in de volgende werkstroom. Hive wordt gebruikt om te bepalen van de modellen, locatie, voertuig, aangedreven voorwaarden en andere eigenschappen die fuel-efficient sparen patronen vertonen.

![Fuel-Efficient sparen patronen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

***Fuel-Efficient sparen patroon Hive-query***

De Hive-script fuelefficientdriving.hql wordt gebruikt voor het analyseren van fuel-efficient sparen voorwaarde patronen. Deze bevindt zich in de map \demo\src\connectedcar\scripts van het gedownloade zip-bestand. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


Het script maakt gebruik van de combinatie van van het voertuig verzending gear positie, bedient vorm status, de snelheid en accelerator pedaal positie om fuel-efficient sparen gedrag op basis van versnelling, remmen, te detecteren en patronen te versnellen. 

Nadat de pijplijn correct wordt uitgevoerd, ziet u de volgende partities in uw storage-account onder de container connectedcar gegenereerd:

![FuelEfficientDrivingPatternPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

**Modelvoorspellingen intrekken**

De machine learning-experiment is ingericht en gepubliceerd als een webservice als onderdeel van de implementatie van de oplossing. De batchscore-eindpunt wordt gebruikt in deze werkstroom. Het is geregistreerd als een gekoppelde factory-service en geoperationaliseerd met behulp van de data factory batchscore-activiteit.

![Machine learning-eindpunt](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

De geregistreerde gekoppelde service wordt gebruikt in DetectAnomalyPipeline om de gegevens met behulp van de detectie van afwijkingen model te beoordelen. 

![Machine learning batch scoring-activiteit in data factory](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png)  

Een paar stappen worden uitgevoerd in deze pijplijn voor het voorbereiden van gegevens, zodat deze kan operationaliseren met de batchscore-webservice. 

![DetectAnomalyPipeline voor voorspelling intrekken](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png)  

***Anomaly detection Hive-query***

Nadat de score is voltooid, wordt een HDInsight-activiteit verwerkt en de gegevens die het model als afwijkingen worden gecategoriseerd. Het model gebruikt een score kans van 0.60 of hoger.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Nadat de pijplijn correct wordt uitgevoerd, ziet u de volgende partities in uw storage-account onder de container connectedcar gegenereerd:

![DetectAnomalyPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

## <a name="publish"></a>Publiceren

### <a name="real-time-analysis"></a>Realtime analyse
Een van de query's in de Stream Analytics-taak publiceert de gebeurtenissen naar een uitvoer event hub-instantie. 

![Stream Analytics-taak is gepubliceerd naar een event hub-instantie van uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

De volgende Stream Analytics-query wordt gebruikt om te publiceren naar de uitvoer event hub-instantie:

![Stream Analytics-query om te publiceren naar de uitvoer event hub-instantie](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

Deze stroom van gebeurtenissen wordt verbruikt door de RealTimeDashboardApp die opgenomen in de oplossing. Deze toepassing maakt gebruik van de machine learning-webservice request response voor het scoren van realtime. Het publiceert de resulterende gegevens naar een Power BI-gegevensset naar verbruik. 

### <a name="batch-analysis"></a>Batchanalyse
De resultaten van de batch- en verwerking in realtime worden gepubliceerd naar Azure SQL Database-tabellen voor gebruik. De SQL-server, de database en de tabellen worden automatisch gemaakt als onderdeel van de setup-script. 

De resultaten van de verwerking van batch worden gekopieerd naar de datamart-werkstroom.

![Batchverwerking resultaten die zijn gekopieerd naar datamart-werkstroom](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

De Stream Analytics-taak is gepubliceerd naar de datamart.

![Stream Analytics-taak is gepubliceerd naar de datamart](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

De datamart-instelling zich in de Stream Analytics-taak.

![Datamart-instelling in Stream Analytics-taak](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

## <a name="consume"></a>Gebruiken
Power BI biedt deze oplossing een uitgebreid dashboard toegevoegd voor gegevens in realtime en voorspellende analyses visualisaties. 

Het laatste dashboard ziet eruit zoals in dit voorbeeld:

![Power BI-dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

## <a name="summary"></a>Samenvatting
Dit document bevat een gedetailleerde Inzoomen op van de Vehicle Telemetry Analytics-oplossing. De lambda-architectuurpatroon wordt gebruikt voor real-time en batch-analyses met voorspellingen en acties. Dit patroon is van toepassing op een breed scala aan toepassingen waarvoor dynamische pad (realtime) en analyses van koude pad (batch). 

### <a name="references"></a>Verwijzingen

* [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
* [Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)
* [Azure Event Hubs-SDK voor opname van streams](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
* [Mogelijkheden van Azure Data Factory gegevens verplaatsen](../../data-factory/copy-activity-overview.md)
* [Azure Data Factory .NET-activiteit](../../data-factory/transform-data-using-dotnet-custom-activity.md)
* [Azure Data Factory .NET-activiteit gebruikt voor het voorbereiden van de voorbeeldgegevens van Visual Studio-oplossing](https://go.microsoft.com/fwlink/?LinkId=717077) 
