---
title: Verificatie en autorisatie in Azure App Service voor mobiele apps | Microsoft Docs
description: Voor conceptuele verwijzing in en overzicht van de verificatie / autorisatie functie voor Azure App Service, speciaal voor mobiele apps
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
ms.openlocfilehash: 87bdfcc827155e5dd0a02ffb1640bf7e9cd4e479
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53715781"
---
# <a name="authentication-and-authorization-in-azure-app-service-for-mobile-apps"></a>Verificatie en autorisatie in Azure App Service voor mobiele apps

Dit artikel wordt beschreven hoe verificatie en autorisatie werkt bij het ontwikkelen van systeemeigen mobiele apps met een back-end van App Service. App Service biedt geïntegreerde verificatie en autorisatie, zodat uw mobiele apps kunnen gebruikers zich aanmelden zonder een code in App Service te wijzigen. Het biedt een eenvoudige manier voor het beveiligen van uw toepassing en werken met gegevens per gebruiker. 

In dit artikel richt zich op de ontwikkeling van mobiele Apps. Als u wilt snel aan de slag met App Service-verificatie en autorisatie voor uw mobiele app, ziet u een van de volgende zelfstudies [verificatie toevoegen aan uw iOS-app] [ iOS] (of [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms], of [Cordova]). 

Zie voor meer informatie over de werking verificatie en autorisatie in App Service [verificatie en autorisatie in Azure App Service](../app-service/overview-authentication-authorization.md).

## <a name="authentication-with-provider-sdk"></a>Verificatie met de SDK-provider

Nadat u alles in App Service is geconfigureerd, kunt u mobiele clients zich aanmelden met App Service kunt wijzigen. Er zijn hier twee methoden:

* Gebruik een SDK die een bepaalde id-provider voor identiteit en vervolgens toegang krijgen tot App Service publiceert.
* Gebruik één regel code zodat de client-SDK van de Mobile Apps van gebruikers aanmelden kunt.

> [!TIP]
> De meeste toepassingen moeten een provider SDK gebruiken om een consistente gebruikerservaring wanneer gebruikers zich aanmelden, tokenvernieuwing-ondersteuning gebruiken en om op te halen van andere voordelen die de provider.
> 
> 

Wanneer u een SDK-provider gebruikt, kunnen gebruikers zich aanmelden bij een ervaring die meer nauw is geïntegreerd met het besturingssysteem waarop de app wordt uitgevoerd. Deze methode biedt u ook een token van de provider en enkele gebruikersgegevens op de client, waardoor het veel eenvoudiger te gebruiken, grafiek-API's en aanpassen van de gebruikerservaring. Af en toe op blogs en forums, wordt deze aangeduid als de 'clientstroom' of 'client omgeleid stroom' omdat de code op de client zich in gebruikers en de clientcode toegang tot de token van een provider heeft.

Nadat een token van de provider wordt verkregen, moet deze worden verzonden naar App Service voor validatie. Nadat de App Service valideert het token, maakt App Service een nieuw App Service-token dat wordt geretourneerd naar de client. De SDK voor Mobile Apps-client heeft helpermethoden voor het beheren van deze exchange en koppelt het token automatisch op alle aanvragen naar de back-end van de toepassing. Ontwikkelaars kunnen ook een verwijzing naar het token provider houden.

Zie voor meer informatie over de verificatiestroom [App Service-verificatiestroom](../app-service/overview-authentication-authorization.md#authentication-flow). 

## <a name="authentication-without-provider-sdk"></a>Verificatie zonder SDK-provider

Als u niet instellen van een SDK-provider wilt, kunt u de functie Mobile Apps van Azure App Service aan te melden voor u. De client-SDK van de Mobile Apps opent u een weergave webpagina met de provider van uw keuze en meld u aan de gebruiker. Af en toe op blogs en forums, wordt deze aangeroepen de 'server stroom' of 'server gerichte stroom' omdat de server beheert het proces dat gebruikers worden aangemeld en de client-SDK nooit een token van de provider ontvangt.

Code voor het starten van deze stroom is opgenomen in de verificatie-zelfstudie voor elk platform. De client-SDK heeft de token van een App Service aan het einde van de stroom, en het token is automatisch gekoppeld aan alle aanvragen naar de back-end van toepassing.

Zie voor meer informatie over de verificatiestroom [App Service-verificatiestroom](../app-service/overview-authentication-authorization.md#authentication-flow). 
## <a name="more-resources"></a>Meer bronnen

De volgende zelfstudies laten zien hoe u verificatie toevoegen aan uw mobiele clients met behulp van de [server gerichte stroom](../app-service/overview-authentication-authorization.md#authentication-flow):

* [Verificatie toevoegen aan uw iOS-app][iOS]
* [Verificatie toevoegen aan uw Android-app][Android]
* [Verificatie toevoegen aan uw Windows-app][Windows]
* [Verificatie toevoegen aan uw Xamarin.iOS-app][Xamarin.iOS]
* [Verificatie toevoegen aan uw Xamarin.Android-app][Xamarin.Android]
* [Verificatie toevoegen aan uw app Xamarin.Forms][Xamarin.Forms]
* [Verificatie toevoegen aan uw Cordova-app][Cordova]

Gebruik de volgende bronnen als u wilt gebruiken de [client omgeleid stroom](../app-service/overview-authentication-authorization.md#authentication-flow) voor Azure Active Directory:

* [Gebruik de Active Directory-Verificatiebibliotheek voor iOS][ADAL-iOS]
* [Gebruik de Active Directory-Verificatiebibliotheek voor Android][ADAL-Android]
* [Gebruik de Active Directory Authentication Library voor Windows en Xamarin][ADAL-dotnet]

Gebruik de volgende bronnen als u wilt gebruiken de [client omgeleid stroom](../app-service/overview-authentication-authorization.md#authentication-flow) voor Facebook:

* [Gebruik de Facebook-SDK voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Gebruik de volgende bronnen als u wilt gebruiken de [client omgeleid stroom](../app-service/overview-authentication-authorization.md#authentication-flow) voor Twitter:

* [Twitter-infrastructuur voor iOS gebruiken](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Gebruik de volgende bronnen als u wilt gebruiken de [client omgeleid stroom](../app-service/overview-authentication-authorization.md#authentication-flow) voor Google:

* [Gebruik de Google-In SDK voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service/configure-authentication-provider-aad.md
[Facebook]: ../app-service/configure-authentication-provider-facebook.md
[Google]: configure-authentication-provider-google.md
[MSA]: ../app-service/configure-authentication-provider-microsoft.md
[Twitter]: ../app-service/configure-authentication-provider-twitter.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
