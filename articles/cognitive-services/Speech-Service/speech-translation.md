---
title: Spraak omzetting met spraak service
titleSuffix: Azure Cognitive Services
description: Met de spraak service kunt u een end-to-end, realtime vertaling van spraak naar uw toepassingen, hulpprogram ma's en apparaten toevoegen. Dezelfde API kan worden gebruikt voor zowel spraak-naar-spraak- en spraak naar tekst converteren.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: cfcefd0b18831163324519b61dbea305f90f44bc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552646"
---
# <a name="what-is-speech-translation"></a>Wat is spraak omzetting?

Spraak omzetting van Azure speech Services, biedt de mogelijkheid om audio gegevensstromen in realtime, meertalige spraak naar spraak en spraak naar tekst te converteren. Met de spraak-SDK hebben uw toepassingen, hulpprogram ma's en apparaten toegang tot de bron-transcripties en de Vertaal uitvoer voor de audio. Tijdelijke transcriptie en vertaal resultaten worden geretourneerd als spraak wordt gedetecteerd en de eind resultaten kunnen worden omgezet in gesynthesizerde spraak.

De Vertaal engine van micro soft wordt aangedreven door twee verschillende benaderingen: statistische machine vertaling (SMT) en Neural machine translation (NMT). SMT maakt gebruik van geavanceerde statistische analyses om de best mogelijke vertalingen te ramen, op basis van de context van enkele woorden. Met NMT worden Neural-netwerken gebruikt om nauw keurige, natuurlijke geluids omzetting te bieden met behulp van de volledige context van zinnen voor het vertalen van woorden.

Momenteel gebruikt micro soft NMT voor de vertaling van de populairste talen. Alle [beschikbare talen voor spraak-naar-spraak vertaling](language-support.md#speech-translation) worden aangestuurd door NMT. Spraak naar tekst converteren kan gebruikmaken van SMT of NMT, afhankelijk van de combinatie van taal. Wanneer de doel taal wordt ondersteund door NMT, is de volledige vertaling NMTd. Wanneer de doel taal niet wordt ondersteund door NMT, is de vertaling een hybride van NMT en SMT, met behulp van Engels als ' draai tabel ' tussen de twee talen.

## <a name="core-features"></a>Kern functies

Hier volgen de functies die beschikbaar zijn via de Speech SDK en REST Api's:

| Use-case | SDK | REST |
|----------|-----|------|
| Conversie van spraak naar tekst met herkennings resultaten. | Ja | Nee |
| Conversie van spraak naar spraak. | Ja | Nee |
| Tussentijdse herkenning en omzettings resultaten. | Ja | Nee |

## <a name="get-started-with-speech-translation"></a>Aan de slag met spraak omzetting

We bieden Quick starts die zijn ontworpen om code in minder dan 10 minuten uit te voeren. Deze tabel bevat een lijst met Quick starts voor spraak vertalingen, geordend op taal.

| Quick Start | Platform | API-verwijzing |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-translate-speech-dotnetcore-windows.md) | Windows | [Door](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](quickstart-translate-speech-dotnetframework-windows.md) | Windows | [Door](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-translate-speech-uwp.md) | Windows | [Door](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-translate-speech-cpp-windows.md) | Windows | [Door](https://aka.ms/csspeech/cppref)|
| [Java](quickstart-translate-speech-java-jre.md) | Windows, Linux, macOS | [Door](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeld code voor de Speech SDK is beschikbaar op GitHub. Deze voor beelden hebben betrekking op veelvoorkomende scenario's, zoals het lezen van audio van een bestand of stream, continue en eenmalige herkenning/omzetting en werken met aangepaste modellen.

* [Voor beelden van spraak naar tekst en vertaling (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Migratie handleidingen

Als uw toepassingen, hulpprogram ma's of producten de [Translator Speech-API](https://docs.microsoft.com/azure/cognitive-services/translator-speech/overview)gebruiken, hebben we gidsen gemaakt om u te helpen migreren naar de spraak Services.

* [Migreren van de Translator Speech-API naar de spraak Services](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Referentiedocumenten

* [Speech-SDK](speech-sdk-reference.md)
* [SDK voor spraak apparaten](speech-devices-sdk.md)
* [REST API: Spraak naar tekst](rest-speech-to-text.md)
* [REST API: Tekst-naar-spraak](rest-text-to-speech.md)
* [REST API: Batch-transcriptie en-aanpassing](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Volgende stappen

* [Gratis een abonnements sleutel voor spraak Services aanschaffen](get-started.md)
* [De Speech SDK ophalen](speech-sdk.md)
