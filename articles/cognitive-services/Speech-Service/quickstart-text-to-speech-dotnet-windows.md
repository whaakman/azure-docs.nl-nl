---
title: 'Quickstart: Spraak, .NET Framework (Windows) - spraakservices bootsen'
titleSuffix: Azure Cognitive Services
description: Deze handleiding gebruiken om een Text to Speech-consoletoepassing met behulp van .NET framework voor Windows en de SDK van de spraakherkenning te maken. Wanneer u klaar bent, kunt u bootsen spraak vanuit tekst en de stem van de spreker in realtime hoort.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 4/03/2019
ms.author: yinhew
ms.openlocfilehash: a013189e45b1c1c8eeb88d62a718d495c0c415a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60620442"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Quickstart: Bootsen spraak met de spraak-SDK voor .NET Framework (Windows)

Deze handleiding gebruiken om een Text to Speech-consoletoepassing met behulp van .NET framework voor Windows en de SDK van de spraakherkenning te maken. Wanneer u klaar bent, kunt u bootsen spraak vanuit tekst en de stem van de spreker in realtime hoort.

Voor een snelle demonstratie (zonder het bouwen van de Visual Studio-project zelf, zoals hieronder weergegeven):

De nieuwste [Cognitive Services Speech SDK-voorbeelden](https://github.com/Azure-Samples/cognitive-services-speech-sdk) vanuit GitHub.

## <a name="prerequisites"></a>Vereisten

Als u dit project wilt voltooien, hebt u het volgende nodig:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een abonnementssleutel voor de Speech Service. [Gratis downloaden](get-started.md).
* Een spreker (of een headset) beschikbaar.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open `Program.cs` en vervang de automatisch gegenereerde code door dit voorbeeld:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Zoek en vervang de tekenreeks `YourSubscriptionKey` met uw abonnementssleutel Speech Services.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld. Als u bijvoorbeeld gebruikmaakt van de gratis proefversie, dan is `westus` de regio.

1. Sla de wijzigingen aan het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Selecteer in de menubalk **Build** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-csharp-dotnet-windows-08-build.png "Geslaagde build")

1. Selecteer in de menubalk **Debug** > **Start Debugging** of druk op **F5** om de toepassing te starten.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Start foutopsporing van de app")

1. Een consolevenster wordt weergegeven, waarin u naar het type tekst wordt gevraagd. Typ een paar woorden of een zin. De tekst die u hebt getypt, is verzonden naar de Services voor spraak en gemaakt naar-spraak, die op de spreker afgespeeld.

    ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Console-uitvoer na geslaagde herkenning")

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Spraakstijlen aanpassen](how-to-customize-voice-font.md)
- [Record voice-voorbeelden](record-custom-voice-samples.md)
