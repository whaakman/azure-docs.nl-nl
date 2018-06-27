---
title: 'Snelstartgids: Herkennen spraak met cognitieve Services spraak C#-SDK voor Windows | Microsoft Docs'
description: Informatie over het herkennen van spraak met de C#-SDK voor spraak-service.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 64281215d139731b61365936bc1b837798ad8fbf
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37016831"
---
# <a name="quickstart-recognize-speech-using-the-cognitive-services-speech-c-sdk"></a>Snelstartgids: Herkennen spraak met behulp van de cognitieve Services spraak C# SDK

In dit artikel leert u hoe een C#-consoletoepassing maken in Windows met behulp van de SDK cognitieve Services spraak transcriberen spraak naar tekst.

## <a name="prerequisites"></a>Vereisten

* De abonnementssleutel van een voor de service spraak. Zie [de spraak-service gratis uitproberen](get-started.md).
* Visual Studio 2017, Community Edition of hoger.
* De **.NET-ontwikkeling voor bureaubladen** werkbelasting in Visual Studio. U kunt het inschakelen in **extra** \> **ophalen's en onderdelen**. 

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Maak een nieuwe Visual C#-consoletoepassing in Visual Studio-2017. In de **nieuw Project** in het dialoogvenster in het linkerdeelvenster Vouw **geïnstalleerde** en selecteer vervolgens **Console-App (.NET Framework)**. Voer voor de naam van het project *CsharpHelloSpeech*.

    ![Visual C#-consoletoepassing maken (.NET Framework)](media/sdk/speechsdk-05-vs-cs-new-console-app.png "Visual C#-consoletoepassing maken")

2. Installeren en verwijzen naar de [spraak SDK NuGet-pakket](https://aka.ms/csspeech/nuget). Klik in Solution Explorer met de rechtermuisknop op de oplossing en selecteer **NuGet-pakketten beheren voor oplossing**.

    ![Klik met de rechtermuisknop NuGet-pakketten beheren voor oplossing](media/sdk/speechsdk-06-vs-cs-manage-nuget-packages.png "NuGet-pakketten beheren voor oplossing")

3. In de rechterbovenhoek in de **pakketbron** optie **Nuget.org**. Zoek en installeer de `Microsoft.CognitiveServices.Speech` van het pakket en installeer het in de **CsharpHelloSpeech** project.

    ![Installeer het NuGet-pakket Microsoft.CognitiveServices.Speech](media/sdk/speechsdk-08-vs-cs-nuget-install.png "installeren Nuget-pakket")

4. In het scherm van het type licentie dat wordt weergegeven, accepteert u de licentie:

    ![Accepteer de licentievoorwaarden](media/sdk/speechsdk-09-vs-cs-nuget-license.png "akkoord gaan met de licentievoorwaarden")

## <a name="create-a-platform-configuration-matching-your-pc-architecture"></a>Maken van een platformconfiguratie die overeenkomt met uw PC-architectuur

In deze sectie kunt u een nieuw platform toevoegen aan de configuratie die overeenkomt met de processorarchitectuur.

1. Start de Configuration Manager. Selecteer **bouwen** > **Configuration Manager**.

    ![Starten van de configuration manager](media/sdk/speechsdk-12-vs-cs-cfg-manager-click.png "starten van de configuration manager")

2. In de **Configuration Manager** dialoogvenster Voeg een nieuw platform. Van de **actieve oplossing platform** vervolgkeuzelijst, selecteer **nieuw**.

    ![Voeg een nieuw platform onder het venster van de configuration manager](media/sdk/speechsdk-14-vs-cs-cfg-manager-new.png "toevoegen van een nieuw platform onder het venster van de configuration manager")

3. Als u 64-bits Windows uitvoert, maakt u een nieuwe platformconfiguratie met de naam `x64`. Als u een 32-bits Windows uitvoert, maakt u een nieuwe platformconfiguratie met de naam `x86`. In dit artikel maakt u een `x64` platformconfiguratie. 

    ![Voeg een nieuw platform met de naam 'x64' op 64-bits Windows](media/sdk/speechsdk-15-vs-cs-cfg-manager-add-x64.png "toevoegen x64 platform")

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. In de `Program.cs` voor Visual Studio-project, vervangt u de hoofdtekst van de `Program` klasse met de volgende. Zorg ervoor dat u de abonnementssleutel en de regio vervangen met een versie die u voor de service verkregen.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#code)]

2. Na het plakken van de code de `Main()` methode moet lijken op zoals weergegeven in de volgende schermafbeelding:

    ![Hoofdmethode na het plakken van de code](media/sdk/speechsdk-17-vs-cs-paste-code.png "laatste Main-methode")

3. Visual Studio IntelliSense licht de verwijzingen naar de spraak-SDK-klassen die niet kan omgezet worden. U kunt deze fout oplossen door de volgende toevoegen `using` instructie naar het begin van de code (ofwel handmatig of met behulp van Visual Studio [snelle acties](https://docs.microsoft.com/visualstudio/ide/quick-actions)).

    [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-csharp/Program.cs#usingstatement)]

    ![Gebruik de snelle actie naar de ontbrekende toevoegen met de instructie](media/sdk/speechsdk-18-vs-cs-add-using.png "IntelliSense los problemen")

4. Zorg ervoor dat het markeren van IntelliSense opgelost is en opslaan van wijzigingen in het project.

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Maken van de toepassing. Selecteer in de menubalk **bouwen** > **Build Solution**. De code moet nu compileren zonder fouten:

    ![Voltooide build](media/sdk/speechsdk-20-vs-cs-build.png "voltooide build")

2. Start de toepassing. Selecteer in de menubalk **Debug** > **foutopsporing starten**, of druk op **F5**. 

    ![De app te starten in de foutopsporing](media/sdk/speechsdk-21-vs-cs-f5.png "de app in de foutopsporing te starten")

3. Een consolevenster verschijnt, te vragen om iets wat er (Engelstalig).
Het resultaat van de erkenning wordt weergegeven op het scherm.

    ![Console uitvoer na de geslaagde opname](media/sdk/speechsdk-22-cs-vs-console-output.png "uitvoer na de geslaagde opname Console")

## <a name="download-code"></a>Code downloaden

Zie voor de meest recente set steekproeven van het [cognitieve Services spraak SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Volgende stappen

- [Spraak vertalen](how-to-translate-speech.md)
- [Spraak modellen aanpassen](how-to-customize-speech-models.md)
