---
title: Configureren van wachtwoorden wijzigen met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Informatie over het inschakelen van de gebruikers hun wachtwoord met behulp van aangepaste beleidsregels in Azure Active Directory B2C te wijzigen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b39c330b555be6b74760c5966e770284fa9da437
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53579189"
---
# <a name="configure-password-change-using-custom-policies-in-azure-active-directory-b2c"></a>Wachtwoord wijzigen met behulp van aangepaste beleidsregels in Azure Active Directory B2C configureren

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In Azure Active Directory (Azure AD) B2C, kunt u gebruikers die zijn aangemeld met een lokaal account hun wachtwoord wijzigen zonder te bewijzen dat de authenticiteit van e-mailverificatie inschakelen. Als de sessie is verlopen op het moment dat de gebruiker het wachtwoord wijzigen stroom, ze wordt gevraagd zich opnieuw aanmelden. In dit artikel leest u hoe het configureren van wachtwoord wijzigen in [aangepast beleid](active-directory-b2c-overview-custom.md). Het is ook mogelijk om te configureren [Self-service voor wachtwoord opnieuw instellen](active-directory-b2c-reference-sspr.md) voor gebruikersstromen.

## <a name="prerequisites"></a>Vereisten

Voer de stappen in [aan de slag met aangepaste beleidsregels in Active Directory B2C](active-directory-b2c-get-started-custom.md).

## <a name="add-the-elements"></a>De elementen toe te voegen 

1. Open uw *TrustframeworkExtensions.xml* -bestand en voeg de volgende **ClaimType** element met een id van `oldPassword` naar de [ClaimsSchema](claimsschema.md) element: 

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

2. Een [ClaimsProvider](claimsproviders.md) element bevat het technische profiel waarmee de gebruiker wordt geverifieerd. Voeg de volgende claims-providers voor de **ClaimsProviders** element:

    ```XML
    <ClaimsProviders>
      <ClaimsProvider>
        <DisplayName>Local Account SignIn</DisplayName>
        <TechnicalProfiles>
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

    Vervang `IdentityExperienceFrameworkAppId` met toepassings-ID van de IdentityExperienceFramework-toepassing die u hebt gemaakt in de vereiste zelfstudie. Vervang `ProxyIdentityExperienceFrameworkAppId` met toepassings-ID van de ProxyIdentityExperienceFramework-toepassing die u ook eerder hebt gemaakt.

3. De [UserJourney](userjourneys.md) element wordt gedefinieerd voor het pad dat de gebruiker worden uitgevoerd wanneer er interactie met uw toepassing. Voeg de **UserJourneys** element als deze niet met bestaat de **UserJourney** geïdentificeerd als `PasswordChange`:

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

4. Sla de *TrustFrameworkExtensions.xml* beleid-bestand.
5. Kopieer de *ProfileEdit.xml* bestand dat u met de beginnerspakket gedownload en geef deze de naam *ProfileEditPasswordChange.xml*.
6. Open het nieuwe bestand en update de **PolicyId** kenmerk met een unieke waarde. Deze waarde is de naam van uw beleid. Bijvoorbeeld, *B2C_1A_profile_edit_password_change*.
7. Wijzig de **ReferenceId** kenmerk in `<DefaultUserJourney>` zodat deze overeenkomen met de ID van de nieuwe gebruikersbeleving die u hebt gemaakt. Bijvoorbeeld, *PasswordChange*.
8. Sla uw wijzigingen op.

U vindt de voorbeeld-beleid [hier](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/password-change). 

## <a name="test-your-policy"></a>Het beleid testen

Bij het testen van uw toepassingen in Azure AD B2C, kan het nuttig zijn om de Azure AD B2C-token dat is geretourneerd naar `https://jwt.ms` om te kunnen controleren van de claims in het.

### <a name="upload-the-files"></a>De bestanden uploaden

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer **Identity-Ervaringsframework**.
5. Klik op de pagina aangepast beleid **uploaden beleid**.
6. Selecteer **het beleid overschrijven als deze bestaat**, en zoek en selecteer de *TrustframeworkExtensions.xml* bestand.
7. Klik op **Uploaden**.
8. Herhaal de stappen 5 tot en met 7 voor de relying party-bestand, zoals *ProfileEditPasswordChange.xml*.

### <a name="run-the-policy"></a>Het beleid uitvoeren

1. Open het beleid dat u hebt gewijzigd. Bijvoorbeeld, *B2C_1A_profile_edit_password_change*.
2. Voor **toepassing**, selecteer uw toepassing die u eerder hebt geregistreerd. Om te zien van het token wordt de **antwoord-URL** moet worden weergegeven `https://jwt.ms`.
3. Klik op **Nu uitvoeren**. Meld u met de acouunt dat u eerder hebt gemaakt. U hebt nu de mogelijkheid om het wachtwoord te wijzigen. 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe u kunt [configureren met behulp van aangepaste beleidsregels in Azure Active Directory B2C wachtwoordcomplexiteit](active-directory-b2c-reference-password-complexity-custom.md). 