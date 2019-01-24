---
title: Beheren van eenmalige aanmelding en token aanpassen met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het beheren van eenmalige aanmelding en token aanpassen met behulp van aangepaste beleidsregels in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c150c63bfdba70b256e639497c8daaf2a8cc7dbe
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54843409"
---
# <a name="manage-sso-and-token-customization-using-custom-policies-in-azure-active-directory-b2c"></a>Beheren van eenmalige aanmelding en token aanpassen met behulp van aangepaste beleidsregels in Azure Active Directory B2C

In dit artikel bevat informatie over hoe u beheren kunt, uw token-, sessie en eenmalige aanmelding (SSO) configuraties met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md) in Azure Active Directory (Azure AD) B2C.

## <a name="token-lifetimes-and-claims-configuration"></a>Configuratie van sessietoken levensduur en claims

Wijzig de instellingen op de levensduur van tokens, die u toevoegt een [ClaimsProviders](claimsproviders.md) -element in de relying party-bestand van het beleid dat u wilt van invloed zijn op.  De **ClaimsProviders** -element is een onderliggend element van de [TrustFrameworkPolicy](trustframeworkpolicy.md) element. Binnen moet u de gegevens die van invloed is op de levensduur van tokens. Het XML-bestand ziet eruit zoals in dit voorbeeld:

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

De volgende waarden worden ingesteld in het vorige voorbeeld:

- **Toegang tot de levensduur van tokens** - toegang levensduur van het token waarde is ingesteld met **token_lifetime_secs** metagegevensitem. De standaardwaarde is 3600 seconden (60 minuten).
- **Levensduur van vernieuwingstoken ID** - ID levensduur van het token waarde is ingesteld met de **id_token_lifetime_secs** metagegevensitem. De standaardwaarde is 3600 seconden (60 minuten).
- **Levensduur van vernieuwingstoken** - de levensduur van vernieuwingstoken waarde is ingesteld met de **refresh_token_lifetime_secs** metagegevensitem. De standaardwaarde is 1209600 seconden (14 dagen).
- **Levensduur van sliding window token vernieuwen** : als u wilt een levensduur van sliding window ingesteld op uw vernieuwingstoken, stel de waarde van **rolling_refresh_token_lifetime_secs** metagegevensitem. De standaardwaarde is 7776000 (90 dagen). Als u niet wilt om af te dwingen een levensduur van sliding window, vervangt u het item met `<Item Key="allow_infinite_rolling_refresh_token">True</Item>`.
- **Claim van verlener (iss)** -claim van de verlener (iss) is ingesteld met de **IssuanceClaimPattern** metagegevensitem. De waarden van toepassing zijn `AuthorityAndTenantGuid` en `AuthorityWithTfp`.
- **Instelling claim beleids-ID vertegenwoordigt** -zijn de opties voor het instellen van deze waarde `TFP` (framework vertrouwensbeleid) en `ACR` (authentication context verwijzing). `TFP` de aanbevolen waarde is. Stel **AuthenticationContextReferenceClaimPattern** met de waarde van `None`. In uw **OutputClaims** item, dit element toe te voegen:
    
    ```XML
    <OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
    ```

    Voor ACR, verwijdert u de **AuthenticationContextReferenceClaimPattern** item.

- **Onderwerp (sub) claim** -deze optie standaard ingesteld op object-id, als u deze instelling om te schakelen `Not Supported`, vervangen door deze regel: 

    ```XML
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
    ```
    
    Met deze regel:
    
    ```XML
    <OutputClaim ClaimTypeReferenceId="sub" />
    ```

## <a name="session-behavior-and-sso"></a>Sessiegedrag en eenmalige aanmelding

Als u wilt wijzigen van uw sessiegedrag en de SSO-configuraties, die u toevoegt een **UserJourneyBehaviors** element in de [RelyingParty](relyingparty.md) element.  De **UserJourneyBehaviors** element moet onmiddellijk volgen het **DefaultUserJourney**. Binnen uw **UserJourneyBehavors** element moet eruitzien als in dit voorbeeld:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

De volgende waarden worden geconfigureerd in het vorige voorbeeld:

- **Eenmalige aanmelding (SSO)** -eenmalige aanmelding is geconfigureerd met de **SingleSignOn**. De waarden van toepassing zijn `Tenant`, `Application`, `Policy`, en `Suppressed`. 
- **Web-appsessie (minuten)** - de web-app-sessie levensduur is ingesteld met de **SessionExpiryInSeconds** element. De standaardwaarde is 86400 seconden (1440 minuten).
- **Web-app sessietime-out** - de web-app-sessie time-out wordt ingesteld met de **SessionExpiryType** element. De waarden van toepassing zijn `Absolute` en `Rolling`.
