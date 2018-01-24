---
title: Azure IoT Hub eindpunten begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - naslaginformatie over IoT Hub apparaat gerichte en gerichte service-eindpunten.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 57ba52ae-19c6-43e4-bc6c-d8a5c2476e95
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: dobett
ms.openlocfilehash: dc983549aea53ed29859205102d6308a3367bec7
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="reference---iot-hub-endpoints"></a>Referentie - eindpunten van IoT-Hub

## <a name="iot-hub-names"></a>Namen van de IoT-Hub

U vindt de naam van de IoT-hub die als host fungeert voor uw eindpunten in de portal op de **overzicht** blade. Standaard is de DNS-naam van een IoT-hub ziet eruit als: `{your iot hub name}.azure-devices.net`.

Azure DNS kunt u een aangepaste DNS-naam voor uw iothub maken. Zie voor meer informatie [gebruik Azure DNS-instellingen van aangepast domein voor een Azure-service kan leveren](../dns/dns-custom-domain.md).

## <a name="list-of-built-in-iot-hub-endpoints"></a>Lijst met ingebouwde IoT Hub-eindpunten

Azure IoT Hub is een multitenant-service waarmee de functionaliteit voor verschillende actoren. Het volgende diagram toont de verschillende eindpunten dat IoT-Hub toont.

![IoT Hub-eindpunten][img-endpoints]

De volgende lijst beschrijft de eindpunten:

* **Resourceprovider**. De resourceprovider IoT-Hub toont een [Azure Resource Manager] [ lnk-arm] interface. Deze interface kunnen eigenaren van de Azure-abonnement maken en verwijderen van IoT hubs en bijwerken van de eigenschappen van de IoT-hub. Eigenschappen van de IoT Hub reguleren [hub niveau beveiligingsbeleid][lnk-accesscontrol], in plaats van op apparaatniveau access control en functionele opties voor cloud-naar-apparaat en apparaat-naar-cloud-berichten. De resourceprovider IoT-Hub kunt u ook [apparaat-id's exporteren][lnk-importexport].
* **Identiteiten Apparaatbeheer**. Elke IoT-hub toont een reeks HTTPS REST-eindpunten voor het beheren van apparaat-id's (maken, ophalen, bijwerken en verwijderen). [Apparaat-id's] [ lnk-device-identities] worden gebruikt voor verificatie en toegangsbeheer van apparaat.
* **Apparaatbeheer twin**. Elke IoT-hub toont een reeks service gerichte HTTPS REST-eindpunt voor query- en [apparaat horende] [ lnk-twins] (update voor labels en eigenschappen).
* **Taken management**. Elke IoT-hub toont een reeks service gerichte HTTPS REST-eindpunt doorzoeken en beheren [taken][lnk-jobs].
* **Apparaat-eindpunten**. Voor elk apparaat in het identiteitenregister toont IoT-Hub een set eindpunten:

  * *Apparaat-naar-cloud-berichten verzenden*. Een apparaat gebruikmaakt van dit eindpunt naar [apparaat-naar-cloud-berichten verzenden][lnk-d2c].
  * *Cloud-naar-apparaat-berichten ontvangen*. Een apparaat gebruikmaakt van dit eindpunt voor het ontvangen van gerichte [cloud-naar-apparaatberichten][lnk-c2d].
  * *Initiëren van bestandsuploads*. Een apparaat gebruikmaakt van dit eindpunt voor het ontvangen van een Azure Storage SAS-URI van IoT Hub [uploaden van een bestand][lnk-upload].
  * *Ophalen en bijwerken van twin apparaateigenschappen*. Een apparaat gebruikmaakt van dit eindpunt voor toegang tot de [apparaat twin][lnk-twins]van eigenschappen.
  * *Directe methodeaanvragen ontvangen*. Een apparaat dit eindpunt wordt geluisterd naar [directe methode][lnk-methods]van aanvragen.

    Deze eindpunten beschikbaar worden gesteld met [protocollen MQTT v3.1.1][lnk-mqtt], HTTPS 1.1, en [AMQP 1.0] [ lnk-amqp] protocollen. AMQP is ook beschikbaar via [WebSockets] [ lnk-websockets] op poort 443.

* **Service-eindpunten**. Elke IoT-hub toont een set eindpunten voor uw back-end oplossing kan communiceren met uw apparaten. Met één uitzondering, deze eindpunten zijn alleen toegankelijk met behulp van de [AMQP] [ lnk-amqp] protocol. Het aanroepen van methode eindpunt is toegankelijk via het HTTPS-protocol.
  
  * *Apparaat-naar-cloud-berichten ontvangen*. Dit eindpunt is compatibel met [Azure Event Hubs][lnk-event-hubs]. Een back-endservice kunt gebruiken om te lezen van de [apparaat-naar-cloudberichten] [ lnk-d2c] verzonden door uw apparaten. U kunt aangepaste eindpunten maken op uw IoT-hub naast deze ingebouwd eindpunt.
  * *Cloud-naar-apparaat-berichten verzenden en ontvangen van bevestigingen voor levering*. Deze eindpunten inschakelen in uw back-end oplossing voor het verzenden van betrouwbare [cloud-naar-apparaatberichten][lnk-c2d], en de bijbehorende levering of verlopen bevestigingen ontvangen.
  * *Bestandsmeldingen ontvangen*. Dit eindpunt messaging kunt u meldingen ontvangen wanneer uw apparaten is een bestand uploaden. 
  * *Directe aanroepen van de methode*. Dit eindpunt kan een back-end-service aan te roepen een [directe methode] [ lnk-methods] op een apparaat.
  * *Controle van gebeurtenissen ontvangstbewerkingen*. Dit eindpunt kunt u ontvangen van de controle van gebeurtenissen als uw IoT-hub is geconfigureerd voor het verzenden van deze bewerkingen. Zie voor meer informatie [IoT Hub operations bewaking][lnk-operations-mon].

De [Azure IoT SDK's] [ lnk-sdks] artikel beschrijft de verschillende manieren toegang krijgen tot deze eindpunten.

Alle eindpunten van IoT Hub gebruiken de [TLS] [ lnk-tls] protocol en er is geen eindpunt ooit wordt weergegeven op niet-versleuteld/niet-beveiligde kanalen.

## <a name="custom-endpoints"></a>Aangepaste eindpunten

U kunt bestaande Azure-services in uw abonnement koppelen aan uw IoT-hub om te fungeren als eindpunten voor het routeren van berichten. Deze eindpunten fungeren als service-eindpunten en worden gebruikt als een PUT voor berichtroutes. Apparaten kunnen niet rechtstreeks naar de extra eindpunten schrijven. Zie voor meer informatie over berichtroutes voor, de vermelding van de handleiding voor ontwikkelaars op [verzenden en ontvangen berichten met iothub][lnk-devguide-messaging].

IoT Hub ondersteunt momenteel de volgende Azure-services als extra eindpunten:

* Azure Storage-containers
* Event Hubs
* Service Bus-wachtrijen
* Service Bus-onderwerpen

IoT Hub moet schrijftoegang tot deze service-eindpunten voor berichtroutering om te werken. Als u uw eindpunten via de Azure portal configureert, worden de benodigde machtigingen voor u toegevoegd. Zorg ervoor dat u uw services ter ondersteuning van de verwachte doorvoer configureren. Wanneer u uw IoT-oplossing voor het eerst configureert, moet u uw extra eindpunten controleren en eventuele benodigde aanpassingen maken voor de daadwerkelijke laadbewerking.

Als een bericht overeenkomt met meerdere routes dat alle verwijzen naar hetzelfde eindpunt, biedt IoT Hub bericht slechts één keer naar dat eindpunt. Daarom is het niet nodig voor het configureren van Ontdubbeling op uw Service Bus-wachtrij of onderwerp. In gepartitioneerde wachtrijen garandeert partitie affiniteit bericht bestellen.

Zie voor de grenzen van het aantal eindpunten die u kunt toevoegen, [quota's en beperking][lnk-devguide-quotas].

### <a name="when-using-azure-storage-containers"></a>Bij gebruik van Azure Storage-containers

IoT Hub biedt alleen ondersteuning voor het schrijven van gegevens naar Azure Storage-containers als blobs in de [Apache Avro](http://avro.apache.org/) indeling. IoT Hub berichten batches en schrijft gegevens naar een blob wanneer deze van een bepaalde grootte bereikt of nadat een bepaalde hoeveelheid tijd is verstreken, afhankelijk van wat er gebeurt eerst. IoT Hub worden geschreven met een lege blob als er geen gegevens zijn schrijven.

IoT Hub wordt standaard ingesteld op de volgende naamconventie van bestand:

```
{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}
```

U kunt elk bestand naamgevingsconventie die u wilt, moet u echter gebruiken alle vermelde tokens.

### <a name="when-using-service-bus-queues-and-topics"></a>Wanneer u Service Bus-wachtrijen en onderwerpen

Service Bus-wachtrijen en onderwerpen die worden gebruikt als IoT-hubeindpunten mag geen **sessies** of **detectie van dubbele** ingeschakeld. Als een van deze opties zijn ingeschakeld, het eindpunt wordt weergegeven als **onbereikbaar** in de Azure portal.

## <a name="field-gateways"></a>Veld gateways

In een IoT-oplossing een *veldgateway* tussen uw apparaten en de eindpunten van uw IoT-Hub. Deze bevindt zich doorgaans dicht bij uw apparaten. Uw apparaten communiceren rechtstreeks met de veldgateway met behulp van een protocol dat wordt ondersteund door de apparaten. De veldgateway verbindt met een IoT Hub-eindpunt met behulp van een protocol dat wordt ondersteund door de IoT Hub. Een veldgateway is mogelijk een specifiek apparaat of een laag energieniveau computer aangepaste gateway-software die wordt uitgevoerd.

U kunt [Azure IoT rand] [ lnk-iot-edge] voor het implementeren van een veldgateway. IoT-rand biedt functionaliteit, zoals multiplexing communicatie van meerdere apparaten op dezelfde IoT Hub-verbinding.

## <a name="next-steps"></a>Volgende stappen

Er zijn andere onderwerpen met naslaginformatie in deze handleiding voor ontwikkelaars van IoT-Hub:

* [IoT Hub-querytaal voor apparaat horende, taken en het routeren van berichten][lnk-devguide-query]
* [Quota's en beperking][lnk-devguide-quotas]
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
