---
title: 'Quickstart: Speech herkennen, C# (UWP)-spraak service'
titleSuffix: Azure Cognitive Services
description: In dit artikel maakt u in C# een UWP-app (Universal Windows Platform) met behulp van de Speech-SDK van Cognitive Services. U gaat in realtime spraak naar tekst transcriberen via de microfoon van uw apparaat. De toepassing is gemaakt met het Speech-SDK NuGet-pakket en Microsoft Visual Studio 2017.
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/23/2019
ms.author: lisaweixu
ms.custom: seodec18
ms.openlocfilehash: 5505288a494aefe165e829f53c1869f4cc5f176f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559427"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Snelstartgids: Gesproken tekst herkennen in een UWP-app met behulp van de Speech-SDK

Quick starts zijn ook beschikbaar voor [tekst-naar-spraak](quickstart-text-to-speech-csharp-uwp.md)-, [spraak-Vertaal](quickstart-translate-speech-uwp.md) -en [spraak-eerste virtuele assistent](quickstart-virtual-assistant-csharp-uwp.md).

Kies indien gewenst een andere programmeer taal en/of-omgeving:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel ontwikkelt u in C# een UWP-app (Universal Windows Platform, Windows-versie 1709 of hoger) met behulp van de [Speech-SDK](speech-sdk.md) van Cognitive Services. Het programma gaat in realtime spraak naar tekst transcriberen via de microfoon van uw apparaat. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

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

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Open het code-behind-bronbestand `MainPage.xaml.cs` (u vindt dit gegroepeerd onder `MainPage.xaml`). Vervang alle code in het bestand door de volgende code.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` in de handler `SpeechRecognitionFromMicrophone_ButtonClicked` in dit bestand door uw abonnementssleutel.

1. Vervang in de handler `SpeechRecognitionFromMicrophone_ButtonClicked` de tekenreeks `YourServiceRegion` door de [regio](regions.md) die is gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla alle wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Selecteer in de menubalk **Build** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-csharp-uwp-08-build.png "Geslaagde build")

1. Start de toepassing. Selecteer in de menubalk **Debug** > **Start Debugging** of druk op **F5**.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-uwp-09-start-debugging.png "Start foutopsporing van de app")

1. Er verschijnt een venster. Selecteer **Enable Microphone** en bevestig dat de app toegang mag hebben tot de microfoon.

    ![Schermafbeelding van verzoek om toegang](media/sdk/qs-csharp-uwp-10-access-prompt.png "Start foutopsporing van de app")

1. Selecteer **Speech recognition with microphone input** en zeg een Engelse woordgroep of zin in de microfoon van uw apparaat. Uw spraak wordt verzonden naar de spraak Services en getranscribeerd naar tekst, die wordt weer gegeven in het venster.

    ![Schermafbeelding van gebruikersinterface voor spraakherkenning](media/sdk/qs-csharp-uwp-11-ui-result.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Spraak vertalen](how-to-translate-speech-csharp.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
