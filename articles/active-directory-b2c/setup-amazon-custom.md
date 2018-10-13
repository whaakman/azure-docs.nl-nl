---
title: Instellen van aanmelding met een Amazon-account met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Instellen van aanmelding met een Amazon-account in Azure Active Directory B2C met behulp van aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3c06a0b0af306aaf46f4aa542e26c3fcf885e754
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49168261"
---
# <a name="set-up-sign-in-with-an-amazon-account-using-custom-policies-in-azure-active-directory-b2c"></a>Instellen van aanmelding met een Amazon-account met behulp van aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelding voor gebruikers met een Amazon-account inschakelen met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md) in Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Vereisten

- Voer de stappen in [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md).
- Als u nog een Amazon-account hebt, maakt u één voor één [ http://www.amazon.com/ ](https://www.amazon.com/).

## <a name="register-the-application"></a>De toepassing registreren

Om in te schakelen aanmelding voor gebruikers van een Amazon-account, moet u een Amazon-toepassing maken.

1. Aanmelden bij de [Amazon Developer Center](https://login.amazon.com/) met de referenties van de Amazon-account.
2. Als u hebt nog niet gedaan, klikt u op **registreren**, volgt u de stappen van de registratie van ontwikkelaars en accepteren van het beleid.
3. Selecteer **nieuwe toepassing registreren**.
4. Voer een **naam**, **beschrijving**, en **Privacy-URL voor kennisgeving**, en klik vervolgens op **opslaan**. De privacyverklaring is een pagina die u beheert waarop privacyinformatie aan gebruikers biedt.
5. In de **Webinstellingen** sectie, Kopieer de waarden van **Client-ID**. Selecteer **geheim weergeven** voor het ophalen van het clientgeheim en kopieer deze vervolgens. U moet beide een Amazon-account configureert als een id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.
6. In de **Webinstellingen** sectie, selecteer **bewerken**, en voer vervolgens `https://your-tenant-name.b2clogin.com` in **JavaScript oorsprongen toegestaan** en `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` in **toegestaan URL's retourneren**. Vervang `your-tenant-name` met de naam van uw tenant. Gebruik alleen kleine letters bij het invoeren van de tenantnaam van uw, zelfs als de tenant is gedefinieerd met behulp van hoofdletters in Azure AD B2C.
7. Klik op **Opslaan**.

## <a name="create-a-policy-key"></a>De beleidssleutel van een maken

U moet voor het opslaan van het clientgeheim die u eerder hebt genoteerd in uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer op de pagina overzicht **Identiteitsfunctie: PREVIEW**.
5. Selecteer **Beleidssleutels** en selecteer vervolgens **toevoegen**.
6. Voor **opties**, kiest u `Manual`.
7. Voer een **naam** voor de beleidssleutel. Bijvoorbeeld `AmazonSecret`. Het voorvoegsel `B2C_1A_` wordt automatisch toegevoegd aan de naam van uw sleutel.
8. In **geheim**, voer het clientgeheim die u eerder hebt genoteerd.
9. Voor **sleutelgebruik**, selecteer `Signature`.
10. Klik op **Create**.

## <a name="add-a-claims-provider"></a>Toevoegen van een claimprovider

Als u wilt dat gebruikers zich aanmelden met een Amazon-account, moet u het account als een claimprovider waarmee Azure AD B2C via een eindpunt communiceren kunnen definiëren. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd. 

U kunt een Amazon-account als een claimprovider definiëren door toe te voegen aan de **ClaimsProviders** element in het bestand uitbreiding van uw beleid.


1. Open de *TrustFrameworkExtensions.xml*.
2. Zoek de **ClaimsProviders** element. Als deze niet bestaat, kunt u deze onder het root-element toevoegen.
3. Toevoegen van een nieuwe **ClaimsProvider** als volgt:  

    ```xml
    <ClaimsProvider>
      <Domain>amazon.com</Domain>
      <DisplayName>Amazon</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Amazon-OAUTH">
        <DisplayName>Amazon</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
          <Item Key="ProviderName">amazon</Item>
          <Item Key="authorization_endpoint">https://www.amazon.com/ap/oa</Item>
          <Item Key="AccessTokenEndpoint">https://api.amazon.com/auth/o2/token</Item>
          <Item Key="ClaimsEndpoint">https://api.amazon.com/user/profile</Item>
          <Item Key="scope">profile</Item>
          <Item Key="HttpBinding">POST</Item>
          <Item Key="UsePolicyInRedirectUri">0</Item>
          <Item Key="client_id">Your Amazon application client ID</Item>
        </Metadata>
        <CryptographicKeys>
          <Key Id="client_secret" StorageReferenceId="B2C_1A_AmazonSecret" />
        </CryptographicKeys>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="user_id" />
          <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
          <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
          <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="amazon.com" />
          <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
          <OutputClaimsTransformations>
          <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
          <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
          <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Stel **client_id** op de toepassings-ID van de registratie van de toepassing.
5. Sla het bestand op.

### <a name="upload-the-extension-file-for-verification"></a>Upload het extensiebestand voor verificatie

U hebt nu uw beleid geconfigureerd zodat Azure AD B2C weet hoe om te communiceren met uw Azure AD-directory. Upload het bestand uitbreiding van uw beleid om te bevestigen dat er geen problemen met tot nu toe.

1. Op de **aangepast beleid** pagina in uw Azure AD B2C-tenant, selecteer **uploaden beleid**.
2. Schakel **het beleid overschrijven als deze bestaat**, en blader vervolgens naar en selecteer de *TrustFrameworkExtensions.xml* bestand.
3. Klik op **Uploaden**.

## <a name="register-the-claims-provider"></a>De claimprovider registreren

Op dit moment wordt de id-provider is ingesteld, maar het is niet beschikbaar zijn in elk van de schermen aanmelden-up-to-date/aanmelden. Om het beschikbaar maken, kunt u een duplicaat van een bestaande sjabloon voor de gebruikersbeleving maken en wijzigen zodat het bevat ook de Amazon-id-provider.

1. Open de *TrustFrameworkBase.xml* -bestand van het starter-pack.
2. Zoeken en kopieer de gehele inhoud van de **UserJourney** element met `Id="SignUpOrSignIn"`.
3. Open de *TrustFrameworkExtensions.xml* en zoek de **UserJourneys** element. Als het element niet bestaat, Voeg een.
4. Plak de volledige inhoud van de **UserJourney** element dat u hebt gekopieerd als onderliggende site van de **UserJourneys** element.
5. Wijzig de naam van de ID van de gebruikersbeleving. Bijvoorbeeld `SignUpSignInAmazon`.

### <a name="display-the-button"></a>De knop weergeven

De **ClaimsProviderSelection** element is vergelijkbaar met een knop identity provider op een scherm aanmelden-up-to-date/aanmelden. Als u een **ClaimsProviderSelection** -element voor een Amazon-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt.

1. Zoek de **OrchestrationStep** element met `Order="1"` in de gebruikersbeleving die u hebt gemaakt.
2. Onder **ClaimsProviderSelects**, voegt u het volgende element toe. Stel de waarde van **TargetClaimsExchangeId** naar een geschikte waarde, bijvoorbeeld `AmazonExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="AmazonExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met een Amazon-account voor het ontvangen van een token.

1. Zoek de **OrchestrationStep** die bevat `Order="2"` in de gebruikersbeleving.
2. Voeg de volgende **ClaimsExchange** element ervoor te zorgen dat u dezelfde waarde voor de ID die u gebruikt voor **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="AmazonExchange" TechnicalProfileReferenceId="Amazon-OAuth" />
    ```
    
    Werk de waarde van **TechnicalProfileReferenceId** aan de ID van het technische profiel dat u eerder hebt gemaakt. Bijvoorbeeld `Amazon-OAuth`.

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

1. Maak een kopie van *SignUpOrSignIn.xml* in uw werkmap en wijzig de naam. Bijvoorbeeld, wijzig de naam *SignUpSignInAmazon.xml*.
2. Open het nieuwe bestand en werk de waarde van de **PolicyId** voor het kenmerk **TrustFrameworkPolicy** met een unieke waarde. Bijvoorbeeld `SignUpSignInAmazon`.
3. Werk de waarde van **PublicPolicyUri** aan de URI voor het beleid. Bijvoorbeeld:`http://contoso.com/B2C_1A_signup_signin_amazon`
4. Werk de waarde van de **ReferenceId** kenmerk in **DefaultUserJourney** zodat deze overeenkomen met de ID van de nieuwe gebruikersbeleving die u hebt gemaakt (SignUpSignAmazon).
5. Sla uw wijzigingen, upload het bestand en selecteer vervolgens het nieuwe beleid in de lijst.
6. Zorg ervoor dat Azure AD B2C-toepassing die u hebt gemaakt is geselecteerd de **toepassing selecteren** veld en vervolgens testen door te klikken op **nu uitvoeren**.