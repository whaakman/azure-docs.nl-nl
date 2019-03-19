---
title: 'Snelstartgids: Spraak herkennen, C++ (Linux) - Spraakservices'
titleSuffix: Azure Cognitive Services
description: Informatie over het herkennen van gesproken tekst in C++ onder Linux met behulp van de Speech SDK
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: wolfma
ms.openlocfilehash: 2b63eea7b8562a6972aecef02395d734d1c2655f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57883862"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-by-using-the-speech-sdk"></a>Quickstart: Gesproken tekst herkennen in C++ onder Linux met behulp van de Speech-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een C++ consoletoepassing voor Ubuntu Linux 16.04 of 18.04. U gebruikt de [Speech SDK](speech-sdk.md) van Cognitive Services om in realtime spraak te transcriberen naar tekst via de microfoon van uw pc. De toepassing is gemaakt met de [Speech SDK voor Linux](https://aka.ms/csspeech/linuxbinary) en de C++ compiler van uw Linux-distributie (bijvoorbeeld `g++`).

## <a name="prerequisites"></a>Vereisten

U moet een abonnementssleutel Speech Services voor het voltooien van deze Quickstart. U kunt er gratis een krijgen. Zie [de Speech Services gratis uitproberen](get-started.md) voor meer informatie.

## <a name="install-speech-sdk"></a>Speech SDK installeren

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Speech SDK van Cognitive Services is `1.3.1`.

De Speech SDK voor Linux kan worden gebruikt om zowel 32-bits als 64-bits toepassingen te compileren. De vereiste bibliotheken en headerbestanden kunnen als een TAR-bestand worden gedownload vanuit https://aka.ms/csspeech/linuxbinary.

Download en installeer de SDK als volgt:

1. Zorg ervoor dat de SDK-afhankelijkheden zijn geïnstalleerd.

   ```sh
   sudo apt-get update
   sudo apt-get install build-essential libssl1.0.0 libasound2 wget
   ```

1. Kies een map waar de bestanden van de Speech SDK moeten worden uitgepakt en laat de `SPEECHSDK_ROOT`-omgevingsvariabele naar die map verwijzen. Met deze variabele kunt u in toekomstige opdrachten eenvoudig naar de map verwijzen. Als u de map `speechsdk` bijvoorbeeld wilt gebruiken in de basismap, gebruik dan een opdracht als de volgende:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Maak de map als deze nog niet bestaat.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Download en extraheer het `.tar.gz`-archief met de binaire bestanden voor de Speech SDK:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Valideer de inhoud van de map op het hoogste niveau van het uitgepakte pakket:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   De mapvermelding moet de kennisgeving van derden en licentiebestanden bevatten, evenals een `include`-map met headerbestanden (`.h`) en een `lib`-map met bibliotheken.

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Maak een C++ bronbestand met de naam `helloworld.cpp` en plak er de volgende code in.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Vervang de tekenreeks in dit nieuwe bestand `YourSubscriptionKey` met uw abonnementssleutel Speech Services.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="build-the-app"></a>De app bouwen

> [!NOTE]
> Zorg ervoor dat u de onderstaande opdrachten als _één opdrachtregel_ invoert. De eenvoudigste manier om dat te doen is om de opdracht te kopiëren met behulp van de knop **Kopiëren** naast elke opdracht en deze vervolgens bij de shell-prompt te plakken.

* Voer op een **x64** (64-bits) systeem de volgende opdracht uit om de toepassing te compileren.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libasound.so.2
  ```

* Voer op een **x86** (32-bits) systeem de volgende opdracht uit om de toepassing te compileren.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libasound.so.2
  ```

## <a name="run-the-app"></a>De app uitvoeren

1. Configureer het bibliotheekpad van het laadprogramma om te verwijzen naar de bibliotheek van de Speech SDK.

   * Voer op een **x64** (64-bits) systeem de volgende opdracht uit.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * Voer op een **x86** (32-bits) systeem de volgende opdracht uit.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
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
