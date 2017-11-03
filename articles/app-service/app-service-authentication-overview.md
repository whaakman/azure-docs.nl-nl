---
title: Verificatie en autorisatie in Azure App Service | Microsoft Docs
description: Voor conceptuele verwijzing en overzicht van de verificatie / autorisatie-functie voor Azure App Service
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: f0d2644903181cd2e20166feae4f90ddd4037fa8
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Verificatie en autorisatie in Azure App Service
## <a name="what-is-app-service-authentication--authorization"></a>Wat is er App Service-verificatie / autorisatie?
App Service-verificatie / autorisatie is een functie die een manier om uw toepassing aan te melden gebruikers biedt zodat u hoeft de code op de back-end voor de app wijzigen. Het biedt een eenvoudige manier voor het beveiligen van uw toepassing en werken met gegevens per gebruiker.

App Service maakt gebruik van federatieve identiteiten, waarbij een derde partij identiteitsprovider accounts worden opgeslagen en verificatie van gebruikers. De toepassing is afhankelijk van de gegevens van de identiteit van de provider zodat de app niet hebben om op te slaan die informatie zelf. App Service ondersteunt vijf identiteitsproviders gebruiksklaar: Azure Active Directory, Facebook, Google, Microsoft-Account en Twitter. Uw app kunt een willekeurig aantal deze id-providers gebruiken voor uw gebruikers met opties voor hoe ze zich aanmelden. Als de ingebouwde ondersteuning wilt uitbreiden, kunt u een andere identiteitsprovider integreren of [uw eigen aangepaste identiteitsoplossing][custom-auth].

Als u meteen aan de slag wilt, ziet u een van de volgende zelfstudies [verificatie toevoegen aan uw iOS-app] [ iOS] (of [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms], of [Cordova]).

## <a name="how-authentication-works-in-app-service"></a>De werking van verificatie in App Service
Om te verifiëren met behulp van een van de id-providers, moet u eerst de id-provider te weten over uw toepassing configureren. De id-provider vervolgens biedt-id's en geheimen die u in App Service opgeeft. Dit is de vertrouwensrelatie voltooid zodat de App Service kunt controleren of de gebruiker asserties, zoals verificatietokens door de identiteitsprovider.

Als u wilt een gebruiker met behulp van een van deze providers aanmelden, moet de gebruiker worden omgeleid naar een eindpunt dat gebruikers zich voor die provider. Als klanten een webbrowser gebruikt, kunt u automatisch alle niet-geverifieerde gebruikers verwijzen naar het eindpunt dat gebruikers worden aangemeld-App Service kunt hebben. Anders moet u uw klanten directe `{your App Service base URL}/.auth/login/<provider>`, waarbij `<provider>` is een van de volgende waarden: aad, facebook, google en microsoft- of twitter. Mobiele en API-scenario's worden in de secties verderop in dit artikel beschreven.

Een cookie instellen, zodat ze geverifieerde blijven kunnen als ze door uw toepassing bladeren hebben gebruikers die met uw toepassing via een webbrowser werken. Voor andere clienttypen, zoals mobile, een JSON web token (JWT), die moet worden gepresenteerd de `X-ZUMO-AUTH` header, worden uitgegeven aan de client. De client-SDK's van de Mobile Apps verwerken dit voor u. U kunt ook een token van Azure Active Directory-identiteit of het toegangstoken mogelijk rechtstreeks opgenomen in de `Authorization` header als een [bearer-token](https://tools.ietf.org/html/rfc6750).

App Service valideert een cookie of het token dat uw toepassing problemen om gebruikers te verifiëren. Als u wilt beperken wie toegang heeft tot uw toepassing, Zie de [autorisatie](#authorization) verderop in dit artikel.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobiele verificatie met een provider SDK
Nadat u alles is geconfigureerd op de back-end, kunt u mobiele clients zich aanmelden met een App Service wijzigen. Er zijn twee benaderingen hier:

* Gebruik een SDK die een opgegeven id-provider voor het vaststellen van identiteit en vervolgens toegang krijgen tot de App Service publiceert.
* Gebruik één regel code zodat de client-Mobile Apps SDK gebruikers kunt aanmelden.

> [!TIP]
> De meeste toepassingen moeten een provider SDK gebruiken om een consistente gebruikerservaring wanneer gebruikers zich aanmelden, vernieuwen-ondersteuning en om op te halen van andere voordelen van de provider geeft.
> 
> 

Wanneer u een provider SDK gebruikt, kunnen gebruikers zich kunnen aanmelden bij een werkwijze die meer nauw geïntegreerd met het besturingssysteem dat de app wordt uitgevoerd op. Dit biedt u eveneens de token van een provider en bepaalde gebruikersgegevens op de client, die veel gemakkelijker graph API's gebruiken en aanpassen van de gebruikerservaring. Tijd tot tijd op de blogs en forums ziet u dit aangeduid als de stroom' client' of 'client omgeleid stroom' omdat de code op de client zich in gebruikers en de clientcode toegang heeft tot een token van de provider.

Wanneer een provider-token wordt ontvangen, moet deze worden verzonden naar App Service voor validatie. Nadat de App Service valideert het token, maakt de App Service een nieuwe App Service-token dat wordt geretourneerd naar de client. De client-Mobile Apps SDK heeft Help-methoden voor het beheren van deze exchange- en automatisch aan alle aanvragen naar de back-end voor de toepassing koppelen het token. Ontwikkelaars kunnen ook een verwijzing naar het token provider behouden desgewenst.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobiele verificatie zonder een SDK-provider
Als u niet instellen van een provider SDK wilt, kunt u de functie Mobile Apps van Azure App Service aan te melden voor u. De client-Mobile Apps SDK wordt een webpagina weergave opent om de provider van uw keuze en meld u aan de gebruiker. Tijd tot tijd op de blogs en forums ziet u dit wel de 'server stroom' of 'server gerichte stroom' omdat de server beheert het proces dat gebruikers worden aangemeld en de client SDK nooit ontvangt het provider-token.

Code voor het starten van deze stroom is opgenomen in de zelfstudie verificatie voor elk platform. De SDK-client heeft een App Service-token aan het einde van stroom en het token is automatisch gekoppeld aan alle aanvragen naar de back-end voor de toepassing.

### <a name="service-to-service-authentication"></a>Verificatie van service-tot-service
Hoewel u gebruikers toegang tot uw toepassing geven kunt, kunt u ook een andere toepassing uw eigen API aan te roepen vertrouwen. Bijvoorbeeld, kunnen u een web-app een API-aanroep in een andere web-app hebben. In dit scenario kunt u referenties voor een serviceaccount in plaats van de referenties van gebruiker geen token ophalen. Een service-account wordt ook wel een *service-principal* in Azure Active Directory-jargon en authenticatie die gebruikmaakt van zo'n account wordt ook wel bekend als een service-naar-service-scenario.

> [!IMPORTANT]
> Omdat mobiele apps worden uitgevoerd op apparaten van de klant, mobiele toepassingen doen *niet* tellen als vertrouwde toepassingen en moet een service-principal stroom niet gebruiken. In plaats daarvan moeten ze gebruiken in de gebruikersstroom van een die eerder is beschreven.
> 
> 

App Service kunt uw toepassing beveiligen met behulp van Azure Active Directory voor scenario's met service-naar-service. De aanroepende toepassing moet alleen een Azure Active Directory service principal verificatietoken die wordt verkregen door de client-ID en geheime van Azure Active Directory-client bieden. Een voorbeeld van dit scenario die gebruikmaakt van ASP.NET-API-apps wordt uitgelegd in de zelfstudie [principal verificatie voor de Service voor API-Apps] [apia-service].

Als u wilt een App Service-verificatie gebruiken om te verwerken van een scenario voor service-naar-service, kunt u clientcertificaten of basisverificatie. Zie voor informatie over clientcertificaten in Azure, [hoe te configureren TLS wederzijdse verificatie voor Web-Apps](app-service-web-configure-tls-mutual-auth.md). Zie voor meer informatie over basisverificatie in ASP.NET [verificatie-Filters in ASP.NET Web API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Verificatie van een logische app van App Service-account in een API-app service is een speciaal geval die wordt beschreven in [uw aangepaste API gebruiken die worden gehost op App Service met Logic apps](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="authorization"></a>De werking van autorisatie in App Service
Hebt u volledige controle over de aanvragen die toegang uw toepassing tot. App Service-verificatie / autorisatie kan worden geconfigureerd met een van de volgende problemen:

* Alleen geverifieerde aanvragen toestaan om uw toepassing bereiken.
  
    Als een browser een anonieme aanvraag verzendt, omleidt App-Service naar een pagina voor de id-provider die u kiest, zodat gebruikers zich kunnen aanmelden. Als de aanvraag afkomstig van een mobiel apparaat is, het geretourneerde antwoord is een HTTP *401 niet geautoriseerd* antwoord.
  
    Met deze optie moet u niet helemaal een verificatiecode te schrijven in uw app. Als u moet de mate autorisatie, is informatie over de gebruiker beschikbaar voor uw code.
* Toestaan dat alle aanvragen voor uw toepassing bereiken maar geverifieerde aanvragen valideren en de verificatiegegevens in de HTTP-headers doorgegeven.
  
    Deze optie past omleiding autorisatiebeslissingen de toepassingscode. Biedt dit meer flexibiliteit bij het anonieme aanvragen voor de verwerking, maar u hebt om code te schrijven.
* Toestaan dat alle aanvragen voor het bereiken van uw toepassing en geen actie onderneemt op verificatiegegevens in de aanvragen.
  
    In dit geval, de verificatie / autorisatie-functie is uitgeschakeld. De taken voor verificatie en autorisatie zijn geheel afhankelijk van uw toepassingscode.

Het vorige gedrag worden beheerd door de **te ondernemen actie wanneer de aanvraag is niet geverifieerd** optie in de Azure-portal. Als u ervoor kiest ** Meld u aan met *providernaam* **, alle aanvragen moeten worden geverifieerd. **Laat de aanvraag (geen actie)** past de autorisatiebeslissing aan uw code omleiding maar nog steeds biedt verificatie-informatie. Als u dat uw code alles verwerken wilt, kunt u de verificatie uitschakelen / functie voor autorisatie.

## <a name="working-with-user-identities-in-your-application"></a>Werken met gebruikers-id's in uw toepassing
App Service geeft bepaalde gebruikersgegevens aan uw toepassing met behulp van speciale headers. Externe aanvragen verbieden deze koppen en wordt alleen aanwezig als ingesteld door App Service-verificatie / autorisatie. Er zijn enkele koppen voorbeeld:

* X-MS-CLIENT-PRINCIPAL-NAAM
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Code die is geschreven in elke taal of elk framework, krijgen de informatie die nodig is van de headers. Voor ASP.NET 4.6-apps, de **claimsprincipal is** automatisch ingesteld met de juiste waarden.

Uw toepassing ook aanvullende gegevens via een HTTP GET te vinden op de `/.auth/me` eindpunt van uw toepassing. Een geldig token dat is opgenomen in de aanvraag wordt een JSON-nettolading met gegevens over de provider die wordt gebruikt, de onderliggende provider-token en andere gebruikersinformatie geretourneerd. De server Mobile Apps SDK's bieden Help-methoden wilt werken met deze gegevens. Zie voor meer informatie [het gebruik van de Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), en [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Documentatie en aanvullende bronnen
### <a name="identity-providers"></a>Id-providers
De volgende zelfstudies laten zien hoe u App Service voor het gebruik van verschillende verificatieproviders configureren:

* [Het configureren van uw app voor het gebruik van Azure Active Directory-aanmelding][AAD]
* [Het configureren van uw app om het gebruik Facebook-aanmelding][Facebook]
* [Het configureren van uw app voor het gebruik van Google-aanmelding][Google]
* [Het configureren van uw app om de aanmelding van de Microsoft-Account gebruiken][MSA]
* [Het configureren van uw app om het gebruik Twitter-aanmelding][Twitter]

Als u gebruiken op een identiteitssysteem dan degene die wilt voorwaarde hier ook u kunt de [preview-ondersteuning voor aangepaste verificatie op de server Mobile Apps .NET SDK][custom-auth], die kan worden gebruikt in de web-apps, mobiele apps of API-apps.

### <a name="mobile-applications"></a>Mobiele toepassingen
De volgende zelfstudies laten zien hoe u verificatie toevoegen aan uw mobiele clients met behulp van de stroom server gerichte:

* [Verificatie toevoegen aan uw iOS-app][iOS]
* [Verificatie toevoegen aan uw Android-app][Android]
* [Verificatie toevoegen aan uw Windows-app][Windows]
* [Verificatie toevoegen aan uw app voor Xamarin.iOS][Xamarin.iOS]
* [Verificatie toevoegen aan uw Xamarin.Android-app][Xamarin.Android]
* [Verificatie toevoegen aan uw app Xamarin.Forms][Xamarin.Forms]
* [Verificatie toevoegen aan uw Cordova-app][Cordova]

Gebruik de volgende bronnen als u wilt gebruiken van de client omgeleid stroom voor Azure Active Directory:

* [Gebruik de Active Directory Authentication Library voor iOS][ADAL-iOS]
* [Gebruik de Active Directory Authentication Library voor Android][ADAL-Android]
* [De Active Directory Authentication Library voor Windows en Xamarin gebruiken][ADAL-dotnet]

Gebruik de volgende bronnen als u wilt gebruiken van de client omgeleid stroom voor Facebook:

* [Gebruik de Facebook-SDK voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Gebruik de volgende bronnen als u wilt gebruiken van de client omgeleid stroom voor Twitter:

* [Gebruik Twitter Fabric voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Gebruik de volgende bronnen als u wilt gebruiken van de client omgeleid stroom voor Google:

* [Gebruik de Google-In SDK voor iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

<!-- ### API applications
The following tutorials show how to protect your API apps:

* [User authentication for API Apps in Azure App Service][apia-user]
* [Service principal authentication for API Apps in Azure App Service][apia-service] -->

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
