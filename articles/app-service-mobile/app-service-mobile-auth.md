---
title: Verificatie en autorisatie in Azure Mobile Apps | Microsoft Docs
description: Voor conceptuele verwijzing en overzicht van de verificatie / autorisatie-functie voor Azure Mobile Apps
services: app-service\mobile
documentationcenter: 
author: mattchenderson
manager: cfowler
editor: 
ms.assetid: a46dbf70-867d-48f6-8885-7f5207ad102e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 57deec4acffc1387fcb5c42c17085bb363dfdbc1
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Verificatie en autorisatie in Azure Mobile Apps
## <a name="what-is-app-service-authentication--authorization"></a>Wat is er App Service-verificatie / autorisatie?
> [!NOTE]
> In dit artikel worden gemigreerd naar een geconsolideerde [App Service-verificatie / autorisatie](../app-service/app-service-authentication-overview.md) artikel, waarin wordt ingegaan op Web-, mobiele en API-Apps.
> 
> 

App Service-verificatie / autorisatie is een functie waarmee uw toepassing gebruikers aanmelden zonder code wijzigingen vereist op de back-end voor de app. Het biedt een eenvoudige manier voor het beveiligen van uw toepassing en werken met gegevens per gebruiker.

App Service maakt gebruik van federatieve identiteiten waarin 3rd derden **identiteitsprovider** ('IDP') slaat accounts en verificatie van gebruikers. De toepassing gebruikt deze identiteit in plaats van een eigen. App Service ondersteunt vijf identiteitsproviders gebruiksklaar: *Azure Active Directory*, *Facebook*, *Google*, *Microsoft-Account*, en *Twitter*. U kunt ook deze ondersteuning voor uw apps uitbreiden door te integreren van een andere id-provider of een oplossing voor uw eigen aangepaste identiteit.

Uw app kunt een willekeurig aantal deze id-providers gebruiken zodat u uw eindgebruikers met opties leveren kan voor hoe deze zich aanmeldt.

Als u meteen aan de slag wilt, ziet u een van de volgende zelfstudies:

* [Verificatie toevoegen aan uw iOS-app]
* [Verificatie toevoegen aan uw app voor Xamarin.iOS]
* [Verificatie toevoegen aan uw Xamarin.Android-app]
* [Verificatie toevoegen aan uw Windows-app]

## <a name="how-authentication-works"></a>De werking van verificatie
Om te verifiëren met een van de id-providers, moet u eerst de id-provider te weten over uw toepassing configureren. De id-provider krijgt u vervolgens met de id's en geheimen die u terug naar de toepassing opgeven. Dit is de vertrouwensrelatie voltooid en kan App Service identiteiten opgegeven te valideren.

Deze stappen zijn aangegeven in de volgende onderwerpen:

* [Uw app configureren voor aanmelding met Azure Active Directory]
* [Uw app configureren voor aanmelding met Facebook]
* [Uw app configureren voor aanmelding met Google]
* [Uw app configureren voor aanmelding met een Microsoft Account]
* [Uw app configureren voor aanmelding met Twitter]

Zodra u alles is geconfigureerd op de back-end, kunt u de client aan te melden. Er zijn twee benaderingen hier:

* Met behulp van één regel code, kunt u de Mobile Apps SDK clients gebruikers aanmelden.
* Gebruik een SDK gepubliceerd door een opgegeven identiteitsprovider identiteit en vervolgens toegang krijgen tot de App Service.

> [!TIP]
> De meeste toepassingen moeten een provider SDK gebruiken om op te halen van een systeemeigen gevoel aanmeldingservaring en gebruikmaken van vernieuwing ondersteuning en andere voordelen providerspecifieke.
> 
> 

### <a name="how-authentication-without-a-provider-sdk-works"></a>De werking van de verificatie zonder een SDK-provider
Als u niet wenst voor het instellen van een provider SDK, kunt u mobiele Apps voor het uitvoeren van de aanmelding voor u toestaan. De client Mobile Apps SDK een webpagina openen met de provider van uw keuze om uit te voeren op de aanmeldingspagina. Soms ziet u deze werkstroom aangeduid als de 'server stroom' of 'server gerichte stroom' omdat de server wordt beheerd door de aanmelding en de client SDK nooit het provider-token ontvangt.

De code die nodig zijn voor het starten van deze stroom wordt beschreven in de zelfstudie verificatie voor elk platform. De SDK-client heeft een App Service-token aan het einde van stroom en het token is automatisch gekoppeld aan alle aanvragen naar de back-end.

### <a name="how-authentication-with-a-provider-sdk-works"></a>De werking van verificatie met een provider SDK
Werken met een provider kunnen SDK de ervaring aanmelden dichter communiceren met het platform OS dat de app wordt uitgevoerd. De SDK-provider hebt u ook een token van de provider en bepaalde gebruikersgegevens op de client, waardoor het veel eenvoudiger graph API's gebruiken en aanpassen van de gebruikerservaring. Soms ziet u mogelijk deze werkstroom aangeduid als de stroom' client' of 'client omgeleid stroom' sinds de code op de client de aanmelding verwerkt en de clientcode toegang heeft tot een token van de provider.

Nadat u een provider-token wordt ontvangen, moet deze worden verzonden naar App Service voor validatie. De SDK-client heeft een App Service-token aan het einde van stroom en het token is automatisch gekoppeld aan alle aanvragen naar de back-end. De ontwikkelaar kunt ook een verwijzing naar het token provider behouden desgewenst.

## <a name="how-authorization-works"></a>De werking van autorisatie
App Service-verificatie / autorisatie beschrijft de verschillende mogelijkheden voor **te ondernemen actie wanneer de aanvraag is niet geverifieerd**. Voordat uw code een bepaalde aanvraag ontvangt, kunt u App Service wordt gecontroleerd om te zien als de aanvraag is geverifieerd en als niet zo is, weigeren en proberen opnieuw te hebben van de gebruiker zich aanmeldt vóór het opnieuw proberen hebben.

Een mogelijkheid is het niet-geverifieerde aanvragen omleiden naar een van de id-providers hebben. In een webbrowser, zou deze omleiding daadwerkelijk duren voordat de gebruiker naar een nieuwe pagina. Echter uw mobiele clients niet op deze manier worden omgeleid en niet-geverifieerde antwoorden ontvangt een HTTP *401 niet geautoriseerd* antwoord. Daardoor vormen de eerste aanvraag de client maakt moet altijd het eindpunt van de aanmelding en vervolgens kunt u aanroepen naar andere API's. Als u een ander API aanroepen probeert voordat u zich aanmeldt, ontvangt de client een fout opgetreden.

Als u meer gedetailleerde wilt laten bepalen via welke eindpunten verificatie vereisen, kunt u kiezen "Er is geen actie (toestaan aanvraag) ' voor niet-geverifieerde aanvragen. In dit geval worden alle beslissingen voor de verificatie uitgesteld de toepassingscode. Hierdoor kunt u ook toegang tot specifieke gebruikers op basis van aangepaste autorisatieregels.

## <a name="documentation"></a>Documentatie
De volgende zelfstudies laten zien hoe u verificatie toevoegen aan uw mobiele clients met behulp van App Service:

* [Verificatie toevoegen aan uw iOS-app]
* [Verificatie toevoegen aan uw app voor Xamarin.iOS]
* [Verificatie toevoegen aan uw Xamarin.Android-app]
* [Verificatie toevoegen aan uw Windows-app]

De volgende zelfstudies laten zien hoe u App Service voor het gebruik van verschillende verificatieproviders configureren:

* [Uw app configureren voor aanmelding met Azure Active Directory]
* [Uw app configureren voor aanmelding met Facebook]
* [Uw app configureren voor aanmelding met Google]
* [Uw app configureren voor aanmelding met een Microsoft Account]
* [Uw app configureren voor aanmelding met Twitter]

Als u gebruiken op een identiteitssysteem dan degene die wilt voorwaarde hier ook u kunt de [preview-ondersteuning voor aangepaste verificatie op de server .NET SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Verificatie toevoegen aan uw iOS-app]: app-service-mobile-ios-get-started-users.md
[Verificatie toevoegen aan uw app voor Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started-users.md
[Verificatie toevoegen aan uw Xamarin.Android-app]: app-service-mobile-xamarin-android-get-started-users.md
[Verificatie toevoegen aan uw Windows-app]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Uw app configureren voor aanmelding met Azure Active Directory]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Uw app configureren voor aanmelding met Facebook]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Uw app configureren voor aanmelding met Google]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Uw app configureren voor aanmelding met een Microsoft Account]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Uw app configureren voor aanmelding met Twitter]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
