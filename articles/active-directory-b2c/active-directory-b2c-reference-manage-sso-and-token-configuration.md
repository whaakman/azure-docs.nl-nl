---
title: Beheren van eenmalige aanmelding en token aanpassen met aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het beheren van eenmalige aanmelding en token aanpassen met aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/02/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 811fb8b2de59c9d324ab4acb8b0f51b4cec80aee
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441794"
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>Azure Active Directory B2C: Eenmalige aanmelding en token aanpassen met aangepaste beleidsregels beheren
Met behulp van aangepaste beleid biedt u de dezelfde controle over uw token-, sessie en eenmalige aanmelding (SSO) configuraties als u via de ingebouwde beleidsregels.  Voor meer informatie over wat elke instelling doet, Zie de documentatie [hier](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Configuratie van sessietoken levensduur en claims
Wijzig de instellingen op de levensduur van tokens, die u wilt toevoegen een `<ClaimsProviders>` -element in de relying party-bestand van het beleid dat u wilt van invloed zijn op.  De `<ClaimsProviders>` -element is een onderliggend element van de `<TrustFrameworkPolicy>`.  Binnen moet u de gegevens die van invloed is op de levensduur van tokens.  Het XML-bestand ziet eruit zoals in dit voorbeeld:

```XML
<ClaimsProviders>
   <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="JwtIssuer">
            <Metadata>
               <Item Key="token_lifetime_secs">3600</Item>
               <Item Key="id_token_lifetime_secs">3600</Item>
               <Item Key="refresh_token_lifetime_secs">1209600</Item>
               <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
               <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
               <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
            </Metadata>
         </TechnicalProfile>
      </TechnicalProfiles>
   </ClaimsProvider>
</ClaimsProviders>
```

**Toegang tot de levensduur van tokens** -de levensduur van tokens toegang kan worden gewijzigd door het wijzigen van de waarde binnen de `<Item>` = met de sleutel 'token_lifetime_secs' in een paar seconden.  De standaardwaarde aan in de ingebouwde is 3600 seconden (60 minuten).

**Levensduur van vernieuwingstoken ID** -levensduur van de ID-token kan worden gewijzigd door het wijzigen van de waarde binnen de `<Item>` = met de sleutel 'id_token_lifetime_secs' in een paar seconden.  De standaardwaarde aan in de ingebouwde is 3600 seconden (60 minuten).

**Levensduur van vernieuwingstoken** -de levensduur van vernieuwingstoken kan worden gewijzigd door het wijzigen van de waarde binnen de `<Item>` = met de sleutel 'refresh_token_lifetime_secs' in een paar seconden.  De standaardwaarde aan in de ingebouwde is 1209600 seconden (14 dagen).

**Levensduur van sliding window token vernieuwen** : als u wilt een levensduur van sliding window ingesteld op uw vernieuwingstoken, wijzigt u de waarde binnen `<Item>` = met de sleutel 'rolling_refresh_token_lifetime_secs' in een paar seconden.  De standaardwaarde aan in de ingebouwde is 7776000 (90 dagen).  Als u niet wilt om af te dwingen een levensduur van sliding window, vervangt u deze regel:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Claim van verlener (iss)** : als u wilt wijzigen van de claim van verlener (iss), wijzigt u de waarde binnen de `<Item>` = "IssuanceClaimPattern" met de sleutel.  De waarden van toepassing zijn `AuthorityAndTenantGuid` en `AuthorityWithTfp`.

**Instelling claim beleids-ID voor** -zijn de opties voor het instellen van deze waarde TFP (vertrouwensbeleid framework) en ACR (authentication context verwijzing).  
Het is raadzaam deze instelling naar TFP, om dit te doen, controleert u of de `<Item>` met de sleutel = "AuthenticationContextReferenceClaimPattern" bestaat en de waarde is `None`.
In uw `<OutputClaims>` item, dit element toe te voegen:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
Voor ACR, verwijdert u de `<Item>` = "AuthenticationContextReferenceClaimPattern" met de sleutel.

**Onderwerp (sub) claim** -deze optie is standaard ingesteld op object-id, als u wilt overschakelen met deze `Not Supported`, doet u het volgende:

Deze regel vervangen 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
Met deze regel:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Sessiegedrag en eenmalige aanmelding

Als u wilt wijzigen van uw sessiegedrag en de SSO-configuraties, die u wilt toevoegen een `<UserJourneyBehaviors>` element in de `<RelyingParty>` element.  De `<UserJourneyBehaviors>` element moet onmiddellijk volgen het `<DefaultUserJourney>`.  Binnen uw `<UserJourneyBehavors>` element moet er als volgt:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Eenmalige aanmelding (SSO)-configuratie van** : als u wilt wijzigen van de configuratie voor eenmalige aanmelding moet u de waarde van wijzigen `<SingleSignOn>`.  De waarden van toepassing zijn `Tenant`, `Application`, `Policy` en `Disabled`. 

**Web-appsessie (minuten)** : als u wilt wijzigen van de web-app sessielevensduur, moet u wijzigen van de waarde van de `<SessionExpiryInSeconds>` element.  De standaardwaarde in het ingebouwde beleid is 86400 seconden (1440 minuten).

**Web-app sessietime-out** : als u wilt wijzigen van de web-appsessie, moet u de waarde van wijzigen `<SessionExpiryType>`.  De waarden van toepassing zijn `Absolute` en `Rolling`.
