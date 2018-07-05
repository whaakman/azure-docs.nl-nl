---
title: Een Azure AD-provider toevoegen met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over Azure Active Directory B2C aangepast beleid.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 139658b57cfdce2603d4413b5bf4c9a86b6a8c14
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444324"
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a>Azure Active Directory B2C: Meld u aan met behulp van Azure AD-accounts

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel laat u het inschakelen van aanmelding voor gebruikers van de organisatie van een specifieke Azure Active Directory (Azure AD) met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten

Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:

1. Het maken van een Azure Active Directory B2C-tenant (Azure AD B2C).
2. Het maken van een Azure AD B2C-toepassing.
3. Twee beleidsengine toepassingen registreren.
4. Instellen van sleutels.
5. Instellen van het starter-pack.

## <a name="create-an-azure-ad-app"></a>Een Azure AD-app maken

Om in te schakelen aanmelding voor gebruikers van een specifieke Azure AD-organisatie, moet u een toepassing registreren met de organisatie Azure AD-tenant.

>[!NOTE]
> We gebruiken 'contoso.com' voor de organisatie Azure AD-tenant en 'fabrikamb2c.onmicrosoft.com' als de Azure AD B2C-tenant in de volgende instructies.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer uw account op de bovenste balk. Uit de **Directory** Kies de organisatie Azure AD-tenant waar u het registreren van uw toepassing (contoso.com).
3. Selecteer **meer services** in het linkerdeelvenster en zoek naar 'App-registraties'.
4. Selecteer **Nieuwe toepassing registreren**.
5. Voer een naam in voor uw toepassing (bijvoorbeeld `Azure AD B2C App`).
6. Selecteer **Web-app / API** als toepassingstype.
7. Voor **aanmeldings-URL**, voer de volgende URL, waarbij `yourtenant` wordt vervangen door de naam van uw Azure AD B2C-tenant (`fabrikamb2c.onmicrosoft.com`):

    >[!NOTE]
    >De waarde voor 'yourtenant' moet alleen kleine letters bevatten in de **aanmeldings-URL**.

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

8. Opslaan van de toepassings-ID.
9. Selecteer de zojuist gemaakte toepassing.
10. Onder de **instellingen** Selecteer **sleutels**.
11. Geef de beschrijving van de sleutel, selecteer een tijdsduur en klik vervolgens op **opslaan**. De waarde van de sleutel wordt weergegeven. Kopieer deze gebruikt u dit in de stappen in de volgende sectie.

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a>De Azure AD-sleutel toevoegen aan Azure AD B2C

U moet voor het opslaan van de Toepassingssleutel contoso.com in uw Azure AD B2C-tenant. Om dit te doen:
1. Ga naar uw Azure AD B2C-tenant, en selecteer **B2C-instellingen** > **Identity-Ervaringsframework** > **Beleidssleutels**.
1. Selecteer **+ toevoegen**.
1. Selecteer of voer deze opties:
   * Selecteer **handmatig**.
   * Voor **naam**, kies een naam die overeenkomt met de naam van uw Azure AD-tenant (bijvoorbeeld `ContosoAppSecret`).  Het voorvoegsel `B2C_1A_` wordt automatisch toegevoegd aan de naam van uw sleutel.
   * Plak de Toepassingssleutel van uw in de **geheim** vak.
   * Selecteer **handtekening**.
1. Selecteer **Maken**.
1. Bevestig dat u de sleutel hebt `B2C_1A_ContosoAppSecret`.


## <a name="add-a-claims-provider-in-your-base-policy"></a>Toevoegen van een claimprovider in het Basisbeleid

Als u wilt dat gebruikers zich aanmelden met behulp van Azure AD, moet u Azure AD als een claimprovider definiëren. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceren zullen opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd. 

U kunt Azure AD als een claimprovider definiëren door toe te voegen van Azure AD de `<ClaimsProvider>` knooppunt in het bestand uitbreiding van uw beleid:

1. Open het extensiebestand (TrustFrameworkExtensions.xml) van uw werkmap.
1. Zoek de `<ClaimsProviders>` sectie. Als deze niet bestaat, kunt u deze onder het hoofdknooppunt toevoegen.
1. Toevoegen van een nieuwe `<ClaimsProvider>` knooppunt als volgt te werk:

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

1. Onder de `<ClaimsProvider>` knooppunt, werk de waarde voor `<Domain>` op een unieke waarde die kan worden gebruikt om u te onderscheiden van andere id-providers.
1. Onder de `<ClaimsProvider>` knooppunt, werk de waarde voor `<DisplayName>` naar een beschrijvende naam voor de claimprovider. Deze waarde wordt momenteel niet gebruikt.

### <a name="update-the-technical-profile"></a>Het technische profiel bijwerken

Als u een token van het Azure AD-eindpunt, moet u voor het definiëren van de protocollen die voor Azure AD B2C wordt gebruikt om te communiceren met Azure AD. Dit wordt gedaan binnen het `<TechnicalProfile>` element van `<ClaimsProvider>`.
1. Bijwerken van de ID van de `<TechnicalProfile>` knooppunt. Deze ID wordt gebruikt om te verwijzen naar dit technisch profiel van andere onderdelen van het beleid.
1. Werk de waarde voor `<DisplayName>`. Deze waarde wordt weergegeven op de knop aanmelden op het aanmeldingsscherm.
1. Werk de waarde voor `<Description>`.
1. Azure AD maakt gebruik van de OpenID Connect-protocol, dus zorg ervoor dat de waarde voor `<Protocol>` is `"OpenIdConnect"`.

U moet bijwerken de `<Metadata>` sectie in het XML-bestand waarnaar wordt verwezen naar eerdere in overeenstemming met de configuratie-instellingen voor uw specifieke Azure AD-tenant. In het XML-bestand, moet u de metagegevenswaarden als volgt bijwerken:

1. Stel `<Item Key="METADATA">` naar `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, waarbij `yourAzureADtenant` is de naam van uw Azure AD-tenant (contoso.com).
1. Open uw browser en Ga naar de `METADATA` URL die u zojuist hebt bijgewerkt.
1. In de browser bekijken voor het object 'verlener' en kopieer de waarde ervan. Het moet er als volgt: `https://sts.windows.net/{tenantId}/`.
1. Plak de waarde voor `<Item Key="ProviderName">` in het XML-bestand.
1. Stel `<Item Key="client_id">` op de toepassings-ID van de app-registratie.
1. Stel `<Item Key="IdTokenAudience">` op de toepassings-ID van de app-registratie.
1. Zorg ervoor dat `<Item Key="response_types">` is ingesteld op `id_token`.
1. Zorg ervoor dat `<Item Key="UsePolicyInRedirectUri">` is ingesteld op `false`.

U moet ook de Azure AD-geheime sleutel die u hebt geregistreerd in uw Azure AD B2C-tenant om de Azure AD een koppeling `<ClaimsProvider>` informatie:

* In de `<CryptographicKeys>` sectie in het voorgaande XML-bestand, werk de waarde voor `StorageReferenceId` naar de verwijzings-ID van de geheime sleutel die u hebt opgegeven (bijvoorbeeld `ContosoAppSecret`).

### <a name="upload-the-extension-file-for-verification"></a>Upload het extensiebestand voor verificatie

U hebt nu uw beleid geconfigureerd zodat Azure AD B2C weet hoe om te communiceren met uw Azure AD-directory. Upload het bestand uitbreiding van uw beleid om te bevestigen dat er geen problemen met tot nu toe. Dit doet u als volgt:

1. Open de **alle beleidsregels** blade in uw Azure AD B2C-tenant.
1. Controleer de **het beleid overschrijven als deze bestaat** vak.
1. Upload het extensiebestand (TrustFrameworkExtensions.xml) en zorg ervoor dat deze niet de validatie mislukt.

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a>Registreren van de Azure AD-claims-provider op een gebruikersbeleving

Nu moet u de id-provider voor Azure AD toevoegen aan een van uw gebruiker reizen. Op dit moment wordt de id-provider is ingesteld, maar het is niet beschikbaar zijn in elk van de schermen aanmelden-up-to-date/aanmelden. Om het beschikbaar maken, we een duplicaat van een bestaande sjabloon voor de gebruikersbeleving maken en wijzigen zodat het bevat ook de Azure AD-id-provider:

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
1. Zoek de `<UserJourneys>` -element en kopieer de gehele `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"`.
1. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en zoek de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
1. Plak de gehele `<UserJourney>` knooppunt dat u hebt gekopieerd als onderliggende site van de `<UserJourneys>` element.
1. Wijzig de naam van de ID van de nieuwe gebruikersbeleving (bijvoorbeeld als naam `SignUpOrSignUsingContoso`).

### <a name="display-the-button"></a>De 'knop' weergeven

De `<ClaimsProviderSelection>` element is vergelijkbaar met een knop identity provider op een scherm aanmelden-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` -element voor Azure AD, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1. Zoek de `<OrchestrationStep>` knooppunt met `Order="1"` in de gebruikersbeleving die u zojuist hebt gemaakt.
1. Voeg het volgende toe:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Stel `TargetClaimsExchangeId` naar een geschikte waarde. We raden aan volgen dezelfde conventie als anderen:  *\[ClaimProviderName\]Exchange*.

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met Azure AD voor het ontvangen van een token. De knop koppelen aan een actie door koppelen aan het technische profiel voor uw Azure AD-claims-provider:

1. Zoek de `<OrchestrationStep>` die bevat `Order="2"` in de `<UserJourney>` knooppunt.
1. Voeg het volgende toe:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Update `Id` op dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie.
1. Update `TechnicalProfileReferenceId` de ID van het technische profiel u eerdere (ContosoProfile) gemaakt.

### <a name="upload-the-updated-extension-file"></a>De bijgewerkte extensiebestand uploaden

U klaar bent met de extensiebestand wijzigt. Sla het op. Vervolgens het bestand uploaden en ervoor te zorgen dat alle validaties slagen.

### <a name="update-the-rp-file"></a>Het RP-bestand bijwerken

Nu moet u bijwerken van de relying party (RP)-bestand dat wordt nu de gebruikersbeleving die u zojuist hebt gemaakt:

1. Maak een kopie van SignUpOrSignIn.xml in uw werkmap en wijzig de naam (bijvoorbeeld, wijzig de SignUpOrSignInWithAAD.xml).
1. Open het nieuwe bestand en update de `PolicyId` voor het kenmerk `<TrustFrameworkPolicy>` met een unieke waarde (bijvoorbeeld SignUpOrSignInWithAAD). <br> Dit is de naam van uw beleid (bijvoorbeeld B2C\_1A\_SignUpOrSignInWithAAD).
1. Wijzig de `ReferenceId` kenmerk in `<DefaultUserJourney>` zodat deze overeenkomen met de ID van de nieuwe gebruikersbeleving die u hebt gemaakt (SignUpOrSignUsingContoso).
1. Sla uw wijzigingen op en upload het bestand.

## <a name="troubleshooting"></a>Problemen oplossen

Het aangepaste beleid dat u net hebt geüpload door te klikken en openen van de blade test **nu uitvoeren**. Als u wilt vaststellen, meer informatie over [probleemoplossing](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Volgende stappen

Feedback geven aan [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
