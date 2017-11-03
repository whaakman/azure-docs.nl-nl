---
title: De status van uw Azure IoT Hub | Microsoft Docs
description: Monitor voor Azure en Azure resourcestatus gebruiken om te bewaken van uw IoT-Hub en snel problemen onderzoeken
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/09/2017
ms.author: kgremban
ms.openlocfilehash: 3051af03d0c1433db98bcc674a072188e7ce80e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>De status van Azure IoT Hub bewaken en problemen snel onderzoeken

Bedrijven die Azure IoT Hub implementeren verwachten betrouwbare prestaties van hun bronnen. Als u een sluiten controle op uw activiteiten onderhouden, IoT-Hub volledig geïntegreerd met [Azure Monitor] [ lnk-AM] en [Azure resourcestatus] [ lnk-ARH]. Deze twee services werken samen om te voorzien van de gegevens die u moet ervoor zorgen dat uw IoT-oplossingen van en uitgevoerd in een foutloze toestand bevindt. 

Monitor voor Azure is één bron van controle en logboekregistratie voor alle Azure-services. U kunt de logboeken die Azure Monitor genereert OMS Log Analytics, Event Hubs of Azure Storage verzenden voor aangepaste verwerking. Azure Monitor metrische gegevens en diagnostische instellingen bieden u realtime zicht krijgt op de prestaties van uw resources. Doorgaan met het lezen van dit artikel voor meer informatie over hoe [gebruik Azure Monitor](#use-azure-monitor) met uw IoT-hub. 

Azure Resource Health helpt u bij het diagnosticeren en ondersteuning krijgen wanneer een Azure problemen heeft gevolgen voor uw resources. Huidige en eerdere status van biedt een aangepaste dashboard voor uw IoT-Hubs. Doorgaan met het lezen van dit artikel voor meer informatie over hoe [gebruik Azure resourcestatus](#use-azure-resource-health) met uw IoT-hub. 

Naast het integreren van deze twee services biedt IoT Hub ook een eigen metrische gegevens die u gebruiken kunt om te begrijpen van de status van uw IoT-resources. Zie voor meer informatie, [begrijpen IoT Hub metrische gegevens][lnk-metrics].

## <a name="use-azure-monitor"></a>Azure Monitor gebruiken

Azure biedt een niveau van de resource-diagnostische gegevens, wat betekent dat u de bewerkingen die in uw IoT-hub plaatsvinden kunt bewaken. 

Azure Monitor diagnostische instellingen vervangt de IoT Hub-bewerkingen controleren. Als u momenteel gebruiker operations bewaking, moet u uw werkstromen migreren. Zie voor meer informatie [migreren van controle-instellingen voor diagnostische gegevens bewerkingen][lnk-migrate].

Zie voor meer informatie over de specifieke metrische gegevens en gebeurtenissen die Azure-Monitor bewaakt, [ondersteund met een Azure-Monitor] [ lnk-AM-metrics] en [services, schema's en categorieën ondersteund voor Azure Diagnostische logboeken][lnk-AM-schemas].

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Inzicht in de logboeken

Azure Monitor houdt verschillende bewerkingen die in IoT-Hub optreden. Elke categorie bevat een schema dat bepaalt hoe gebeurtenissen in die categorie worden gemeld. 

#### <a name="connections"></a>Verbindingen

De categorie verbindingen houdt fouten die optreden wanneer apparaten verbinding maken met of Verbreek de verbinding een IoT-hub tussen. Het bijhouden van deze categorie is handig voor het identificeren van niet-geautoriseerde verbindingspogingen en voor het bijhouden van wanneer een verbinding verbroken voor apparaten in de gebieden van slechte connectiviteit wordt.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Information",
    "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"maskedIpAddress\":\"<maskedIpAddress>\"}", 
    "location": "Resource location"
}
```

#### <a name="cloud-to-device-commands"></a>Cloud-naar-apparaatopdrachten

De categorie cloud-naar-apparaatopdrachten houdt fouten die optreden bij de IoT-hub en gerelateerd zijn aan de pijplijn cloud-naar-apparaat-bericht. Deze categorie bevat fouten die optreden bij het verzenden van berichten van de cloud-naar-apparaat (zoals niet-geautoriseerde afzender), het ontvangen van berichten van de cloud-naar-apparaat (zoals levering aantal overschreden) en het cloud-naar-apparaat bericht feedback ontvangen (zoals feedback verlopen). Deze categorie onderschept fouten van een apparaat dat een bericht cloud-naar-apparaat niet goed verwerkt als de cloud-naar-apparaat-bericht is bezorgd niet.

```json
{
    "time": " UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "messageExpired",
    "category": "C2DCommands",
    "level": "Error",
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "properties": "{\"deviceId\":\"<deviceId>\",\"messageId\":\"<messageId>\",\"messageSizeInBytes\":\"<messageSize>\",\"protocol\":\"Amqp\",\"deliveryAcknowledgement\":\"<None, NegativeOnly, PositiveOnly, Full>\",\"deliveryCount\":\"0\",\"expiryTime\":\"<timestamp>\",\"timeInSystem\":\"<timeInSystem>\",\"ttl\":<ttl>, \"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\", \"maskedIpAddresss\": \"<maskedIpAddress>\", \"statusCode\": \"4XX\"}",
    "location": "Resource location"
}
```

#### <a name="device-identity-operations"></a>Bewerkingen voor apparaat-id

De apparaatcategorie identity-bewerkingen bijgehouden fouten die zich voordoen wanneer u probeert te maken, bijwerken of verwijderen van een vermelding in het identiteitenregister van uw IoT-hub. Het bijhouden van deze categorie is nuttig voor het inrichten van scenario's.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "get",
    "category": "DeviceIdentityOperations",
    "level": "Error",    
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "properties": "{\"maskedIpAddress\":\"<maskedIpAddress>\",\"deviceId\":\"<deviceId>\", \"statusCode\":\"4XX\"}",
    "location": "Resource location"
}
```

#### <a name="routes"></a>Routes

De routering categorie bericht houdt fouten die optreden tijdens bericht route evaluatie- en eindpunt status waargenomen door de IoT Hub. Deze categorie omvat gebeurtenissen zoals wanneer een regel resulteert in 'niet-gedefinieerde', wanneer IoT Hub markeert een eindpunt als onbestelbare en eventuele andere fouten die zijn ontvangen van een eindpunt. Deze categorie omvat geen specifieke fouten over de berichten zelf (zoals apparaat beperking fouten), die worden gemeld onder de categorie 'apparaattelemetrie '.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "endpointUnhealthy",
    "category": "Routes",
    "level": "Error",
    "properties": "{\"deviceId\": \"<deviceId>\",\"endpointName\":\"<endpointName>\",\"messageId\":<messageId>,\"details\":\"<errorDetails>\",\"routeName\": \"<routeName>\"}",
    "location": "Resource location"
}
```

#### <a name="device-telemetry"></a>De apparaattelemetrie

De telemetrie apparaatcategorie houdt fouten die optreden bij de IoT-hub en gerelateerd zijn aan de pijplijn telemetrie. Deze categorie bevat fouten die optreden bij het verzenden van telemetriegebeurtenissen (zoals beperking) en telemetrische gebeurtenissen (zoals onbevoegde lezer) ontvangen. Deze categorie kan geen catch-fouten worden veroorzaakt door de code die wordt uitgevoerd op het apparaat zelf.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "ingress",
    "category": "DeviceTelemetry",
    "level": "Error",
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "properties": "{\"deviceId\":\"<deviceId>\",\"batching\":\"0\",\"messageSizeInBytes\":\"<messageSizeInBytes>\",\"EventProcessedUtcTime\":\"<UTC timestamp>\",\"EventEnqueuedUtcTime\":\"<UTC timestamp>\",\"partitionId\":\"1\"}", 
    "location": "Resource location"
}
```

#### <a name="file-upload-operations"></a>Bestandsuploadbewerkingen

Het bestand uploaden categorie houdt fouten die optreden bij de IoT-hub en gerelateerd zijn aan de functionaliteit van het uploaden van bestanden. Deze categorie omvat:

* Fouten die bij de SAS-URI optreden, zoals wanneer het voordat een apparaat een melding van de hub van het uploaden van een voltooide verloopt.
* Kan geen uploads gemeld door het apparaat.
* Fouten die optreden wanneer een bestand niet in de opslag tijdens het maken van message notification IoT Hub gevonden is.

Deze categorie kan geen catch-fouten die rechtstreeks zich tijdens het uploaden van het apparaat is een bestand naar de opslag.

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "ingress",
    "category": "FileUploadOperations",
    "level": "Error",
    "resultType": "Event status",
    "resultDescription": "MessageDescription",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\",\"blobUri\":\"http//bloburi.com\"}",
    "location": "Resource location"
}
```

#### <a name="cloud-to-device-twin-operations"></a>Cloud-naar-apparaat twin bewerkingen

De categorie van cloud-naar-apparaat twin bewerkingen houdt gebeurtenissen service is gestart op horende apparaten. Deze bewerkingen kunnen get twin opnemen, de eigenschappen van de gerapporteerde bijwerken en zich abonneren op de gewenste eigenschappen

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "read",
    "category": "C2DTwinOperations",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\"}", 
    "location": "Resource location"
}
```

#### <a name="device-to-cloud-twin-operations"></a>Apparaat-naar-cloud-twin bewerkingen

De apparaat-naar-cloud-twin operations categorie houdt apparaat geïnitieerde gebeurtenissen op horende apparaten. Deze bewerkingen kunnen opnemen get twin, bijwerken of vervangen labels, en bijwerken of vervangen gewenste eigenschappen. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "update",
    "category": "D2CTwinOperations",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\",\"protocol\":\"<protocol>\",\"authenticationType\":\"{\\\"scope\\\":\\\"device\\\",\\\"type\\\":\\\"sas\\\",\\\"issuer\\\":\\\"iothub\\\",\\\"acceptingIpFilterRule\\\":null}\"}", 
    "location": "Resource location"
}
```

#### <a name="twin-queries"></a>Dubbele query 's

De categorie van de query's twin rapporten over queryaanvragen voor apparaat horende die geïnitieerd zijn in de cloud. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "query",
    "category": "TwinQueries",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"query\":\"<twin query>\",\"sdkVersion\":\"<sdkVersion>\",\"messageSize\":\"<messageSize>\",\"pageSize\":\"<pageSize>\", \"continuation\":\"<true, false>\", \"resultSize\":\"<resultSize>\"}", 
    "location": "Resource location"
}
```

#### <a name="jobs-operations"></a>Taakbewerkingen

De categorie taken bewerkingen rapporten over taakaanvragen apparaat horende bijwerken of directe methoden op meerdere apparaten. Deze aanvragen worden gestart in de cloud. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "jobCompleted",
    "category": "JobsOperations",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"jobId\":\"<jobId>\", \"sdkVersion\": \"<sdkVersion>\",\"messageSize\": <messageSize>,\"filter\":\"DeviceId IN ['1414ded9-b445-414d-89b9-e48e8c6285d5']\",\"startTimeUtc\":\"Wednesday, September 13, 2017\",\"duration\":\"0\"}", 
    "location": "Resource location"
}
```

#### <a name="direct-methods"></a>Rechtstreekse methoden

De categorie direct methoden houdt interacties van aanvraag-antwoord verzonden naar de afzonderlijke apparaten. Deze aanvragen worden gestart in de cloud. 

```json
{
    "time": "UTC timestamp",
    "resourceId": "Resource Id",
    "operationName": "send",
    "category": "DirectMethods",
    "level": "Information",
    "durationMs": "1",
    "properties": "{\"deviceId\":\"<deviceId>\", \"RequestSize\": 1, \"ResponseSize\": 1, \"sdkVersion\": \"2017-07-11\"}", 
    "location": "Resource location"
}
```

### <a name="read-logs-from-azure-event-hubs"></a>Alleen logboeken van Azure Event Hubs

Nadat u via de diagnostische instellingen voor logboekregistratie van gebeurtenissen hebt ingesteld, kunt u toepassingen die uit de logboeken, lezen zodat u op basis van de informatie in deze actie kunt ondernemen. Deze voorbeeldcode haalt de logboeken van een event hub:

```
class Program 
{ 
    static string connectionString = "{your AMS eventhub endpoint connection string}"; 
    static string monitoringEndpointName = "{your AMS event hub endpoint name}"; 
    static EventHubClient eventHubClient; 
//This is the Diagnostic Settings schema 
    class AzureMonitorDiagnosticLog 
    { 
        string time { get; set; } 
        string resourceId { get; set; } 
        string operationName { get; set; } 
        string category { get; set; } 
        string level { get; set; } 
        string resultType { get; set; } 
        string resultDescription { get; set; } 
        string durationMs { get; set; } 
        string callerIpAddress { get; set; } 
        string correlationId { get; set; } 
        string identity { get; set; } 
        string location { get; set; } 
        Dictionary<string, string> properties { get; set; } 
    }; 
    static void Main(string[] args) 
    { 
        Console.WriteLine("Monitoring. Press Enter key to exit.\n"); 
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName); 
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds; 
        CancellationTokenSource cts = new CancellationTokenSource(); 
        var tasks = new List<Task>(); 
        foreach (string partition in d2cPartitions) 
        { 
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token)); 
        } 
        Console.ReadLine(); 
        Console.WriteLine("Exiting..."); 
        cts.Cancel(); 
        Task.WaitAll(tasks.ToArray()); 
    } 
    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct) 
    { 
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow); 
        while (true) 
        { 
            if (ct.IsCancellationRequested) 
            { 
                await eventHubReceiver.CloseAsync(); 
                break; 
            } 
            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10)); 
            if (eventData != null) 
            { 
                string data = Encoding.UTF8.GetString(eventData.GetBytes()); 
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data); 
                var deserializer = new JavaScriptSerializer(); 
                //deserialize json data to azure monitor object 
                AzureMonitorDiagnosticLog message = new JavaScriptSerializer().Deserialize<AzureMonitorDiagnosticLog>(result); 
 
            } 
        } 
    } 
} 
```

## <a name="use-azure-resource-health"></a>Gebruik Azure resourcestatus

Resourcestatus Azure gebruiken om te controleren of uw IoT-hub actief is. U kunt ook meer te weten of een regionale uitval is die invloed hebben op de status van uw IoT-hub. Om specifieke details over de status van uw Azure-IoT-Hub te begrijpen, raden we u [gebruik Azure Monitor](#use-azure-monitor). 

Azure IoT-Hub geeft health een regionaal niveau. Als er een regionale uitval die invloed hebben op uw IoT-hub, de status wordt weergegeven als **onbekende**. Zie voor meer informatie over de specifieke controles die de resourcestatus Azure uitvoert, [resourcetypen en health controleert in Azure resourcestatus][lnk-ARH-checks].

U kunt de status van uw IoT-hubs controleren door de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar **servicestatus** > **resourcestatus**.
1. Selecteer uw abonnement in de vervolgkeuzelijsten en **IoT Hub**.

Zie voor meer informatie over het interpreteren van de van gezondheidsgegevens, [health overzicht van Azure-resource][lnk-ARH]

## <a name="next-steps"></a>Volgende stappen

- [Inzicht in IoT Hub metrische gegevens][lnk-metrics]
- [Externe controle IoT en meldingen met Azure Logic Apps die gebruikmaken van uw IoT-hub en Postvak][lnk-monitoring-notifications]


[lnk-AM]: ../monitoring-and-diagnostics/index.yml
[lnk-ARH]: ../service-health/resource-health-overview.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-migrate]: iot-hub-migrate-to-diagnostics-settings.md
[lnk-AM-metrics]: ../monitoring-and-diagnostics/monitoring-supported-metrics.md
[lnk-AM-schemas]: ../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md
[lnk-ARH-checks]: ../service-health/resource-health-checks-resource-types.md
[lnk-monitoring-notifications]: iot-hub-monitoring-notifications-with-azure-logic-apps.md