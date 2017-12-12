---
title: 'Azure Active Directory B2C: Wijziging selfservice voor wachtwoordherstel | Microsoft Docs'
description: Een onderwerp aan te tonen het instellen van de wijziging van de selfservice voor wachtwoordherstel voor uw consumenten in Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: mtillman
ms.assetid: 712a7128-5788-4914-8a52-24e200aa4de1
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2016
ms.author: vigunase
ms.openlocfilehash: 76e7ed328716d09dc57e25f15c411f07fda77bb9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-configure-password-change-in-custom-policies"></a>Azure Active Directory B2C: Wachtwoordwijziging configureren in het aangepaste beleid  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Met de functie wachtwoord wijzigen kunnen aangemelde gebruikers (met behulp van de lokale accounts) hun wachtwoord wijzigen zonder de authenticiteit bewijzen door e-mailverificatie zoals beschreven in de [selfservice voor wachtwoordherstel stroom.](active-directory-b2c-reference-sspr.md) Als de sessie is verlopen op het moment dat de consument opgehaald wachtwoord wijzigen stroom, gebruiker wordt gevraagd om zich opnieuw aanmelden. 

## <a name="prerequisites"></a>Vereisten

Een Azure AD B2C-tenant die is geconfigureerd voor het voltooien van een lokaal account sign-up-to-date/aanmelden, zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

## <a name="how-to-configure-password-change-in-custom-policy"></a>Het wijzigen van wachtwoorden in aangepast beleid configureren

Als u wilt configureren wachtwoordwijziging in aangepast beleid voor de volgende wijzigingen aanbrengen in uw framework extensies vertrouwensbeleid, 

## <a name="define-a-claimtype-oldpassword"></a>Een ClaimType 'OudWachtwoord' definiëren

De algemene structuur van het aangepaste beleid moet bevatten een `ClaimsSchema`en definieert u een nieuwe `ClaimType` 'OudWachtwoord, zoals hieronder, 

```XML
  <BuildingBlocks>
    <ClaimsSchema>
      <ClaimType Id="oldPassword">
        <DisplayName>Old Password</DisplayName>
        <DataType>string</DataType>
        <UserHelpText>Enter password</UserHelpText>
        <UserInputType>Password</UserInputType>
      </ClaimType>
    </ClaimsSchema>
  </BuildingBlocks>
```

Het doel van deze elementen is als volgt:

- De `ClaimsSchema` wordt gedefinieerd welke claim wordt gevalideerd.  In dit geval wordt wordt het oude wachtwoord' gevalideerd. 

## <a name="add-a-password-change-claims-provider-with-its-supporting-elements"></a>Toevoegen van een claimprovider voor wachtwoord wijzigen met de ondersteunende elementen

Claims provider wordt het wijzigen van wachtwoorden

1. De gebruiker op basis van het oude wachtwoord verifiëren
2. En als 'nieuw wachtwoord' komt overeen met 'het nieuwe wachtwoord bevestigen', deze waarde wordt opgeslagen in het gegevensarchief B2C en daarom is het wachtwoord is gewijzigd. 

![img](images/passwordchange.jpg)

De volgende claimprovider toevoegen aan uw beleid voor uitbreidingen. 

```XML
<ClaimsProviders>
    <ClaimsProvider>
      <DisplayName>Local Account SignIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="login-NonInteractive">
          <Metadata>
           <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
           <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
          </InputClaims>
        </TechnicalProfile>
        <TechnicalProfile Id="login-NonInteractive-PasswordChange">
          <DisplayName>Local Account SignIn</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
            <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
            <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
            <Item Key="ProviderName">https://sts.windows.net/</Item>
            <Item Key="METADATA">https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration</Item>
            <Item Key="authorization_endpoint">https://login.microsoftonline.com/{tenant}/oauth2/token</Item>
            <Item Key="response_types">id_token</Item>
            <Item Key="response_mode">query</Item>
            <Item Key="scope">email openid</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
            <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="username" Required="true" />
            <InputClaim ClaimTypeReferenceId="oldPassword" PartnerClaimType="password" Required="true" />
            <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
            <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
            <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
          </OutputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    <ClaimsProvider>
      <DisplayName>Local Account Password Change</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LocalAccountWritePasswordChangeUsingObjectId">
          <DisplayName>Change password (username)</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
          </CryptographicKeys>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="oldPassword" Required="true" />
            <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
            <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="login-NonInteractive-PasswordChange" />
            <ValidationTechnicalProfile ReferenceId="AAD-UserWritePasswordUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
  </ClaimsProviders>
```



### <a name="add-the-application-ids-to-your-custom-policy"></a>De toepassings-id's toevoegen aan het aangepaste beleid

De toepassings-id's toevoegen aan het extensiebestand (`TrustFrameworkExtensions.xml`):

1. Zoek het element in het extensiebestand (TrustFrameworkExtensions.xml) `<TechnicalProfile Id="login-NonInteractive">` en`<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Vervang alle exemplaren van `IdentityExperienceFrameworkAppId` met de ID van de toepassing identiteit ervaring Framework zoals is beschreven in [aan de slag](active-directory-b2c-get-started-custom.md). Hier volgt een voorbeeld:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Vervang alle exemplaren van `ProxyIdentityExperienceFrameworkAppId` met de ID van de toepassing Proxy identiteit ervaring Framework zoals is beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

4. Sla het bestand uitbreidingen.



## <a name="create-a-password-change-user-journey"></a>Maken van een gebruiker reis van wachtwoord wijzigen

```XML
 <UserJourneys>
    <UserJourney Id="PasswordChange">
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
          <ClaimsProviderSelections>
            <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
          </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="NewCredentials" TechnicalProfileReferenceId="LocalAccountWritePasswordChangeUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
      <ClientDefinition ReferenceId="DefaultWeb" />
    </UserJourney>
  </UserJourneys>
```

U klaar bent met wijzigen van het extensiebestand. Sla en dit bestand niet uploaden. Zorg ervoor dat alle validaties slaagt.



## <a name="create-a-relying-party-rp-file"></a>Een relying party (RP)-bestand maken

Werk vervolgens de relying party (RP)-bestand waarmee de gebruiker reis die u hebt gemaakt:

1. Maak een kopie van ProfileEdit.xml in uw werkmap. Wijzig de naam (bijvoorbeeld PasswordChange.xml).
2. Open het nieuwe bestand en update de `PolicyId` kenmerk voor `<TrustFrameworkPolicy>` met een unieke waarde. Dit is de naam van uw beleid (bijvoorbeeld PasswordChange).
3. Wijzig de `ReferenceId` kenmerk in `<DefaultUserJourney>` overeenkomen met de `Id` van de nieuwe gebruiker reis die u hebt gemaakt (bijvoorbeeld PasswordChange).
4. De wijzigingen opslaan en vervolgens te uploaden.
5. Test het aangepaste beleid dat u hebt geüpload, in de Azure-portal, gaat u naar de blade beleid op en klik vervolgens op **nu uitvoeren**.




## <a name="link-to-password-change-sample-policy"></a>Koppeling naar wachtwoord voorbeeld beleid wijzigen

U vindt het beleid voorbeeld [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change). 










