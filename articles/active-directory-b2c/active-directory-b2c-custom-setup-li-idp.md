---
title: LinkedIn toevoegen als een OAuth2-id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Een procedure artikel over het instellen van een LinkedIn-toepassing met behulp van de OAuth2-protocol en aangepaste beleidsregels.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/23/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 334f696d79cf801facf7c5301b2240b69f7134f7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444375"
---
# <a name="azure-active-directory-b2c-add-linkedin-as-an-identity-provider-by-using-custom-policies"></a>Azure Active Directory B2C: LinkedIn toevoegen als een id-provider met behulp van aangepaste beleidsregels
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelding voor gebruikers van een LinkedIn-account inschakelen met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

## <a name="step-1-create-a-linkedin-account-application"></a>Stap 1: Een toepassing van LinkedIn-account maken
LinkedIn gebruiken als een id-provider in Azure Active Directory B2C (Azure AD B2C), die u moet een LinkedIn-toepassing maken en geven met de juiste parameters. U kunt een LinkedIn-toepassing registreren door te gaan naar de [pagina voor het registreren van LinkedIn](https://www.linkedin.com/start/join).

1. Ga naar de [LinkedIn Toepassingsbeheer](https://www.linkedin.com/secure/developer?newapp=) website, meld u aan met de referenties van uw LinkedIn-account en selecteer vervolgens **toepassing maken**.

    ![LinkedIn-account - toepassing maken](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app1.png)

2. Op de **Maak een nieuwe toepassing** pagina, de volgende handelingen uit:

    a. Typ uw **bedrijfsnaam**, een beschrijvende **naam** voor het bedrijf, en een **beschrijving** van de nieuwe app.

    b. Upload uw **toepassing Logo**.

    c. Selecteer een **gebruik**.

    d. In de **Website-URL** vak, plak **https://login.microsoftonline.com**.

    e. Typ uw **zakelijke e-mailadres** adres en **telefoon (werk)** getal.

    f. Aan de onderkant van de pagina, lees en accepteer de gebruiksvoorwaarden en selecteer vervolgens **indienen**.

    ![LinkedIn-account - eigenschappen voor de toepassing configureren](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app2.png)

3. Selecteer **verificatie**, en noteer vervolgens de **Client-ID** en **Clientgeheim** waarden.

4. In de **Omleidings-URL's toegestaan** vak, plak **https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/oauth2/authresp**. Vervang {*tenant*} met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com). Zorg ervoor dat u van het HTTPS-schema gebruikmaakt. 

    ![LinkedIn-account - Set geautoriseerd Omleidings-URL 's](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app3.png)

    >[!NOTE]
    >Het clientgeheim is een belangrijke beveiligingsreferentie. Dit geheim met iedereen delen en distribueren met uw app niet.

5. Selecteer **Toevoegen**.

6. Selecteer **instellingen**, wijzigt de **toepassingsstatus** naar **Live**, en selecteer vervolgens **Update**.

    ![LinkedIn-account - status van de toepassing instellen](media/active-directory-b2c-custom-setup-li-idp/adb2c-ief-setup-li-idp-new-app4.png)

## <a name="step-2-add-your-linkedin-application-key-to-azure-ad-b2c"></a>Stap 2: De sleutel van uw LinkedIn-toepassing toevoegen aan Azure AD B2C
Federatie met LinkedIn-accounts vereist een clientgeheim voor de LinkedIn-account aan Azure AD B2C-vertrouwensrelatie namens de toepassing. Voor het opslaan van de LinkedIn-toepassingsgeheim in uw Azure AD B2C-tenant, het volgende doen:  

1. Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen** > **Identity-Ervaringsframework**.

2. Als u de sleutels die beschikbaar in uw tenant zijn, selecteer **Beleidssleutels**.

3. Selecteer **Toevoegen**.

4. In de **opties** Schakel **uploaden**.

5. In de **naam** in het vak **B2cRestClientCertificate**.  
    Het voorvoegsel *B2C_1A_* mogelijk automatisch worden toegevoegd.

6. In de **geheim** voert u uw LinkedIn-toepassingsgeheim uit de [Portal voor Appregistratie](https://apps.dev.microsoft.com).

7. Voor **sleutelgebruik**, selecteer **versleuteling**.

8. Selecteer **Maken**. 

9. Bevestig dat u hebt gemaakt de `B2C_1A_LinkedInSecret`sleutel.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>Stap 3: Een claimprovider in de uitbreiding beleid toevoegen
Als u wilt dat gebruikers zich aanmelden met behulp van hun LinkedIn-account, moet u LinkedIn definiëren als een claimprovider. Met andere woorden, moet u de eindpunten die Azure AD B2C met communiceert opgeven. De eindpunten bieden een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd.

LinkedIn definiëren als een claimprovider door toe te voegen een `<ClaimsProvider>` knooppunt in de uitbreiding beleid-bestand:

1. Open in uw werkmap en de *TrustFrameworkExtensions.xml* extensiebestand beleid. 

2. Zoek de `<ClaimsProviders>` element.

3. In de `<ClaimsProviders>` -element, Voeg het volgende XML-fragment toe: 

    ```xml
    <ClaimsProvider>
      <Domain>linkedin.com</Domain>
      <DisplayName>LinkedIn</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="LinkedIn-OAUTH">
          <DisplayName>LinkedIn</DisplayName>
          <Protocol Name="OAuth2" />
          <Metadata>
            <Item Key="ProviderName">linkedin</Item>
            <Item Key="authorization_endpoint">https://www.linkedin.com/oauth/v2/authorization</Item>
            <Item Key="AccessTokenEndpoint">https://www.linkedin.com/oauth/v2/accessToken</Item>
            <Item Key="ClaimsEndpoint">https://api.linkedin.com/v1/people/~:(id,first-name,last-name,email-address,headline)</Item>
            <Item Key="ClaimsEndpointAccessTokenName">oauth2_access_token</Item>
            <Item Key="ClaimsEndpointFormatName">format</Item>
            <Item Key="ClaimsEndpointFormat">json</Item>
            <Item Key="scope">r_emailaddress r_basicprofile</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">0</Item>
            <Item Key="client_id">Your LinkedIn application client ID</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_LinkedInSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="emailAddress" />
            <!--<OutputClaim ClaimTypeReferenceId="jobTitle" PartnerClaimType="headline" />-->
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="linkedin.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
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

4. Vervang de *client_id* waarde met uw LinkedIn-toepassing client-ID.

5. Sla het bestand op.

## <a name="step-4-register-the-linkedin-account-claims-provider"></a>Stap 4: De LinkedIn-account claimprovider registreren
U kunt de id-provider hebt ingesteld. Maar is het nog niet beschikbaar in een van de windows registreren of aanmelden. Nu u de id-provider van LinkedIn-account aan uw gebruikers toevoegen moet `SignUpOrSignIn` gebruikersbeleving.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Stap 4.1: Maak een kopie van de gebruikersbeleving
De gebruikersbeleving om beschikbaar te maken, moet u een duplicaat van een bestaande sjabloon voor de reis van gebruiker en voegt u de LinkedIn-id-provider:

>[!NOTE]
>Als u hebt gekopieerd de `<UserJourneys>` element uit de base-bestand van uw beleid aan de *TrustFrameworkExtensions.xml* extensiebestand is, kunt u deze sectie overslaan.

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).

2. Zoek de `<UserJourneys>` -element, selecteert u de volledige inhoud van de `<UserJourney>` knooppunt en selecteert u vervolgens **Knippen** te verplaatsen van de geselecteerde tekst naar het Klembord.

3. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en zoek de `<UserJourneys>` element. Als het element niet bestaat, deze toevoegen.

4. Plak de volledige inhoud van de `<UserJourney>` knooppunt, dat u hebt verplaatst naar het Klembord in stap 2, in de `<UserJourneys>` element.

### <a name="step-42-display-the-button"></a>Stap 4.2: De 'knop' weergeven
De `<ClaimsProviderSelections>` element wordt de lijst met opties voor de selectie van claims-provider en de volgorde gedefinieerd. De `<ClaimsProviderSelection>` knooppunt is vergelijkbaar met een knop identity provider op een pagina voor registreren of aanmelden. Als u een `<ClaimsProviderSelection>` knooppunt voor een LinkedIn-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Als u wilt toevoegen van dit element, het volgende doen:

1. Zoek de `<UserJourney>` knooppunt dat bevat `Id="SignUpOrSignIn"` in de gebruikersbeleving die u hebt gekopieerd.

2. Zoek de `<OrchestrationStep>` knooppunt met `Order="1"`.

3. In de `<ClaimsProviderSelections>` -element, Voeg het volgende XML-fragment toe:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="LinkedInExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>Stap 4.3: De knop koppelen aan een actie
Nu dat u een knop op locatie hebt, moet u deze koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met de LinkedIn-account voor het ontvangen van een token. De knop koppelen aan een actie door koppelen aan het technische profiel voor de claimprovider van uw LinkedIn-account:

1. Zoek de `<OrchestrationStep>` knooppunt dat bevat `Order="2"` in de `<UserJourney>` knooppunt.

2. In de `<ClaimsExchanges>` -element, Voeg het volgende XML-fragment toe:

    ```xml
    <ClaimsExchange Id="LinkedInExchange" TechnicalProfileReferenceId="LinkedIn-OAuth" />
    ```

    >[!NOTE]
    >* Zorg ervoor dat `Id` heeft dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie.
    >* Zorg ervoor dat de `TechnicalProfileReferenceId` -ID is ingesteld op het technische profiel dat u eerder (LinkedIn-OAuth) gemaakt.

## <a name="step-5-upload-the-policy-to-your-tenant"></a>Stap 5: Het beleid voor uploaden naar uw tenant
1. In de [Azure-portal](https://portal.azure.com), Ga naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en selecteer vervolgens **Azure AD B2C**.

2. Selecteer **Identity-Ervaringsframework**.

3. Selecteer **alle beleidsregels**.

4. Selecteer **beleid uploaden**.

5. Selecteer de **het beleid overschrijven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkBase.xml* en *TrustFrameworkExtensions.xml* bestanden en ervoor te zorgen dat ze gevalideerd worden.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>Stap 6: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Selecteer **Azure AD B2C-instellingen**, en selecteer vervolgens **Identity-Ervaringsframework**.

    >[!NOTE]
    >Voer nu vereist dat ten minste één toepassing vooraf op de tenant worden geregistreerd. Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast beleid u geüpload en selecteer vervolgens **nu uitvoeren**.  
    U zou nu moeten kunnen aanmelden met behulp van de LinkedIn-account.

## <a name="step-7-optional-register-the-linkedin-account-claims-provider-to-the-profile-edit-user-journey"></a>Stap 7: (Optioneel) Registreer de LinkedIn-account claims provider voor de gebruikersbeleving profiel bewerken
U kunt ook om toe te voegen van de LinkedIn-account-id-provider op uw `ProfileEdit` gebruikersbeleving. Zodat de gebruiker reis beschikbaar, herhaalt 'stap 4." Selecteer deze keer de `<UserJourney>` knooppunt dat bevat `Id="ProfileEdit"`. Opslaan, uploaden en te testen van uw beleid.

## <a name="optional-download-the-complete-policy-files"></a>(Optioneel) De volledige bestanden downloaden
Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario bouwen met behulp van uw eigen aangepaste beleidsbestanden. Ter referentie, we hebben opgegeven [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-li-app).
