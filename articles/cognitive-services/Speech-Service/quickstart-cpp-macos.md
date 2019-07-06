---
title: 'Quickstart: Water, C++ (Mac OS) - spraakservices'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in C++ in macOS met behulp van de spraak-SDK
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: de170955ced45040541565800fc9853edea322f5
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603068"
---
# <a name="quickstart-recognize-speech-in-c-on-macos-by-using-the-speech-sdk"></a>Quickstart: Herkennen van gesproken tekst in C++ in macOS met behulp van de spraak-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een C++-consoletoepassing voor macOS 10.13 en hoger. U gebruikt de Cognitive Services [spraak SDK](speech-sdk.md) spraak naar tekst in realtime uit van uw Mac microfoon transcriberen. De toepassing is gemaakt met de [spraak-SDK voor Mac OS](https://aka.ms/csspeech/macosbinary) en van uw Mac standaard C++-compiler (bijvoorbeeld `g++`).

## <a name="prerequisites"></a>Vereisten

U moet een abonnementssleutel Speech Services voor het voltooien van deze Quickstart. U kunt er gratis een krijgen. Zie [de Speech Services gratis uitproberen](get-started.md) voor meer informatie.

## <a name="install-speech-sdk"></a>Speech SDK installeren

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Speech SDK van Cognitive Services is `1.6.0`.

De spraak-SDK voor Mac OS kan worden gedownload als een bundel gezipte framework van https://aka.ms/csspeech/macosbinary.

Download en installeer de SDK als volgt:

1. Kies een map waar de bestanden van de Speech SDK moeten worden uitgepakt en laat de `SPEECHSDK_ROOT`-omgevingsvariabele naar die map verwijzen. Met deze variabele kunt u in toekomstige opdrachten eenvoudig naar de map verwijzen. Als u de map `speechsdk` bijvoorbeeld wilt gebruiken in de basismap, gebruik dan een opdracht als de volgende:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Maak de map als deze nog niet bestaat.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Downloaden en uitpakken van de `.zip` archief met de spraak-SDK-framework:

   ```sh
   wget -O SpeechSDK-macOS.zip https://aka.ms/csspeech/macosbinary
   unzip SpeechSDK-macOS.zip -d "$SPEECHSDK_ROOT"
   ```

1. Valideer de inhoud van de map op het hoogste niveau van het uitgepakte pakket:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   De mapweergave mag de kennisgeving van derden en licentiebestanden, evenals een `MicrosoftCognitiveServicesSpeech.framework` directory.

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Maak een C++ bronbestand met de naam `helloworld.cpp` en plak er de volgende code in.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-macos/helloworld.cpp#code)]

1. Vervang de tekenreeks in dit nieuwe bestand `YourSubscriptionKey` met uw abonnementssleutel Speech Services.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="build-the-app"></a>De app bouwen

> [!NOTE]
> Zorg ervoor dat u de onderstaande opdrachten als _één opdrachtregel_ invoert. De eenvoudigste manier om dat te doen is om de opdracht te kopiëren met behulp van de knop **Kopiëren** naast elke opdracht en deze vervolgens bij de shell-prompt te plakken.

* Voer de volgende opdracht om de toepassing te bouwen.

  ```sh
  g++ helloworld.cpp -o helloworld --std=c++14 -F${SPEECHSDK_ROOT} -framework MicrosoftCognitiveServicesSpeech
  ```

## <a name="run-the-app"></a>De app uitvoeren

1. Configureer het bibliotheekpad van het laadprogramma om te verwijzen naar de bibliotheek van de Speech SDK.

    ```sh
    export DYLD_FRAMEWORK_PATH="$DYLD_FRAMEWORK_PATH:$SPEECHSDK_ROOT"
    ```

1. Voer de toepassing uit.

   ```sh
   ./helloworld
   ```

1. In het consolevenster wordt een prompt weergegeven waarin u wordt gevraagd om iets te zeggen. Spreek een Engelse woordgroep of zin in. Uw stem is verzonden naar de Services voor spraak en getranscribeerde tekst, die in hetzelfde venster wordt weergegeven.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C++-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
