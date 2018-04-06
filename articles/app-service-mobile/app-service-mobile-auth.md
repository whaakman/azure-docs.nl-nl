---
title: Verificatie en autorisatie in Azure App Service voor mobiele apps | Microsoft Docs
description: Voor conceptuele verwijzing en overzicht van de verificatie / autorisatie functie voor Azure App Service, speciaal voor mobiele apps
services: app-service
documentationcenter: ''
author: mattchenderson
manager: erikre
editor: ''
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 237310c607eb8488e53631b6e69d01703d1ebf99
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="authentication-and-authorization-in-azure-app-service-for-mobile-apps"></a>Verificatie en autorisatie in Azure App Service voor mobiele apps

In dit artikel wordt beschreven hoe verificatie en autorisatie werkt bij het ontwikkelen van systeemeigen mobiele apps met de back-end van een App Service. App Service biedt geïntegreerde verificatie en autorisatie, zodat uw mobiele apps gebruikers aanmelden kunnen zonder een code in App Service te wijzigen. Het biedt een eenvoudige manier voor het beveiligen van uw toepassing en werken met gegevens per gebruiker. 

Dit artikel is gericht op de mobiele app-ontwikkeling. Zie een van de volgende zelfstudies om snel aan de slag met App Service-verificatie en autorisatie voor uw mobiele app, [verificatie toevoegen aan uw iOS-app] [ iOS] (of [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms], of [Cordova ]). 

Zie voor meer informatie over de werking verificatie en autorisatie in App Service [verificatie en autorisatie in Azure App Service](../app-service/app-service-authentication-overview.md).

## <a name="authentication-with-provider-sdk"></a>Verificatie met SDK-provider

Nadat u alles in App Service is geconfigureerd, kunt u mobiele clients zich aanmelden met een App Service wijzigen. Er zijn twee benaderingen hier:

* Gebruik een SDK die een opgegeven id-provider voor het vaststellen van identiteit en vervolgens toegang krijgen tot de App Service publiceert.
* Gebruik één regel code zodat de client-Mobile Apps SDK gebruikers kunt aanmelden.

> [!TIP]
> De meeste toepassingen moeten een provider SDK gebruiken om een consistente gebruikerservaring wanneer gebruikers zich aanmelden, token vernieuwen-ondersteuning en om op te halen van andere voordelen van de provider geeft.
> 
> 

Wanneer u een provider SDK gebruikt, kunnen gebruikers zich kunnen aanmelden bij een werkwijze die meer nauw geïntegreerd met het besturingssysteem dat de app wordt uitgevoerd op. Deze methode kunt u ook een token van de provider en bepaalde gebruikersgegevens op de client, die veel gemakkelijker graph API's gebruiken en aanpassen van de gebruikerservaring. Van tijd tot tijd op blogs en forums wordt dit aangeduid als de 'stroom' of 'client omgeleid stroom' omdat de code op de client zich aanmeldt gebruikers en de clientcode toegang tot de token van een provider heeft.

Wanneer een provider-token wordt ontvangen, moet deze worden verzonden naar App Service voor validatie. Nadat de App Service valideert het token, maakt de App Service een nieuwe App Service-token dat wordt geretourneerd naar de client. De client-Mobile Apps SDK heeft Help-methoden voor het beheren van deze exchange- en automatisch aan alle aanvragen naar de back-end van de toepassing koppelen het token. Ontwikkelaars kunnen ook een verwijzing naar het token provider behouden.

Zie voor meer informatie over de authenticatiestroom [App Service-authenticatiestroom](../app-service/app-service-authentication-overview.md#authentication-flow). 

## <a name="authentication-without-provider-sdk"></a>Authenticatie zonder provider SDK

Als u niet instellen van een provider SDK wilt, kunt u de functie Mobile Apps van Azure App Service aan te melden voor u. De client-Mobile Apps SDK wordt een webpagina weergave opent om de provider van uw keuze en meld u aan de gebruiker. Tijd tot tijd op de blogs en forums wordt genoemd de 'server stroom' of 'server gerichte stroom' omdat de server beheert het proces dat gebruikers worden aangemeld en de client SDK nooit het provider-token ontvangt.

Code voor het starten van deze stroom is opgenomen in de zelfstudie verificatie voor elk platform. De SDK-client heeft een App Service-token aan het einde van stroom en het token is automatisch gekoppeld aan alle aanvragen naar de back-end voor de toepassing.

Zie voor meer informatie over de authenticatiestroom [App Service-authenticatiestroom](../app-service/app-service-authentication-overview.md#authentication-flow). 
## <a name="more-resources"></a>Meer bronnen

De volgende zelfstudies laten zien hoe u verificatie toevoegen aan uw mobiele clients met behulp van de [server gerichte stroom](../app-service/app-service-authentication-overview.md#authentication-flow):

* [Verificatie toevoegen aan uw iOS-app][iOS]
* [Verificatie toevoegen aan uw Android-app][Android]
* [Verificatie toevoegen aan uw Windows-app][Windows]
* [Verificatie toevoegen aan uw app voor Xamarin.iOS][Xamarin.iOS]
* [Verificatie toevoegen aan uw Xamarin.Android-app][Xamarin.Android]
* [Verificatie toevoegen aan uw app Xamarin.Forms][Xamarin.Forms]
* [Verificatie toevoegen aan uw Cordova-app][Cordova ]

Gebruik de volgende bronnen als u wilt gebruiken de [client omgeleid stroom](../app-service/app-service-authentication-overview.md#authentication-flow) voor Azure Active Directory:

* [Gebruik de Active Directory Authentication Library voor iOS][ADAL-iOS]
* [Gebruik de Active Directory Authentication Library voor Android][ADAL-Android]
* [De Active Directory Authentication Library voor Windows en Xamarin gebruiken][ADAL-dotnet]

Gebruik de volgende bronnen als u wilt gebruiken de [client omgeleid stroom](../app-service/app-service-authentication-overview.md#authentication-flow) voor Facebook:

* [Gebruik de Facebook-SDK voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Gebruik de volgende bronnen als u wilt gebruiken de [client omgeleid stroom](../app-service/app-service-authentication-overview.md#authentication-flow) voor Twitter:

* [Gebruik Twitter Fabric voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Gebruik de volgende bronnen als u wilt gebruiken de [client omgeleid stroom](../app-service/app-service-authentication-overview.md#authentication-flow) voor Google:

* [Gebruik de Google-In SDK voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova ]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
