---
title: 'Snelstartgids: Herkennen gesproken tekst in C++ op Windows-bureaublad met de Cognitive Services spraak-SDK'
titleSuffix: Microsoft Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in C++ op Windows-bureaublad met de Cognitive Services spraak-SDK
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 7c93f9f450cc2d244a0d716401171cde02abf70b
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324568"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-desktop-using-the-speech-sdk"></a>Snelstartgids: Herkennen gesproken tekst in C++ op Windows-bureaublad met de spraak-SDK

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Wordt beschreven hoe u een op basis van een C++-consoletoepassing maken voor Windows-bureaublad die gebruik maakt van de spraak-SDK.
De toepassing is op basis van de [Microsoft Cognitive Services Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017.

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [de spraakservice gratis uitproberen](get-started.md).
* Een Windows-PC met een werkende microfoon.
* [Microsoft Visual Studio 2017](https://www.visualstudio.com/), Community Edition of hoger.
* De **bureaubladontwikkeling met C++** workload in Visual Studio en de **NuGet-Pakketbeheer** onderdeel in Visual Studio.
  U kunt beide in **hulpprogramma's** \> **hulpprogramma's en onderdelen**onder de **Workloads** en **afzonderlijke onderdelen** tabbladen , respectievelijk:

  ![Bureaubladontwikkeling met C++ werkbelasting inschakelen](media/sdk/vs-enable-cpp-workload.png)

  ![NuGet-Pakketbeheer in Visual Studio inschakelen ](media/sdk/vs-enable-nuget-package-manager.png)

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

Maak een nieuwe Visual C++ Windows Desktop Windows consoletoepassing in Visual Studio 2017. In de **nieuw Project** in het dialoogvenster, in het linkerdeelvenster Vouw **geïnstalleerde** \> **Visual C++** \> **Windows Desktop** en selecteer vervolgens **Windows-consoletoepassing**. Voer voor de naam van het project, *helloworld*.

![Visual C++ Windows-bureaublad van Windows-consoletoepassing maken](media/sdk/qs-cpp-windows-01-new-console-app.png)

Als u op een 64-bits Windows-installatie uitvoert, (optioneel) uw build-platform om te schakelen `x64`:

![Het build-platform overschakelen naar x64](media/sdk/qs-cpp-windows-02-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Installeren en verwijzen naar de Speech SDK NuGet-pakket

In de Solution Explorer met de rechtermuisknop op de oplossing en klik op **NuGet-pakketten beheren voor oplossing**.

![Met de rechtermuisknop op de NuGet-pakketten beheren voor oplossing](media/sdk/qs-cpp-windows-03-manage-nuget-packages.png)

In de rechterbovenhoek, in de **pakketbron** Kies 'Nuget.org'.
Uit de **Bladeren** tabblad, zoek het pakket 'Microsoft.CognitiveServices.Speech', selecteert u deze en controleer de **Project** en **helloworld** vakken aan de rechterkant en selecteer **Installeren** om deze te installeren in de helloworld-project.

> [!NOTE]
> De huidige versie van de Cognitive Services Speech SDK is `0.5.0`.

![Microsoft.CognitiveServices.Speech NuGet-pakket installeren](media/sdk/qs-cpp-windows-04-nuget-install-0.5.0.png)

In het scherm van het type licentie dat verschijnt, accepteer de licentie:

![Accepteer de gebruiksrechtovereenkomst](media/sdk/qs-cpp-windows-05-nuget-license.png)

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. De code van uw standaard-starter vervangen door het volgende:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` met de abonnementssleutel van uw.

1. Vervang de tekenreeks `YourServiceRegion` met de [regio](regions.md) die zijn gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen aan het project.

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Maken van de toepassing. Selecteer in de menubalk **bouwen** > **Build Solution**. De code moet nu compileren zonder fouten:

   ![Geslaagde build](media/sdk/qs-cpp-windows-06-build.png)

1. De toepassing te starten. Selecteer in de menubalk **Debug** > **Start Debugging**, of druk op **F5**.

   ![Start de app into-foutopsporing](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Een consolevenster verschijnt, waarin u kunt iets wat er op (in het Engels).
   Het resultaat van de opname wordt weergegeven op het scherm.

   ![Console-uitvoer na geslaagde opname](media/sdk/qs-cpp-windows-08-console-output-release.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek in dit voorbeeld in de `quickstart/cpp-windows` map.

## <a name="next-steps"></a>Volgende stappen

* [Onze voorbeelden ophalen](speech-sdk.md#get-the-samples)
