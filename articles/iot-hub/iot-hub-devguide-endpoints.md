---
title: Azure IoT Hub-eindpunten begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - referentie-informatie over IoT Hub apparaat gerichte en gerichte service-eindpunten.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: bf23046b8a80b02bc1667f647cb1d475503a8feb
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125773"
---
# <a name="reference---iot-hub-endpoints"></a>Referentie - IoT-Hub-eindpunten

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="iot-hub-names"></a>Namen van de IoT Hub

U vindt de hostnaam van de IoT-hub die als host fungeert voor uw eindpunten in de portal van uw hub **overzicht** pagina. Standaard is de DNS-naam van een IoT-hub ziet eruit als: `{your iot hub name}.azure-devices.net`.

Azure DNS kunt u een aangepaste DNS-naam voor uw IoT-hub maken. Zie [Use Azure DNS to provide custom domain settings for an Azure service](../dns/dns-custom-domain.md) (Azure DNS gebruiken om aangepaste domeininstellingen te verstrekken voor een Azure-service) voor meer informatie.

## <a name="list-of-built-in-iot-hub-endpoints"></a>Lijst met ingebouwde IoT Hub-eindpunten

Azure IoT Hub is een multitenant-service waarmee wordt aangegeven dat de functionaliteit ervan naar verschillende actoren. Het volgende diagram toont de verschillende eindpunten die IoT-Hub toont.

![IoT Hub-eindpunten][img-endpoints]

De volgende lijst beschrijft de eindpunten:

* **Resourceprovider**. De resourceprovider van IoT-Hub toont een [Azure Resource Manager] [ lnk-arm] interface. Deze interface kunnen eigenaren van Azure-abonnement maken en verwijderen van IoT-hubs en bijwerken van eigenschappen van de IoT hub. Eigenschappen van de IoT Hub regelen [niveau van de hub beveiligingsbeleid][lnk-accesscontrol], in plaats van op apparaatniveau, toegangsbeheer en functionele opties voor cloud-naar-apparaat- en apparaat-naar-cloud-berichten. De resourceprovider van IoT Hub kunt u ook [exporteren van apparaat-id's][lnk-importexport].
* **Apparaat-identiteitsbeheer**. Elke IoT-hub toont een set REST voor HTTPS-eindpunten voor het beheren van apparaat-id's (maken, ophalen, bijwerken en verwijderen). [Apparaat-id's] [ lnk-device-identities] worden gebruikt voor verificatie en toegangsbeheer van apparaat.
* **Dubbele Apparaatbeheer**. Elke IoT-hub toont een set van gerichte service HTTPS REST-eindpunt voor de query- en [apparaatdubbels] [ lnk-twins] (update voor labels en eigenschappen).
* **Taken management**. Elke IoT-hub toont een set van gerichte service HTTPS REST-eindpunt voor query's uitvoeren en beheren van [taken][lnk-jobs].
* **Apparaat-eindpunten**. Voor elk apparaat in het id-register toont IoT-Hub een set met eindpunten:

  * *Apparaat-naar-cloud-berichten verzenden*. Een apparaat maakt gebruik van dit eindpunt naar [apparaat-naar-cloud-berichten verzenden][lnk-d2c].
  * *Cloud-naar-apparaat-berichten ontvangen*. Een apparaat maakt gebruik van dit eindpunt voor het ontvangen van gerichte [cloud-naar-apparaatberichten][lnk-c2d].
  * *Starten van het uploaden van bestanden*. Een apparaat maakt gebruik van dit eindpunt voor het ontvangen van een Azure Storage SAS-URI van IoT Hub kunt u [uploaden van een bestand][lnk-upload].
  * *Ophalen en bijwerken van apparaatdubbeleigenschappen*. Een apparaat dit eindpunt gebruikt voor toegang tot de [apparaatdubbel][lnk-twins]van eigenschappen.
  * *Ontvangen aanvragen van de directe methode*. Een apparaat maakt gebruik van dit eindpunt om te luisteren naar [directe methode][lnk-methods]van aanvragen.

    Deze eindpunten worden beschikbaar gesteld met [MQTT v3.1.1][lnk-mqtt], HTTPS 1.1, en [AMQP 1.0] [ lnk-amqp] protocollen. AMQP is ook beschikbaar via [WebSockets] [ lnk-websockets] op poort 443.

* **Service-eindpunten**. Elke IoT-hub toont een set met eindpunten voor uw back-end oplossing om te communiceren met uw apparaten. Met één uitzondering, deze eindpunten zijn alleen toegankelijk met behulp van de [AMQP] [ lnk-amqp] protocol. Het eindpunt van de methode aanroepen is toegankelijk via het HTTPS-protocol.
  
  * *Apparaat-naar-cloud-berichten ontvangen*. Dit eindpunt is compatibel met [Azure Event Hubs][lnk-event-hubs]. Een back-end-service kunt gebruiken om te lezen de [apparaat-naar-cloud-berichten] [ lnk-d2c] verzonden door uw apparaten. U kunt aangepaste eindpunten op uw IoT-hub naast deze ingebouwde eindpunt maken.
  * *Cloud-naar-apparaat-berichten verzenden en ontvangen van bevestigingen voor levering*. Deze eindpunten kunt u uw back-end oplossing voor het verzenden van betrouwbare [cloud-naar-apparaatberichten][lnk-c2d], en de bijbehorende levering of verlopen bevestigingen ontvangen.
  * *Bestand ontvangen*. Dit eindpunt berichten kunt u meldingen ontvangen wanneer uw apparaten is een bestand uploadt. 
  * *Directe aanroepen van de methode*. Dit eindpunt kan een back-end-service om aan te roepen een [directe methode] [ lnk-methods] op een apparaat.
  * *Bewerkingen controleren gebeurtenissen ontvangen*. Dit eindpunt kunt u voor het ontvangen van gebeurtenissen controleren als uw IoT-hub is geconfigureerd voor het verzenden van deze bewerkingen. Zie voor meer informatie, [IoT Hub-bewerkingen controleren][lnk-operations-mon].

De [Azure IoT SDK's] [ lnk-sdks] artikel beschrijft de verschillende manieren toegang hebben tot deze eindpunten.

Alle IoT Hub-eindpunten gebruiken de [TLS] [ lnk-tls] -protocol en er is geen eindpunt ooit wordt weergegeven op niet-versleutelde/niet-beveiligde kanalen.

## <a name="custom-endpoints"></a>Aangepaste eindpunten

U kunt bestaande Azure-services in uw abonnement koppelen aan uw IoT-hub om te fungeren als eindpunten voor het routeren van berichten. Deze eindpunten fungeren als een service-eindpunten en worden gebruikt als sink voor berichtroutes. Apparaten kunnen niet rechtstreeks naar de extra eindpunten worden geschreven. Zie voor meer informatie over berichtroutes, de vermelding van de handleiding voor ontwikkelaars op [verzenden en ontvangen van berichten met IoT hub][lnk-devguide-messaging].

IoT Hub ondersteunt momenteel de volgende Azure-services als extra eindpunten:

* Azure Storage-containers
* Event Hubs
* Service Bus-wachtrijen
* Service Bus-onderwerpen

IoT Hub moet schrijftoegang tot deze service-eindpunten voor de routering van berichten om te werken. Als u uw eindpunten via Azure portal configureert, worden de benodigde machtigingen voor u toegevoegd. Zorg ervoor dat u uw services ter ondersteuning van de verwachte doorvoer configureren. Wanneer u uw IoT-oplossing voor het eerst configureert, moet u mogelijk uw extra eindpunten bewaken en breng de gewenste wijzigingen voor de werkelijke belasting.

Als een bericht komt overeen met meerdere routes die allemaal verwijzen naar hetzelfde eindpunt, biedt IoT Hub bericht slechts één keer naar dit eindpunt. U hoeft daarom niet voor het configureren van Ontdubbeling op uw Service Bus-wachtrij of onderwerp. In gepartitioneerde wachtrijen garandeert partitie affiniteit berichtvolgorde.

Zie voor de grenzen van het aantal eindpunten die u kunt toevoegen, [quota en beperkingen][lnk-devguide-quotas].

### <a name="when-using-azure-storage-containers"></a>Bij het gebruik van Azure Storage-containers

IoT Hub worden alleen ondersteund bij het schrijven van gegevens naar Azure Storage-containers als blobs in de [Apache Avro](http://avro.apache.org/) indeling. IoT Hub berichten batches en schrijft gegevens naar een blob wanneer:

* De batch een bepaalde grootte heeft bereikt.
* Of een bepaalde hoeveelheid tijd is verstreken.

IoT Hub worden geschreven naar een lege blob als er zijn geen gegevens om te schrijven.

IoT Hub wordt standaard op de volgende naamconventie voor bestand:

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

U kunt ongeacht bestand naamgevingsconventie die u wilt, maar u moet alle vermelde tokens gebruiken.

### <a name="when-using-service-bus-queues-and-topics"></a>Wanneer u Service Bus-wachtrijen en onderwerpen

Service Bus-wachtrijen en onderwerpen die worden gebruikt als IoT Hub-eindpunten mag geen **sessies** of **detectie van dubbele** ingeschakeld. Als een van deze opties zijn ingeschakeld, het eindpunt wordt weergegeven als **onbereikbaar** in Azure portal.

## <a name="field-gateways"></a>Veldgateways

In een IoT-oplossing een *veldgateway* tussen uw apparaten en uw IoT Hub-eindpunten. Deze bevindt zich doorgaans dicht bij uw apparaten. Uw apparaten communiceren rechtstreeks met de veldgateway met behulp van een protocol dat wordt ondersteund door de apparaten. De veldgateway verbindt met een IoT Hub-eindpunt met behulp van een protocol dat wordt ondersteund door IoT Hub. Een veldgateway mogelijk op een specifiek apparaat of een stand-by-computer waarop aangepaste gateway-software wordt uitgevoerd.

U kunt [Azure IoT Edge] [ lnk-iot-edge] voor het implementeren van een veldgateway. IoT Edge biedt de functionaliteit, zoals multiplexing communicatie van meerdere apparaten op dezelfde IoT Hub-verbinding.

## <a name="next-steps"></a>Volgende stappen

Er zijn andere onderwerpen met naslaginformatie in deze IoT Hub developer guide:

* [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering][lnk-devguide-query]
* [Quota en beperkingen][lnk-devguide-quotas]
* [IoT Hub MQTT-ondersteuning][lnk-devguide-mqtt]

[lnk-iot-edge]: https://github.com/Azure/iot-edge

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[lnk-operations-mon]: iot-hub-operations-monitoring.md
