---
title: Controleer de status van uw Azure IoT Hub | Microsoft Docs
description: Azure Monitor en Azure Resource Health gebruiken om te controleren van uw IoT-Hub en snel problemen vaststellen
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/09/2018
ms.author: kgremban
ms.openlocfilehash: c7d8631a8e7b99cca8f84ea1011c361db01e399e
ms.sourcegitcommit: 0fcd6e1d03e1df505cf6cb9e6069dc674e1de0be
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42059431"
---
# <a name="monitor-the-health-of-azure-iot-hub-and-diagnose-problems-quickly"></a>Controleer de status van Azure IoT Hub en snel problemen vaststellen

Bedrijven die Azure IoT Hub implementeren verwachten betrouwbare prestaties van hun resources. Voor hulp bij het onderhouden van een sluiten controle over uw bewerkingen, IoT-Hub volledig is geïntegreerd met [Azure Monitor] [ lnk-AM] en [Azure Resource Health] [ lnk-ARH]. Deze twee services werken samen om te voorzien van de gegevens die u nodig hebt om uw IoT-oplossingen en die worden uitgevoerd in een status in orde te houden. 

Azure Monitor is één bron van controle en registratie voor uw Azure-services. U kunt de diagnostische logboeken die Azure Monitor genereert verzenden naar Log Analytics, Event Hubs of Azure Storage voor de verwerking van aangepaste. Instellingen voor het metrische en diagnostische gegevens van Azure Monitor kunnen u inzicht in de prestaties van uw resources. Doorgaan met het lezen van dit artikel voor meer informatie over hoe u [gebruikt Azure Monitor](#use-azure-monitor) met uw IoT-hub. 

> [!IMPORTANT]
> De gebeurtenissen die zijn gegenereerd door de IoT Hub-service met behulp van Azure Monitor logboeken met diagnostische gegevens zijn niet gegarandeerd betrouwbaar of geordende. Sommige gebeurtenissen kunnen verloren of die niet de juiste volgorde worden geleverd. Logboeken met diagnostische gegevens ook zijn niet bedoeld om te worden realtime en het duurt enkele minuten voor gebeurtenissen naar uw keuze van de bestemming wordt geschreven.

Azure Resource Health helpt u bij het diagnosticeren en ondersteuning krijgen wanneer een problemen met Azure invloed is op uw resources. Huidige en eerdere status van biedt een gepersonaliseerd dashboard voor uw IoT-Hubs. Doorgaan met het lezen van dit artikel voor meer informatie over hoe u [gebruik Azure Resource Health](#use-azure-resource-health) met uw IoT-hub. 

Naast integreren met deze twee services, biedt IoT Hub ook een eigen metrische gegevens die u gebruiken kunt om te begrijpen van de status van uw IoT-resources. Zie voor meer informatie, [metrische gegevens van IoT-Hub begrijpen][lnk-metrics].

## <a name="use-azure-monitor"></a>Azure Monitor gebruiken

Azure Monitor biedt resourceniveau-diagnostische gegevens, wat betekent dat bewerkingen die binnen uw IoT-hub plaatsvinden te bewaken. 

Azure Monitor diagnostische instellingen vervangt de IoT Hub-bewerkingen bewaken. Als u momenteel gebruikmaakt van bewerkingen controleren, moet u uw werkstromen migreren. Zie voor meer informatie, [migreren uit bewerkingen voor controle-instellingen voor diagnostische gegevens][lnk-migrate].

Zie voor meer informatie over de specifieke metrische gegevens en gebeurtenissen die Azure Monitor bewaakt, [ondersteunde metrische gegevens met Azure Monitor] [ lnk-AM-metrics] en [ondersteunde services, schema's en categorieën voor Azure Diagnostische logboeken][lnk-AM-schemas].

[!INCLUDE [iot-hub-diagnostics-settings](../../includes/iot-hub-diagnostics-settings.md)]

### <a name="understand-the-logs"></a>Logboeken begrijpen

Azure Monitor houdt bij of verschillende bewerkingen die zich in IoT Hub voordoen. Elke categorie bevat een schema dat definieert hoe gebeurtenissen in die categorie worden gerapporteerd. 

#### <a name="connections"></a>Verbindingen

De verbindingen categorie nummers apparaat verbinding maken en gebeurtenissen loskoppelen van een IoT-hub, evenals fouten. Het bijhouden van deze categorie is handig voor het identificeren van niet-geautoriseerde verbindingspogingen en voor het bijhouden van een verbinding is verbroken voor apparaten in de gebieden van slechte connectiviteit.

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

#### <a name="cloud-to-device-commands"></a>Cloud-naar-apparaat-opdrachten

De categorie cloud-naar-apparaatopdrachten houdt bij of fouten die optreden bij de IoT-hub en zijn gerelateerd aan de pijplijn cloud-naar-apparaat bericht. Deze categorie bevat fouten die optreden bij het verzenden van berichten van cloud-naar-apparaat (zoals niet-geautoriseerde afzender), het ontvangen van berichten van cloud-naar-apparaat (zoals aantal bezorgingen is overschreden) en het ontvangen van feedback van cloud-naar-apparaat bericht (zoals feedback verlopen). Deze categorie geen fouten van een apparaat dat een cloud-naar-apparaat-bericht niet goed verwerkt als de cloud-naar-apparaat-bericht met succes is afgeleverd af.

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

De apparaatcategorie die identiteit operations houdt bij of fouten die optreden wanneer u probeert te maken, bijwerken of verwijderen van een vermelding in het identiteitenregister van uw IoT-hub. Het bijhouden van deze categorie is handig voor het inrichten van scenario's.

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

De categorie voor het doorsturen van bericht houdt bij of fouten die optreden tijdens de evaluatie van bericht route en eindpunt status van de waargenomen door de IoT Hub. Deze categorie omvat gebeurtenissen zoals wanneer een regel resulteert in 'niet-gedefinieerde', wanneer IoT-Hub markeert een eindpunt als onbestelbaar en eventuele andere fouten die zijn ontvangen van een eindpunt. Deze categorie bevat geen specifieke fouten over berichten zelf (zoals apparaat beperkingsfouten), die worden gerapporteerd in de categorie 'apparaattelemetrie'.

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

#### <a name="device-telemetry"></a>Telemetrie van apparaten

De apparaatcategorie telemetrie houdt bij of fouten die optreden bij de IoT-hub en zijn gerelateerd aan de pijplijn telemetrie. Deze categorie bevat fouten die optreden bij het verzenden van telemetriegebeurtenissen (zoals beperking) en telemetriegebeurtenissen (zoals niet-geautoriseerde lezer) ontvangen. Deze categorie worden geen fouten veroorzaakt door de code die wordt uitgevoerd op het apparaat zelf gedetecteerd.

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

De categorie van bestand uploaden bijgehouden fouten die optreden bij de IoT-hub en zijn gerelateerd aan functionaliteit voor het uploaden van bestand. Deze categorie omvat:

* Fouten die met de SAS-URI optreden, zoals wanneer het verloopt voordat een apparaat aan de hub van het uploaden van een voltooide gecommuniceerd.
* Kan geen uploads gemeld door het apparaat.
* Fouten die optreden wanneer een bestand niet in de opslag tijdens het maken van IoT-Hub melding weergegeven gevonden is.

Deze categorie worden geen fouten die optreden wanneer het apparaat een bestand naar storage uploaden is gedetecteerd.

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

#### <a name="cloud-to-device-twin-operations"></a>Cloud-naar-apparaat dubbele bewerkingen

De categorie van cloud-naar-apparaat dubbele bewerkingen bijgehouden service geïnitieerde gebeurtenissen op dubbele apparaten. Deze bewerkingen kunnen bevatten dubbele ophalen, bijwerken of vervangen van tags, en bijwerken of vervangen gewenste eigenschappen. 

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

#### <a name="device-to-cloud-twin-operations"></a>Dubbele apparaat-naar-cloud-bewerkingen

De categorie van het dubbele apparaat-naar-cloud-bewerkingen bijgehouden apparaat geïnitieerde gebeurtenissen op dubbele apparaten. Deze bewerkingen kunnen get dubbele opnemen, gerapporteerde eigenschappen bij te werken en Abonneer u op de gewenste eigenschappen.

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

#### <a name="twin-queries"></a>Apparaatdubbel-query 's

De categorie van de query's dubbele rapporteert over queryaanvragen voor apparaatdubbels die geïnitieerd zijn in de cloud. 

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

De categorie van de bewerkingen taken rapporteert over taakaanvragen voor het bijwerken van apparaatdubbels of directe methoden aanroepen op meerdere apparaten. Deze aanvragen worden gestart in de cloud. 

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

#### <a name="direct-methods"></a>Directe methoden

De categorie directe methoden houdt request response-interacties verzonden naar afzonderlijke apparaten. Deze aanvragen worden gestart in de cloud. 

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

### <a name="read-logs-from-azure-event-hubs"></a>Lezen Logboeken uit Azure Event Hubs

Na het instellen van via diagnostische instellingen voor logboekregistratie van gebeurtenissen, kunt u toepassingen die de logboeken gelezen zodat u op basis van de informatie in deze actie kunt ondernemen. Deze voorbeeldcode haalt de logboeken van een event hub:

```csharp
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

## <a name="use-azure-resource-health"></a>Gebruik Azure Resource Health

Gebruik Azure Resource Health om te controleren of uw IoT-hub actief en werkend is. U kunt ook meer informatie of een regionale storing is die invloed hebben op de status van uw IoT-hub. Voor meer informatie over specifieke details over de status van uw Azure-IoT-Hub, raden wij aan dat u [gebruikt Azure Monitor](#use-azure-monitor). 

Azure IoT Hub geeft aan dat de status op het niveau van een regionale. Als er een regionale onderbreking die invloed hebben op uw IoT-hub, de status wordt weergegeven als **onbekende**. Zie voor meer informatie over de specifieke statuscontroles die Azure Resource Health uitvoert, [resourcetypen en statuscontroles in Azure resource health][lnk-ARH-checks].

Om te controleren of de status van uw IoT-hubs, de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar **servicestatus** > **resourcestatus**.
1. Selecteer uw abonnement in de vervolgkeuzelijsten en **IoT-Hub**.

Zie voor meer informatie over het interpreteren van de van gezondheidsgegevens, [overzicht van Azure resource health][lnk-ARH]

## <a name="next-steps"></a>Volgende stappen

- [Informatie over metrische gegevens van IoT Hub][lnk-metrics]
- [Externe controle IoT en meldingen met Azure Logic Apps die gebruikmaken van uw IoT-hub en Postvak][lnk-monitoring-notifications]


[lnk-AM]: ../monitoring-and-diagnostics/index.yml
[lnk-ARH]: ../service-health/resource-health-overview.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-migrate]: iot-hub-migrate-to-diagnostics-settings.md
[lnk-AM-metrics]: ../monitoring-and-diagnostics/monitoring-supported-metrics.md
[lnk-AM-schemas]: ../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md
[lnk-ARH-checks]: ../service-health/resource-health-checks-resource-types.md
[lnk-monitoring-notifications]: iot-hub-monitoring-notifications-with-azure-logic-apps.md
