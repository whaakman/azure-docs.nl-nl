---
title: 'Azure Active Directory B2C: Twitter toevoegen als een OAuth1 id-provider met behulp van aangepaste beleid'
description: Twitter gebruiken als een id-provider met behulp van het protocol OAuth1
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
ms.date: 10/23/2017
ms.author: yoelh
ms.openlocfilehash: 629e0bbaa7c62ef5d381085588c6a99c203c41cb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-add-twitter-as-an-oauth1-identity-provider-by-using-custom-policies"></a>Azure Active Directory B2C: Twitter toevoegen als een OAuth1 id-provider met behulp van aangepaste beleid
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel laat zien hoe u aanmelden voor gebruikers van een Twitter-account inschakelen met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten
Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

## <a name="step-1-create-a-twitter-account-application"></a>Stap 1: Een toepassing Twitter-account maken
Twitter gebruiken als een id-provider in Azure Active Directory B2C (Azure AD B2C) heeft, moet u een Twitter-toepassing maken en deze met de juiste parameters opgeven. U kunt een Twitter-toepassing registreren door te gaan naar de [Twitter-aanmeldingspagina](https://twitter.com/signup).

1. Ga naar de [Twitter ontwikkelaars](https://apps.twitter.com/) website, meld u aan met de referenties van uw Twitter-account en selecteer vervolgens **nieuwe App maken**.

    ![Account Twitter - nieuwe app maken](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app1.png)

2. In de **maken van een toepassing** venster de volgende handelingen uit:
 
    a. Typ de **naam** en een **beschrijving** voor uw nieuwe app. 

    b. In de **Website** vak, plak **https://login.microsoftonline.com**. 

    c. In de **retouraanroep URL** vak, plak **https://login.microsoftonline.com/te/ {tenant}.onmicrosoft.com/oauth2/authresp**. Vervang {*tenant*} met de tenantnaam van uw (bijvoorbeeld contosob2c.onmicrosoft.com). Zorg ervoor dat u van het HTTPS-schema gebruikmaakt. 

    d. Aan de onderkant van de pagina lees en accepteer de voorwaarden en selecteer vervolgens **uw Twitter-toepassing maken**.

    ![Twitter-account: een nieuwe app toevoegen](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app2.png)

3. In de **B2C demo** Selecteer **instellingen**, selecteer de **toe dat deze toepassing worden gebruikt voor het aanmelden met Twitter** selectievakje en selecteer vervolgens **Update Instellingen**.

4. Selecteer **sleutels en toegangstokens**, en noteer de **Consumer-sleutel (API-sleutel)** en **consumentgeheim (API geheim)** waarden.

    ![Twitter-account - eigenschappen voor toepassingen instellen](media/active-directory-b2c-custom-setup-twitter-idp/adb2c-ief-setup-twitter-idp-new-app3.png)

    >[!NOTE]
    >De consument geheime sleutel is een belangrijke beveiligingsreferentie. Geen dit geheim met anderen delen of deze met uw app te distribueren.

## <a name="step-2-add-your-twitter-account-application-key-to-azure-ad-b2c"></a>Stap 2: De sleutel van uw toepassing Twitter toevoegen aan Azure AD B2C
Federatie met Twitter accounts vereist een consumentgeheim voor de Twitter-account aan Azure AD B2C-vertrouwensrelatie voor de toepassing is. Voor het opslaan van de toepassing Twitter consumentgeheim in uw Azure AD B2C-tenant, het volgende doen: 

1. Selecteer in uw Azure AD B2C-tenant, **B2C-instellingen** > **identiteit ervaring Framework**.

2. Als u wilt weergeven van de sleutels die beschikbaar in uw tenant zijn, selecteer **beleid sleutels**.

3. Selecteer **Toevoegen**.

4. In de **opties** de optie **handmatige**.

5. In de **naam** de optie **TwitterSecret**.  
    Het voorvoegsel *B2C_1A_* kunnen automatisch worden toegevoegd.

6. In de **geheim** Voer uw Microsoft-toepassingsgeheim van de [Registratieportal toepassing](https://apps.dev.microsoft.com).

7. Voor **sleutelgebruik**, gebruik **versleuteling**.

8. Selecteer **Maken**.

9. Controleer of u hebt gemaakt de `B2C_1A_TwitterSecret` sleutel.

## <a name="step-3-add-a-claims-provider-in-your-extension-policy"></a>Stap 3: Een claimprovider in de uitbreiding beleid toevoegen

Als u wilt dat gebruikers zich aanmelden via Twitter-account, moet u Twitter definiëren als een claimprovider. Met andere woorden, moet u de eindpunten die Azure AD B2C met communiceert opgeven. De eindpunten bieden een set claims die worden gebruikt door Azure AD B2C om te controleren dat een specifieke gebruiker is geverifieerd.

Twitter definiëren als een claimprovider door toe te voegen `<ClaimsProvider>` knooppunt in het beleidsbestand extensie:

1. Open in uw werkmap, de *TrustFrameworkExtensions.xml* extensiebestand beleid. 

2. Zoeken naar de `<ClaimsProviders>` sectie.

3. In de `<ClaimsProviders>` knooppunt, voeg de volgende XML-fragment:  

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

4. Vervang de *client_id*'-waarde met uw sleutel Twitter-account van toepassing consumer.

5. Sla het bestand op.

## <a name="step-4-register-the-twitter-account-claims-provider-to-your-sign-up-or-sign-in-user-journey"></a>Stap 4: De claimprovider Twitter-account aan uw reis registreren of aanmelden gebruiker registreren
U kunt de id-provider hebt ingesteld. Echter, het is nog niet beschikbaar in een van de vensters registreren of aanmelden. Nu u de id-provider van de Twitter-account aan uw gebruikers toevoegen moet `SignUpOrSignIn` traject van de gebruiker.

### <a name="step-41-make-a-copy-of-the-user-journey"></a>Stap 4.1: Maak een kopie van het traject van gebruiker
Als u het traject gebruiker beschikbaar, een duplicaat van een bestaande gebruiker reis sjabloon maken en vervolgens de Twitter-id-provider toevoegen:

>[!NOTE]
>Als u hebt gekopieerd de `<UserJourneys>` element uit de base-bestand van uw beleid aan de *TrustFrameworkExtensions.xml* extensiebestand is, kunt u doorgaan met de volgende sectie.

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).

2. Zoeken naar de `<UserJourneys>` element, selecteert u de volledige inhoud van de `<UserJourney>` knooppunt en selecteer vervolgens **Knippen** verplaatsen van de geselecteerde tekst naar het Klembord.

3. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en zoek vervolgens naar de `<UserJourneys>` element. Als het element niet bestaat, moet u deze toevoegen.

4. Plak de volledige inhoud van de `<UserJourney>` knooppunt, dat u naar het Klembord in stap 2 hebt verplaatst naar de `<UserJourneys>` element.

### <a name="step-42-display-the-button"></a>Stap 4.2: 'De knop' weergeven
De `<ClaimsProviderSelections>` element wordt de lijst met opties voor de selectie van claims provider en de volgorde gedefinieerd. De `<ClaimsProviderSelection>` knooppunt is vergelijkbaar met een knop identiteit provider op een pagina registreren of aanmelden. Als u een `<ClaimsProviderSelection>` knooppunt voor een Twitter-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Als u wilt toevoegen van dit element, het volgende doen:

1. Zoeken naar de `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"` in het traject gebruiker die u hebt gekopieerd.

2. Zoek de `<OrchestrationStep>` knooppunt met `Order="1"`.

3. In de `<ClaimsProviderSelections>` element, het volgende XML-fragment toevoegen:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
    ```

### <a name="step-43-link-the-button-to-an-action"></a>Stap 4.3: De knop koppelen aan een actie
Nu dat u een knop geïmplementeerd hebt, moet u deze koppelen aan een actie. Er is in dit geval de actie voor Azure AD B2C om te communiceren met de Twitter-account geen token ontvangen. De knop koppelen aan een actie door koppelen aan het technische profiel voor de claimprovider van uw Twitter-account:

1. Zoeken naar de `<OrchestrationStep>` knooppunt met `Order="2"` in de `<UserJourney>` knooppunt.
2. In de `<ClaimsExchanges>` element, het volgende XML-fragment toevoegen:

    ```xml
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAUTH1" />
    ```

    >[!NOTE]
    >* Zorg ervoor dat `Id` heeft dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie.
    >* Zorg ervoor dat de `TechnicalProfileReferenceId` ID wordt ingesteld op het technische profiel dat u eerder (Twitter-OAUTH1) gemaakt.

## <a name="step-5-upload-the-policy-to-your-tenant"></a>Stap 5: Het beleid uploaden naar uw tenant
1. In de [Azure-portal](https://portal.azure.com), overschakelen naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en selecteer vervolgens **Azure AD B2C**.

2. Selecteer **identiteit ervaring Framework**.

3. Selecteer **alle beleidsregels**.

4. Selecteer **uploaden beleid**.

5. Selecteer de **het beleid overschreven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkBase.xml* en *TrustFrameworkExtensions.xml* -bestanden, en ervoor te zorgen dat ze gevalideerd worden.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>Stap 6: Het aangepaste beleid testen met behulp van nu uitvoeren

1. Selecteer **Azure AD B2C-instellingen**, en selecteer vervolgens **identiteit ervaring Framework**.

    >[!NOTE]
    >Uitvoeren is nu vereist ten minste één toepassing om te worden preregistered op de tenant. Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid die u hebt geüpload en selecteer vervolgens **nu uitvoeren**.  
    Nu moet u zich aanmelden via Twitter-account.

## <a name="step-7-optional-register-the-twitter-account-claims-provider-to-the-profile-edit-user-journey"></a>Stap 7: (Optioneel) registreren Twitter-account claimprovider naar het traject van de gebruiker profiel bewerken
U kunt ook om toe te voegen van de identiteitsprovider Twitter-account aan uw `ProfileEdit` traject van de gebruiker. De gebruiker op reis beschikbaar, herhaalt 'stap 4.' maken Selecteer deze keer de `<UserJourney>` knooppunt met `Id="ProfileEdit"`. Opslaan, uploaden en testen van uw beleid.


## <a name="optional-download-the-complete-policy-files"></a>(Optioneel) De volledige beleidsbestanden downloaden
Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario maken met behulp van uw eigen aangepaste beleidsbestanden. We hebben opgegeven voor eigen referentie [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-twitter-app).
