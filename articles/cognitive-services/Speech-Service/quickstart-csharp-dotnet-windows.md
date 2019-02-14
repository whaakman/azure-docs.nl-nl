---
title: 'Snelstartgids: Spraak herkennen, .NET Framework (Windows) - Speech Services'
titleSuffix: Azure Cognitive Services
description: Gebruik deze handleiding om een spraakconsoletoepassing voor -naar-tekst te maken met .NET Framework voor Windows en de Speech-SDK. Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: wolfma
ms.openlocfilehash: 8795dd7f1f3a2eb30932b2af1e13b4ea46c8c07c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55877810"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Quickstart: Spraak herkennen met de spraak-SDK voor .NET Framework (Windows)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Gebruik deze handleiding om een spraakconsoletoepassing voor -naar-tekst te maken met .NET Framework voor Windows en de Speech-SDK. Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.

## <a name="prerequisites"></a>Vereisten

Als u dit project wilt voltooien, hebt u het volgende nodig:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een abonnementssleutel voor de Speech Service. [Gratis downloaden](get-started.md).
* Toegang tot de microfoon van uw computer

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open `Program.cs` en vervang de automatisch gegenereerde code door dit voorbeeld:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` door de abonnementssleutel van Speech Service.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld. Als u bijvoorbeeld gebruikmaakt van de gratis proefversie, dan is `westus` de regio.

1. Sla de wijzigingen aan het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Selecteer in de menubalk **Build** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-csharp-dotnet-windows-08-build.png "Geslaagde build")

1. Selecteer in de menubalk **Debug** > **Start Debugging** of druk op **F5** om de toepassing te starten.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Start foutopsporing van de app")

1. Er verschijnt een consolevenster en u wordt gevraagd te spreken. Zeg iets in het Engels. Uw gesproken tekst wordt verzonden naar Speech Service en in realtime getranscribeerd naar tekst. Het resultaat wordt weergegeven in de console.

    ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Console-uitvoer na geslaagde herkenning")

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
