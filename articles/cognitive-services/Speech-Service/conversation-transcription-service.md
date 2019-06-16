---
title: Conversatie Transcriptie - spraakservices
titleSuffix: Azure Cognitive Services
description: Conversatie transcriptie is een geavanceerde functie van de spraakservices waarin realtime spraakherkenning, sprekeridentificatie en diarization worden gecombineerd. Conversatie transcriptie is perfect voor te transcriberen vergaderingen, de mogelijkheid om u te onderscheiden van sprekers, dit laat u weten hoe en wanneer, waardoor deelnemers zich kunt richten op de vergadering en snel voor de volgende stappen volgen in deze. Deze functie verbetert tevens de toegankelijkheid. Met transcriptie, kunt u deelnemers met gehoorproblemen actief betrekken.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: a31921e229a4bbfccd6fdd871666aad1eeef3232
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66243850"
---
# <a name="what-is-conversation-transcription"></a>Wat is conversatie transcriptie?

Conversatie transcriptie is een geavanceerde functie van de spraakservices waarin realtime spraakherkenning, sprekeridentificatie en diarization worden gecombineerd. Conversatie transcriptie is perfect voor te transcriberen vergaderingen, de mogelijkheid om u te onderscheiden van sprekers, dit laat u weten hoe en wanneer, waardoor deelnemers zich kunt richten op de vergadering en snel voor de volgende stappen volgen in deze. Deze functie verbetert tevens de toegankelijkheid. Met transcriptie, kunt u deelnemers met gehoorproblemen actief betrekken.   

Conversatie transcriptie biedt nauwkeurige herkenning met aanpasbare spraakmodellen die u aanpassen kunt om te begrijpen van de branche en bedrijfsspecifieke vocabulaire. U kunt bovendien transcriptie conversatie met de SDK van de apparaten spraak voor het optimaliseren van de ervaring voor meerdere microfoon apparaten worden gekoppeld.

>[!NOTE]
> Op dit moment wordt conversatie transcriptie aanbevolen voor kleine vergaderingen. Als u wilt uitbreiden van de conversatie transcriptie voor grote vergaderingen op schaal, neem dan contact met ons.

In dit diagram ziet u de hardware, software en services die met de conversatie transcriptie samen.

![Het importeren van conversatie transcriptie Diagram](media/scenarios/conversation-transcription-service.png)

>[!IMPORTANT]
> Er is een circulaire zeven Microfoonmatrix met specifieke geometrie-configuratie vereist. Zie voor meer informatie-specificatie en -ontwerp, [Microsoft Speech Device SDK microfoon](https://aka.ms/cts/microphone). Zie voor meer informatie of een development kit aanschaffen, [Microsoft Speech apparaat-SDK ophalen](https://aka.ms/cts/getsdk).

## <a name="get-started-with-conversation-transcription"></a>Aan de slag met de conversatie transcriptie

Er zijn drie stappen die u nodig hebt om aan te nemen aan de slag met de conversatie transcriptie.

1. Toon voorbeelden van gebruikers verzamelen.
2. Genereren van gebruikersprofielen met behulp van de gebruiker stem-voorbeelden
3. De spraak-SDK gebruiken om te bepalen van gebruikers (sprekers) en moet transcriberen spraak

## <a name="collect-user-voice-samples"></a>Gebruiker stem voorbeelden verzamelen

De eerste stap is naar geluidsopnamen verzamelen van elke gebruiker. Gebruiker spraak moet worden geregistreerd in een stille omgeving zonder achtergrondgeluiden. De lengte van de aanbevolen voor elk audio voorbeeld ligt tussen 30 seconden en twee minuten. Bij het identificeren van luidsprekers resulteert langer audiovoorbeelden in betere nauwkeurigheid. Audio moet mono kanaal met een samplefrequentie 16 KHz.

Meer dan de hiervoor genoemde richtlijnen, hoe de audio wordt vastgelegd en opgeslagen is aan u--wordt een beveiligde database aanbevolen. In de volgende sectie wordt we bekijken hoe deze audio wordt gebruikt voor het genereren van gebruikersprofielen die worden gebruikt met de spraak-SDK voor het herkennen van luidsprekers.

## <a name="generate-user-profiles"></a>Gebruikersprofielen genereren

Vervolgens moet u voor het verzenden van de audio-opnamen u hebt verzameld voor de Service voor het genereren van handtekeningen voor valideren van de audio en het genereren van gebruikersprofielen. De [Service voor het genereren van handtekeningen](https://aka.ms/cts/signaturegenservice) is een set REST-API's, waarmee u genereren en ophalen van gebruikersprofielen.

Voor het maken van een gebruikersprofiel, moet u gebruiken de `GenerateVoiceSignature` API. Details van de specificatie en voorbeeldcode zijn beschikbaar:

> [!NOTE]
> Conversatie transcriptie is momenteel beschikbaar in 'en-US' en 'zh-CN' in de volgende regio's: `centralus` en `eastasia`.

* [REST-specificatie](https://aka.ms/cts/signaturegenservice)
* [Het gebruik van de conversatie transcriptie](https://aka.ms/cts/howto)

## <a name="transcribe-and-identify-speakers"></a>Transcriberen en identificeren van luidsprekers

Conversatie transcriptie wordt verwacht dat meerdere kanalen audiostreams en gebruikersprofielen als invoer voor het genereren van transcripties en sprekers te identificeren. Audio- en profielgegevens worden verzonden naar de conversatie transcriptie service met behulp van de SDK van de apparaten spraak. Zoals eerder gezegd, een circulaire zeven Microfoonmatrix en de SDK van de apparaten spraak overstappen naar conversatie transcriptie.

>[!NOTE]
> Zie voor meer informatie-specificatie en -ontwerp, [Microsoft Speech Device SDK microfoon](https://aka.ms/cts/microphone). Zie voor meer informatie of een development kit aanschaffen, [Microsoft Speech apparaat-SDK ophalen](https://aka.ms/cts/getsdk).

Zie voor meer informatie over het gebruik van de conversatie transcriptie met de SDK van de apparaten spraak, [over het gebruik van de conversatie transcriptie](https://aka.ms/cts/howto).


## <a name="quick-start-with-a-sample-app"></a>Snel starten met een voorbeeld-app

Microsoft Speech Device SDK heeft een snel aan de slag-voorbeeld-app voor alle apparaten die betrekking hebben voorbeelden. Conversatie transcriptie is een van beide. U vindt deze in [android Quick Start voor een apparaat-SDK voor spraak](https://aka.ms/sdsdk-quickstart) met voorbeeld-app en de broncode ter referentie.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over de SDK van de apparaten spraak](speech-devices-sdk.md)
