---
title: Azure IoT Hub-berichtindeling begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - descibes de indeling en de verwachte inhoud van IoT Hub-berichten.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 7c08848698f07d64bbbff429682c18525659f7bf
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286514"
---
# <a name="create-and-read-iot-hub-messages"></a>IoT Hub-berichten maken en lezen

Ter ondersteuning van naadloze interoperabiliteit alle protocollen, definieert de IoT Hub een algemene berichtindeling voor alle apparaat gerichte protocollen. Deze indeling wordt gebruikt voor zowel [apparaat-naar-cloud] [ lnk-d2c] en [cloud-naar-apparaat] [ lnk-c2d] berichten. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Een [IoT Hub-bericht] [ lnk-messaging] bestaat uit:

* Een vooraf bepaald reeks *Systeemeigenschappen* zoals hieronder vermeld.
* Een set *toepassingseigenschappen*. Een woordenlijst met eigenschappen van een verbindingsreeks die de toepassing kunt definiëren en toegang, zonder dat u nodig hebt voor het deserialiseren van de berichttekst. IoT Hub wordt nooit gewijzigd voor deze eigenschappen.
* Een ondoorzichtige binaire hoofdtekst.

Namen van eigenschappen en -waarden mag alleen alfanumerieke ASCII-tekens, plus ```{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}``` wanneer u:  

* Apparaat-naar-cloud-berichten met behulp van het HTTPS-protocol worden verzonden.
* Cloud-naar-apparaat-berichten worden verzonden.

Zie voor meer informatie over het coderen en decoderen van berichten met verschillende protocollen [Azure IoT SDK's][lnk-sdks].

De volgende tabel wordt de lijst met Systeemeigenschappen in IoT Hub-berichten.

| Eigenschap | Beschrijving | Kan de gebruiker worden ingesteld? |
| --- | --- | --- |
| MessageId |Een gebruiker instelbare id voor het bericht dat wordt gebruikt voor aanvraag / antwoord-patronen. Indeling: Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens lang) van ASCII-7-bits alfanumerieke tekens + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. | Ja |
| Volgnummer |Een aantal (uniek per apparaat-wachtrij) door IoT Hub is toegewezen aan elk cloud-naar-apparaat-bericht. | Nee voor C2D-berichten; anders Ja. |
| Handeling |Een doel dat is opgegeven in [Cloud-naar-apparaat] [ lnk-c2d] berichten. | Nee voor C2D-berichten; anders Ja. |
| ExpiryTimeUtc |Datum en tijd van verlopen van berichten. | Ja |
| EnqueuedTime |Datum en tijd waarop de [Cloud-naar-apparaat] [ lnk-c2d] bericht is ontvangen door de IoT Hub. | Nee voor C2D-berichten; anders Ja. |
| CorrelationId |Een string-eigenschap in een antwoordbericht die normaal gesproken de MessageId van de aanvraag, in de aanvraag / antwoord-patronen bestaat. | Ja |
| UserId |Een ID die wordt gebruikt om op te geven van de oorsprong van berichten. Wanneer berichten worden gegenereerd door de IoT Hub, dat is ingesteld op `{iot hub name}`. | Nee |
| ACK |De generator van een feedback-bericht. Deze eigenschap wordt gebruikt in de cloud-naar-apparaat-berichten om aan te vragen van IoT Hub voor het genereren van Feedbackberichten als gevolg van het verbruik van het bericht door het apparaat. Mogelijke waarden: **geen** (standaard): Er is geen Feedbackbericht wordt gegenereerd, **positieve**: een feedbackbericht ontvangen als het bericht is voltooid, **negatieve**: ontvangen een feedback wordt weergegeven als het bericht is verlopen (of maximum aantal bezorgingen is bereikt) zonder wordt voltooid door het apparaat of **volledige**: positieve en negatieve. Zie voor meer informatie, [bericht feedback][lnk-feedback]. | Ja |
| ConnectionDeviceId |Een ID die is ingesteld door IoT Hub apparaat-naar-cloud-berichten. Deze bevat de **deviceId** van het apparaat dat het bericht heeft verzonden. | Nee voor D2C berichten; anders Ja. |
| ConnectionDeviceGenerationId |Een ID die is ingesteld door IoT Hub apparaat-naar-cloud-berichten. Deze bevat de **generationId** (volgens [identiteit apparaateigenschappen][lnk-device-properties]) van het apparaat dat het bericht heeft verzonden. | Nee voor D2C berichten; anders Ja. |
| ConnectionAuthMethod |Een verificatiemethode is ingesteld door de IoT Hub apparaat-naar-cloud-berichten. Deze eigenschap bevat informatie over de verificatiemethode die wordt gebruikt om het verzenden van het bericht apparaat te verifiëren. Zie voor meer informatie, [apparaat naar de cloud anti-adresvervalsing][lnk-antispoofing]. | Nee voor D2C berichten; anders Ja. |
| CreationTimeUtc | De datum en tijd waarop die het bericht is gemaakt op een apparaat. Een apparaat moet deze waarde expliciet ingesteld. | Ja |

## <a name="message-size"></a>Berichtgrootte

IoT Hub meet grootte van het bericht in een protocol platformonafhankelijk, alleen de nettolading van de werkelijke overweegt. De grootte in bytes wordt berekend als de som van de volgende opties:

* De hoofdtekst van de grootte in bytes.
* De grootte in bytes van de waarden van de eigenschappen van het bericht.
* De grootte in bytes van alle namen van de gebruiker eigenschappen en waarden.

Namen van eigenschappen en waarden zijn beperkt tot ASCII-tekens, zodat de lengte van de tekenreeksen die gelijk is aan de grootte in bytes.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over limieten voor de berichtgrootte in IoT Hub [IoT Hub-quota en beperkingen][lnk-quotas].

Zie voor meer informatie over het maken en lezen van IoT Hub-berichten in diverse programmeertalen, de [snelstartgidsen][lnk-get-started].

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: quickstart-send-telemetry-node.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
