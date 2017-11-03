---
title: Inzicht in Azure IoT Hub apparaat-naar-cloud messaging | Microsoft Docs
description: Handleiding voor ontwikkelaars - gebruiken met IoT Hub apparaat-naar-cloud-berichten. Bevat informatie over het verzenden van Telemetrie- en niet-telemtry gegevens, en routering gebruiken om berichten te bezorgen.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: dobett
ms.openlocfilehash: 4e346306ecb8f4897a249454c537ce9a1a4c4011
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="send-device-to-cloud-messages-to-iot-hub"></a>Apparaat-naar-cloud-berichten verzenden met IoT Hub

Voor het verzenden van telemetrie van de tijdreeks en waarschuwingen van uw apparaten naar de back-end van uw oplossing, apparaat-naar-cloud-berichten van uw apparaat te verzenden naar uw IoT-hub. Zie voor een beschrijving van de andere ondersteund door de IoT Hub apparaat-naar-cloud-opties, [apparaat-naar-cloud communicatie richtlijnen][lnk-d2c-guidance].

U apparaat-naar-cloud-berichten verzenden via een apparaat gerichte-eindpunt (**/devices/ {deviceId} / berichten/gebeurtenissen**). Routering van regels en vervolgens uw berichten doorsturen naar een van de service gerichte eindpunten op uw IoT-hub. Routeringsregels gebruiken de kopteksten en de hoofdtekst van de apparaat-naar-cloud-berichten die door uw hub bepalen waar ze doorsturen. Standaard berichten worden doorgestuurd naar het ingebouwde gerichte service-eindpunt (**berichten/gebeurtenissen**), die compatibel is met [Event Hubs][lnk-event-hubs]. Daarom kunt u standaard [Event Hubs-integratie en SDK's] [ lnk-compatible-endpoint] apparaat-naar-cloud-berichten ontvangen in de back-end van uw oplossing.

IoT Hub implementeert apparaat-naar-cloud-berichten met behulp van een streaming messaging-patroon. IoT-Hub apparaat-naar-cloudberichten lijken meer op [Event Hubs] [ lnk-event-hubs] *gebeurtenissen* dan [Service Bus] [ lnk-servicebus] *berichten* dat er wordt een groot aantal gebeurtenissen dat door de service die kan worden gelezen door meerdere lezers.

Apparaat-naar-cloud-berichten met IoT Hub heeft de volgende kenmerken:

* Apparaat-naar-cloud-berichten worden duurzame en bewaard in een IoT-hub standaard **berichten/gebeurtenissen** eindpunt voor maximaal zeven dagen.
* Apparaat-naar-cloud-berichten kunnen niet groter zijn dan 256 KB en kunnen worden gegroepeerd in batches te optimaliseren verzendt. Batches mag hoogstens 256 KB.
* Zoals wordt beschreven in de [toegangsbeheer met IoT Hub] [ lnk-devguide-security] gedeelte, IoT-Hub kunt per apparaat authenticatie en toegangsbeheer.
* IoT-Hub kunt u maximaal 10 aangepaste eindpunten maken. Berichten worden afgeleverd bij de eindpunten op basis van de routes geconfigureerd op uw IoT-hub. Zie voor meer informatie [routering regels](#routing-rules).
* IoT-Hub kunt miljoenen gelijktijdig verbonden apparaten (Zie [quota's en beperking][lnk-quotas]).
* IoT Hub is niet toegestaan voor het partitioneren van willekeurige. Apparaat-naar-cloud-berichten worden gepartitioneerd op basis van hun oorspronkelijke **deviceId**.

Zie voor meer informatie over de verschillen tussen de IoT Hub en Event Hubs services [vergelijking van Azure IoT Hub en Azure Event Hubs][lnk-comparison].

## <a name="send-non-telemetry-traffic"></a>Niet-telemetrie verkeer verzenden

Vaak naast telemetriegegevenspunten verzenden apparaten van berichten en aanvragen die afzonderlijke uitvoering en verwerking in de back-end oplossing vereisen. Bijvoorbeeld, kritieke waarschuwingen die een specifieke actie in de back-end moeten activeren. U kunt eenvoudig schrijven een [routeringsregel] [ lnk-devguide-custom] deze typen berichten verzenden naar een eindpunt dat is toegewezen aan hun verwerking op basis van de header voor het bericht of een waarde in de berichttekst.

Zie voor meer informatie over de beste manier om dit soort bericht te verwerken, de [zelfstudie: IoT Hub apparaat-naar-cloud-berichten verwerken] [ lnk-d2c-tutorial] zelfstudie.

## <a name="route-device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten rondsturen

Hebt u twee opties voor het routeren van apparaat-naar-cloud-berichten naar uw back-end-apps:

* Gebruik de ingebouwde [Event Hub-compatibele eindpunt] [ lnk-compatible-endpoint] zodat back-end apps Lees de apparaat-naar-cloud-berichten dat is ontvangen door de hub. Zie voor meer informatie over de ingebouwde Event Hub-compatibele eindpunt, [apparaat-naar-cloud-berichten lezen van het ingebouwde eindpunt][lnk-devguide-builtin].
* Regels voor doorsturen gebruiken om berichten te verzenden naar aangepaste eindpunten in uw IoT-hub. Aangepaste eindpunten inschakelen voor uw back-end-apps om apparaat-naar-cloud-berichten met Event Hubs, Service Bus-wachtrijen of Service Bus-onderwerpen te lezen. Zie voor meer informatie over Routering en aangepaste eindpunten, [aangepaste eindpunten en routeringsregels gebruikt voor apparaat-naar-cloud-berichten][lnk-devguide-custom].

## <a name="anti-spoofing-properties"></a>Eigenschappen anti-adresvervalsing (spoofing)

Om te voorkomen dat apparaat vervalsing in IoT Hub apparaat-naar-cloud-berichten stempels alle berichten met de volgende eigenschappen:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

De eerste twee bevatten de **deviceId** en **generationId** van het oorspronkelijke apparaat conform [identiteit apparaateigenschappen][lnk-device-properties].

De **ConnectionAuthMethod** eigenschap bevat een JSON-geserialiseerd object, met de volgende eigenschappen:

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de SDK's die u kunt gebruiken om apparaat-naar-cloud-berichten te verzenden [Azure IoT SDK's][lnk-sdks].

De [aan de slag] [ lnk-get-started] zelfstudies ziet u het apparaat-naar-cloud-berichten verzenden vanuit gesimuleerde en fysieke apparaten. Zie voor meer details over de [proces IoT Hub apparaat-naar-cloud-berichten met behulp van routes] [ lnk-d2c-tutorial] zelfstudie.

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: iot-hub-get-started.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
