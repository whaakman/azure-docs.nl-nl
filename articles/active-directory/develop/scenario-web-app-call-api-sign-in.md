---
title: Web-app dat aanroepen van web-API's (aanmelden) - Microsoft identity-platform
description: Informatie over het bouwen van een Web-app aanroepen van web-API's (aanmelden)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 663cea72eb620217ad5fa8925d3bb00eedbf890c
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074558"
---
# <a name="web-app-that-calls-web-apis---sign-in"></a>Web-app die web-API's - aanmelden-aanroepen

U weet al aanmelden toevoegen aan uw web-app. Leert u die in [Web-app dat gebruikers zich aanmeldt: aanmelding toevoegen](scenario-web-app-sign-user-sign-in.md).

Wat is er een andere, is dat wanneer de gebruiker heeft zich aangemeld uit van deze toepassing, of vanuit elke toepassing, dat u wilt verwijderen uit de tokencache, de tokens die zijn gekoppeld aan de gebruiker.

## <a name="intercepting-the-callback-after-sign-out---single-sign-out"></a>De callback onderscheppen na het afmelden - één afmelden

Uw toepassing kan worden onderschept de nadat `logout` gebeurtenis, bijvoorbeeld om te wissen van de vermelding van de token cache die is gekoppeld aan het account dat zich afgemeld. We zien in het tweede gedeelte van deze zelfstudie (over de Web-app aanroepen van een Web-API), dat de web-app-toegangstokens voor de gebruiker in een cache worden opgeslagen. Onderschept de nadat `logout` callback kunt u uw webtoepassing om de gebruiker verwijderen uit de cache-token. Dit mechanisme wordt weergegeven in de `AddMsal()` -methode van [StartupHelper.cs L137 143](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/blob/b87a1d859ff9f9a4a98eb7b701e6a1128d802ec5/Microsoft.Identity.Web/StartupHelpers.cs#L137-L143)

De **afmeldings-Url van** dat u hebt geregistreerd voor uw toepassing u kunt voor het implementeren van eenmalige aanmelding uit. Het Microsoft identity-platform `logout` eindpunt roept de **afmeldings-URL van** geregistreerd bij uw toepassing. Deze aanroep gebeurt er als de afmelding van uw web-app, of van een andere web-app of de browser is gestart. Zie voor meer informatie, [eenmalige afmelding](https://docs.microsoft.com/azure/active-directory/develop/v2-protocols-oidc#single-sign-out) in de algemene documentatie.

```CSharp
public static IServiceCollection AddMsal(this IServiceCollection services, IEnumerable<string> initialScopes)
{
    services.AddTokenAcquisition();

    services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
    {
     ...
        // Handling the sign-out: removing the account from MSAL.NET cache
        options.Events.OnRedirectToIdentityProviderForSignOut = async context =>
        {
            // Remove the account from MSAL.NET token cache
            var _tokenAcquisition = context.HttpContext.RequestServices.GetRequiredService<ITokenAcquisition>();
            await _tokenAcquisition.RemoveAccount(context);
        };
    });
    return services;
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ophalen van een token voor de web-app](scenario-web-app-call-api-acquire-token.md)
