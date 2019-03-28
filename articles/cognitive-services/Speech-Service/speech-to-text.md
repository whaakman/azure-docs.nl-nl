---
title: Spraak-naar-tekst met Azure Speech Services
titleSuffix: Azure Cognitive Services
description: Spraak-naar-tekst van Azure Speech Services, ook wel bekend als spraak naar tekst, kunnen realtime transcriptie van audiostreams naar tekst die uw toepassingen, hulpprogramma's of apparaten gebruiken kunnen, weergeven en actie ondernemen als opdrachtinvoer. Deze service wordt mogelijk gemaakt door de dezelfde opname-technologie die door Microsoft gebruikt voor Cortana en Office-producten en werkt naadloos samen met de vertaling en tekst naar spraak.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 6400e3d3fa7f0317ff927f2931e705365a450770
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58520663"
---
# <a name="what-is-speech-to-text"></a>Wat is de spraak-naar-tekst?

Spraak-naar-tekst van Azure Speech Services, ook wel bekend als spraak naar tekst, kunnen realtime transcriptie van audiostreams naar tekst die uw toepassingen, hulpprogramma's of apparaten gebruiken kunnen, weergeven en actie ondernemen als opdrachtinvoer. Deze service wordt mogelijk gemaakt door de dezelfde opname-technologie die door Microsoft gebruikt voor Cortana en Office-producten en werkt naadloos samen met de vertaling en tekst naar spraak.  Zie voor een volledige lijst van beschikbare talen voor spraak-naar-tekst, [ondersteunde talen](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/language-support#speech-to-text).

De spraak-naar-tekst-service gebruikt standaard het universele taalmodel. Dit model is getraind met gegevens die eigendom zijn van Microsoft en is de cloud geïmplementeerd. Dit is geoptimaliseerd voor conversatie en dicteren scenario's. Als u van spraak-naar-tekst voor de opname- en schrijffouten in een unieke omgeving gebruikmaakt, kunt u maken en trainen aangepaste akoestische, taal en de uitspraak modellen adres omgevingsgeluid of branchespecifieke vocabulaire. 

U kunt eenvoudig vastleggen audio van een microfoon, lezen via een stroom of toegang tot audio-bestanden uit de opslag met de spraak-SDK en REST-API's. De spraak-SDK biedt ondersteuning voor WAV/PCM-16-bits, 16 kHz, één kanaal audio voor spraakherkenning. Aanvullende audio-indelingen worden ondersteund met behulp van de [REST-eindpunt voor spraak-naar-tekst](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) of de [batch-service transcriptie](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats).

## <a name="core-features"></a>Belangrijkste functies

Hier zijn de functies die beschikbaar zijn via de spraak-SDK en REST-API's:

| Toepassing | SDK | REST |
|----------|-----|------|
| Korte uitingen transcriberen (< 15 seconden). Biedt alleen ondersteuning voor definitieve transcriptie resultaat. | Ja | Ja |
| Continue transcriptie van lange uitingen en audio streamen (> 15 seconden). Biedt ondersteuning voor tussentijdse en definitieve transcriptie resultaten. | Ja | Nee |
| Intents worden afgeleid van herkenningsresultaten met [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis). | Ja | Nee\* |
| Batch-asynchroon transcriptie van audio-bestanden. | Nee | Ja\** |
| Maken en beheren van spraakmodellen. | Nee | Ja\** |
| Maken en beheren van implementaties van aangepast model. | Nee | Ja\** |
| Nauwkeurigheidstests om te meten van de nauwkeurigheid van het model basislijn ten opzichte van aangepaste modellen maken. | Nee | Ja\** |
| Abonnementen beheren. | Nee | Ja\** |

\* *LUIS intenties en entiteiten kunnen worden afgeleid met behulp van een afzonderlijk LUIS-abonnement. Met dit abonnement, de SDK LUIS-aanroep voor u en bieden entiteits- en intentie resultaten. Met de REST-API, roept u LUIS zelf voor het afleiden van intenties en entiteiten met uw LUIS-abonnement.*

\** *Deze services zijn beschikbaar met behulp van het eindpunt cris.ai. Zie [Swagger verwijzing](https://westus.cris.ai/swagger/ui/index).*

## <a name="get-started-with-speech-to-text"></a>Aan de slag met de spraak-naar-tekst

We bieden snelstartgidsen in de populairste programmeertalen, elk ontworpen dat u de uitvoering van code in minder dan 10 minuten. Deze tabel bevat een volledige lijst van spraak SDK snelstartgidsen ingedeeld per taal.

| Snelstartgids | Platform | API-verwijzing |
|------------|----------|---------------|
| [C#, .NET Core](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-csharp-dotnetcore-windows) | Windows | [Bladeren](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-csharp-dotnet-windows) | Windows | [Bladeren](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-csharp-uwp) | Windows | [Bladeren](https://aka.ms/csspeech/csharpref) |
| [C++](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-cpp-windows) | Windows | [Bladeren](https://aka.ms/csspeech/cppref)|
| [C++](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-cpp-linux) | Linux | [Bladeren](https://aka.ms/csspeech/cppref) |
| [Java](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-java-android) | Android | [Bladeren](https://aka.ms/csspeech/javaref) |
| [Java](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-java-jre) | Windows, Linux | [Bladeren](https://aka.ms/csspeech/javaref) |
| [JavaScript, Browser](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-js-browser) | Browser, Windows, Linux, macOS | [Bladeren](https://aka.ms/AA434tv) |
| [JavaScript, Node.js](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-js-node) | Windows, Linux, macOS | [Bladeren](https://aka.ms/AA434tv) |
| [Objective-C](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-objectivec-ios) | iOS | [Bladeren](https://aka.ms/csspeech/objectivecref) |
| [Python](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-python) | Windows, Linux, macOS | [Bladeren](https://aka.ms/AA434tr)  |

Als u liever de spraak-naar-tekst REST-service wilt gebruiken, Zie [REST-API's](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

## <a name="tutorials-and-sample-code"></a>Zelfstudies en voorbeeldcode

Nadat u een kans om de spraak-Services te gebruiken hebt, Probeer onze zelfstudie u hoe u leert voor het herkennen van intenties van gesproken inhoud met behulp van de SDK van spraak en LUIS.

* [Zelfstudie: Intents van gesproken inhoud met de spraak-SDK en LUIS, herkennenC#](how-to-recognize-intents-from-speech-csharp.md)

Voorbeeldcode voor de spraak-SDK is beschikbaar op GitHub. Deze voorbeelden voor algemene scenario's, zoals lezen van audio van een bestand of de stroom, herkenning van doorlopende en één en werken met aangepaste modellen.

* [Voorbeelden van spraak-naar-tekst (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Voorbeelden van batch transcriptie (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>Aanpassing

Naast het universele model door de Speech Services gebruikt, kunt u aangepaste akoestische, taal en de uitspraak modellen specifieke tot uw ervaring. Hier volgt een lijst met opties voor het aanpassen:

| Model | Description |
|-------|-------------|
| [Akoestisch model](how-to-customize-acoustic-models.md) | Het maken van een aangepast akoestisch model is handig als uw toepassing, hulpprogramma's of apparaten worden gebruikt in een bepaalde omgeving, zoals in een auto of een gegevensfactory met de voorwaarden van de specifieke opnemen. Voorbeelden zijn spraak met een accent, specifieke achtergrondgeluiden of het gebruik van een specifieke microfoon voor opname. |
| [Taalmodel](how-to-customize-language-model.md) | Maak een aangepast taalmodel ter verbetering van transcriptie van branchespecifieke vocabulaire en grammatica, zoals medische terminologie of IT-jargon. |
| [Uitspraakmodel](how-to-customize-pronunciation.md) | Met een aangepaste uitspraak-model, kunt u de fonetische vorm en de weergave van een woord of een term kunt definiëren. Dit is handig voor het verwerken van aangepaste voorwaarden, zoals productnamen of afkortingen. Alles wat u nodig hebt om te beginnen is een uitspraak van bestand--een eenvoudige txt-bestand. |

> [!NOTE]
> Opties voor het aanpassen variëren per taal/landinstelling (Zie [ondersteunde talen](supported-languages.md)).

## <a name="migration-guides"></a>Migratiehandleidingen

> [!WARNING]
> Bing Speech wordt gesteld op 15 oktober 2019.

Als uw toepassingen, hulpprogramma's of producten die van de Bing Speech-API's of aangepaste spraak gebruikmaakt, hebben we hulplijnen waarmee u kunt migreren naar Speech Services gemaakt.

* [Migreren van Bing Speech naar de spraakservices](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-bing-speech)
* [Migreren van aangepaste spraak naar de spraakservices](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-custom-speech-service)

## <a name="reference-docs"></a>Referentiedocumenten

* [Speech-SDK](speech-sdk-reference.md)
* [Spraak apparaten SDK](speech-devices-sdk.md)
* [REST API: Spraak-naar-tekst](rest-speech-to-text.md)
* [REST API: Text-to-speech](rest-text-to-speech.md)
* [REST API: Batch transcriptie en aanpassen](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Volgende stappen

* [Ontvangt u een abonnementssleutel Speech Services gratis](get-started.md)
* [De spraak-SDK ophalen](speech-sdk.md)
