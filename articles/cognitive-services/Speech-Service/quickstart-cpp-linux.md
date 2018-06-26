---
title: Snelstartgids voor spraak-SDK voor C++- en Linux | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de SDK spraak met Linux en C++ in cognitieve Services.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: cee70ba585f93dda3249fc5b39f25fb613b57a45
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753654"
---
# <a name="quickstart-for-c-and-linux"></a>Quick Start voor C++- en Linux

De huidige versie van de cognitieve Services spraak SDK is `0.4.0`.

De cognitieve Services spraak SDK voor Linux is beschikbaar voor het bouwen van 64-bits en 32-bits toepassingen. Kunnen de vereiste bestanden worden gedownload als een tar-bestand van https://aka.ms/csspeech/linuxbinary.

> [!NOTE]
> Als u een Quick Start voor C++ en Windows zoekt, gaat u [hier](quickstart-cpp-windows.md).
> Als u een Quick Start voor C#- en Windows zoekt, gaat u [hier](quickstart-csharp-windows.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Deze instructies wordt ervan uitgegaan dat u op Ubuntu 16.04 op een PC (x86 of x64) uitvoert.
> Op een andere Ubuntu-versie of een ander distributiepunt voor Linux hebt u aan te passen aan de vereiste stappen.

## <a name="prerequisites"></a>Vereisten

[!include[Ubuntu Prerequisites](includes/ubuntu1604-prerequisites.md)]

## <a name="getting-the-binary-package"></a>Ophalen van het binaire pakket

[!include[License Notice](includes/license-notice.md)]

1. Kies een map (absoluut pad) waar u wilt plaatsen van de binaire bestanden voor spraak-SDK en -koppen.
   Bijvoorbeeld, kies het pad `speechsdk` onder de basismap:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Maken van de map als deze nog niet bestaat:

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

   Kennisgeving van derden en licentiebestanden, moet worden weergegeven, evenals een `include` map voor headers en een `lib` map voor bibliotheken.

   [!include[Linux Binary Archive Content](includes/linuxbinary-content.md)]

## <a name="sample-code"></a>Voorbeeldcode

De volgende code herkent Engelse spraak van de microfoon.
Plaats deze in een bestand met de naam `quickstart-linux.cpp`:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Linux/quickstart-linux/quickstart-linux.cpp#code)]

Vervang de abonnementssleutel in de code door degene die u hebt verkregen.

## <a name="building"></a>Bouwen

> [!NOTE]
> Zorg ervoor dat u Kopieer en plak de onderstaande opdrachten build als een _enkele lijn_.

* Voer de volgende opdracht voor het bouwen van de toepassing op een x64 machine:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Voer de volgende opdracht voor het bouwen van de toepassing op een x86 machine:

  ```sh
  g++ quickstart-linux.cpp -o quickstart-linux -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="running"></a>Actief

Om de toepassing uitvoert, moet u de lader bibliotheekpad om te verwijzen naar de spraak SDK-bibliotheek configureren.

* Op een x64 machine uitvoeren:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
  ```

* Op een x86 machine uitvoeren:

  ```sh
  export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
  ```

Voer de toepassing als volgt:

```sh
./quickstart-linux
```

Als alles goed gaat, ziet u uitvoer is vergelijkbaar met het volgende:

```text
Say something...
We recognized: What's the weather
```

## <a name="downloading-the-sample"></a>Het voorbeeld downloaden

Zie voor de meest recente set steekproeven van het [cognitieve Services spraak SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Volgende stappen

* Ga naar de [pagina voorbeelden](samples.md) voor aanvullende voorbeelden.
