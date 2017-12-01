---
title: Diepgaand in de manier waarop te voorspellen vehicle status en groot gewoonten - Azure | Microsoft Docs
description: De mogelijkheden van Cortana Intelligence gebruiken om inzicht in real-time en voorspeld op vehicle status en verkeer te krijgen gewoonten.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: bradsev
ms.openlocfilehash: a21316ef6ab05918f07a09243b5ce04950ecd9dc
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Vehicle telemetrie Analytics-oplossing playbook: Deep Duik in de oplossing
Dit menu is gekoppeld aan de secties van dit playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Dit document zoomt in op in elk van de fasen beschreven in de oplossingsarchitectuur. Instructies en verwijzingen voor aanpassing zijn opgenomen. 

## <a name="data-sources"></a>Gegevensbronnen
De oplossing maakt gebruik van twee verschillende gegevensbronnen:

* Gesimuleerde vehicle signalen en diagnostische gegevens
* Vehicle catalogus

Een drager telematica simulator is opgenomen als onderdeel van deze oplossing, zoals wordt weergegeven in de volgende schermafbeelding. Deze verzendt diagnostische gegevens en signalen die overeenkomen met de status van de drager en het aangedreven patroon op een bepaald tijdstip. Als u wilt de drager telematica Simulator Visual Studio-oplossing voor aanpassingen op basis van uw vereisten hebt gedownload, gaat u naar de [Vehicle telematica simulator](http://go.microsoft.com/fwlink/?LinkId=717075) webpagina. De catalogus drager bevat een verwijzing van een gegevensset die vehicle-id's (VINs) wordt toegewezen aan modellen.

![Vehicle telematica simulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)


Deze gegevensset JSON-indeling bevat het volgende schema.

| Kolom | Beschrijving | Waarden |
| --- | --- | --- |
| CHASSISNUMMER |Willekeurig gegenereerde VIN |Verkregen van een master lijst 10.000 willekeurig gegenereerde VINs |
| Externe temperatuur |De externe temperatuur waar het besturen van de drager |Willekeurig gegenereerd nummer tussen 0 en 100 |
| Engine temperatuur |De engine temperatuur van de drager |Willekeurig gegenereerde getal tussen 0 en 500 |
| Snelheid |De engine-snelheid waarmee het besturen van de drager |Willekeurig gegenereerd nummer tussen 0 en 100 |
| Brandstof |De brandstofpeil van de drager |Gegenereerde willekeurig getal tussen 0 en 100 (geeft brandstof niveau percentage) |
| EngineOil |De engine olie-niveau van de drager |Gegenereerde willekeurig getal tussen 0 en 100 (geeft engine olie niveau percentage) |
| Druk band |Druk op de band van de drager |Gegenereerde willekeurig getal tussen 0 en 50 (geeft band zware belasting op het niveau percentage) |
| Kilometerstand |De kilometerstand van de drager |Willekeurig gegenereerde getal tussen 0 en 200.000 |
| Accelerator_pedal_position |De accelerator vorm positie van de drager |Gegenereerde willekeurig getal tussen 0 en 100 (geeft accelerator niveau percentage) |
| Parking_brake_status |Hiermee wordt aangegeven of de drager geparkeerd of niet |True of False |
| Headlamp_status |Hiermee wordt aangegeven of het licht op of niet |True of False |
| Brake_pedal_status |Hiermee wordt aangegeven of het rempedaal wordt ingedrukt of niet |True of False |
| Transmission_gear_position |De positie van de versnelling verzending van de drager |Statussen: eerste, tweede, derde, vierde vijfde, zesde, zevende, achtste |
| Ignition_status |Hiermee wordt aangegeven of de drager uitgevoerd of gestopt |True of False |
| Windshield_wiper_status |Hiermee wordt aangegeven of de combinatie voorruit is ingeschakeld of niet |True of False |
| ABS |Hiermee wordt aangegeven of ABS is ingeschakeld of niet |True of False |
| Timestamp |De tijdstempel wanneer het gegevenspunt wordt gemaakt |Date |
| Plaats |De locatie van de drager |Vier steden in deze oplossing: Bellevue, Redmond, Sammamish, Seattle |

De gegevensset vehicle model verwijzing toegewezen VINs aan modellen. 

| CHASSISNUMMER | Model |
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
| 26WJSGHX4MA5ROHNL |Converteerbaar |
| GHLUB6ONKMOSI7E77 |Station wagon |
| 9C2RHVRVLMEJDBXLP |Compacte auto |
| BRNHVMZOUJ6EOCP32 |Kleine standaard |
| VCYVW0WUZNBTM594J |Sport auto |
| HNVCE6YFZSA5M82NY |Gemiddeld standaard |
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
| 8OMCL3LGI7XNCC21U |Converteerbaar |
| ……. | |

## <a name="ingestion"></a>Opname
Combinaties van Azure Event Hubs, Azure Stream Analytics en Azure Data Factory worden gebruikt voor de drager signalen, de diagnostische gebeurtenissen opnemen en realtime en analytics batch. Al deze onderdelen zijn gemaakt en geconfigureerd als onderdeel van de implementatie van de oplossing. 

### <a name="real-time-analysis"></a>Realtime analyses
De gebeurtenissen die worden gegenereerd door de drager telematica simulator worden gepubliceerd naar de event hub met behulp van de event hub SDK.  

![Event hub-dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

De Stream Analytics-taak opgenomen van deze gebeurtenissen van de event hub en de gegevens in realtime analyseren van de status vehicle verwerkt.

![Stream Analytics-taak verwerken van gegevens](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 


De Stream Analytics-taak:

* Gegevens van de event hub opgenomen.
* Voert een koppeling met de referentiegegevens de drager VIN toewijzen aan de desbetreffende model. 
* Persistente ze in Azure Blob-opslag voor uitgebreide batch analyses. 

De volgende Stream Analytics query wordt gebruikt om de gegevens in de Blob-opslag: 

![Stream Analytics-taak query voor gegevensopname](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 


### <a name="batch-analysis"></a>Batchanalyse
Een extra volume van de gesimuleerde vehicle signalen en diagnostische gegevens wordt ook gegenereerd voor uitgebreidere batch analytics. Deze extra volume is nodig om een goede representatief gegevensvolume voor batchverwerking. Voor dit doel PrepareSampleDataPipeline gebruikt in de Data Factory-werkstroom voor het genereren van een jaar lang gesimuleerde vehicle signalen en diagnostische gegevens. Voor het downloaden van de Data Factory aangepaste .NET activiteit Visual Studio-oplossing op basis van de vereisten van uw aanpassingen, gaat u naar de [Data Factory aangepaste activiteit](http://go.microsoft.com/fwlink/?LinkId=717077) webpagina. 

Deze werkstroom ziet u voorbeeldgegevens voorbereid voor batchverwerking.

![Voorbeeldgegevens voorbereid voor de werkstroom voor batch](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 


De pijplijn bestaat uit een aangepaste activiteit van de Data Factory .NET.

![PrepareSampleDataPipeline activiteit](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

Nadat de pijplijn met succes uitgevoerd en de gegevensset RawCarEventsTable is gemarkeerd als 'Gereed', een jaar lang gesimuleerde vehicle signalen en diagnostische gegevens worden geproduceerd. U ziet de volgende map en -bestand gemaakt in uw opslagaccount in de container connectedcar:

![PrepareSampleDataPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

## <a name="partition-the-data-set"></a>Partitioneren van de gegevensset
In de stap ter voorbereiding van gegevens worden de onbewerkte semi-gestructureerde vehicle signalen en diagnostische gegevens naar een indeling maand/gepartitioneerd. Deze partitioneren bevordert efficiënter uitvoeren van query's en schaalbare langetermijnopslag doordat fout over. Bijvoorbeeld als het eerste account voor blob vol raakt, bedrijfsstoringen het boven aan het volgende account. 

>[!NOTE] 
>Deze stap in de oplossing geldt alleen voor batchverwerking.

Invoer en uitvoer gegevensbeheer:

* **Uitvoergegevens** (gelabelde PartitionedCarEventsTable) voor een lange periode wordt bewaard als de fundamentele / 'rawest' vorm van gegevens in data lake van de klant. 
* **Invoergegevens** aan deze pijplijn is doorgaans verwijderd omdat de uitvoergegevens heeft volledige overeenkomen met de invoer. Wordt opgeslagen (gepartitioneerd) beter voor later gebruik.

De werkstroom van de gebeurtenissen in de partitie auto.

![Partitie auto gebeurtenissen werkstroom](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)


De onbewerkte gegevens is gepartitioneerd met behulp van een Azure HDInsight Hive-activiteit in PartitionCarEventsPipeline, zoals wordt weergegeven in de volgende schermafbeelding. De voorbeeldgegevens gegenereerd voor een jaar in de stap gegevens ter voorbereiding is op jaar/maand gepartitioneerd. De partities worden gebruikt voor het genereren van vehicle signalen en diagnostische gegevens voor elke maand (totaal van 12 partities) van een jaar. 

![PartitionCarEventsPipeline activiteit](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)


**PartitionConnectedCarEvents Hive-script**

Het Hive-script partitioncarevents.hql wordt gebruikt voor het partitioneren. Bevindt het zich in de map \demo\src\connectedcar\scripts van het gedownloade zip-bestand. 
    
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

Nadat de pijplijn succes is uitgevoerd, ziet u de volgende partities in uw opslagaccount in de container connectedcar gegenereerd:

![Gepartitioneerde uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

De gegevens zijn nu geoptimaliseerd beter kan worden beheerd en klaar voor verdere verwerking voor het verkrijgen van inzicht uitgebreide batch. 

## <a name="data-analysis"></a>Gegevensanalyse
In deze sectie ziet u het combineren van Stream Analytics, Azure Machine Learning, Data Factory en HDInsight voor rich geavanceerde analyses van vehicle status en die zorg draagt gewoonten.

### <a name="machine-learning"></a>Machine learning
Dit dient om te voorspellen van de voertuigen waarvoor onderhoud of intrekken op basis van bepaalde Health statistische gegevens, op basis van de volgende veronderstellingen:

* Als een van de volgende drie voorwaarden voldaan wordt, vereisen de voertuigen onderhoud onderhoud:
  
  * De druk band is laag.
  * Het niveau van de engine olie is laag.
  * De engine temperatuur is hoog.

* Als een van de volgende voorwaarden voldaan wordt, kunnen de voertuigen hebben een probleem met de veiligheid en terughalen vereisen:
  
  * De temperatuur engine hoog is, maar de externe temperatuur laag is.
  * De engine temperatuur laag is, maar de externe temperatuur is hoog.

Op basis van de vorige vereisten, twee afzonderlijke modellen afwijkingen gedetecteerd. Één model is voor de detectie van vehicle onderhoud en één model is voor de detectie van vehicle intrekken. Beide modellen in wordt de ingebouwde principal onderdeel analysis (Pso)-algoritme gebruikt voor afwijkingsdetectie. 

#### <a name="maintenance-detection-model"></a>**Model voor onderhoud**

Als een van drie indicatoren--band druk, engine olie of engine temperatuur--voldoet aan de respectieve voorwaarde, het model van de detectie onderhoud een afwijkingsdetectie rapporten. Alleen deze drie variabelen moeten als gevolg hiervan worden rekening houden met het maken van het model. In het experiment in machine learning zijn de **Select Columns in Dataset** module wordt gebruikt voor het uitpakken van deze drie variabelen. De detectiemodule PCA-gebaseerd anomaliedetectie wordt vervolgens gebruikt om het model van de detectie afwijkingsdetectie samen te stellen. 

PCA is een tot stand gebrachte techniek in machine learning die kan worden toegepast op Functieselectie, classificatie en anomaly detection. PCA zet een set met aanvragen die mogelijk gecorreleerde variabelen in een set waarden principal onderdelen genoemd bevatten. Het belangrijkste idee modellering PCA-gebaseerd is om gegevens naar een lager dimensionale ruimte aan functies en afwijkingen gemakkelijker te identificeren.

Voor elke nieuwe invoer in het model van de detectie wordt de detectie van de afwijkingsdetectie eerst de projectie op de eigenvectors berekend. Vervolgens wordt de fout genormaliseerde herstel berekend. Deze fout genormaliseerde is de score afwijkingsdetectie: hoe hoger de fout en de afwijkende het exemplaar. 

Elke record wordt beschouwd als een punt in een driedimensionale ruimte conform band druk, engine olie en temperatuur motor coördinaten in het probleem van de detectie van onderhoud. Voor het vastleggen van deze afwijkingen wordt PCA gebruikt om de oorspronkelijke gegevens in de driedimensionale ruimte projecteren op een tweedimensionale ruimte. De parameter aantal onderdelen moet worden gebruikt in PCA is dus ingesteld op twee. Deze parameter speelt een belangrijke rol bij het toepassen van PCA-gebaseerd anomaliedetectie. Als u met PCA projectgegevens, worden deze afwijkingen eenvoudiger geïdentificeerd.

#### <a name="recall-anomaly-detection-model"></a>**Model voor afwijkingsdetectie intrekken**

In het model intrekken afwijkingsdetectie detectie, de **Select Columns in Dataset** en PCA-gebaseerd anomaliedetectie detectiemodules op vergelijkbare wijze worden gebruikt. In het bijzonder drie variabelen--temperatuur, buiten engine temperatuur- en snelheid--zijn uitgepakt eerst met behulp van de **Select Columns in Dataset** module. De variabele snelheid is ook opgenomen, omdat de engine temperatuur doorgaans heeft betrekking op de snelheid. De detectiemodule PCA-gebaseerd anomaliedetectie wordt vervolgens gebruikt om de gegevens van de driedimensionale ruimte projecteren op een tweedimensionale ruimte. De intrekken criteria is voldaan. De drager vereist intrekken wanneer engine temperatuur- en externe temperatuur maximaal negatieve worden gecorreleerd. Nadat de PCA wordt uitgevoerd, wordt de PCA-gebaseerd anomaliedetectie-algoritme gebruikt om vast te leggen van de afwijkingen. 

Bij het trainen van een model wordt normale gegevens als de invoergegevens gebruikt voor het model van de detectie PCA-gebaseerd anomaliedetectie trainen. (Normale gegevens niet vereist onderhoud of intrekken.) Het model van de detectie getraind afwijkingsdetectie wordt in het scoreprofiel experiment gebruikt om te detecteren of de drager onderhoud of intrekken vereist. 

### <a name="real-time-analysis"></a>Realtime analyses
De volgende Stream Analytics SQL-query wordt gebruikt voor het ophalen van het gemiddelde van alle belangrijke vehicle-parameters. Deze parameters omvatten vehicle snelheid, brandstofpeil engine temperatuur, kilometerstand, band druk, engine olie niveau en anderen. De gemiddelden worden gebruikt voor afwijkingen detecteren, het verlenen van waarschuwingen en het bepalen van de algehele status voorwaarden van voertuigen uitgevoerd in een specifieke regio. De gemiddelden worden vervolgens gecorreleerd met demografische gegevens. 

![Stream Analytics query voor realtime verwerking](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Alle gemiddelden worden via een drie seconden tumblingvenster berekend. Een tumblingvenster wordt gebruikt omdat niet-overlappende en aaneengesloten tijdsintervallen vereist zijn. 

Zie voor meer informatie over de mogelijkheden windowing in Stream Analytics, [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

#### <a name="real-time-prediction"></a>**Realtime voorspelling**

Er is een toepassing opgenomen als onderdeel van de oplossing voor de machine learning-model in realtime operationeel. De toepassing RealTimeDashboardApp is gemaakt en geconfigureerd als onderdeel van de implementatie van de oplossing. De toepassing:

* Luistert naar een event hub-instantie waarop Stream Analytics de gebeurtenissen in een patroon continu publiceert.

    ![Stream Analytics query voor het publiceren van de gegevens](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) 

* Gebeurtenissen ontvangt. Voor elke gebeurtenis die deze toepassing ontvangt: 
   
   * De gegevens worden verwerkt met behulp van een machine learning-score (RR's) eindpunt aanvragen en antwoorden. Het eindpunt RR's wordt automatisch gepubliceerd als onderdeel van de implementatie.
   * De uitvoer RRS is gepubliceerd op een Power BI-gegevensset met behulp van de push-API's.

Dit patroon is ook van toepassing op scenario's waarin u wilt integreren van een line-of-business-toepassing met de stroom van realtime-analyses. Deze scenario's omvatten waarschuwingen, meldingen en messaging.

Zie voor het downloaden van de RealtimeDashboardApp Visual Studio-oplossing voor aanpassingen de [RealtimeDashboardApp downloaden](http://go.microsoft.com/fwlink/?LinkId=717078) webpagina. 

#### <a name="execute-the-real-time-dashboard-application"></a>**De dashboardtoepassing realtime uitvoeren**
1. Pak de RealtimeDashboardApp en lokaal opslaat.

    ![RealTimeDashboardApp map](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) 

2. De toepassing RealtimeDashboardApp.exe uitvoeren.

3. Voer uw geldige Power BI-referenties in en selecteer **aanmelden**.  

    ![Realtime dashboard-app aanmelden venster](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 
    
4. Selecteer **accepteren**.

    ![Realtime dashboard app laatste aanmelden venster](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

>[!NOTE] 
>Als u wilt leegmaken van de gegevensset Power BI, worden de RealtimeDashboardApp met de parameter 'flushdata' uitgevoerd. 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Batchanalyse
Dit dient om weer te geven hoe de motoren Contoso maakt gebruik van de Azure compute-mogelijkheden voor het benutten van big data. Deze gegevens onthult veel inzichten op aangedreven patronen, gebruik gedrag en vehicle health. Deze informatie kan worden:

* Verbetering van de gebruikerservaring en goedkoper maken door op te geven inzicht gerichtheid gewoonten en fuel-efficient aangedreven gedrag.
* Informatie over proactief over klanten en de aangedreven implementaties om reguleren zakelijke beslissingen te nemen en beste eersteklas producten en services te bieden.

In deze oplossing worden de volgende metrische gegevens gericht:

* **Agressief gedrag besturen**: identificeert de trend van de modellen, locaties, aangedreven voorwaarden en tijd van het jaar om inzicht in agressief aangedreven patronen te krijgen. Contoso motoren kunt deze inzichten gebruiken voor marketingcampagnes inleidende informatie over nieuwe functies voor persoonlijke en verzekering op basis van informatie over het gebruik.
* **Fuel-Efficient aangedreven gedrag**: identificeert de trend van de modellen, locaties, aangedreven voorwaarden en tijd van het jaar om inzicht op fuel-efficient aangedreven patronen te krijgen. Contoso motoren kunnen deze inzichten voor marketingcampagnes gebruiken voor het introduceren van nieuwe functies en -stuurprogramma's voor rendabele en omgeving-vriendelijk aangedreven gewoonten proactieve rapportage.
* **Intrekken van modellen**: identificeert modellen waarvoor teruggehaald operationele de afwijkingsdetectie detectie machine learning-experiment.

We bekijken in de details van elk van deze metrische gegevens.

#### <a name="aggressive-driving-behavior-patterns"></a>**Agressieve aangedreven gedrag patronen**

De gepartitioneerde vehicle signalen en diagnostische gegevens worden verwerkt in AggresiveDrivingPatternPipeline, zoals wordt weergegeven in de volgende werkstroom. Hive wordt gebruikt om te bepalen van de modellen, locatie, vehicle, aangedreven voorwaarden en andere parameters die agressieve aangedreven patronen vertonen.

![Agressieve aangedreven patroon-werkstroom](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 

***Agressieve aangedreven patroon Hive-query***

De Hive-script aggresivedriving.hql wordt gebruikt voor het analyseren van agressieve aangedreven voorwaarde patronen. Bevindt het zich in de map \demo\src\connectedcar\scripts van het gedownloade zip-bestand. 

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


Het script maakt gebruik van de combinatie van een medium transmission tandwielpictogram positie, bedient vorm status en snelheid voor het detecteren van roekeloze/agressieve aangedreven gedrag op basis van patronen met hoge snelheid remmen. 

Nadat de pijplijn succes is uitgevoerd, ziet u de volgende partities in uw opslagaccount in de container connectedcar gegenereerd:

![AggressiveDrivingPatternPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 


#### <a name="fuel-efficient-driving-behavior-patterns"></a>**Fuel-Efficient aangedreven gedrag patronen**

De gepartitioneerde vehicle signalen en diagnostische gegevens worden verwerkt in FuelEfficientDrivingPatternPipeline, zoals wordt weergegeven in de volgende werkstroom. Hive wordt gebruikt om te bepalen van de modellen, locatie, vehicle, aangedreven voorwaarden en andere eigenschappen die fuel-efficient aangedreven patronen vertonen.

![Fuel-Efficient aangedreven patronen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

***Fuel-Efficient aangedreven patroon Hive-query***

Het Hive-script fuelefficientdriving.hql wordt gebruikt voor het analyseren van fuel-efficient aangedreven voorwaarde patronen. Bevindt het zich in de map \demo\src\connectedcar\scripts van het gedownloade zip-bestand. 

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


Het script maakt gebruik van de combinatie van een medium transmission tandwielpictogram positie, bedient vorm status, de snelheid en de accelerator positie voor het detecteren van fuel-efficient aangedreven gedrag op basis van versnelling, remvermogen pedaal en patronen te versnellen. 

Nadat de pijplijn succes is uitgevoerd, ziet u de volgende partities in uw opslagaccount in de container connectedcar gegenereerd:

![FuelEfficientDrivingPatternPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

**Modelvoorspellingen intrekken**

De machine learning-experiment worden ingericht en gepubliceerd als een webservice als onderdeel van de implementatie van de oplossing. De score-eindpunt wordt gebruikt in deze workflow. Het is geregistreerd als een data factory gekoppelde service en geoperationaliseerd met behulp van de data factory score activiteit.

![Machine learning-eindpunt](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

De geregistreerde gekoppelde service wordt gebruikt in DetectAnomalyPipeline voor de beoordeling van de gegevens met behulp van de afwijkingsdetectie detectie-model. 

![Machine learning score activiteit in de gegevensfactory](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png)  

Een paar stappen worden uitgevoerd in deze pijplijn voor het voorbereiden van gegevens, zodat deze kan worden geoperationaliseerd met de webservice voor de score. 

![DetectAnomalyPipeline voor de voorspelling intrekken](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png)  

***Afwijkingsdetectie detectie Hive-query***

Nadat de score berekenen is voltooid, wordt een HDInsight-activiteit verwerkt en de gegevens die het model is ingedeeld als afwijkingen samenvoegt. Een kans score van 0.60 of hoger wordt gebruikt door het model.

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


Nadat de pijplijn succes is uitgevoerd, ziet u de volgende partities in uw opslagaccount in de container connectedcar gegenereerd:

![DetectAnomalyPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

## <a name="publish"></a>Publiceren

### <a name="real-time-analysis"></a>Realtime analyses
Een van de query's in de Stream Analytics-taak publiceert de gebeurtenissen naar een exemplaar van uitvoer event hub. 

![Stream Analytics-taak die is gepubliceerd naar een event hub-instantie van uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

De volgende Stream Analytics query wordt gebruikt om te publiceren naar de uitvoer event hub-instantie:

![Stream Analytics query om te publiceren naar de uitvoer event hub-instantie](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

Deze stroom van gebeurtenissen wordt gebruikt door de RealTimeDashboardApp die opgenomen in de oplossing. Deze toepassing maakt gebruik van de machine learning-webservice reactie op aanvragen voor score berekenen voor realtime. De resulterende gegevens wordt gepubliceerd op een Power BI-gegevensset voor verbruik. 

### <a name="batch-analysis"></a>Batchanalyse
De resultaten van de batch- en realtime verwerking worden gepubliceerd naar Azure SQL Database-tabellen voor verbruik. De SQL-server, de database en de tabellen worden automatisch gemaakt als onderdeel van het installatiescript. 

De resultaten van de verwerking van batch worden gekopieerd naar de werkstroom van de datamart gegevens.

![Batchverwerking resultaten gekopieerd naar de werkstroom datamart data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

De Stream Analytics-taak is gepubliceerd naar de datamart.

![Stream Analytics-taak die is gepubliceerd naar de datamart](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

De instelling van de datamart gegevens is in de Stream Analytics-taak.

![Datamart-instelling van Data in Stream Analytics-taak](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

## <a name="consume"></a>Gebruiken
Power BI biedt deze oplossing een uitgebreide dashboard voor realtime-gegevens en visualisaties predictive analytics. 

Het laatste dashboard ziet er in dit voorbeeld:

![Power BI-dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

## <a name="summary"></a>Samenvatting
Dit document bevat een gedetailleerde inzoomen van de drager telemetrie Analytics-oplossing. Het patroon van lambda-architectuur wordt gebruikt voor realtime en batch-analyses met voorspellingen en acties. Dit patroon van toepassing is op een breed scala aan gebruiksvoorbeelden waarvoor hot pad (real-time) en analyses van koude pad (batch). 

### <a name="references"></a>Verwijzingen

* [Vehicle telematica Simulator Visual Studio-oplossing](http://go.microsoft.com/fwlink/?LinkId=717075) 
* [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
* [Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)
* [Azure Event Hubs-SDK voor streamopname](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
* [Data movement mogelijkheden van Azure Data Factory](../../data-factory/v1/data-factory-data-movement-activities.md)
* [Azure Data Factory .NET-activiteit](../../data-factory/v1/data-factory-use-custom-activities.md)
* [Azure Data Factory .NET-activiteit gebruikt voor het voorbereiden van de voorbeeldgegevens van Visual Studio-oplossing](http://go.microsoft.com/fwlink/?LinkId=717077) 
