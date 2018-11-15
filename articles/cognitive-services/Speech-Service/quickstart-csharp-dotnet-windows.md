---
title: 'Snelstart: spraak herkennen en transcriberen met .NET Framework (Windows) - Speech Service'
titleSuffix: Azure Cognitive Services
description: Gebruik deze handleiding om een spraakconsoletoepassing voor -naar-tekst te maken met .NET Framework voor Windows en de Speech-SDK. Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 11/05/2018
ms.author: wolfma
ms.openlocfilehash: 502f59c88808480f26e6ea5a6d2c5b362c78869b
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683490"
---
# <a name="quickstart-recognize-and-transcribe-speech-using-the-speech-sdk-and-net-framework-windows"></a>Snelstart: spraak herkennen en transcriberen met de Speech-SDK en .NET Framework (Windows)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Gebruik deze handleiding om een spraakconsoletoepassing voor -naar-tekst te maken met .NET Framework voor Windows en de Speech-SDK. Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.

Deze snelstartgids vereist een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) en dat Microsoft Speech is ingeschakeld. Als u geen account hebt, kunt u de [gratis proefversie](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

Als u dit project wilt voltooien, hebt u het volgende nodig:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een abonnementssleutel voor Speech Service
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

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
De code is beschikbaar in de map `quickstart/csharp-dotnet-windows`.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Intenties van gesproken inhoud herkennen met behulp van de Speech SDK voor C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Zie ook

- [Spraak vertalen](how-to-translate-speech-csharp.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
