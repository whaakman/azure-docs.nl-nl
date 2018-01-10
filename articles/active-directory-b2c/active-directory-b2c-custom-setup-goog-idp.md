---
title: 'Azure Active Directory B2C: Voeg Google + als een OAuth2-id-provider met behulp van aangepaste beleid'
description: Voorbeeld met behulp van Google + als id-provider met behulp van OAuth2-protocol
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: d389a44ce38d84e510060f3b0a53cda58513dee5
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Voeg Google + als een OAuth2-id-provider met behulp van aangepaste beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Deze handleiding ziet u het inschakelen van aanmelden voor gebruikers van Google + account met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten

Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:

1.  Maken van een toepassing Google + account.
2.  Google + Toepassingssleutel toevoegen aan Azure AD B2C
3.  Het toevoegen van de claimprovider naar een door beleid
4.  De provider wordt geregistreerd Google + account claims voor een gebruiker reis
5.  Uploaden van het beleid naar een Azure AD B2C-tenant en testen

## <a name="create-a-google-account-application"></a>Een toepassing Google + account maken
Voor het gebruik van Google + als een id-provider in Azure Active Directory (Azure AD) B2C, moet u een Google +-toepassing maken en geeft deze met de juiste parameters. U kunt een Google +-toepassing hier registreren: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

1.  Ga naar de [Google ontwikkelaars Console](https://console.developers.google.com/) en meld u aan met de referenties van uw Google + account.
2.  Klik op **project maken**, voer een **projectnaam**, en klik vervolgens op **maken**.

3.  Klik op de **menu projecten**.

    ![Google + account - Selecteer project](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Klik op de  **+**  knop.

    ![Google + rekening - nieuw project maken](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Voer een **projectnaam**, en klik vervolgens op **maken**.

    ![Google + account - nieuw project](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Wacht totdat het project gereed is en klik op de **menu projecten**.

    ![Google + account - wacht totdat de nieuw project is klaar voor gebruik](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Klik op de projectnaam van uw.

    ![Selecteer het nieuwe project-Google + account:](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Klik op **API Manager** en klik vervolgens op **referenties** in de linkernavigatiebalk.
9.  Klik op de **OAuth toestemming scherm** boven op het tabblad.

    ![Google + account - ingesteld OAuth toestemming scherm](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Selecteer of geef een geldige **e-mailadres**, bieden een **Productnaam**, en klik op **opslaan**.

    ![Google + - toepassing-referenties](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Klik op **nieuwe referenties** en kies vervolgens **OAuth-Clientidentiteit**.

    ![Google + - referenties van de nieuwe toepassing maken](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  Onder **toepassingstype**, selecteer **webtoepassing**.

    ![Google + - toepassingstype selecteren](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Geef een **naam** voor uw toepassing, voert u `https://login.microsoftonline.com` in de **geautoriseerd JavaScript oorsprongen** veld en `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **geautoriseerde omleidings-URI's** veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c.onmicrosoft.com). De **{tenant}** hoofdlettergevoelig. Klik op **Create**.

    ![Google + - JavaScript geautoriseerd oorsprongen bieden en omleidings-URI's](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Kopieer de waarden van **Client-Id** en **clientgeheim**. U moet zowel voor het configureren van Google + als een id-provider in uw tenant. **Clientgeheim** is een belangrijke beveiligingsreferentie.

    ![Google + - Kopieer de waarden van de client-Id en Client geheime](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-the-google-account-application-key-to-azure-ad-b2c"></a>De Google + account Toepassingssleutel toevoegen aan Azure AD B2C
Federatie met Google + accounts vereist een clientgeheim voor Google + account aan Azure AD B2C-vertrouwensrelatie voor de toepassing is. U moet uw Google + toepassingsgeheim opslaan in Azure AD B2C-tenant:  

1.  Ga naar uw Azure AD B2C-tenant en selecteer **B2C-instellingen** > **identiteit ervaring Framework**
2.  Selecteer **beleid sleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.
3.  Klik op **+ toevoegen**.
4.  Voor **opties**, gebruik **handmatige**.
5.  Voor **naam**, gebruik `GoogleSecret`.  
    Het voorvoegsel `B2C_1A_` kunnen automatisch worden toegevoegd.
6.  In de **geheim** Voer uw Google-toepassingsgeheim van de [Google ontwikkelaars Console](https://console.developers.google.com/) die u hierboven hebt gekopieerd.
7.  Voor **sleutelgebruik**, gebruik **handtekening**.
8.  Klik op **Maken**.
9.  Controleer of u de sleutel hebt gemaakt `B2C_1A_GoogleSecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Toevoegen van een claimprovider in de uitbreiding beleid

Als u wilt dat gebruikers zich aanmelden via Google + account, moet u Google + account definiëren als een claimprovider. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceert opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren dat een specifieke gebruiker is geverifieerd.

Google + Account definiëren als een claimprovider door toe te voegen `<ClaimsProvider>` knooppunt in het beleidsbestand extensie:

1.  Open het bestand met de extensie (TrustFrameworkExtensions.xml) van uw werkmap. Als u een XML-editor moet [Visual Studio Code probeert](https://code.visualstudio.com/download), een lichtgewicht platformoverschrijdende-editor.
2.  Zoek de `<ClaimsProviders>` sectie
3.  Voeg de volgende XML-fragment onder de `ClaimsProviders` element en vervang `client_id` waarde met Google + toepassing client ID van uw account voordat u het bestand opslaat.  

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

## <a name="register-the-google-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>De Google + account claimprovider om te registreren of aanmelden gebruiker reis registreren

De id-provider is ingesteld.  Het is echter niet beschikbaar in een van de schermen sign-up-to-date/aanmelden. De Google + account id-provider toevoegen aan uw gebruikers `SignUpOrSignIn` traject van de gebruiker. Als u deze beschikbaar, maken we een duplicaat van een bestaande sjabloon gebruiker reis.  Vervolgens wordt de Google + account id-provider toevoegen:

>[!NOTE]
>
>Als u hebt gekopieerd de `<UserJourneys>` element uit de base-bestand van uw beleid aan het extensiebestand (TrustFrameworkExtensions.xml), kunt u doorgaan met deze sectie.

1.  Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
2.  Zoek de `<UserJourneys>` element en kopieert u de volledige inhoud van `<UserJourneys>` knooppunt.
3.  Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en Ga naar de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
4.  Plak de volledige inhoud van `<UserJourney>` knooppunt dat u hebt gekopieerd als een onderliggend element van de `<UserJourneys>` element.

### <a name="display-the-button"></a>De knop weergeven
De `<ClaimsProviderSelections>` element wordt de lijst met opties voor de selectie van claims provider en de volgorde gedefinieerd.  `<ClaimsProviderSelection>`element is vergelijkbaar met een knop identiteit provider op een pagina sign-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` element voor Google + account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1.  Zoek de `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"` in het traject gebruiker die u hebt gekopieerd.
2.  Zoek de `<OrchestrationStep>` knooppunt bevat`Order="1"`
3.  Plaats de volgende XML-fragment onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
Nu u een knop hebt geïmplementeerd, moet u deze koppelen aan een actie. Er is in dit geval de actie voor Azure AD B2C om te communiceren met Google + account geen token ontvangen.

1.  Zoek de `<OrchestrationStep>` die omvat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Plaats de volgende XML-fragment onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Zorg ervoor dat de `Id` heeft dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie
> * Zorg ervoor dat `TechnicalProfileReferenceId` -ID is ingesteld in het technische profiel u eerdere (Google OAUTH) gemaakt.

## <a name="upload-the-policy-to-your-tenant"></a>Het beleid uploaden naar uw tenant
1.  In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2.  Selecteer **identiteit ervaring Framework**.
3.  Open de **alle beleidsregels** blade.
4.  Selecteer **uploaden beleid**.
5.  Controleer **het beleid overschreven als deze bestaat** vak.
6.  **Uploaden** TrustFrameworkExtensions.xml en zorg ervoor dat deze niet de validatie mislukt

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van nu uitvoeren
1.  Open **Azure AD B2C-instellingen** en Ga naar **identiteit ervaring Framework**.

    >[!NOTE]
    >
    >    **Nu uitvoeren** vereist ten minste één toepassing om te worden preregistered op de tenant. 
    >    Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.


2.  Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid die u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet zich aanmelden met Google + account.

## <a name="optional-register-the-google-account-claims-provider-to-profile-edit-user-journey"></a>[Optioneel] De Google + account claimprovider te bewerken van profielen gebruiker reis registreren
U kunt de Google + account identiteitsprovider ook toevoegen aan uw gebruikers `ProfileEdit` traject van de gebruiker. Als u deze beschikbaar, herhaalt u de laatste twee stappen:

### <a name="display-the-button"></a>De knop weergeven
1.  Open het extensiebestand van uw beleid (bijvoorbeeld TrustFrameworkExtensions.xml).
2.  Zoek de `<UserJourney>` knooppunt met `Id="ProfileEdit"` in het traject gebruiker die u hebt gekopieerd.
3.  Zoek de `<OrchestrationStep>` knooppunt bevat`Order="1"`
4.  Plaats de volgende XML-fragment onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
1.  Zoek de `<OrchestrationStep>` die omvat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Plaats de volgende XML-fragment onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="upload-the-policy-to-your-tenant"></a>Het beleid uploaden naar uw tenant
1.  In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2.  Selecteer **identiteit ervaring Framework**.
3.  Open de **alle beleidsregels** blade.
4.  Selecteer **uploaden beleid**.
5.  Controleer de **het beleid overschreven als deze bestaat** vak.
6.  **Uploaden** TrustFrameworkExtensions.xml en zorg ervoor dat deze niet de validatie mislukt.

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Het aangepaste profiel bewerken beleid testen met behulp van nu uitvoeren

1.  Open **Azure AD B2C-instellingen** en Ga naar **identiteit ervaring Framework**.
2.  Open **B2C_1A_ProfileEdit**, de relying party (RP) aangepast-beleid die u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet zich aanmelden met Google + account.

## <a name="download-the-complete-policy-files"></a>De volledige beleidsbestanden downloaden
Optioneel: We raden aan bouwen van uw scenario in plaats van deze voorbeeldbestanden met behulp van uw eigen aangepaste beleid bestanden na het voltooien van de aan de slag met aangepast beleid dat is doorlopen.  [Beleid voorbeeldbestanden voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
