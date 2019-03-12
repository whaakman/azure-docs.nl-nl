---
title: Azure IoT Hub-eindpunten begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - referentie-informatie over IoT Hub apparaat gerichte en gerichte service-eindpunten.
author: robinsh
manager: philmea
ms.author: robin.shahan
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.openlocfilehash: 28019163cfec1a9d2e3c12346a6aba2bd00b30b1
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57539544"
---
# <a name="reference---iot-hub-endpoints"></a>Referentie - IoT-Hub-eindpunten

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="iot-hub-names"></a>Namen van de IoT Hub

U vindt de hostnaam van de IoT-hub die als host fungeert voor uw eindpunten in de portal van uw hub **overzicht** pagina. Standaard is de DNS-naam van een IoT-hub ziet eruit als: `{your iot hub name}.azure-devices.net`.

Azure DNS kunt u een aangepaste DNS-naam voor uw IoT-hub maken. Zie [Use Azure DNS to provide custom domain settings for an Azure service](../dns/dns-custom-domain.md) (Azure DNS gebruiken om aangepaste domeininstellingen te verstrekken voor een Azure-service) voor meer informatie.

## <a name="list-of-built-in-iot-hub-endpoints"></a>Lijst met ingebouwde IoT Hub-eindpunten

Azure IoT Hub is een multitenant-service waarmee wordt aangegeven dat de functionaliteit ervan naar verschillende actoren. Het volgende diagram toont de verschillende eindpunten die IoT-Hub toont.

![IoT Hub-eindpunten](./media/iot-hub-devguide-endpoints/endpoints.png)

De volgende lijst beschrijft de eindpunten:

* **Resourceprovider**. De resourceprovider van IoT-Hub toont een [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) interface. Deze interface kunnen eigenaren van Azure-abonnement maken en verwijderen van IoT-hubs en bijwerken van eigenschappen van de IoT hub. Eigenschappen van de IoT Hub regelen [niveau van de hub beveiligingsbeleid](iot-hub-devguide-security.md#access-control-and-permissions), in plaats van op apparaatniveau, toegangsbeheer en functionele opties voor cloud-naar-apparaat- en apparaat-naar-cloud-berichten. De resourceprovider van IoT Hub kunt u ook [apparaat-id's exporteren](iot-hub-devguide-identity-registry.md#import-and-export-device-identities).

* **Apparaat-identiteitsbeheer**. Elke IoT-hub toont een set REST voor HTTPS-eindpunten voor het beheren van apparaat-id's (maken, ophalen, bijwerken en verwijderen). [Apparaat-id's](iot-hub-devguide-identity-registry.md) worden gebruikt voor verificatie en toegangsbeheer van apparaat.

* **Dubbele Apparaatbeheer**. Elke IoT-hub toont een set van gerichte service HTTPS REST-eindpunt voor de query- en [apparaatdubbels](iot-hub-devguide-device-twins.md) (update voor labels en eigenschappen).

* **Taken management**. Elke IoT-hub toont een set van gerichte service HTTPS REST-eindpunt voor query's uitvoeren en beheren van [taken](iot-hub-devguide-jobs.md).

* **Apparaat-eindpunten**. Voor elk apparaat in het id-register toont IoT-Hub een set met eindpunten:

  * *Apparaat-naar-cloud-berichten verzenden*. Een apparaat maakt gebruik van dit eindpunt naar [apparaat-naar-cloud-berichten verzenden](iot-hub-devguide-messages-d2c.md).

  * *Cloud-naar-apparaat-berichten ontvangen*. Een apparaat maakt gebruik van dit eindpunt voor het ontvangen van gerichte [cloud-naar-apparaatberichten](iot-hub-devguide-messages-c2d.md).

  * *Starten van het uploaden van bestanden*. Een apparaat maakt gebruik van dit eindpunt voor het ontvangen van een Azure Storage SAS-URI van IoT Hub kunt u [uploaden van een bestand](iot-hub-devguide-file-upload.md).

  * *Ophalen en bijwerken van apparaatdubbeleigenschappen*. Een apparaat dit eindpunt gebruikt voor toegang tot de [apparaatdubbel](iot-hub-devguide-device-twins.md)van eigenschappen.

  * *Ontvangen aanvragen van de directe methode*. Een apparaat maakt gebruik van dit eindpunt om te luisteren naar [directe methode](iot-hub-devguide-direct-methods.md)van aanvragen.

    Deze eindpunten worden beschikbaar gesteld met [MQTT v3.1.1](https://mqtt.org/), HTTPS 1.1, en [AMQP 1.0](https://www.amqp.org/) protocollen. AMQP is ook beschikbaar via [WebSockets](https://tools.ietf.org/html/rfc6455) op poort 443.

* **Service-eindpunten**. Elke IoT-hub toont een set met eindpunten voor uw back-end oplossing om te communiceren met uw apparaten. Met één uitzondering, deze eindpunten zijn alleen toegankelijk met behulp van de [AMQP](https://www.amqp.org/) protocol. Het eindpunt van de methode aanroepen is toegankelijk via het HTTPS-protocol.
  
  * *Apparaat-naar-cloud-berichten ontvangen*. Dit eindpunt is compatibel met [Azure Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Een back-end-service kunt gebruiken om te lezen de [apparaat-naar-cloud-berichten](iot-hub-devguide-messages-d2c.md) verzonden door uw apparaten. U kunt aangepaste eindpunten op uw IoT-hub naast deze ingebouwde eindpunt maken.
  
  * *Cloud-naar-apparaat-berichten verzenden en ontvangen van bevestigingen voor levering*. Deze eindpunten kunt u uw back-end oplossing voor het verzenden van betrouwbare [cloud-naar-apparaatberichten](iot-hub-devguide-messages-c2d.md), en de bijbehorende levering of verlopen bevestigingen ontvangen.
  
  * *Bestand ontvangen*. Dit eindpunt berichten kunt u meldingen ontvangen wanneer uw apparaten is een bestand uploadt. 
  
  * *Directe aanroepen van de methode*. Dit eindpunt kan een back-end-service om aan te roepen een [directe methode](iot-hub-devguide-direct-methods.md) op een apparaat.
  
  * *Bewerkingen controleren gebeurtenissen ontvangen*. Dit eindpunt kunt u voor het ontvangen van gebeurtenissen controleren als uw IoT-hub is geconfigureerd voor het verzenden van deze bewerkingen. Zie voor meer informatie, [IoT Hub-bewerkingen controleren](iot-hub-operations-monitoring.md).

De [Azure IoT SDK's](iot-hub-devguide-sdks.md) artikel beschrijft de verschillende manieren toegang hebben tot deze eindpunten.

Alle IoT Hub-eindpunten gebruiken de [TLS](https://tools.ietf.org/html/rfc5246) -protocol en er is geen eindpunt ooit wordt weergegeven op niet-versleutelde/niet-beveiligde kanalen.

## <a name="custom-endpoints"></a>Aangepaste eindpunten

U kunt bestaande Azure-services in uw abonnement koppelen aan uw IoT-hub om te fungeren als eindpunten voor het routeren van berichten. Deze eindpunten fungeren als een service-eindpunten en worden gebruikt als sink voor berichtroutes. Apparaten kunnen niet rechtstreeks naar de extra eindpunten worden geschreven. Meer informatie over [berichtroutering](../iot-hub/iot-hub-devguide-messages-d2c.md).

IoT Hub ondersteunt momenteel de volgende Azure-services als extra eindpunten:

* Azure-opslagcontainers
* Event Hubs
* Service Bus-wachtrijen
* Service Bus-onderwerpen

Zie voor de grenzen van het aantal eindpunten die u kunt toevoegen, [quota en beperkingen](iot-hub-devguide-quotas-throttling.md).

U kunt de REST-API gebruiken [ophalen eindpunt Health](https://docs.microsoft.com/de-de/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) om op te halen van de status van de eindpunten. Wordt u aangeraden de [metrische gegevens van IoT-Hub](iot-hub-metrics.md) met betrekking tot routering bericht latentie om te bepalen en fouten opsporen, wanneer de status van het eindpunt dode of niet in orde is.

|Status|Description|
|---|---|
|in orde|Het eindpunt is berichten geaccepteerd zoals verwacht.|
|Niet in orde|Het eindpunt is geen berichten geaccepteerd zoals verwacht en IoT Hub wordt opnieuw geprobeerd om gegevens te verzenden naar dit eindpunt. De status van een eindpunt niet in orde wordt bijgewerkt op in orde wanneer IoT Hub heeft een uiteindelijke consistente status van de gezondheid van tot stand worden gebracht.|
|onbekend|IoT Hub is een verbinding met het eindpunt is niet ingesteld. Er zijn geen berichten zijn die worden geleverd aan of afgewezen op dit eindpunt.|
|Dead|Het eindpunt accepteert geen berichten, na het IoT-Hub opnieuw verzenden van berichten voor de periode retrial uitgevoerd.|

## <a name="field-gateways"></a>Veldgateways

In een IoT-oplossing een *veldgateway* tussen uw apparaten en uw IoT Hub-eindpunten. Deze bevindt zich doorgaans dicht bij uw apparaten. Uw apparaten communiceren rechtstreeks met de veldgateway met behulp van een protocol dat wordt ondersteund door de apparaten. De veldgateway verbindt met een IoT Hub-eindpunt met behulp van een protocol dat wordt ondersteund door IoT Hub. Een veldgateway mogelijk op een specifiek apparaat of een stand-by-computer waarop aangepaste gateway-software wordt uitgevoerd.

U kunt [Azure IoT Edge](/azure/iot-edge/) voor het implementeren van een veldgateway. IoT Edge biedt de functionaliteit, zoals multiplexing communicatie van meerdere apparaten op dezelfde IoT Hub-verbinding.

## <a name="next-steps"></a>Volgende stappen

Er zijn andere onderwerpen met naslaginformatie in deze IoT Hub developer guide:

* [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering](iot-hub-devguide-query-language.md)
* [Quota en beperkingen](iot-hub-devguide-quotas-throttling.md)
* [IoT Hub MQTT-ondersteuning](iot-hub-mqtt-support.md)
