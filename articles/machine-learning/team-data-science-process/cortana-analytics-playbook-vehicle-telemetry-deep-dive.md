---
title: Diepgaand in voorspellen vehicle gezondheid en die zorg draagt gewoonten - Azure | Microsoft Docs
description: De mogelijkheden van Cortana Intelligence gebruiken om inzicht in real-time en voorspeld op vehicle status en verkeer te krijgen gewoonten.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 4050fdc2056df395bbcc37e3783f61eebd90f80a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Draaiboek met oplossingen voor voertuigtelemetrieanalyse: duik in de diepte van de oplossing
Dit **menu** koppelingen naar de secties van dit playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Deze sectie zoomt in op in elk van de fasen beschreven in de oplossingsarchitectuur met instructies en verwijzingen voor aanpassing. 

## <a name="data-sources"></a>Gegevensbronnen
De oplossing maakt gebruik van twee verschillende gegevensbronnen:

* **gesimuleerde vehicle signalen en diagnostische gegevensset** en 
* **vehicle catalogus**

Er is een vehicle telematica simulator opgenomen als onderdeel van deze oplossing. Deze diagnostische gegevens verzendt en signalen overeenkomt voor de status van de drager en het aangedreven patroon op een bepaald tijdstip. Klik op [Vehicle telematica Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) voor het downloaden van de **Vehicle telematica Simulator Visual Studio-oplossing** voor aanpassingen op basis van uw vereisten. De catalogus drager bevat een verwijzingsgegevensset met een Chassisnummer toewijzing van het model.

![Vehicle telematica simulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

*Afbeelding 1 – Vehicle telematica Simulator*

Dit is een JSON-indeling gegevensset met het volgende schema.

| Kolom | Beschrijving | Waarden |
| --- | --- | --- |
| CHASSISNUMMER |Willekeurig gegenereerde Vehicle id-nummer |Hiermee wordt opgehaald uit een master lijst 10.000 willekeurig gegenereerde vehicle-id's. |
| Externe temperatuur |De externe temperatuur waar het besturen van de drager |Willekeurig gegenereerd nummer tussen 0-100 |
| Engine temperatuur |De engine temperatuur van de drager |Willekeurig gegenereerd nummer tussen 0-500 |
| Snelheid |De engine-snelheid waarmee het besturen van de drager |Willekeurig gegenereerd nummer tussen 0-100 |
| Brandstof |De brandstofpeil van de drager |Willekeurig gegenereerd nummer tussen 0-100 (geeft brandstof niveau percentage) |
| EngineOil |De engine olie-niveau van de drager |Willekeurig gegenereerd nummer tussen 0-100 (geeft engine olie niveau percentage) |
| Druk band |Druk op de band van de drager |Willekeurig gegenereerde getal van 0 tot 50 (geeft band zware belasting op het niveau percentage) |
| Kilometerstand |De kilometerstand van de drager |Willekeurig gegenereerd nummer van 0 200000 |
| Accelerator_pedal_position |De accelerator vorm positie van de drager |Willekeurig gegenereerd nummer tussen 0-100 (geeft accelerator niveau percentage) |
| Parking_brake_status |Hiermee wordt aangegeven of de drager geparkeerd of niet |True of False |
| Headlamp_status |Geeft aan waar het licht op of niet |True of False |
| Brake_pedal_status |Hiermee wordt aangegeven of het rempedaal wordt ingedrukt of niet |True of False |
| Transmission_gear_position |De positie van de versnelling verzending van de drager |Statussen: eerste, tweede, derde, vierde vijfde, zesde, zevende, achtste |
| Ignition_status |Hiermee wordt aangegeven of de drager uitgevoerd of gestopt |True of False |
| Windshield_wiper_status |Hiermee wordt aangegeven of de combinatie voorruit of niet is ingeschakeld |True of False |
| ABS |Hiermee wordt aangegeven of ABS is ingeschakeld of niet |True of False |
| tijdstempel |Het tijdstip waarop het gegevenspunt wordt gemaakt |Date |
| Plaats |De locatie van de drager |4 plaatsen zijn in deze oplossing: Bellevue, Redmond, Sammamish, Seattle |

De verwijzingsgegevensset vehicle model bevat VIN de modeltoewijzing. 

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
| GHLUB6ONKMOSI7E77 |Station Wagon |
| 9C2RHVRVLMEJDBXLP |Compacte auto |
| BRNHVMZOUJ6EOCP32 |Kleine standaard |
| VCYVW0WUZNBTM594J |Sport auto |
| HNVCE6YFZSA5M82NY |Gemiddeld standaard |
| 4R30FOR7NUOBL05GJ |Station Wagon |
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

### <a name="references"></a>Verwijzingen
[Vehicle telematica Simulator Visual Studio-oplossing](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Azure Event Hub](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a>Opname
Combinaties van Azure Event Hubs, Stream Analytics en Data Factory worden gebruikt voor het opnemen van de drager signalen, de diagnostische gebeurtenissen en realtime en analytics batch. Al deze onderdelen zijn gemaakt en geconfigureerd als onderdeel van de implementatie van de oplossing. 

### <a name="real-time-analysis"></a>Realtime analyses
De gebeurtenissen die worden gegenereerd door de drager telematica Simulator worden gepubliceerd naar de Event Hub met behulp van de Event Hub SDK. De Stream Analytics-taak opgenomen van deze gebeurtenissen van de Event Hub en de gegevens in realtime analyseren van de status vehicle verwerkt. 

![Event hub-dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

*Afbeelding 4: Event Hub-dashboard*

![Stream Analytics-taak verwerken van gegevens](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Afbeelding 5 - Stream Analytics-taak verwerken van gegevens*

De Stream Analytics-taak.

* gegevens van de Event Hub opgenomen 
* voert een koppeling met de referentiegegevens de drager VIN toewijzen aan de desbetreffende model 
* persistente ze in Azure blob storage voor uitgebreide batch analytics. 

De volgende Stream Analytics query wordt gebruikt om de gegevens in Azure blob-opslag. 

![Stream Analytics-taak query voor gegevensopname](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Afbeelding 6 - Stream Analytics-taak query voor gegevensopname*

### <a name="batch-analysis"></a>Batchanalyse
Er zijn ook een extra volume van de gesimuleerde vehicle signalen en diagnostische gegevensset voor uitgebreidere batch analytics genereren. Dit is vereist om ervoor te zorgen voor batchverwerking een goede representatief gegevensvolume. Voor dit doel gebruiken we een pijplijn met de naam 'PrepareSampleDataPipeline' in de Azure Data Factory-werkstroom voor het genereren van een jaar lang gesimuleerde vehicle signalen en diagnostische gegevensset. Klik op [Data Factory aangepaste activiteit](http://go.microsoft.com/fwlink/?LinkId=717077) voor het downloaden van de Data Factory aangepaste DotNet activiteit Visual Studio-oplossing voor aanpassingen op basis van uw vereisten. 

![Voorbeeldgegevens voor batchverwerking werkstroom voorbereiden](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Afbeelding 7: voorbeeldgegevens voorbereiden voor de werkstroom voor batch*

De pijplijn bestaat uit een aangepaste ADF .net activiteit, hier weergeven:

![PrepareSampleDataPipeline activiteit](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Afbeelding 8 - PrepareSampleDataPipeline*

Nadat de pijplijn met succes uitgevoerd en 'RawCarEventsTable' dataset is gemarkeerd als 'Gereed' één jaar waard gesimuleerde vehicle signalen en diagnostische worden gegevens geproduceerd. U ziet de volgende map en -bestand gemaakt in uw opslagaccount in de container 'connectedcar':

![PrepareSampleDataPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Afbeelding 9 - PrepareSampleDataPipeline uitvoer*

### <a name="references"></a>Verwijzingen
[Azure Event Hub SDK voor streamopname](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Mogelijkheden van Azure Data Factory data movement](../../data-factory/v1/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet-activiteit](../../data-factory/v1/data-factory-use-custom-activities.md)

[Azure Data Factory DotNet activiteit visual studio-oplossing voor het voorbereiden van voorbeeldgegevens](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-the-dataset"></a>Partitioneren van de gegevensset
De onbewerkte semi-gestructureerde vehicle signalen en diagnostische gegevensset worden in de stap ter voorbereiding van gegevens naar een indeling maand/gepartitioneerd. Deze partitioneren bevordert efficiënter uitvoeren van query's en schaalbare langetermijnopslag doordat fault-over van een blob-account naar de volgende omdat het eerste account vol is. 

>[!NOTE] 
>Deze stap in de oplossing is alleen van toepassing op batchverwerking.

Invoer en uitvoer gegevensbeheer gegevens:

* De **uitvoergegevens** (met het label *PartitionedCarEventsTable*) moet worden bewaard gedurende lange tijd als de fundamentele / 'rawest' vorm van gegevens in de klant 'Data Lake'. 
* De **invoergegevens** aan deze pijplijn zou doorgaans worden verwijderd, omdat de uitvoergegevens volledige overeenkomen met de invoer heeft-alleen wordt opgeslagen (gepartitioneerd) beter voor later gebruik.

![Partitie auto gebeurtenissen werkstroom](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

*Afbeelding 10 – partitie auto gebeurtenissen werkstroom*

De onbewerkte gegevens is gepartitioneerd met behulp van een HDInsight Hive-activiteit in 'PartitionCarEventsPipeline'. De voorbeeldgegevens in stap 1 van een jaar gegenereerd is per jaar/maand gepartitioneerd. De partities worden gebruikt voor het genereren van vehicle signalen en diagnostische gegevens voor elke maand (totaal 12 partities) van een jaar. 

![PartitionCarEventsPipeline activiteit](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

*Afbeelding 11 - PartitionCarEventsPipeline*

***PartitionConnectedCarEvents Hive-Script***

De volgende Hive-script met de naam 'partitioncarevents.hql' wordt gebruikt voor het partitioneren en bevindt zich in de map '\demo\src\connectedcar\scripts' van het gedownloade ZIP-bestand. 
    
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

Nadat de pijplijn met succes is uitgevoerd, ziet u de volgende partities in uw opslagaccount in de container 'connectedcar' wordt gegenereerd.

![Gepartitioneerde uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

*Afbeelding 12 - gepartitioneerde uitvoer*

De gegevens nu is geoptimaliseerd, beter beheerbaar en gereed voor verdere verwerking voor het verkrijgen van inzicht uitgebreide batch. 

## <a name="data-analysis"></a>Data-analyse
In deze sectie ziet u het combineren van Azure Stream Analytics, Azure Machine Learning, Azure Data Factory en Azure HDInsight voor rich geavanceerde analyses van vehicle status en die zorg draagt gewoonten. Er zijn drie subsecties hier:

1. **Machine Learning**: in deze subsectie bevat informatie over het detectie-experiment voor afwijkingsdetectie die we in deze oplossing hebt gebruikt om te voorspellen voertuigen vereisen onderhoud onderhoud en voertuigen teruggehaald vanwege problemen met de veiligheid vereisen.
2. **Analyse van realtime**: in deze subsectie bevat informatie over de realtime-analyses met behulp van de Stream Analytics Query Language en operationele het machine learning-experiment in realtime met een aangepaste toepassing.
3. **Batch-analyse**: in deze subsectie bevat informatie met betrekking tot de transformeren en verwerking van de batch-gegevens met Azure HDInsight en Azure Machine Learning geoperationaliseerd door Azure Data Factory.

### <a name="machine-learning"></a>Machine Learning
Hier ons doel is om te voorspellen van de voertuigen waarvoor onderhoud of intrekken op basis van bepaalde Health statistieken. We maken de volgende veronderstellingen

* Als een van de volgende drie voorwaarden voldaan wordt, wordt de voertuigen vereisen **onderhoud onderhoud**:
  
  * Band druk is laag
  * Engine olie niveau is laag
  * Engine temperatuur is hoog
* Als een van de volgende voorwaarden voldaan wordt, de voertuigen wellicht een **veiligheid probleem** en vereisen **intrekken**:
  
  * Engine temperatuur hoog is, maar externe temperatuur is laag
  * Engine temperatuur laag is, maar externe temperatuur is hoog

Wij hebben twee afzonderlijke modellen om te detecteren afwijkingen, één voor de detectie van vehicle onderhoud en één voor vehicle intrekken detectie op basis van de vorige vereisten, gemaakt. In beide modellen, worden de ingebouwde Principal onderdeel Analysis (Pso)-algoritme wordt gebruikt voor afwijkingsdetectie. 

**Model voor onderhoud**

Als een van drie indicatoren - band druk, engine olie of engine temperatuur - aan de respectieve voorwaarde voldoet, rapporteert het model van de detectie onderhoud een afwijkingsdetectie. Als gevolg hiervan moet we alleen rekening houden met deze drie variabelen bij het bouwen van het model. In ons experiment in Azure Machine Learning, gebruiken we eerst een **Select Columns in Dataset** module uitpakken van deze drie variabelen. We gebruiken de detectiemodule PCA-gebaseerd anomaliedetectie naast het afwijkingsdetectie detectie model bouwen. 

Principal onderdeel analyse (Pso) is een techniek tot stand gebrachte in machine learning die kan worden toegepast op Functieselectie, classificatie en anomaly detection. PCA zet een set krat met mogelijk gecorreleerde variabelen in een set waarden principal onderdelen genoemd. Het belangrijkste idee modellering PCA-gebaseerd is om gegevens naar een lager dimensionale ruimte om functies en afwijkingen kunnen gemakkelijk worden geïdentificeerd.

Voor elke nieuwe invoer in het model van de detectie van de detectie van de afwijkingsdetectie eerst berekent de projectie op de eigenvectors en berekent vervolgens de genormaliseerde herstel-fout. Deze fout genormaliseerde is de score afwijkingsdetectie. Hoe hoger de fout en de afwijkende wordt het exemplaar is. 

In het probleem van de detectie onderhoud kunt elke record worden beschouwd als een punt in een 3-dimensionale ruimte gedefinieerd door band druk, engine olie en temperatuur motor coördinaten. Voor het vastleggen van deze afwijkingen kunnen we de oorspronkelijke gegevens in de ruimte 3-dimensionale project naar een 2-dimensionale ruimte met PCA. We stellen dus de parameter aantal onderdelen te gebruiken in PCA om 2. Deze parameter speelt een belangrijke rol bij het toepassen van PCA-gebaseerd anomaliedetectie. Na projecteren gegevens met behulp van Pso, kunnen we deze afwijkingen gemakkelijker identificeren.

**Model voor afwijkingsdetectie intrekken** In het model intrekken afwijkingsdetectie detectie, gebruiken we de Select Columns in Dataset en PCA-gebaseerd anomaliedetectie detectiemodules op soortgelijke wijze. In het bijzonder we eerst uitpakken drie variabelen - buiten temperatuur- en snelheid engine temperatuur - met behulp van de **Select Columns in Dataset** module. We ook de variabele snelheid omdat de engine temperatuur doorgaans wordt gecorreleerd met de snelheid. We gebruiken PCA-gebaseerd anomaliedetectie detectiemodule naast de gegevens vanuit de ruimte 3-dimensionale projecteren naar een 2-dimensionale ruimte. De intrekken criteria is voldaan en dus intrekken door de drager is vereist wanneer engine temperatuur- en externe temperatuur maximaal negatieve worden gecorreleerd. PCA-gebaseerd anomaliedetectie-algoritme kunnen we de afwijkingen vastleggen na het uitvoeren van Pso. 

Bij het model trainen, moeten we normale gegevens waarvoor geen vereist onderhoud of intrekken als de invoergegevens voor het trainen van het model van de detectie PCA-gebaseerd anomaliedetectie gebruiken. In het experiment scoreprofiel gebruiken we het model van de detectie getraind afwijkingsdetectie om te detecteren of de drager vereist onderhoud of intrekken. 

### <a name="real-time-analysis"></a>Realtime analyses
De volgende Stream Analytics SQL-Query wordt gebruikt voor het ophalen van het gemiddelde van alle belangrijke vehicle-parameters zoals vehicle snelheid, brandstofpeil engine temperatuur, kilometerstand, band druk, engine olie niveau en anderen. De gemiddelden worden gebruikt voor afwijkingen gedetecteerd, waarschuwingen, uitgeven en de algehele status vaststelling van voertuigen gerund in specifieke regio en deze vervolgens correleert met demografische gegevens. 

![Stream Analytics query voor realtime verwerking](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

*Afbeelding 13-Stream Analytics query voor realtime verwerking*

Alle gemiddelden worden via een TumblingWindow 3 seconden berekend. We gebruiken TubmlingWindow in dit geval omdat we is vereist voor niet-overlappende en aaneengesloten tijdsintervallen. 

Voor meer informatie over alle mogelijkheden van de 'Windowing' in Azure Stream Analytics, klikt u op [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Realtime voorspelling**

Er is een toepassing opgenomen als onderdeel van de oplossing voor de machine learning-model in realtime operationeel. Deze toepassing met de naam 'RealTimeDashboardApp' is gemaakt en geconfigureerd als onderdeel van de implementatie van de oplossing. De toepassing, voert het volgende:

1. Luistert naar een Event Hub-instantie waar Stream Analytics de gebeurtenissen in een patroon continu publiceert. ![Stream Analytics query voor het publiceren van de gegevens](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *afbeelding 14 – Stream Analytics query voor het publiceren van de gegevens naar uitvoer Event Hub-instantie* 
2. Voor elke gebeurtenis die deze toepassing ontvangt: 
   
   * De gegevens met behulp van Machine Learning aanvragen en antwoorden score berekenen (RR's) eindpunt verwerkt. Het eindpunt RR's wordt automatisch gepubliceerd als onderdeel van de implementatie.
   * De uitvoer RRS is gepubliceerd op een Power BI-gegevensset met behulp van de push-API's.

Dit patroon is ook van toepassing op scenario's waarin u wilt integreren van een Line of Business (LoB)-toepassing met de stroom realtime analyses voor scenario's zoals waarschuwingen, meldingen en messaging.

Klik op [RealtimeDashboardApp downloaden](http://go.microsoft.com/fwlink/?LinkId=717078) voor het downloaden van de RealtimeDashboardApp Visual Studio-oplossing voor aanpassingen. 

**De Real-time dashboardtoepassing uitvoeren**
1. Uitpakken en lokaal opslaan ![RealtimeDashboardApp map](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *afbeelding 16 – RealtimeDashboardApp map*  
2. De toepassing RealtimeDashboardApp.exe uitvoeren
3. Geldige Power BI-referenties opgeven, meld u aan en klik op accepteren ![Realtime dashboard-app aanmelden bij Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Realtime dashboard-app aanmelden bij Power BI voltooien](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Afbeelding 17 – RealtimeDashboardApp: Aanmelden bij Power BI*

>[!NOTE] 
>Als u wilt leegmaken van de Power BI-gegevensset, voert u de RealtimeDashboardApp met de parameter 'flushdata' uit: 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Batchanalyse
Dit dient om weer te geven hoe de Azure compute-mogelijkheden voor het benutten van big data voor het verkrijgen van veel inzichten gerichtheid patroon, gebruik gedrag en vehicle health maakt gebruik van Contoso motoren. Dit maakt het mogelijk om te:

* Verbetering van de gebruikerservaring en goedkoper maken door op te geven inzicht gerichtheid gewoonten en efficiënte aangedreven gedrag brandstof
* Meer informatie over proactief over klanten en hun aangedreven patters om te bepalen zakelijke beslissingen te nemen en bieden de beste klasse-producten en services

In deze oplossing ontwikkelt we de volgende metrische gegevens:

1. **Agressief gedrag besturen**: identificeert de trend van de modellen, locaties, aangedreven voorwaarden en tijd van het jaar om inzicht in agressief aangedreven patronen te krijgen. Contoso motoren kunnen deze inzichten gebruiken voor marketingcampagnes, nieuwe functies voor persoonlijke en verzekering op basis van informatie over het gebruik.
2. **Efficiënte aangedreven gedrag brandstof**: identificeert de trend van de modellen, locaties, aangedreven voorwaarden en tijd van het jaar om inzicht op brandstof efficiënt aangedreven patronen te krijgen. Contoso motoren kunt deze inzichten voor marketingcampagnes, kunnen nieuwe functies en effectief en omgeving beschrijvende aangedreven gewoonten proactieve rapporteren aan de stuurprogramma's voor kosten. 
3. **Intrekken van modellen**: identificeert modellen teruggehaald waarvoor de afwijkingsdetectie detectie machine learning-experiment operationele

We bekijken in de details van elk van deze metrische gegevens,

**Agressieve aangedreven patroon**

De gepartitioneerde vehicle signalen en diagnostische gegevens worden verwerkt in de pijplijn met de naam 'AggresiveDrivingPatternPipeline' met Hive om te bepalen van de modellen, locatie, vehicle, aangedreven voorwaarden en andere parameters die agressieve besturen vertoont patroon.

![Agressieve aangedreven patroon werkstroom](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*figuur 18 – agressieve aangedreven patroon-werkstroom*


***Agressieve aangedreven patroon Hive-query***

Het Hive-script met de naam 'aggresivedriving.hql"gebruikt voor het analyseren van agressieve aangedreven voorwaarde patroon bevindt zich in de map '\demo\src\connectedcar\scripts' van het gedownloade ZIP-bestand. 

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


Met behulp van de combinatie van verzending tandwielpictogram positie, bedient vorm status en snelheid van vehicle wordt roekeloze/agressieve aangedreven gedrag op basis van het patroon met hoge snelheid remmen detecteren. 

Nadat de pijplijn met succes is uitgevoerd, ziet u de volgende partities in uw opslagaccount in de container 'connectedcar' wordt gegenereerd.

![AggressiveDrivingPatternPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Afbeelding 19-AggressiveDrivingPatternPipeline uitvoer*

**Brandstof efficiënt aangedreven patroon**

De gepartitioneerde vehicle signalen en diagnostische gegevens worden verwerkt in de pijplijn met de naam 'FuelEfficientDrivingPatternPipeline'. Hive wordt gebruikt om te bepalen van de modellen, locatie, vehicle, aangedreven voorwaarden en andere eigenschappen die brandstof efficiënt aangedreven patroon vertonen.

![Fuel-Efficient aangedreven patroon](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Afbeelding 20 – Fuel-efficient aangedreven patroon-werkstroom*

***Brandstof efficiënt aangedreven patroon Hive-query***

Het Hive-script met de naam 'fuelefficientdriving.hql"gebruikt voor het analyseren van agressieve aangedreven voorwaarde patroon bevindt zich in de map '\demo\src\connectedcar\scripts' van het gedownloade ZIP-bestand. 

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


De combinatie van de drager transmission tandwielpictogram positie wordt gebruikt, bedient vorm status, de snelheid en de accelerator positie voor het detecteren van brandstof efficiënt aangedreven gedrag op basis van versnelling, remvermogen pedaal en patronen te versnellen. 

Nadat de pijplijn met succes is uitgevoerd, ziet u de volgende partities in uw opslagaccount in de container 'connectedcar' wordt gegenereerd.

![FuelEfficientDrivingPatternPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Afbeelding 21 – FuelEfficientDrivingPatternPipeline uitvoer*

**Voorspellingen intrekken**

De machine learning-experiment worden ingericht en gepubliceerd als een webservice als onderdeel van de implementatie van de oplossing. De score-eindpunt wordt gebruikt in deze werkstroom geregistreerd als een data factory gekoppelde service en geoperationaliseerd met behulp van data factory batch activiteit voor score berekenen.

![Machine Learning-eindpunt](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

*Afbeelding 22 – Machine learning-eindpunt is geregistreerd als een gekoppelde service in de gegevensfactory*

De geregistreerde gekoppelde service wordt gebruikt in de DetectAnomalyPipeline voor de beoordeling van de gegevens met behulp van de afwijkingsdetectie detectie-model. 

![Machine Learning score activiteit in de gegevensfactory](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

*Afbeelding 23: Azure Machine Learning-Batchscoreberekening activiteit in de gegevensfactory* 

Er zijn enkele stappen die worden uitgevoerd in deze pijplijn voor het voorbereiden van gegevens, zodat deze kan worden geoperationaliseerd met de webservice voor de score. 

![DetectAnomalyPipeline voor het voorspellen van voertuigen terughalen vereisen](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

*Afbeelding 24 – DetectAnomalyPipeline voor het voorspellen van voertuigen terughalen vereisen* 

***Afwijkingsdetectie detectie Hive-query***

Zodra de score berekenen is voltooid, wordt een HDInsight-activiteit gebruikt om te verwerken en samenvoegen van de gegevens die gecategoriseerd als afwijkingen door het model met een goede kans score 0,60 of hoger worden.

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


Nadat de pijplijn met succes is uitgevoerd, ziet u de volgende partities in uw opslagaccount in de container 'connectedcar' wordt gegenereerd.

![DetectAnomalyPipeline uitvoer](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Afbeelding 25 – DetectAnomalyPipeline uitvoer*

## <a name="publish"></a>Publiceren

### <a name="real-time-analysis"></a>Realtime analyses
Een van de query's in de Stream Analytics-taak de gebeurtenissen publiceert naar uitvoer Event Hub-instantie. 

![Stream Analytics-taak publiceert naar uitvoer Event Hub-instantie](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Afbeelding 26 – Stream Analytics-taak naar uitvoer publiceert Event Hub-instantie*

![Stream Analytics query om te publiceren naar de uitvoer van de Event Hub-instantie](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Afbeelding 27 – Stream Analytics query om te publiceren naar de uitvoer van de Event Hub-instantie*

Deze stroom van gebeurtenissen wordt gebruikt door de RealTimeDashboardApp opgenomen in de oplossing. Deze toepassing maakt gebruik van de webservice voor de Machine Learning-reactie op aanvragen voor score berekenen voor realtime en de resulterende gegevens publiceert naar een gegevensset met de Power BI voor verbruik. 

### <a name="batch-analysis"></a>Batchanalyse
De resultaten van de batch- en realtime verwerking worden gepubliceerd op de Azure SQL Database-tabellen voor verbruik. De Azure SQL-Server, Database en de tabellen worden automatisch gemaakt als onderdeel van het installatiescript. 

![Batchverwerking resultaten kopiëren naar de werkstroom datamart data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Afbeelding 28 – batchverwerking resultaten kopiëren naar de werkstroom datamart data*

![Stream Analytics-taak publiceert naar datamart](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Afbeelding 29 – Stream Analytics-taak naar datamart publiceert*

![Datamart-instelling van Data in Stream Analytics-taak](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Afbeelding 30 – datamart instellen in Stream Analytics-taak*

## <a name="consume"></a>Gebruiken
Power BI biedt deze oplossing een uitgebreide dashboard voor realtime-gegevens en visualisaties predictive analytics. 

Klik hier voor gedetailleerde instructies voor het instellen van de Power BI-rapporten en het dashboard. Het laatste dashboard ziet er als volgt:

![Power BI-dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

*Afbeelding 31 - Power BI-Dashboard*

## <a name="summary"></a>Samenvatting
Dit document bevat een gedetailleerde inzoomen van de drager telemetrie Analytics-oplossing. Dit patroon met een lambda-architectuur voor gepresenteerd realtime en batch-analyses met voorspellingen en acties. Dit patroon van toepassing is op een breed scala aan gebruiksvoorbeelden waarvoor hot pad (real-time) en analyses van koude pad (batch). 

