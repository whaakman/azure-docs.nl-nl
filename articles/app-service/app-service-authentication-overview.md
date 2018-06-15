---
title: Verificatie en autorisatie in Azure App Service | Microsoft Docs
description: Voor conceptuele verwijzing en overzicht van de verificatie / autorisatie-functie voor Azure App Service
services: app-service
documentationcenter: ''
author: mattchenderson
manager: erikre
editor: ''
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: c180dcf5d769245f3fa2485ccee2cbc18ecf5f67
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33763489"
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Verificatie en autorisatie in Azure App Service

Azure App Service biedt ingebouwde verificatie en autorisatie ondersteunen, zodat u kunt gebruikers aanmelden en toegang gegevens door schrijven minimale of er geen code in uw web-app, API en mobiele back-end tot, en ook [Azure Functions](../azure-functions/functions-overview.md). Dit artikel wordt beschreven hoe App Service helpt bij het vereenvoudigen van verificatie en autorisatie voor uw app. 

Veilige authenticatie en autorisatie vereist grondige kennis van beveiliging, waaronder de Federatie, versleuteling, [JSON webtokens (JWT)](https://wikipedia.org/wiki/JSON_Web_Token) management, [verlenen typen](https://oauth.net/2/grant-types/), enzovoort. App Service biedt deze hulpprogramma's, zodat u meer tijd en energie besteden kunt aan de zakelijke voordelen bieden aan uw klant.

> [!NOTE]
> Bent u niet vereist voor het gebruik van App Service voor verificatie en autorisatie. Veel web frameworks worden gebundeld met beveiligingsfuncties en u indien gewenst kunt gebruiken. Als u meer flexibiliteit dan door App Service biedt, kunt u ook uw eigen's schrijven.  
>

Zie voor informatie die specifiek zijn voor mobiele apps systeemeigen [verificatie en autorisatie voor mobiele apps met Azure App Service](../app-service-mobile/app-service-mobile-auth.md).

## <a name="how-it-works"></a>Hoe werkt het?

De module voor verificatie en autorisatie wordt uitgevoerd in de sandbox van uw toepassingscode. Wanneer deze ingeschakeld, geeft elke inkomende HTTP-aanvraag via het voordat het wordt verwerkt door de toepassingscode.

![](media/app-service-authentication-overview/architecture.png)

Deze module worden verwerkt op verschillende manieren voor uw app:

- Verificatie van gebruikers met de opgegeven provider
- Valideert en vernieuwen van tokens worden opgeslagen
- Beheert de geverifieerde sessie
- Gegevens van identiteit in de aanvraagheaders injects

De module wordt afzonderlijk van uw toepassingscode wordt uitgevoerd en is geconfigureerd met app-instellingen. Er is geen SDK's, specifieke talen of wijzigingen in uw toepassingscode zijn vereist. 

### <a name="user-claims"></a>Gebruikersclaims

Voor alle taalframeworks, App Service kunt u claims van de gebruiker beschikbaar is voor uw code door het injecteren ze in de aanvraagheaders. Voor ASP.NET 4.6-apps-App Service wordt gevuld [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) met claims van de geverifieerde gebruiker, zodat u de standaard .NET-code patroon volgen kunt, waaronder de `[Authorize]` kenmerk. Op deze manier voor PHP-apps-App Service vult de `_SERVER['REMOTE_USER']` variabele.

Voor [Azure Functions](../azure-functions/functions-overview.md), `ClaimsPrincipal.Current` is niet gehydrateerd voor .NET-code, maar u kunt nog steeds de gebruikersclaims vinden in de aanvraagheaders.

Zie voor meer informatie [toegang tot gebruikersclaims](app-service-authentication-how-to.md#access-user-claims).

### <a name="token-store"></a>Tokenopslag

App Service biedt een ingebouwde tokenopslag, dit is een opslagplaats van tokens die gekoppeld aan de gebruikers van uw web-apps, API's of systeemeigen mobiele apps zijn. Als u verificatie met elke provider inschakelt, wordt dit token archief onmiddellijk beschikbaar in uw app. Als uw toepassingscode moet toegang tot gegevens van deze providers namens de gebruiker, zoals: 

- post een bericht naar de geverifieerde gebruiker Facebook tijdlijn
- zakelijke gegevens van de gebruiker lezen van de Azure Active Directory Graph API of zelfs Microsoft Graph

De id-tokens, toegangstokens en vernieuwen van tokens in de cache opgeslagen voor de geverifieerde sessie en ze zijn alleen toegankelijk is voor de gebruiker gekoppeld.  

Normaal gesproken moet u code voor het verzamelen, opslaan en vernieuw deze tokens in uw toepassing schrijven. Met de tokenopslag die u zojuist hebt [ophalen van de tokens](app-service-authentication-how-to.md#retrieve-tokens-in-app-code) wanneer u ze nodig hebt en [App Service vernieuwen ze instrueren](app-service-authentication-how-to.md#refresh-access-tokens) wanneer ze ongeldig geworden. 

Als u niet hoeft te werken met tokens in uw app, kunt u de tokenopslag uitschakelen.

### <a name="logging-and-tracing"></a>Logboekregistratie en tracering

Als u [Schakel toepassingslogboeken](web-sites-enable-diagnostic-log.md), ziet u verificatie en autorisatie traceringen rechtstreeks in de logboekbestanden. Als u een verificatiefout die u hebt niet verwacht ziet, kunt u gemakkelijk alle details vinden door te kijken in uw bestaande toepassingslogboeken. Als u inschakelt [tracering van mislukte aanvragen](web-sites-enable-diagnostic-log.md), ziet u precies welke rol de verificatie en autorisatiemodule in een mislukte aanvraag kan hebben gespeeld. Zoek in de traceerlogboeken voor verwijzingen naar een module met de naam `EasyAuthModule_32/64`. 

## <a name="identity-providers"></a>Id-providers

App Service gebruikt [federatieve identiteit](https://en.wikipedia.org/wiki/Federated_identity), waarin een derde partij id-provider de gebruikers-id's en authenticatiestroom voor u beheert. Vijf identiteitsproviders zijn standaard beschikbaar: 

| Provider | Aanmelden-eindpunt |
| - | - |
| [Azure Active Directory](../active-directory/active-directory-whatis.md) | `/.auth/login/aad` |
| [Microsoft-account](../active-directory/develop/active-directory-appmodel-v2-overview.md) | `/.auth/login/microsoft` |
| [Facebook](https://developers.facebook.com/docs/facebook-login) | `/.auth/login/facebook` |
| [Google](https://developers.google.com/+/web/api/rest/oauth) | `/.auth/login/google` |
| [Twitter](https://developer.twitter.com/docs/basics/authentication) | `/.auth/login/twitter` |

Als u verificatie en autorisatie met een van deze providers inschakelt, is het eindpunt aanmelden beschikbaar voor verificatie van gebruiker en voor de validatie van verificatietokens van de provider. U kunt uw gebruikers met een willekeurig aantal deze opties aanmelden met gemak opgeven. U kunt ook integreren met een andere id-provider of [uw eigen aangepaste identiteitsoplossing][custom-auth].

## <a name="authentication-flow"></a>Verificatiestroom

De authenticatiestroom is hetzelfde voor alle providers, maar is afhankelijk van of u zich wilt aanmelden met de provider SDK:

- Zonder provider SDK: de toepassing delegeert federatieve aanmelding bij App Service. Dit is meestal het geval is bij de browserapps, die de aanmeldingspagina van de provider voor de gebruiker kan opleveren. De servercode beheert het aanmelden, dus ook wel _server gerichte stroom_ of _server stroom_. Dit geldt voor web-apps. Dit geldt ook voor systeemeigen apps die gebruikers in de client-Mobile Apps SDK gebruiken omdat de SDK een webpagina openen om de gebruikers zich aanmelden met App Service-verificatie te ondertekenen. 
- Met SDK-provider: gebruiker handmatig aanmeldt en verzendt het vervolgens het verificatietoken in App Service voor validatie van de toepassing. Dit is meestal het geval is bij de browser minder apps, die de aanmeldingspagina van de provider kan niet worden gepresenteerd aan de gebruiker. De toepassingscode beheert het proces voor aanmelden, dus ook wel _client omgeleid stroom_ of _stroom_. Dit geldt voor de REST-API's [Azure Functions](../azure-functions/functions-overview.md), en JavaScript-browser-clients, evenals web-apps die meer flexibiliteit in het proces aanmelden. Dit geldt ook voor systeemeigen mobiele apps die gebruikers in met behulp van de provider SDK ondertekent.

> [!NOTE]
> Een andere REST-API-aanroepen vanuit een vertrouwde browserapp in App Service aanroepen in App Service of [Azure Functions](../azure-functions/functions-overview.md) kan worden geverifieerd met behulp van de stroom server omgeleid. Zie voor meer informatie [verificatie van gebruikers met Azure App Service]().
>

De onderstaande tabel bevat de stappen van de verificatiestroom.

| Stap | Zonder provider SDK | Met SDK-provider |
| - | - | - |
| 1. Aanmelding van gebruiker in | Client naar doorstuurt `/.auth/login/<provider>`. | Clientcode gebruiker zich aanmeldt rechtstreeks met de SDK van de provider en ontvangt geen verificatietoken. Zie de documentatie van de provider voor informatie. |
| 2. Post-authenticatie | Provider van client naar doorstuurt `/.auth/login/<provider>/callback`. | Clientcode boekt token van de leverancier van `/.auth/login/<provider>` voor validatie. |
| 3. Geverifieerde sessie tot stand brengen | App Service geverifieerde cookie aan antwoord toegevoegd. | App Service retourneert een eigen verificatietoken en clientcode. |
| 4. Geverifieerde inhoud leveren | Client neemt verificatiecookie in de volgende aanvragen (automatisch verwerkt door de browser). | Clientcode is verificatietoken in `X-ZUMO-AUTH` header (automatisch verwerkt door de client-Mobile Apps SDK's). |

Voor clientbrowsers App Service automatisch alle niet-geverifieerde gebruikers kan direct `/.auth/login/<provider>`. U kunt gebruikers ook aanwezig zijn met een of meer `/.auth/login/<provider>` koppelingen aan te melden bij uw app met behulp van de provider van keuze.

<a name="authorization"></a>

## <a name="authorization-behavior"></a>Autorisatie-gedrag

In de [Azure-portal](https://portal.azure.com), kunt u App Service-autorisatie configureren met een aantal gedrag.

![](media/app-service-authentication-overview/authorization-flow.png)

De volgende rubrieken beschrijven de opties.

### <a name="allow-all-requests-default"></a>Toestaan dat alle aanvragen (standaard)

Verificatie en autorisatie wordt niet beheerd door App Service (uitgeschakeld). 

Selecteer deze optie als u geen verificatie en autorisatie, of als u wilt uw eigen code voor verificatie en autorisatie schrijven.

### <a name="allow-only-authenticated-requests"></a>Alleen geverifieerde aanvragen toestaan

De optie is **Meld u aan met \<provider >**. App Service leidt alle anonieme aanvragen voor `/.auth/login/<provider>` voor de provider die u kiest. Als de anonieme aanvraag afkomstig van een systeemeigen mobiele app is, het geretourneerde antwoord is een `HTTP 401 Unauthorized`.

Met deze optie moet u niet een verificatiecode te schrijven in uw app. Hoe fijner autorisatie, zoals autorisatie, rolspecifieke, kan worden verwerkt door te inspecteren van de gebruiker claims (Zie [toegang tot gebruikersclaims](app-service-authentication-how-to.md#access-user-claims)).

### <a name="allow-all-requests-but-validate-authenticated-requests"></a>Alle aanvragen toestaan, maar geverifieerde aanvragen valideren

De optie is **toestaan anonieme aanvragen**. Deze optie Hiermee schakelt u verificatie en autorisatie in App Service, maar past omleiding autorisatiebeslissingen de toepassingscode. Voor geverifieerde aanvragen geeft App Service ook langs de verificatiegegevens in de HTTP-headers. 

Deze optie biedt meer flexibiliteit bij het verwerken van anonieme aanvragen. Bijvoorbeeld, u kunt [meerdere aanmeldingsopties aanwezig](app-service-authentication-how-to.md#configure-multiple-sign-in-options) aan uw gebruikers. U hebt echter om code te schrijven. 

## <a name="more-resources"></a>Meer bronnen

[Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end in Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)  
[Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end in Azure App Service voor Linux](containers/tutorial-auth-aad.md)  
[Verificatie en autorisatie in App Service aanpassen](app-service-authentication-how-to.md)

Providerspecifiek handleidingen:

* [Het configureren van uw app voor het gebruik van Azure Active Directory-aanmelding][AAD]
* [Het configureren van uw app om het gebruik Facebook-aanmelding][Facebook]
* [Het configureren van uw app voor het gebruik van Google-aanmelding][Google]
* [Het configureren van uw app om de aanmelding van de Microsoft-Account gebruiken][MSA]
* [Het configureren van uw app om het gebruik Twitter-aanmelding][Twitter]
* [Procedure: aangepaste verificatie voor uw toepassing gebruiken][custom-auth]

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
