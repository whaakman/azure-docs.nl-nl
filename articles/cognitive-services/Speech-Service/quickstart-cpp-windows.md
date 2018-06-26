---
title: Snelstartgids voor spraak-SDK voor C++ en Windows | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met Windows en C++ in cognitieve Services met behulp van de SDK-spraak.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 37228ca2f887f0993f707a2b10a2b5dcd888b1b9
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753656"
---
# <a name="quickstart-for-c-and-windows"></a>Quick Start voor C++ en Windows

De huidige versie van de cognitieve Services spraak SDK is `0.4.0`.

Er wordt beschreven hoe een consoletoepassing op basis van C++-maken voor Windows-bureaublad die gebruik maakt van de SDK-spraak.
De toepassing is op basis van de [Microsoft cognitieve Services SDK NuGet-pakket](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech) en Microsoft VisualStudio 2017.

> [!NOTE]
> Als u een Quick Start voor C++- en Linux zoekt, gaat u [hier](quickstart-cpp-linux.md).<br>
> Als u een Quick Start voor C#- en Windows zoekt, gaat u [hier](quickstart-csharp-windows.md).

> [!NOTE]
> Deze snelstartgids vereist een computer met een werkende microfoon.<br>
> Zie voor een steekproef die het spraakherkenning uit een opgegeven audio invoerbestand herkent de [voorbeeld](speech-to-text-sample.md#speech-recognition-from-a-file).

> [!NOTE]
> Zorg ervoor dat de installatie van Visual Studio bevat de **ontwikkeling van een bureaublad met C++** werkbelasting.
> Als u niet zeker weet, volg deze stappen om te controleren en te corrigeren: In Visual Studio 2017, selecteer **extra** \> **ophalen's en onderdelen** en het beheer van gebruikersaccount vragen erkent door te kiezen **Ja**.
> In de **werkbelastingen** tabblad **ontwikkeling van een bureaublad met C++** geen een set selectievakje ernaast, stelt u deze en klikt u op **wijzigen** wijzigingen op te slaan.

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

## <a name="creating-an-empty-console-application-project"></a>Een lege consoletoepassingsproject maken

Maak een nieuwe Visual C++ Windows Desktop Windows-consoletoepassing met de naam 'CppHelloSpeech' in Visual Studio-2017:

![Visual C++ Windows-bureaublad van Windows-consoletoepassing maken](media/sdk/speechsdk-05-vs-cpp-new-console-app.png)

Als u op een 64-bits Windows-installatie uitvoert, eventueel overschakelen van uw platform build `x64`:

![Het build-platform overschakelen naar x64](media/sdk/speechsdk-07-vs-cpp-switch-to-x64.png)

## <a name="install-and-reference-the-speech-sdk-nuget-package"></a>Installeren en verwijzen naar het Speech SDK NuGet-pakket

> [!NOTE]
> Zorg ervoor dat de NuGet package manager is ingeschakeld voor de installatie van Visual Studio 2017.
> Selecteer in Visual Studio-2017 **extra** \> **ophalen's en onderdelen** en het beheer van gebruikersaccount vragen erkent door te kiezen **Ja**. Selecteer vervolgens de **afzonderlijke onderdelen** tabblad en zoekt u **NuGet-Pakketbeheer** onder **Code extra**.
> Als het selectievakje aan de linkerkant niet is ingesteld, moet u deze instellen en klik op **wijzigen** wijzigingen op te slaan.
>
> ![NuGet-Pakketbeheer in Visual Studio inschakelen ](media/sdk/speechsdk-05-vs-enable-nuget-package-manager.png)

Klik in Solution Explorer met de rechtermuisknop op de oplossing en klik op **NuGet-pakketten beheren voor oplossing**.

![Klik met de rechtermuisknop NuGet-pakketten beheren voor oplossing](media/sdk/speechsdk-09-vs-cpp-manage-nuget-packages.png)

In de rechterbovenhoek in de **pakketbron** Kies 'Nuget.org'.
Van de **Bladeren** tabblad, zoekt u het pakket 'Microsoft.CognitiveServices.Speech', selecteert u deze en controleer de **Project** en **CppHelloSpeech** vakken aan de rechterkant en Selecteer **installeren** om deze te installeren in het project CppHelloSpeech.

![Installeer Microsoft.CognitiveServices.Speech NuGet-pakket](media/sdk/speechsdk-11-vs-cpp-manage-nuget-install.png)

In het scherm van het type licentie dat wordt weergegeven, accepteert u de licentie:

![Accepteer de licentievoorwaarden](media/sdk/speechsdk-12-vs-cpp-manage-nuget-license.png)

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

Uw starter standaardcode vervangen door het volgende:

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/Windows/quickstart-cpp/CppHelloSpeech.cpp#code)]

> [!IMPORTANT]
> De abonnementssleutel vervangen door een die u hebt verkregen. <br>
> De regio vervangen door uw regio in de [REST-API van spraak](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis), bijvoorbeeld vervangen door 'westus'.

![De abonnementssleutel van uw toevoegen](media/sdk/sub-key-recognize-speech-cpp.png)

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

De code moet nu compileren zonder fouten:

![Voltooide build](media/sdk/speechsdk-16-vs-cpp-build.png)

Start het programma onder het foutopsporingsprogramma met de knop starten of gebruik de sneltoets F5:

![Start de app in Foutopsporing](media/sdk/speechsdk-17-vs-cpp-f5.png)

Een consolevenster wordt weergegeven, waarin u iets wat er (Engelstalig).
Het resultaat van de erkenning wordt weergegeven op het scherm.

![Console-uitvoer na de geslaagde opname](media/sdk/speechsdk-18-vs-cpp-console-output-release.png)

## <a name="downloading-the-sample"></a>Het voorbeeld downloaden

Zie voor de meest recente set steekproeven van het [cognitieve Services spraak SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Volgende stappen

* Ga naar de [pagina voorbeelden](samples.md) voor aanvullende voorbeelden.
