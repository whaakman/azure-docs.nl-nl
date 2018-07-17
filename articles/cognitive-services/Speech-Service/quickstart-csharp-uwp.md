---
title: 'Snelstartgids: Herkennen gesproken tekst in C# in een UWP-app de Cognitive Services Speech SDK | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in een UWP-app met behulp van de Cognitive Services spraak-SDK
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 4599c3c4c69397a1ab1f65c246e4440085b8bb91
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072561"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-using-the-speech-sdk"></a>Snelstartgids: Herkennen gesproken tekst in een UWP-app met behulp van de spraak-SDK

In dit artikel leert u over het maken van een Universal Windows Platform (UWP)-toepassing met behulp van de Cognitive Services Speech SDK spraak naar tekst te transcriberen.
De toepassing is op basis van de [Microsoft Cognitive Services Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017.

> [!NOTE]
> UWP-apps die zijn gebouwd met de spraak-SDK nog doorgegeven niet de Windows App Certification Kit (WACK).
> Sideloading van apps is het mogelijk, verzenden van dat een app naar Windows Store is niet.
> Dit wordt opgelost in een toekomstige release.

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [de spraakservice gratis uitproberen](get-started.md).
* Een Windows-PC (Windows 10 Fall Creators Update of hoger) met een microfoon werken.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community Edition of hoger.
* De **universele Windows-platformontwikkeling** workload in Visual Studio.You kunt inschakelen in **extra** \> **hulpprogramma's en onderdelen**.

  ![Universal Windows Platform-ontwikkeling mogelijk maken](media/sdk/vs-enable-uwp-workload.png)

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Maak een nieuwe Visual C# Windows universele lege App in Visual Studio 2017. In de **nieuw Project** in het dialoogvenster, in het linkerdeelvenster Vouw **geïnstalleerde** \> **Visual C#** \> **Windows Universal** en selecteer vervolgens **lege App (universeel Windows)**. Voer voor de naam van het project, *helloworld*.

    ![](media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. In de **nieuw Universal Windows Platform-Project** venster dat pop's, kies **Windows 10 Fall Creators Update (10.0; Build 16299)** als **minimumversie**, en deze of een latere versie als **doelversie**, klikt u vervolgens op **OK**.

    ![](media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Als u op een 64-bits Windows-installatie uitvoert, u uw build-platform te kan schakelen `x64`:

   ![Het build-platform overschakelen naar x64](media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > De spraak-SDK biedt op dit moment geen ondersteuning op doel ARM processors.

1. Installeren en verwijzen naar de [spraak SDK NuGet-pakket](https://aka.ms/csspeech/nuget). In de Solution Explorer met de rechtermuisknop op de oplossing en selecteer **NuGet-pakketten beheren voor oplossing**.

    ![Met de rechtermuisknop op de NuGet-pakketten beheren voor oplossing](media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. In de rechterbovenhoek, in de **pakketbron** veld **Nuget.org**. Zoek en installeer de `Microsoft.CognitiveServices.Speech` verpakken en installeer het in de **helloworld** project.

    ![Installeer het NuGet-pakket Microsoft.CognitiveServices.Speech](media/sdk/qs-csharp-uwp-05-nuget-install-0.5.0.png "installeren Nuget-pakket")

1. In het scherm van het type licentie dat verschijnt, accepteer de licentie:

    ![Accepteer de licentie](media/sdk/qs-csharp-uwp-06-nuget-license.png "gaat akkoord met de licentie")

1. In de Package Manager console ziet u de volgende uitvoerregel:

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 0.5.0' to helloworld
   ```

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Dubbelklik in Solution Explorer op **Package.appxmanifest** bewerken van het manifest van uw toepassing.
   Selecteer de **mogelijkheden** tabblad, selecteert u het selectievakje voor de **microfoon** mogelijkheid, en sla de wijzigingen.

   ![](media/sdk/qs-csharp-uwp-07-capabilities.png)

1. Uw app UI bewerken door te dubbelklikken op `MainPage.xaml` in Solution Explorer.
   In de weergave van de ontwerpfunctie XAML, invoegen van het volgende fragment van XAML in de tag raster (tussen `<Grid>` en `</Grid>`):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. De XAML-code-behind bewerken door te dubbelklikken op `MainPage.xaml.cs` in Solution Explorer (deze wordt gegroepeerd onder de `MainPage.xaml` item).
   Vervang alle code door het volgende:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. In de `SpeechRecognitionFromMicrophone_ButtonClicked` handler, vervang de tekenreeks `YourSubscriptionKey` met de abonnementssleutel van uw.

1. In de `SpeechRecognitionFromMicrophone_ButtonClicked` handler, vervang de tekenreeks `YourServiceRegion` met de [regio](regions.md) die zijn gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla alle wijzigingen in het project.

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Maken van de toepassing. Selecteer in de menubalk **bouwen** > **Build Solution**. De code moet nu compileren zonder fouten:

    ![Geslaagde build](media/sdk/qs-csharp-uwp-08-build.png "geslaagde build")

1. De toepassing te starten. Selecteer in de menubalk **Debug** > **Start Debugging**, of druk op **F5**.

    ![De app te starten in foutopsporing](media/sdk/qs-csharp-uwp-09-start-debugging.png "bij het opsporen van fouten in de app te starten")

1. Er verschijnt een venster GUI. Klikt u eerst op de **inschakelen microfoon** knop en te bevestigen van de machtiging-aanvraag die wordt weergegeven.

    ![De app te starten in foutopsporing](media/sdk/qs-csharp-uwp-10-access-prompt.png "bij het opsporen van fouten in de app te starten")

1. Klik vervolgens op de **spraakherkenning met invoer van de microfoon** en spreek een korte zin in de microfoon.
   Het resultaat van de spraakherkenning spraak moet weergeven in de gebruikersinterface.

    ![](media/sdk/qs-csharp-uwp-11-ui-result.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek in dit voorbeeld in de `quickstart/csharp-uwp` map.

## <a name="next-steps"></a>Volgende stappen

- [Spraak vertalen](how-to-translate-speech.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
