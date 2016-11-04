---
title: Walkthrough over vooraf geconfigureerde oplossing voor externe controle | Microsoft Docs
description: Een beschrijving van de vooraf geconfigureerde oplossing voor externe controle IoT Azure en de bijbehorende architectuur.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: dobett

---
# Walkthrough over vooraf geconfigureerde oplossing voor externe controle
## Inleiding
De [vooraf geconfigureerde oplossing][Ink-preconfigured-solutions] voor externe controle met IoT Suite is een implementatie van een end-to-endoplossing voor controle voor meerdere computers die actief zijn op externe locaties. De oplossing combineert belangrijke Azure-services voor een algemene implementatie van het bedrijfsscenario. Bovendien kunt u de oplossing gebruiken als uitgangspunt voor uw eigen implementatie. U kunt de oplossing [aanpassen][lnk-customize] zodat deze voldoet aan uw eigen specifieke zakelijke vereisten.

In dit artikel wordt stapsgewijs een aantal belangrijke elementen van externe controle beschreven, zodat u beter begrijpt hoe dit werkt. Deze kennis helpt u bij:

* Het oplossen van problemen met de oplossing.
* Het plannen van een aanpassing van de oplossing zodat deze voldoet aan uw eigen specifieke vereisten. 
* Het ontwerpen van uw eigen IoT-oplossing die gebruikmaakt van Azure-services.

## Logische architectuur
Het volgende diagram geeft een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing:

![Logische architectuur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## Gesimuleerde apparaten
In de vooraf geconfigureerde oplossing vertegenwoordigt het gesimuleerde apparaat een koelapparaat (zoals een airconditioner of een luchtververser in een gebouw). Wanneer u de vooraf geconfigureerde oplossing implementeert, richt u ook automatisch vier gesimuleerde apparaten in waarop een [Azure-webtaak][lnk-webjobs] wordt uitgevoerd. Met de gesimuleerde apparaten kunt u eenvoudig het gedrag van de oplossing bekijken zonder dat u fysieke apparaten hoeft te implementeren. Zie de zelfstudie [Connect your device to the remote monitoring preconfigured solution][Ink-connect-rm] (Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe controle) om een echt fysiek apparaat te implementeren.

Via elk gesimuleerd apparaat kunnen de volgende berichttypen worden verzonden naar IoT Hub:

| Bericht | Beschrijving |
| --- | --- |
| Opstarten |Wanneer het apparaat start, wordt er een bericht over **apparaatgegevens** verzonden naar de back-end met hierin informatie over het apparaat. Deze gegevens bevatten de apparaat-id, de apparaatmetagegevens, een lijst met de opdrachten die worden ondersteund op het apparaat en de huidige configuratie van het apparaat. |
| Aanwezigheid |Via een apparaat wordt periodiek een bericht verzonden over **aanwezigheid** om te melden of het apparaat de aanwezigheid van een sensor bemerkt. |
| Telemetrie |Via een apparaat wordt periodiek een bericht verzonden over **telemetrie** waarin gesimuleerde waarden voor de temperatuur en vochtigheid worden gemeld die zijn verzameld met de gesimuleerde sensoren van het apparaat. |

Via de gesimuleerde apparaten worden de volgende apparaateigenschappen verzonden in een bericht over **apparaatgegevens**:

| Eigenschap | Doel |
| --- | --- |
| Apparaat-id |De id die met het apparaat wordt verstrekt of die is toegewezen als een apparaat in de oplossing wordt gemaakt. |
| Fabrikant |Fabrikant van apparaat |
| Modelnummer |Modelnummer van het apparaat |
| Serienummer |Serienummer van het apparaat |
| Firmware |Huidige versie van de firmware op het apparaat |
| Platform |Platformarchitectuur van het apparaat |
| Processor |Processor waarop het apparaat wordt uitgevoerd |
| Geïnstalleerd RAM-geheugen |Hoeveelheid RAM-geheugen dat op het apparaat is geïnstalleerd |
| Status hub ingeschakeld |Eigenschap IoT Hub-status van het apparaat |
| Gemaakt om |Tijd waarop het apparaat in de oplossing is gemaakt |
| Bijgewerkt om |De tijd waarop de laatste keer de eigenschappen van het apparaat zijn bijgewerkt |
| Breedtegraad |Breedtegraad van locatie waar het apparaat zich bevindt |
| Lengtegraad |Lengtegraad van locatie waar het apparaat zich bevindt |

De simulator voorziet deze eigenschappen in de gesimuleerde apparaten van voorbeeldwaarden.  Telkens wanneer de simulator een gesimuleerd apparaat initialiseert, plaatst het apparaat de vooraf gedefinieerde metagegevens in IoT Hub. Houd er rekening mee dat hiermee alle in de portal voor apparaten bijgewerkte metagegevens worden overschreven.

De gesimuleerde apparaten kunnen de volgende opdrachten verwerken die van het dashboard van de oplossing worden verzonden via de IoT Hub:

| Opdracht | Beschrijving |
| --- | --- |
| PingDevice |Hiermee wordt een *ping* verzonden naar het apparaat om te controleren of het actief is |
| StartTelemetry |Geeft het apparaat opdracht om te beginnen met het verzenden van telemetriegegevens |
| StopTelemetry |Geeft het apparaat opdracht om te stoppen met het verzenden van telemetriegegevens |
| ChangeSetPointTemp |Hiermee wordt de waarde van het instelpunt gewijzigd waaromheen de willekeurige gegevens worden gegenereerd |
| DiagnosticTelemetry |Hiermee wordt de apparaatsimulator geactiveerd voor het verzenden van een extra telemetriewaarde (externalTemp) |
| ChangeDeviceState |Hiermee wordt een eigenschap uitgebreide status voor het apparaat gewijzigd en wordt het bericht met apparaatgegevens verzonden vanaf het apparaat |

De bevestiging van de opdracht voor het apparaat wordt via IoT Hub verzonden naar de back-end van de oplossing.

## IoT Hub
De [IoT Hub][Ink-iothub] neemt gegevens op die vanaf de apparaten worden verzonden naar de cloud, en maakt ze beschikbaar voor de ASA-taken (Azure Stream Analytics). Via IoT Hub worden ook opdrachten naar de apparaten verzonden namens de portal voor apparaten. Voor elke stream maakt de ASA-taak gebruik van een afzonderlijke IoT Hub-consumentengroep om de stroom berichten van de apparaten te lezen.

## Azure Stream Analytics
Bij de oplossing voor externe controle worden via [ASA (Azure Stream Analytics)][Ink-asa] apparaatberichten die zijn ontvangen via de IoT Hub, verzonden naar andere back-endonderdelen voor verwerking of opslag. Met verschillende ASA-taken worden specifieke functies uitgevoerd gebaseerd op de inhoud van de berichten.

**Taak 1: apparaatgegevens** filtert berichten met apparaatgegevens uit de binnenkomende berichtenstroom en stuurt deze naar een Event Hub-eindpunt. Via een apparaat worden berichten met apparaatgegevens verzonden bij het opstarten en als reactie op een opdracht **SendDeviceInfo**. Tijdens deze taak wordt gebruikgemaakt van de volgende querydefinitie om berichten over **apparaatgegevens** te identificeren:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Tijdens deze taak wordt de uitvoer verzonden naar een Event Hub voor verdere verwerking.

**Taak 2: regels** evalueren de binnenkomende telemetriegegevens over temperatuur en vochtigheid aan de hand van de drempelwaarden voor elk apparaat. Drempelwaarden kunnen worden ingesteld met de regeleditor die beschikbaar is op het dashboard van de oplossing. Elk koppel apparaat/waarde wordt per tijdstempel opgeslagen in een blob die met Stream Analytics wordt gelezen als **referentiegegevens**. De taak vergelijkt elke niet-lege waarde met de ingestelde drempelwaarde voor het apparaat. Als deze de voorwaarde > overschrijdt, bestaat de taakuitvoer uit een **waarschuwings**gebeurtenis die aangeeft dat de drempelwaarde is overschreden, en worden tevens de waarden voor het apparaat, de waarde en de tijdstempel gegeven. Voor deze taak wordt gebruikgemaakt van de volgende querydefinitie om berichten over telemetrie te identificeren die een alarm moeten activeren:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Tijdens deze taak wordt de uitvoer verzonden naar een Event Hub voor verdere verwerking. Ook worden details van elke waarschuwing opgeslagen in de blobopslag, waar de informatie uit de waarschuwing kan worden gelezen op het dashboard van de oplossing.

**Taak 3: telemetrie** is op twee manieren actief op de binnenkomende stroom telemetriegegevens van het apparaat. Allereerst worden alle telemetrieberichten vanaf de apparaten naar een permanente blobopslag verzonden voor langdurige opslag. Ten tweede worden elke vijf minuten de gemiddelde, minimale en maximale vochtigheidswaarden berekend. Deze gegevens worden verzonden naar de blobopslag. Met het dashboard van de oplossing worden de telemetriegegevens uit de blobopslag gelezen om de grafieken te vullen. Tijdens deze taak wordt gebruikgemaakt van de volgende querydefinitie:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## Event Hubs
De ASA-taken voor **apparaatgegevens** en **regels** voeren hun gegevens uit naar Event Hubs die zo kunnen worden doorgestuurd naar de **gebeurtenisprocessor** die actief is in de webtaak.

## Azure-opslag
De oplossing maakt gebruik van Azure Blob-opslag om alle ruwe en samengevatte telemetriegegevens op de apparaten in de oplossing te behouden. Met het dashboard worden de telemetriegegevens uit de blobopslag gelezen om de grafieken te vullen. Voor het weergeven van waarschuwingen worden met het dashboard de gegevens uit de blobopslag gelezen waarin is vastgelegd wanneer telemetriewaarden de geconfigureerde drempelwaarden hebben overschreden. Bij de oplossing wordt ook gebruikgemaakt van blobopslag om de drempelwaarden vast te leggen die u hebt ingesteld op het dashboard.

## Webtaken
Naast de apparaatsimulatoren hosten de webtaken in de oplossing ook de **gebeurtenisprocessor** die actief is in een Azure-webtaak en die informatieberichten voor het apparaat en opdrachtantwoorden verwerkt. Hierbij:

* Worden berichten met apparaatgegevens gebruikt om het apparaatregister bij te werken (opgeslagen in de DocumentDB-database) met de huidige apparaatgegevens.
* Worden berichten met reacties op opdrachten gebruikt om de opdrachtgeschiedenis van het apparaat bij te werken (opgeslagen in de DocumentDB-database).

## DocumentDB
De oplossing maakt gebruik van een DocumentDB-database om informatie op te slaan over de apparaten die zijn verbonden aan de oplossing. Deze informatie omvat de apparaatmetagegevens en de opdrachtengeschiedenis die vanuit het dashboard zijn verzonden naar het apparaat.

## Web-apps
### Het dashboard Externe controle
Deze pagina in de webtoepassing maakt gebruik van Power BI javascript-besturingselementen (zie [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)) om de telemetriegegevens van de apparaten te visualiseren. De oplossing maakt gebruik van de ASA-telemetrietaak om de telemetriegegevens naar de blobopslag te schrijven.

### Portal voor apparaatbeheer
Met deze web-app kunt u:

* Nieuwe apparaten inrichten. Met deze actie wordt het unieke apparaat-id ingesteld en de verificatiesleutel gegenereerd. Er wordt informatie over het apparaat geschreven naar het IoT Hub-identiteitsregister en de oplossingsspecifieke DocumentDB-database.
* Apparaateigenschappen beheren. Deze actie omvat het weergeven van bestaande eigenschappen en het bijwerken met nieuwe eigenschappen.
* Opdrachten naar een apparaat verzenden.
* De opdrachtgeschiedenis voor een apparaat weergeven.
* Apparaten in- en uitschakelen.

## Volgende stappen
De volgende TechNet-blogberichten bieden meer details over de vooraf geconfigureerde oplossing voor externe controle:

* [IoT Suite - Under The Hood - Remote Monitoring (IoT Suite - achter de schermen - externe controle) ](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [IoT Suite - Remote Monitoring - Adding Live and Simulated Devices (IoT Suite - externe controle - live- en gesimuleerde apparaten toevoegen)](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

U kunt verder aan de slag gaan met IoT Suite door de volgende artikelen te lezen:

* [Connect your device to the remote monitoring preconfigured solution][Ink-connect-rm] (Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe controle)
* [Permissions on the azureiotsuite.com site][lnk-permissions] (Machtigingen op de site azureiotsuite.com)

[Ink-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[Ink-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[Ink-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[Ink-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md


<!--HONumber=Sep16_HO3-->


