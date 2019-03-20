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
ms.openlocfilehash: 05028704c08ebd06f9b9e4e3f45c5137eb1e6b58
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226909"
---
# <a name="what-is-text-to-speech"></a>Wat is voor tekst naar spraak?

Tekst naar spraak vanuit Azure Speech Services is een op REST gebaseerde service waarmee uw toepassingen, hulpprogramma's of apparaten naar tekst converteren naar natuurlijke menselijke kunstmatige spraak. Kies in de standaard- en neurale stemmen of maak uw eigen [aangepaste gesproken](#custom-voice-fonts) uniek is voor uw product of een merk. 75 + standard stemmen zijn beschikbaar in meer dan 45 talen en landinstellingen en 5 neurale stemmen zijn beschikbaar in 4 talen of landinstellingen. Zie voor een volledige lijst [ondersteunde talen](language-support.md#text-to-speech).

Text to Speech-technologie kunnen makers van inhoud om te communiceren met hun gebruikers op verschillende manieren. Text to Speech kan toegankelijkheid verbeteren door gebruikers met de mogelijkheid om te communiceren met inhoud hoorbaar. Of de gebruiker een gezichtsvermogen, een achterstand learning heeft of navigatie-gegevens en tot vereist, kunnen de Text to Speech een bestaande ervaring kan verbeteren. Tekst naar spraak is ook een waardevolle-invoegtoepassing voor spraak bots en virtual Assistant.

### <a name="neural-voices"></a>Neurale stemmen

Neurale stemmen kunnen worden gebruikt voor interactie met chatbots en virtuele assistenten natuurlijker maken en aantrekkelijke digitale teksten, zoals e-books converteren naar audiobooks en in de auto-navigatiesystemen te verbeteren. Met de menselijke natuurlijke prosody en duidelijke afbakening van woorden verkorten Neural stemmen aanzienlijk luisterende intensief gebruik wanneer u met AI-systemen communiceren. Zie voor meer informatie over neurale stemmen [ondersteunde talen](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Aangepaste stemmen

Stem aanpassingen kunt u een herkenbare, één van een unieke stem voor uw merk maken. Voor het maken van uw aangepaste spraakstijl, moet u een studio-opname maken en uploaden van de bijbehorende scripts als de trainingsgegevens. De service maakt vervolgens een unieke stem-model dat is afgestemd op de opname. U kunt deze aangepaste spraakstijl gebruiken om na te bootsen spraak. Zie voor meer informatie, [aangepaste stemmen](how-to-customize-voice-font.md).

## <a name="core-features"></a>Belangrijkste functies

Deze tabel bevat de kernfuncties voor tekst naar spraak:

| Toepassing | SDK | REST |
|----------|-----|------|
| Converteert tekst naar spraak. | Nee | Ja |
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

| Snelstartgids | Platform | API-verwijzing |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Bladeren](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Window, macOS, Linux | [Bladeren](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api) |
| [Python](quickstart-python-text-to-speech.md) | Window, macOS, Linux | [Bladeren](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api) |

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeldcode voor tekst naar spraak is beschikbaar op GitHub. Deze voorbeelden omvatten tekst naar spraak converteren in de populairste programmeertalen.

* [Text to Speech-voorbeelden (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Referentiedocumenten

* [Speech-SDK](speech-sdk-reference.md)
* [Spraak apparaten SDK](speech-devices-sdk.md)
* [REST API: Spraak-naar-tekst](rest-speech-to-text.md)
* [REST API: Text-to-speech](rest-text-to-speech.md)
* [REST API: Batch transcriptie en aanpassen](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Volgende stappen

* [Neem een abonnement op spraak-gratis Services](get-started.md)
* [Aangepaste spraakstijlen maken](how-to-customize-voice-font.md)
