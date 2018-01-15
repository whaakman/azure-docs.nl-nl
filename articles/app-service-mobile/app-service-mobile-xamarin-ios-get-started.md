---
title: Aan de slag met Azure App Service Mobile Apps voor Xamarin.iOS-apps | Microsoft Docs
description: Volg deze zelfstudie om aan de slag te gaan met Mobile Apps voor Xamarin.iOS-ontwikkeling.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: 
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 8b890630d352619da86c3017426e24f55ef016d9
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-xamarinios-app"></a>Een Xamarin.iOS-app maken
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u een cloudgebaseerde back-endservice toevoegt aan een mobiele Xamarin.iOS-app met een back-end voor mobiele apps van Azure.  U maakt zowel een nieuwe back-end voor mobiele apps als een eenvoudige Xamarin.iOS-app voor *takenlijsten* die app-gegevens opslaat in Azure.

Het volgen van deze zelfstudie is een vereiste voor alle andere Xamarin.iOS- zelfstudies over het gebruik van de Mobile Apps-functie in Azure App Service.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* Een actief Azure-account. Als u geen account hebt, meldt u zich aan voor een proefversie van Azure en krijgt u maximaal tien gratis mobiele apps die u ook na de proefperiode kunt blijven gebruiken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.
* Visual Studio met Xamarin. Zie [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (Installeren en instellen voor Visual Studio en Xamarin) voor instructies.
* Een Mac met Xcode v7.0 of hoger en waarop Xamarin Studio Community is geïnstalleerd. Zie [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (Installeren en instellen voor Visual Studio en Xamarin) en [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (Instructies voor installatie, configuratie en verificatie voor Mac-gebruikers) (MSDN).

## <a name="create-an-azure-mobile-app-backend"></a>Een back-end voor mobiele apps van Azure maken
Volg deze stappen voor het maken van een back-end voor mobiele apps.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Het serverproject configureren
U hebt nu een back-end voor mobiele apps van Azure ingericht, die kan worden gebruikt door uw mobiele-clienttoepassingen. Download vervolgens een serverproject voor een eenvoudige back-end voor takenlijsten en publiceer deze naar Azure.

Volg de onderstaande stappen voor het configureren van het serverproject voor het gebruik van de Node.js- of .NET-back-end.

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>De Xamarin iOS-app downloaden en uitvoeren
1. Open de [Azure Portal] in een browservenster.
2. Klik op de blade Instellingen voor uw Mobile App op **Aan de slag** > **Xamarin.iOS**. Klik in stap 3 op **Een nieuwe app maken** als deze optie nog niet is geselecteerd.  Klik vervolgens op de knop **Downloaden**.

      Een clienttoepassing die verbinding maakt met uw mobiele back-end wordt gedownload. Sla het gecomprimeerde projectbestand op uw lokale computer op en noteer de opslaglocatie.
3. Pak het project uit dat u hebt gedownload en open het in Xamarin Studio (of Visual Studio).

    ![][9]

    ![][8]
4. Druk op F5 om het project te bouwen en de app te starten in de iPhone-emulator.
5. Typ in de app zinvolle tekst, zoals *Xamarin leren kennen*, en klik vervolgens op de knop **+**.

    ![][10]

    Gegevens van de aanvraag worden opgenomen in de takentabel. Items die zijn opgeslagen in de tabel, worden geretourneerd door de back-end voor mobiele apps en de gegevens worden weergegeven in de lijst.

> [!NOTE]
> U kunt de code die toegang geeft tot uw back-end voor mobiele apps, controleren om gegevens in het QSTodoService.cs C#-bestand op te vragen en in te voegen.
>
>

## <a name="next-steps"></a>Volgende stappen
* [Offlinesynchronisatie toevoegen aan uw app](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Verificatie toevoegen aan uw app ](app-service-mobile-xamarin-ios-get-started-users.md)
* [Pushmeldingen toevoegen aan uw Xamarin.Android-app](app-service-mobile-xamarin-ios-get-started-push.md)
* [De beheerde client gebruiken voor Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/
