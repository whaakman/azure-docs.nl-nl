---
title: 'Quickstart: Speech herkennen, .NET Framework (Windows)-spraak service'
titleSuffix: Azure Cognitive Services
description: Gebruik deze handleiding om een spraakconsoletoepassing voor -naar-tekst te maken met .NET Framework voor Windows en de Speech-SDK. Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 47f955734070be4adfe7f58da98265b976e643d5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68554157"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Quickstart: Spraak herkennen met de spraak-SDK voor .NET Framework (Windows)

Quick starts zijn ook beschikbaar voor [tekst naar spraak](quickstart-text-to-speech-dotnet-windows.md) en [spraak omzetting](quickstart-translate-speech-dotnetframework-windows.md).

Kies indien gewenst een andere programmeer taal en/of-omgeving:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Gebruik deze handleiding om een spraakconsoletoepassing voor -naar-tekst te maken met .NET Framework voor Windows en de Speech-SDK. Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.

Voor een snelle demonstratie (zonder het Visual Studio-project zelf te bouwen zoals hieronder wordt weer gegeven):

Down load de nieuwste [Cognitive Services Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk) -voor beelden van github.

## <a name="prerequisites"></a>Vereisten

Als u dit project wilt voltooien, hebt u het volgende nodig:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een abonnementssleutel voor de Speech Service. [Gratis downloaden](get-started.md).
* Toegang tot de microfoon van uw computer

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open `Program.cs` en vervang de automatisch gegenereerde code door dit voorbeeld:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Zoek de teken reeks `YourSubscriptionKey` en vervang deze door uw abonnement sleutel voor spraak Services.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld. Als u bijvoorbeeld gebruikmaakt van de gratis proefversie, dan is `westus` de regio.

1. Sla de wijzigingen aan het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Selecteer in de menubalk **Build** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-csharp-dotnet-windows-08-build.png "Geslaagde build")

1. Selecteer in de menubalk **Debug** > **Start Debugging** of druk op **F5** om de toepassing te starten.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Start foutopsporing van de app")

1. Er verschijnt een consolevenster en u wordt gevraagd te spreken. Zeg iets in het Engels. Uw spraak wordt verzonden naar de spraak Services en getranscribeerd naar tekst in real-time. Het resultaat wordt weergegeven in de console.

    ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Console-uitvoer na geslaagde herkenning")

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
