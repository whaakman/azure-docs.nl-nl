---
title: Self-service voor wachtwoord wijzigen in Azure Active Directory B2C | Microsoft Docs
description: Een onderwerp over het instellen van de opties wachtwoorden wijzigen voor uw consumenten in Azure Active Directory B2C aan te tonen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fdbf2d9bebb26a36e3f83e1149c4f97921aeaa1d
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407024"
---
# <a name="azure-active-directory-b2c-configure-password-change-in-custom-policies"></a>Azure Active Directory B2C: Wachtwoord wijzigen in het aangepaste beleid configureren  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Met de functie wachtwoord wijzigen is aangemeld consumenten (met behulp van de lokale accounts) kunnen hun wachtwoord te wijzigen zonder dat om te bewijzen dat de echtheid van e-mailverificatie, zoals beschreven in de [Self-service voor wachtwoord opnieuw instellen van flow.](active-directory-b2c-reference-sspr.md) Als de sessie is verlopen op het moment dat de consument opgehaald wachtwoord stroom wijzigen, gebruiker wordt gevraagd om opnieuw aan te melden. 

## <a name="prerequisites"></a>Vereisten

Een Azure AD B2C-tenant die is geconfigureerd voor het voltooien van een lokaal account aanmelden-up-to-date/aanmelden, zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

## <a name="how-to-configure-password-change-in-custom-policy"></a>Wachtwoord wijzigen in het aangepaste beleid configureren

Voor het configureren van wachtwoord wijzigen in het aangepaste beleid de volgende wijzigingen aanbrengen in uw beleid vertrouwensrelatie framework-extensies 

## <a name="define-a-claimtype-oldpassword"></a>Een ClaimType 'oldPassword' definiëren

De algemene structuur van het aangepaste beleid moet bevatten een `ClaimsSchema`en definieert u een nieuwe `ClaimType` 'oldPassword' zoals hieronder, 

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

- De `ClaimsSchema` wordt gedefinieerd welke claim wordt gevalideerd.  In dit geval wordt het oude wachtwoord' worden gevalideerd. 

## <a name="add-a-password-change-claims-provider-with-its-supporting-elements"></a>Toevoegen van een claimprovider voor wachtwoord wijzigen met de ondersteunende elementen

Claims provider wordt het wijzigen van wachtwoord

1. Verifiëren van de gebruiker op basis van het oude wachtwoord
2. En als 'nieuw wachtwoord' overeenkomt met 'nieuw wachtwoord bevestigen', deze waarde wordt opgeslagen in de B2C-gegevensopslag en kan daarom is het wachtwoord is gewijzigd. 

![img](images/passwordchange.jpg)

De volgende claimprovider toevoegen aan uw beleid extensies. 

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



### <a name="add-the-application-ids-to-your-custom-policy"></a>De toepassings-id's toevoegen aan uw aangepaste beleid

De toepassings-id's toevoegen aan het extensiebestand (`TrustFrameworkExtensions.xml`):

1. Zoek het element in het extensiebestand (TrustFrameworkExtensions.xml) `<TechnicalProfile Id="login-NonInteractive">` en `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Vervang alle exemplaren van `IdentityExperienceFrameworkAppId` met toepassings-ID van de application Identity-Ervaringsframework zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md). Hier volgt een voorbeeld:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Vervang alle exemplaren van `ProxyIdentityExperienceFrameworkAppId` met toepassings-ID van de Proxy Identiteitservaring-Framework-toepassing zoals beschreven in [aan de slag](active-directory-b2c-get-started-custom.md).

4. Sla het extensiebestand.



## <a name="create-a-password-change-user-journey"></a>Maken van een gebruikersbeleving voor wachtwoord wijzigen

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

U klaar bent met de extensiebestand wijzigt. Opslaan en upload dit bestand. Zorg ervoor dat alle validaties slagen.



## <a name="create-a-relying-party-rp-file"></a>Maak een relying party (RP)-bestand

Werk vervolgens de relying party (RP)-bestand dat initieert de gebruikersbeleving die u hebt gemaakt:

1. Maak een kopie van ProfileEdit.xml in uw werkmap. Wijzig de naam van het (bijvoorbeeld PasswordChange.xml).
2. Open het nieuwe bestand en update de `PolicyId` voor het kenmerk `<TrustFrameworkPolicy>` met een unieke waarde. Dit is de naam van uw beleid (bijvoorbeeld PasswordChange).
3. Wijzig de `ReferenceId` kenmerk in `<DefaultUserJourney>` zodat deze overeenkomen met de `Id` van de nieuwe gebruikersbeleving die u hebt gemaakt (bijvoorbeeld PasswordChange).
4. Sla uw wijzigingen op en upload het bestand.
5. Als u wilt testen het aangepaste beleid dat u hebt geüpload, in de Azure-portal, gaat u naar de beleidsblade op en klik vervolgens op **nu uitvoeren**.




## <a name="link-to-password-change-sample-policy"></a>Koppeling naar wachtwoord voorbeeld beleid wijzigen

U vindt de voorbeeld-beleid [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change). 










