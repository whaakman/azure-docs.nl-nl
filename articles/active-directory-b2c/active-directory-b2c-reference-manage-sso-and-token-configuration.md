---
title: 'Azure Active Directory B2C: Eenmalige aanmelding en token aanpassing met aangepast beleid beheren | Microsoft Docs'
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/02/2017
ms.author: sama
ms.openlocfilehash: 8f5703d15766f221517cd89352d41685652d32d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>Azure Active Directory B2C: Eenmalige aanmelding en token aanpassing met aangepast beleid beheren
Gebruik van aangepast beleid biedt u de dezelfde controle over uw token, sessie en eenmalige aanmelding (SSO) configuraties als u via de ingebouwde beleid.  Zie de documentatie voor meer informatie over wat elke instelling doet, [hier](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Configuratie van token levensduur en claims
Wijzig de instellingen op uw token levensduur, moet u toevoegen een `<ClaimsProviders>` element in het relying party-bestand van het beleid dat u wilt worden beïnvloed.  De `<ClaimsProviders>` -element is een onderliggend element van de `<TrustFrameworkPolicy>`.  U moet binnen de gegevens die van invloed op uw token levensduur.  Het XML-bestand ziet er als volgt:

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

**Toegang tot de levensduur van token** de levensduur van tokens toegang kan worden gewijzigd door het wijzigen van de waarde binnen de `<Item>` = met de sleutel 'token_lifetime_secs' in seconden.  De standaardwaarde in ingebouwde is 3600 seconden (60 minuten).

**Levensduur van token ID** levensduur van de ID-tokens kan worden gewijzigd door het wijzigen van de waarde binnen de `<Item>` = met de sleutel 'id_token_lifetime_secs' in seconden.  De standaardwaarde in ingebouwde is 3600 seconden (60 minuten).

**Vernieuwen van de levensduur van token** de levensduur vernieuwen van tokens kan worden gewijzigd door het wijzigen van de waarde binnen de `<Item>` = met de sleutel 'refresh_token_lifetime_secs' in seconden.  De standaardwaarde in ingebouwde is 1209600 seconden (14 dagen).

**Vernieuwen van de levensduur van token verschuivende venster** als u een verschuivende venster levensduur ingesteld op uw vernieuwingstoken wilt, wijzigt u de waarde binnen `<Item>` = met de sleutel 'rolling_refresh_token_lifetime_secs' in seconden.  De standaardwaarde in ingebouwde is 7776000 (90 dagen).  Als u niet enfore een Verschuivend wilt venster levensduur, vervangt u deze regel:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Certificaatverlener (iss) claim** als u wilt wijzigen van de claim verlener (iss), wijzigt u de waarde binnen de `<Item>` met de sleutel = 'IssuanceClaimPattern'.  De waarden van toepassing zijn `AuthorityAndTenantGuid` en `AuthorityWithTfp`.

**Instelling claim die beleids-ID vertegenwoordigt** zijn de opties voor het instellen van deze waarde TFP (vertrouwensbeleid framework) en ACR (authentication context verwijzing).  
Het is raadzaam om te TFP in te stellen, om dit te doen, zorgt u ervoor de `<Item>` met de sleutel = "AuthenticationContextReferenceClaimPattern" bestaat en de waarde is `None`.
In uw `<OutputClaims>` item, het toevoegen van dit element:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
ACR, verwijdert u de `<Item>` met de sleutel = 'AuthenticationContextReferenceClaimPattern'.

**Onderwerpnaam (sub) claim** deze optie is standaard ingesteld op object-id als u wilt overschakelen met deze `Not Supported`, het volgende doen:

Deze regel vervangen 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
Met deze regel:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Sessie-gedrag en eenmalige aanmelding
Als u wilt uw sessie gedrag en de SSO-configuraties wijzigen, moet u toevoegen een `<UserJourneyBehaviors>` element in de `<RelyingParty>` element.  De `<UserJourneyBehaviors>` element moet direct volgen op de `<DefaultUserJourney>`.  Binnen uw `<UserJourneyBehavors>` element ziet er als volgt:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Eenmalige aanmelding (SSO) configuratie** om de configuratie voor eenmalige aanmelding wijzigt, moet u de waarde van wijzigen `<SingleSignOn>`.  De waarden van toepassing zijn `Tenant`, `Application`, `Policy` en `Disabled`. 

**Sessie-levensduur (minuten) van de Web-app** wijzigen de web-app sessie levensduur, moet u wijzigen waarde van de `<SessionExpiryInSeconds>` element.  De standaardwaarde in ingebouwde beleid is 86400 seconden (1440 minuten).

**Web-app sessietime-out** om te wijzigen van de sessietime-out van de web-app, moet u de waarde van wijzigen `<SessionExpiryType>`.  De waarden van toepassing zijn `Absolute` en `Rolling`.
