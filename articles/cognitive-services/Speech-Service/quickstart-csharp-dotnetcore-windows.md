---
title: 'Snelstartgids: Herkennen gesproken tekst in C# onder .NET Core in Windows met behulp van de Cognitive Services spraak-SDK'
titleSuffix: Microsoft Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in C# onder .NET Core in Windows met behulp van de Cognitive Services spraak-SDK
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: f66917ed49393988a378de0a55525105c5d19b7c
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282371"
---
# <a name="quickstart-recognize-speech-in-c-under-net-core-on-windows-using-the-speech-sdk"></a>Snelstartgids: Herkennen gesproken tekst in C# onder .NET Core in Windows met behulp van de spraak-SDK

In dit artikel leert u hoe u een C#-consoletoepassing maken voor .NET Core in Windows met behulp van de Cognitive Services Speech SDK spraak naar tekst te transcriberen.
De toepassing is gemaakt met de [Microsoft Cognitive Services Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017.

> [!NOTE]
> .NET core is een open-source, platformoverschrijdende .NET-platform uitvoering van de [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) specificatie.

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [de spraakservice gratis uitproberen](get-started.md).
* Een Windows-PC met een werkende microfoon.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community Edition of hoger.
* De **.NET Core platformoverschrijdende ontwikkeling** workload in Visual Studio. U kunt deze inschakelen in **extra** \> **hulpprogramma's en onderdelen**.

  ![.NET Core platformoverschrijdende ontwikkeling mogelijk maken](media/sdk/vs-enable-net-core-workload.png)

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Maak een nieuwe Visual C# .NET Core-consoletoepassing in Visual Studio 2017. In de **nieuw Project** in het dialoogvenster, in het linkerdeelvenster Vouw **geïnstalleerde** \> **Visual C#** \> **.NET Core**en selecteer vervolgens **Console-App (.NET Core)**. Voer voor de naam van het project, *helloworld*.

    ![Visual C#-consoletoepassing (.NET Core)](media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Visual C#-consoletoepassing (.NET Core)")

1. Installeren en verwijzen naar de [spraak SDK NuGet-pakket](https://aka.ms/csspeech/nuget). In de Solution Explorer met de rechtermuisknop op de oplossing en selecteer **NuGet-pakketten beheren voor oplossing**.

    ![Met de rechtermuisknop op de NuGet-pakketten beheren voor oplossing](media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "NuGet-pakketten beheren voor oplossing")

1. In de rechterbovenhoek, in de **pakketbron** veld **Nuget.org**. Zoek en installeer de `Microsoft.CognitiveServices.Speech` verpakken en installeer het in de **helloworld** project.

    ![Installeer het NuGet-pakket Microsoft.CognitiveServices.Speech](media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-0.5.0.png "installeren Nuget-pakket")

1. Accepteer de gebruiksrechtovereenkomst in het dialoogvenster dat wordt weergegeven.

    ![Accepteer de licentie](media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "gaat akkoord met de licentie")

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Open `Program.cs` in uw Visual Studio-project en vervang de code in dit bestand door het volgende.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnetcore-windows/helloworld/Program.cs#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` met de abonnementssleutel van uw.

1. Vervang de tekenreeks `YourServiceRegion` met de [regio](regions.md) die zijn gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen aan het project.

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Maken van de toepassing. Kies in de menubalk **bouwen** > **Build Solution**. De code moet nu compileren zonder fouten.

    ![Geslaagde build](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "geslaagde build")

1. De toepassing te starten. Kies in de menubalk **Debug** > **Start Debugging**, of druk op **F5**.

    ![De app te starten in foutopsporing](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "bij het opsporen van fouten in de app te starten")

1. Een consolevenster wordt weergegeven, waarin u kunt iets wat er op (in het Engels). De herkende tekst wordt vervolgens weergegeven in hetzelfde venster.

    ![Console-uitvoer na geslaagde opname](media/sdk/qs-csharp-dotnetcore-windows-07-console-output.png "Console-uitvoer na geslaagde opname")

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek in dit voorbeeld in de `quickstart/csharp-dotnetcore-windows` map.

## <a name="next-steps"></a>Volgende stappen

- [Spraak vertalen](how-to-translate-speech.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
