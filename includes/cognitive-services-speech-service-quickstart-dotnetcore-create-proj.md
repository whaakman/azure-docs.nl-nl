---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 12/12/2018
ms.author: erhopf
ms.openlocfilehash: 6e49db90aa9e9f933a190425afbafd15e0057fca
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53729489"
---
1. Start Visual Studio 2017.

1. Zorg ervoor dat de workload voor **.NET platformoverschrijdende ontwikkeling** beschikbaar is. Kies **Extra** > **Hulpmiddelen en onderdelen ophalen** in de menubalk van Visual Studio om het installatieprogramma voor Visual Studio te openen. Als deze workload al is ingeschakeld, sluit u het dialoogvenster.

   ![Schermopname van het Visual Studio-installatieprogramma met het tabblad Workloads gemarkeerd](../articles/cognitive-services/Speech-Service/media/sdk/vs-enable-net-core-workload.png)

   Selecteer anders het vak naast **platformoverschrijdende ontwikkelmogelijkheden van .NET Core** en selecteer **Wijzigen** in de rechterbenedenhoek van het dialoogvenster. De installatie van de nieuwe functie kan even duren.

1. Maak een nieuwe Visual C# .NET Core-consoletoepassing. Vouw in het dialoogvenster **Nieuw project**, vanuit het linkerdeelvenster, **Geïnstalleerd** > **Visual C#** > **.NET Core** uit. Selecteer vervolgens **Consoletoepassing (.NET Core)**. Voer als projectnaam *helloworld* in.

   ![Schermopname van dialoogvenster Nieuw project](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-01-new-console-app.png "Visual C#-consoletoepassing (.NET Core) maken")

1. Installeer en bekijk het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget). Klik in Solution Explorer met de rechtermuisknop op de oplossing en kies **NuGet-pakketten beheren voor oplossing**.

   ![Schermopname van Solution Explorer, met NuGet-pakketten beheren voor oplossing gemarkeerd](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-02-manage-nuget-packages.png "NuGet-pakketten beheren voor oplossing")

1. In de rechterbovenhoek selecteert u in het veld **Pakketbron** de optie **nuget.org**. Zoek het pakket `Microsoft.CognitiveServices.Speech` en installeer het in het project **helloworld**.

   ![Schermafbeelding van het dialoogvenster Manage Packages for Solution](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-03-nuget-install-1.0.0.png "NuGet-pakket installeren")

1. Ga akkoord met de weergegeven licentie om te beginnen met het installeren van het NuGet-pakket.

   ![Schermafbeelding van het dialoogvenster voor akkoord gaan met licentie](../articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-dotnetcore-windows-04-nuget-license.png "Licentie accepteren")

Nadat het pakket is geïnstalleerd, wordt er een bevestiging weergegeven in de Package Manager-console.
