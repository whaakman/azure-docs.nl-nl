---
title: 'Azure Active Directory B2C: Een Azure AD-provider toevoegen met behulp van aangepaste beleidsregels | Microsoft Docs'
description: Meer informatie over Azure Active Directory B2C aangepast beleid
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 31f0dfe5-1ad0-4a25-a53b-8acc71bcea72
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: 6c073d70debfdc3560405955d65fa9ccaa7d8b1f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a>Azure Active Directory B2C: Meld u aan met behulp van Azure AD-accounts

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel ziet u het inschakelen van aanmelden voor gebruikers van een specifieke organisatie Azure Active Directory (Azure AD) met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten

Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:

1. Maken van een Azure Active Directory B2C-tenant (Azure AD B2C).
2. Maken van een Azure AD B2C-toepassing.
3. Twee beleidsengine toepassingen wordt geregistreerd.
4. Instellen van sleutels.
5. Instellen van het starter pack.

## <a name="create-an-azure-ad-app"></a>Een Azure AD-app maken

Om in te schakelen aanmelden voor gebruikers met een specifiek Azure AD-organisatie, moet u een toepassing binnen de organisatie registreren Azure AD-tenant.

>[!NOTE]
> We gebruiken 'contoso.com' voor de organisatie Azure AD-tenant en 'fabrikamb2c.onmicrosoft.com' als de Azure AD B2C-tenant in de volgende instructies.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer uw account op de bovenste balk. Van de **Directory** Kies de organisatie waar u de toepassing (contoso.com) registreren voor Azure AD-tenant.
1. Selecteer **meer services** in het linkerdeelvenster, en zoek naar "App registraties."
1. Selecteer **registratie van de nieuwe toepassing**.
1. Voer een naam voor uw toepassing (bijvoorbeeld `Azure AD B2C App`).
1. Selecteer **Web-app / API** voor het toepassingstype.
1. Voor **aanmeldings-URL**, voer de volgende URL waar `yourtenant` wordt vervangen door de naam van uw Azure AD B2C-tenant (`fabrikamb2c.onmicrosoft.com`):

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Opslaan van de toepassings-ID.
1. Selecteer de zojuist gemaakte toepassing.
1. Onder de **instellingen** blade Selecteer **sleutels**.
1. Maak een nieuwe sleutel en sla het. U gebruikt deze in de stappen in de volgende sectie.

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a>De Azure AD-sleutel toevoegen aan Azure AD B2C

U moet de sleutel van de toepassing contoso.com opslaan in uw Azure AD B2C-tenant. Om dit te doen:
1. Ga naar uw Azure AD B2C-tenant en selecteer **B2C-instellingen** > **identiteit ervaring Framework** > **beleid sleutels**.
1. Selecteer **+ toevoegen**.
1. Selecteer of typ de volgende opties:
   * Selecteer **handmatige**.
   * Voor **naam**, kies een naam die overeenkomt met de naam van uw Azure AD-tenant (bijvoorbeeld `ContosoAppSecret`).  Het voorvoegsel `B2C_1A_` automatisch is toegevoegd aan de naam van uw sleutel.
   * Plak uw Toepassingssleutel in de **geheim** vak.
   * Selecteer **handtekening**.
1. Selecteer **Maken**.
1. Controleer of u de sleutel hebt gemaakt `B2C_1A_ContosoAppSecret`.


## <a name="add-a-claims-provider-in-your-base-policy"></a>Toevoegen van een claimprovider in de basis-beleid

Als u wilt dat gebruikers zich aanmelden met Azure AD, moet u Azure AD te definiëren als een claimprovider. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceren zullen opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren dat een specifieke gebruiker is geverifieerd. 

U kunt Azure AD definiëren als een claimprovider door Azure AD toe te voegen de `<ClaimsProvider>` knooppunt in het extensiebestand van het beleid:

1. Open het extensiebestand (TrustFrameworkExtensions.xml) van uw werkmap.
1. Zoek de `<ClaimsProviders>` sectie. Als deze niet bestaat, kunt u deze toevoegen onder het hoofdknooppunt.
1. Voeg een nieuwe `<ClaimsProvider>` knooppunt als volgt:

    ```XML
    <ClaimsProvider>
        <Domain>Contoso</Domain>
        <DisplayName>Login using Contoso</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="ContosoProfile">
                <DisplayName>Contoso Employee</DisplayName>
                <Description>Login with your Contoso account</Description>
                <Protocol Name="OpenIdConnect"/>
                <OutputTokenFormat>JWT</OutputTokenFormat>
                <Metadata>
                    <Item Key="METADATA">https://login.windows.net/contoso.com/.well-known/openid-configuration</Item>
                    <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
                    <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="response_types">id_token</Item>
                    <Item Key="UsePolicyInRedirectUri">false</Item>
                </Metadata>
                <CryptographicKeys>
                    <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
                </CryptographicKeys>
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
                    <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
                    <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
                    <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
                    <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
                    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="contosoAuthentication" />
                    <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
                </OutputClaims>
                <OutputClaimsTransformations>
                    <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
                    <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
                    <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
                    <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
                </OutputClaimsTransformations>
                <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
            </TechnicalProfile>
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

1. Onder de `<ClaimsProvider>` knooppunt, werk de waarde voor `<Domain>` in een unieke waarde die kan worden gebruikt om deze te onderscheiden van andere id-providers.
1. Onder de `<ClaimsProvider>` knooppunt, werk de waarde voor `<DisplayName>` naar een beschrijvende naam voor de claimprovider. Deze waarde wordt momenteel niet gebruikt.

### <a name="update-the-technical-profile"></a>Het technische profiel bijwerken

Als u een token van het eindpunt van de Azure AD, moet u de protocollen die Azure AD B2C gebruiken moet voor communicatie met Azure AD te definiëren. Dit wordt gedaan binnen de `<TechnicalProfile>` element van `<ClaimsProvider>`.
1. Bijwerken van de ID van de `<TechnicalProfile>` knooppunt. Deze ID wordt gebruikt om te verwijzen naar dit technische profiel van andere onderdelen van het beleid.
1. Werk de waarde voor `<DisplayName>`. Deze waarde wordt weergegeven op de knop aanmelden op uw scherm aanmelden.
1. Werk de waarde voor `<Description>`.
1. Azure AD het OpenID Connect-protocol gebruikt, dus zorg ervoor dat de waarde voor `<Protocol>` is `"OpenIdConnect"`.

Werk de `<Metadata>` sectie in het XML-bestand waarnaar wordt verwezen eerder in overeenstemming met de configuratie-instellingen voor uw specifieke Azure AD-tenant. In het XML-bestand waarden van de metagegevens als volgt te werk:

1. Stel `<Item Key="METADATA">` naar `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, waarbij `yourAzureADtenant` is de naam van uw Azure AD-tenant (contoso.com).
1. Open uw browser en Ga naar de `METADATA` URL die u zojuist hebt bijgewerkt.
1. In de browser, zoekt u naar het object 'verlener' en kopieer de waarde ervan. Deze ziet er als volgt: `https://sts.windows.net/{tenantId}/`.
1. Plak de waarde voor `<Item Key="ProviderName">` in het XML-bestand.
1. Stel `<Item Key="client_id">` naar de toepassings-ID van de registratie van de app.
1. Stel `<Item Key="IdTokenAudience">` naar de toepassings-ID van de registratie van de app.
1. Zorg ervoor dat `<Item Key="response_types">` is ingesteld op `id_token`.
1. Zorg ervoor dat `<Item Key="UsePolicyInRedirectUri">` is ingesteld op `false`.

U moet ook koppelen van het Azure AD-geheim dat u in uw Azure AD B2C-tenant met Azure AD geregistreerd `<ClaimsProvider>` informatie:

* In de `<CryptographicKeys>` in de XML-bestand in de voorgaande sectie, werk de waarde voor `StorageReferenceId` naar de verwijzings-ID van het geheim die u hebt gedefinieerd (bijvoorbeeld `ContosoAppSecret`).

### <a name="upload-the-extension-file-for-verification"></a>Upload het extensiebestand voor verificatie

U hebt nu uw beleid geconfigureerd zodat Azure AD B2C weet hoe om te communiceren met uw Azure AD-directory. Upload het extensiebestand van uw beleid om te bevestigen dat er geen problemen die tot nu toe. Dit doet u als volgt:

1. Open de **alle beleidsregels** blade in uw Azure AD B2C-tenant.
1. Controleer de **het beleid overschreven als deze bestaat** vak.
1. Upload het extensiebestand (TrustFrameworkExtensions.xml) en zorg ervoor dat deze niet de validatie mislukt.

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a>Registreren van de claimprovider Azure AD voor een gebruiker reis

Nu moet u de id-provider voor Azure AD toevoegt aan een van de gebruiker trajecten. Op dit moment de identiteitsprovider is ingesteld, maar het is niet beschikbaar is in geen van de schermen sign-up-to-date/aanmelden. Als u deze beschikbaar, we een duplicaat van een bestaande sjabloon gebruiker reis maken en wijzigen zodat er ook de Azure AD-id-provider:

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
1. Zoek de `<UserJourneys>` element en kopieer de gehele `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"`.
1. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en Ga naar de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
1. Plak de gehele `<UserJourney>` knooppunt dat u hebt gekopieerd als een onderliggend element van de `<UserJourneys>` element.
1. Wijzig de naam van de ID van de nieuwe gebruiker reis (bijvoorbeeld wijzigen van de naam `SignUpOrSignUsingContoso`).

### <a name="display-the-button"></a>'De knop' weergeven

De `<ClaimsProviderSelection>` element is vergelijkbaar met een knop identiteit provider op een scherm sign-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` element voor Azure AD, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1. Zoek de `<OrchestrationStep>` knooppunt met `Order="1"` in het traject gebruiker die u zojuist hebt gemaakt.
1. Voeg het volgende toe:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Stel `TargetClaimsExchangeId` naar een geschikte waarde. Het is raadzaam na de dezelfde overeenkomst als anderen:  *\[ClaimProviderName\]Exchange*.

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu u een knop hebt geïmplementeerd, moet u deze koppelen aan een actie. Er is in dit geval de actie voor Azure AD B2C om te communiceren met Azure AD geen token ontvangen. De knop koppelen aan een actie door koppelen aan het technische profiel voor uw Azure AD-claimprovider:

1. Zoek de `<OrchestrationStep>` die omvat `Order="2"` in de `<UserJourney>` knooppunt.
1. Voeg het volgende toe:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Update `Id` op dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie.
1. Update `TechnicalProfileReferenceId` naar de ID van het profiel voor technische u eerdere (ContosoProfile) gemaakt.

### <a name="upload-the-updated-extension-file"></a>Het bijgewerkte extensiebestand uploaden

U klaar bent met wijzigen van het extensiebestand. Sla het op. Upload het bestand vervolgens en zorg ervoor dat alle validaties slaagt.

### <a name="update-the-rp-file"></a>Het RP-bestand bijwerken

U moet nu bijwerken van de relying party (RP)-bestand dat wordt nu het traject gebruiker die u zojuist hebt gemaakt:

1. Maak een kopie van SignUpOrSignIn.xml in uw werkmap en de naam (bijvoorbeeld: Wijzig deze in SignUpOrSignInWithAAD.xml).
1. Open het nieuwe bestand en update de `PolicyId` kenmerk voor `<TrustFrameworkPolicy>` met een unieke waarde (bijvoorbeeld SignUpOrSignInWithAAD). <br> Dit is de naam van uw beleid (bijvoorbeeld B2C\_1A\_SignUpOrSignInWithAAD).
1. Wijzig de `ReferenceId` kenmerk in `<DefaultUserJourney>` overeenkomen met de ID van de nieuwe gebruiker reis die u hebt gemaakt (SignUpOrSignUsingContoso).
1. Sla uw wijzigingen op en upload het bestand.

## <a name="troubleshooting"></a>Problemen oplossen

Testen van het aangepaste beleid dat u zojuist hebt geüpload door de blade te openen en te klikken **nu uitvoeren**. Meer informatie over om problemen te onderzoeken, [probleemoplossing](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Volgende stappen

Feedback geven aan [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
