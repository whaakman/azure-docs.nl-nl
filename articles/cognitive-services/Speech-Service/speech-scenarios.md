---
title: Scenario's voor Azure Cognitive Services spraak
description: Scenario's
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 07/02/2018
ms.author: panosper
ms.openlocfilehash: 5179f0a81ea67c1b20e9cbf77e18595e0b4bafa4
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495278"
---
# <a name="speech-scenarios"></a>Spraakscenario's

Er zijn veel scenario's die kunnen worden gemachtigd met behulp van technologie voor spraak. We analyseren van enkele van de meest voorkomende en ontdekt u de desbetreffende functies in de documentatie. Voor het merendeel van de inhoud, de [SDK](speech-sdk.md) speelt een centrale rol bij het inschakelen van deze scenario's.

De pagina wordt beschreven hoe u:
> [!div class="checklist"]
> * Stem geactiveerd apps maken
> * Call center audio gesprekken transcriberen
> * Stem Bots

## <a name="voice-triggered-apps"></a>Stem geactiveerd Apps

Veel gebruikers wilt inschakelen spraakinvoer in op hun toepassingen. Spraakinvoer is een uitstekende manier om uw app maken voor flexibele, worden met behulp van deze gratis is (bijvoorbeeld in een auto) handen of versnellen van uiteenlopende taken zoals het vragen naar aanwijzingen gegevens van het nieuws of weer. 

### <a name="voice-triggered-apps-with-baseline-models"></a>Stem geactiveerd Apps met basislijn-modellen

Als uw app wordt gebruikt om te worden gebruikt door het grote publiek in omgevingen waar de achtergrondgeluiden niet overmatig is, de eenvoudigste en snelste manier om dit te doen gewoon worden downloaden van onze [spraak SDK](speech-sdk.md) en de relevante [ Voorbeelden](quickstart-csharp-dotnet-windows.md). De SDK mogelijk gemaakt door uw [Azure-abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/) biedt ontwikkelaars de mogelijkheid voor het uploaden van audio naar basislijn erkenning spraakmodellen die ten grondslag Cortana en Skype. De modellen van moderne, en worden gebruikt door de hiervoor genoemde producten. U kunt binnen een paar minuten aan de slag gaan.

### <a name="voice-triggered-apps-with-custom-models"></a>Stem geactiveerd Apps met aangepaste modellen

Als uw app in een specifiek domein, (zeg chemie, biologie of speciale wensen)-adressen is u misschien wilt gebruiken om aan te passen een [taalmodel](how-to-customize-language-model.md). Een taalmodel aan te passen, leert de decoder over de meest voorkomende zinnen en woorden die worden gebruikt door uw app. De decoder is mogelijk een met een aangepast taalmodel voor een bepaald domein in plaats van de basislijn-model spraak nauwkeuriger te transcriberen. Op dezelfde manier als de achtergrondgeluiden waar uw app gaat worden gebruikt opvallende kunt u een akoestisch model aanpassen. Verken de documentatie voor andere gevallen waarin [aanpassing van taal](how-to-customize-language-model.md) en [akoestische aanpassing](how-to-customize-acoustic-models.md) waarde opgeven en Ga naar onze [aanpassing van woordenlijsten portal](https://customspeech.ai) voor kick-Starting creatieprocedure in het model. Net als bij basislijn modellen, aangepaste modellen worden aangeroepen onze [spraak SDK](speech-sdk.md) en de relevante [voorbeelden](quickstart-csharp-dotnet-windows.md).

## <a name="transcribe-call-center-audio-calls"></a>Call center audio gesprekken transcriberen

Callcenters oplopen grote hoeveelheden audio. Verborgen binnen deze audiobestanden accent-waarde die kan worden verkregen via transcriptie. De duur van de oproep, worden het sentiment, de tevredenheid van de klant en de algemene waarde de aanroep naar de aanroeper opgegeven gedetecteerd door het verkrijgen van de aanroep transcripties.

Het beste beginpunt is de [transcriptie API voor Batch](batch-transcription.md) samen met verwante [voorbeeld](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

U moet eerst een [Azure-abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/) en vervolgens moet u raadplegen de [documentatie]([Batch transcription API](batch-transcription.md)).

### <a name="transcribe-call-center-audio-calls-with-baseline-models"></a>Transcriberen Call center audio aanroepen met een basislijn modellen

De beslissing die moet worden gemaakt, is of u de interne basislijn-modellen uitvoeren van de transcriptie, aanpassen van een taal of een akoestisch model of beide wilt gebruiken. De API vereist alleen de API-sleutel voor het gebruik van basislijn-modellen. Intern wordt de API aanroepen van het beste model voor uw gegevens en aan te passen.

### <a name="transcribe-call-center-audio-calls-with-custom-models"></a>Transcriberen Call center-audio aanroepen met aangepaste modellen

Als u van plan bent te gebruiken van een aangepast model, klikt u vervolgens moet u de ID van dit model, samen met de API-sleutel. De Model-ID wordt verkregen uit de [aanpassing van woordenlijsten portal](https://customspeech.ai). Dit is niet de eindpunt-ID die u op de weergave Details van eindpunt vinden, maar de model-ID die u ophalen kunt wanneer u de Details van dit model op.

## <a name="voice-bots"></a>Stem Bots

Ontwikkelaars kunnen hun toepassingen met spraak uitvoer maken. De Spraakservice kunnen synthetize spraak voor een aantal [talen](supported-languages.md) en biedt de [eindpunten](rest-apis.md) voor het raadplegen en die mogelijkheid toe te voegen aan uw app.

Bovendien kunnen de Speech-Service voor gebruikers die u wilt meer herkenbaarheid en uniekheid toevoegen aan hun bots, ontwikkelaars een unieke spraakstijl aanpassen. Vergelijkbaar met het aanpassen van spraakherkenning modellen spraakstijlen vereisen gebruikersgegevens. Ontwikkelaars zijn geüpload dat de gegevens in onze [stem aanpassing van woordenlijsten portal](https://customspeech.ai) en bouw uw unieke merk van stem voor uw bot. Details worden beschreven [hier](how-to-text-to-speech.md) , evenals de [Veelgestelde vragen over](faq-text-to-speech.md) pagina's 

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Beginnen met de spraak-SDK](speech-sdk.md)
