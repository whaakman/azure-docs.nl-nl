---
title: Verificatie en autorisatie in Azure App Service | Microsoft Docs
description: Voor conceptuele verwijzing in en overzicht van de verificatie / autorisatie-functie voor Azure App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/24/2018
ms.author: mahender,cephalin
ms.openlocfilehash: 9b4e86e719998c5f19a70734480356bcb98b8d2f
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888097"
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Verificatie en autorisatie in Azure App Service

Azure App Service biedt ingebouwde verificatie en autorisatie ondersteunen, zodat u kunt gebruikers aanmelden en toegang gegevens via het schrijven van minimale of geen code in uw web-app, API en mobiele back-end tot, en ook [Azure Functions](../azure-functions/functions-overview.md). Dit artikel wordt beschreven hoe App Service helpt bij het vereenvoudigen van verificatie en autorisatie voor uw app. 

Verificatie en autorisatie vereist grondige kennis van beveiliging, met inbegrip van Federatie, versleuteling, [JSON webtokens (JWT)](https://wikipedia.org/wiki/JSON_Web_Token) management, [toezeggingstypen](https://oauth.net/2/grant-types/), enzovoort. App Service biedt deze hulpprogramma's die u besteden kunt meer tijd en energie bedrijfswaarde voor uw klant.

> [!NOTE]
> Bent u niet vereist voor het gebruik van App Service voor verificatie en autorisatie. Veel web-frameworks worden gebundeld met beveiligingsfuncties, en kunt u deze als u dit wilt gebruiken. Als u meer flexibiliteit biedt dan de App Service biedt, kunt u ook uw eigen hulpprogramma's voor schrijven.  
>

Zie voor informatie die specifiek is voor eigen mobiele apps [verificatie en autorisatie voor mobiele apps met Azure App Service](../app-service-mobile/app-service-mobile-auth.md).

## <a name="how-it-works"></a>Hoe alles werkt

De verificatie en autorisatie-module wordt uitgevoerd in de sandbox van uw toepassingscode. Wanneer deze ingeschakeld, wordt elke binnenkomende HTTP-aanvraag doorgegeven via het voordat het wordt verwerkt door code van uw toepassing.

![](media/app-service-authentication-overview/architecture.png)

Deze module worden verwerkt op verschillende manieren voor uw app:

- Verificatie van gebruikers met de opgegeven provider
- Valideert, worden opgeslagen en wordt vernieuwd tokens
- Beheert de geverifieerde sessie
- Injects identiteitsgegevens in aanvraagheaders

De module wordt afzonderlijk van uw toepassingscode wordt uitgevoerd en is geconfigureerd met behulp van app-instellingen. Er is geen SDK's, specifieke talen of wijzigingen in uw toepassingscode zijn vereist. 

### <a name="user-claims"></a>Gebruikersclaims

Voor alle taalframeworks, App Service kan claims van de gebruiker beschikbaar is voor uw code door ze te injecteren in de aanvraagheaders. Voor apps, ASP.NET 4.6, App Service wordt gevuld [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) met claims van de geverifieerde gebruiker, zodat u het standaardpatroon voor .NET-code volgen kunt, inclusief de `[Authorize]` kenmerk. Op dezelfde manier voor PHP-apps, App Service vult de `_SERVER['REMOTE_USER']` variabele.

Voor [Azure Functions](../azure-functions/functions-overview.md), `ClaimsPrincipal.Current` is niet gemigreerd voor .NET-code, maar u kunt nog steeds de gebruikersclaims vinden in de aanvraagheaders.

Zie voor meer informatie, [toegang tot gebruikersclaims](app-service-authentication-how-to.md#access-user-claims).

### <a name="token-store"></a>Tokenarchief

App Service biedt een ingebouwde tokenopslag, dit is een opslagplaats van tokens die gekoppeld aan de gebruikers van uw web-apps, API's of systeemeigen mobiele apps zijn. Als u verificatie met een provider inschakelt, wordt dit token archief is onmiddellijk beschikbaar voor uw app. Als de code van uw toepassing nodig heeft voor toegang tot gegevens uit deze providers namens de gebruiker, zoals: 

- op de geverifieerde gebruiker Facebook-tijdlijn plaatsen
- lezen van de gebruiker bedrijfsgegevens vanuit de Azure Active Directory Graph API of zelfs Microsoft Graph

De id-tokens, toegangstokens en vernieuwen van tokens in de cache opgeslagen voor de geverifieerde sessie en ze zijn alleen toegankelijk is voor de gebruiker is gekoppeld.  

Normaal gesproken moet u code voor het verzamelen, opslaan en vernieuwen van deze tokens in uw toepassing schrijven. Met de token store die u zojuist hebt [ophalen van de tokens](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) wanneer u ze nodig hebt en [zien van App Service, zodat ze vernieuwen](app-service-authentication-how-to.md#refresh-access-tokens) wanneer ze ongeldig geworden. 

Als u niet nodig hebt om te werken met tokens in uw app, kunt u de tokenopslag uitschakelen.

### <a name="logging-and-tracing"></a>Logboekregistratie en tracering

Als u [Schakel toepassingslogboeken in](web-sites-enable-diagnostic-log.md), ziet u verificatie en autorisatie traceringen rechtstreeks in uw logboekbestanden. Als u een verificatiefout die u niet verwachtte ziet, kunt u alle details kunt gemakkelijk vinden door te kijken in uw bestaande toepassingslogboeken. Als u inschakelt [tracering van mislukte aanvragen](web-sites-enable-diagnostic-log.md), ziet u precies welke rol de verificatie en autorisatiemodule kan hebben gespeeld in een mislukte aanvragen. Zoek in de logboeken met traceringen verwijzingen naar een module met de naam `EasyAuthModule_32/64`. 

## <a name="identity-providers"></a>Id-providers

App Service gebruikt [federatieve identiteit](https://en.wikipedia.org/wiki/Federated_identity), waarin een externe id-provider de gebruikers-id's en de authenticatiestroom voor u beheert. Vijf id-providers zijn standaard beschikbaar zijn: 

| Provider | Aanmelden-eindpunt |
| - | - |
| [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) | `/.auth/login/aad` |
| [Microsoft-account](../active-directory/develop/active-directory-appmodel-v2-overview.md) | `/.auth/login/microsoftaccount` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/+/web/api/rest/oauth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/en/docs/basics/authentication) | `/.auth/login/twitter` |

Als u verificatie en autorisatie met een van deze providers inschakelt, is het eindpunt voor aanmelding is beschikbaar voor verificatie van de gebruiker en voor het valideren van verificatietokens van de provider. U kunt uw gebruikers met een willekeurig aantal deze opties aanmelden met gemak opgeven. U kunt een andere id-provider ook integreren of [uw eigen aangepaste id-oplossing][custom-auth].

## <a name="authentication-flow"></a>Verificatie-stroom

De verificatie-stroom is hetzelfde voor alle providers, maar is afhankelijk van of u wilt aanmelden met de SDK van de provider:

- Zonder de SDK-provider: de toepassing delegeert federatieve aanmelding naar App Service. Dit is meestal het geval is bij de browser-apps, die de aanmeldingspagina van de provider voor de gebruiker kan opleveren. De code de aanmeldprocedure beheert, zodat deze wordt ook wel genoemd _server gerichte stroom_ of _server flow_. Dit geldt voor web-apps. Dit geldt ook voor systeemeigen apps waarmee gebruikers bij het gebruik van de client-SDK van de Mobile Apps, omdat de SDK een weergave webpagina gebruikers zich aanmelden met App Service-verificatie wordt geopend. 
- Met de SDK-provider: de toepassing zich van de gebruiker handmatig en klikt u vervolgens verzendt het verificatietoken naar App Service voor validatie. Dit is meestal het geval is bij de browser zonder apps, die de aanmeldingspagina van de provider voor de gebruiker kan niet aanwezig. De toepassingscode beheert het aanmeldingsproces, zodat deze wordt ook wel genoemd _client omgeleid stroom_ of _clientstroom_. Dit geldt voor de REST-API's, [Azure Functions](../azure-functions/functions-overview.md), en JavaScript-browser-clients, evenals web-apps die meer flexibiliteit in de aanmeldprocedure. Dit geldt ook voor systeemeigen mobiele apps waarmee gebruikers in met behulp van de SDK van de provider.

> [!NOTE]
> Een andere REST-API-aanroepen vanuit een vertrouwde browserapp in App Service aanroepen in App Service of [Azure Functions](../azure-functions/functions-overview.md) kan worden geverifieerd met behulp van de flow-server omgeleid. Zie voor meer informatie, [aanpassen verificatie en autorisatie in App Service](app-service-authentication-how-to.md).
>

De onderstaande tabel bevat de stappen van de verificatiestroom.

| Stap | Zonder de SDK-provider | Met de SDK-provider |
| - | - | - |
| 1. Aanmelding van gebruiker in | -Client wordt omgeleid `/.auth/login/<provider>`. | Clientcode gebruiker zich aanmeldt rechtstreeks met de SDK van de provider en ontvangt een verificatietoken. Voor meer informatie, Zie de documentatie van de provider. |
| 2. Post-authenticatie | Provider-client wordt omgeleid `/.auth/login/<provider>/callback`. | Clientcode token berichten van de leverancier van `/.auth/login/<provider>` voor validatie. |
| 3. Geverifieerde sessie tot stand brengen | App Service kunt u geverifieerde cookie aan antwoord toegevoegd. | App Service retourneert een eigen verificatietoken clientcode. |
| 4. Geverifieerde inhoud leveren | Client omvat verificatiecookie in de volgende aanvragen (automatisch afgehandeld door de browser). | Clientcode is in het verificatietoken `X-ZUMO-AUTH` header (automatisch afgehandeld door Mobile Apps client-SDK's). |

Voor clientbrowsers, App Service automatisch alle niet-geverifieerde gebruikers kan sturen `/.auth/login/<provider>`. U kunt gebruikers ook presenteren met een of meer `/.auth/login/<provider>` koppelingen te melden bij uw app met behulp van hun keuze-provider.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Autorisatie-gedrag

In de [Azure-portal](https://portal.azure.com), kunt u App Service-verificatie configureren met een aantal problemen.

![](media/app-service-authentication-overview/authorization-flow.png)

De volgende rubrieken beschrijven de opties.

### <a name="allow-all-requests-default"></a>Toestaan dat alle aanvragen (standaard)

Verificatie en autorisatie wordt niet beheerd door App Service (uitgeschakeld). 

Kies deze optie als u hoeft verificatie en autorisatie, of als u wilt uw eigen verificatie en autorisatie-code te schrijven.

### <a name="allow-only-authenticated-requests"></a>Alleen geverifieerde aanvragen toestaan

De optie **Meld u aan met \<provider >**. App Service alle anonieme aanvragen om te worden omgeleid `/.auth/login/<provider>` voor de provider die u kiest. Als de anonieme aanvraag afkomstig zijn uit een systeemeigen mobiele app, het geretourneerde antwoord is een `HTTP 401 Unauthorized`.

Met deze optie moet u niet een verificatiecode op te geven in uw app in te schrijven. Hoe fijner autorisatie, zoals rolspecifieke autorisatie, kan worden verwerkt door het inspecteren van de gebruiker claims (Zie [toegang tot gebruikersclaims](app-service-authentication-how-to.md#access-user-claims)).

### <a name="allow-all-requests-but-validate-authenticated-requests"></a>Alle aanvragen toestaan, maar geverifieerde aanvragen valideren

De optie **toestaan anonieme aanvragen**. Deze optie Hiermee schakelt u verificatie en autorisatie in App Service, maar wordt uitgesteld autorisatie beslissingen te nemen aan de code van uw toepassing. Voor geverifieerde aanvragen doorgeeft App Service ook naar de verificatiegegevens in de HTTP-headers. 

Deze optie biedt meer flexibiliteit bij het verwerken van anonieme aanvragen. Bijvoorbeeld, kunt u [presenteren meerdere aanmeldingsopties](app-service-authentication-how-to.md#configure-multiple-sign-in-options) aan uw gebruikers. U hebt echter om code te schrijven. 

## <a name="more-resources"></a>Meer bronnen

[Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end in Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)  
[Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end in Azure App Service voor Linux](containers/tutorial-auth-aad.md)  
[Verificatie en autorisatie in App Service aanpassen](app-service-authentication-how-to.md)

Provider-specifieke-handleidingen:

* [Uw app voor het gebruik van Azure Active Directory-aanmelding configureren][AAD]
* [Uw app voor het gebruik van Facebook-aanmelding configureren][Facebook]
* [Uw app voor het gebruik van Google-aanmelding configureren][Google]
* [Uw app voor het gebruik van Microsoft-Account-aanmelding configureren][MSA]
* [Het configureren van uw app aanmelding met Twitter][Twitter]
* [Hoe: aangepaste verificatie voor uw toepassing gebruiken][custom-auth]

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
