---
title: 'Snelstartgids: Gesproken tekst herkennen in C# in een UWP-app met behulp van de Speech SDK van Cognitive Services'
titleSuffix: Microsoft Cognitive Services
description: Leer hoe u gesproken tekst herkent in een UWP-app met behulp van de Speech SDK van Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 2eb6bda7066a01e5532fe7c0e20b0ee13f4289b6
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47432984"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Snelstartgids: Gesproken tekst herkennen in een UWP-app met behulp van de Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u in C# een UWP-app (Universal Windows Platform) met behulp van de [Speech SDK](speech-sdk.md) van Cognitive Services. U gaat in realtime spraak naar tekst transcriberen via de microfoon van uw apparaat. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

> [!NOTE]
> Het Universal Windows Platform stelt u in staat om apps te ontwikkelen die kunnen worden uitgevoerd op elk apparaat dat ondersteuning biedt voor Windows 10, met inbegrip van pc's, Xbox, Surface Hub en andere apparaten.

## <a name="prerequisites"></a>Vereisten

U hebt een abonnementssleutel voor de Speech-service nodig om deze snelstartgids uit te voeren. U kunt er een gratis downloaden. Zie [Speech-service gratis uitproberen](get-started.md) voor meer informatie.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Start Visual Studio 2017.

1. Zorg ervoor dat de workload **Universal Windows Platform development** beschikbaar is. Kies **Tools** > **Get Tools and Features** in de menubalk van Visual Studio om het installatieprogramma voor Visual Studio te openen. Als deze workload al is ingeschakeld, sluit u het dialoogvenster. 

    ![Schermopname van het installatieprogramma van Visual Studio, met het tabblad Workloads gemarkeerd](media/sdk/vs-enable-uwp-workload.png)

    Selecteer anders het vakje naast **.NET cross-platform development** en selecteer **Modify** in de rechterbenedenhoek van het dialoogvenster. De installatie van de nieuwe functie duurt even.

1. Maak een lege, universele Windows-app in C#. Kies eerst **File** > **New** > **Project** in het menu. Vouw in het dialoogvenster **New Project** **Installed** > **Visual C#** > **Windows Universal** uit in het linkerdeelvenster. Selecteer vervolgens **Blank App (Universal Windows)**. Voer als projectnaam *helloworld* in.

    ![Schermafbeelding van het dialoogvenster New Project](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. De Speech SDK vereist dat uw toepassing wordt gecompileerd voor de Windows 10-makersupdate van het najaar of later. Kies in het venster **New Universal Windows Platform Project** dat wordt weergegeven **Windows 10 Fall Creators Update (10.0; Build 16299)** bij **Minimum version**. Selecteer bij **Target version** deze of een latere versie en klik op **OK**.

    ![Schermafbeelding van het venster New Universal Windows Platform Project](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Als u 64-bits Windows gebruikt, kunt u uw build-platform omschakelen naar `x64` via de vervolgkeuzelijst in de werkbalk van Visual Studio. (Met 64-bits Windows kunt u ook 32-bits toepassingen uitvoeren, dus u kunt dit laten staan op `x86` als u dat liever hebt.)

   ![Schermafbeelding van de Visual Studio-werkbalk, met x64 gemarkeerd](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > De Speech SDK biedt alleen ondersteuning voor Intel-compatibele processors. ARM wordt momenteel niet ondersteund.

1. Installeer het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en verwijs ernaar. Klik in Solution Explorer met de rechtermuisknop op de oplossing en selecteer **Manage NuGet Packages for Solution**.

    ![Schermafbeelding van Solution Explorer, met Manage NuGet Packages for Solution gemarkeerd](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. Selecteer in de rechterbovenhoek, in het veld **Package Source**, het pakket **nuget.org**. Zoek het pakket `Microsoft.CognitiveServices.Speech` en installeer het in het project **helloworld**.

    ![Schermafbeelding van het dialoogvenster Manage Packages for Solution](media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png "NuGet-pakket installeren")

1. Ga akkoord met de weergegeven licentie om te beginnen met het installeren van het NuGet-pakket.

    ![Schermafbeelding van het licentievenster](media/sdk/qs-csharp-uwp-06-nuget-license.png "Akkoord gaan met licentie")

1. De volgende regel wordt uitgevoerd in de Package Manager Console.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 1.0.0' to helloworld
   ```

1. Omdat de toepassing de microfoon gebruikt voor spraakinvoer, voegt u **Microphone** toe aan het project. Dubbelklik in Solution Explorer op **Package.appxmanifest** om het manifest van uw toepassing te bewerken. Ga vervolgens naar het tabblad **Capabilities**, selecteer het selectievakje bij **Microphone** en sla de wijzigingen op.

   ![Schermafbeelding van het Visual Studio-toepassingsmanifest, met Capabilities en Microphone gemarkeerd](media/sdk/qs-csharp-uwp-07-capabilities.png)


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

1. Selecteer **Speech recognition with microphone input** en zeg een Engelse woordgroep of zin in de microfoon van uw apparaat. Uw gesproken tekst wordt verzonden naar de Speech-service en getranscribeerd naar tekst, die in hetzelfde venster wordt weergegeven.

    ![Schermafbeelding van gebruikersinterface voor spraakherkenning](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek naar dit voorbeeld in de map `quickstart/csharp-uwp`.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Intenties van gesproken inhoud herkennen met behulp van de Speech SDK voor C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Zie ook

- [Spraak vertalen](how-to-translate-speech-csharp.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
