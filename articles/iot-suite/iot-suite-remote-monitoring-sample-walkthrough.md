---
title: Walkthrough over vooraf geconfigureerde oplossing voor externe controle | Microsoft Docs
description: Een beschrijving van de vooraf geconfigureerde oplossing voor externe controle IoT Azure en de bijbehorende architectuur.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: b28105f300723b542fa6d1aebc569439d5c73dc4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Walkthrough over vooraf geconfigureerde oplossing voor externe controle

De [vooraf geconfigureerde oplossing][lnk-preconfigured-solutions] voor externe controle met IoT Suite is een implementatie van een end-to-endoplossing voor controle voor meerdere computers die op externe locaties worden gebruikt. De oplossing combineert belangrijke Azure-services voor een algemene implementatie van het bedrijfsscenario. U kunt de oplossing gebruiken als uitgangspunt voor uw eigen implementatie en u kunt deze [aanpassen][lnk-customize] aan uw eigen specifieke zakelijke vereisten.

In dit artikel wordt stapsgewijs een aantal belangrijke elementen van externe controle beschreven, zodat u beter begrijpt hoe dit werkt. Deze kennis helpt u bij:

* Het oplossen van problemen met de oplossing.
* Het plannen van een aanpassing van de oplossing zodat deze voldoet aan uw eigen specifieke vereisten. 
* Het ontwerpen van uw eigen IoT-oplossing die gebruikmaakt van Azure-services.

## <a name="logical-architecture"></a>Logische architectuur

Het volgende diagram geeft een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing:

![Logische architectuur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="simulated-devices"></a>Gesimuleerde apparaten

In de vooraf geconfigureerde oplossing vertegenwoordigt het gesimuleerde apparaat een koelapparaat (zoals een airconditioner of een luchtververser in een gebouw). Wanneer u de vooraf geconfigureerde oplossing implementeert, richt u ook automatisch vier gesimuleerde apparaten in waarop een [Azure-webtaak][lnk-webjobs] wordt uitgevoerd. Met de gesimuleerde apparaten kunt u eenvoudig het gedrag van de oplossing bekijken zonder dat u fysieke apparaten hoeft te implementeren. Zie de zelfstudie [Connect your device to the remote monitoring preconfigured solution][lnk-connect-rm] (Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe controle) als u een echt fysiek apparaat wilt implementeren.

### <a name="device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten

Via elk gesimuleerd apparaat kunnen de volgende berichttypen worden verzonden naar IoT Hub:

| Bericht | Beschrijving |
| --- | --- |
| Opstarten |Wanneer het apparaat start, wordt er een bericht over **apparaatgegevens** verzonden naar de back-end met hierin informatie over het apparaat. Deze gegevens omvatten de apparaat-id en een lijst met de opdrachten en methoden die door het apparaat worden ondersteund. |
| Aanwezigheid |Via een apparaat wordt periodiek een bericht verzonden over **aanwezigheid** om te melden of het apparaat de aanwezigheid van een sensor bemerkt. |
| Telemetrie |Via een apparaat wordt periodiek een bericht verzonden over **telemetrie** waarin gesimuleerde waarden voor de temperatuur en vochtigheid worden gemeld die zijn verzameld met de gesimuleerde sensoren van het apparaat. |

> [!NOTE]
> De oplossing slaat de lijst met opdrachten die door het apparaat worden ondersteund, op in een Cosmos DB-database en niet in de apparaatdubbel.

### <a name="properties-and-device-twins"></a>Eigenschappen en apparaatdubbels

De gesimuleerde apparaten verzenden de volgende apparaateigenschappen als *gerapporteerde eigenschappen* naar de [dubbele][lnk-device-twins] in de IoT Hub. Het apparaat verzendt gerapporteerde eigenschappen bij het opstarten en als reactie op een opdracht of methode voor het **wijzigen van de apparaatstatus**.

| Eigenschap | Doel |
| --- | --- |
| Config.TelemetryInterval | Frequentie (seconden) waarmee het apparaat telemetrie verzendt |
| Config.TemperatureMeanValue | Geeft de gemiddelde waarde voor de gesimuleerde temperatuurtelemetrie |
| Device.DeviceID |De id die wordt verstrekt of toegewezen wanneer een apparaat in de oplossing wordt gemaakt |
| Device.DeviceState | Status die door het apparaat wordt gemeld |
| Device.CreatedTime |Tijd waarop het apparaat in de oplossing is gemaakt |
| Device.StartupTime |Het tijdstip waarop het apparaat is gestart |
| Device.LastDesiredPropertyChange |Het versienummer van de laatst gewenste eigenschapswijziging |
| Device.Location.Latitude |Breedtegraad van locatie waar het apparaat zich bevindt |
| Device.Location.Longitude |Lengtegraad van locatie waar het apparaat zich bevindt |
| System.Manufacturer |Fabrikant van apparaat |
| System.ModelNumber |Modelnummer van het apparaat |
| System.SerialNumber |Serienummer van het apparaat |
| System.FirmwareVersion |Huidige versie van de firmware op het apparaat |
| System.Platform |Platformarchitectuur van het apparaat |
| System.Processor |Processor waarop het apparaat wordt uitgevoerd |
| System.InstalledRAM |Hoeveelheid RAM-geheugen dat op het apparaat is geïnstalleerd |

De simulator voorziet deze eigenschappen in de gesimuleerde apparaten van voorbeeldwaarden. Telkens wanneer de simulator een gesimuleerd apparaat initialiseert, rapporteert het apparaat de vooraf gedefinieerde metagegevens als gerapporteerde eigenschappen aan IoT Hub. Gerapporteerde eigenschappen kunnen alleen worden bijgewerkt door het apparaat. Als u een gerapporteerde eigenschap wilt wijzigen, stelt u in de oplossingsportal een gewenste eigenschap in. Het is de verantwoordelijkheid van het apparaat om:

1. Regelmatig gewenste eigenschappen op te halen uit de IoT Hub.
2. Diens configuratie bij te werken met de gewenste eigenschapswaarde.
3. De nieuwe waarde weer als een gerapporteerde eigenschap terug te sturen naar de hub.

Vanuit het dashboard van de oplossing kunt u *gewenste eigenschappen* gebruiken om eigenschappen op een apparaat in te stellen met behulp van de [apparaatdubbel][lnk-device-twins]. Gewoonlijk haalt een apparaat de waarde van een gewenste eigenschap op uit de hub om diens interne status bij te werken en de wijziging weer als een gerapporteerde eigenschap terug te melden.

> [!NOTE]
> De code van het gesimuleerde apparaat gebruikt alleen de gewenste eigenschappen **Desired.Config.TemperatureMeanValue** en **Desired.Config.TelemetryInterval** om de gerapporteerde eigenschappen bij te werken die naar IoT Hub worden teruggestuurd. Alle andere aanvragen om gewenste eigenschappen te wijzigen worden in het gesimuleerde apparaat genegeerd.

### <a name="methods"></a>Methoden

De gesimuleerde apparaten kunnen de volgende methoden ([directe methoden][lnk-direct-methods]) afhandelen die via de IoT Hub vanuit oplossingsportal zijn geactiveerd:

| Methode | Beschrijving |
| --- | --- |
| InitiateFirmwareUpdate |Geeft het apparaat de opdracht om een firmware-update uit te voeren |
| Opnieuw opstarten |Geeft het apparaat de opdracht opnieuw op te starten |
| FactoryReset |Geeft het apparaat de opdracht om de fabrieksinstellingen terug te zetten |

Sommige methoden gebruiken gerapporteerde eigenschappen om te rapporteren over hun voortgang. Zo simuleert de methode **InitiateFirmwareUpdate** bijvoorbeeld een asynchrone uitvoering van de update op het apparaat. De methode keert onmiddellijk terug naar het apparaat terwijl de asynchrone taak met behulp van gerapporteerde eigenschappen statusupdates naar het dashboard van de oplossing blijft verzenden.

### <a name="commands"></a>Opdrachten

De gesimuleerde apparaten kunnen de volgende opdrachten (cloud-naar-apparaat-berichten) afhandelen die vanuit de oplossingsportal via de IoT Hub worden verzonden:

| Opdracht | Beschrijving |
| --- | --- |
| PingDevice |Hiermee wordt een *ping* verzonden naar het apparaat om te controleren of het actief is |
| StartTelemetry |Geeft het apparaat opdracht om te beginnen met het verzenden van telemetriegegevens |
| StopTelemetry |Geeft het apparaat opdracht om te stoppen met het verzenden van telemetriegegevens |
| ChangeSetPointTemp |Hiermee wordt de waarde van het instelpunt gewijzigd waaromheen de willekeurige gegevens worden gegenereerd |
| DiagnosticTelemetry |Hiermee wordt de apparaatsimulator geactiveerd voor het verzenden van een extra telemetriewaarde (externalTemp) |
| ChangeDeviceState |Hiermee wordt een eigenschap uitgebreide status voor het apparaat gewijzigd en wordt het bericht met apparaatgegevens verzonden vanaf het apparaat |

> [!NOTE]
> Zie [Cloud-to-device communications guidance][lnk-c2d-guidance] (Richtlijnen voor communicatie tussen cloud en apparaat) voor een vergelijking van deze opdrachten (berichten van cloud naar apparaat) en methoden (directe methoden).

## <a name="iot-hub"></a>IoT Hub

De [IoT Hub][lnk-iothub] neemt gegevens op die vanaf de apparaten worden verzonden naar de cloud, en maakt ze beschikbaar voor de ASA-taken (Azure Stream Analytics). Voor elke stream maakt de ASA-taak gebruik van een afzonderlijke IoT Hub-consumentengroep om de stroom berichten van de apparaten te lezen.

De IoT Hub in de oplossing doet ook het volgende:

- Houdt een register van identiteiten bij waarin de id's en verificatiesleutels zijn opgeslagen van alle apparaten die verbinding mogen maken met de portal. U kunt apparaten in- en uitschakelen via het register van identiteiten.
- Verstuurt namens de oplossingsportal opdrachten naar uw apparaten.
- Roept namens de oplossingsportal methoden aan op uw apparaten.
- Onderhoudt apparaatdubbels voor alle geregistreerde apparaten. Een apparaatdubbel slaat de eigenschapswaarden op die door een apparaat worden gerapporteerd. Een apparaatdubbel slaat ook gewenste eigenschappen op die in de oplossingsportal zijn ingesteld, zodat het apparaat deze kan ophalen wanneer opnieuw verbinding wordt maakt.
- Plant taken voor het instellen van eigenschappen voor meerdere apparaten of voor het aanroepen van methoden op meerdere apparaten.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

Bij de oplossing voor externe controle worden via [ASA][lnk-asa]-apparaatberichten (Azure Stream Analytics) die zijn ontvangen via de IoT Hub, verzonden naar andere back-endonderdelen voor verwerking of opslag. Met verschillende ASA-taken worden specifieke functies uitgevoerd gebaseerd op de inhoud van de berichten.

**Taak 1: apparaatgegevens** filtert berichten met apparaatgegevens uit de binnenkomende berichtenstroom en stuurt deze naar een Event Hub-eindpunt. Via een apparaat worden berichten met apparaatgegevens verzonden bij het opstarten en als reactie op een opdracht **SendDeviceInfo**. Tijdens deze taak wordt gebruikgemaakt van de volgende querydefinitie om berichten over **apparaatgegevens** te identificeren:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Tijdens deze taak wordt de uitvoer verzonden naar een Event Hub voor verdere verwerking.

**Taak 2: regels** evalueren de binnenkomende telemetriegegevens over temperatuur en vochtigheid aan de hand van de drempelwaarden voor elk apparaat. Drempelwaarden kunnen worden ingesteld met de regeleditor die beschikbaar is in de oplossingsportal. Elk koppel apparaat/waarde wordt per tijdstempel opgeslagen in een blob die met Stream Analytics wordt gelezen als **referentiegegevens**. De taak vergelijkt elke niet-lege waarde met de ingestelde drempelwaarde voor het apparaat. Als deze de voorwaarde > overschrijdt, bestaat de taakuitvoer uit een **waarschuwings**gebeurtenis die aangeeft dat de drempelwaarde is overschreden, en worden tevens de waarden voor het apparaat, de waarde en de tijdstempel gegeven. Voor deze taak wordt gebruikgemaakt van de volgende querydefinitie om berichten over telemetrie te identificeren die een alarm moeten activeren:

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

Tijdens deze taak wordt de uitvoer verzonden naar een Event Hub voor verdere verwerking. Ook worden details van elke waarschuwing opgeslagen in de blobopslag, waar de informatie uit de waarschuwing kan worden gelezen in de oplossingsportal.

**Taak 3: telemetrie** is op twee manieren actief op de binnenkomende stroom telemetriegegevens van het apparaat. Allereerst worden alle telemetrieberichten vanaf de apparaten naar een permanente blobopslag verzonden voor langdurige opslag. Ten tweede worden elke vijf minuten de gemiddelde, minimale en maximale vochtigheidswaarden berekend. Deze gegevens worden verzonden naar de blobopslag. Met de oplossingsportal worden de telemetriegegevens uit de blobopslag gelezen om de grafieken te vullen. Tijdens deze taak wordt gebruikgemaakt van de volgende querydefinitie:

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

## <a name="event-hubs"></a>Event Hubs

De ASA-taken voor **apparaatgegevens** en **regels** voeren hun gegevens uit naar Event Hubs die zo kunnen worden doorgestuurd naar de **gebeurtenisprocessor** die actief is in de webtaak.

## <a name="azure-storage"></a>Azure-opslag

De oplossing maakt gebruik van Azure Blob-opslag om alle ruwe en samengevatte telemetriegegevens op de apparaten in de oplossing te behouden. Met de portal worden de telemetriegegevens uit de blobopslag gelezen om de grafieken te vullen. Voor het weergeven van waarschuwingen leest de oplossingsportal de gegevens uit de blobopslag, waarin is vastgelegd wanneer telemetriewaarden de geconfigureerde drempelwaarden hebben overschreden. In de oplossing wordt blobopslag ook gebruikt om de drempelwaarden vast te leggen die u in de oplossingsportal hebt ingesteld.

## <a name="webjobs"></a>Webtaken

De webtaken in de oplossing hosten niet alleen de apparaatsimulatoren maar ook de **gebeurtenisprocessor** die wordt uitgevoerd in een Azure-webtaak die opdrachtantwoorden verwerkt. Met behulp van berichten met reacties op opdrachten wordt de opdrachtgeschiedenis van het apparaat bijgewerkt (opgeslagen in de Cosmos DB-database).

## <a name="cosmos-db"></a>Cosmos DB

De oplossing maakt gebruik van een Cosmos DB-database om informatie op te slaan over de apparaten die zijn verbonden met de oplossing. Deze informatie omvat de geschiedenis van opdrachten die vanuit de oplossingsportal naar apparaten zijn verzonden en van methoden die vanuit de oplossingsportal zijn aangeroepen.

## <a name="solution-portal"></a>Oplossingsportal

De oplossingsportal is een webtoepassing die als onderdeel van de vooraf geconfigureerde oplossing wordt geïmplementeerd. De belangrijkste pagina's in de oplossingsportal zijn het dashboard en de lijst met apparaten.

### <a name="dashboard"></a>Dashboard

Deze pagina in de webtoepassing maakt gebruik van Power BI javascript-besturingselementen (zie [PowerBI-visuals repo](https://www.github.com/Microsoft/PowerBI-visuals)) om de telemetriegegevens van de apparaten te visualiseren. De oplossing maakt gebruik van de ASA-telemetrietaak om de telemetriegegevens naar de blobopslag te schrijven.

### <a name="device-list"></a>Lijst met apparaten

Op deze pagina van de oplossingsportal kunt u het volgende doen:

* Nieuwe apparaten inrichten. Met deze actie wordt het unieke apparaat-id ingesteld en de verificatiesleutel gegenereerd. Er wordt informatie over het apparaat geschreven naar het IoT Hub-identiteitsregister en de oplossingsspecifieke Cosmos DB-database.
* Apparaateigenschappen beheren. Deze actie omvat het weergeven van bestaande eigenschappen en het bijwerken met nieuwe eigenschappen.
* Opdrachten naar een apparaat verzenden.
* De opdrachtgeschiedenis voor een apparaat weergeven.
* Apparaten in- en uitschakelen.

## <a name="next-steps"></a>Volgende stappen

De volgende TechNet-blogberichten bieden meer details over de vooraf geconfigureerde oplossing voor externe controle:

* [IoT Suite - Under The Hood - Remote Monitoring (IoT Suite - achter de schermen - externe controle)](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
* [IoT Suite - Remote Monitoring - Adding Live and Simulated Devices (IoT Suite - externe controle - live- en gesimuleerde apparaten toevoegen)](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

U kunt verder aan de slag gaan met IoT Suite door de volgende artikelen te lezen:

* [Uw apparaat koppelen aan de vooraf geconfigureerde oplossing voor externe bewaking][lnk-connect-rm]
* [Machtigingen op de site azureiotsuite.com][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twins]:  ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
