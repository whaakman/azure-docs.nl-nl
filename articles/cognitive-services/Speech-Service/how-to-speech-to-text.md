---
title: Gebruik spraak naar tekst | Microsoft Docs
description: Informatie over het gebruik van spraak naar tekst in de Speech-service
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 26cecedfc3ad2d472b9686e25054fe08253cee77
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39068519"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>'Spraak naar tekst' in de Speech-service gebruiken

U kunt **spraak naar tekst** in uw toepassingen op twee verschillende manieren.

| Methode | Beschrijving |
|-|-|
| [SDK](speech-sdk.md) | Eenvoudigste methode voor C/C++, C# en Java-ontwikkelaars |
| [REST](rest-apis.md) | Herkent korte uitingen met behulp van een HTTP POST-aanvraag | 

## <a name="using-the-sdk"></a>De SDK gebruiken

De [spraak SDK](speech-sdk.md) biedt de eenvoudigste manier om het gebruik van **spraak naar tekst** in uw toepassing met volledige functionaliteit.

1. Maken van een factory spraak, bieden een abonnementssleutel voor spraak-service en [regio](regions.md) of een verificatietoken. U kunt ook opties, zoals de taal of een aangepast eindpunt voor uw eigen modellen voor spraakherkenning spraakherkenning, op dit moment configureren.

2. Ontvang een kenmerk van de factory. Er zijn drie verschillende typen kenmerken zijn beschikbaar. Elk type herkenning kunt van het apparaat standaard microfoon, een audiostream of audio uit een bestand gebruiken.

    Herkenning | Functie
    -|-
    Spraakherkenning|Biedt tekst transcriptie van spraak
    Intentie herkenning|Is afgeleid van intentie van de spreker via [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) na de opname\*
    NAT-herkenning|Vertaalt de getranscribeerde tekst naar een andere taal (Zie [Spraakomzetting](how-to-translate-speech.md))

    \* *Voor intentieherkenning moet u een afzonderlijke LUIS-abonnementssleutel gebruiken bij het maken van een fabriek spraak voor de intentie herkenning.*
    
4. De gebeurtenissen voor asynchrone bewerking bezighouden indien gewenst. De gebeurtenis-handlers de herkenning wordt aangeroepen wanneer er een tijdelijke en laatste resultaten. Anders ontvangt uw toepassing een definitieve transcriptie-resultaat.

5. Opname starten.
   Gebruik voor één spraakherkenning, zoals opdracht of een query spraakherkenning, `RecognizeAsync()`, die de eerste utterance wordt erkend als resultaat geeft.
   Voor langlopende spraakherkenning, zoals transcriptie, gebruikt u `StartContinuousRecognitionAsync()` en de gebeurtenissen voor asynchrone herkenningsresultaten bezighouden.

### <a name="sdk-samples"></a>SDK-voorbeelden

Zie voor de meest recente set voorbeelden, de [Cognitive Services Speech SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>Met behulp van de REST-API

De REST-API is de eenvoudigste manier om spraak herkennen als u een taal die wordt ondersteund door de SDK niet gebruikt. U een HTTP POST-aanvraag naar de service-eindpunt, de hele utterance in de hoofdtekst van de aanvraag wordt doorgegeven. U ontvangt een antwoord met de herkende tekst.

> [!NOTE]
> Uitingen zijn beperkt tot 15 seconden of minder bij het gebruik van de REST-API.

Voor meer informatie over de **spraak naar tekst** REST-API, Zie [REST-API's](rest-apis.md#speech-to-text). Om te zien hoe actie, downloaden de [REST-API-voorbeelden](https://github.com/Azure-Samples/SpeechToText-REST) vanuit GitHub.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Het herkennen van gesproken tekst in C++](quickstart-cpp-windows.md)
- [Spraak herkennen in C#](quickstart-csharp-dotnet-windows.md)
- [Het herkennen van gesproken tekst in Java](quickstart-java-android.md)
