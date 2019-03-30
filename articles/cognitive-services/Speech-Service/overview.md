---
title: Wat zijn de Speech Services van Azure?
titleSuffix: Azure Cognitive Services
description: De Azure Speech Services zijn de vereniging van spraak naar tekst, tekst naar spraak en spraakomzetting in een enkel Azure-abonnement. Het is gemakkelijk om toe te voegen spraak in uw toepassingen, hulpprogramma's en apparaten met de SDK van spraak, spraak Devices SDK of REST-API's. Spraakfunctionaliteit toevoegen aan een bestaande chatbot, te converteren tekst naar spraak in een toepassing vertaling of grote gegevensvolumes call center transcriberen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 03/13/2019
ms.author: erhopf
ms.openlocfilehash: b536c55b0f7c29bc463781d9fda0348ef994c5ce
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662221"
---
# <a name="what-are-the-speech-services"></a>Wat zijn de Speech Services?

Azure Speech Services zijn van spraak naar tekst, tekst naar spraak en spraakomzetting combineert in een enkel Azure-abonnement. Spraak eenvoudig inschakelen van uw toepassingen, hulpprogramma's en apparaten met de [spraak SDK](speech-sdk-reference.md), [spraak Devices SDK](speech-devices-sdk-qsg.md), of [REST-API's](rest-apis.md).

> [!IMPORTANT]
> Bing Speech-API, Translator Speech- en aangepaste spraak vervangen spraakservices. Zie *instructies begeleidt > migratie* voor migratie-instructies.

Deze functies zijn vormen van de Azure-Services voor spraak. Gebruik de koppelingen in deze tabel voor meer informatie over algemene scenario's voor elke functie of de API-verwijzing bladeren.

| Service | Functie | Description | SDK | REST |
|---------|---------|-------------|-----|------|
| [Spraak-naar-tekst](speech-to-text.md) | Spraak-naar-tekst | Spraak-naar-tekst transcribes audiostreams naar tekst in realtime die uw toepassingen, hulpprogramma's of apparaten kunnen gebruiken of weergeven. Gebruik spraak-naar-tekst met [Language Understanding (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) worden afgeleid van de gebruiker intents van getranscribeerde spraak- en act op gesproken opdrachten. | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Batch transcriptie](batch-transcription.md) | Batch transcriptie kunt asynchrone transcriptie van spraak-naar-tekst van grote hoeveelheden gegevens. Dit is een REST gebaseerde service die gebruikmaakt van hetzelfde eindpunt als aanpassing en Modelbeheer. | Nee | [Ja](https://westus.cris.ai/swagger/ui/index) |
| | [Aanpassing](#customize-your-speech-experience) | Als u van spraak-naar-tekst voor de opname- en schrijffouten in een unieke omgeving gebruikmaakt, kunt u maken en trainen aangepaste akoestische, taal en de uitspraak modellen adres omgevingsgeluid of branchespecifieke vocabulaire. | Nee | [Ja](https://westus.cris.ai/swagger/ui/index) |
| [Tekst naar spraak](text-to-speech.md) | Tekst naar spraak | Tekst naar spraak converteert invoertekst naar menselijke kunstmatige spraak. Kies uit de standard stemmen en neurale stemmen (Zie [taalondersteuning](language-support.md)). | Nee | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Aanpassing](#customize-your-speech-experience) | Maak aangepaste spraakstijlen uniek is voor uw merk of product. | Nee | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Spraakomzetting](speech-translation.md) | Spraakomzetting | Spraakomzetting kan realtime, meerdere talen vertaling van spraak naar uw toepassingen, hulpprogramma's en apparaten. Deze service voor spraak-naar-spraak- en spraak-naar-tekst vertalen gebruiken. | [Ja](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Nee |

## <a name="news-and-updates"></a>Nieuws en updates

Meer informatie over wat is er nieuw in de Azure-Services voor spraak.

* Maart 2019 - is een nieuw eindpunt voor spraak die als resultaat een volledige lijst met beschikbare stemmen in een specifieke regio's geeft nu beschikbaar. Bovendien worden nieuwe regio's worden nu ondersteund voor TTS. Zie voor meer informatie, [Text to Speech-API reference (REST)](rest-text-to-speech.md).
* Februari 2019 - die zijn uitgebracht spraak SDK 1.3.0 met ondersteuning voor [Unity (bèta)](quickstart-csharp-unity.md). Ondersteuning toegevoegd voor de `AudioInput` klasse, kunt u de streamingbron van audio kiezen. Zie voor een volledige lijst van verbeteringen en bekende problemen, [opmerkingen bij de Release](releasenotes.md).
* December 2018 - die zijn uitgebracht spraak SDK 1.2.0 met ondersteuning voor [Python](quickstart-python.md) en [Node.js](quickstart-js-node.md), en als Ubuntu 18.04 LTS. Zie voor meer informatie, [opmerkingen bij de Release](releasenotes.md).

## <a name="try-speech-services"></a>Speech Services proberen

We bieden snelstartgidsen in de populairste programmeertalen, elk ontworpen dat u de uitvoering van code in minder dan 10 minuten. Deze tabel bevat de meest populaire snelstartgidsen voor elke functie. Extra talen en platforms verkennen met behulp van de navigatiebalk aan de linkerkant.

| Spraak-naar-tekst (SDK) | Vertaling (SDK) | Text to Speech (REST) |
|-------------------|-------------------|-----------------------|
| [C#, .NET core (Windows)](quickstart-csharp-dotnet-windows.md) | [Java (Windows, Linux)](quickstart-translate-speech-java-jre.md) | [Python (Windows, Linux, macOS)](quickstart-python-text-to-speech.md) |
| [JavaScript (Browser)](quickstart-js-browser.md) | [C#, .NET core (Windows)](quickstart-translate-speech-dotnetcore-windows.md) | [C#, .NET Core (Windows, Linux, macOS)](quickstart-dotnet-text-to-speech.md) |
| [Python (Windows, Linux, macOS)](quickstart-python.md) | [C#, .NET framework (Windows)](quickstart-translate-speech-dotnetframework-windows.md) | [Node.js (Windows, Linux, macOS)](quickstart-nodejs-text-to-speech.md) |
| [Java (Windows, Linux)](quickstart-java-jre.md) | [C++ (Windows)](quickstart-translate-speech-cpp-windows.md) | |

Nadat u een kans om de spraak-Services te gebruiken hebt, Probeer onze zelfstudie u hoe u leert voor het herkennen van intenties van gesproken inhoud met behulp van de SDK van spraak en LUIS.

* [Zelfstudie: Intents van gesproken inhoud met de spraak-SDK en LUIS, herkennenC#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="get-sample-code"></a>Voorbeeldcode ophalen

Voorbeeldcode is beschikbaar op GitHub voor elk van de Azure-Services voor spraak. Deze voorbeelden voor algemene scenario's, zoals lezen van audio van een bestand of de stroom, herkenning van doorlopende en één en werken met aangepaste modellen. Gebruik deze koppelingen om weer te geven SDK en REST-voorbeelden:

* [Spraak-naar-tekst en spraak NAT-voorbeelden (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Voorbeelden van batch transcriptie (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [Text to Speech-voorbeelden (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customize-your-speech-experience"></a>Uw spraakherkenning aanpassen

Azure Speech Services werkt goed met ingebouwde modellen, kunt u echter verder aanpassen en afstemmen van de ervaring voor het product of de omgeving. Aanpassing van opties voor het bereik van akoestisch model afstemmen op de unieke spraakstijlen voor uw merk. Nadat u een aangepast model hebt gemaakt, kunt u deze kunt gebruiken met een van de Azure-Services voor spraak.

| Speech Service | Model | Description |
|----------------|-------|-------------|
| Spraak naar tekst | [Akoestisch model](how-to-customize-acoustic-models.md) | Maak een aangepast akoestisch model voor toepassingen, hulpprogramma's, of apparaten die worden gebruikt in het bijzonder omgevingen, zoals in een auto of op een fabriek, elk met specifieke opname voorwaarden. Voorbeelden zijn onder meer accenten zijn niet toegestaan spraak, specifieke achtergrondgeluiden of met behulp van een specifieke microfoon voor de opname. |
| | [Taalmodel](how-to-customize-language-model.md) | Maak een aangepast taalmodel ter verbetering van transcriptie van veld-specifieke vocabulaire en grammatica, zoals medische terminologie of IT-jargon. |
| | [Uitspraakmodel](how-to-customize-pronunciation.md) | Met een aangepaste uitspraak-model, kunt u de fonetische vorm en de weergave van een woord of een term kunt definiëren. Dit is handig voor het verwerken van aangepaste voorwaarden, zoals productnamen of afkortingen. Alles wat u nodig hebt om te beginnen is een uitspraak van bestand--een eenvoudige txt-bestand. |
| Tekst naar spraak | [Spraakstijl](how-to-customize-voice-font.md) | Aangepaste spraakstijlen kunnen u een herkenbare, één van een unieke stem voor uw merk maken. Het duurt slechts een kleine hoeveelheid gegevens aan de slag. Hoe meer gegevens dat u opgeeft, wordt de meer natuurlijke en menselijke uw spraakstijl klinkt. |

## <a name="reference-docs"></a>Referentiedocumenten

* [Speech-SDK](speech-sdk-reference.md)
* [Spraak apparaten SDK](speech-devices-sdk.md)
* [REST API: Spraak-naar-tekst](rest-speech-to-text.md)
* [REST API: Text-to-speech](rest-text-to-speech.md)
* [REST API: Batch transcriptie en aanpassen](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ontvangt u een abonnementssleutel Speech Services gratis](get-started.md)
