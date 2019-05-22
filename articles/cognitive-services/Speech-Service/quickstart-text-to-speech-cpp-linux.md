---
title: 'Quickstart: Spraak, C++ (Linux) - spraakservices bootsen'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het nabootsen van gesproken tekst in C++ in Linux met behulp van de spraak-SDK
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: yinhew
ms.openlocfilehash: b5b768e52a40652840e9e3d3069df8a9c895f77b
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001623"
---
# <a name="quickstart-synthesize-speech-in-c-on-linux-by-using-the-speech-sdk"></a>Quickstart: Nabootsen van gesproken tekst in C++ in Linux met behulp van de spraak-SDK

Snelstartgidsen zijn ook beschikbaar voor [spraakherkenning](quickstart-cpp-linux.md).

In dit artikel maakt u een C++ console-toepassing voor Linux (Ubuntu 16.04, 18.04 Ubuntu, Debian 9). U gebruikt de Cognitive Services [spraak SDK](speech-sdk.md) te bootsen spraak vanuit tekst in realtime en de gesproken tekst te spelen op uw PC's spreker. De toepassing is gemaakt met de [Speech SDK voor Linux](https://aka.ms/csspeech/linuxbinary) en de C++ compiler van uw Linux-distributie (bijvoorbeeld `g++`).

## <a name="prerequisites"></a>Vereisten

U moet een abonnementssleutel Speech Services voor het voltooien van deze Quickstart. U kunt er gratis een krijgen. Zie [de Speech Services gratis uitproberen](get-started.md) voor meer informatie.

## <a name="install-speech-sdk"></a>Speech SDK installeren

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Speech SDK van Cognitive Services is `1.5.1`.

De Speech SDK voor Linux kan worden gebruikt om zowel 32-bits als 64-bits toepassingen te compileren. De vereiste bibliotheken en headerbestanden kunnen als een TAR-bestand worden gedownload vanuit https://aka.ms/csspeech/linuxbinary.

Download en installeer de SDK als volgt:

1. Zorg ervoor dat de SDK-afhankelijkheden zijn geïnstalleerd.

   * Op Ubuntu:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

   * Op Debian 9:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.2 libasound2 wget
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

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-linux/helloworld.cpp#code)]

1. Vervang de tekenreeks in dit nieuwe bestand `YourSubscriptionKey` met uw abonnementssleutel Speech Services.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="build-the-app"></a>De app bouwen

> [!NOTE]
> Zorg ervoor dat u de onderstaande opdrachten als _één opdrachtregel_ invoert. De eenvoudigste manier om dat te doen is om de opdracht te kopiëren met behulp van de knop **Kopiëren** naast elke opdracht en deze vervolgens bij de shell-prompt te plakken.

* Voer op een **x64** (64-bits) systeem de volgende opdracht uit om de toepassing te compileren.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libasound.so.2
  ```

* Voer op een **x86** (32-bits) systeem de volgende opdracht uit om de toepassing te compileren.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libasound.so.2
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

1. In het consolevenster wordt een prompt weergegeven, waarin u naar het type tekst wordt gevraagd. Typ een paar woorden of een zin. De tekst die u hebt getypt, is verzonden naar de Services voor spraak en gemaakt naar-spraak, die op de spreker afgespeeld.

   ```text
   Type some text that you want to speak...
   > hello
   Speech synthesized to speaker for text [hello]
   Press enter to exit...
   ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C++-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Spraakstijlen aanpassen](how-to-customize-voice-font.md)
- [Record voice-voorbeelden](record-custom-voice-samples.md)
