---
title: Gesprek transcriptie-Speech Service
titleSuffix: Azure Cognitive Services
description: Conversation transcriptie is een geavanceerde functie van de spraak services die realtime spraak herkenning, luidspreker identificatie en diarization combi neren. Conversation transcriptie is perfect voor het overzetten van persoonlijke vergaderingen, met de mogelijkheid om de luid sprekers te onderscheiden, waarmee u kunt zien wie wat en wanneer, hoe deel nemers zich kunnen richten op de vergadering en snel op de volgende stappen moeten volgen. Deze functie verbetert ook de toegankelijkheid. Met transcriptie kunt u deel nemers actief houden met gehoor problemen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: acbf82335523ee226f4b899180f0f22cb94cca5e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562886"
---
# <a name="what-is-conversation-transcription"></a>Wat is een conversatie transcriptie?

Conversation transcriptie is een geavanceerde functie van de spraak services die realtime spraak herkenning, luidspreker identificatie en diarization combi neren. Conversation transcriptie is perfect voor het overzetten van persoonlijke vergaderingen, met de mogelijkheid om de luid sprekers te onderscheiden, waarmee u kunt zien wie wat en wanneer, hoe deel nemers zich kunnen richten op de vergadering en snel op de volgende stappen moeten volgen. Deze functie verbetert ook de toegankelijkheid. Met transcriptie kunt u deel nemers actief houden met gehoor problemen.   

Conversation transcriptie levert nauw keurige herkenning met aanpas bare spraak modellen die u kunt aanpassen om inzicht te krijgen in de industrie en bedrijfsspecifieke woorden lijst. Daarnaast kunt u gespreks transcriptie koppelen aan de speech-apparaten SDK om de ervaring voor apparaten met meerdere microfoons te optimaliseren.

>[!NOTE]
> Conversatie transcriptie wordt momenteel aanbevolen voor kleine vergaderingen. Neem contact met ons op als u de conversatie transcriptie voor grote vergaderingen op schaal wilt verlengen.

Dit diagram illustreert de hardware, software en services die samen werken met de conversatie-transcriptie.

![Het diagram gesprek transcriptie importeren](media/scenarios/conversation-transcription-service.png)

>[!IMPORTANT]
> Er is een circulaire zeven microfoon matrix met een specifieke geometrie configuratie vereist. Zie de [micro soft speech Device SDK-microfoon](https://aka.ms/cts/microphone)voor informatie over specificaties en ontwerpen. Raadpleeg voor meer informatie of een Development Kit aanschaffen [micro soft speech Device SDK](https://aka.ms/cts/getsdk)(Engelstalig).

## <a name="get-started-with-conversation-transcription"></a>Aan de slag met een gesprek transcriptie

Er zijn drie stappen die u moet uitvoeren om aan de slag te gaan met Conversation transcriptie.

1. Verzamel spraak voorbeelden van gebruikers.
2. Gebruikers profielen genereren met behulp van de voor beelden van de gebruikers spraak
3. De Speech SDK gebruiken om gebruikers (sprekers) te identificeren en spraak te transcriberen

## <a name="collect-user-voice-samples"></a>Voor beelden van gebruikers spraak verzamelen

De eerste stap is het verzamelen van audio-opnames van elke gebruiker. Gebruikers spraak moet worden vastgelegd in een stille omgeving zonder achtergrond geluid. De aanbevolen lengte voor elk audio sample ligt tussen 30 seconden en twee minuten. Meer geluids voorbeelden leiden tot betere nauw keurigheid bij het identificeren van luid sprekers. Audio moet mono-kanaal zijn met een sample frequentie van 16 KHz.

Naast de voor noemde richt lijnen, hoe audio wordt vastgelegd en opgeslagen, is een beveiligde data base aanbevolen. In de volgende sectie wordt beschreven hoe deze audio wordt gebruikt voor het genereren van gebruikers profielen die worden gebruikt met de spraak-SDK om luid sprekers te herkennen.

## <a name="generate-user-profiles"></a>Gebruikers profielen genereren

Vervolgens moet u de audio-opnames die u hebt verzameld, naar de service Signature generate verzenden om de audio te valideren en gebruikers profielen te genereren. De [service voor het genereren van hand tekeningen](https://aka.ms/cts/signaturegenservice) is een set rest-api's waarmee u gebruikers profielen kunt genereren en ophalen.

Als u een gebruikers profiel wilt maken, moet u de `GenerateVoiceSignature` API gebruiken. Specificatie details en voorbeeld code zijn beschikbaar:

> [!NOTE]
> De conversatie transcriptie is momenteel beschikbaar in de volgende regio's: `centralus` en `eastasia`-US en "zh-cn".

* [REST-specificatie](https://aka.ms/cts/signaturegenservice)
* [Hoe kan ik gesprek transcriptie gebruiken?](https://aka.ms/cts/howto)

## <a name="transcribe-and-identify-speakers"></a>Luid sprekers transcriberen en identificeren

Conversatie transcriptie verwacht meerkanaalse audio stromen en gebruikers profielen als invoer voor het genereren van transcripties en het identificeren van luid sprekers. Audio-en gebruikers profiel gegevens worden verzonden naar de conversatie transcriptie-service met behulp van de speech-apparaten SDK. Zoals eerder vermeld, zijn er een circulaire zeven microfoon matrix en de speech apparaten SDK vereist voor het gebruik van conversatie transcriptie.

>[!NOTE]
> Zie de [micro soft speech Device SDK-microfoon](https://aka.ms/cts/microphone)voor informatie over specificaties en ontwerpen. Raadpleeg voor meer informatie of een Development Kit aanschaffen [micro soft speech Device SDK](https://aka.ms/cts/getsdk)(Engelstalig).

Zie [conversatie-transcriptie gebruiken](https://aka.ms/cts/howto)voor meer informatie over het gebruik van conversatie transcriptie met de speech-apparaten SDK.


## <a name="quick-start-with-a-sample-app"></a>Quick Start met een voor beeld-app

De SDK voor spraak apparaten van micro soft heeft een snel starten-voor beeld van een app voor alle aan apparaten gerelateerde voor beelden. Conversation transcriptie is een van beide. U kunt dit vinden in de Quick start voor [spraak apparaten SDK](https://aka.ms/sdsdk-quickstart) met voor beeld-apps en de bijbehorende bron code voor uw referentie.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over de SDK voor spraak apparaten](speech-devices-sdk.md)
