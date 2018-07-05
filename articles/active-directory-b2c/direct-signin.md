---
title: Instellen van direct aanmelden met behulp van Azure Active Directory B2C | Microsoft Docs
description: Informatie over het vullen van de naam van de aanmelding of omleiden rechtstreeks naar een sociale id-provider.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/18/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 62ded87067bf597a2f40ec8e8405142297d4fb78
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440533"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Direct aanmelden met behulp van Azure Active Directory B2C instellen

Bij het instellen van de aanmelding voor uw toepassing met behulp van Azure Active Directory (AD) B2C, kunt u vooraf invullen met de naam of direct aanmelden met een specifieke sociale id-provider, zoals Facebook of LinkedIn, een Microsoft-account. 

## <a name="prepopulate-the-sign-in-name"></a>De naam van de aanmelding vullen

Tijdens de reis van een gebruiker aanmelden en kan een relying party-toepassing gericht zijn op de naam van een specifieke gebruiker of domein. Wanneer de doelcomputer een gebruiker, een toepassing kunt opgeven, in de autorisatieaanvraag, de `login_hint` queryparameter met de gebruikersnaam aanmelden. Azure AD B2C wordt automatisch gevuld voor de naam aanmelden, terwijl de gebruiker alleen moet het wachtwoord op te geven.

![met behulp van aanmelding hint](./media/direct-signin/login-hint.png) 

De gebruiker kan de waarde in het tekstvak aanmelding te wijzigen.

Als u van een aangepast beleid gebruikmaakt, overschrijven de `SelfAsserted-LocalAccountSignin-Email` technisch profiel. In de `<InputClaims>` sectie, stelt u de standaardwaarde van de claim signInName `{OIDC:LoginHint}`. De `{OIDC:LoginHint}` variabele bevat de waarde van de `login_hint` parameter. Azure AD B2C wordt de waarde van de claim signInName gelezen en het tekstvak signInName vooraf gevuld.

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

## <a name="redirect-sign-in-to-a-social-provider"></a>Aanmelden omleiden naar een provider voor sociale netwerken

Als u hebt geconfigureerd dat de reis-in voor uw toepassing om op te nemen van sociale accounts, zoals Facebook of LinkedIn, Google, kunt u opgeven de `domain_hint` parameter. Deze queryparameter bevat een hint aan Azure AD B2C over de sociale id-provider die moet worden gebruikt voor aanmelding bij. Bijvoorbeeld, als de toepassing bevat `domain_hint=facebook.com`, aanmelden gaat u rechtstreeks naar de aanmeldingspagina van Facebook.

![met behulp van domeinhint](./media/direct-signin/domain-hint.png) 

Als u een aangepast beleid gebruikt, kunt u configureren de domain name met behulp van de `<Domain>domain name</Domain>` XML-element van een `<ClaimsProvider>`. 

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```


