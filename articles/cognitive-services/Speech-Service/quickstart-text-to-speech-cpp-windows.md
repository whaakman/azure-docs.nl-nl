---
title: 'Quickstart: Spraak, C++ (Windows) - spraakservices bootsen'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het nabootsen van gesproken tekst in C++ op Windows-bureaublad met behulp van de spraak-SDK
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: yinhew
ms.openlocfilehash: dfad425d6da2e3dabec138ef4f108957ebea7ee9
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67602919"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Quickstart: Nabootsen van gesproken tekst in C++ op Windows met behulp van de spraak-SDK

Snelstartgidsen zijn ook beschikbaar voor [spraakherkenning](quickstart-cpp-windows.md) en [spraakomzetting](quickstart-translate-speech-cpp-windows.md).

In dit artikel maakt u een C++-consoletoepassing voor Windows. U gebruikt de Cognitive Services [spraak SDK](speech-sdk.md) te bootsen spraak vanuit tekst in realtime en de gesproken tekst te spelen op uw PC's spreker. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

De functie die wordt beschreven in dit artikel is beschikbaar via [spraak SDK 1.5.0](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/1.5.0).

Zie voor een volledige lijst met talen/stemmen beschikbaar zijn voor spraaksynthese [taalondersteuning](language-support.md#text-to-speech).

## <a name="prerequisites"></a>Vereisten

U moet een abonnementssleutel Speech Services voor het voltooien van deze Quickstart. U kunt er gratis een krijgen. Zie [de Speech Services gratis uitproberen](get-started.md) voor meer informatie.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open het bronbestand *helloworld.cpp*. Vervang alle code onder de eerste include-instructie (`#include "stdafx.h"` of `#include "pch.h"`) door het volgende:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die gekoppeld is aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Kies in de menubalk **Build** > **Build Solution**. De code moet zonder fouten worden gecompileerd.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-cpp-windows-06-build.png)

1. Start de toepassing. Kies in de menubalk **Debug** > **Start Debugging** of druk op **F5**.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Een consolevenster wordt weergegeven, waarin u naar het type tekst wordt gevraagd. Typ een paar woorden of een zin. De tekst die u hebt getypt, is verzonden naar de Services voor spraak en gemaakt naar-spraak, die op de spreker afgespeeld.

   ![Schermopname van console-uitvoer na geslaagde synthese](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Volgende stappen

Aanvullende voorbeelden, zoals het opslaan van spraak naar een geluidsbestand, zijn beschikbaar op GitHub.

> [!div class="nextstepaction"]
> [C++-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Spraakstijlen aanpassen](how-to-customize-voice-font.md)
- [Record voice-voorbeelden](record-custom-voice-samples.md)
