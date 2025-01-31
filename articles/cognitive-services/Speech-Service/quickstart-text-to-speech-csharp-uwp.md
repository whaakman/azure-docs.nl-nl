---
title: 'Quickstart: Spraak, nabootsen C# (UWP) - spraakservices'
titleSuffix: Azure Cognitive Services
description: In dit artikel maakt u in C# een UWP-app (Universal Windows Platform) met behulp van de Speech-SDK van Cognitive Services. U bootsen spraak vanuit tekst in realtime op de spreker van uw apparaat. De toepassing is gemaakt met het Speech-SDK NuGet-pakket en Microsoft Visual Studio 2017.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: c11fbff883d2699bdd0a107fc462524d92ee410d
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467444"
---
# <a name="quickstart-synthesize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Quickstart: Nabootsen van gesproken tekst in een UWP-app met behulp van de spraak-SDK

Snelstartgidsen zijn ook beschikbaar voor [spraakherkenning](quickstart-csharp-uwp.md), [spraakomzetting](quickstart-translate-speech-uwp.md) en [stem op de eerste virtuele assistent](quickstart-virtual-assistant-csharp-uwp.md).

In dit artikel ontwikkelt u in C# een UWP-app (Universal Windows Platform, Windows-versie 1709 of hoger) met behulp van de [Speech-SDK](speech-sdk.md) van Cognitive Services. Het programma wordt spraak vanuit tekst in realtime op van uw apparaat spreker bootsen. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

> [!NOTE]
> Het Universal Windows Platform stelt u in staat om apps te ontwikkelen die kunnen worden uitgevoerd op elk apparaat dat ondersteuning biedt voor Windows 10, met inbegrip van pc's, Xbox, Surface Hub en andere apparaten.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. De gebruikersinterface van de toepassing wordt gedefinieerd met behulp van XAML. Open `MainPage.xaml` in Solution Explorer. Voeg in de XAML-weergave van de ontwerpfunctie het volgende XAML-fragment toe aan de tag Grid (tussen `<Grid>` en `</Grid>`).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Open het code-behind-bronbestand `MainPage.xaml.cs` (u vindt dit gegroepeerd onder `MainPage.xaml`). Vervang alle code in het bestand door de volgende code.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` in de handler `Speak_ButtonClicked` in dit bestand door uw abonnementssleutel.

1. Vervang in de handler `Speak_ButtonClicked` de tekenreeks `YourServiceRegion` door de [regio](regions.md) die is gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla alle wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Selecteer in de menubalk **Build** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-csharp-uwp-08-build.png "Geslaagde build")

1. Start de toepassing. Selecteer in de menubalk **Debug** > **Start Debugging** of druk op **F5**.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-uwp-09-start-debugging.png "Start foutopsporing van de app")

1. Voer tekst in het tekstvak en klikt u op **Speak**. De tekst is verzonden naar de Services spraak en gemaakt naar-spraak, die op de spreker afgespeeld.

    ![Schermopname van gebruikersinterface van spraak synthese](media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Spraakstijlen aanpassen](how-to-customize-voice-font.md)
- [Record voice-voorbeelden](record-custom-voice-samples.md)
