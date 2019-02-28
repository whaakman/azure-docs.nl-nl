---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 2/20/2019
ms.author: erhopf
ms.openlocfilehash: 2e495f9aed18ebc4007e63b8ef03641a72d21079
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56963339"
---
1. Start Visual Studio 2017.

1. Zorg ervoor dat de workload **Universal Windows Platform development** beschikbaar is. Kies **Tools** > **Get Tools and Features** in de menubalk van Visual Studio om het installatieprogramma voor Visual Studio te openen. Als deze workload al is ingeschakeld, sluit u het dialoogvenster.

    ![Schermopname van het installatieprogramma van Visual Studio, met het tabblad Workloads gemarkeerd](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-uwp-workload.png)

    Selecteer anders het vakje naast **.NET cross-platform development** en selecteer **Modify** in de rechterbenedenhoek van het dialoogvenster. De installatie van de nieuwe functie duurt even.

1. Maak een lege, universele Windows-app in C#. Kies eerst **File** > **New** > **Project** in het menu. Vouw in het dialoogvenster **New Project** **Installed** > **Visual C#** > **Windows Universal** uit in het linkerdeelvenster. Selecteer vervolgens **Blank App (Universal Windows)**. Voer als projectnaam *helloworld* in.

    ![Schermafbeelding van het dialoogvenster New Project](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-01-new-blank-app.png)

1. De Speech SDK vereist dat uw toepassing wordt gecompileerd voor de Windows 10-makersupdate van het najaar of later. Kies in het venster **New Universal Windows Platform Project** dat wordt weergegeven **Windows 10 Fall Creators Update (10.0; Build 16299)** bij **Minimum version**. Selecteer bij **Target version** deze of een latere versie en klik op **OK**.

    ![Schermafbeelding van het venster New Universal Windows Platform Project](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-02-new-uwp-project.png)

1. Als u 64-bits Windows gebruikt, kunt u uw build-platform omschakelen naar `x64` via de vervolgkeuzelijst in de werkbalk van Visual Studio. (Met 64-bits Windows kunt u ook 32-bits toepassingen uitvoeren, dus u kunt dit laten staan op `x86` als u dat liever hebt.)

   ![Schermafbeelding van de Visual Studio-werkbalk, met x64 gemarkeerd](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-03-switch-to-x64.png)

   > [!NOTE]
   > De Speech SDK biedt alleen ondersteuning voor Intel-compatibele processors. ARM wordt momenteel niet ondersteund.

1. Installeer het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en verwijs ernaar. Klik in Solution Explorer met de rechtermuisknop op de oplossing en selecteer **Manage NuGet Packages for Solution**.

    ![Schermafbeelding van Solution Explorer, met Manage NuGet Packages for Solution gemarkeerd](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-04-manage-nuget-packages.png)

1. Selecteer in de rechterbovenhoek, in het veld **Package Source**, het pakket **nuget.org**. Zoek het pakket `Microsoft.CognitiveServices.Speech` en installeer het in het project **helloworld**.

    ![Schermafbeelding van het dialoogvenster Manage Packages for Solution](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-05-nuget-install-1.0.0.png "NuGet-pakket installeren")

1. Ga akkoord met de weergegeven licentie om te beginnen met het installeren van het NuGet-pakket.

    ![Schermafbeelding van het licentievenster](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-06-nuget-license.png "Akkoord gaan met licentie")

1. De volgende regel wordt uitgevoerd in de Package Manager Console.

   ```text
   Successfully installed 'Microsoft.CognitiveServices.Speech 1.3.1' to helloworld
   ```

1. Omdat de toepassing de microfoon gebruikt voor spraakinvoer, voegt u **Microphone** toe aan het project. Dubbelklik in Solution Explorer op **Package.appxmanifest** om het manifest van uw toepassing te bewerken. Ga vervolgens naar het tabblad **Capabilities**, selecteer het selectievakje bij **Microphone** en sla de wijzigingen op.

   ![Schermafbeelding van het Visual Studio-toepassingsmanifest, met Capabilities en Microphone gemarkeerd](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-07-capabilities.png)
