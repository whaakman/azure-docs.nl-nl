---
title: 'Snelstartgids: Spraakherkenning in C++ op Linux met behulp van de Cognitive Services Speech SDK herkennen'
titleSuffix: Microsoft Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in C++ in Linux met behulp van de Cognitive Services spraak-SDK
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: a9a5a6b3c3f4831844083d082ba5b65cd900ec02
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281606"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-using-the-speech-sdk"></a>Snelstartgids: Herkennen gesproken tekst in C++ in Linux met behulp van de spraak-SDK

In dit artikel leert u hoe u een C++-consoletoepassing maken in Linux (Ubuntu 16.04) met behulp van de Cognitive Services Speech SDK spraak naar tekst te transcriberen.

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [de spraakservice gratis uitproberen](get-started.md).
* Een Ubuntu-16.04-PC met een microfoon werken.
* Om pakketten te installeren die nodig zijn voor het bouwen en uitvoeren van dit voorbeeld voert u het volgende:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="get-the-speech-sdk"></a>De spraak-SDK ophalen

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Cognitive Services Speech SDK is `0.5.0`.

De Cognitive Services spraak-SDK voor Linux is beschikbaar voor het bouwen van 64-bits en 32-bits toepassingen.
Kunnen de vereiste bestanden worden gedownload als een tar-bestand van https://aka.ms/csspeech/linuxbinary.
Download en installeer de SDK als volgt:

1. Kies een map (absoluut pad) waar u wilt plaatsen van de binaire bestanden voor spraak-SDK en -koppen.
   Kies bijvoorbeeld het pad `speechsdk` onder de basismap:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. De map niet maken als deze nog niet bestaat:

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Downloaden en uitpakken van de `.tar.gz` archief met de binaire bestanden voor spraak-SDK:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. De inhoud van de map op het hoogste niveau van de uitgepakte pakket valideren:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Kennisgevingen van derden en licentiebestanden, moet worden weergegeven, evenals een `include` Active directory voor kopteksten en een `lib` Active directory voor bibliotheken.

   [!include[Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Voeg de volgende code in een bestand met de naam `helloworld.cpp`:

  [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` met de abonnementssleutel van uw.

1. Vervang de tekenreeks `YourServiceRegion` met de [regio](regions.md) die zijn gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="building"></a>Bouwen

> [!NOTE]
> Zorg ervoor dat u kopieert en plakt u de onderstaande opdrachten build als een _Eén regel_.

* Op een **x64** machine, voer de volgende opdracht om de toepassing te bouwen:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Op een **x86** machine, voer de volgende opdracht om de toepassing te bouwen:

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

1. Het configureren van het laadprogramma bibliotheekpad om te verwijzen naar de spraak-SDK-bibliotheek configureren.

   * Op een **x64** machine, worden uitgevoerd:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * Op een **x86** machine, worden uitgevoerd:

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Voer de toepassing als volgt uit:

   ```sh
   ./helloworld
   ```

1. Hier ziet u uitvoer die lijkt op dit:

   ```text
   Say something...
   We recognized: What's the weather
   ```

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek in dit voorbeeld in de `quickstart/cpp-linux` map.

## <a name="next-steps"></a>Volgende stappen

* Ga naar de [pagina met codevoorbeelden](samples.md) voor meer voorbeelden.
