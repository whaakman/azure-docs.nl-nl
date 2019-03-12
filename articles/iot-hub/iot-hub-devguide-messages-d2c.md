---
title: Informatie over Azure IoT Hub-berichtroutering | Microsoft Docs
description: Ontwikkelaars begeleiden - berichtroutering gebruiken voor het verzenden van apparaat-naar-cloud-berichten. Bevat informatie over het verzenden van Telemetrie- en niet-telemetriegegevens.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: dc5bfe6b431659b7b99140eb29a0e64922a42275
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57576331"
---
# <a name="use-iot-hub-message-routing-to-send-device-to-cloud-messages-to-different-endpoints"></a>Gebruik IoT Hub-berichtroutering apparaat-naar-cloud-berichten te verzenden naar verschillende eindpunten

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Berichtroutering, kunt u het verzenden van berichten van uw apparaten naar de cloud services op een geautomatiseerde, schaalbare en betrouwbare manier. Routering van berichten kan worden gebruikt voor: 

* **Verzenden van apparaat telemetrie-berichten, evenals gebeurtenissen** namelijk levenscyclusgebeurtenissen voor het apparaat en apparaatdubbel gebeurtenissen met de ingebouwde-eindpunt en aangepaste eindpunten wijzigen. Meer informatie over [routering eindpunten](#routing-endpoints).

* **Filteren van gegevens voordat deze routering naar verschillende eindpunten** door toe te passen van uitgebreide query's. Berichtroutering, kunt u een query uitvoeren op de eigenschappen van berichten en de berichttekst, evenals de device twin tags en de apparaatdubbeleigenschappen. Meer informatie over het gebruik van [query's in berichtroutering](iot-hub-devguide-routing-query-syntax.md).

IoT Hub moet schrijftoegang tot deze service-eindpunten voor de routering van berichten om te werken. Als u uw eindpunten via Azure portal configureert, worden de benodigde machtigingen voor u toegevoegd. Zorg ervoor dat u uw services ter ondersteuning van de verwachte doorvoer configureren. Wanneer u uw IoT-oplossing voor het eerst configureert, moet u mogelijk uw extra eindpunten bewaken en breng de gewenste wijzigingen voor de werkelijke belasting.

De IoT-Hub definieert een [algemene indeling](iot-hub-devguide-messages-construct.md) voor alle apparaat-naar-cloud-berichten de interoperabiliteit voor alle protocollen. Als een bericht overeenkomt met meerdere routes die naar hetzelfde eindpunt verwijzen, biedt IoT Hub bericht slechts één keer naar dit eindpunt. Daarom moet u niet de Ontdubbeling configureren op uw Service Bus-wachtrij of onderwerp. In gepartitioneerde wachtrijen garandeert partitie affiniteit berichtvolgorde. Gebruik deze zelfstudie voor meer informatie over het [configureren berichtroutering](tutorial-routing.md).

## <a name="routing-endpoints"></a>Routering eindpunten

Een IoT-hub is een ingebouwde-in-eindpunt (**berichten/gebeurtenissen**) die compatibel is met Event Hubs. U kunt maken [aangepaste eindpunten](iot-hub-devguide-endpoints.md#custom-endpoints) te routeren van berichten naar andere services in uw abonnement koppelt aan de IoT-Hub. IoT Hub ondersteunt momenteel de volgende services als aangepaste eindpunten:

### <a name="built-in-endpoint"></a>Ingebouwd eindpunt

U kunt standard [Event Hubs-integratie en SDK's](iot-hub-devguide-messages-read-builtin.md) apparaat-naar-cloud om berichten te ontvangen van het geïntegreerde eindpunt (**berichten/gebeurtenissen**). Zodra een Route is gemaakt, stopt gegevens naar het ingebouwde-in-eindpunt wordt geleid, tenzij er een Route naar dit eindpunt is gemaakt.

### <a name="azure-blob-storage"></a>Azure Blob Storage

IoT Hub worden ondersteund voor het schrijven van gegevens naar Azure Blob-opslag in de [Apache Avro](https://avro.apache.org/) en JSON-indeling. De mogelijkheid voor het coderen van JSON-indeling is in preview in alle regio's die IOT Hub verkrijgbaar in, met uitzondering van VS-Oost, VS-West en West-Europa is. De standaardwaarde is AVRO. De coderingsindeling kan alleen worden ingesteld als het eindpunt van blob storage is geconfigureerd. De indeling kan niet worden bewerkt voor een bestaand eindpunt. Wanneer u de JSON-codering, moet u de contentType instellen in JSON en beperking contentEncoding naar UTF-8 in het bericht [Systeemeigenschappen](iot-hub-devguide-routing-query-syntax.md#system-properties). U kunt de coderingsindeling met behulp van de IoT Hub maken of bijwerken REST API, specifiek selecteren de [RoutingStorageContainerProperties](https://docs.microsoft.com/rest/api/iothub/iothubresource/createorupdate#routingstoragecontainerproperties), de Azure-Portal [Azure CLI](https://docs.microsoft.com/cli/azure/iot/hub/routing-endpoint?view=azure-cli-latest) of de [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.iothub/add-aziothubroutingendpoint?view=azps-1.3.0). Het volgende diagram toont hoe u de coderingsindeling selecteert in de Azure Portal.

![BLOB storage endpoint codering](./media/iot-hub-devguide-messages-d2c/blobencoding.png)

IoT Hub berichten batches en schrijft gegevens naar een blob telkens wanneer een bepaalde grootte heeft bereikt van de batch of een bepaalde hoeveelheid tijd is verstreken. IoT Hub wordt standaard op de volgende naamconventie voor bestand:

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

U kunt een naamgevingsconventie voor bestanden, maar u moet alle vermelde tokens gebruiken. IoT Hub worden geschreven naar een lege blob als er zijn geen gegevens om te schrijven.

Wanneer Routering BLOB storage, raden wij de blobs opnemen en vervolgens iteratie van deze, om ervoor te zorgen dat voor alle containers worden gelezen zonder een veronderstellingen van de partitie. Het partitiebereik kan mogelijk wijzigen tijdens een [Microsoft geïnitieerde failover](iot-hub-ha-dr.md#microsoft-initiated-failover) of IoT-Hub [handmatige failover](iot-hub-ha-dr.md#manual-failover-preview). U kunt de [lijst met Blobs API](https://docs.microsoft.com/rest/api/storageservices/list-blobs) opsommen van de lijst met blobs. Raadpleeg het volgende voorbeeld als richtlijn.

   ```csharp
        public void ListBlobsInContainer(string containerName, string iothub)
        {
            var storageAccount = CloudStorageAccount.Parse(this.blobConnectionString);
            var cloudBlobContainer = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
            if (cloudBlobContainer.Exists())
            {
                var results = cloudBlobContainer.ListBlobs(prefix: $"{iothub}/");
                foreach (IListBlobItem item in results)
                {
                    Console.WriteLine(item.Uri);
                }
            }
        }
   ```

### <a name="service-bus-queues-and-service-bus-topics"></a>Service Bus-wachtrijen en Service Bus-onderwerpen

Service Bus-wachtrijen en onderwerpen die worden gebruikt als IoT Hub-eindpunten mag geen **sessies** of **detectie van dubbele** ingeschakeld. Als een van deze opties zijn ingeschakeld, het eindpunt wordt weergegeven als **onbereikbaar** in Azure portal.

### <a name="event-hubs"></a>Event Hubs

Naast de ingebouwde-Event Hubs compatibele eindpunt, kunt u ook gegevens doorsturen naar aangepaste eindpunten van het type Event Hubs. 

## <a name="reading-data-that-has-been-routed"></a>Lezen van gegevens dat wordt gerouteerd

U kunt een route configureren met dit [zelfstudie](tutorial-routing.md).

Gebruik de volgende zelfstudies voor informatie over het lezen van bericht van een eindpunt.

* Lezen van [ingebouwde-eindpunt](quickstart-send-telemetry-node.md)

* Lezen van [Blob storage](../storage/blobs/storage-blob-event-quickstart.md)

* Lezen van [Eventhubs](../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

* Lezen van [Service Bus-wachtrijen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)

* Lezen van [Service Bus-onderwerpen](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)

## <a name="fallback-route"></a>Alternatieve route

De alternatieve route verzendt alle berichten die niet voldoen aan de queryvoorwaarden op een van de bestaande routes naar de ingebouwde-Event Hubs (**berichten/gebeurtenissen**), dat wil zeggen compatibel is met [Event Hubs](/azure/event-hubs/). Als de routering van berichten is ingeschakeld, kunt u de mogelijkheid alternatieve route inschakelen. Zodra een route is gemaakt, gegevens stopt naar het ingebouwde-in-eindpunt geleid, tenzij er een route naar dit eindpunt wordt gemaakt. Als er geen routes met het ingebouwde-in-eindpunt zijn en een alternatieve route is ingeschakeld, wordt alleen de berichten die niet overeenkomen met de queryvoorwaarden van een op routes met het ingebouwde-in-eindpunt worden verzonden. Als alle bestaande routes worden verwijderd, moet er ook alternatieve route worden ingeschakeld voor het ontvangen van alle gegevens op het ingebouwde-in-eindpunt. 

U kunt in-of uitschakelen de alternatieve route in Azure Portal-blade berichtroutering >. U kunt ook gebruiken met Azure Resource Manager voor [FallbackRouteProperties](/rest/api/iothub/iothubresource/createorupdate#fallbackrouteproperties) met een aangepast eindpunt voor alternatieve route.

## <a name="non-telemetry-events"></a>Niet-telemetriegebeurtenissen

Naast de telemetrie van apparaten kan berichtroutering ook verzenden device twin gebeurtenissen en levenscyclusgebeurtenissen voor het apparaat. Bijvoorbeeld, als een route is gemaakt met de gegevensbron is ingesteld op **device twin gebeurtenissen**, IoT Hub-berichten verzendt naar het eindpunt dat de wijziging in de apparaatdubbel bevatten. Op dezelfde manier als een route is gemaakt met de gegevensbron is ingesteld op **levenscyclusgebeurtenissen voor het apparaat**, IoT-Hub ontvangt een bericht dat aangeeft of het apparaat is verwijderd of is gemaakt. 

[IoT Hub kan ook worden geïntegreerd met Azure Event Grid](iot-hub-event-grid.md) apparaatgebeurtenissen ter ondersteuning van realtime-integraties en automatisering van werkstromen op basis van deze gebeurtenissen publiceren. Sleutel [verschillen tussen de routering van berichten en Event Grid](iot-hub-event-grid-routing-comparison.md) voor meer informatie over die geschikt is voor uw scenario.

## <a name="testing-routes"></a>Routes testen

Wanneer u een nieuwe route maken of bewerken van een bestaande route, moet u de route-query met een voorbeeldbericht testen. U kunt afzonderlijke routes testen of alle routes in één keer te testen en geen berichten worden doorgestuurd naar de eindpunten tijdens de test. Azure Portal, Azure Resource Manager, Azure PowerShell en Azure CLI kunnen worden gebruikt voor het testen. Resultaten vast te stellen of de voorbeeldbericht overeenkomen met de query, bericht komt niet overeen met de query of test kan niet worden uitgevoerd omdat de voorbeeld-bericht of query-syntaxis onjuist zijn. Zie voor meer informatie, [Route testen](/rest/api/iothub/iothubresource/testroute) en [Test alle routes](/rest/api/iothub/iothubresource/testallroutes).

## <a name="latency"></a>Latentie

Wanneer u berichten over telemetrie voor apparaat-naar-cloud met behulp van ingebouwde eindpunten versturen, moet u er een lichte toename van de end-to-end-latentie is na het maken van de eerste route.

In de meeste gevallen is de gemiddelde toename in latentie van minder dan 500 ms. U kunt controleren de latentie **routering: bericht latentie voor berichten/gebeurtenissen** of **d2c.endpoints.latency.builtIn.events** metrische gegevens van IoT-Hub. Maken of verwijderen van een route na de eerste map heeft geen invloed op de end-to-end-latentie.

## <a name="monitoring-and-troubleshooting"></a>Bewaking en probleemoplossing

IoT Hub biedt verschillende Routering en eindpunt verwante metrische gegevens waarin u een overzicht van de status van uw hub en de berichten die worden verzonden. U kunt gegevens uit meerdere metrische gegevens om de hoofdoorzaak van problemen vast te combineren. Gebruik bijvoorbeeld de statistiek voor **routering: telemetrieberichten verwijderd** of **d2c.telemetry.egress.dropped** voor het identificeren van het aantal berichten dat is verwijderd als ze niet overeenkomen met de query's op een van de routes en alternatieve route is uitgeschakeld. [Metrische gegevens van IoT-Hub](iot-hub-metrics.md) geeft een lijst van alle metrische gegevens die zijn standaard ingeschakeld voor uw IoT-Hub.

U kunt de REST-API gebruiken [Endpoint-status ophalen](https://docs.microsoft.com/de-de/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) om op te halen [de integriteitsstatus van de](iot-hub-devguide-endpoints.md#custom-endpoints) van de eindpunten. Wordt u aangeraden de [metrische gegevens van IoT-Hub](iot-hub-metrics.md) met betrekking tot routering bericht latentie om te bepalen en fouten opsporen, wanneer de status van het eindpunt dode of niet in orde is. Bijvoorbeeld, voor het eindpunt van het type Event Hubs, kunt u controleren **d2c.endpoints.latency.eventHubs**. De status van een eindpunt niet in orde wordt bijgewerkt op in orde wanneer IoT Hub heeft een uiteindelijke consistente status van de gezondheid van tot stand worden gebracht.

Met behulp van de **routes** diagnostische logboeken in Azure Monitor [diagnostische instellingen](../iot-hub/iot-hub-monitor-resource-health.md), kunt u de nummers fouten die tijdens de evaluatie van een routering query- en eindpunt status optreden zoals waargenomen door de IoT-Hub, bijvoorbeeld een eindpunt is als inactief. Deze logboeken met diagnostische gegevens kunnen worden verzonden naar Azure Monitor-Logboeken, Event Hubs of Azure Storage voor aangepaste verwerking.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het maken van berichtroutes, [Process IoT Hub apparaat-naar-cloud-berichten met behulp van routes](tutorial-routing.md).

* [Over het verzenden van apparaat-naar-cloud-berichten](quickstart-send-telemetry-node.md)

* Zie voor meer informatie over de SDK's die u kunt gebruiken voor het verzenden van apparaat-naar-cloud-berichten [Azure IoT SDK's](iot-hub-devguide-sdks.md).
