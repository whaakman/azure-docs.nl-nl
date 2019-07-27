---
title: 'Quickstart: Bewaak spraak, .NET Framework (Windows)-Speech Service'
titleSuffix: Azure Cognitive Services
description: Gebruik deze hand leiding voor het maken van een tekst-naar-spraak-console toepassing met behulp van .NET Framework voor Windows en de spraak-SDK. Wanneer u klaar bent, kunt u spraak van tekst verwerken en de spraak op uw spreker in realtime belui Steren.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: yinhew
ms.openlocfilehash: 3c925e0160a4c57f7e43c3463fc1db7098a6e606
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553605"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Quickstart: Stem de spraak met de Speech SDK voor .NET Framework (Windows)

Quick starts zijn ook beschikbaar voor [spraak herkenning](quickstart-csharp-dotnet-windows.md) en [spraak omzetting](quickstart-translate-speech-dotnetframework-windows.md).

Gebruik deze hand leiding voor het maken van een tekst-naar-spraak-console toepassing met behulp van .NET Framework voor Windows en de spraak-SDK. Wanneer u klaar bent, kunt u spraak van tekst verwerken en de spraak op uw spreker in realtime belui Steren.

Voor een snelle demonstratie (zonder het Visual Studio-project zelf te bouwen zoals hieronder wordt weer gegeven):

Down load de nieuwste [Cognitive Services Speech SDK](https://github.com/Azure-Samples/cognitive-services-speech-sdk) -voor beelden van github.

## <a name="prerequisites"></a>Vereisten

Als u dit project wilt voltooien, hebt u het volgende nodig:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een abonnementssleutel voor de Speech Service. [Gratis downloaden](get-started.md).
* Er is een spreker (of headset) beschikbaar.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open `Program.cs` en vervang de automatisch gegenereerde code door dit voorbeeld:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Zoek de teken reeks `YourSubscriptionKey` en vervang deze door uw abonnement sleutel voor spraak Services.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld. Als u bijvoorbeeld gebruikmaakt van de gratis proefversie, dan is `westus` de regio.

1. Sla de wijzigingen aan het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Selecteer in de menubalk **Build** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-csharp-dotnet-windows-08-build.png "Geslaagde build")

1. Selecteer in de menubalk **Debug** > **Start Debugging** of druk op **F5** om de toepassing te starten.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Start foutopsporing van de app")

1. Er wordt een console venster weer gegeven waarin u wordt gevraagd om tekst te typen. Typ enkele woorden of een zin. De tekst die u hebt getypt, wordt verzonden naar de spraak Services en gesynthesizerd op spraak, die op uw spreker wordt afgespeeld.

    ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Console-uitvoer na geslaagde herkenning")

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Spraak lettertypen aanpassen](how-to-customize-voice-font.md)
- [Spraak voorbeelden vastleggen](record-custom-voice-samples.md)
