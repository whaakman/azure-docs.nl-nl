---
title: Voeg Google + als een OAuth2-id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Voorbeeld met Google + als id-provider met behulp van OAuth2-protocol.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f076a906ba38e6c8e8c9530baba1607553b41ea6
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338325"
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Toevoegen Google + als een OAuth2-id-provider met behulp van aangepaste beleidsregels

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Deze handleiding wordt beschreven hoe u om in te schakelen aanmelding voor gebruikers van Google + account via het gebruik van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten

Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:

1.  Het maken van een toepassing Google +-account.
2.  Google + accountsleutel van de toepassing toe te voegen aan Azure AD B2C
3.  Claimprovider toevoegen aan een beleid
4.  Google + account claimprovider registreren voor een gebruikersbeleving
5.  Het uploaden van het beleid naar een Azure AD B2C-tenant en het testen

## <a name="create-a-google-account-application"></a>Een toepassing Google +-account maken
Voor het gebruik van Google + als id-provider in Azure Active Directory (Azure AD) B2C, moet u een Google +-toepassing maken en geven met de juiste parameters. U kunt een Google +-toepassing hier registreren: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

1.  Ga naar de [Google ontwikkelaars Console](https://console.developers.google.com/) en meld u aan met de referenties van uw Google + account.
2.  Klik op **project maken**, voer een **projectnaam**, en klik vervolgens op **maken**.

3.  Klik op de **menu projecten**.

    ![Google + account - project selecteren](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Klik op de **+** knop.

    ![Google + rekening - nieuw project maken](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Voer een **projectnaam**, en klik vervolgens op **maken**.

    ![Google + account - nieuw project](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Wacht totdat het project klaar is en klik op de **menu projecten**.

    ![Google + account - wacht totdat het nieuwe project is klaar voor gebruik](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Klik op de projectnaam van uw.

    ![Google + account: Selecteer het nieuwe project](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Klik op **API Manager** en klik vervolgens op **referenties** in het linkernavigatievenster.
9.  Klik op de **OAuth instemmingsscherm** tabblad bovenaan.

    ![Google + account - toestemmingsscherm OAuth instellen](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Selecteer of geef een geldige **e-mailadres**, bieden een **Productnaam**, en klikt u op **opslaan**.

    ![Google + - toepassing-referenties](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Klik op **nieuwe referenties** en kies vervolgens **OAuth-Clientidentiteit**.

    ![Google + - referenties voor nieuwe toepassingen maken](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  Onder **toepassingstype**, selecteer **webtoepassing**.

    ![Google + - toepassingstype selecteren](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Geef een **naam** voor uw toepassing, voert u `https://{tenant}.b2clogin.com` in de **geautoriseerd JavaScript oorsprongen** veld en `https://{tenant}.b2clogin.com/te/{tenant}.onmicrosoft.com/oauth2/authresp` in de **geautoriseerde omleidings-URI's** het veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c). De **{tenant}** hoofdlettergevoelig. Klik op **Create**.

    ![Google + - JavaScript geautoriseerd oorsprongen opgeven en omleidings-URI's](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Kopieer de waarden van **Client-Id** en **clientgeheim**. U moet zowel het configureren van Google + als id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.

    ![Google + - Kopieer de waarden van de client-Id en het clientgeheim](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-the-google-account-application-key-to-azure-ad-b2c"></a>Google + accountsleutel van de toepassing toevoegen aan Azure AD B2C
Federatie met Google + accounts vereist een clientgeheim voor Google +-account aan Azure AD B2C-vertrouwensrelatie namens de toepassing. U moet voor het opslaan van uw Google + toepassingsgeheim in Azure AD B2C-tenant:  

1.  Ga naar uw Azure AD B2C-tenant, en selecteer **B2C-instellingen** > **Identity-Ervaringsframework**
2.  Selecteer **Beleidssleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.
3.  Klik op **+ toevoegen**.
4.  Voor **opties**, gebruikt u **handmatig**.
5.  Voor **naam**, gebruikt u `GoogleSecret`.  
    Het voorvoegsel `B2C_1A_` mogelijk automatisch worden toegevoegd.
6.  In de **geheim** voert u uw Google-toepassingsgeheim uit de [Google ontwikkelaars Console](https://console.developers.google.com/) die u hebt gekopieerd hierboven.
7.  Voor **sleutelgebruik**, gebruikt u **handtekening**.
8.  Klik op **Maken**.
9.  Bevestig dat u de sleutel hebt `B2C_1A_GoogleSecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Toevoegen van een claimprovider in uw extensie-beleid

Als u wilt dat gebruikers zich aanmelden met Google +-account, moet u voor het definiëren van Google + account als een claimprovider. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceert opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd.

Google + Account definiëren als een claimprovider door toe te voegen `<ClaimsProvider>` knooppunt in de uitbreiding beleid-bestand:

1.  Open het beleid extensiebestand (TrustFrameworkExtensions.xml) van uw werkmap. Als u een XML-editor, moet [probeer Visual Studio Code](https://code.visualstudio.com/download), een lichtgewicht editor voor meerdere platforms.
2.  Zoek de `<ClaimsProviders>` sectie
3.  Voeg de volgende XML-fragment uit onder de `ClaimsProviders` -element en vervang `client_id` waarde met Google + account client ID van uw toepassing voordat u het bestand opslaat.  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want the user to select his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-the-google-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>De Google + account claimprovider registreren om te registreren of aanmelden in de gebruikersbeleving

De id-provider is ingesteld.  Het is echter niet beschikbaar in elk van de schermen aanmelden-up-to-date/aanmelden. De Google + account id-provider toevoegen aan uw gebruikers `SignUpOrSignIn` gebruikersbeleving. Om het beschikbaar maken, maken we een duplicaat van een bestaande sjabloon voor de gebruikersbeleving.  Vervolgens voegen we de Google + account id-provider:

>[!NOTE]
>
>Als u hebt gekopieerd de `<UserJourneys>` element van het base-bestand van uw beleid aan het extensiebestand (TrustFrameworkExtensions.xml), kunt u doorgaan met deze sectie.

1.  Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
2.  Zoek de `<UserJourneys>` element en kopieer de gehele inhoud van `<UserJourneys>` knooppunt.
3.  Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en zoek de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
4.  Plak de volledige inhoud van `<UserJourney>` knooppunt dat u hebt gekopieerd als onderliggende site van de `<UserJourneys>` element.

### <a name="display-the-button"></a>De knop weergeven
De `<ClaimsProviderSelections>` element wordt de lijst met opties voor de selectie van claims-provider en de volgorde gedefinieerd.  `<ClaimsProviderSelection>` element is vergelijkbaar met een knop identity provider op een pagina aanmelden-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` -element voor Google + account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1.  Zoek de `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"` in de gebruikersbeleving die u hebt gekopieerd.
2.  Zoek de `<OrchestrationStep>` knooppunt met `Order="1"`
3.  Voeg de volgende XML-fragment uit onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met Google + account voor het ontvangen van een token.

1.  Zoek de `<OrchestrationStep>` die bevat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Voeg de volgende XML-fragment uit onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Zorg ervoor dat de `Id` heeft dezelfde waarde als die van `TargetClaimsExchangeId` in de voorgaande sectie
> * Zorg ervoor dat `TechnicalProfileReferenceId` -ID is ingesteld in het technische profiel u eerdere (Google-OAUTH) hebt gemaakt.

## <a name="upload-the-policy-to-your-tenant"></a>Uploaden van het beleid aan uw tenant
1.  In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2.  Selecteer **Identity-Ervaringsframework**.
3.  Open de **alle beleidsregels** blade.
4.  Selecteer **beleid uploaden**.
5.  Controleer **het beleid overschrijven als deze bestaat** vak.
6.  **Uploaden** TrustFrameworkExtensions.xml en zorg ervoor dat deze niet de validatie mislukt

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van nu uitvoeren
1.  Open **Azure AD B2C-instellingen** en Ga naar **Identity-Ervaringsframework**.

    >[!NOTE]
    >
    >    **Nu uitvoeren** vereist ten minste één toepassing vooraf op de tenant worden geregistreerd. 
    >    Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.


2.  Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid dat u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet aanmelden met Google +-account.

## <a name="optional-register-the-google-account-claims-provider-to-profile-edit-user-journey"></a>[Optioneel] De Google + account claimprovider registreren om te bewerken van profielen gebruikersbeleving
U kunt de Google + account id-provider ook toevoegen aan uw gebruikers `ProfileEdit` gebruikersbeleving. Om het beschikbaar maken, herhaalt u de laatste twee stappen:

### <a name="display-the-button"></a>De knop weergeven
1.  Open het bestand uitbreiding van uw beleid (bijvoorbeeld TrustFrameworkExtensions.xml).
2.  Zoek de `<UserJourney>` knooppunt met `Id="ProfileEdit"` in de gebruikersbeleving die u hebt gekopieerd.
3.  Zoek de `<OrchestrationStep>` knooppunt met `Order="1"`
4.  Voeg de volgende XML-fragment uit onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
1.  Zoek de `<OrchestrationStep>` die bevat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Voeg de volgende XML-fragment uit onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="upload-the-policy-to-your-tenant"></a>Uploaden van het beleid aan uw tenant
1.  In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2.  Selecteer **Identity-Ervaringsframework**.
3.  Open de **alle beleidsregels** blade.
4.  Selecteer **beleid uploaden**.
5.  Controleer de **het beleid overschrijven als deze bestaat** vak.
6.  **Uploaden** TrustFrameworkExtensions.xml en zorg ervoor dat deze niet de validatie mislukt.

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Het aangepaste profiel bewerken-beleid testen met behulp van nu uitvoeren

1.  Open **Azure AD B2C-instellingen** en Ga naar **Identity-Ervaringsframework**.
2.  Open **B2C_1A_ProfileEdit**, de relying party (RP) aangepast-beleid dat u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet aanmelden met Google +-account.

## <a name="download-the-complete-policy-files"></a>De volledige bestanden downloaden
Optioneel: Wordt aangeraden bouwen van uw scenario met behulp van uw eigen aangepaste beleid bestanden na het voltooien van de aan de slag met aangepast beleid doorlopen in plaats van deze voorbeeldbestanden.  [Voorbeeld-beleidsbestanden ter referentie](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
