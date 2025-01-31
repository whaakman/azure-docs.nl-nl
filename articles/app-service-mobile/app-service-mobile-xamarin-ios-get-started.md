---
title: Aan de slag met Azure App Service Mobile Apps voor Xamarin.iOS-apps | Microsoft Docs
description: Volg deze zelfstudie om aan de slag te gaan met Mobile Apps voor Xamarin.iOS-ontwikkeling.
services: app-service\mobile
documentationcenter: xamarin
author: elamalani
manager: crdun
editor: ''
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 57867eeca9f29cfc3983cbdca94c830aa7a20500
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446248"
---
# <a name="create-a-xamarinios-app"></a>Een Xamarin.iOS-app maken
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Visual Studio App Center investeert in nieuwe en geïntegreerde services centraal staat in de ontwikkeling van mobiele Apps. Ontwikkelaars kunnen gebruikmaken van **bouwen**, **Test** en **verdelen** services voor het instellen van de pijplijn voor continue integratie en levering. Zodra de app is geïmplementeerd, ontwikkelaars controleren de status en het gebruik van het gebruik van de app de **Analytics** en **Diagnostics** -services en Communiceer met gebruikers met behulp van de **Push** de service. Ontwikkelaars kunnen ook gebruikmaken van **Auth** om hun gebruikers te verifiëren en **gegevens** service behouden en synchroniseren van app-gegevens in de cloud. Bekijk [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-xamarin-ios-get-started) vandaag nog.
>

## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u een cloudgebaseerde back-endservice toevoegt aan een mobiele Xamarin.iOS-app met een back-end voor mobiele apps van Azure.  U maakt zowel een nieuwe back-end voor mobiele apps als een eenvoudige Xamarin.iOS-app voor *takenlijsten* die app-gegevens opslaat in Azure.

Het volgen van deze zelfstudie is een vereiste voor alle andere Xamarin.iOS- zelfstudies over het gebruik van de Mobile Apps-functie in Azure App Service.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* Een actief Azure-account. Als u geen account hebt, meldt u zich aan voor een proefversie van Azure en krijgt u maximaal tien gratis mobiele apps die u ook na de proefperiode kunt blijven gebruiken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.
* Visual Studio voor Mac. Zie [instellen en installeren voor Visual Studio voor Mac](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019)
* Een Mac met Xcode 9.0 of hoger.
  
## <a name="create-an-azure-mobile-app-backend"></a>Een back-end voor mobiele apps van Azure maken
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>De verbinding met een database maken en configureren van het project client en server
[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinios-app"></a>De app voor Xamarin.iOS uitvoeren
1. Open het Xamarin.iOS-project.

2. Ga naar de [Azure-portal](https://portal.azure.com/) en navigeer naar de mobiele app die u hebt gemaakt. Op de `Overview` blade, zoekt u de URL op die het openbare eindpunt voor uw mobiele app. Voorbeeld: de sitenaam voor mijn app-naam 'test123' worden https://test123.azurewebsites.net.

3. Open het bestand `QSTodoService.cs` in deze map - xamarin.iOS/ZUMOAPPNAME. De toepassingsnaam is `ZUMOAPPNAME`.

4. In `QSTodoService` klasse, Vervang `ZUMOAPPURL` met openbare eindpunt van het bovenstaande variabele.

    `const string applicationURL = @"ZUMOAPPURL";`

    wordt
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Druk op F5 om te implementeren en uitvoeren van de app in een iPhone-emulator.

6. Typ in de app zinvolle tekst, zoals *Voltooi de zelfstudie* en klik vervolgens op de knop +.

    ![][10]

    Gegevens van de aanvraag worden opgenomen in de takentabel. Items die zijn opgeslagen in de tabel, worden geretourneerd door de back-end voor mobiele apps en de gegevens verschijnen in de lijst.

   > [!NOTE]
   > U kunt de code die toegang heeft tot de back-end van uw mobiele app bekijken om gegevens op te vragen en in te voegen. Deze code vindt u in het C#-bestand ToDoActivity.cs.
   
<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png