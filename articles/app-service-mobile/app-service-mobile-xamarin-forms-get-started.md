---
title: Aan de slag met Mobile Apps met behulp van Xamarin.Forms
description: Volg deze zelfstudie om aan de slag te gaan met Mobile Apps voor Xamarin.Forms-ontwikkeling
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 01a8e356649c070a5cf36ae070195abce699f85e
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-xamarinforms-app"></a>Een Xamarin.Forms-app maken
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u een cloudgebaseerde back-endservice toevoegt aan een mobiele Xamarin.Forms-app door de functie Mobile Apps van Azure App Service als de back-end te gebruiken. U maakt zowel een nieuwe back-end voor Mobile Apps als een eenvoudige Xamarin.Forms-app voor takenlijsten die app-gegevens opslaat in Azure.

Het voltooien van deze zelfstudie is een vereiste voor alle andere zelfstudies over Mobile Apps voor Xamarin.Forms.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een actief Azure-account. Als u geen account hebt, kunt u zich aanmelden voor een proefversie van Azure en maximaal tien gratis mobiele apps krijgen die u ook na de proefperiode kunt blijven gebruiken. Zie [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

* Visual Studio met Xamarin. Zie [Setup and install](https://msdn.microsoft.com/library/mt613162.aspx) (Configureren en installeren) voor meer informatie.

* Een Mac met Xcode v7.0 of hoger en waarop Xamarin Studio Community is geïnstalleerd. Zie [Setup and install](https://msdn.microsoft.com/library/mt613162.aspx) (Configureren en installeren) en [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (Instructies voor installatie, configuratie en verificatie voor Mac-gebruikers) (MSDN) voor meer informatie.

## <a name="create-a-new-mobile-apps-back-end"></a>Een nieuwe back-end voor Mobile Apps maken

Doe het volgende om een nieuwe back-end voor Mobile Apps te maken:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

U hebt nu een back-end voor Mobile Apps ingesteld die uw mobiele clienttoepassingen kunnen gebruiken. Nu gaat u een serverproject downloaden voor een eenvoudige back-end voor takenlijsten en deze vervolgens publiceren naar Azure.

## <a name="configure-the-server-project"></a>Het serverproject configureren

Doe het volgende om het serverproject te configureren voor het gebruik van de Node.js- of .NET-back-end:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>De Xamarin.Forms-oplossing downloaden en uitvoeren

U kunt de oplossing op twee manieren downloaden. Download de oplossing naar een Mac en open deze in Xamarin Studio of download naar een Windows-computer en open de oplossing in Visual Studio en gebruik een Mac in het netwerk om de iOS-app te bouwen. Zie [Setup and install](https://msdn.microsoft.com/library/mt613162.aspx) (Configureren en installeren) voor meer informatie.

Ga als volgt te werk op een Mac of Windows-computer:

1. Ga naar de [Azure Portal].

2. In de blade Instellingen voor uw mobiele app klikt u op **Quickstart** (onder Implementatie) > **Xamarin.Forms**. Klik in stap 3 op  **Een nieuwe app maken** als deze optie nog niet is geselecteerd.  Klik vervolgens op de knop **Downloaden**.

   Er wordt nu een project gedownload dat een clienttoepassing bevat die is verbonden met uw mobiele app. Sla het gecomprimeerde projectbestand op uw lokale computer op en noteer de opslaglocatie.

3. Pak het project uit dat u hebt gedownload en open het in Xamarin Studio (Mac) of Visual Studio (Windows).

   ![Uitgepakt project in Xamarin Studio][9]

   ![Uitgepakt project in Visual Studio][8]

## <a name="optional-run-the-ios-project"></a>(Optioneel) Het iOS-project uitvoeren
In deze sectie gaat u het Xamarin iOS-project uitvoeren voor iOS-apparaten. Als u niet met iOS-apparaten werkt, kunt u deze sectie overslaan.

#### <a name="in-xamarin-studio"></a>In Xamarin Studio
1. Klik met de rechtermuisknop op het iOS-project en selecteer vervolgens **Set As Startup Project**.

2. Selecteer **Start Debugging** in het menu **Run** om het project te bouwen en de app te starten in de iPhone-emulator.

#### <a name="in-visual-studio"></a>In Visual Studio
1. Klik met de rechtermuisknop op het iOS-project en selecteer vervolgens **Set as StartUp Project**.

2. Selecteer **Configuration Manager** in het menu **Bouwen**.

3. Schakel in het dialoogvenster **Configuration Manager** de selectievakjes **Bouwen** en **Implementeren** in voor het iOS-project.

4. Selecteer de toets **F5** om het project te bouwen en de app te starten in de iPhone-emulator.

   > [!NOTE]
   > Als u er problemen zijn met het bouwen van het project, voert u NuGet Package Manager uit om bij te werken naar de nieuwste versie van de Xamarin-ondersteuningspakketten. Het is mogelijk dat het even duurt voordat Quick Start-projecten zijn bijgewerkt naar de nieuwste versie.    
   >
   >

5. Typ zinvolle tekst in de app, zoals *Xamarin leren kennen*, en selecteer vervolgens het plusteken (**+**).

    ![][10]

    Hierdoor wordt er een POST-aanvraag verzonden naar de nieuwe back-end voor Mobile Apps die wordt gehost in Azure. Gegevens van de aanvraag worden opgenomen in de takentabel. Items die zijn opgeslagen in de tabel, worden geretourneerd door de back-end voor Mobile Apps en de gegevens worden weergegeven in de lijst.

    > [!NOTE]
    > U vindt de code die toegang geeft tot de back-end voor Mobile Apps in het bestand TodoItemManager.cs C# van het 'portable class library'-project van uw oplossing.
    >
    >

## <a name="optional-run-the-android-project"></a>(Optioneel) Het Android-project uitvoeren
In deze sectie gaat u het Xamarin Droid-project voor Android uitvoeren. Als u niet met Android-apparaten werkt, kunt u deze sectie overslaan.

#### <a name="in-xamarin-studio"></a>In Xamarin Studio

1. Klik met de rechtermuisknop op het Android-project en selecteer vervolgens **Set As Startup Project**.

2. Selecteer **Start Debugging** in het menu **Run** om het project te bouwen en de app te starten in een Android-emulator.

#### <a name="in-visual-studio"></a>In Visual Studio

1. Klik met de rechtermuisknop op het Android-project (Droid) en selecteer vervolgens **Instellen als opstartproject**.

2. Selecteer **Configuration Manager** in het menu **Bouwen**.

3. Schakel in het dialoogvenster **Configuration Manager** de selectievakjes **Bouwen** en **Implementeren** in voor het Android-project.

4. Selecteer de toets **F5** om het project te bouwen en de apps te starten in een Android-emulator.

   > [!NOTE]
   > Als u er problemen zijn met het bouwen van het project, voert u NuGet Package Manager uit om bij te werken naar de nieuwste versie van de Xamarin-ondersteuningspakketten. Het is mogelijk dat het even duurt voordat Quick Start-projecten zijn bijgewerkt naar de nieuwste versie.    
   >
   >

5. Typ zinvolle tekst in de app, zoals *Xamarin leren kennen*, en selecteer vervolgens het plusteken (**+**).

    ![][11]
    
    Hierdoor wordt er een POST-aanvraag verzonden naar de nieuwe back-end voor Mobile Apps die wordt gehost in Azure. Gegevens van de aanvraag worden opgenomen in de takentabel. Items die zijn opgeslagen in de tabel, worden geretourneerd door de back-end voor Mobile Apps en de gegevens worden weergegeven in de lijst.
    
    > [!NOTE]
    > U vindt de code die toegang geeft tot de back-end voor Mobile Apps in het bestand TodoItemManager.cs C# van het 'portable class library'-project van uw oplossing.
    >
    >

## <a name="optional-run-the-windows-project"></a>(Optioneel) Het Windows-project uitvoeren

In deze sectie gaat u het project Xamarin WinApp uitvoeren voor Windows-apparaten. Als u niet met Windows-apparaten werkt, kunt u deze sectie overslaan.

#### <a name="in-visual-studio"></a>In Visual Studio

1. Klik met de rechtermuisknop op een van de Windows-projecten en selecteer vervolgens **Instellen als opstartproject**.

2. Selecteer **Configuration Manager** in het menu **Bouwen**.

3. Schakel in het dialoogvenster **Configuration Manager** de selectievakjes **Bouwen** en **Implementeren** in van het Windows-project dat u hebt gekozen.

4. Selecteer de toets **F5** om het project te bouwen en de apps te starten in een Windows-emulator.

   > [!NOTE]
   > Als u er problemen zijn met het bouwen van het project, voert u NuGet Package Manager uit om bij te werken naar de nieuwste versie van de Xamarin-ondersteuningspakketten. Het is mogelijk dat het even duurt voordat Quick Start-projecten zijn bijgewerkt naar de nieuwste versie.    
   >
   >

5. Typ zinvolle tekst in de app, zoals *Xamarin leren kennen*, en selecteer vervolgens het plusteken (**+**).

    Hierdoor wordt er een POST-aanvraag verzonden naar de nieuwe back-end voor Mobile Apps die wordt gehost in Azure. Gegevens van de aanvraag worden opgenomen in de takentabel. Items die zijn opgeslagen in de tabel, worden geretourneerd door de back-end voor Mobile Apps en de gegevens worden weergegeven in de lijst.
    
    ![][12]
    
    > [!NOTE]
    > U vindt de code die toegang geeft tot de back-end voor Mobile Apps in het bestand TodoItemManager.cs C# van het 'portable class library'-project van uw oplossing.
    >
    >

## <a name="next-steps"></a>Volgende stappen

* [Verificatie toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-users.md)  
  Ontdek hoe u gebruikers van uw app verifieert met een id-provider.

* [Pushmeldingen toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-push.md)  
  Lees hoe u ondersteuning voor pushmeldingen toevoegt aan uw app en de backend voor Mobile Apps configureert voor het gebruik van Azure Notification Hubs voor het verzenden van pushmeldingen.

* [Offlinesynchronisatie voor uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Ontdek hoe u offlineondersteuning voor uw app toevoegt met behulp van een back-end voor Mobile Apps. Offline synchroniseren zorgt ervoor dat u gegevens van een mobiele app kunt weergeven, toevoegen of wijzigen, zelfs als er geen netwerkverbinding is.

* [De beheerde client gebruiken voor Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
  Informatie over het werken met SDK voor beheerde clients in uw Xamarin-app.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/
