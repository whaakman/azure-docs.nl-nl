---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 2/20/2019
ms.author: erhopf
ms.openlocfilehash: 27fcf32a9a268488da318567d3edc55d23bd8967
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66482336"
---
1. Start Visual Studio 2019.

1. Zorg ervoor dat de workload **Desktop development with C++** beschikbaar is. Kies **Tools** > **Get Tools and Features** in de menubalk van Visual Studio om het installatieprogramma voor Visual Studio te openen. Als de werkbelasting al is ingeschakeld, gaat u naar de volgende stap.

    ![Schermafbeelding van het tabblad Workloads van Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-cpp-workload.png)

    Schakel anders het selectievakje naast **Desktop development with C++** in.

1. Zorg ervoor dat het onderdeel **NuGet package manager** beschikbaar is. Ga naar het tabblad **Individual components** van het dialoogvenster van het installatieprogramma van Visual Studio en selecteer **NuGet package manager** als deze optie nog niet is ingeschakeld.

      ![Schermafbeelding van het tabblad Individual components van Visual Studio](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-nuget-package-manager.png)

1. Als u de workload C++ of NuGet hebt moeten inschakelen, selecteert u **Modify** (in de rechterbenedenhoek van het dialoogvenster). De installatie van de nieuwe functies duurt even. Als beide functies al zijn ingeschakeld, kunt u het dialoogvenster sluiten.

1. Maak een nieuwe Windows-consoletoepassing van het type Windows Desktop voor Visual C++. Kies eerst **File** > **New** > **Project** in het menu. Vouw in het dialoogvenster **New Project** **Installed** > **Visual C++**  > **Windows Desktop** uit in het linkerdeelvenster. Selecteer vervolgens **Windows Console Application**. Voer als projectnaam *helloworld* in.

    ![Schermafbeelding van het dialoogvenster New Project](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-01-new-console-app.png)

1. Als u 64-bits Windows gebruikt, kunt u uw build-platform omschakelen naar `x64` via de vervolgkeuzelijst in de werkbalk van Visual Studio. (64-bits versies van Windows kunnen 32-bits toepassingen uitvoeren, dus dit is geen vereiste.)

    ![Schermafbeelding van de Visual Studio-werkbalk, met de optie x64 gemarkeerd](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-02-switch-to-x64.png)

1. Klik in Solution Explorer met de rechtermuisknop op de oplossing en kies **Manage NuGet Packages for Solution**.

    ![Schermafbeelding van Solution Explorer, met de optie Manage NuGet Packages for Solution gemarkeerd](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. Selecteer in de rechterbovenhoek, in het veld **Package Source**, het pakket **nuget.org**. Zoek het pakket `Microsoft.CognitiveServices.Speech` en installeer het in het project **helloworld**.

    ![Schermafbeelding van het dialoogvenster Manage Packages for Solution](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

    > [!NOTE]
    > De huidige versie van de Speech SDK van Cognitive Services is `1.5.0`.

1. Accepteer de weergegeven licentie om te beginnen met het installeren van het NuGet-pakket.

    ![Schermafbeelding van het licentievenster](../articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-05-nuget-license.png)

Nadat het pakket is geïnstalleerd, wordt er een bevestiging weergegeven in de Package Manager-console.
