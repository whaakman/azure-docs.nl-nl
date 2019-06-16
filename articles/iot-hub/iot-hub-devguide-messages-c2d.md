---
title: Informatie over Azure IoT Hub cloud-naar-apparaat-berichten | Microsoft Docs
description: Deze handleiding voor ontwikkelaars wordt beschreven hoe u cloud-naar-apparaat-berichten met uw IoT-hub. Het bevat informatie over de levenscyclus van het bericht en de configuratie-opties.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: 0fc1b65a4ba1c8a3d76b48206d6a4703035e05bc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055321"
---
# <a name="send-cloud-to-device-messages-from-an-iot-hub"></a>Cloud-naar-apparaat-berichten verzenden vanuit een IoT-hub

Eenzijdige om meldingen te verzenden naar een apparaat-app uit de back-end, cloud-naar-apparaat-berichten van uw IoT-hub te verzenden naar uw apparaat. Zie voor een bespreking van de andere opties die cloud-naar-apparaat worden ondersteund door Azure IoT Hub, [Cloud-to-device communications guidance](iot-hub-devguide-c2d-guidance.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Verzenden van berichten van cloud-naar-apparaat via een eindpunt service gerichte */berichten/devicebound*. Een apparaat ontvangt vervolgens de berichten via een eindpunt apparaatspecifieke */devices/ {apparaat-id} / berichten/devicebound*.

Voor elk cloud-naar-apparaat bericht in een enkel apparaat, uw IoT-hub stelt de **naar** eigenschap */devices/ {apparaat-id} / berichten/devicebound*.

Elke apparaatwachtrij bevat, maximaal 50 cloud-naar-apparaat-berichten. Om te proberen meer berichten te verzenden naar de resultaten hetzelfde apparaat een fout.

## <a name="the-cloud-to-device-message-life-cycle"></a>De levenscyclus van cloud-naar-apparaat bericht

Om te garanderen bezorging van berichten op-één keer, persistente uw IoT-hub cloud-naar-apparaat-berichten in wachtrijen per apparaat. Voor de IoT-hub te verwijderen van de berichten uit de wachtrij de apparaten expliciet moeten bevestigen *voltooiing*. Deze aanpak zorgt ervoor dat tolerantie tegen connectiviteit en apparaatfouten.

De levenscyclus van een status-grafiek wordt weergegeven in het volgende diagram:

![Levenscyclus van de cloud-naar-apparaat bericht](./media/iot-hub-devguide-messages-c2d/lifecycle.png)

Wanneer de IoT hub-service een bericht naar een apparaat verzendt, wordt de berichtstatus met de service ingesteld op *in de wachtrij geplaatste*. Wanneer een apparaat wil *ontvangen* een bericht, de IoT-hub *vergrendelingen* het bericht door de status in te stellen *onzichtbaar*. Deze status kan andere threads op het apparaat om te beginnen met het ontvangen van andere berichten. Wanneer een apparaat-thread de verwerking van een bericht is voltooid, wordt rechtsonder weergegeven door de IoT-hub *voltooien* het bericht. De IoT-hub vervolgens de status wordt ingesteld op *voltooid*.

Een apparaat kan ook:

* *Afwijzen* het bericht dat ervoor zorgt de IoT-hub dat kunt u dit instellen op de *dode lettered* staat. Apparaten die verbinding via de Message Queuing-telemetrie Transport (MQTT)-Protocol maken niet cloud-naar-apparaat-berichten worden afgewezen.

* *Afbreken* het bericht dat ervoor zorgt de IoT-hub dat kunt u het bericht terug in de wachtrij geplaatst met de status ingesteld op *in de wachtrij geplaatste*. Apparaten die verbinding via het MQTT-Protocol maken kunnen niet afbreken cloud-naar-apparaat-berichten.

Een thread kan voor het verwerken van een bericht zonder de IoT-hub meldingen mislukken. In dit geval automatisch de overgang van berichten van de *onzichtbaar* staat terug naar de *in de wachtrij geplaatste* status na een *zichtbaarheid* time-out (of *vergrendeling* time). De standaardwaarde van de time-outwaarde is één minuut.

De **maximum aantal bezorgingen** eigenschap van de IoT-hub bepaalt het maximum aantal keren dat een bericht tussen schakelen kunt de *in de wachtrij geplaatste* en *onzichtbaar* Staten. Na dit aantal overgangen, de IoT-hub stelt de status van het bericht *dode lettered*. Op dezelfde manier de IoT-hub stelt de status van een bericht naar *dode lettered* na de vervaltijd. Zie voor meer informatie, [Time to live van](#message-expiration-time-to-live).

De [over het verzenden van berichten van cloud-naar-apparaat met IoT Hub](iot-hub-csharp-csharp-c2d.md) artikel leest u hoe u cloud-naar-apparaat-berichten vanuit de cloud verzendt en ontvangt op een apparaat.

Een apparaat is een cloud-naar-apparaat bericht gewoonlijk voltooid wanneer het verlies van gegevens van het bericht heeft geen invloed op de toepassingslogica. Een voorbeeld hiervan is mogelijk wanneer het apparaat heeft persistente lokaal de inhoud van het bericht of een bewerking met succes is uitgevoerd. Het bericht kan ook uitvoeren voor tijdelijke gegevens, waarvan verlies voor de functionaliteit van de toepassing gevolgen wouldn't. Voor langlopende taken kunt u soms:

* Het cloud-naar-apparaat-bericht voltooien nadat het apparaat heeft persistente de taakbeschrijving in de lokale opslag.

* Informeer de back-end oplossing met een of meer apparaat-naar-cloud-berichten in verschillende stadia van de voortgang van de taak.

## <a name="message-expiration-time-to-live"></a>Verlopen van berichten (time to live van)

Elk bericht dat cloud-naar-apparaat heeft een verlooptijd. Deze tijd is ingesteld door een van de volgende:

* De **ExpiryTimeUtc** eigenschap in de service
* De IoT-hub met behulp van de standaard *time to live van* die opgegeven als een eigenschap van een IoT hub

Zie [configuratieopties voor Cloud-naar-apparaat](#cloud-to-device-configuration-options).

Een veelgebruikte manier om te profiteren van een verlopen van berichten en om te voorkomen dat berichten verzenden naar niet-verbonden apparaten is om in te stellen korte *time to live van* waarden. Deze benaderingen geven hetzelfde resultaat als het onderhouden van de verbindingsstatus van het apparaat, maar het is efficiënter. Wanneer u een bericht bevestigingen aanvraagt, wordt de IoT-hub waarschuwt u die apparaten zijn:

* Kan om berichten te ontvangen.
* Niet online zijn of zijn mislukt.

## <a name="message-feedback"></a>Bericht feedback

Als u een cloud-naar-apparaat bericht verzendt, kan de service de levering van per bericht feedback over de laatste status van dat bericht aanvragen. U doet dit door in te stellen de **iothub-ack** eigenschap van de toepassing in de cloud-naar-apparaat-bericht dat wordt verzonden naar een van de volgende vier waarden:

| De waarde van de eigenschap ACK | Gedrag |
| ------------ | -------- |
| Geen     | De IoT-hub niet genereren een feedbackbericht (standaardinstelling). |
| positief | Als het cloud-naar-apparaat bericht bereikt de *voltooid* staat, de IoT-hub genereert een feedbackbericht. |
| negatief zijn | Als het cloud-naar-apparaat bericht bereikt de *dode lettered* staat, de IoT-hub genereert een feedbackbericht. |
| volledig     | De IoT-hub genereert een feedbackbericht in beide gevallen. |

Als de **Ack** waarde *volledige*, en u niet een feedbackbericht ontvangt, betekent dit dat het Feedbackbericht is verlopen. De service weten niet wat is er gebeurd met het oorspronkelijke bericht. In de praktijk moet een service die de feedback die kunnen worden verwerkt voordat deze verloopt. De verlooptijd van de maximale is twee dagen tijd om op te halen van de service blijft nogmaals uit te voeren als er een fout optreedt.

Zoals uitgelegd in [eindpunten](iot-hub-devguide-endpoints.md), de IoT-hub biedt feedback via een gerichte service-eindpunt, */messages/servicebound/feedback*, als berichten. De semantiek voor het ontvangen van feedback zijn dezelfde als die voor cloud-naar-apparaat-berichten. Indien mogelijk, batch bericht feedback wordt verwerkt in een enkel bericht met de volgende indeling:

| Eigenschap     | Description |
| ------------ | ----------- |
| EnqueuedTime | Een tijdstempel die aangeeft wanneer de Feedbackbericht is ontvangen door de hub |
| Gebruikers-id       | `{iot hub name}` |
| ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

De hoofdtekst is een JSON-geserialiseerd matrix van records, elk met de volgende eigenschappen:

| Eigenschap           | Description |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | Een tijdstempel die aangeeft wanneer het resultaat van het bericht is er gebeurd (bijvoorbeeld de hub de Feedbackbericht ontvangen of het oorspronkelijke bericht verlopen) |
| OriginalMessageId  | De *MessageId* van het cloud-naar-apparaat bericht waarop deze feedback informatie betrekking heeft |
| StatusCode         | Een vereiste tekenreeks, die wordt gebruikt in de feedbackberichten die worden gegenereerd door de IoT-hub: <br/> *Geslaagd* <br/> *Verlopen* <br/> *DeliveryCountExceeded* <br/> *Geweigerd* <br/> *Opgeschoond* |
| Description        | Waarden voor de tekenreeks *StatusCode* |
| DeviceId           | De *DeviceId* van het doelapparaat van de cloud-naar-apparaat bericht waarop deze stukje feedback betrekking heeft |
| DeviceGenerationId | De *DeviceGenerationId* van het doelapparaat van de cloud-naar-apparaat bericht waarop deze stukje feedback betrekking heeft |

Voor de cloud-naar-apparaat-bericht naar de feedback correleren met het oorspronkelijke bericht, de service moet opgeven een *MessageId*.

De hoofdtekst van een feedbackbericht wordt weergegeven in de volgende code:

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
| defaultTtlAsIso8601       | Standaard-TTL voor cloud-naar-apparaat-berichten | Interval voor ISO_8601 tot 2 dagen (minimaal 1 minuut); Standaard: 1 uur |
| maxDeliveryCount          | Maximumaantal leveringen voor cloud-naar-apparaat per apparaat wachtrijen | 1 tot en met 100; Standaard: 10 |
| feedback.ttlAsIso8601     | Bewaartermijn voor Feedbackberichten afhankelijk van de service | Interval voor ISO_8601 tot 2 dagen (minimaal 1 minuut); Standaard: 1 uur |
| feedback.maxDeliveryCount | Maximumaantal leveringen voor de feedbackwachtrij | 1 tot en met 100; Standaard: 100 |

Zie voor meer informatie over het instellen van deze configuratieopties [maken IoT-hubs](iot-hub-create-through-portal.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de SDK's die u gebruiken kunt om cloud-naar-apparaat-berichten te ontvangen, [Azure IoT SDK's](iot-hub-devguide-sdks.md).

Als u wilt uitproberen ontvangen van berichten van cloud-naar-apparaat, Zie de [cloud naar apparaat verzenden](iot-hub-csharp-csharp-c2d.md) zelfstudie.
