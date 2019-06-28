---
title: 'Quickstart: Spraak, nabootsen C# (.NET Core) - spraakservices'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het nabootsen van gesproken tekst in C# onder .NET Core in Windows met behulp van de spraak-SDK
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: 7b4a018e38ca625e38dc1658a95d3ce0e677f711
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467423"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-core"></a>Quickstart: Bootsen spraak met de spraak-SDK voor .NET Core

Snelstartgidsen zijn ook beschikbaar voor [spraakherkenning](quickstart-csharp-dotnetcore-windows.md) en [spraakomzetting](quickstart-translate-speech-dotnetcore-windows.md).

In dit artikel maakt u een C#-consoletoepassing voor .NET Core op Windows met behulp van de [Speech SDK](speech-sdk.md) van Cognitive Services. U bootsen spraak vanuit tekst in realtime op uw PC's spreker. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

> [!NOTE]
> .NET Core is een open-source, platformoverschrijdend .NET-platform waarmee de [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)-specificatie wordt geïmplementeerd.

U moet een abonnementssleutel Speech Services voor het voltooien van deze Quickstart. U kunt er gratis een krijgen. Zie [de Speech Services gratis uitproberen](get-started.md) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [.NET Core-SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open `Program.cs` en vervang daarin alle code door het volgende.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang ook de tekenreeks `YourServiceRegion` door de [regio](regions.md) die is gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Kies in de menubalk **Build** > **Build Solution**. De code moet zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Oplossing bouwen gemarkeerd](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Geslaagde build")

1. Start de toepassing. Selecteer in de menubalk **Fouten opsporen** > **Foutopsporing starten** of druk op **F5**.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Start foutopsporing van de app")

1. Een consolevenster wordt weergegeven, waarin u naar het type tekst wordt gevraagd. Typ een paar woorden of een zin. De tekst die u hebt getypt, is verzonden naar de Services voor spraak en gemaakt naar-spraak, die op de spreker afgespeeld.

    ![Schermopname van console-uitvoer na geslaagde synthese](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Console-uitvoer na geslaagde synthese")

## <a name="next-steps"></a>Volgende stappen

Aanvullende voorbeelden, zoals het nabootsen van spraak-naar-een geluidsbestand, zijn beschikbaar op GitHub.

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Spraakstijlen aanpassen](how-to-customize-voice-font.md)
- [Record voice-voorbeelden](record-custom-voice-samples.md)
