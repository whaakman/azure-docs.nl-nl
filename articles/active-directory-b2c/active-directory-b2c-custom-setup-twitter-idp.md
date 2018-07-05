---
title: Twitter als id-provider OAuth1 toevoegen met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Gebruik Twitter als id-provider met behulp van de OAuth1-protocol.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/23/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 786f0dfd0cf3cf2e9ab0d16e26811fabd6bfc17c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440944"
---
# <a name="azure-active-directory-b2c-add-twitter-as-an-oauth1-identity-provider-by-using-custom-policies"></a>Azure Active Directory B2C: Twitter als id-provider OAuth1 toevoegen met behulp van aangepaste beleidsregels
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelding voor gebruikers van een Twitter-account inschakelen met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

## <a name="step-1-create-a-twitter-account-application"></a>Stap 1: Een Twitter-account-toepassing maken
Gebruik van Twitter als id-provider in Azure Active Directory B2C (Azure AD B2C) heeft, moet u een Twitter-toepassing maken en geven met de juiste parameters. U kunt een Twitter-toepassing registreren door te gaan naar de [pagina voor het registreren van Twitter](https://twitter.com/signup).

1. Ga naar de [Twitter ontwikkelaars](https://apps.twitter.com/) website, meld u aan met de referenties van uw Twitter-account en selecteer vervolgens **Create New App**.

    ![Twitter-account: nieuwe app maken](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app1.png)

2. In de **maken van een toepassing** venster de volgende handelingen uit:
 
    a. Type de **naam** en een **beschrijving** voor uw nieuwe app. 

    b. In de **Website** vak, plak **https://login.microsoftonline.com**. 

    c. 4. Voor de **URL voor terugbellen**, voer `https://login.microsoftonline.com/te/{tenant}/{policyId}/oauth1/authresp`. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com) en **{policyId}** met uw beleids-id (bijvoorbeeld b2c_1_policy).  **De callback URL moet zich in alleen kleine letters.** U moet een URL voor terugbellen voor alle beleidsregels die gebruikmaken van de Twitter-aanmelding toevoegen. Zorg ervoor dat u `b2clogin.com` in plaats van ` login.microsoftonline.com` als u deze in uw toepassing.

    d. Aan de onderkant van de pagina, lees en accepteer de voorwaarden en selecteer vervolgens **uw Twitter-toepassing maken**.

    ![Twitter-account: een nieuwe app toevoegen](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app2.png)

3. In de **B2C demo** venster **instellingen**, selecteer de **toestaan dat deze toepassing worden gebruikt voor het aanmelden met Twitter** selectievakje en selecteer vervolgens **Update Instellingen voor**.

4. Selecteer **Keys and Access Tokens**, en noteer de **Consumer Key (API-sleutel)** en **Consumer Secret (API-geheim)** waarden.

    ![Twitter-account - eigenschappen van de toepassing instellen](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app3.png)

    >[!NOTE]
    >De consumentgeheim is een belangrijke beveiligingsreferentie. Dit geheim met iedereen delen en distribueren met uw app niet.

## <a name="step-2-add-your-twitter-account-application-key-to-azure-ad-b2c"></a>Stap 2: De Toepassingssleutel van uw Twitter-account toevoegen aan Azure AD B2C
Federatie met Twitter-accounts vereist een consumentgeheim voor de Twitter-account aan Azure AD B2C-vertrouwensrelatie namens de toepassing. Voor het opslaan van de Twitter-toepassing consumentgeheim in uw Azure AD B2C-tenant, het volgende doen: 

1. Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen** > **Identity-Ervaringsframework**.

2. Als u de sleutels die beschikbaar in uw tenant zijn, selecteer **Beleidssleutels**.

3. Selecteer **Toevoegen**.

4. In de **opties** Schakel **handmatig**.

5. In de **naam** Schakel **TwitterSecret**.  
    Het voorvoegsel *B2C_1A_* mogelijk automatisch worden toegevoegd.

6. In de **geheim** voert u uw Microsoft-toepassingsgeheim uit de [Portal voor Appregistratie](https://apps.dev.microsoft.com).

7. Voor **sleutelgebruik**, gebruikt u **versleuteling**.

8. Selecteer **Maken**.

9. Bevestig dat u hebt gemaakt de `B2C_1A_TwitterSecret` sleutel.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>Stap 3: Een claimprovider in de uitbreiding beleid toevoegen

Als u wilt dat gebruikers zich aanmelden via Twitter-account, moet u Twitter definiëren als een claimprovider. Met andere woorden, moet u de eindpunten die Azure AD B2C met communiceert opgeven. De eindpunten bieden een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd.

Twitter definiëren als een claimprovider door toe te voegen `<ClaimsProvider>` knooppunt in de uitbreiding beleid-bestand:

1. Open in uw werkmap en de *TrustFrameworkExtensions.xml* extensiebestand beleid. 

2. Zoek de `<ClaimsProviders>` sectie.

3. In de `<ClaimsProviders>` knooppunt, Voeg het volgende XML-fragment toe:  

    ```xml
    <ClaimsProvider>
        <Domain>twitter.com</Domain>
        <DisplayName>Twitter</DisplayName>
        <TechnicalProfiles>
        <TechnicalProfile Id="Twitter-OAUTH1">
            <DisplayName>Twitter</DisplayName>
            <Protocol Name="OAuth1" />
            <Metadata>
            <Item Key="ProviderName">Twitter</Item>
            <Item Key="authorization_endpoint">https://api.twitter.com/oauth/authenticate</Item>
            <Item Key="access_token_endpoint">https://api.twitter.com/oauth/access_token</Item>
            <Item Key="request_token_endpoint">https://api.twitter.com/oauth/request_token</Item>
            <Item Key="ClaimsEndpoint">https://api.twitter.com/1.1/account/verify_credentials.json?include_email=true</Item>
            <Item Key="ClaimsResponseFormat">json</Item>
            <Item Key="client_id">Your Twitter application consumer key</Item>
            </Metadata>
            <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_TwitterSecret" />
            </CryptographicKeys>
            <InputClaims />
            <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="user_id" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
            <OutputClaim ClaimTypeReferenceId="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
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

4. Vervang de *client_id*'-waarde met de sleutel van de toepassing consumenten voor uw Twitter-account.

5. Sla het bestand op.

## <a name="step-4-register-the-twitter-account-claims-provider-to-your-sign-up-or-sign-in-user-journey"></a>Stap 4: De claimprovider van Twitter-account aan uw reis registreren of aanmelden gebruiker registreren
U kunt de id-provider hebt ingesteld. Maar is het nog niet beschikbaar in een van de windows registreren of aanmelden. Nu u de Twitter-account-id-provider aan uw gebruikers toevoegen moet `SignUpOrSignIn` gebruikersbeleving.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Stap 4.1: Maak een kopie van de gebruikersbeleving
De gebruikersbeleving om beschikbaar te maken, moet u een duplicaat van een bestaande sjabloon voor de reis van gebruiker en voegt u de Twitter-id-provider:

>[!NOTE]
>Als u hebt gekopieerd de `<UserJourneys>` element uit de base-bestand van uw beleid aan de *TrustFrameworkExtensions.xml* extensiebestand is, kunt u doorgaan met de volgende sectie.

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).

2. Zoek de `<UserJourneys>` -element, selecteert u de volledige inhoud van de `<UserJourney>` knooppunt en selecteert u vervolgens **Knippen** te verplaatsen van de geselecteerde tekst naar het Klembord.

3. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en zoek vervolgens de `<UserJourneys>` element. Als het element niet bestaat, deze toevoegen.

4. Plak de volledige inhoud van de `<UserJourney>` knooppunt, dat u hebt verplaatst naar het Klembord in stap 2, in de `<UserJourneys>` element.

### <a name="step-42-display-the-button"></a>Stap 4.2: De 'knop' weergeven
De `<ClaimsProviderSelections>` element wordt de lijst met opties voor de selectie van claims-provider en de volgorde gedefinieerd. De `<ClaimsProviderSelection>` knooppunt is vergelijkbaar met een knop identity provider op een pagina voor registreren of aanmelden. Als u een `<ClaimsProviderSelection>` knooppunt voor een Twitter-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Als u wilt toevoegen van dit element, het volgende doen:

1. Zoek de `<UserJourney>` knooppunt dat bevat `Id="SignUpOrSignIn"` in de gebruikersbeleving die u hebt gekopieerd.

2. Zoek de `<OrchestrationStep>` knooppunt dat bevat `Order="1"`.

3. In de `<ClaimsProviderSelections>` -element, Voeg het volgende XML-fragment toe:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>Stap 4.3: De knop koppelen aan een actie
Nu dat u een knop op locatie hebt, moet u deze koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met de Twitter-account voor het ontvangen van een token. De knop koppelen aan een actie door koppelen aan het technische profiel voor de claimprovider van uw Twitter-account:

1. Zoek de `<OrchestrationStep>` knooppunt dat bevat `Order="2"` in de `<UserJourney>` knooppunt.
2. In de `<ClaimsExchanges>` -element, Voeg het volgende XML-fragment toe:

    ```xml
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
    ```

    >[!NOTE]
    >* Zorg ervoor dat `Id` heeft dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie.
    >* Zorg ervoor dat de `TechnicalProfileReferenceId` -ID is ingesteld op het technische profiel dat u eerder (Twitter-OAUTH1) gemaakt.

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
    U zou nu moeten kunnen aanmelden met behulp van de Twitter-account.

## <a name="step-7-optional-register-the-twitter-account-claims-provider-to-the-profile-edit-user-journey"></a>Stap 7: (Optioneel) Registreer de Twitter-account claims provider voor de gebruikersbeleving profiel bewerken
U kunt ook om toe te voegen van de Twitter-account-id-provider op uw `ProfileEdit` gebruikersbeleving. Zodat de gebruiker reis beschikbaar, herhaalt 'stap 4." Selecteer deze keer de `<UserJourney>` knooppunt dat bevat `Id="ProfileEdit"`. Opslaan, uploaden en te testen van uw beleid.


## <a name="optional-download-the-complete-policy-files"></a>(Optioneel) De volledige bestanden downloaden
Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario bouwen met behulp van uw eigen aangepaste beleidsbestanden. Ter referentie, we hebben opgegeven [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-twitter-app).
