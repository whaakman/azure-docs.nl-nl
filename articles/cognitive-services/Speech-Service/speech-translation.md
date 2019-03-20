---
title: Spraakomzetting met Azure Speech Services
titlesuffix: Azure Cognitive Services
description: De spraak-Services kunt u end-to-end-, realtime, meerdere talen vertaling van spraak kunt toevoegen aan uw toepassingen, hulpprogramma's en apparaten. Dezelfde API kan worden gebruikt voor zowel spraak-naar-spraak- en spraak naar tekst converteren.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 95682612b4b0fdb1baa5038039630e74abddb1a9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57890464"
---
# <a name="what-is-speech-translation"></a>Wat is de vertaling van gesproken tekst?

Met spraakomzetting van Azure Speech Services, kunt realtime, meerdere talen spraak-naar-spraak en spraak-naar-tekst vertalen van audiostreams. Met de SDK-spraak zijn uw toepassingen, hulpprogramma's en apparaten toegang tot de bron transcripties en uitvoer van de vertaling voor de opgegeven audio. Tijdelijke transcriptie en vertaling resultaten worden geretourneerd als spraak wordt gedetecteerd en eindronden resultaten kunnen worden omgezet in kunstmatige spraak.

NAT-engine van Microsoft wordt aangestuurd door twee verschillende benaderingen: statistische machinevertalingen (SMT) en neurale machinevertalingen (NMT). SMT maakt gebruik van geavanceerde statistische analyse kunt u schatten van de best mogelijke vertalingen gegeven van de context van een paar woorden. Met NMT, zijn neurale netwerken gebruikt voor nauwkeurigere, natuurlijk klinkende vertalingen opgeven met behulp van de volledige context van zinnen om te vertalen woorden.

Vandaag de dag door Microsoft NMT gebruikt voor het omzetten naar de meest populaire talen. Alle [beschikbare talen voor spraak-naar-spraak vertaling](language-support.md#speech-translation) worden aangestuurd door NMT. Spraak naar tekst converteren kan gebruikmaken van SMT of NMT, afhankelijk van de combinatie van taal. Wanneer de doeltaal wordt ondersteund door NMT, is de volledige vertaling NMT aangestuurde. Wanneer de doeltaal wordt niet ondersteund door NMT, is de omzetting van een hybride van NMT en SMT, met behulp van Engels als een "draaien" tussen de twee talen.

## <a name="core-features"></a>Belangrijkste functies

Hier zijn de functies die beschikbaar zijn via de spraak-SDK en REST-API's:

| Toepassing | SDK | REST |
|----------|-----|------|
| Spraak naar tekst converteren met van herkenningsresultaten. | Ja | Nee |
| Spraak-naar-spraak vertaling. | Ja | Nee |
| Tijdelijke opname- en NAT-resultaten. | Ja | Nee |

## <a name="get-started-with-speech-translation"></a>Aan de slag met spraakomzetting

We bieden snelstartgidsen ontworpen dat u de uitvoering van code in minder dan 10 minuten. Deze tabel bevat een lijst met spraak vertaling snelstartgidsen ingedeeld per taal.

| Snelstartgids | Platform | API-verwijzing |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-translate-speech-dotnetcore-windows.md) | Windows | [Bladeren](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](quickstart-translate-speech-dotnetframework-windows.md) | Windows | [Bladeren](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-translate-speech-uwp.md) | Windows | [Bladeren](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-translate-speech-cpp-windows.md) | Windows | [Bladeren](https://aka.ms/csspeech/cppref)|
| [Java](quickstart-translate-speech-java-jre.md) | Windows | [Bladeren](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Voorbeeldcode

Voorbeeldcode voor de spraak-SDK is beschikbaar op GitHub. Deze voorbeelden voor algemene scenario's, zoals lezen van audio van een bestand of de stroom, continue en één erkenning/vertaling van en werken met aangepaste modellen.

* [Spraak-naar-tekst- en NAT-voorbeelden (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Migratiehandleidingen

> [!WARNING]
> Translator Speech wordt gesteld op 15 oktober 2019.

Als uw toepassingen, hulpprogramma's of producten die Translator Speech, hebben we hulplijnen waarmee u kunt migreren naar de Speech Services gemaakt.

* [Migreren van de Translator Speech-API naar de spraakservices](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Referentiedocumenten

* [Speech-SDK](speech-sdk-reference.md)
* [Spraak apparaten SDK](speech-devices-sdk.md)
* [REST API: Spraak-naar-tekst](rest-speech-to-text.md)
* [REST API: Text-to-speech](rest-text-to-speech.md)
* [REST API: Batch transcriptie en aanpassen](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Volgende stappen

* [Ontvangt u een abonnementssleutel Speech Services gratis](get-started.md)
* [De spraak-SDK ophalen](speech-sdk.md)
