---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/13/2018
ms.author: wolfma
ms.openlocfilehash: 8ff8e8341b6f39f66c2cc8014d41d3d3a2918d2b
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47454529"
---
1. Open Visual Studio 2017.
 
1. Controleer of de werkbelasting voor de **.NET-desktopomgeving** beschikbaar is. Kies **Extra** \> **Hulpmiddelen en onderdelen ophalen** in de menubalk van Visual Studio om het installatieprogramma voor Visual Studio te openen. Als de werkbelasting al is ingeschakeld, sluit u het dialoogvenster. 

    Schakel anders het selectievakje in naast **.NET-desktopontwikkeling** en klik rechtsonder in het dialoogvenster op **Wijzigen**. De installatie van de nieuwe functie kan even duren.

    ![.NET-desktopontwikkeling inschakelen](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Maak een nieuwe Visual C#-consoletoepassing. In het dialoogvenster **Nieuw project** in het linkerdeelvenster vouwt u **Geïnstalleerd** \> **Visual C#** \> **Windows-bureaublad** uit en kiest u **Consoletoepassing (.NET Framework)**. Voer als projectnaam *helloworld* in.

    ![Visual C#-consoletoepassing maken (.NET Framework)](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-01-new-console-app.png "Visual C#-consoletoepassing maken (.NET Framework)")

1. Installeer en bekijk het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget). Klik in Solution Explorer met de rechtermuisknop op de oplossing en kies **NuGet-pakketten beheren voor oplossing**.

    ![Met de rechtermuisknop klikken op NuGet-pakketten beheren voor oplossing](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-02-manage-nuget-packages.png "NuGet-pakketten beheren voor oplossing")

1. In de rechterbovenhoek selecteert u in het veld **Pakketbron** de optie **nuget.org**. Zoek het pakket `Microsoft.CognitiveServices.Speech` en installeer het in het project **helloworld**.

    ![Het NuGet-pakket Microsoft.CognitiveServices.Speech installeren](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png "NuGet-pakket installeren")

1. Accepteer de weergegeven licentie om te beginnen met het installeren van het NuGet-pakket.

    ![Accepteer de licentie](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-04-nuget-license.png "Accepteer de licentie")

    Wanneer het pakket is geïnstalleerd, wordt er een bevestiging weergegeven in de Package Manager-console.

1. Maak via Configuration Manager een platformconfiguratie die overeenkomt met de architectuur van uw pc. Selecteer **Bouwen** > **Configuration Manager**.

    ![Configuration Manager openen](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-05-cfg-manager-click.png "Configuration Manager openen")

1. In het **Configuration Manager**-dialoogvenster voegt u een nieuw platform toe. In de vervolgkeuzelijst **Platform actieve oplossing** selecteert u **Nieuw**.

    ![Een nieuw platform toevoegen in het Configuration Manager-venster](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-06-cfg-manager-new.png "Een nieuw platform toevoegen in het Configuration Manager-venster")

1. Als u 64-bits Windows uitvoert, maakt u een nieuwe platformconfiguratie met de naam `x64`. Als u 32-bits Windows uitvoert, maakt u een nieuwe platformconfiguratie met de naam `x86`.

    ![In 64-bits Windows voegt u een nieuw platform toe met de naam x64](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-07-cfg-manager-add-x64.png "Het platform x64 toevoegen")


