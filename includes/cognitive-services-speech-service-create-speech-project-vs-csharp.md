---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 308ee2ef121648cb45152948926c5fd7fb934744
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68362486"
---
1. Open Visual Studio 2019.

1. Selecteer in het Start venster **een nieuw project maken**. 

1. Selecteer **console-app (.NET Framework)** en selecteer **volgende**.

1. Voer in **project naam**, `helloworld`ENTER en selecteer vervolgens **maken**.

1. Selecteer in de menu balk van Visual Studio **hulpprogram** > ma's**en functies ophalen**en controleer of de werk belasting van **.net desktop Development** beschikbaar is. Als de werk belasting niet is geïnstalleerd, schakelt u het selectie vakje in en selecteert u vervolgens **wijzigen** om de installatie te starten. Het downloaden en installeren kan een paar minuten duren.

   Als het selectievakje naast **.NET-desktopontwikkeling** is ingeschakeld, kunt u nu het dialoogvenster sluiten.

   ![.NET-desktopontwikkeling inschakelen](../articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

De volgende stap is het installeren van het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget), zodat u ernaar kunt verwijzen in de code.

1. Klik `helloworld`in de Solution Explorer met de rechter muisknop en selecteer vervolgens **NuGet-pakketten beheren** om de NuGet-pakket beheer weer te geven.

   ![NuGet-pakket beheer](../articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. Zoek in de rechter bovenhoek de vervolg keuzelijst **pakket bron** en zorg ervoor dat **nuget.org** is geselecteerd.

1. Selecteer in de linkerbovenhoek de optie **Bladeren**.

1. Typ in het zoekvak de tekst `Microsoft.CognitiveServices.Speech` package en druk op ENTER.

1. Selecteer `Microsoft.CognitiveServices.Speech`en selecteer **installeren** om de nieuwste stabiele versie te installeren.

   ![Het NuGet-pakket micro soft. CognitiveServices. speech installeren](../articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Accepteer alle overeenkomsten en licenties om de installatie te starten.

   Nadat het pakket is geïnstalleerd, wordt er een bevestiging weer gegeven in het console venster van **Package Manager** .

Maak nu een platform configuratie die overeenkomt met de architectuur van uw computer om de console toepassing te bouwen en uit te voeren.

1. Selecteer in de menu balk de optie **Build** > **Configuration Manager**. De **Configuration Manager** in het dialoogvenster wordt weergegeven.

   ![Het dialoog venster Configuration Manager](../articles/cognitive-services/speech-service/media/sdk/vs-configuration-manager-dialog-box.png)

1. Selecteer in de vervolg keuzelijst **actief oplossings platform** **nieuwe**. Het dialoog venster **nieuw platform voor oplossingen** wordt weer gegeven.

1. **Typ of selecteer** in het vak de vervolg keuzelijst nieuw platform:
   - Als u een 64-bits versie van Windows gebruikt, selecteert u **x64**.
   - Als u een 32-bits versie van Windows gebruikt, selecteert u **x86**.

1. Selecteer **OK** en vervolgens **sluiten**.
