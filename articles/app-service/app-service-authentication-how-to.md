---
title: Verificatie en autorisatie in Azure App Service aanpassen | Microsoft Docs
description: Laat zien hoe u verificatie en autorisatie in App Service aanpassen en ophalen van gebruikersclaims en andere tokens.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2018
ms.author: cephalin
ms.openlocfilehash: c41cb3ef2939fe7271b1f8738fcf0cb95c4b1111
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="customize-authentication-and-authorization-in-azure-app-service"></a>Verificatie en autorisatie in Azure App Service aanpassen

Dit artikel laat zien hoe u kunt aanpassen [verificatie en autorisatie in App Service](app-service-authentication-overview.md), en voor het beheren van de identiteit van uw toepassing. 

Om snel aan de slag, ziet u een van de volgende zelfstudies:

* [Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end in Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)
* [Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end in Azure App Service voor Linux](containers/tutorial-auth-aad.md)
* [Uw app configureren voor aanmelding met Azure Active Directory](app-service-mobile-how-to-configure-active-directory-authentication.md)
* [Uw app configureren voor aanmelding met Facebook](app-service-mobile-how-to-configure-facebook-authentication.md)
* [Uw app configureren voor aanmelding met Google](app-service-mobile-how-to-configure-google-authentication.md)
* [Uw app configureren voor aanmelding met een Microsoft Account](app-service-mobile-how-to-configure-microsoft-authentication.md)
* [Uw app configureren voor aanmelding met Twitter](app-service-mobile-how-to-configure-twitter-authentication.md)

## <a name="configure-multiple-sign-in-options"></a>Aanmelden-opties configureren

De configuratie van de portal mogelijkheden geen directe kunnen meerdere aanmeldingsopties presenteren aan uw gebruikers (zoals Facebook en Twitter). Het is echter niet moeilijk om de functionaliteit toevoegen aan uw web-app. De stappen uit worden als volgt beschreven:

Eerste in de **verificatie / autorisatie** pagina in de Azure portal, het configureren van elk van de id-provider die u wilt inschakelen.

In **te ondernemen actie wanneer de aanvraag is niet geverifieerd**, selecteer **toestaan anonieme aanvragen (geen actie)**.

In de aanmeldingspagina, of de navigatiebalk, of een andere locatie van uw web-app, kunt u een koppeling aanmelden toevoegen aan elk van de providers die u hebt ingeschakeld (`/.auth/login/<provider>`). Bijvoorbeeld:

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Wanneer de gebruiker op een van de koppelingen klikt, de respectieve aanmeldingspagina wordt geopend Meld u aan de gebruiker.

## <a name="access-user-claims"></a>Claims van de gebruiker toegang

App Service doorgegeven gebruikersclaims voor uw toepassing met behulp van speciale headers. Externe aanvragen zijn niet toegestaan deze koppen instellen zodat deze aanwezig zijn alleen als ingesteld door App Service. Er zijn enkele koppen voorbeeld:

* X-MS-CLIENT-PRINCIPAL-NAAM
* X-MS-CLIENT-PRINCIPAL-ID

Code die is geschreven in elke taal of elk framework, krijgen de informatie die nodig is van de headers. Voor ASP.NET 4.6-apps, de **claimsprincipal is** automatisch ingesteld met de juiste waarden.

Uw toepassing kunt ook meer informatie over de geverifieerde gebruiker ophalen door het aanroepen van `/.auth/me`. De server Mobile Apps SDK's bieden Help-methoden wilt werken met deze gegevens. Zie voor meer informatie [het gebruik van de Azure Mobile Apps Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), en [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="retrieve-tokens-in-app-code"></a>Ophalen van tokens in app-code

Vanuit uw servercode de providerspecifieke tokens in de aanvraagheader geïnjecteerd zodat u deze eenvoudig kunt openen. De volgende tabel ziet u mogelijk token headernamen:

| | |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook-Token | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Microsoft-account | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

Verzenden vanuit uw clientcode (zoals een mobiele app of in de browser JavaScript), een HTTP `GET` aanvraag voor het `/.auth/me`. De geretourneerde JSON heeft de provider-specifieke-tokens.

> [!NOTE]
> Toegangstokens zijn voor het openen van bronnen van de provider, zodat ze alleen aanwezig als u uw provider met een clientgeheim configureren. Zie voor het vernieuwen van tokens ophalen [vernieuwen toegangstokens](#refresh-access-tokens).

## <a name="refresh-access-tokens"></a>Toegangstokens vernieuwen

Wanneer uw provider toegangstoken is verlopen, moet u de gebruiker te verifiëren. U kunt het verlopen van het token voorkomen door het maken van een `GET` aanroep van de `/.auth/refresh` eindpunt van uw toepassing. Als deze wordt aangeroepen, wordt de toegangstokens te geven in het token archief voor de geverifieerde gebruiker automatisch vernieuwd door App Service. De volgende aanvragen voor tokens die door uw app-code krijgen de vernieuwd tokens. Echter, de tokenopslag moet bevatten voor de token vernieuwing werkt [vernieuwingstokens](https://auth0.com/learn/refresh-tokens/) voor uw provider. De manier om te vernieuwen van tokens krijgen door elke provider worden beschreven, maar de volgende lijst bevat een korte samenvatting:

- **Google**: Append een `access_type=offline` querytekenreeksparameter naar uw `/.auth/login/google` API-aanroep. Als de Mobile Apps SDK wordt gebruikt, kunt u de parameter toevoegen aan een van de `LogicAsync` overloads (Zie [Google vernieuwen Tokens](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: geen biedt vernieuwen van tokens. Lange levensduur hebben tokens verloopt over 60 dagen (Zie [Facebook is verlopen en de extensie toegangstokens](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: toegangstokens verloopt niet (Zie [Twitter OAuth Veelgestelde vragen over het](https://developer.twitter.com/docs/basics/authentication/guides/oauth-faq)).
- **Microsoft-Account**: wanneer [verificatie-instellingen voor Microsoft-Account configureren](app-service-mobile-how-to-configure-microsoft-authentication.md), selecteer de `wl.offline_access` bereik.
- **Azure Active Directory**: In [ https://resources.azure.com ](https://resources.azure.com), moet u de volgende stappen uitvoeren:
    1. Selecteer boven aan de pagina **lezen/schrijven**.
    1. Navigeer in de browser links naar **abonnementen** > **_\<abonnement\_naam_**   >  **resourceGroups** > _**\<resource\_groep\_name >**_   >  **providers** > **Microsoft.Web** > **sites** > _**\<app \_name >**_ > **config** > **authsettings**. 
    1. Klik op **Bewerken**.
    1. De volgende eigenschap wijzigen. Vervang  _\<app\_id >_ met de Azure Active Directory-toepassings-ID van de service die u wilt openen.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    1. Klik op **plaatsen**. 

Zodra uw provider is geconfigureerd, kunt u zien of vernieuwen van tokens in de tokenopslag door aan te roepen zijn `/.auth/me`. 

Als u wilt vernieuwen uw toegangstoken op elk gewenst moment, roept u `/.auth/refresh` in een andere taal. Het volgende fragment gebruikt jQuery voor het vernieuwen van de toegangstokens te geven van een JavaScript-client.

```JavaScript
function refreshTokens() {
  var refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Als een gebruiker de machtigingen te krijgen tot uw app, de aanroep van trekt `/.auth/me` kan mislukken met een `403 Forbidden` antwoord. Om op te sporen fouten, Controleer uw toepassingslogboeken voor meer informatie.

## <a name="extend-session-expiration-grace-period"></a>Sessie verlopen respijtperiode uitbreiden

Nadat een geverifieerde sessie is verlopen, moet u er een respijtperiode 72 uur is standaard. In deze respijtperiode kunt u mag de sessiecookie of sessietoken met App Service zonder herverifiëren van de gebruiker vernieuwen. U kunt alleen aanroepen `/.auth/refresh` wanneer uw sessiecookie of het sessietoken is ongeldig en u niet wilt bijhouden van verlopen van het token. Nadat de respijtperiode 72 uur verstrijkt is, de gebruiker moet zich aanmelden opnieuw ophalen van een geldig sessiecookie of sessietoken.

Als 72 uur niet voldoende tijd is voor u is, kunt u dit venster verlopen uitbreiden. Uitbreiden van de vervaldatum gedurende een langere periode kan aanzienlijke beveiligingsgebied (zoals wanneer een verificatietoken is vermist of gestolen) hebben. U moet dus 72 uur laat de standaardwaarde of de uitbreiding periode instellen op de kleinste waarde.

Om het venster van de vervaldatum standaard uitbreidt, kunt u de volgende opdracht uitvoeren de [Cloud Shell](../cloud-shell/overview.md).

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> De respijtperiode is alleen van toepassing op de App Service geverifieerde sessie, niet de tokens van de id-providers. Er is geen respijtperiode voor de tokens verlopen provider. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Limiet voor het domein van e-mailaccounts

Zowel Microsoft-Account en Azure Active Directory kunt u zich aanmelden via meerdere domeinen. Microsoft-Account kan bijvoorbeeld _outlook.com_, _live.com_, en _hotmail.com_ accounts. Azure Active Directory kunt een willekeurig aantal aangepaste domeinen voor de accounts aanmelden. Dit is mogelijk ongewenste voor een interne app, dat u niet dat iedereen met wilt een _outlook.com_ account voor toegang. Volg deze stappen om te beperken van de domeinnaam van de e-mailaccounts.

In [ https://resources.azure.com ](https://resources.azure.com), gaat u naar **abonnementen** > **_\<abonnement\_naam_**   >  **resourceGroups** > _**\<resource\_groep\_name >**_   >  **providers** > **Microsoft.Web** > **sites**  >    _**\<app\_name >**_ > **config** > **authsettings**. 

Klik op **bewerken**, de volgende eigenschap wijzigen en klik vervolgens op **plaatsen**. Zorg ervoor dat u  _\<domein\_name >_ met het domein dat u wilt.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Verifiëren en autoriseren van gebruikers end-to-end (Windows)](app-service-web-tutorial-auth-aad.md)
> [zelfstudie: verifiëren en autoriseren van gebruikers end-to-end (Linux)](containers/tutorial-auth-aad.md)