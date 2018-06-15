---
title: Spraak naar tekst gebruiken | Microsoft Docs
description: Informatie over het gebruik van spraak naar tekst in de service spraak
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 878a31992415b1f8688afcfb186fcd94ce2567b4
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "35349981"
---
# <a name="use-speech-to-text-in-the-speech-service"></a>Gebruik 'Spraak-tekst' in de service spraak

U kunt **spraak naar tekst** in uw toepassingen op twee verschillende manieren.

| Methode | Beschrijving |
|-|-|
| [SDK](speech-sdk.md) | Eenvoudigste methode voor C/C++ C# en Java *-ontwikkelaars |
| [REST](rest-apis.md) | Herkent korte utterances met behulp van een HTTP POST-aanvraag | 

\* *De Java SDK maakt deel uit van de [spraak apparaten SDK](speech-devices-sdk.md).*

## <a name="using-the-sdk"></a>De SDK gebruiken

De [spraak SDK](speech-sdk.md) biedt de eenvoudigste manier om te gebruiken **spraak naar tekst** in uw toepassing met volledige functionaliteit.

1. Maken van een fabriek spraak, bieden van een sleutel Speech service-abonnement of een verificatietoken. U kunt ook op dit moment opties, zoals de taal of een aangepaste eindpunt voor uw eigen modellen herkenning van spraak configureren.

2. Een kenmerk ophalen van de fabriek. Er zijn drie soorten kenmerken zijn beschikbaar. Elk type herkenner kunt van uw apparaat standaard microfoon, een audiostroom of audio uit een bestand gebruiken.

    Herkenner | Functie
    -|-
    Spraakherkenner|Tekst schrijffouten van spraak biedt
    Opzet herkenner|Afgeleid spreker bedoeling via [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/) na de opname\*
    De vertaling herkenner|Zet de transcribed tekst in een andere taal (Zie [spraak vertaling](how-to-translate-speech.md))

    \* *Voor opname opzet moet u een afzonderlijke LUIS abonnementssleutel wordt gebruikt bij het maken van een fabriek spraak voor de herkenner opzet.*
    
4. Gebeurtenissen voor een asynchrone bewerking bezighouden indien gewenst. De herkenner roept vervolgens de gebeurtenis-handlers wanneer het tussentijdse en definitieve resultaten heeft. Anders wordt ontvangen uw toepassing een definitieve schrijffouten resultaat.

5. Start herkenning.

### <a name="sdk-samples"></a>SDK-voorbeelden

Zie voor de meest recente set steekproeven van het [cognitieve Services spraak SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="using-the-rest-api"></a>Met behulp van de REST-API

De REST-API is de eenvoudigste manier om spraakherkenning herkennen als u een taal die wordt ondersteund door de SDK niet gebruikt. U maakt een HTTP POST-aanvraag met het service-eindpunt de hele utterance in de hoofdtekst van de aanvraag wordt doorgegeven. U ontvangt een antwoord dat de herkende tekst bevat.

> [!NOTE]
> Utterances zijn beperkt tot 15 seconden of minder wanneer u de REST-API.


Voor meer informatie over de **spraak naar tekst** REST-API, Zie [REST-API's](rest-apis.md#speech-to-text). Als u wilt in actie weergeven, downloaden de [REST-API samples](https://github.com/Azure-Samples/SpeechToText-REST) vanuit GitHub.

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Het herkennen van spraak in C#](quickstart-csharp-windows.md)
