---
title: Tekst naar spraak met Azure Speech Services
titleSuffix: Azure Cognitive Services
description: Tekst naar spraak vanuit Azure Speech Services is een op REST gebaseerde service waarmee uw toepassingen, hulpprogramma's of apparaten naar tekst converteren naar natuurlijke menselijke kunstmatige spraak. Kiezen uit de standard- en neurale stemmen of maak uw eigen aangepaste gesproken uniek is voor uw product of een merk. 75 + standard stemmen zijn beschikbaar in meer dan 45 talen en landinstellingen en 5 neurale stemmen zijn beschikbaar in 4 talen of landinstellingen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 52f74bb3cb9e460fc5c572079355f47b4b0bf0a3
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "59010444"
---
# <a name="what-is-text-to-speech"></a>Wat is voor tekst naar spraak?

Tekst naar spraak vanuit Azure Speech Services is een op REST gebaseerde service waarmee uw toepassingen, hulpprogramma's of apparaten naar tekst converteren naar natuurlijke menselijke kunstmatige spraak. Kiezen uit de standard- en neurale stemmen of maak uw eigen aangepaste gesproken uniek is voor uw product of een merk. 75 + standard stemmen zijn beschikbaar in meer dan 45 talen en landinstellingen en 5 neurale stemmen zijn beschikbaar in 4 talen of landinstellingen. Zie voor een volledige lijst [ondersteunde talen](language-support.md#text-to-speech).

Text to Speech-technologie kunnen makers van inhoud om te communiceren met hun gebruikers op verschillende manieren. Text to Speech kan toegankelijkheid verbeteren door gebruikers met de mogelijkheid om te communiceren met inhoud hoorbaar. Of de gebruiker een gezichtsvermogen, een achterstand learning heeft of navigatie-gegevens en tot vereist, kunnen de Text to Speech een bestaande ervaring kan verbeteren. Tekst naar spraak is ook een waardevolle-invoegtoepassing voor spraak bots en virtual Assistant.

### <a name="neural-voices"></a>Neurale stemmen

Neurale stemmen kunnen worden gebruikt voor interactie met chatbots en virtuele assistenten natuurlijker maken en aantrekkelijke digitale teksten, zoals e-books converteren naar audiobooks en in de auto-navigatiesystemen te verbeteren. Met de menselijke natuurlijke prosody en duidelijke afbakening van woorden verkorten Neural stemmen aanzienlijk luisterende intensief gebruik wanneer u met AI-systemen communiceren. Zie voor meer informatie over neurale stemmen [ondersteunde talen](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Aangepaste stemmen

Stem aanpassingen kunt u een herkenbare, één van een unieke stem voor uw merk maken. Voor het maken van uw aangepaste spraakstijl, moet u een studio-opname maken en uploaden van de bijbehorende scripts als de trainingsgegevens. De service maakt vervolgens een unieke stem-model dat is afgestemd op de opname. U kunt deze aangepaste spraakstijl gebruiken om na te bootsen spraak. Zie voor meer informatie, [aangepaste stemmen](how-to-customize-voice-font.md).

## <a name="core-features"></a>Belangrijkste functies

Deze tabel bevat de kernfuncties voor tekst naar spraak:

| Toepassing | SDK | REST |
|----------|-----|------|
| Converteert tekst naar spraak. | Ja | Ja |
| Gegevenssets voor de toon aanpassing uploaden. | Nee | Ja\* |
| Maken en beheren van spraak lettertype modellen. | Nee | Ja\* |
| Maken en beheren van implementaties van spraak lettertype. | Nee | Ja\* |
| Maken en beheren van spraak lettertype tests. | Nee | Ja\* |
| Abonnementen beheren. | Nee | Ja\* |

\* *Deze services zijn beschikbaar met behulp van het eindpunt cris.ai. Zie [Swagger verwijzing](https://westus.cris.ai/swagger/ui/index).*

> [!NOTE]
> Het eindpunt van de Text to Speech implementeert beperking waarmee aanvragen voor 25 per vijf seconden wordt beperkt. Wanneer bandbreedtebeperking optreedt, moet u een melding via berichtkoppen.

## <a name="get-started-with-text-to-speech"></a>Aan de slag met tekst naar spraak

We bieden snelstartgidsen ontworpen dat u de uitvoering van code in minder dan 10 minuten. Deze tabel bevat een lijst met tekst naar spraak snelstartgidsen ingedeeld per taal.

| Quickstart (REST) | Platform | API-verwijzing |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Bladeren](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Window, macOS, Linux | [Bladeren](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Window, macOS, Linux | [Bladeren](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

| Quickstart (SDK) | Platform | API-verwijzing |
|------------|----------|---------------|
| [C#, .NET Framework](quickstart-text-to-speech-dotnet-windows.md) | Windows | [Bladeren](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-text-to-speech-cpp-windows.md) | Windows | [Bladeren](https://aka.ms/csspeech/cppref) |
| [C++](quickstart-text-to-speech-cpp-linux.md) | Linux | [Bladeren](https://aka.ms/csspeech/cppref) |

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeldcode voor tekst naar spraak is beschikbaar op GitHub. Deze voorbeelden omvatten tekst naar spraak converteren in de populairste programmeertalen.

* [Text to Speech-voorbeelden (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)
* [Text to Speech-voorbeelden (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="reference-docs"></a>Referentiedocumenten

* [Speech-SDK](speech-sdk-reference.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [REST-API: Spraak-naar-tekst](rest-speech-to-text.md)
* [REST-API: Tekst naar spraak](rest-text-to-speech.md)
* [REST-API: Batch transcriptie en aanpassen](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Volgende stappen

* [Neem een abonnement op spraak-gratis Services](get-started.md)
* [Aangepaste spraakstijlen maken](how-to-customize-voice-font.md)
