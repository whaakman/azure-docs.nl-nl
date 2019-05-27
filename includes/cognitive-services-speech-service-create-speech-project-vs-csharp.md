---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2018
ms.author: wolfma
ms.openlocfilehash: afe6f1493c7fa8272c67f23d6708ad6e4eea9381
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145507"
---
1. Start Visual Studio 2017.

1. Selecteer in de menubalk in Visual Studio, **Extra > hulpmiddelen ophalen** en zorg ervoor dat de **.NET-desktopontwikkeling** werkbelasting beschikbaar is. Als de workload niet is geïnstalleerd, schakelt u het selectievakje in en klikt u op **Wijzigen** om de installatie te starten. Het downloaden en installeren kan een paar minuten duren.

   Als het selectievakje naast **.NET-desktopontwikkeling** is ingeschakeld, kunt u nu het dialoogvenster sluiten.

   ![.NET-desktopontwikkeling inschakelen](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Vervolgens gaat u een project maken. Selecteer **Bestand > Nieuw > Project** in de menubalk. Selecteer in het linkerdeelvenster van het dialoogvenster de secties **Geïnstalleerd > Visual C# > Windows-bureaublad** en selecteer **Consoletoepassing (.NET Framework)**. Geef dit project de naam *hallowereld*.

    ![Visual C#-consoletoepassing maken (.NET Framework)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Visual C#-consoletoepassing maken (.NET Framework)")

1. Nu het project is ingesteld, moet het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) worden geïnstalleerd en moet er in de code een verwijzing naar het pakket worden opgenomen. Zoek de Solution Explorer en klik met de rechtermuisknop op hallowereld. Selecteer in het menu de optie **NuGet-pakketten beheren...**.

   ![Met de rechtermuisknop klikken op NuGet-pakketten beheren voor oplossing](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "NuGet-pakketten beheren voor oplossing")

1. Open rechtsboven in NuGet Package Manager de vervolgkeuzelijst **Package Source** en zorg ervoor dat **nuget.org** is geselecteerd. Selecteer **Browse**, zoek naar het pakket `Microsoft.CognitiveServices.Speech` en installeer de laatste stabiele versie.

   ![Het NuGet-pakket Microsoft.CognitiveServices.Speech installeren](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png "NuGet-pakket installeren")

1. Accepteer alle overeenkomsten en licenties om de installatie te starten.

   ![Accepteer de licentie](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Accepteer de licentie")

    Nadat het pakket is geïnstalleerd, wordt er een bevestiging weergegeven in de Package Manager-console.

1. De volgende stap is het maken van een platformconfiguratie die overeenkomt met de architectuur van de computer waarmee u de consoletoepassing wilt bouwen en uitvoeren. Selecteer **Build** > **Configuration Manager...** in de menubalk.

    ![Configuration Manager openen](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Configuration Manager openen")

1. Open in het dialoogvenster **Configuration Manager** de vervolgkeuzelijst **Active solution platform** en selecteer **New**.

    ![Een nieuw platform toevoegen in het Configuration Manager-venster](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Een nieuw platform toevoegen in het Configuration Manager-venster")

1. Als u met Windows 64-bits werkt en u de melding **Typ of selecteer het nieuwe platform** ziet, selecteert u `x64`. Werkt u met Windows 32-bits, selecteer dan `x86`. Als u klaar bent, klikt u op **OK**.

    ![In 64-bits Windows voegt u een nieuw platform toe met de naam x64](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "Het platform x64 toevoegen")
