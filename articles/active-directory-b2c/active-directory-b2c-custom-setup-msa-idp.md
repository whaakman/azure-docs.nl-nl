---
title: Microsoft-Account (MSA) toevoegen als een id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C
description: Voorbeeld met behulp van Microsoft als id-provider met behulp van OIDC (OpenID Connect)-protocol.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 9ae944a9d587e71c4be83bd524cf3875a7b52dd1
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67654158"
---
# <a name="set-up-sign-in-with-a-microsoft-account-using-custom-policies-in-azure-active-directory-b2c"></a>Instellen van aanmelding met een Microsoft-account met behulp van aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelding voor gebruikers van een Microsoft-account inschakelen met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md) in Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Vereisten

- Voer de stappen in [aan de slag met aangepaste beleidsregels in Azure Active Directory B2C](active-directory-b2c-get-started-custom.md).
- Als u nog een Microsoft-account hebt, maakt u één voor één [ https://www.live.com/ ](https://www.live.com/).

## <a name="add-an-application"></a>Een toepassing toevoegen

Om in te schakelen aanmelding voor gebruikers met een Microsoft-account, moet u een toepassing registreren met de Azure AD-tenant. De Azure AD-tenant is niet gelijk zijn aan uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zorg ervoor dat u de map met uw Azure AD-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map met uw Azure AD-tenant te kiezen.
1. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **App-registraties**.
1. Selecteer **registratie van nieuwe**.
1. Voer een **naam** voor uw toepassing. Bijvoorbeeld, *MSAapp1*.
1. Onder **ondersteund accounttypen**, selecteer **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts (zoals Skype, Xbox, Outlook.com)** .
1. Onder **omleidings-URI (optioneel)** , selecteer **Web** en voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in het tekstvak in. Vervang `your-tenant-name` met de naam van uw Azure AD B2C-tenant.
1. Selecteer **registreren**
1. Record de **(client) toepassings-ID** weergegeven op de overzichtspagina van de toepassing. U hebt deze nodig wanneer u de claimprovider in een volgende sectie configureren.
1. Selecteer **certificaten en geheimen**
1. Klik op **nieuwe clientgeheim**
1. Voer een **beschrijving** voor het geheim, bijvoorbeeld *MSA Client Toepassingsgeheim*, en klik vervolgens op **toevoegen**.
1. Noteer het wachtwoord wordt weergegeven in de **waarde** kolom. U gebruikt deze waarde in de volgende sectie.

## <a name="create-a-policy-key"></a>De beleidssleutel van een maken

Nu u de toepassing in uw Azure AD-tenant hebt gemaakt, moet u voor het opslaan van het clientgeheim van de toepassing in uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant. Selecteer de **map- en abonnementsfilter** in het bovenste menu en kiest u de map waarin uw tenant.
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer op de pagina overzicht **Identity-Ervaringsframework**.
1. Selecteer **Beleidssleutels** en selecteer vervolgens **toevoegen**.
1. Voor **opties**, kiest u `Manual`.
1. Voer een **naam** voor de beleidssleutel. Bijvoorbeeld `MSASecret`. Het voorvoegsel `B2C_1A_` wordt automatisch toegevoegd aan de naam van uw sleutel.
1. In **geheim**, voer het clientgeheim die u in de vorige sectie hebt genoteerd.
1. Voor **sleutelgebruik**, selecteer `Signature`.
1. Klik op **Create**.

## <a name="add-a-claims-provider"></a>Toevoegen van een claimprovider

Als u wilt dat uw gebruikers zich aanmelden met een Microsoft-account, moet u het account als een claimprovider waarmee Azure AD B2C via een eindpunt communiceren kunnen definiëren. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd.

U kunt Azure AD als een claimprovider definiëren door toe te voegen de **ClaimsProvider** element in het bestand uitbreiding van uw beleid.

1. Open de *TrustFrameworkExtensions.xml* beleid-bestand.
1. Zoek de **ClaimsProviders** element. Als deze niet bestaat, kunt u deze onder het root-element toevoegen.
1. Toevoegen van een nieuwe **ClaimsProvider** als volgt:

    ```xml
    <ClaimsProvider>
      <Domain>live.com</Domain>
      <DisplayName>Microsoft Account</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="MSA-OIDC">
          <DisplayName>Microsoft Account</DisplayName>
          <Protocol Name="OpenIdConnect" />
          <Metadata>
            <Item Key="ProviderName">https://login.live.com</Item>
            <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
            <Item Key="response_types">code</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="scope">openid profile email</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your Microsoft application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="sub" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="email" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Vervang de waarde van **client_id** met de Azure AD-toepassing *(client) toepassings-ID* die u eerder hebt genoteerd.
1. Sla het bestand op.

U hebt nu uw beleid geconfigureerd zodat Azure AD B2C weet hoe om te communiceren met uw Microsoft-account-toepassing in Azure AD.

### <a name="upload-the-extension-file-for-verification"></a>Upload het extensiebestand voor verificatie

Upload het gewijzigde beleid om te bevestigen dat er geen problemen met tot nu toe voordat u verdergaat.

1. Navigeer naar uw Azure AD B2C-tenant in Azure portal en selecteer **Identity-Ervaringsframework**.
1. Op de **aangepast beleid** weergeeft, schakelt **aangepaste beleid uploaden**.
1. Schakel **het beleid overschrijven als deze bestaat**, en blader vervolgens naar en selecteer de *TrustFrameworkExtensions.xml* bestand.
1. Klik op **Uploaden**.

Als er geen fouten worden weergegeven in de portal, gaat u verder met de volgende sectie.

## <a name="register-the-claims-provider"></a>De claimprovider registreren

Op dit moment dat u de id-provider hebt ingesteld, maar deze nog niet beschikbaar in elk van de schermen registreren of aanmelden. Om het beschikbaar maken, een duplicaat van een bestaande sjabloon voor de gebruikersbeleving maken en dit zodanig aanpassen dat ook de Microsoft-account-id-provider heeft.

1. Open de *TrustFrameworkBase.xml* -bestand van het starter-pack.
1. Zoeken en kopieer de gehele inhoud van de **UserJourney** element met `Id="SignUpOrSignIn"`.
1. Open de *TrustFrameworkExtensions.xml* en zoek de **UserJourneys** element. Als het element niet bestaat, Voeg een.
1. Plak de volledige inhoud van de **UserJourney** element dat u hebt gekopieerd als onderliggende site van de **UserJourneys** element.
1. Wijzig de naam van de ID van de gebruikersbeleving. Bijvoorbeeld `SignUpSignInMSA`.

### <a name="display-the-button"></a>De knop weergeven

De **ClaimsProviderSelection** element is vergelijkbaar met een id-provider-knop op het scherm registreren of aanmelden. Als u een **ClaimsProviderSelection** -element voor een Microsoft-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt.

1. In de *TrustFrameworkExtensions.xml* bestand, zoek de **OrchestrationStep** element met `Order="1"` in de gebruikersbeleving die u hebt gemaakt.
1. Onder **ClaimsProviderSelects**, voegt u het volgende element toe. Stel de waarde van **TargetClaimsExchangeId** naar een geschikte waarde, bijvoorbeeld `MicrosoftAccountExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met een Microsoft-account voor het ontvangen van een token.

1. Zoek de **OrchestrationStep** die bevat `Order="2"` in de gebruikersbeleving.
1. Voeg de volgende **ClaimsExchange** element ervoor te zorgen dat u dezelfde waarde voor de ID die u gebruikt voor **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    ```

    Werk de waarde van **TechnicalProfileReferenceId** zodat deze overeenkomen met die van de `Id` waarde in de **TechnicalProfile** element van de claimprovider die u eerder hebt toegevoegd. Bijvoorbeeld `MSA-OIDC`.

1. Sla de *TrustFrameworkExtensions.xml* -bestand en upload het opnieuw om te verifiëren.

## <a name="create-an-azure-ad-b2c-application"></a>Een Azure AD B2C-toepassing maken

Communicatie met Azure AD B2C vindt plaats via een toepassing die u in uw Azure AD B2C-tenant maakt. Deze sectie vindt u optionele stappen die u uitvoeren kunt voor het maken van een testtoepassing als u dat nog niet hebt gedaan.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant. Selecteer de **map- en abonnementsfilter** in het bovenste menu en kiest u de map waarin uw tenant.
1. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer **Toepassingen** en vervolgens **Toevoegen**.
1. Voer een naam voor de toepassing, bijvoorbeeld *testapp1*.
1. Voor **Web-App / Web-API**, selecteer `Yes`, en voer vervolgens `https://jwt.ms` voor de **antwoord-URL**.
1. Klik op **Create**.

## <a name="update-and-test-the-relying-party-file"></a>Bijwerken en testen van de relying party-bestand

Bijwerken van de relying party (RP)-bestand dat initieert de gebruikersbeleving die u hebt gemaakt.

1. Maak een kopie van *SignUpOrSignIn.xml* in uw werkmap en wijzig de naam. Bijvoorbeeld, wijzig de naam *SignUpSignInMSA.xml*.
1. Open het nieuwe bestand en werk de waarde van de **PolicyId** voor het kenmerk **TrustFrameworkPolicy** met een unieke waarde. Bijvoorbeeld `SignUpSignInMSA`.
1. Werk de waarde van **PublicPolicyUri** aan de URI voor het beleid. Bijvoorbeeld:`http://contoso.com/B2C_1A_signup_signin_msa`
1. Werk de waarde van de **ReferenceId** kenmerk in **DefaultUserJourney** zodat deze overeenkomen met de ID van de gebruikersbeleving die u eerder (SignUpSignInMSA) hebt gemaakt.
1. Sla uw wijzigingen, upload het bestand en selecteer vervolgens het nieuwe beleid in de lijst.
1. Zorg ervoor dat Azure AD B2C-toepassing die u in de vorige sectie hebt gemaakt (of door te voeren van de vereisten, bijvoorbeeld *webapp1* of *testapp1*) is geselecteerd in de **selecteren toepassing** veld en vervolgens testen door te klikken op **nu uitvoeren**.
1. Selecteer de **Microsoft-Account** knop en meld u aan.

    Als de aanmelding geslaagd is, wordt u doorgestuurd naar `jwt.ms` weergeven met het decoderen-Token, vergelijkbaar met:

    ```json
    {
      "typ": "JWT",
      "alg": "RS256",
      "kid": "<key-ID>"
    }.{
      "exp": 1562365200,
      "nbf": 1562361600,
      "ver": "1.0",
      "iss": "https://your-b2c-tenant.b2clogin.com/10000000-0000-0000-0000-000000000000/v2.0/",
      "sub": "20000000-0000-0000-0000-000000000000",
      "aud": "30000000-0000-0000-0000-000000000000",
      "acr": "b2c_1a_signupsigninmsa",
      "nonce": "defaultNonce",
      "iat": 1562361600,
      "auth_time": 1562361600,
      "idp": "live.com",
      "name": "Azure User",
      "email": "azureuser@contoso.com",
      "tid": "6fc3b573-7b38-4c0c-b627-2e8684f6c575"
    }.[Signature]
    ```
