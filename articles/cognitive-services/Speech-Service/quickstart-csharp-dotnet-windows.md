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
ms.openlocfilehash: a3990bfac9199fadeff0573d7547d7e5bc551ece
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57838609"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Quickstart: Spraak herkennen met de spraak-SDK voor .NET Framework (Windows)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Gebruik deze handleiding om een spraakconsoletoepassing voor -naar-tekst te maken met .NET Framework voor Windows en de Speech-SDK. Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.

Voor een snelle demonstratie (zonder het bouwen van de Visual Studio-project zelf, zoals hieronder weergegeven):

De nieuwste [Cognitive Services Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk) vanuit Github.

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

1. Zoek en vervang de tekenreeks `YourSubscriptionKey` met uw abonnementssleutel Speech Services.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld. Als u bijvoorbeeld gebruikmaakt van de gratis proefversie, dan is `westus` de regio.

1. Sla de wijzigingen aan het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Selecteer in de menubalk **Build** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-csharp-dotnet-windows-08-build.png "Geslaagde build")

1. Selecteer in de menubalk **Debug** > **Start Debugging** of druk op **F5** om de toepassing te starten.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Start foutopsporing van de app")

1. Er verschijnt een consolevenster en u wordt gevraagd te spreken. Zeg iets in het Engels. Uw stem is verzonden naar de Services voor spraak en getranscribeerde tekst in realtime. Het resultaat wordt weergegeven in de console.

    ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Console-uitvoer na geslaagde herkenning")

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
