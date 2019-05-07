---
title: Desktop-app dat aanroepen van web-API's (verplaatsen naar productie) - Microsoft identity-platform
description: Informatie over het bouwen van een bureaublad-app dat aanroepen van web-API's (verplaatsen naar productie)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ca66a41f26c54bf04273682d14889a36b688c70
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075128"
---
# <a name="desktop-app-that-calls-web-apis---move-to-production"></a>Desktop-app die aanroepen van web-API's - verplaatsen naar productie

Dit artikel vindt u details voor uw toepassing verder te verbeteren en verplaatsen naar productie.

## <a name="handling-errors-in-desktop-applications"></a>Afhandeling van fouten in bureaubladtoepassingen

In de verschillende stromen, hebt u geleerd hoe u kunt de fouten voor de stromen op de achtergrond verwerken (zoals weergegeven in de codefragmenten). Hebt u gezien dat er zijn gevallen waarin interactie nodig (incrementele toestemming en voorwaardelijke toegang).

## <a name="how-to-have--the-user-consent-upfront-for-several-resources"></a>Hoe u met de toestemming van de gebruiker vooraf voor verschillende resources

> [!NOTE]
> Toestemming ophalen voor verschillende bronnen werken voor Microsoft identity-platform, maar niet voor Azure Active Directory (Azure AD) B2C. Azure AD B2C ondersteunt alleen beheerderstoestemming, niet de toestemming van de gebruiker.

Het eindpunt van de Microsoft identity-platform (v2.0) kunt niet u een token verkrijgen voor meerdere resources tegelijk. Daarom de `scopes` parameter mag alleen bereiken voor één resource. U kunt ervoor zorgen dat de gebruiker vooraf toestemming heeft voor verschillende resources met behulp van de `extraScopesToConsent` parameter.

Bijvoorbeeld, hebt u twee resources, die twee hebben het bereik van elke:

- `https://mytenant.onmicrosoft.com/customerapi` -met 2 scopes `customer.read` en `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi` -met 2 scopes `vendor.read` en `vendor.write`

Moet u de `.WithAdditionalPromptToConsent` modifier waarvoor de `extraScopesToConsent` parameter.

Bijvoorbeeld:

```CSharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

Deze aanroep krijgt u een toegangstoken voor de eerste web-API.

Wanneer u nodig hebt voor het aanroepen van de tweede web-API, kunt u bellen:

```CSharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsenting-each-time-the-app-is-run"></a>Persoonlijke Microsoft-account vereist reconsenting telkens wanneer die de app wordt uitgevoerd

Voor gebruikers van Microsoft persoonlijke accounts is reprompting om toestemming op elke clientaanroep native (desktop/mobiele app) om te autoriseren het beoogde gedrag. Systeemeigen client-id is inherent onveilig zijn (in tegenstelling tot vertrouwelijke client-toepassing die een geheim met de Microsoft Identity-platform om hun identiteit te bewijzen exchange). Het Microsoft identity-platform hebt gekozen om deze onzekerheid voor consumentenservices door het vragen van de gebruiker om toestemming te geven, telkens wanneer die de toepassing is geautoriseerd.

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
