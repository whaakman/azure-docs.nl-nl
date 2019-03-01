---
title: Informatie over Azure IoT Hub cloud-naar-apparaat-berichten | Microsoft Docs
description: Ontwikkelaars begeleiden - het gebruik van cloud-naar-apparaat met IoT Hub-berichten. Bevat informatie over de levenscyclus van het bericht en de configuratie-opties.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: c8424743f30ec1bbf8d8096f6630c7451bc910c8
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010239"
---
# <a name="send-cloud-to-device-messages-from-iot-hub"></a>Cloud-naar-apparaat-berichten verzenden vanuit IoT Hub

Eenzijdige om meldingen te verzenden naar de apparaat-app uit de back-end, cloud-naar-apparaten berichten uit IoT hub te verzenden naar uw apparaat. Zie voor een bespreking van de andere opties cloud naar apparaten die worden ondersteund door IoT Hub, [Cloud-to-device communications guidance](iot-hub-devguide-c2d-guidance.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Verzenden van berichten van cloud-naar-apparaat via een gerichte service-eindpunt (**/berichten/devicebound**). Een apparaat ontvangt vervolgens de berichten via een apparaatspecifiek eindpunt (**/devices/ {apparaat-id} / berichten/devicebound**).

Voor elk cloud-naar-apparaat bericht in een enkel apparaat, IoT-Hub stelt de **naar** eigenschap **/devices/ {apparaat-id} / berichten/devicebound**.

Elke apparaatwachtrij bevat maximaal 50 cloud-naar-apparaat-berichten. Wilt meer berichten verzenden naar de resultaten hetzelfde apparaat een fout.

## <a name="the-cloud-to-device-message-lifecycle"></a>De levenscyclus van cloud-naar-apparaat bericht

Om te garanderen bezorging van berichten op-één keer, persistente IoT-Hub cloud-naar-apparaat-berichten in wachtrijen per apparaat. Apparaten moeten expliciet erkent *voltooiing* voor IoT-Hub te verwijderen uit de wachtrij. Deze aanpak zorgt ervoor dat tolerantie tegen connectiviteit en apparaatfouten.

Het volgende diagram toont de grafiek van de levenscyclus van status voor een cloud-naar-apparaat-bericht van IoT-Hub.

![Levenscyclus van cloud-naar-apparaat bericht](./media/iot-hub-devguide-messages-c2d/lifecycle.png)

Wanneer de IoT Hub-service een bericht naar een apparaat verzendt, wordt de berichtstatus met de service ingesteld op **in de wachtrij geplaatste**. Wanneer een apparaat wil *ontvangen* een bericht, IoT-Hub *vergrendelingen* het bericht (door de status in te stellen **onzichtbaar**), waarmee andere threads op het apparaat te ontvangen andere berichten. Wanneer een thread van het apparaat de verwerking van een bericht is voltooid, wordt de hoogte gebracht IoT-Hub door *voltooien* het bericht. De status van de IoT Hub vervolgens ingesteld op **voltooid**.

Een apparaat kunt ook kiezen:

* *Afwijzen* het bericht dat ervoor zorgt dat de IoT Hub kunt u dit instellen op de **dode lettered** staat. Apparaten die verbinding via het MQTT-protocol maken niet cloud-naar-apparaat-berichten worden afgewezen.

* *Afbreken* het bericht dat ervoor zorgt dat de IoT Hub kunt u het bericht terug in de wachtrij geplaatst met de status ingesteld op **in de wachtrij geplaatste**. Apparaten die verbinding via het MQTT-protocol maken kunnen niet afbreken cloud-naar-apparaat-berichten.

Een thread kan voor het verwerken van een bericht zonder de IoT Hub meldingen mislukken. In dit geval automatisch de overgang van berichten van de **onzichtbaar** staat terug naar de **in de wachtrij geplaatste** status na een *time-out voor zichtbaarheid (of vergrendelen)*. De standaardwaarde van deze time-out is één minuut.

De **maximum aantal bezorgingen** eigenschap op IoT-Hub bepaalt het maximum aantal keren dat een bericht tussen schakelen kunt de **in de wachtrij geplaatste** en **onzichtbaar** Staten. Na dit aantal overgangen, IoT-Hub stelt de status van het bericht **dode lettered**. Op dezelfde manier, IoT-Hub stelt de status van een bericht naar **dode lettered** na de vervaltijd (Zie [Time to live van](#message-expiration-time-to-live)).

De [over het verzenden van berichten van cloud-naar-apparaat met IoT Hub](iot-hub-csharp-csharp-c2d.md) ziet u hoe u cloud-naar-apparaat-berichten vanuit de cloud verzendt en ontvangt op een apparaat.

Een apparaat wordt normaal gesproken een cloud-naar-apparaat bericht voltooid wanneer het verlies van gegevens van het bericht heeft geen invloed op de toepassingslogica. Bijvoorbeeld, wanneer het apparaat het bericht inhoud lokaal is opgeslagen of is een bewerking uitgevoerd. Het bericht kan ook uitvoeren voor tijdelijke gegevens, waarvan verlies niet gevolgen voor de functionaliteit van de toepassing hebben zou. Voor langlopende taken kunt u soms:

* Het bericht cloud-naar-apparaat voltooien na het opslaan van de taakbeschrijving in de lokale opslag.

* Informeer de back-end oplossing met een of meer apparaat-naar-cloud-berichten in verschillende stadia van de voortgang van de taak.

## <a name="message-expiration-time-to-live"></a>Verlopen van berichten (time to live van)

Elk bericht dat cloud-naar-apparaat heeft een verlooptijd. Deze tijd is ingesteld door een van:

* De **ExpiryTimeUtc** eigenschap in de service.
* IoT-Hub met behulp van de standaard *time to live van* opgegeven als een eigenschap van een IoT-Hub.

Zie [configuratieopties voor Cloud-naar-apparaat](#cloud-to-device-configuration-options).

Een veelgebruikte manier om te profiteren van verlopen van berichten en te voorkomen dat is berichten verzenden naar niet-verbonden apparaten, het tijd in live waarden instellen. Deze benaderingen geven hetzelfde resultaat als de status van de apparaat-verbinding terwijl u efficiënter te onderhouden. Wanneer u een bericht bevestigingen aanvraagt, ontvangt u die apparaten zijn een IoT-Hub melding:

* Kan om berichten te ontvangen.
* Niet online zijn of zijn mislukt.

## <a name="message-feedback"></a>Bericht feedback

Als u een cloud-naar-apparaat bericht verzendt, kan de service de levering van per bericht feedback over de laatste status van dat bericht aanvragen.

| ACK-eigenschap | Gedrag |
| ------------ | -------- |
| **positieve** | Als het cloud-naar-apparaat bericht bereikt de **voltooid** staat, IoT-Hub genereert een feedbackbericht. |
| **negatief zijn** | Als het cloud-naar-apparaat bericht bereikt de **dode lettered** staat, IoT-Hub genereert een feedbackbericht. |
| **Volledige**     | IoT Hub genereert een feedbackbericht in beide gevallen. |

Als **Ack** is **volledige**, en u niet een feedbackbericht ontvangt, betekent dit dat het Feedbackbericht is verlopen. De service weten niet wat is er gebeurd met het oorspronkelijke bericht. In de praktijk moet een service die de feedback die kunnen worden verwerkt voordat deze verloopt. De maximale vervaltijd is twee dagen tijd om op te halen van de service blijft nogmaals uit te voeren als er een fout optreedt.

Zoals uitgelegd in [eindpunten](iot-hub-devguide-endpoints.md), IoT-Hub biedt feedback via een gerichte service-eindpunt (**/messages/servicebound/feedback**) als berichten. De semantiek voor het ontvangen van feedback zijn dezelfde als die voor cloud-naar-apparaat-berichten. Indien mogelijk, batch bericht feedback wordt verwerkt in een enkel bericht met de volgende indeling:

| Eigenschap     | Description |
| ------------ | ----------- |
| EnqueuedTime | De tijdstempel die aangeeft wanneer de Feedbackbericht is ontvangen door de hub. |
| UserId       | `{iot hub name}` |
| ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

De hoofdtekst is een JSON-geserialiseerd matrix van records, elk met de volgende eigenschappen:

| Eigenschap           | Description |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | De tijdstempel die aangeeft wanneer het resultaat van het bericht is er gebeurd. Bijvoorbeeld, de hub de Feedbackbericht ontvangen of het oorspronkelijke bericht is verlopen. |
| OriginalMessageId  | **MessageId** van het cloud-naar-apparaat bericht waarop deze feedback informatie betrekking heeft. |
| StatusCode         | Vereiste tekenreeks. In de feedback die die zijn gegenereerd door de IoT Hub gebruikt. <br/> 'Success' <br/> 'Expired' <br/> 'DeliveryCountExceeded' <br/> 'Geweigerd' <br/> 'Purged' |
| Description        | Waarden voor de tekenreeks **StatusCode**. |
| DeviceId           | **Apparaat-id** van het doelapparaat van de cloud-naar-apparaat bericht waarop deze stukje feedback betrekking heeft. |
| DeviceGenerationId | **DeviceGenerationId** van het doelapparaat van de cloud-naar-apparaat bericht waarop deze stukje feedback betrekking heeft. |

De service moet opgeven een **MessageId** voor de cloud-naar-apparaat bericht kunnen correleren van de feedback met het oorspronkelijke bericht.

Het volgende voorbeeld ziet de hoofdtekst van een feedbackbericht.

```json
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0,
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

## <a name="cloud-to-device-configuration-options"></a>Configuratieopties voor cloud-naar-apparaat

Elke IoT hub bevat de volgende configuratieopties voor cloud-naar-apparaat-berichten:

| Eigenschap                  | Description | Bereik en de standaard |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | Standaard-TTL voor cloud-naar-apparaat-berichten. | Interval voor ISO_8601 maximaal 2D (minimaal 1 minuut). Standaard: 1 uur. |
| maxDeliveryCount          | Maximumaantal leveringen voor cloud-naar-apparaat per apparaat wachtrijen. | 1 tot en met 100. Standaard: 10. |
| feedback.ttlAsIso8601     | Bewaartermijn voor Feedbackberichten afhankelijk van de service. | Interval voor ISO_8601 maximaal 2D (minimaal 1 minuut). Standaard: 1 uur. |
| feedback.maxDeliveryCount |Maximumaantal leveringen voor feedbackwachtrij. | 1 tot en met 100. Standaard: 100. |

Zie voor meer informatie over het instellen van deze configuratieopties [maken IoT-hubs](iot-hub-create-through-portal.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de SDK's die u kunt gebruiken om cloud-naar-apparaat-berichten te ontvangen [Azure IoT SDK's](iot-hub-devguide-sdks.md).

Als u wilt uitproberen ontvangen van berichten van cloud-naar-apparaat, Zie de [cloud naar apparaat verzenden](iot-hub-csharp-csharp-c2d.md) zelfstudie.