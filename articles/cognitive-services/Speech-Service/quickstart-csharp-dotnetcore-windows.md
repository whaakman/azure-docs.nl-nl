---
title: 'Quickstart: Speech herkennen, C# (.net core)-Speech Service'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het herkennen van C# spraak in onder .net core in Windows of macOS met behulp van de Speech SDK
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 3ba82f2bb3d8325b7cf77f357471dc4f25382ff6
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559519"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-core"></a>Quickstart: Spraak herkennen met de Speech-SDK voor .NET Core

Quick starts zijn ook beschikbaar voor [tekst naar spraak](quickstart-text-to-speech-dotnetcore.md) en [spraak omzetting](quickstart-translate-speech-dotnetcore-windows.md).

Kies indien gewenst een andere programmeer taal en/of-omgeving:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een C# console toepassing voor .net core in Windows of macOS met behulp van de [SDK](speech-sdk.md)van Cognitive Services speech. Doel is om realtime spraak naar tekst om te zetten vanuit de microfoon van uw pc. De toepassing is gemaakt met het [NuGet-pakket van de Speech SDK](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

> [!NOTE]
> .NET Core is een open-source, platformoverschrijdend .NET-platform waarmee de [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)-specificatie wordt geïmplementeerd.

U hebt een abonnement op spraak services nodig om deze Quick Start te volt ooien. U kunt er gratis een krijgen. Zie [de spraak services gratis uitproberen](get-started.md) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [.NET Core-SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open `Program.cs` en vervang daarin alle code door het volgende.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang ook de tekenreeks `YourServiceRegion` door de [regio](regions.md) die is gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Kies in de menubalk **Build** > **Build Solution**. De code moet zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Oplossing bouwen gemarkeerd](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Geslaagde build")

1. Start de toepassing. Selecteer in de menubalk **Fouten opsporen** > **Foutopsporing starten** of druk op **F5**.

    ![Schermafbeelding van Visual Studio-toepassing met optie Foutopsporing starten gemarkeerd](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Start foutopsporing voor de app")

1. Een consolevenster wordt weergegeven, waarin u wordt gevraagd iets te zeggen. Spreek een Engelse woordgroep of zin in. Uw spraak wordt verzonden naar de spraak Services en getranscribeerd naar tekst, die in hetzelfde venster wordt weer gegeven.

    ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Console-uitvoer na geslaagde herkenning")

## <a name="next-steps"></a>Volgende stappen

Op GitHub vindt u aanvullende voorbeelden, zoals hoe u spraak kunt lezen vanuit een audiobestand.

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
