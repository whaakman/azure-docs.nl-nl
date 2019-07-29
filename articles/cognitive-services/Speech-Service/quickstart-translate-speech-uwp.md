---
title: 'Quickstart: Vertaal spraak, C# (UWP)-Speech Service'
titleSuffix: Azure Cognitive Services
description: In deze quickstart maakt u een eenvoudige UWP-toepassing (Universeel Windows-platform) om spraak van gebruikers vast te leggen, deze om te zetten in een andere taal en de tekst uit te voeren naar de opdrachtregel. Deze handleiding is bedoeld voor Windows-gebruikers.
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 07/23/2019
ms.author: erhopf
ms.openlocfilehash: 71f8ecfaa11538cf43284d2aafff61cbf9655c0e
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607761"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c-uwp"></a>Quickstart: Spraak omzetten met de Speech SDK voor C# (UWP)

Quick starts zijn ook beschikbaar voor de virtuele assistent [spraak naar tekst](quickstart-csharp-uwp.md), [tekst naar spraak](quickstart-text-to-speech-csharp-uwp.md) en spraak [-eerste](quickstart-virtual-assistant-csharp-uwp.md).

In deze quickstart maakt u een eenvoudige UWP-toepassing (Universeel Windows-platform) waarmee spraak van gebruikers via de microfoon van de computer wordt vastgelegd, waarna de spraak wordt omgezet en de omgezette tekst in realtime wordt getranscribeerd naar de opdrachtregel. Deze toepassing is ontworpen om te worden uitgevoerd op 64-bits Windows en is gebouwd met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en micro soft Visual Studio 2017 of hoger.

Zie [Taalondersteuning](language-support.md) voor een volledige lijst met talen die voor spraakomzetting beschikbaar zijn.

> [!NOTE]
> UWP stelt u in staat om apps te ontwikkelen die kunnen worden uitgevoerd op elk apparaat dat ondersteuning biedt voor Windows 10, met inbegrip van pc's, Xbox, Surface Hub en andere apparaten.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) of hoger
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. De gebruikersinterface van de toepassing wordt gedefinieerd met behulp van XAML. Open `MainPage.xaml` in Solution Explorer. Voeg in de XAML-weergave van de ontwerpfunctie het volgende XAML-fragment toe (tussen `<Grid>` en `</Grid>`).

    [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Open het code-behind-bronbestand `MainPage.xaml.cs` (u vindt dit gegroepeerd onder `MainPage.xaml`). Vervang alle code in het bestand door de volgende code.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` in de handler `SpeechTranslationFromMicrophone_ButtonClicked` in dit bestand door uw abonnementssleutel.

1. Vervang in de handler `SpeechTranslationFromMicrophone_ButtonClicked` de tekenreeks `YourServiceRegion` door de [regio](regions.md) die is gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla alle wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Selecteer in de menubalk **Build** > **Build Solution**. De code moet nu zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-csharp-uwp-08-build.png "Geslaagde build")

1. Start de toepassing. Selecteer in de menubalk **Debug** > **Start Debugging** of druk op **F5**.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-csharp-uwp-09-start-debugging.png "Start foutopsporing van de app")

1. Er verschijnt een venster. Selecteer **Enable Microphone** en bevestig dat de app toegang mag hebben tot de microfoon.

    ![Schermafbeelding van verzoek om toegang](media/sdk/qs-csharp-uwp-10-access-prompt.png "Start foutopsporing van de app")

1. Selecteer **Speech recognition with microphone input** en zeg een Engelse woordgroep of zin in de microfoon van uw apparaat. Uw gesproken tekst wordt verzonden naar de Speech-service en getranscribeerd naar tekst, die in hetzelfde venster wordt weergegeven.

    ![Schermafbeelding van gebruikersinterface voor spraakherkenning](media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
