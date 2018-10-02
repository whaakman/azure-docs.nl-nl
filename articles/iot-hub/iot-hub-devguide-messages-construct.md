---
title: Azure IoT Hub-berichtindeling begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - descibes de indeling en de verwachte inhoud van IoT Hub-berichten.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: 168f78ed2236d70a37a111c628e45e7bf0bbfa4b
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585404"
---
# <a name="create-and-read-iot-hub-messages"></a>IoT Hub-berichten maken en lezen

Ter ondersteuning van naadloze interoperabiliteit alle protocollen, definieert de IoT Hub een algemene berichtindeling voor alle apparaat gerichte protocollen. Deze indeling wordt gebruikt voor zowel [routering van apparaat-naar-cloud](iot-hub-devguide-messages-d2c.md) en [cloud-naar-apparaat](iot-hub-devguide-messages-c2d.md) berichten. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub implementeert het apparaat-naar-cloud-berichten met behulp van een streaming messaging-patroon. De IoT Hub apparaat-naar-cloud-berichten lijken meer op [Event Hubs](/azure/event-hubs/) *gebeurtenissen* dan [Service Bus](/azure/service-bus-messaging/) *berichten* in dat er een hoog volume is gebeurtenissen die zijn doorgegeven via de service die kan worden gelezen door meerdere lezers.

Een IoT Hub-bericht bestaat uit:

* Een vooraf bepaald reeks *Systeemeigenschappen* zoals hieronder vermeld.

* Een set *toepassingseigenschappen*. Een woordenlijst met eigenschappen van een verbindingsreeks die de toepassing kunt definiëren en toegang, zonder dat u nodig hebt voor het deserialiseren van de berichttekst. IoT Hub wordt nooit gewijzigd voor deze eigenschappen.

* Een ondoorzichtige binaire hoofdtekst.

Namen van eigenschappen en -waarden mag alleen alfanumerieke ASCII-tekens, plus ```{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}``` wanneer het verzenden van apparaat-naar-cloud-berichten met behulp van de HTTPS-protocol of cloud-naar-apparaat-berichten te verzenden.

Apparaat-naar-cloud-berichten met IoT Hub heeft de volgende kenmerken:

* Apparaat-naar-cloud-berichten zijn duurzame en kunnen worden bewaard in een IoT-hub standaard **berichten/gebeurtenissen** -eindpunt voor maximaal zeven dagen.

* Apparaat-naar-cloud-berichten mag maximaal 256 KB, en kunnen worden gegroepeerd in batches verzendt optimaliseren. Batches mag maximaal 256 KB.

* IoT Hub is niet toegestaan voor het partitioneren van willekeurige. Apparaat-naar-cloud-berichten worden gepartitioneerd op basis van hun oorspronkelijke **deviceId**.

* Zoals uitgelegd in [beheren van toegang tot IoT Hub](iot-hub-devguide-security.md), IoT Hub kunt per apparaat verificatie en toegangsbeheer.

Zie voor meer informatie over het coderen en decoderen van berichten met verschillende protocollen [Azure IoT SDK's](iot-hub-devguide-sdks.md).

De volgende tabel wordt de lijst met Systeemeigenschappen in IoT Hub-berichten.

| Eigenschap | Beschrijving | Kan de gebruiker worden ingesteld? |
| --- | --- | --- |
| MessageId |Een gebruiker instelbare id voor het bericht dat wordt gebruikt voor aanvraag / antwoord-patronen. Indeling: Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens lang) van ASCII-7-bits alfanumerieke tekens + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. | Ja |
| Volgnummer |Een aantal (uniek per apparaat-wachtrij) door IoT Hub is toegewezen aan elk cloud-naar-apparaat-bericht. | Nee voor C2D-berichten; anders Ja. |
| Handeling |Een doel dat is opgegeven in [Cloud-naar-apparaat](iot-hub-devguide-c2d-guidance.md) berichten. | Nee voor C2D-berichten; anders Ja. |
| ExpiryTimeUtc |Datum en tijd van verlopen van berichten. | Ja |
| EnqueuedTime |Datum en tijd waarop de [Cloud-naar-apparaat](iot-hub-devguide-c2d-guidance.md) bericht is ontvangen door de IoT Hub. | Nee voor C2D-berichten; anders Ja. |
| CorrelationId |Een string-eigenschap in een antwoordbericht die normaal gesproken de MessageId van de aanvraag, in de aanvraag / antwoord-patronen bestaat. | Ja |
| UserId |Een ID die wordt gebruikt om op te geven van de oorsprong van berichten. Wanneer berichten worden gegenereerd door de IoT Hub, dat is ingesteld op `{iot hub name}`. | Nee |
| ACK |De generator van een feedback-bericht. Deze eigenschap wordt gebruikt in de cloud-naar-apparaat-berichten om aan te vragen van IoT Hub voor het genereren van Feedbackberichten als gevolg van het verbruik van het bericht door het apparaat. Mogelijke waarden: **geen** (standaard): Er is geen Feedbackbericht wordt gegenereerd, **positieve**: een feedbackbericht ontvangen als het bericht is voltooid, **negatieve**: ontvangen een feedback wordt weergegeven als het bericht is verlopen (of maximum aantal bezorgingen is bereikt) zonder wordt voltooid door het apparaat of **volledige**: positieve en negatieve. 
<!-- robinsh For more information, see [Message feedback][lnk-feedback].--> | Ja |
| ConnectionDeviceId |Een ID die is ingesteld door IoT Hub apparaat-naar-cloud-berichten. Deze bevat de **deviceId** van het apparaat dat het bericht heeft verzonden. | Nee voor D2C berichten; anders Ja. |
| ConnectionDeviceGenerationId |Een ID die is ingesteld door IoT Hub apparaat-naar-cloud-berichten. Deze bevat de **generationId** (volgens [identiteit apparaateigenschappen](iot-hub-devguide-identity-registry.md#device-identity-properties)) van het apparaat dat het bericht heeft verzonden. | Nee voor D2C berichten; anders Ja. |
| ConnectionAuthMethod |Een verificatiemethode is ingesteld door de IoT Hub apparaat-naar-cloud-berichten. Deze eigenschap bevat informatie over de verificatiemethode die wordt gebruikt om het verzenden van het bericht apparaat te verifiëren. <!-- ROBINSH For more information, see [Device to cloud anti-spoofing][lnk-antispoofing].--> | Nee voor D2C berichten; anders Ja. |
| CreationTimeUtc | De datum en tijd waarop die het bericht is gemaakt op een apparaat. Een apparaat moet deze waarde expliciet ingesteld. | Ja |

## <a name="message-size"></a>Berichtgrootte

IoT Hub meet grootte van het bericht in een protocol platformonafhankelijk, alleen de nettolading van de werkelijke overweegt. De grootte in bytes wordt berekend als de som van de volgende opties:

* De hoofdtekst van de grootte in bytes.
* De grootte in bytes van de waarden van de eigenschappen van het bericht.
* De grootte in bytes van alle namen van de gebruiker eigenschappen en waarden.

Namen van eigenschappen en waarden zijn beperkt tot ASCII-tekens, zodat de lengte van de tekenreeksen die gelijk is aan de grootte in bytes.

## <a name="anti-spoofing-properties"></a>Eigenschappen van anti-adresvervalsing (spoofing)

Om te voorkomen dat apparaat adresvervalsing (spoofing) in apparaat-naar-cloud-berichten, IoT-Hub stempels alle berichten met de volgende eigenschappen:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

De eerste twee bevatten de **deviceId** en **generationId** van het oorspronkelijke apparaat volgens [identiteit apparaateigenschappen](iot-hub-devguide-identity-registry.md#device-identity-properties).

De **ConnectionAuthMethod** eigenschap bevat een JSON-geserialiseerd object met de volgende eigenschappen:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over limieten voor de berichtgrootte in IoT Hub [IoT Hub-quota en beperkingen](iot-hub-devguide-quotas-throttling.md).

* Zie voor meer informatie over het maken en lezen van IoT Hub-berichten in diverse programmeertalen, de [snelstartgidsen](quickstart-send-telemetry-node.md).