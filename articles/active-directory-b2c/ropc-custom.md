---
title: De resource-eigenaar wachtwoord referentiestroom configureren in Azure Active Directory B2C | Microsoft Docs
description: Informatie over het configureren van de resource-eigenaar wachtwoord referentiestroom in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 7dffa1480be73f1dbf5e99d11fd8d33eb2ab9038
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196409"
---
# <a name="configure-the-resource-owner-password-credentials-flow-in-azure-active-directory-b2c-using-a-custom-policy"></a>De resource-eigenaar wachtwoord referentiestroom in Azure Active Directory B2C met een aangepast beleid configureren

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In Azure Active Directory (Azure AD) B2C is de stroom voor referenties (ROPC) de resource-eigenaar wachtwoord een standaard OAuth-verificatiestroom. In deze stroom uitwisselt een toepassing, ook wel bekend als de relying party, geldige referenties voor tokens. De referenties bestaan uit een gebruikers-ID en het wachtwoord. De tokens die zijn geretourneerd, zijn een ID-token, toegangstoken en een vernieuwingstoken. 

De volgende opties worden ondersteund in de stroom ROPC:

- **Native Client** -tussenkomst van de gebruiker tijdens de verificatie gebeurt wanneer de code wordt uitgevoerd op een apparaat van de gebruiker aan clientzijde.
- **Openbare clientstroom** -alleen de gebruiker referenties die worden verzameld door een toepassing, zijn verzonden in de API-aanroep. De referenties van de toepassing worden niet verzonden.
- **Toevoegen van nieuwe claims** -inhoud van de ID-tokens kunnen worden gewijzigd om toe te voegen nieuwe claims. 

De volgende stromen worden niet ondersteund:

- **Server-naar-server** -het identity protection-systeem moet een betrouwbare IP-adres van de beller (de systeemeigen client) verzameld als onderdeel van de interactie. In een server-side-API-aanroep, zijn alleen van de server-IP-adres wordt gebruikt. Als er niet te veel aanmeldingen, kan het identity protection-systeem als een aanvaller er uitzien op een herhaalde IP-adres.
- **Eén pagina toepassing** -een front-end-toepassing die is voornamelijk geschreven in JavaScript. De toepassing is vaak het geval is, met behulp van een framework zoals AngularJS, Ember.js of Durandal geschreven.
- **Vertrouwelijke clientstroom** : de client-ID van de toepassing is gevalideerd, maar het toepassingsgeheim niet is.

## <a name="prerequisites"></a>Vereisten

Voer de stappen in [aan de slag met aangepaste beleidsregels in Azure Active Directory B2C](active-directory-b2c-get-started-custom.md).

## <a name="register-an-application"></a>Een toepassing registreren

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**. 
4. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
5. Voer een naam voor de toepassing, zoals *ROPC_Auth_app*.
6. Selecteer **Nee** voor **Web App of Web-API**, en selecteer vervolgens **Ja** voor **Native client**.
7. Alle andere waarden laten zoals ze zijn, en selecteer vervolgens **maken**.
8. Selecteer de nieuwe toepassing en noteer de toepassings-ID voor later gebruik.

##  <a name="create-a-resource-owner-policy"></a>Maak een beleid voor resource-eigenaar

1. Open de *TrustFrameworkExtensions.xml* bestand.
2. Als deze niet al bestaat, voegt u toe een **ClaimsSchema** -element en de onderliggende elementen als het eerste element onder de **BuildingBlocks** element:

    ```XML
    <ClaimsSchema>
      <ClaimType Id="logonIdentifier">
        <DisplayName>User name or email address that the user can use to sign in</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="resource">
        <DisplayName>The resource parameter passes to the ROPC endpoint</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="refreshTokenIssuedOnDateTime">
        <DisplayName>An internal parameter used to determine whether the user should be permitted to authenticate again using their existing refresh token.</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="refreshTokensValidFromDateTime">
        <DisplayName>An internal parameter used to determine whether the user should be permitted to authenticate again using their existing refresh token.</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
    </ClaimsSchema>
    ```

3. Na **ClaimsSchema**, Voeg een **ClaimsTransformations** -element en de onderliggende elementen aan de **BuildingBlocks** element:

    ```XML
    <ClaimsTransformations>
      <ClaimsTransformation Id="CreateSubjectClaimFromObjectID" TransformationMethod="CreateStringClaim">
        <InputParameters>
          <InputParameter Id="value" DataType="string" Value="Not supported currently. Use oid claim." />
        </InputParameters>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="sub" TransformationClaimType="createdClaim" />
        </OutputClaims>
      </ClaimsTransformation>
    
      <ClaimsTransformation Id="AssertRefreshTokenIssuedLaterThanValidFromDate" TransformationMethod="AssertDateTimeIsGreaterThan">
        <InputClaims>
          <InputClaim ClaimTypeReferenceId="refreshTokenIssuedOnDateTime" TransformationClaimType="leftOperand" />
          <InputClaim ClaimTypeReferenceId="refreshTokensValidFromDateTime" TransformationClaimType="rightOperand" />
        </InputClaims>
        <InputParameters>
          <InputParameter Id="AssertIfEqualTo" DataType="boolean" Value="false" />
          <InputParameter Id="AssertIfRightOperandIsNotPresent" DataType="boolean" Value="true" />
        </InputParameters>
      </ClaimsTransformation>
    </ClaimsTransformations>
    ```

4. Zoek de **ClaimsProvider** -element met een **DisplayName** van `Local Account SignIn` en de volgende technische profiel toevoegen:

    ```XML
    <TechnicalProfile Id="ResourceOwnerPasswordCredentials-OAUTH2">
      <DisplayName>Local Account SignIn</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <Metadata>
        <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
        <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
        <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
        <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
        <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item>
        <Item Key="METADATA">https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration</Item>
        <Item Key="authorization_endpoint">https://login.microsoftonline.com/{tenant}/oauth2/token</Item>
        <Item Key="response_types">id_token</Item>
        <Item Key="response_mode">query</Item>
        <Item Key="scope">email openid</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="logonIdentifier" PartnerClaimType="username" Required="true" DefaultValue="{OIDC:Username}"/>
        <InputClaim ClaimTypeReferenceId="password" Required="true" DefaultValue="{OIDC:Password}" />
        <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
        <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
        <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
        <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="00000000-0000-0000-0000-000000000000" />
        <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="00000000-0000-0000-0000-000000000000" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
        <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
      </OutputClaims>
      <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromObjectID" />
      </OutputClaimsTransformations>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
    ```

    Vervang de **DefaultValue** van **client_id** en **bron-id** met toepassings-ID van de ProxyIdentityExperienceFramework-toepassing die u hebt gemaakt in de vereiste zelfstudie.

5. Voeg volgende **ClaimsProvider** elementen met hun technische profielen aan de **ClaimsProviders** element:

    ```XML
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-UserReadUsingObjectId-CheckRefreshTokenDate">
          <Metadata>
            <Item Key="Operation">Read</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" />
            <OutputClaim ClaimTypeReferenceId="refreshTokensValidFromDateTime" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="AssertRefreshTokenIssuedLaterThanValidFromDate" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromObjectID" />
          </OutputClaimsTransformations>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

    <ClaimsProvider>
      <DisplayName>Session Management</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SM-RefreshTokenReadAndSetup">
          <DisplayName>Trustframework Policy Engine Refresh Token Setup Technical Profile</DisplayName>
          <Protocol Name="None" />
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" />
            <OutputClaim ClaimTypeReferenceId="refreshTokenIssuedOnDateTime" />
          </OutputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

    <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="JwtIssuer">
          <Metadata>
            <!-- Point to the redeem refresh token user journey-->
            <Item Key="RefreshTokenUserJourneyId">ResourceOwnerPasswordCredentials-RedeemRefreshToken</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>    
    ```

6. Voeg een **UserJourneys** -element en de onderliggende elementen aan de **TrustFrameworkPolicy** element:

    ```XML
    <UserJourney Id="ResourceOwnerPasswordCredentials">
      <PreserveOriginalAssertion>false</PreserveOriginalAssertion>
        <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="ResourceOwnerFlow" TechnicalProfileReferenceId="ResourceOwnerPasswordCredentials-OAUTH2" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
    </UserJourney>
    <UserJourney Id="ResourceOwnerPasswordCredentials-RedeemRefreshToken">
      <PreserveOriginalAssertion>false</PreserveOriginalAssertion>
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="RefreshTokenSetupExchange" TechnicalProfileReferenceId="SM-RefreshTokenReadAndSetup" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="CheckRefreshTokenDateFromAadExchange" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId-CheckRefreshTokenDate" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
    </UserJourney>
    ```

7. Op de **aangepast beleid** pagina in uw Azure AD B2C-tenant, selecteer **uploaden beleid**.
8. Schakel **het beleid overschrijven als deze bestaat**, en blader vervolgens naar en selecteer de *TrustFrameworkExtensions.xml* bestand.
9. Klik op **Uploaden**.

## <a name="create-a-relying-party-file"></a>Maak een relying party-bestand

Werk vervolgens de relying party-bestand dat initieert de gebruikersbeleving die u hebt gemaakt:

1. Maak een kopie van *SignUpOrSignin.xml* -bestand in uw werkmap en wijzig de naam *ROPC_Auth.xml*.
2. Open het nieuwe bestand en wijzig de waarde van de **PolicyId** voor het kenmerk **TrustFrameworkPolicy** naar een unieke waarde. De beleids-ID is de naam van uw beleid. Bijvoorbeeld, **B2C_1A_ROPC_Auth**.
3. Wijzig de waarde van de **ReferenceId** kenmerk in **DefaultUserJourney** naar `ResourceOwnerPasswordCredentials`.
4. Wijzig de **OutputClaims** element bevat alleen de volgende claims:
    
    ```XML
    <OutputClaim ClaimTypeReferenceId="sub" />
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="displayName" DefaultValue="" />
    <OutputClaim ClaimTypeReferenceId="givenName" DefaultValue="" />
    <OutputClaim ClaimTypeReferenceId="surname" DefaultValue="" />
    ```

5. Op de **aangepast beleid** pagina in uw Azure AD B2C-tenant, selecteer **uploaden beleid**.
6. Schakel **het beleid overschrijven als deze bestaat**, en blader vervolgens naar en selecteer de *TrustFrameworkExtensions.xml* bestand.
7. Klik op **Uploaden**.

## <a name="test-the-policy"></a>Het beleid testen

Gebruik uw favoriete API-ontwikkeling-toepassing voor het genereren van een API-aanroep en bekijk de reactie voor foutopsporing van uw beleid. Bouw een aanroep zoals in dit voorbeeld met de volgende informatie als de hoofdtekst van de POST-aanvraag:

`https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/v2.0/token?p=B2C_1_ROPC_Auth`

- Vervang `your-tenant-name` met de naam van uw Azure AD B2C-tenant.
- Vervang `B2C_1A_ROPC_Auth` met de volledige naam van uw resource-eigenaar wachtwoordbeleid referenties.

| Sleutel | Value |
| --- | ----- |
| gebruikersnaam | `user-account` |
| wachtwoord | `password1` |
| grant_type | wachtwoord |
| scope | openid `application-id` offline_access |
| client_id | `application-id` |
| response_type | token id_token |

- Vervang `user-account` met de naam van een gebruikersaccount in uw tenant.
- Vervang `password1` met het wachtwoord van het gebruikersaccount.
- Vervang `application-id` met toepassings-ID van de *ROPC_Auth_app* registratie. 
- *Offline_access* is optioneel als u wilt een vernieuwingstoken ontvangen.

De werkelijke POST-aanvraag ziet eruit als in het volgende voorbeeld:

```HTTPS
POST /yourtenant.onmicrosoft.com/oauth2/v2.0/token?B2C_1_ROPC_Auth HTTP/1.1
Host: yourtenant.b2clogin.com
Content-Type: application/x-www-form-urlencoded

username=contosouser.outlook.com.ws&password=Passxword1&grant_type=password&scope=openid+bef22d56-552f-4a5b-b90a-1988a7d634ce+offline_access&client_id=bef22d56-552f-4a5b-b90a-1988a7d634ce&response_type=token+id_token
```


Een geslaagde respons met offline toegang ziet eruit als in het volgende voorbeeld:

```JSON
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlki...", 
    "token_type": "Bearer", 
    "expires_in": "3600", 
    "refresh_token": "eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3REVk1EVFBLbUJLb0FUcWQ1ZWFja1hBIiwidmVyIjoiMS4wIiwiemlwIjoiRGVmbGF0ZSIsInNlciI6Ij...", 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlki..." 
} 
```

## <a name="redeem-a-refresh-token"></a>Een vernieuwingstoken inwisselen

Maken van een POST-aanroep zoals hier weergegeven. Gebruik de informatie in de volgende tabel als de hoofdtekst van de aanvraag:

`https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/v2.0/token?p=B2C_1_ROPC_Auth`

- Vervang `your-tenant-name` met de naam van uw Azure AD B2C-tenant.
- Vervang `B2C_1A_ROPC_Auth` met de volledige naam van uw resource-eigenaar wachtwoordbeleid referenties.

| Sleutel | Value |
| --- | ----- |
| grant_type | refresh_token |
| response_type | id_token |
| client_id | `application-id` |
| Bron | `application-id` |
| refresh_token | `refresh-token` |

- Vervang `application-id` met toepassings-ID van de *ROPC_Auth_app* registratie.
- Vervang `refresh-token` met de **refresh_token** die in het vorige antwoord is verzonden. 

Een geslaagde respons ziet eruit als in het volgende voorbeeld:

```JSON
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrNHh5b2pORnVtMWtsMll0djhkbE5QNC1jNTdkTzZRR1RWQndhT...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrNHh5b2pORnVtMWtsMll0djhkbE5QNC1jNTdkTzZRR1RWQn...",
    "token_type": "Bearer",
    "not_before": 1533672990,
    "expires_in": 3600,
    "expires_on": 1533676590,
    "resource": "bef2222d56-552f-4a5b-b90a-1988a7d634c3",
    "id_token_expires_in": 3600,
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiI1MTZmYzA2NS1mZjM2LTRiOTMtYWE1YS1kNmVlZGE3Y2JhYzgiLCJzdWIiOm51bGwsIm5hbWUiOiJEYXZpZE11IiwicHJlZmVycmVkX3VzZXJuYW1lIjpudWxsLCJpZHAiOiJMb2NhbEFjY291bnQifQ",
    "refresh_token": "eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMCIsInppcCI6IkRlZmxhdGUiLCJzZXIiOiIxLjAi...",
    "refresh_token_expires_in": 1209600
}
```

## <a name="use-a-native-sdk-or-app-auth"></a>Gebruik een systeemeigen SDK of App Auth

Azure AD B2C voldoet aan de OAuth 2.0-standaarden voor openbare client-referenties voor wachtwoord van resource-eigenaar en moet compatibel zijn met de meeste client-SDK's. Voor de meest recente informatie, Zie [systeemeigen App SDK voor OAuth 2.0 en OpenID verbinding maken met het implementeren van best practices voor moderne](https://appauth.io/).

## <a name="next-steps"></a>Volgende stappen

- Een volledig voorbeeld van dit scenario in de [beginnerspakket voor Azure Active Directory B2C-beleid voor aangepaste](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/source/aadb2c-ief-ropc).
- Meer informatie over de tokens die worden gebruikt door Azure Active Directory B2C in de [Token verwijzing](active-directory-b2c-reference-tokens.md).


