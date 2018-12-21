---
title: 'Snelstartgids: Spraak herkennen, .NET Core (Windows) - Speech Services'
titleSuffix: Azure Cognitive Services
description: Informatie over het herkennen van gesproken tekst in C# onder .NET Core in Windows met behulp van de Speech Service SDK
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 10/12/2018
ms.author: wolfma
ms.openlocfilehash: e82e39eb3fc6c7ebaf4798ad10038bfd2fa9a41b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085481"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-by-using-the-speech-sdk"></a>Snelstartgids: Gesproken tekst herkennen in C# onder .NET Core op Windows met behulp van de Speech-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een C#-consoletoepassing voor .NET Core op Windows met behulp van de [Speech SDK](speech-sdk.md) van Cognitive Services. U gaat in realtime spraak naar tekst transcriberen via de microfoon van uw pc. De toepassing is gemaakt met het [NuGet-pakket van de Speech SDK](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

> [!NOTE]
> .NET Core is een open-source, platformoverschrijdend .NET-platform waarmee de [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard)-specificatie wordt geïmplementeerd.

U hebt een abonnementssleutel voor de Speech-service nodig om deze snelstart uit te voeren. U kunt er gratis een krijgen. Zie [Speech-service gratis uitproberen](get-started.md) voor meer informatie.


## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Start Visual Studio 2017.

1. Zorg ervoor dat de workload voor **.NET platformoverschrijdende ontwikkeling** beschikbaar is. Kies **Extra** > **Hulpmiddelen en onderdelen ophalen** in de menubalk van Visual Studio om het installatieprogramma voor Visual Studio te openen. Als deze workload al is ingeschakeld, sluit u het dialoogvenster.

    ![Schermopname van het Visual Studio-installatieprogramma met het tabblad Workloads gemarkeerd](media/sdk/vs-enable-net-core-workload.png)

    Selecteer anders het vak naast **platformoverschrijdende ontwikkelmogelijkheden van .NET Core** en selecteer **Wijzigen** in de rechterbenedenhoek van het dialoogvenster. De installatie van de nieuwe functie kan even duren.

1. Maak een nieuwe Visual C# .NET Core-consoletoepassing. Vouw in het dialoogvenster **Nieuw project**, vanuit het linkerdeelvenster, **Geïnstalleerd** > **Visual C#** > **.NET Core** uit. Selecteer vervolgens **Consoletoepassing (.NET Core)**. Voer als projectnaam *helloworld* in.

    ![Schermopname van dialoogvenster Nieuw project](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Visual C#-consoletoepassing (.NET Core) maken")

1. Installeer en bekijk het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget). Klik in Solution Explorer met de rechtermuisknop op de oplossing en kies **NuGet-pakketten beheren voor oplossing**.

    ![Schermopname van Solution Explorer, met NuGet-pakketten beheren voor oplossing gemarkeerd](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "NuGet-pakketten beheren voor oplossing")

1. In de rechterbovenhoek selecteert u in het veld **Pakketbron** de optie **nuget.org**. Zoek het pakket `Microsoft.CognitiveServices.Speech` en installeer het in het project **helloworld**.

    ![Schermafbeelding van het dialoogvenster Manage Packages for Solution](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "NuGet-pakket installeren")

1. Ga akkoord met de weergegeven licentie om te beginnen met het installeren van het NuGet-pakket.

    ![Schermafbeelding van het dialoogvenster voor akkoord gaan met licentie](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Licentie accepteren")

Wanneer het pakket is geïnstalleerd, wordt er een bevestiging weergegeven in de Package Manager-console.


## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open `Program.cs` en vervang daarin alle code door het volgende.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang ook de tekenreeks `YourServiceRegion` door de [regio](regions.md) die is gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Kies in de menubalk **Build** > **Build Solution**. De code moet zonder fouten worden gecompileerd.

    ![Schermafbeelding van Visual Studio-toepassing met de optie Oplossing bouwen gemarkeerd](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Geslaagde build")

1. Start de toepassing. Selecteer in de menubalk **Fouten opsporen** > **Foutopsporing starten** of druk op **F5**.

    ![Schermafbeelding van Visual Studio-toepassing met optie Foutopsporing starten gemarkeerd](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Start foutopsporing voor de app")

1. Een consolevenster wordt weergegeven, waarin u wordt gevraagd iets te zeggen. Spreek een Engelse woordgroep of zin in. Uw stem wordt verzonden naar de Speech-service en getranscribeerd naar tekst, die in hetzelfde venster wordt weergegeven.

    ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Console-uitvoer na geslaagde herkenning")

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek naar dit voorbeeld in de map `quickstart/csharp-dotnetcore-windows`.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Intenties van gesproken inhoud herkennen met behulp van de Speech SDK voor C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="see-also"></a>Zie ook

- [Spraak vertalen](how-to-translate-speech-csharp.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
