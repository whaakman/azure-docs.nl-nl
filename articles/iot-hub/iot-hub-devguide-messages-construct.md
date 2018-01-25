---
title: Azure IoT Hub-berichtindeling begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - descibes de indeling en de verwachte inhoud van IoT Hub berichten.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: c57ceb83951341d4a7bf368e209ba0f88825672c
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="create-and-read-iot-hub-messages"></a>Maken en IoT Hub berichten lezen

IoT Hub definieert ter ondersteuning naadloze interoperabiliteit van alle protocollen, een gemeenschappelijke berichtindeling voor alle apparaten gerichte protocollen. Deze berichtindeling wordt gebruikt voor zowel [apparaat-naar-cloud] [ lnk-d2c] en [cloud-naar-apparaat] [ lnk-c2d] berichten. Een [IoT Hub bericht] [ lnk-messaging] bestaat uit:

* Een set *Systeemeigenschappen*. Eigenschappen die IoT Hub worden geïnterpreteerd of ingesteld. Deze verzameling is een vooraf vastgesteld.
* Een set *toepassingseigenschappen*. Een woordenlijst van eigenschappen van een verbindingsreeks die de toepassing kunt definiëren en toegang zonder deserialiseren van de berichttekst. IoT Hub wordt nooit wijzigt deze eigenschappen.
* Een ondoorzichtige binaire instantie.

Namen en waarden mag alleen ASCII-alfanumerieke tekens, plus ```{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}``` wanneer u:  

* Apparaat-naar-cloud-berichten verzenden met het HTTPS-protocol.
* Cloud-naar-apparaat-berichten verzenden.

Zie voor meer informatie over het coderen en decoderen van berichten die worden verzonden met behulp van verschillende protocollen [Azure IoT SDK's][lnk-sdks].

De volgende tabel bevat de reeks Systeemeigenschappen in IoT Hub berichten.

| Eigenschap | Beschrijving |
| --- | --- |
| MessageId |Een gebruiker worden ingesteld-id voor het bericht dat wordt gebruikt voor aanvragen / antwoorden patronen. Indeling: Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens lang) van ASCII-7-bits alfanumerieke tekens + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Volgnummer |Een getal (uniek per apparaat wachtrij) door de IoT Hub is toegewezen aan elk bericht cloud-naar-apparaat. |
| Handeling |Een doel dat is opgegeven in [Cloud-naar-apparaat] [ lnk-c2d] berichten. |
| ExpiryTimeUtc |Datum en tijd van de verloopdatum voor het bericht. |
| EnqueuedTime |Datum en tijd waarop de [Cloud-naar-apparaat] [ lnk-c2d] bericht is ontvangen door de IoT Hub. |
| CorrelationId |Een tekenreekseigenschap in een antwoordbericht die normaal gesproken de MessageId van de aanvraag, in aanvragen / antwoorden patronen bestaat. |
| UserId |Een ID die wordt gebruikt voor het opgeven van de oorsprong van berichten. Wanneer berichten worden gegenereerd door de IoT Hub, is ingesteld op `{iot hub name}`. |
| ACK |Een bericht generator van feedback. Deze eigenschap wordt gebruikt in de cloud-naar-apparaat-berichten aan te vragen van IoT Hub Feedbackberichten als gevolg van het verbruik van het bericht te genereren door het apparaat. Mogelijke waarden: **geen** (standaard): geen Feedbackbericht wordt gegenereerd, **positieve**: ontvangen een feedbackbericht als het bericht is voltooid, **negatieve**: ontvangen een feedbackbericht als het bericht is verlopen (of levering van het maximum aantal is bereikt) zonder wordt voltooid door het apparaat of **volledige**: positieve en negatieve. Zie voor meer informatie [bericht feedback][lnk-feedback]. |
| ConnectionDeviceId |Een ID die is ingesteld door de IoT Hub apparaat-naar-cloud-berichten. Bevat de **deviceId** van het apparaat dat het bericht heeft verzonden. |
| ConnectionDeviceGenerationId |Een ID die is ingesteld door de IoT Hub apparaat-naar-cloud-berichten. Bevat de **generationId** (conform [identiteit apparaateigenschappen][lnk-device-properties]) van het apparaat dat het bericht heeft verzonden. |
| ConnectionAuthMethod |Een verificatiemethode is ingesteld door de IoT Hub apparaat-naar-cloud-berichten. Deze eigenschap bevat informatie over de verificatiemethode die wordt gebruikt voor het verifiëren van het apparaat te verzenden. Zie voor meer informatie [apparaat naar de cloud anti-adresvervalsing][lnk-antispoofing]. |

## <a name="message-size"></a>Berichtgrootte

IoT Hub meet grootte van het bericht op een manier protocol networkdirect alleen de nettolading van de werkelijke overwegen. De grootte in bytes wordt berekend als de som van de volgende opties:

* De hoofdtekst-grootte in bytes.
* De grootte in bytes van de waarden van de eigenschappen van het bericht.
* De grootte in bytes van alle gebruikersnamen en waarden.

Namen en waarden zijn beperkt tot ASCII-tekens, zodat de lengte van de tekenreeksen die gelijk is aan de grootte in bytes.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over limieten voor de berichtgrootte van IoT-Hub [IoT Hub quota's en beperking][lnk-quotas].

Zie voor meer informatie over het maken en IoT Hub berichten in diverse programmeertalen, de [aan de slag] [ lnk-get-started] zelfstudies.

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
