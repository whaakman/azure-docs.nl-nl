---
title: Microsoft-Account (MSA) toevoegen als een id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Voorbeeld met behulp van Microsoft als id-provider met behulp van OIDC (OpenID Connect)-protocol.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 4bcc51c3efce95178dbb190eb86cb7ac8e224cd0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55187518"
---
# <a name="set-up-sign-in-with-a-microsoft-account-using-custom-policies-in-azure-active-directory-b2c"></a>Instellen van aanmelding met een Microsoft-account met behulp van aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelding voor gebruikers van een Microsoft-account inschakelen met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md) in Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Vereisten

- Voer de stappen in [aan de slag met aangepaste beleidsregels in Azure Active Directory B2C](active-directory-b2c-get-started-custom.md).
- Als u nog een Microsoft-account hebt, maakt u één voor één [ https://www.live.com/ ](https://www.live.com/).

## <a name="add-an-application"></a>Een toepassing toevoegen

Voor het gebruik van een Microsoft-account als id-provider in Azure AD B2C, moet u een toepassing van Microsoft-account toevoegen.

1. Aanmelden bij de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) met uw Microsoft-accountreferenties.
2. Selecteer in de rechterbovenhoek **een app toevoegen**.
3. Voer een **toepassingsnaam**, en klik vervolgens op **maken** 
4. Selecteer **nieuw wachtwoord genereren** en zorg ervoor dat u het wachtwoord moet worden gebruikt wanneer u de id-provider configureren. Ook kopiëren de **toepassings-Id**. 
5. Voer `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **Omleidings-URL's**. Vervang `your-tenant-name` met de naam van uw tenant.
6. Selecteer **Opslaan**.

## <a name="create-a-policy-key"></a>De beleidssleutel van een maken

U moet voor het opslaan van het wachtwoord dat u hebt gegenereerd en eerder zijn vastgelegd in uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer op de pagina overzicht **Identiteitsfunctie: PREVIEW**.
5. Selecteer **Beleidssleutels** en selecteer vervolgens **toevoegen**.
6. Voor **opties**, kiest u `Manual`.
7. Voer een **naam** voor de beleidssleutel. Bijvoorbeeld `MSASecret`. Het voorvoegsel `B2C_1A_` wordt automatisch toegevoegd aan de naam van uw sleutel.
8. In **geheim**, voer het wachtwoord die u eerder hebt genoteerd.
9. Voor **sleutelgebruik**, selecteer `Signature`.
10. Klik op **Create**.

## <a name="add-a-claims-provider"></a>Toevoegen van een claimprovider

Als u wilt dat gebruikers zich aanmelden met een Microsoft-account, moet u het account als een claimprovider waarmee Azure AD B2C via een eindpunt communiceren kunnen definiëren. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd. 

U kunt Azure AD als een claimprovider definiëren door toe te voegen de **ClaimsProvider** element in het bestand uitbreiding van uw beleid.

1. Open de *TrustFrameworkExtensions.xml*.
2. Zoek de **ClaimsProviders** element. Als deze niet bestaat, kunt u deze onder het root-element toevoegen.
3. Toevoegen van een nieuwe **ClaimsProvider** als volgt:

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
            <Item Key="client_id">Your Microsoft application client id</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
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

4.  Vervang de waarde van **client_id** met toepassings-ID die u eerder hebt genoteerd.
5.  Sla het bestand op.

### <a name="upload-the-extension-file-for-verification"></a>Upload het extensiebestand voor verificatie

U hebt nu uw beleid geconfigureerd zodat Azure AD B2C weet hoe om te communiceren met uw Microsoft-account. Upload het bestand uitbreiding van uw beleid om te bevestigen dat er geen problemen met tot nu toe.

1. Op de **aangepast beleid** pagina in uw Azure AD B2C-tenant, selecteer **uploaden beleid**.
2. Schakel **het beleid overschrijven als deze bestaat**, en blader vervolgens naar en selecteer de *TrustFrameworkExtensions.xml* bestand.
3. Klik op **Uploaden**.

## <a name="register-the-claims-provider"></a>De claimprovider registreren

Op dit moment wordt de id-provider is ingesteld, maar het is niet beschikbaar zijn in elk van de schermen registreren of aanmelden. Om het beschikbaar maken, kunt u een duplicaat van een bestaande sjabloon voor de gebruikersbeleving maken en wijzigen zodat deze ook de Microsoft-account-id-provider heeft.

1. Open de *TrustFrameworkBase.xml* -bestand van het starter-pack.
2. Zoeken en kopieer de gehele inhoud van de **UserJourney** element met `Id="SignUpOrSignIn"`.
3. Open de *TrustFrameworkExtensions.xml* en zoek de **UserJourneys** element. Als het element niet bestaat, Voeg een.
4. Plak de volledige inhoud van de **UserJourney** element dat u hebt gekopieerd als onderliggende site van de **UserJourneys** element.
5. Wijzig de naam van de ID van de gebruikersbeleving. Bijvoorbeeld `SignUpSignInMSA`.

### <a name="display-the-button"></a>De knop weergeven

De **ClaimsProviderSelection** element is vergelijkbaar met een id-provider-knop op het scherm registreren of aanmelden. Als u een **ClaimsProviderSelection** -element voor een Microsoft-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt.

1. In de *TrustFrameworkExtensions.xml* bestand, zoek de **OrchestrationStep** element met `Order="1"` in de gebruikersbeleving die u hebt gemaakt.
2. Onder **ClaimsProviderSelects**, voegt u het volgende element toe. Stel de waarde van **TargetClaimsExchangeId** naar een geschikte waarde, bijvoorbeeld `MicrosoftAccountExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met een Twitter-account voor het ontvangen van een token.

1. Zoek de **OrchestrationStep** die bevat `Order="2"` in de gebruikersbeleving.
2. Voeg de volgende **ClaimsExchange** element ervoor te zorgen dat u dezelfde waarde voor **Id** die u hebt gebruikt voor **TargetClaimsExchangeId**:

    ```xml
    <ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
    ```
    
    Werk de waarde van **TechnicalProfileReferenceId** naar de **Id** van het technische profiel dat u eerder hebt gemaakt. Bijvoorbeeld `MSA-OIDC`.

3. Sla de *TrustFrameworkExtensions.xml* -bestand en upload het opnieuw om te verifiëren.

## <a name="create-an-azure-ad-b2c-application"></a>Een Azure AD B2C-toepassing maken

Communicatie met Azure AD B2c vindt plaats via een toepassing die u in uw tenant maakt. Deze sectie vindt u optionele stappen die u uitvoeren kunt voor het maken van een testtoepassing als u dat nog niet hebt gedaan.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
5. Voer een naam voor de toepassing, bijvoorbeeld *testapp1*.
6. Voor **Web-App / Web-API**, selecteer `Yes`, en voer vervolgens `https://jwt.ms` voor de **antwoord-URL**.
7. Klik op **Create**.

## <a name="update-and-test-the-relying-party-file"></a>Bijwerken en testen van de relying party-bestand

Bijwerken van de relying party (RP)-bestand dat initieert de gebruikersbeleving die u hebt gemaakt.

1. Maak een kopie van *SignUpOrSignIn.xml* in uw werkmap en wijzig de naam. Bijvoorbeeld, wijzig de naam *SignUpSignInMSA.xml*.
2. Open het nieuwe bestand en werk de waarde van de **PolicyId** voor het kenmerk **TrustFrameworkPolicy** met een unieke waarde. Bijvoorbeeld `SignUpSignInMSA`.
3. Werk de waarde van **PublicPolicyUri** aan de URI voor het beleid. Bijvoorbeeld:`http://contoso.com/B2C_1A_signup_signin_msa`
4. Werk de waarde van de **ReferenceId** kenmerk in **DefaultUserJourney** zodat deze overeenkomen met de ID van de nieuwe gebruikersbeleving die u hebt gemaakt (SignUpSignInMSA).
5. Sla uw wijzigingen, upload het bestand en selecteer vervolgens het nieuwe beleid in de lijst.
6. Zorg ervoor dat Azure AD B2C-toepassing die u hebt gemaakt is geselecteerd de **toepassing selecteren** veld en vervolgens testen door te klikken op **nu uitvoeren**.
