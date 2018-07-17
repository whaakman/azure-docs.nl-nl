---
title: 'Snelstartgids: Herkennen gesproken tekst in C# onder .NET Framework op Windows met behulp van de Cognitive Services Speech SDK | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in C# onder .NET Framework op Windows met behulp van de Cognitive Services spraak-SDK
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 07f22345d2bf8c481091652d9faa0bdd10205fdb
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072562"
---
# <a name="quickstart-recognize-speech-in-c-under-net-framework-on-windows-using-the-speech-sdk"></a>Snelstartgids: Herkennen gesproken tekst in C# onder .NET Framework op Windows met behulp van de spraak-SDK

In dit artikel leert u hoe u een C#-consoletoepassing maken voor .NET Framework voor Windows met behulp van de Cognitive Services Speech SDK spraak naar tekst te transcriberen.
De toepassing is op basis van de [Microsoft Cognitive Services Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [de spraakservice gratis uitproberen](get-started.md).
* Een Windows-PC met een werkende microfoon.
* Visual Studio 2017, Community Edition of hoger.
* De **.NET-desktopontwikkeling** workload in Visual Studio. U kunt deze inschakelen in **extra** \> **hulpprogramma's en onderdelen**.

  ![.NET-desktopontwikkeling inschakelen](media/sdk/vs-enable-net-desktop-workload.png)

  ![.NET Core platformoverschrijdende ontwikkeling mogelijk maken](media/sdk/vs-enable-net-desktop-workload.png)

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Maak een nieuwe Visual C#-consoletoepassing in Visual Studio 2017. In de **nieuw Project** in het dialoogvenster, in het linkerdeelvenster Vouw **geïnstalleerde** \> **Visual C#** \> **Windows Desktop** en selecteer vervolgens **Console-App (.NET Framework)**. Voer voor de naam van het project, *helloworld*.

    ![Visual C#-consoletoepassing (.NET Framework)](media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Visual C#-consoletoepassing (.NET Framework)")

1. Installeren en verwijzen naar de [spraak SDK NuGet-pakket](https://aka.ms/csspeech/nuget). In de Solution Explorer met de rechtermuisknop op de oplossing en selecteer **NuGet-pakketten beheren voor oplossing**.

    ![Met de rechtermuisknop op de NuGet-pakketten beheren voor oplossing](media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "NuGet-pakketten beheren voor oplossing")

1. In de rechterbovenhoek, in de **pakketbron** veld **Nuget.org**. Zoek de `Microsoft.CognitiveServices.Speech` verpakt en installeert u deze in de **helloworld** project.

    ![Installeer het NuGet-pakket Microsoft.CognitiveServices.Speech](media/sdk/qs-csharp-dotnet-windows-03-nuget-install-0.5.0.png "installeren Nuget-pakket")

1. In het scherm van het type licentie dat verschijnt, accepteer de licentie:

    ![Accepteer de licentie](media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "gaat akkoord met de licentie")

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>Maken van een platformconfiguratie die overeenkomen met uw PC-architectuur

In deze sectie maakt toevoegen u een nieuw platform aan de configuratie die overeenkomt met uw processorarchitectuur.

1. Start de Configuration Manager. Selecteer **bouwen** > **Configuration Manager**.

    ![De configuration manager starten](media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "starten van de configuration manager")

1. In de **Configuration Manager** dialoogvenster vak, voegt u een nieuw platform. Uit de **Active oplossingsplatform** vervolgkeuzelijst, selecteer **nieuw**.

    ![Toevoegen van een nieuw platform onder het venster van de configuration manager](media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "toevoegen van een nieuw platform onder het venster van de configuration manager")

1. Als u een 64-bits Windows uitvoert, maakt u een nieuw platformconfiguratie met de naam `x64`. Als u een 32-bits Windows uitvoert, maakt u een nieuw platformconfiguratie met de naam `x86`. In dit artikel maakt u een `x64` platformconfiguratie.

    ![Op 64-bits Windows, voegt u een nieuw platform met de naam "x64"](media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "toevoegen x64 platform")

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. In de `Program.cs` bestand in Visual Studio-project, alle code vervangen door het volgende:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` met de abonnementssleutel van uw.

1. Vervang de tekenreeks `YourServiceRegion` met de [regio](regions.md) die zijn gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen aan het project.

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Maken van de toepassing. Selecteer in de menubalk **bouwen** > **Build Solution**. De code moet nu compileren zonder fouten:

    ![Geslaagde build](media/sdk/qs-csharp-dotnet-windows-08-build.png "geslaagde build")

1. De toepassing te starten. Selecteer in de menubalk **Debug** > **Start Debugging**, of druk op **F5**.

    ![De app te starten in foutopsporing](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "bij het opsporen van fouten in de app te starten")

1. Een consolevenster verschijnt, waarin u kunt iets wat er op (in het Engels).
   Het resultaat van de opname wordt op het scherm weergegeven.

    ![Console-uitvoer na geslaagde opname](media/sdk/qs-csharp-dotnet-windows-10-console-output.png "Console-uitvoer na geslaagde opname")

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek in dit voorbeeld in de `quickstart/csharp-dotnet-windows` map.

## <a name="next-steps"></a>Volgende stappen

- [Spraak vertalen](how-to-translate-speech.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
