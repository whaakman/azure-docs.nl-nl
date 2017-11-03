---
title: Inzicht in Azure IoT Hub cloud-naar-apparaat messaging | Microsoft Docs
description: Handleiding voor ontwikkelaars - het gebruik van cloud-naar-apparaat met IoT Hub berichten. Bevat informatie over de levenscyclus van het bericht en configuratie-opties.
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
ms.date: 09/06/2017
ms.author: dobett
ms.openlocfilehash: a3ebda292d16b2a420fb6d586f18201e34efffa7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="send-cloud-to-device-messages-from-iot-hub"></a>Cloud-naar-apparaat-berichten uit IoT Hub verzenden

Eenzijdige om meldingen te verzenden naar de apparaat-app uit de back-end van uw oplossing, cloud naar apparaten berichten uit uw IoT-hub te verzenden naar uw apparaat. Zie voor een beschrijving van de andere opties cloud naar apparaten die worden ondersteund door de IoT Hub, [Cloud-naar-apparaat communicatie richtlijnen][lnk-c2d-guidance].

Verzenden van berichten van de cloud-naar-apparaat via een gerichte service-eindpunt (**berichten/devicebound**). Een apparaat vervolgens ontvangt de berichten via een eindpunt apparaatspecifieke (**/devices/ {deviceId} / berichten/devicebound**).

Voor de doelhostserver elk cloud-naar-apparaat-bericht in een enkel apparaat, IoT Hub stelt de **naar** eigenschap **/devices/ {deviceId} / berichten/devicebound**.

Elke apparaatwachtrij bevat maximaal 50 cloud-naar-apparaat-berichten. Poging meer om berichten te verzenden naar de dezelfde apparaten leidt tot een fout opgetreden.

## <a name="the-cloud-to-device-message-lifecycle"></a>De levenscyclus van de cloud-naar-apparaat-bericht

Om te garanderen levering van berichten op in de minste eenmaal, persistente IoT Hub cloud-naar-apparaat-berichten in per apparaat wachtrijen. Apparaten moeten expliciet bevestigen *voltooiing* voor IoT Hub om ze te verwijderen uit de wachtrij. Deze aanpak wordt gegarandeerd dat tolerantie tegen connectiviteit en tegen fouten.

Het volgende diagram toont de grafiek lifecycle staat voor een cloud-naar-apparaat-bericht in IoT-Hub.

![De levenscyclus van de cloud-naar-apparaat-bericht][img-lifecycle]

Wanneer de service IoT Hub een bericht naar een apparaat verzendt, wordt de berichtstatus met de service ingesteld op **in wachtrij gezet**. Wanneer een apparaat wil *ontvangen* een bericht voor de IoT Hub *vergrendelingen* het bericht (door de status in te stellen op **onzichtbaar**), waarmee andere threads op het apparaat te ontvangen van andere berichten. Wanneer een apparaat-thread is voltooid voor de verwerking van een bericht, meldt deze IoT Hub door *voltooien* het bericht. De status van de IoT Hub vervolgens ingesteld op **voltooid**.

Een apparaat kunt ook:

* *Afwijzen* het bericht, waardoor de IoT Hub worden ingesteld op de **Deadlettered** status. Apparaten die verbinding via het protocol MQTT maken afwijzen niet cloud-naar-apparaat-berichten.
* *Afbreken* het bericht, waardoor de IoT-Hub te plaatsen van het bericht weer in de wachtrij met de status ingesteld op **in wachtrij gezet**. Apparaten die verbinding via het protocol MQTT maken kunnen niet afbreken cloud-naar-apparaat-berichten.

Een thread kan een bericht te verwerken zonder dit IoT-Hub te mislukken. In dit geval automatisch de overgang van berichten van de **onzichtbaar** status terug naar de **in wachtrij gezet** status na een *zichtbaarheid (of vergrendelen) time-out*. De standaardwaarde van deze time-out is 1 minuut.

De **max. aantal levering** eigenschap in IoT Hub bepaalt het maximum aantal keren dat een bericht kan worden overgedragen tussen de **in wachtrij gezet** en **onzichtbaar** statussen. Na dit aantal transities IoT Hub stelt u de status van het bericht **Deadlettered**. Op deze manier IoT Hub stelt u de status van een bericht naar **Deadlettered** na de verlooptijd (Zie [Time to live][lnk-ttl]).

De [het verzenden van berichten van de cloud-naar-apparaat met IoT Hub] [ lnk-c2d-tutorial] ziet u hoe cloud-naar-apparaat-berichten vanuit de cloud verzendt en ontvangt op een apparaat.

Een apparaat is doorgaans een cloud-naar-apparaat-bericht voltooid wanneer het verlies van het bericht heeft geen invloed op de toepassingslogica. Bijvoorbeeld, wanneer het apparaat heeft het bericht inhoud lokaal worden gehandhaafd, of is een bewerking uitgevoerd. Het bericht kan ook tijdelijke informatie waarvan verlies niet gevolgen voor de functionaliteit van de toepassing hebben zou uitvoeren. Soms voor langlopende taken worden uitgevoerd kunt u:

* Het bericht cloud-naar-apparaat voltooien na het persistent maken van de taakbeschrijving van de in de lokale opslag.
* Waarschuw de back-end oplossing met een of meer apparaat-naar-cloud-berichten in verschillende stadia van de voortgang van de taak.

## <a name="message-expiration-time-to-live"></a>Verloopdatum voor het bericht (tijd TTL)

Elk bericht dat cloud-naar-apparaat heeft een verlooptijd. Deze tijd is ingesteld door een van:

* De **ExpiryTimeUtc** eigenschap in de service.
* Met behulp van de IoT-Hub *TTL* opgegeven als een eigenschap IoT Hub.

Zie [configuratieopties voor Cloud-naar-apparaat][lnk-c2d-configuration].

Een veelgebruikte manier om te profiteren van de verloopdatum voor het bericht en te voorkomen dat is berichten verzenden naar niet-verbonden apparaten, het instellen van korte tijd levensduur waarden. Deze aanpak bereikt hetzelfde resultaat als de status van het apparaat verbinding onderhouden tijdens efficiënter wordt. Wanneer u een bericht bevestigingen aanvraagt, IoT Hub een melding die apparaten zijn:

* Berichten kunnen ontvangen.
* Niet online zijn of zijn mislukt.

## <a name="message-feedback"></a>Bericht feedback

Wanneer u een cloud-naar-apparaat-bericht verzendt, kunnen de levering van per bericht feedback met betrekking tot de eindstatus van dat bericht serviceaanvraag.

| ACK-eigenschap | Gedrag |
| ------------ | -------- |
| **positief** | Als het cloud-naar-apparaat-bericht bereikt de **voltooid** staat, IoT-Hub genereert een feedbackbericht. |
| **negatieve** | Als het cloud-naar-apparaat-bericht bereikt de **Deadlettered** staat, IoT-Hub genereert een feedbackbericht. |
| **volledige**     | IoT Hub genereert een feedbackbericht in beide gevallen. |

Als **Ack** is **volledige**, en u niet een feedbackbericht ontvangt, betekent dit dat het Feedbackbericht is verlopen. De service kan niet weet wat is er gebeurd met het oorspronkelijke bericht. In de praktijk kan een service Zorg ervoor dat de feedback verwerken kan voordat deze verloopt. De maximale verlooptijd is twee dagen, die tijd om de service blijft nogmaals uit te voeren als er een fout optreedt.

Zoals uitgelegd in [eindpunten][lnk-endpoints], IoT-Hub levert feedback via een gerichte service-eindpunt (**/messages/servicebound/feedback**) als berichten. De semantiek voor het ontvangen van feedback zijn hetzelfde als voor cloud-naar-apparaat-berichten en dezelfde [bericht lifecycle][lnk-lifecycle]. Indien mogelijk batch bericht feedback wordt verwerkt in één bericht met de volgende indeling:

| Eigenschap     | Beschrijving |
| ------------ | ----------- |
| EnqueuedTime | De tijdstempel die aangeeft wanneer het bericht is gemaakt. |
| Gebruikers-id       | `{iot hub name}` |
| ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

De hoofdtekst is een JSON-geserialiseerd matrix met records, elk met de volgende eigenschappen:

| Eigenschap           | Beschrijving |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | De tijdstempel die aangeeft wanneer het resultaat van het bericht is er gebeurd. Bijvoorbeeld, verlopen het apparaat is voltooid of het bericht. |
| OriginalMessageId  | **MessageId** van het cloud-naar-apparaat-bericht waarop deze informatie feedback betrekking heeft. |
| statusCode         | Een vereiste tekenreeks. In de feedbackberichten die gegenereerd worden door de IoT Hub gebruikt. <br/> 'Geslaagd' <br/> 'Verlopen' <br/> 'DeliveryCountExceeded' <br/> 'Geweigerd' <br/> 'Opgeschoond' |
| Beschrijving        | Waarden voor de tekenreeks **StatusCode**. |
| Apparaat-id           | **DeviceId** van het doelapparaat van het cloud-naar-apparaat-bericht waarop deze stukje feedback betrekking heeft. |
| DeviceGenerationId | **DeviceGenerationId** van het doelapparaat van het cloud-naar-apparaat-bericht waarop deze stukje feedback betrekking heeft. |

De service moet opgeven een **MessageId** voor het bericht cloud-naar-apparaat kunnen de feedback correleren met het oorspronkelijke bericht.

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

Elke IoT-hub toont de volgende configuratieopties voor cloud-naar-apparaat messaging:

| Eigenschap                  | Beschrijving | Bereik en standaard |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | Standaard-TTL voor cloud-naar-apparaat-berichten. | Interval ISO_8601 maximaal 2D (minimaal 1 minuut). Standaardwaarde: 1 uur. |
| maxDeliveryCount          | Levering van het maximum aantal voor cloud-naar-apparaat per apparaat wachtrijen. | 1 tot 100. Standaard: 10. |
| feedback.ttlAsIso8601     | Bewaren voor service-gebonden Feedbackberichten. | Interval ISO_8601 maximaal 2D (minimaal 1 minuut). Standaardwaarde: 1 uur. |
| feedback.maxDeliveryCount |Levering van het maximum aantal voor feedbackwachtrij. | 1 tot 100. Standaard: 100. |

Zie voor meer informatie over het instellen van deze configuratieopties [maken IoT hubs][lnk-portal].

## <a name="next-steps"></a>Volgende stappen

Zie voor informatie over de SDK's kunt u cloud-naar-apparaat-berichten ontvangen [Azure IoT SDK's][lnk-sdks].

Als u wilt uitproberen ontvangen van berichten van de cloud-naar-apparaat, Zie de [cloud naar apparaat verzenden] [ lnk-c2d-tutorial] zelfstudie.

[img-lifecycle]: ./media/iot-hub-devguide-messages-c2d/lifecycle.png

[lnk-portal]: iot-hub-create-through-portal.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-ttl]: #message-expiration-time-to-live
[lnk-c2d-configuration]: #cloud-to-device-configuration-options
[lnk-lifecycle]: #message-lifecycle
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
