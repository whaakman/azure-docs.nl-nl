---
title: Informatie over Azure IoT Hub apparaat-naar-cloud messaging | Microsoft Docs
description: Ontwikkelaars begeleiden - het gebruik van apparaat-naar-cloud-berichten met IoT Hub. Bevat informatie over het verzenden van Telemetrie- en niet-telemtry gegevens en het gebruik van routering om berichten te bezorgen.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: be87b00f27f0d0b25cd77a0634ab1c653a85e5ac
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126439"
---
# <a name="send-device-to-cloud-messages-to-iot-hub"></a>Apparaat-naar-cloud-berichten verzenden naar IoT Hub

Voor het verzenden van time series-telemetriegegevens en waarschuwingen van uw apparaten naar de back-end van uw oplossing, apparaat-naar-cloud-berichten van uw apparaat te verzenden naar uw IoT hub. Zie voor een bespreking van de andere opties die apparaat-naar-cloud worden ondersteund door IoT Hub, [richtlijnen voor communicatie van apparaat-naar-cloud][lnk-d2c-guidance].

Verzenden van apparaat-naar-cloud-berichten via een apparaat gerichte-eindpunt (**/devices/ {apparaat-id} / berichten/gebeurtenissen**). Regels voor doorsturen en vervolgens uw berichten routeren naar een van de service gerichte eindpunten op uw IoT-hub. Regels voor doorsturen gebruiken de headers en hoofdtekst van de apparaat-naar-cloud-berichten om te bepalen waar u wilt ze doorsturen. Standaard berichten worden doorgestuurd naar het eindpunt van de ingebouwde service gerichte (**berichten/gebeurtenissen**), dat wil zeggen compatibel is met [Event Hubs][lnk-event-hubs]. Daarom kunt u standaard [Event Hubs-integratie en SDK's] [ lnk-compatible-endpoint] voor het ontvangen van apparaat-naar-cloud-berichten in de back-end van uw oplossing.

IoT Hub implementeert het apparaat-naar-cloud-berichten met behulp van een streaming messaging-patroon. De IoT Hub apparaat-naar-cloud-berichten lijken meer op [Event Hubs] [ lnk-event-hubs] *gebeurtenissen* dan [Service Bus] [ lnk-servicebus] *berichten* in dat er is een groot aantal gebeurtenissen dat is doorgegeven via de service die kan worden gelezen door meerdere lezers.

Apparaat-naar-cloud-berichten met IoT Hub heeft de volgende kenmerken:

* Apparaat-naar-cloud-berichten zijn duurzame en kunnen worden bewaard in een IoT-hub standaard **berichten/gebeurtenissen** -eindpunt voor maximaal zeven dagen.
* Apparaat-naar-cloud-berichten mag maximaal 256 KB, en kunnen worden gegroepeerd in batches verzendt optimaliseren. Batches mag maximaal 256 KB.
* Zoals uitgelegd in de [beheren van toegang tot IoT Hub] [ lnk-devguide-security] sectie, IoT-Hub kunt per apparaat verificatie en toegangsbeheer.
* IoT Hub kunt u maximaal 10 aangepaste eindpunten maken. Berichten worden bezorgd op de eindpunten op basis van routes die zijn geconfigureerd op uw IoT-hub. Zie voor meer informatie, [Routing regels](iot-hub-devguide-query-language.md#device-to-cloud-message-routes-query-expressions).
* Met IoT Hub kunt u miljoenen gelijktijdig verbonden apparaten (Zie [quota en beperkingen][lnk-quotas]).
* IoT Hub is niet toegestaan voor het partitioneren van willekeurige. Apparaat-naar-cloud-berichten worden gepartitioneerd op basis van hun oorspronkelijke **deviceId**.

Zie voor meer informatie over de verschillen tussen IoT-Hub en Event Hubs [vergelijking van Azure IoT Hub en Azure Event Hubs][lnk-comparison].

## <a name="send-non-telemetry-traffic"></a>Niet-Telemetrisch verkeer verzenden

Vaak naast telemetrie verzenden apparaten van berichten en aanvragen die afzonderlijk kan worden uitgevoerd en verwerken in de back-end oplossing vereist. Bijvoorbeeld, kritieke waarschuwingen die een specifieke actie in de back-end moeten activeren. U kunt schrijven een [routeringsregel] [ lnk-devguide-custom] voor het verzenden van dergelijke berichten naar een eindpunt dat is toegewezen aan de verwerking op basis van een van beide een koptekst voor het bericht of een waarde in de hoofdtekst van het bericht.

Zie voor meer informatie over de beste manier om dit type bericht te verwerken, de [zelfstudie: How to process IoT Hub apparaat-naar-cloud-berichten verwerken] [ lnk-d2c-tutorial] zelfstudie.

## <a name="route-device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten routeren

U hebt twee opties voor apparaat-naar-cloud-berichten routeren naar uw back-end-apps:

* Gebruik de ingebouwde [Event Hub-compatibele eindpunt] [ lnk-compatible-endpoint] om in te schakelen van back-end-apps lezen van de apparaat-naar-cloud-berichten ontvangen door de hub. Zie voor meer informatie over de ingebouwde Event Hub-compatibele eindpunt, [apparaat-naar-cloud-berichten lezen van het ingebouwde eindpunt][lnk-devguide-builtin].
* Regels voor doorsturen gebruiken voor het verzenden van berichten met aangepaste eindpunten in uw IoT-hub. Aangepaste eindpunten kunt uw back-end-apps om apparaat-naar-cloud-berichten met Event Hubs, Service Bus-wachtrijen of Service Bus-onderwerpen te lezen. Zie voor meer informatie over Routering en aangepaste eindpunten, [aangepaste eindpunten en routeringsregels gebruiken voor apparaat-naar-cloud-berichten][lnk-devguide-custom].

## <a name="anti-spoofing-properties"></a>Eigenschappen van anti-adresvervalsing (spoofing)

Om te voorkomen dat apparaat adresvervalsing (spoofing) in apparaat-naar-cloud-berichten, IoT-Hub stempels alle berichten met de volgende eigenschappen:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

De eerste twee bevatten de **deviceId** en **generationId** van het oorspronkelijke apparaat volgens [identiteit apparaateigenschappen][lnk-device-properties].

De **ConnectionAuthMethod** eigenschap bevat een JSON-geserialiseerd object met de volgende eigenschappen:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de SDK's die u kunt gebruiken voor het verzenden van apparaat-naar-cloud-berichten [Azure IoT SDK's][lnk-sdks].

De [snelstartgidsen] [ lnk-get-started] ziet u hoe u apparaat-naar-cloud-berichten verzenden vanuit gesimuleerde apparaten. Zie voor meer informatie de [Process IoT Hub apparaat-naar-cloud-berichten met behulp van routes] [ lnk-d2c-tutorial] zelfstudie.

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: quickstart-send-telemetry-node.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: tutorial-routing.md
