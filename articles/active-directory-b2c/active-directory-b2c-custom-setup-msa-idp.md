---
title: 'Azure Active Directory B2C: Microsoft-Account (MSA) toevoegen als een id-provider met behulp van aangepaste beleid'
description: Voorbeeld met behulp van Microsoft als id-provider met OpenID Connect (OIDC)-protocol
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
ms.openlocfilehash: cdc77d093358fa15bb1acbc9ba6b1867bae062f8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: Microsoft-Account (MSA) toevoegen als een id-provider met behulp van aangepaste beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel laat zien hoe u aanmelden voor gebruikers van Microsoft-account (MSA) inschakelen met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:

1.  Maken van een toepassing van Microsoft-account.
2.  De Toepassingssleutel van Microsoft-account toevoegen aan Azure AD B2C
3.  Het toevoegen van de claimprovider naar een door beleid
4.  Registreren van de claimprovider Microsoft-Account aan een gebruiker reis
5.  Uploaden van het beleid naar een Azure AD B2C-tenant en testen

## <a name="create-a-microsoft-account-application"></a>Een toepassing van Microsoft-account maken
Voor het gebruik van Microsoft-account als een id-provider in Azure Active Directory (Azure AD) B2C, moet u een toepassing van Microsoft-account maken en geeft deze met de juiste parameters. U moet een Microsoft-account. Als u niet hebt, gaat u naar [https://www.live.com/](https://www.live.com/).

1.  Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) en meld u aan met uw Microsoft-accountreferenties.
2.  Klik op **een app toevoegen**.

    ![Microsoft-account: een app toevoegen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  Bieden een **naam** voor uw toepassing **Neem contact op met de e-mailadres**, schakel het selectievakje **wij kunnen u helpen** en klik op **maken**.

    ![Microsoft-account - uw toepassing registreren](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  Kopieer de waarde van **toepassings-Id**. U moet het Microsoft-account configureren als een id-provider in uw tenant.

    ![Microsoft-account - exemplaar de waarde van toepassings-Id](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  Klik op **platform toevoegen**

    ![Microsoft-account - platform toevoegen](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  Kies in de lijst platform **Web**.

    ![Microsoft-account - Kies uit de lijst met platforms op Web](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  Voer `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in de **omleidings-URI's** veld. Vervang **{tenant}** met de naam van uw tenant (bijvoorbeeld contosob2c.onmicrosoft.com).

    ![Microsoft-account - Set Omleidings-URL 's](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  Klik op **nieuw wachtwoord genereren** onder de **toepassing geheimen** sectie. Kopieer het nieuwe wachtwoord op het scherm wordt weergegeven. U moet het Microsoft-account configureren als een id-provider in uw tenant. Dit wachtwoord is een belangrijke beveiligingsreferentie.

    ![Microsoft-account: nieuw wachtwoord genereren](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft-account - kopie van het nieuwe wachtwoord](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  Schakel het selectievakje waarin staat dat **ondersteuning voor Live SDK** onder de **geavanceerde opties** sectie. Klik op **Opslaan**.

    ![Microsoft-account - ondersteuning voor Live SDK](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a>De Microsoft-account Toepassingssleutel toevoegen aan Azure AD B2C
Federatie met Microsoft-accounts vereist een clientgeheim voor Microsoft-account aan Azure AD B2C-vertrouwensrelatie voor de toepassing is. U moet voor het opslaan van uw toepassing secert van Microsoft-account in Azure AD B2C-tenant:   

1.  Ga naar uw Azure AD B2C-tenant en selecteer **B2C-instellingen** > **identiteit ervaring Framework**
2.  Selecteer **beleid sleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.
3.  Klik op **+ toevoegen**.
4.  Voor **opties**, gebruik **handmatige**.
5.  Voor **naam**, gebruik `MSASecret`.  
    Het voorvoegsel `B2C_1A_` kunnen automatisch worden toegevoegd.
6.  In de **geheim** Voer uw Microsoft-toepassingsgeheim van https://apps.dev.microsoft.com
7.  Voor **sleutelgebruik**, gebruik **handtekening**.
8.  Klik op **Maken**.
9.  Controleer of u de sleutel hebt gemaakt `B2C_1A_MSASecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Toevoegen van een claimprovider in de uitbreiding beleid
Als u wilt dat gebruikers zich aanmelden met behulp van Microsoft-Account, moet u de Microsoft-Account te definiëren als een claimprovider. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceert opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren dat een specifieke gebruiker is geverifieerd.

Microsoft-Account definiëren als een claimprovider door toe te voegen `<ClaimsProvider>` knooppunt in het beleidsbestand extensie:

1.  Open het bestand met de extensie (TrustFrameworkExtensions.xml) van uw werkmap. Als u een XML-editor moet [Visual Studio Code probeert](https://code.visualstudio.com/download), een lichtgewicht platformoverschrijdende-editor.
2.  Zoek de `<ClaimsProviders>` sectie
3.  Plaats de volgende XML-fragment onder de `ClaimsProviders` element:

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

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Het Microsoft-Account claimprovider voor het ondertekenen van registreren of aanmelden gebruiker reis

Op dit moment de identiteitsprovider is ingesteld, maar het is niet beschikbaar is in geen van de schermen sign-up-to-date/aanmelden. Nu moet u de id-provider van Microsoft-Account toevoegen aan de gebruiker `SignUpOrSignIn` traject van de gebruiker. Als u deze beschikbaar, maken we een duplicaat van een bestaande sjabloon gebruiker reis.  Vervolgens wordt de id-provider van Microsoft-Account toevoegen:

> [!NOTE]
>
>Als u eerder hebt gekopieerd de `<UserJourneys>` element uit de base-bestand van uw beleid aan het extensiebestand `TrustFrameworkExtensions.xml`, kunt u in deze sectie overslaan.

1.  Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
2.  Zoek de `<UserJourneys>` element en kopieert u de volledige inhoud van `<UserJourneys>` knooppunt.
3.  Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en Ga naar de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
4.  Plak de volledige inhoud van `<UserJournesy>` knooppunt dat u hebt gekopieerd als een onderliggend element van de `<UserJourneys>` element.

### <a name="display-the-button"></a>De knop weergeven
De `<ClaimsProviderSelections>` element wordt de lijst met opties voor de selectie van claims provider en de volgorde gedefinieerd.  `<ClaimsProviderSelection>`element is vergelijkbaar met een knop identiteit provider op een pagina sign-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` element voor Microsoft-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1.  Zoek de `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"` in het traject gebruiker die u hebt gekopieerd.
2.  Zoek de `<OrchestrationStep>` knooppunt bevat`Order="1"`
3.  Plaats de volgende XML-fragment onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
Nu u een knop hebt geïmplementeerd, moet u deze koppelen aan een actie. Er is in dit geval de actie voor Azure AD B2C om te communiceren met Microsoft-Account geen token ontvangen. De knop koppelen aan een actie door koppelen aan het technische profiel voor de claimprovider van uw Microsoft-Account:

1.  Zoek de `<OrchestrationStep>` die omvat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Plaats de volgende XML-fragment onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * Zorg ervoor dat de `Id` heeft dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie
>   * Zorg ervoor dat `TechnicalProfileReferenceId` -ID is ingesteld in het technische profiel u eerdere (MSA OIDC) gemaakt.

## <a name="upload-the-policy-to-your-tenant"></a>Het beleid uploaden naar uw tenant
1.  In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2.  Selecteer **identiteit ervaring Framework**.
3.  Open de **alle beleidsregels** blade.
4.  Selecteer **uploaden beleid**.
5.  Controleer **het beleid overschreven als deze bestaat** vak.
6.  **Uploaden** TrustFrameworkExtensions.xml en zorg ervoor dat deze niet de validatie mislukt

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van nu uitvoeren

1.  Open **Azure AD B2C-instellingen** en Ga naar **identiteit ervaring Framework**.
> [!NOTE]
>
>**Nu uitvoeren** vereist ten minste één toepassing om te worden preregistered op de tenant. Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.
2.  Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid die u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet zich aanmelden met Microsoft-account.

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a>[Optioneel] Registreren van de claimprovider Microsoft-Account om te bewerken van profielen gebruiker reis
U kunt ook de id-provider van Microsoft-Account toevoegen aan de gebruiker `ProfileEdit` traject van de gebruiker. Als u deze beschikbaar, herhaalt u de laatste twee stappen:

### <a name="display-the-button"></a>De knop weergeven
1.  Open het extensiebestand van uw beleid (bijvoorbeeld TrustFrameworkExtensions.xml).
2.  Zoek de `<UserJourney>` knooppunt met `Id="ProfileEdit"` in het traject gebruiker die u hebt gekopieerd.
3.  Zoek de `<OrchestrationStep>` knooppunt bevat`Order="1"`
4.  Plaats de volgende XML-fragment onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
1.  Zoek de `<OrchestrationStep>` die omvat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Plaats de volgende XML-fragment onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Het aangepaste profiel bewerken beleid testen met behulp van nu uitvoeren
1.  Open **Azure AD B2C-instellingen** en Ga naar **identiteit ervaring Framework**.
2.  Open **B2C_1A_ProfileEdit**, de relying party (RP) aangepast-beleid die u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet zich aanmelden met Microsoft-account.

## <a name="download-the-complete-policy-files"></a>De volledige beleidsbestanden downloaden
Optioneel: We raden aan bouwen van uw scenario in plaats van deze voorbeeldbestanden met behulp van uw eigen aangepaste beleid bestanden na het voltooien van de aan de slag met aangepast beleid dat is doorlopen.  [Beleid voorbeeldbestanden voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
