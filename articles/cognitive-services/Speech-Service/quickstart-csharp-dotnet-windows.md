---
title: 'Snelstart: Gesproken tekst herkennen in C# onder .NET Framework in Windows met behulp van de Speech SDK van Cognitive Services'
titleSuffix: Microsoft Cognitive Services
description: Gesproken tekst leren herkennen in C# onder .NET Framework in Windows met behulp van de Speech SDK van Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 32b484451c4ee2264c25cca92b1d03d91b955a29
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47053993"
---
# <a name="quickstart-recognize-speech-in-c-under-net-framework-on-windows-by-using-the-speech-sdk"></a>Snelstart: Gesproken tekst herkennen in C# onder .NET Framework in Windows met behulp van de Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een C#-consoletoepassing voor .NET Framework voor Windows met behulp van de [Speech SDK](speech-sdk.md). U gaat in realtime spraak naar tekst transcriberen via de microfoon van uw pc. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

## <a name="prerequisites"></a>Vereisten

U hebt een abonnementssleutel voor de Speech-service nodig om deze snelstart uit te voeren. U kunt er een gratis downloaden. Zie [Probeer de Speech-service gratis uit](get-started.md) voor meer informatie.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open `Program.cs` en vervang daarin alle code door het volgende.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel van de Speech-service.

1. Vervang ook de tekenreeks `YourServiceRegion` door de [regio](regions.md) die gekoppeld is aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen aan het project op.

## <a name="build-and-run-the-app"></a>De app compileren en uitvoeren

1. Compileer de toepassing. Selecteer in de menubalk **Build** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met Build Solution-optie gemarkeerd](media/sdk/qs-csharp-dotnet-windows-08-build.png "Geslaagde build")

1. Start de toepassing. Selecteer in de menubalk **Debug** > **Start Debugging** of druk op **F5**.

    ![Schermafbeelding van Visual Studio-toepassing met Start Debugging-optie gemarkeerd](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Start foutopsporing door de app")

1. Een consolevenster wordt weergegeven, waarin u wordt gevraagd iets te zeggen. Spreek een Engelse woordgroep of zin in. Uw stem wordt verzonden naar de Speech-service en getranscribeerd naar tekst, die in hetzelfde venster wordt weergegeven.

    ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Console-uitvoer na geslaagde herkenning")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek naar dit voorbeeld in de map `quickstart/csharp-dotnet-windows`.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Intents van gesproken inhoud herkennen met behulp van de Speech SDK voor C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Zie ook

- [Spraak vertalen](how-to-translate-speech-csharp.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
