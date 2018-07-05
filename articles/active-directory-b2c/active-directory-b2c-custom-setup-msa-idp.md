---
title: Microsoft-Account (MSA) toevoegen als een id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Voorbeeld met behulp van Microsoft als id-provider met behulp van OIDC (OpenID Connect)-protocol.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7a83ace83176d75abdac03b354c4c4ac71eb4238
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449970"
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Microsoft-Account (MSA) toevoegen als een id-provider met behulp van aangepaste beleidsregels

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelding voor gebruikers van Microsoft-account (MSA) inschakelen via het gebruik van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:

1.  Het maken van een toepassing van Microsoft-account.
2.  De sleutel van de Microsoft-account-toepassing toevoegen aan Azure AD B2C
3.  Claimprovider toevoegen aan een beleid
4.  Registreren van de claimprovider Microsoft-Account aan een gebruikersbeleving
5.  Het uploaden van het beleid naar een Azure AD B2C-tenant en het testen

## <a name="create-a-microsoft-account-application"></a>Een toepassing van Microsoft-account maken
Voor het gebruik van Microsoft-account als id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing van Microsoft-account maken en geven met de juiste parameters. U hebt een Microsoft-account nodig. Als u niet hebt, gaat u naar [ https://www.live.com/ ](https://www.live.com/).

1.  Ga naar de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) en meld u aan met uw Microsoft-accountreferenties.
2.  Klik op **een app toevoegen**.

    ![Microsoft-account: een app toevoegen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  Geef een **naam** voor uw toepassing, **Neem contact op met de e-mailbericht**, schakel het selectievakje **zodat we kunnen u helpen** en klikt u op **maken**.

    ![Microsoft-account - uw toepassing registreren](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  Kopieer de waarde van **toepassings-Id**. U moet het Microsoft-account configureren als een id-provider in uw tenant.

    ![Microsoft-account - exemplaar de waarde van de toepassings-Id](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  Klik op **platform toevoegen**

    ![Microsoft-account - platform toevoegen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  Kies in de lijst platform **Web**.

    ![Microsoft-account - Kies in de lijst met platforms Web](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **omleidings-URI's** veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com).

    ![Microsoft-account - Set Omleidings-URL 's](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  Klik op **nieuw wachtwoord genereren** onder de **Toepassingsgeheimen** sectie. Kopieer het nieuwe wachtwoord weergegeven op het scherm. U moet het Microsoft-account configureren als een id-provider in uw tenant. Dit wachtwoord is een belangrijke beveiligingsreferentie.

    ![Microsoft-account: nieuw wachtwoord genereren](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft-account - kopie van het nieuwe wachtwoord](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  Schakel het selectievakje in met de melding dat **Live SDK-ondersteuning** onder de **geavanceerde opties** sectie. Klik op **Opslaan**.

    ![Microsoft-account - ondersteuning voor Live SDK](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a>De sleutel van de Microsoft-account-toepassing toevoegen aan Azure AD B2C
Federatie met Microsoft-accounts vereist een clientgeheim voor Microsoft-account aan Azure AD B2C-vertrouwensrelatie namens de toepassing. U moet voor het opslaan van uw toepassing secert van Microsoft-account in Azure AD B2C-tenant:   

1.  Ga naar uw Azure AD B2C-tenant, en selecteer **B2C-instellingen** > **Identity-Ervaringsframework**
2.  Selecteer **Beleidssleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.
3.  Klik op **+ toevoegen**.
4.  Voor **opties**, gebruikt u **handmatig**.
5.  Voor **naam**, gebruikt u `MSASecret`.  
    Het voorvoegsel `B2C_1A_` mogelijk automatisch worden toegevoegd.
6.  In de **geheim** voert u uw Microsoft-toepassingsgeheim uit https://apps.dev.microsoft.com
7.  Voor **sleutelgebruik**, gebruikt u **handtekening**.
8.  Klik op **Maken**.
9.  Bevestig dat u de sleutel hebt `B2C_1A_MSASecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Toevoegen van een claimprovider in uw extensie-beleid
Als u wilt dat gebruikers zich aanmelden met behulp van Microsoft-Account, moet u voor het definiëren van Microsoft-Account als een claimprovider. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceert opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd.

Microsoft-Account als een claimprovider definiëren door toe te voegen `<ClaimsProvider>` knooppunt in de uitbreiding beleid-bestand:

1.  Open het beleid extensiebestand (TrustFrameworkExtensions.xml) van uw werkmap. Als u een XML-editor, moet [probeer Visual Studio Code](https://code.visualstudio.com/download), een lichtgewicht editor voor meerdere platforms.
2.  Zoek de `<ClaimsProviders>` sectie
3.  Voeg de volgende XML-fragment uit onder de `ClaimsProviders` element:

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

4.  Vervang `client_id` waarde met uw client-Id van de toepassing in Microsoft-Account

5.  Sla het bestand op.

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Het Microsoft-Account claims provider voor het ondertekenen van registreren of aanmelden gebruikersbeleving

Op dit moment wordt de id-provider is ingesteld, maar het is niet beschikbaar zijn in elk van de schermen aanmelden-up-to-date/aanmelden. Nu moet u de id-provider van Microsoft-Account toevoegen aan uw gebruiker `SignUpOrSignIn` gebruikersbeleving. Om het beschikbaar maken, maken we een duplicaat van een bestaande sjabloon voor de gebruikersbeleving.  Vervolgens wordt de Microsoft-Account-id-provider toevoegen:

> [!NOTE]
>
>Als u eerder hebt gekopieerd de `<UserJourneys>` element van het base-bestand van uw beleid aan het extensiebestand `TrustFrameworkExtensions.xml`, kunt u in deze sectie overslaan.

1.  Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
2.  Zoek de `<UserJourneys>` element en kopieer de gehele inhoud van `<UserJourneys>` knooppunt.
3.  Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en zoek de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
4.  Plak de volledige inhoud van `<UserJourneys>` knooppunt dat u hebt gekopieerd als onderliggende site van de `<UserJourneys>` element.

### <a name="display-the-button"></a>De knop weergeven
De `<ClaimsProviderSelections>` element wordt de lijst met opties voor de selectie van claims-provider en de volgorde gedefinieerd.  `<ClaimsProviderSelection>` element is vergelijkbaar met een knop identity provider op een pagina aanmelden-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` -element voor Microsoft-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1.  Zoek de `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"` in de gebruikersbeleving die u hebt gekopieerd.
2.  Zoek de `<OrchestrationStep>` knooppunt met `Order="1"`
3.  Voeg de volgende XML-fragment uit onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MicrosoftAccountExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met Microsoft-Account voor het ontvangen van een token. De knop koppelen aan een actie door koppelen aan het technische profiel voor uw Microsoft-Account claims-provider:

1.  Zoek de `<OrchestrationStep>` die bevat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Voeg de volgende XML-fragment uit onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="MicrosoftAccountExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * Zorg ervoor dat de `Id` heeft dezelfde waarde als die van `TargetClaimsExchangeId` in de voorgaande sectie
>   * Zorg ervoor dat `TechnicalProfileReferenceId` -ID is ingesteld in het technische profiel u eerdere (MSA-OIDC) hebt gemaakt.

## <a name="upload-the-policy-to-your-tenant"></a>Uploaden van het beleid aan uw tenant
1.  In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2.  Selecteer **Identity-Ervaringsframework**.
3.  Open de **alle beleidsregels** blade.
4.  Selecteer **beleid uploaden**.
5.  Controleer **het beleid overschrijven als deze bestaat** vak.
6.  **Uploaden** TrustFrameworkExtensions.xml en zorg ervoor dat deze niet de validatie mislukt

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van nu uitvoeren

1.  Open **Azure AD B2C-instellingen** en Ga naar **Identity-Ervaringsframework**.
> [!NOTE]
>
>**Nu uitvoeren** vereist ten minste één toepassing vooraf op de tenant worden geregistreerd. Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.
2.  Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid dat u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet aanmelden met Microsoft-account.

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a>[Optioneel] Registreren van de claimprovider Microsoft-Account om te bewerken van profielen gebruikersbeleving
U kunt ook de id-provider van Microsoft-Account toevoegen aan uw gebruiker `ProfileEdit` gebruikersbeleving. Om het beschikbaar maken, herhaalt u de laatste twee stappen:

### <a name="display-the-button"></a>De knop weergeven
1.  Open het bestand uitbreiding van uw beleid (bijvoorbeeld TrustFrameworkExtensions.xml).
2.  Zoek de `<UserJourney>` knooppunt met `Id="ProfileEdit"` in de gebruikersbeleving die u hebt gekopieerd.
3.  Zoek de `<OrchestrationStep>` knooppunt met `Order="1"`
4.  Voeg de volgende XML-fragment uit onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
1.  Zoek de `<OrchestrationStep>` die bevat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Voeg de volgende XML-fragment uit onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Het aangepaste profiel bewerken-beleid testen met behulp van nu uitvoeren
1.  Open **Azure AD B2C-instellingen** en Ga naar **Identity-Ervaringsframework**.
2.  Open **B2C_1A_ProfileEdit**, de relying party (RP) aangepast-beleid dat u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet aanmelden met Microsoft-account.

## <a name="download-the-complete-policy-files"></a>De volledige bestanden downloaden
Optioneel: Wordt aangeraden bouwen van uw scenario met behulp van uw eigen aangepaste beleid bestanden na het voltooien van de aan de slag met aangepast beleid doorlopen in plaats van deze voorbeeldbestanden.  [Voorbeeld-beleidsbestanden ter referentie](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
