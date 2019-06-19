---
title: Tekst naar spraak met Azure Speech Services
titleSuffix: Azure Cognitive Services
description: Tekst naar spraak vanuit Azure Speech Services is een service waarmee u uw toepassingen, hulpprogramma's of apparaten naar tekst converteren naar natuurlijke menselijke kunstmatige spraak. Kiezen uit de standard- en neurale stemmen of maak uw eigen aangepaste gesproken uniek is voor uw product of een merk. 75 + standard stemmen zijn beschikbaar in meer dan 45 talen en landinstellingen en 5 neurale stemmen zijn beschikbaar in 4 talen of landinstellingen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: erhopf
ms.openlocfilehash: a3469e4f7fe1c234a6df694d7bdd6d9e2c46407a
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204893"
---
# <a name="what-is-text-to-speech"></a>Wat is voor tekst naar spraak?

Tekst naar spraak vanuit Azure Speech Services is een service waarmee u uw toepassingen, hulpprogramma's of apparaten naar tekst converteren naar natuurlijke menselijke kunstmatige spraak. Kiezen uit de standard- en neurale stemmen of maak uw eigen aangepaste gesproken uniek is voor uw product of een merk. 75 + standard stemmen zijn beschikbaar in meer dan 45 talen en landinstellingen en 5 neurale stemmen zijn beschikbaar in 4 talen of landinstellingen. Zie voor een volledige lijst [ondersteunde talen](language-support.md#text-to-speech).

Text to Speech-technologie kunnen makers van inhoud om te communiceren met hun gebruikers op verschillende manieren. Text to Speech kan toegankelijkheid verbeteren door gebruikers met de mogelijkheid om te communiceren met inhoud hoorbaar. Of de gebruiker een gezichtsvermogen, een achterstand learning heeft of navigatie-gegevens en tot vereist, kunnen de Text to Speech een bestaande ervaring kan verbeteren. Tekst naar spraak is ook een waardevolle-invoegtoepassing voor spraak bots en virtual Assistant.

### <a name="standard-voices"></a>Standard stemmen

Standard stemmen worden gemaakt met behulp van statistische parametrische synthese en/of samenvoeging synthese technieken. Deze stemmen zijn zeer begrijpelijke en geluid natuurlijke. U kunt eenvoudig uw toepassingen te spreken in meer dan 45 talen, met een breed scala aan Spraakopties voor inschakelen. Deze stemmen bieden hoge uitspraak van nauwkeurigheid, inclusief ondersteuning voor afkortingen, acroniem uitbreidingen, datum/tijd interpretaties, polyphones en meer. Standard stem gebruiken voor het verbeteren van toegankelijkheid voor uw toepassingen en services doordat gebruikers kunnen communiceren met de inhoud van uw hoorbaar.

### <a name="neural-voices"></a>Neurale stemmen

Stemmen neurale deep neural networks te strijden tegen de beperkingen van traditionele Text to Speech systemen in die overeenkomen met de patronen van stress en intonation in gesproken taal en in de stof de eenheden van spraak in de stem van een computer voor de synthese gebruiken Standard tekst naar spraak een uitsplitsing van prosody in afzonderlijke linguïstische analyse- en akoestische voorspelling stappen die onafhankelijk modellen, dit in spellen gesproken synthese resulteren kunnen gelden. Onze mogelijkheid voor neurale biedt prosody voorspelling en toon synthese tegelijkertijd, wat resulteert in een meer vloeiende en natuurlijk klinkende spraak.

Neurale stemmen kunnen worden gebruikt voor interactie met chatbots en virtuele assistenten natuurlijker maken en aantrekkelijke digitale teksten, zoals e-books converteren naar audiobooks en in de auto-navigatiesystemen te verbeteren. Met de menselijke natuurlijke prosody en duidelijke afbakening van woorden verkorten neural stemmen aanzienlijk luisterende intensief gebruik wanneer u met AI-systemen communiceren.

Neurale stemmen ondersteuning voor verschillende stijlen, zoals neutraal en vrolijke. De stem Jessa (en-US) kan bijvoorbeeld cheerfully, die is geoptimaliseerd voor warme, blij conversatie spreken. U kunt de uitvoer van de gesproken zoals toon, verkopen, aanpassen en versnellen met behulp van [spraak synthese Markup Language](speech-synthesis-markup.md). Zie voor een volledige lijst van beschikbare stemmen, [ondersteunde talen](language-support.md#text-to-speech).

Zie voor meer informatie over de voordelen van neurale stemmen, [van Microsoft nieuwe neurale Text to Speech service kunt machines spreken zoals mensen](https://azure.microsoft.com/blog/microsoft-s-new-neural-text-to-speech-service-helps-machines-speak-like-people/).

### <a name="custom-voices"></a>Aangepaste stemmen

Stem aanpassingen kunt u een herkenbare, één van een unieke stem voor uw merk maken. Voor het maken van uw aangepaste spraakstijl, moet u een studio-opname maken en uploaden van de bijbehorende scripts als de trainingsgegevens. De service maakt vervolgens een unieke stem-model dat is afgestemd op de opname. U kunt deze aangepaste spraakstijl gebruiken om na te bootsen spraak. Zie voor meer informatie, [aangepaste stemmen](how-to-customize-voice-font.md).

## <a name="speech-synthesis-markup-language-ssml"></a>Speech Synthesis Markup Language (SSML)

Spraak synthese Markup Language (SSML) is een op XML gebaseerde opmaakcodetaal waarmee ontwikkelaars opgeven hoe invoertekst wordt omgezet in kunstmatige spraak met behulp van de Text to Speech-service. Vergeleken met de tekst zonder opmaak, kan SSML ontwikkelaars voor het afstemmen van de presentatie, uitspraak, spreken snelheid, volume en meer van de Text to Speech-uitvoer. Normale interpunctie, zoals na een periode onderbreken of met behulp van de juiste intonation wanneer een zin met een vraagteken eindigt worden automatisch verwerkt.

Alle tekst-invoer voor verzonden naar de Text to Speech-service moeten worden gestructureerd als SSML. Zie voor meer informatie, [spraak synthese Markup Language](speech-synthesis-markup.md).

### <a name="pricing-note"></a>Houd er rekening mee prijzen

Wanneer u de Text to Speech-service gebruikt, worden in rekening gebracht voor elk teken dat wordt geconverteerd naar spraak, met inbegrip van leestekens. Hoewel het document SSML zelf niet te factureren, worden optionele elementen die worden gebruikt om aan te passen hoe de tekst wordt geconverteerd naar spraak, zoals Fonemen en verkopen, gerekend als factureerbare tekens. Hier volgt een lijst van wat gefactureerd wordt:

* Tekst die wordt doorgegeven aan de Text to Speech-service in de hoofdtekst SSML van de aanvraag
* Alle aantekeningen in het tekstveld van de hoofdtekst van de aanvraag in de indeling SSML, met uitzondering van `<speak>` en `<voice>` tags
* Letters, interpunctie, spaties, tabs, aantekeningen en alle spatietekens
* Elk codepunt is gedefinieerd in Unicode

Zie voor gedetailleerde informatie [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

> [!IMPORTANT]
> Elke taal Chinese, Japans en Koreaanse teken wordt geteld als twee tekens voor facturering.

## <a name="core-features"></a>Belangrijkste functies

Deze tabel bevat de kernfuncties voor tekst naar spraak:

| Use-case | SDK | REST |
|----------|-----|------|
| Converteert tekst naar spraak. | Ja | Ja |
| Gegevenssets voor de toon aanpassing uploaden. | Nee | Ja\* |
| Maken en beheren van spraak lettertype modellen. | Nee | Ja\* |
| Maken en beheren van implementaties van spraak lettertype. | Nee | Ja\* |
| Maken en beheren van spraak lettertype tests. | Nee | Ja\* |
| Abonnementen beheren. | Nee | Ja\* |

\* *Deze services zijn beschikbaar met behulp van het eindpunt cris.ai. Zie [Swagger verwijzing](https://westus.cris.ai/swagger/ui/index). Deze aangepaste gesproken training en beheer-API's implementeren dat aanvragen van de limieten voor 25 per 5 seconden, terwijl de spraaksynthese API zelf wordt geïmplementeerd die beperking kunt 200 aanvragen per seconde met de hoogste beperking. Wanneer bandbreedtebeperking optreedt, moet u een melding via berichtkoppen.*

## <a name="get-started-with-text-to-speech"></a>Aan de slag met tekst naar spraak

We bieden snelstartgidsen ontworpen dat u de uitvoering van code in minder dan 10 minuten. Deze tabel bevat een lijst met tekst naar spraak snelstartgidsen ingedeeld per taal.

### <a name="sdk-quickstarts"></a>SDK-snelstartgidsen

| Quickstart (SDK) | Platform | API-verwijzing |
|------------|----------|---------------|
| [C#, .NET Framework](quickstart-text-to-speech-dotnet-windows.md) | Windows | [Bladeren](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-text-to-speech-cpp-windows.md) | Windows | [Bladeren](https://aka.ms/csspeech/cppref) |
| [C++](quickstart-text-to-speech-cpp-linux.md) | Linux | [Bladeren](https://aka.ms/csspeech/cppref) |

### <a name="rest-quickstarts"></a>REST-snelstartgidsen

| Quickstart (REST) | Platform | API-verwijzing |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Bladeren](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Window, macOS, Linux | [Bladeren](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Window, macOS, Linux | [Bladeren](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeldcode voor tekst naar spraak is beschikbaar op GitHub. Deze voorbeelden omvatten tekst naar spraak converteren in de populairste programmeertalen.

* [Text to Speech-voorbeelden (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
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
