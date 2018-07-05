---
title: AD FS toevoegen als een SAML-id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Een artikel met instructies over het instellen van AD FS 2016 met behulp van SAML-protocol en aangepaste beleidsregels
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: a81baae553bbf9c58d42372e25e90cd7588f2952
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445072"
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: AD FS toevoegen als een SAML-id-provider met behulp van aangepaste beleidsregels

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelding voor gebruikers van AD FS-account via het gebruik van inschakelen [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten

Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:

1.  Het maken van een AD FS Relying Party-vertrouwensrelatie.
2.  Het certificaat ADFS Relying Party Trust toevoegen aan Azure AD B2C.
3.  Claimprovider toevoegen aan een beleid.
4.  Registreren van de AD FS claimprovider account naar een gebruikersbeleving.
5.  Het uploaden van het beleid naar een Azure AD B2C-tenant en het testen.

## <a name="to-create-a-claims-aware-relying-party-trust"></a>Een claimbewuste Relying Party Trust maken

Voor het gebruik van ADFS als id-provider in Azure Active Directory (Azure AD) B2C, moet u een AD FS Relying Party Trust maken en geven met de juiste parameters.

Als u wilt toevoegen van een nieuwe relying party trust met behulp van de AD FS-Management-module en de instellingen handmatig configureren, moet u de volgende procedure uitvoeren op een federatieserver.

Lidmaatschap van **beheerders**, of gelijkwaardig, op de lokale computer is de minimale vereiste om deze procedure te voltooien. Lees de informatie over het gebruik van de juiste accounts en groepslidmaatschappen op [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477)

1.  Klik in Serverbeheer op **extra**, en selecteer vervolgens **AD FS Management**.

2.  Klik op **toevoegen vertrouwensrelatie van Relying Party**.
    ![Vertrouwensrelatie van Relying Party toevoegen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)

3.  Op de **Welkom** pagina, kies **Claims bewuste** en klikt u op **Start**.
    ![Kies op de pagina Welkom Claims op de hoogte](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)
4.  Op de **gegevensbron selecteren** pagina, klikt u op **gegevens over de relying party handmatig invoeren**, en klik vervolgens op **volgende**.
    ![Gegevens over de relying party invoeren](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)

5.  Op de **weergavenaam opgeven** pagina, typ een naam in **weergavenaam**onder **opmerkingen bij de** Typ een beschrijving voor deze relying partyvertrouwensrelatie, en klik vervolgens op **volgende** .
    ![Geef de weergavenaam en opmerkingen bij de](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)
6.  Optioneel. Als u een optionele token versleutelingscertificaat, klikt u vervolgens op de **certificaat configureren** pagina, klikt u op **Bladeren** Zoek uw certificaatbestand en klik vervolgens op **volgende**.
    ![Certificaat configureren](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)
7.  Op de **URL configureren** weergeeft, schakelt de **inschakelen van ondersteuning voor het protocol SAML 2.0 WebSSO** selectievakje. Onder **Relying party SAML 2.0 SSO service URL**, typt u de Security Assertion Markup Language (SAML) service-eindpunt-URL voor deze relying partyvertrouwensrelatie, en klik vervolgens op **volgende**.  Voor de **Relying party SAML 2.0 SSO service URL**, plak de `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`. Vervang {tenant} met de naam van uw tenant (bijvoorbeeld: contosob2c.onmicrosoft.com) en de {beleid} vervangen door de naam van uw extensies beleid (bijvoorbeeld B2C_1A_TrustFrameworkExtensions).
    > [!IMPORTANT]
    >Naam van het beleid wordt dat beleid signup_or_signin neemt over van, in dit geval is: `B2C_1A_TrustFrameworkExtensions`.
    >Bijvoorbeeld de URL is: https://login.microsoftonline.com/te/ **contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.

    ![Relying party SAML 2.0 SSO service URL](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. Op de **id's configureren** pagina dezelfde URL opgeven als de vorige stap, klikt u op **toevoegen** toevoegen aan de lijst en klik vervolgens op **volgende**.
    ![Relying party trust-id 's](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)
9.  Op de **kiezen toegangscontrolebeleid** selecteert u een beleid en klikt u op **volgende**.
    ![Beleid voor toegangsbeheer kiezen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)
10.  Op de **gereed voor toevoegen vertrouwensrelatie** pagina, Controleer de instellingen en klik vervolgens op **volgende** om op te slaan, uw relying party trust informatie.
    ![Sla uw relying party trust informatie](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)
11.  Op de **voltooien** pagina, klikt u op **sluiten**, deze actie wordt automatisch de **Claimregels bewerken** in het dialoogvenster.
    ![Claimregels bewerken](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)
12. Klik op **regel toevoegen**.  
      ![Nieuwe regel toevoegen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)
13.  In **claimregelsjabloon**, selecteer **verzenden LDAP-kenmerken als claims**.
    ![Verzenden van LDAP-kenmerken als claims sjabloonregel selecteren](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)
14.  Geef **naam van Claimregel**. Voor de **kenmerkopslag** Selecteer **Active Directory selecteren** voegt u de volgende claims en klik vervolgens op **voltooien** en **OK**.
    ![Eigenschappen van de regel instellen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)
15.  Selecteer in Serverbeheer **Relying Party-vertrouwensrelaties** en selecteer de relying party vertrouwt u hebt gemaakt en klik vervolgens **eigenschappen**.
    ![Eigenschappen van relying party bewerken](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)
16.  Het eigenschappenvenster van relying party trust (B2C-Demo) klikt u op één **handtekening** tabblad en klik op **toevoegen**.  
    ![Set-handtekening](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)
17.  Uw handtekeningcertificaat (.cert-bestand, zonder persoonlijke sleutel) toevoegen.  
    ![Uw handtekeningcertificaat toevoegen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  Klik in het eigenschappenvenster van relying party trust (B2C-Demo) op **Geavanceerd** tabblad en wijzig de **veilige hash-algoritme** naar **SHA-1**, klikt u op **Ok**.  
    ![Instellen van veilige hash-algoritme SHA-1](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>De sleutel van het ADFS-toepassing toevoegen aan Azure AD B2C
Federatie met AD FS-accounts vereist een clientgeheim voor AD FS-account aan Azure AD B2C-vertrouwensrelatie namens de toepassing. U moet uw AD FS-certificaat wordt opgeslagen in uw Azure AD B2C-tenant. 

1.  Ga naar uw Azure AD B2C-tenant, en selecteer **B2C-instellingen** > **Identity-Ervaringsframework**
2.  Selecteer **Beleidssleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.
3.  Klik op **+ toevoegen**.
4.  Voor **opties**, gebruikt u **uploaden**.
5.  Voor **naam**, gebruikt u `ADFSSamlCert`.  
    Het voorvoegsel `B2C_1A_` mogelijk automatisch worden toegevoegd.
6.  In het bestand te uploaden, ** selecteert u het PFX-certificaatbestand met persoonlijke sleutel. Opmerking: dit certificaat (met de persoonlijke sleutel) moet hetzelfde account dat is uitgegeven en gebruikt voor de relying party van AD FS zijn.
![Sleutel voor het uploaden](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)
7.  Klik op **Maken**.
8.  Bevestig dat u de sleutel hebt `B2C_1A_ADFSSamlCert`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Toevoegen van een claimprovider in uw extensie-beleid
Als u wilt dat gebruikers zich aanmelden met behulp van AD FS-account, moet u AD FS-account als een claimprovider definiëren. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceert opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd.

AD FS definiëren als een claimprovider door toe te voegen `<ClaimsProvider>` knooppunt in de uitbreiding beleid-bestand:

1. Open het beleid extensiebestand (TrustFrameworkExtensions.xml) van uw werkmap. Als u een XML-editor, moet [probeer Visual Studio Code](https://code.visualstudio.com/download), een lichtgewicht editor voor meerdere platforms.
2. Zoek de `<ClaimsProviders>` sectie
3. Voeg de volgende XML-fragment uit onder de `ClaimsProviders` -element en vervang `identityProvider` met uw DNS (willekeurige waarde die aangeeft van uw domein) en sla het bestand. 

```xml
<ClaimsProvider>
    <Domain>contoso.com</Domain>
    <DisplayName>Contoso ADFS</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Contoso-SAML2">
        <DisplayName>Contoso ADFS</DisplayName>
        <Description>Login with your Contoso account</Description>
        <Protocol Name="SAML2"/>
        <Metadata>
        <Item Key="RequestsSigned">false</Item>
        <Item Key="WantsEncryptedAssertions">false</Item>
        <Item Key="PartnerEntity">https://{your_ADFS_domain}/federationmetadata/2007-06/federationmetadata.xml</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
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

## <a name="register-the-adfs-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>Registreren van de claimprovider van AD FS-account voor het ondertekenen van of zich in de gebruikersbeleving
Op dit moment is de id-provider ingesteld.  Het is echter niet beschikbaar in elk van de schermen aanmelden-up-to-date/aanmelden. Nu moet u de id-provider van de AD FS-account toevoegen aan uw gebruiker `SignUpOrSignIn` gebruikersbeleving. Om het beschikbaar maken, maken we een duplicaat van een bestaande sjabloon voor de gebruikersbeleving.  Vervolgens wijzigen we het zodat het de ADFS-id-provider bevat:

>[!NOTE]
>Als u eerder hebt gekopieerd de `<UserJourneys>` element van het base-bestand van uw beleid aan het extensiebestand (TrustFrameworkExtensions.xml), kunt u deze sectie overslaan.

1.  Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
2.  Zoek de `<UserJourneys>` element en kopieer de gehele inhoud van `<UserJourneys>` knooppunt.
3.  Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en zoek de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
4.  Plak de volledige inhoud van `<UserJournesy>` knooppunt dat u hebt gekopieerd als onderliggende site van de `<UserJourneys>` element.

### <a name="display-the-button"></a>De knop weergeven
De `<ClaimsProviderSelections>` element wordt de lijst met opties voor de selectie van claims-provider en de volgorde gedefinieerd.  `<ClaimsProviderSelection>` element is vergelijkbaar met een knop identity provider op een pagina aanmelden-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` -element voor AD FS-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1.  Zoek de `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"` in de gebruikersbeleving die u hebt gekopieerd.
2.  Zoek de `<OrchestrationStep>` knooppunt met `Order="1"`
3.  Voeg de volgende XML-fragment uit onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met AD FS-account voor het ontvangen van een token. De knop koppelen aan een actie door koppelen aan het technische profiel voor de claimprovider van uw AD FS-account:

1.  Zoek de `<OrchestrationStep>` die bevat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Voeg de volgende XML-fragment uit onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * Zorg ervoor dat de `Id` heeft dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie.
> * Zorg ervoor dat `TechnicalProfileReferenceId` is ingesteld in het technische profiel u eerdere (Contoso-SAML2) hebt gemaakt.

## <a name="upload-the-policy-to-your-tenant"></a>Uploaden van het beleid aan uw tenant
1.  In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2.  Selecteer **Identity-Ervaringsframework**.
3.  Open de **alle beleidsregels** blade.
4.  Selecteer **beleid uploaden**.
5.  Controleer **het beleid overschrijven als deze bestaat** vak.
6.  **Uploaden** TrustFrameworkExtensions.xml en zorg ervoor dat deze niet de validatie mislukt

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van nu uitvoeren
1.  Open **Azure AD B2C-instellingen** en Ga naar **Identity-Ervaringsframework**.
2.  Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid dat u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet aanmelden met AD FS-account.

## <a name="optional-register-the-adfs-account-claims-provider-to-profile-edit-user-journey"></a>[Optioneel] Registreren van de claimprovider van AD FS-account om te bewerken van profielen gebruikersbeleving
U kunt ook de id-provider van de AD FS-account toevoegen aan uw gebruiker `ProfileEdit` gebruikersbeleving. Om het beschikbaar maken, herhaalt u de laatste twee stappen:

### <a name="display-the-button"></a>De knop weergeven
1.  Open het bestand uitbreiding van uw beleid (bijvoorbeeld TrustFrameworkExtensions.xml).
2.  Zoek de `<UserJourney>` knooppunt met `Id="ProfileEdit"` in de gebruikersbeleving die u hebt gekopieerd.
3.  Zoek de `<OrchestrationStep>` knooppunt met `Order="1"`
4.  Voeg de volgende XML-fragment uit onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
1.  Zoek de `<OrchestrationStep>` die bevat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Voeg de volgende XML-fragment uit onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Het aangepaste profiel bewerken-beleid testen met behulp van nu uitvoeren
1.  Open **Azure AD B2C-instellingen** en Ga naar **Identity-Ervaringsframework**.
2.  Open **B2C_1A_ProfileEdit**, de relying party (RP) aangepast-beleid dat u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet aanmelden met AD FS-account.

## <a name="download-the-complete-policy-files"></a>De volledige bestanden downloaden
Optioneel: Wordt aangeraden bouwen van uw scenario met behulp van uw eigen aangepaste beleid bestanden na het voltooien van de aan de slag met aangepast beleid doorlopen. [De voorbeeldbestanden beleid alleen ter informatie](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
