---
title: Aan de slag met Azure Mobile Apps voor Xamarin.Android-apps
description: Volg deze zelfstudie om aan de slag te gaan met Azure Mobile Apps voor Xamarin.Android-ontwikkeling
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: f3e8ca4f9736dffe4928fc8920b0890dff87367b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236030"
---
# <a name="create-a-xamarinandroid-app"></a>Een Xamarin.Android-app maken
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u een back-endservice toevoegt aan een Xamarin.Android-app in de cloud. Zie [What are Mobile Apps](app-service-mobile-value-prop.md) (Wat zijn Mobile Apps?) voor meer informatie.

Hieronder ziet u een schermafbeelding van de voltooide app:

![][0]

Het voltooien van deze zelfstudie is een vereiste voor alle andere Mobile Apps-zelfstudies voor Xamarin.Android-apps.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* Een actief Azure-account. Als u geen account hebt, meld u zich aan voor een proefversie van Azure en ontvangt u maximaal 10 gratis mobiele apps. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.
* Visual Studio met Xamarin. Zie [Setup and install for Visual Studio and Xamarin](/visualstudio/cross-platform/setup-and-install) (Installeren en instellen voor Visual Studio en Xamarin) voor instructies.

## <a name="create-an-azure-mobile-app-backend"></a>Een back-end voor mobiele apps van Azure maken
Volg deze stappen voor het maken van een back-end voor mobiele apps.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

U hebt nu een back-end voor mobiele apps van Azure ingericht, die kan worden gebruikt door uw mobiele-clienttoepassingen. Download vervolgens een serverproject voor een eenvoudige back-end voor takenlijsten en publiceer deze naar Azure.

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>De verbinding met een database maken en configureren van het project client en server
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinandroid-app"></a>De Xamarin.Android-app uitvoeren
1. Open het Xamarin.Android-project.

2. Ga naar de [Azure-portal](https://portal.azure.com/) en navigeer naar de mobiele app die u hebt gemaakt. Op de `Overview` blade, zoekt u de URL op die het openbare eindpunt voor uw mobiele app. Voorbeeld: de sitenaam voor mijn app-naam 'test123' worden https://test123.azurewebsites.net.

3. Open het bestand `ToDoActivity.cs` in deze map - xamarin.android/ZUMOAPPNAME/ToDoActivity.cs. De toepassingsnaam is `ZUMOAPPNAME`.

4. In `ToDoActivity` klasse, Vervang `ZUMOAPPURL` met openbare eindpunt van het bovenstaande variabele.

    `const string applicationURL = @"ZUMOAPPURL";`

    wordt
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Druk op F5 om te implementeren en uitvoeren van de app.

6. Typ zinvolle tekst in de app, zoals *Voltooi de zelfstudie*, en klik vervolgens op de knop **Toevoegen**.

    ![][10]

    Gegevens van de aanvraag worden opgenomen in de takentabel. Items die zijn opgeslagen in de tabel, worden geretourneerd door de back-end voor mobiele apps en de gegevens verschijnen in de lijst.

   > [!NOTE]
   > U kunt de code die toegang heeft tot de back-end van uw mobiele app bekijken om gegevens op te vragen en in te voegen. Deze code vindt u in het C#-bestand ToDoActivity.cs.
   
## <a name="troubleshooting"></a>Problemen oplossen
Als u er problemen zijn met het bouwen van de oplossing, voert u NuGet Package Manager uit en werkt u de `Xamarin.Android`-ondersteuningspakketten bij. Quick Start-projecten bevatten mogelijk niet altijd de nieuwste versies.

Alle ondersteuningspakketten waarnaar wordt verwezen in uw project, moeten dezelfde versie hebben. Het [Azure Mobile Apps NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) heeft `Xamarin.Android.Support.CustomTabs`-afhankelijkheid voor het Android-platform, dus als uw project gebruikmaakt van nieuwere ondersteuningspakketten, moet u dit pakket met de vereiste versie rechtstreeks installeren om conflicten te voorkomen.

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png
<!-- URLs. -->
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
