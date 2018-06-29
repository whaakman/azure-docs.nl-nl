---
title: Direct aanmelden met Azure Active Directory B2C instellen | Microsoft Docs
description: Informatie over het vullen van de naam of omleiden meteen naar een identiteitsprovider van sociale.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/18/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: cc3baa8fe4139acec94a722bf8c2bccb708a9470
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102324"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Direct aanmelden met Azure Active Directory B2C instellen

Bij het instellen van de aanmeldingspagina voor uw toepassing met behulp van Azure Active Directory (AD) B2C, kunt u de aanmeldingsnaam of directe aanmelding bij een specifieke sociale id-provider, zoals Facebook, LinkedIn of een Microsoft-account vooraf invullen. 

## <a name="prepopulate-the-sign-in-name"></a>De naam van de aanmeldingspagina vullen

Tijdens een reis aanmelden gebruiker mogelijk een relying party-toepassing gericht zijn op de naam van een specifieke gebruiker of domein. Wanneer de doelcomputer een gebruiker, een toepassing kunt opgeven, in de autorisatieaanvraag de `login_hint` queryparameter met de gebruikersnaam aanmelden. Azure AD B2C wordt automatisch gevuld voor de naam aanmelden, terwijl de gebruiker alleen moet het wachtwoord op te geven.

![met behulp van de hint aanmelding](./media/direct-signin/login-hint.png) 

De gebruiker kan de waarde in het tekstvak voor de aanmeldingspagina te wijzigen.

Als u van een aangepast beleid gebruikmaakt, overschrijven de `SelfAsserted-LocalAccountSignin-Email` technische profiel. In de `<InputClaims>` sectie, stelt u de standaardwaarde van de claim signInName op `{OIDC:LoginHint}`. De `{OIDC:LoginHint}` variabele bevat de waarde van de `login_hint` parameter. Azure AD B2C wordt de waarde van de claim signInName gelezen en vooraf gevuld signInName textbox.

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <InputClaims>
        <!-- Add the login hint value to the sign-in names claim type -->
        <InputClaim ClaimTypeReferenceId="signInName" DefaultValue="{OIDC:LoginHint}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="redirect-sign-in-to-a-social-provider"></a>Aanmelden met een provider van sociale omleiden

Als u het traject aanmeldingspagina voor uw toepassing om op te nemen van sociale accounts, zoals Facebook, LinkedIn of Google, geconfigureerd kunt u de `domain_hint` parameter. Deze queryparameter biedt een hint aan Azure AD B2C over de sociale id-provider die moet worden gebruikt voor aanmelding. Bijvoorbeeld, als de toepassing bevat `domain_hint=facebook.com`, aanmelden gaat u rechtstreeks naar de aanmeldingspagina Facebook.

![met behulp van geheugensteun voor het domein](./media/direct-signin/domain-hint.png) 

Als u van een aangepast beleid gebruikmaakt, kunt u configureren de domain name met behulp van de `<Domain>domain name</Domain>` XML-element van een `<ClaimsProvider>`. 

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```


