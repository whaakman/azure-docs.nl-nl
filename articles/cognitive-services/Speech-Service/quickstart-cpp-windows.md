---
title: 'Snelstart: Gesproken tekst herkennen in C++ onder Windows met behulp van de Speech Service SDK'
titleSuffix: Azure Cognitive Services
description: Informatie over het herkennen van gesproken tekst in C++ onder Windows Desktop met behulp van de Speech Service SDK
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 10/12/2018
ms.author: wolfma
ms.openlocfilehash: ddac12c6414365f8d20c42d324695be9855774dc
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49469864"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Snelstartgids: Gesproken tekst herkennen in C++ onder Windows met behulp van de Speech SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een C++-consoletoepassing voor Windows. U gebruikt de [Speech SDK](speech-sdk.md) van Cognitive Services om in realtime spraak te transcriberen naar tekst via de microfoon van uw pc. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

## <a name="prerequisites"></a>Vereisten

U hebt een abonnementssleutel voor de Speech-service nodig om deze snelstartgids te doorlopen. U kunt er gratis een krijgen. Zie [Speech-service gratis uitproberen](get-started.md) voor meer informatie.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Start Visual Studio 2017.

1. Zorg ervoor dat de workload **Desktop development with C++** beschikbaar is. Kies **Tools** > **Get Tools and Features** in de menubalk van Visual Studio om het installatieprogramma voor Visual Studio te openen. Als de werkbelasting al is ingeschakeld, gaat u naar de volgende stap. 

    ![Schermafbeelding van het tabblad Workloads van Visual Studio](media/sdk/vs-enable-cpp-workload.png)

    Schakel anders het selectievakje naast **Desktop development with C++** in. 

1. Zorg ervoor dat het onderdeel **NuGet package manager** beschikbaar is. Ga naar het tabblad **Individual components** van het dialoogvenster van het installatieprogramma van Visual Studio en selecteer **NuGet package manager** als deze optie nog niet is ingeschakeld.

      ![Schermafbeelding van het tabblad Individual components van Visual Studio](media/sdk/vs-enable-nuget-package-manager.png)

1. Als u de workload C++ of NuGet hebt moeten inschakelen, selecteert u **Modify** (in de rechterbenedenhoek van het dialoogvenster). De installatie van de nieuwe functies duurt even. Als beide functies al zijn ingeschakeld, kunt u het dialoogvenster sluiten.

1. Maak een nieuwe Windows-consoletoepassing van het type Windows Desktop voor Visual C++. Kies eerst **File** > **New** > **Project** in het menu. Vouw in het dialoogvenster **New Project** **Installed** > **Visual C++** > **Windows Desktop** uit in het linkerdeelvenster. Selecteer vervolgens **Windows Console Application**. Voer als projectnaam *helloworld* in.

    ![Schermafbeelding van het dialoogvenster New Project](media/sdk/qs-cpp-windows-01-new-console-app.png)

1. Als u 64-bits Windows gebruikt, kunt u uw build-platform omschakelen naar `x64` via de vervolgkeuzelijst in de werkbalk van Visual Studio. (64-bits versies van Windows kunnen 32-bits toepassingen uitvoeren, dus dit is geen vereiste.)

    ![Schermafbeelding van de Visual Studio-werkbalk, met de optie x64 gemarkeerd](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

1. Klik in Solution Explorer met de rechtermuisknop op de oplossing en kies **Manage NuGet Packages for Solution**.

    ![Schermafbeelding van Solution Explorer, met de optie Manage NuGet Packages for Solution gemarkeerd](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

1. Selecteer in de rechterbovenhoek, in het veld **Package Source**, het pakket **nuget.org**. Zoek het pakket `Microsoft.CognitiveServices.Speech` en installeer het in het project **helloworld**.

    ![Schermafbeelding van het dialoogvenster Manage Packages for Solution](media/sdk/qs-cpp-windows-04-nuget-install-1.0.0.png)

    > [!NOTE]
    > De huidige versie van de Speech SDK van Cognitive Services is `1.0.1`.

1. Accepteer de weergegeven licentie om te beginnen met het installeren van het NuGet-pakket.

    ![Schermafbeelding van het licentievenster](media/sdk/qs-cpp-windows-05-nuget-license.png)

Nadat het pakket is geïnstalleerd, wordt er een bevestiging weergegeven in de Package Manager-console.

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open het bronbestand *helloworld.cpp*. Vervang alle code onder de eerste include-instructie (`#include "stdafx.h"` of `#include "pch.h"`) door het volgende:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die gekoppeld is aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Kies in de menubalk **Build** > **Build Solution**. De code moet zonder fouten worden gecompileerd.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-cpp-windows-06-build.png)

1. Start de toepassing. Kies in de menubalk **Debug** > **Start Debugging** of druk op **F5**.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Er wordt een consolevenster weergegeven waarin u wordt gevraagd iets te zeggen. Spreek een Engelse woordgroep of zin in. De gesproken tekst wordt verzonden naar de Speech-service en getranscribeerd naar tekst, die in hetzelfde venster wordt weergegeven.

   ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek naar dit voorbeeld in de map `quickstart/cpp-windows`.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Intenties van gesproken inhoud herkennen met behulp van de Speech SDK voor C++](how-to-recognize-intents-from-speech-cpp.md)

## <a name="see-also"></a>Zie ook

- [Spraak vertalen](how-to-translate-speech-csharp.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
