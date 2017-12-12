---
title: 'Azure Active Directory B2C: ADFS als SAML-identiteitsprovider gebruik van aangepast beleid toevoegen'
description: Een artikel instructies over het instellen van ADFS-2016 met SAML-protocol en aangepaste beleidsregels
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
ms.openlocfilehash: 22b360aec8878925ebe8d2c67c76d275a42ca7a8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-add-adfs-as-a-saml-identity-provider-using-custom-policies"></a>Azure Active Directory B2C: ADFS als SAML-identiteitsprovider gebruik van aangepast beleid toevoegen

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel ziet u het inschakelen van aanmelden voor gebruikers van AD FS-account met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Vereisten

Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

Deze stappen omvatten:

1.  Maken van een Relying Party Trust ADFS.
2.  Het certificaat ADFS Relying Party Trust toevoegen aan Azure AD B2C.
3.  Claimprovider toevoegen aan een beleid.
4.  Registreren van de AD FS claimprovider account aan een gebruiker reis.
5.  Uploaden van het beleid naar een Azure AD B2C-tenant en testen.

## <a name="to-create-a-claims-aware-relying-party-trust"></a>Een claimbewuste Relying Party Trust maken

Voor het gebruik van AD FS als een id-provider in Azure Active Directory (Azure AD) B2C, moet u een AD FS vertrouwen voor Relying Party maken en deze met de juiste parameters opgeven.

Om een nieuwe relying party trust toevoegen met behulp van de module AD FS-beheer en de instellingen handmatig configureren, moet u de volgende procedure uitvoeren op een federatieserver.

Lidmaatschap van **beheerders**, of gelijkwaardig op de lokale computer is de minimale vereiste om deze procedure te voltooien. Meer informatie over het gebruik van de juiste accounts en groepslidmaatschappen op [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477)

1.  Klik in Serverbeheer op **extra**, en selecteer vervolgens **ADFS Management**.

2.  Klik op **Add Relying Party Trust**.
    ![Vertrouwensrelatie van Relying Party toevoegen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-1.png)

3.  Op de **Welkom** pagina **Claims aware** en klik op **Start**.
    ![Kies op de welkomstpagina op de hoogte van Claims](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-2.png)
4.  Op de **gegevensbron selecteren** pagina, klikt u op **gegevens over de relying party handmatig invoeren**, en klik vervolgens op **volgende**.
    ![Gegevens over de relying party invoeren](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-3.png)

5.  Op de **weergavenaam opgeven** pagina, typ een naam in **weergavenaam**onder **notities** Typ een beschrijving voor deze vertrouwensrelatie van relying party en klik vervolgens op **volgende**.
    ![Geef de weergavenaam en -opmerkingen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-4.png)
6.  Optioneel. Als u een certificaat met optionele tokenversleuteling, klikt u vervolgens op de **certificaat configureren** pagina, klikt u op **Bladeren** Zoek uw certificaatbestand en klik vervolgens op **volgende**.
    ![Certificaat configureren](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-5.png)
7.  Op de **URL configureren** pagina de **inschakelen van ondersteuning voor het SAML 2.0 WebSSO protocol** selectievakje. Onder **Relying party SAML 2.0 SSO service URL**, typt u de Security Assertion Markup Language (SAML) service-eindpunt-URL voor deze vertrouwensrelatie van relying party en klik vervolgens op **volgende**.  Voor de **Relying party SAML 2.0 SSO service URL**, plak de `https://login.microsoftonline.com/te/{tenant}.onmicrosoft.com/{policy}`. {Tenant} vervangen door de naam van uw tenant (bijvoorbeeld contosob2c.onmicrosoft.com) en de {beleid} vervangen door de naam van uw uitbreidingen beleid (bijvoorbeeld B2C_1A_TrustFrameworkExtensions).
    > [!IMPORTANT]
    >Naam van het beleid wordt dat beleid signup_or_signin neemt over van, in dit geval is: `B2C_1A_TrustFrameworkExtensions`.
    >Bijvoorbeeld de URL is: https://login.microsoftonline.com/te/**contosob2c**.onmicrosoft.com/**B2C_1A_TrustFrameworkBase**.

    ![Relying party SAML 2.0 SSO service URL](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-6.png)
8. Op de **id's configureren** pagina, dezelfde URL opgeven als de vorige stap, klik op **toevoegen** toe te voegen aan de lijst en klik vervolgens op **volgende**.
    ![Relying party trust-id 's](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-7.png)
9.  Op de **beleid voor toegangsbeheer kiezen** selecteert u een beleid en klik op **volgende**.
    ![Beleid voor toegangsbeheer kiezen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-8.png)
10.  Op de **gereed voor toevoegen vertrouwensrelatie** pagina, controleert u de instellingen en klik vervolgens op **volgende** om op te slaan van de relying party trust informatie.
    ![Sla uw relying party trust-gegevens](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-9.png)
11.  Op de **voltooien** pagina, klikt u op **sluiten**, met deze actie wordt automatisch de **Claimregels bewerken** in het dialoogvenster.
    ![Claimregels bewerken](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-rp-10.png)
12. Klik op **regel toevoegen**.  
      ![Nieuwe regel toevoegen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-1.png)
13.  In **claimregelsjabloon**, selecteer **verzenden LDAP-kenmerken als claims**.
    ![Verzenden van LDAP-kenmerken als claims sjabloonregel selecteren](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-2.png)
14.  Geef **naam Claimregel**. Voor de **kenmerkopslag** Selecteer **Active Directory selecteren** de volgende claims toe te voegen en klik vervolgens op **voltooien** en **OK**.
    ![Eigenschappen van de regel instellen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)
15.  Selecteer in Serverbeheer **Relying Party-vertrouwensrelaties** en selecteer de relying party vertrouwt u hebt gemaakt en klik vervolgens **eigenschappen**.
    ![Eigenschappen van relying party bewerken](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-1.png)
16.  Het eigenschappenvenster van relying party trust (B2C Demo) klikt u op een **handtekening** tabblad en klik op **toevoegen**.  
    ![Set-handtekening](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-2.png)
17.  Uw handtekeningcertificaat (.cert-bestand, zonder persoonlijke sleutel) toevoegen.  
    ![Uw handtekeningcertificaat toevoegen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-3.png)
18.  Klik in het eigenschappenvenster van relying party trust (B2C Demo) op **Geavanceerd** tabblad en wijzig de **veilige hash-algoritme** naar **SHA-1**, klikt u op **Ok**.  
    ![Secure hash-algoritme SHA-1 instellen](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-sig-4.png)

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>De AD FS-account Toepassingssleutel toevoegen aan Azure AD B2C
Federatie met AD FS-accounts vereist een clientgeheim voor AD FS-account aan Azure AD B2C-vertrouwensrelatie voor de toepassing is. U moet uw ADFS-certificaat wordt opgeslagen in uw Azure AD B2C-tenant. 

1.  Ga naar uw Azure AD B2C-tenant en selecteer **B2C-instellingen** > **identiteit ervaring Framework**
2.  Selecteer **beleid sleutels** om de sleutels die beschikbaar zijn in uw tenant weer te geven.
3.  Klik op **+ toevoegen**.
4.  Voor **opties**, gebruik **uploaden**.
5.  Voor **naam**, gebruik `ADFSSamlCert`.  
    Het voorvoegsel `B2C_1A_` kunnen automatisch worden toegevoegd.
6.  In het bestand te uploaden, ** selecteert u het PFX-certificaatbestand met persoonlijke sleutel. Opmerking: dit certificaat (met de persoonlijke sleutel) moet hetzelfde account zijn verleend en die worden gebruikt voor de relying party ADFS hebben.
![Beleidssleutel uploaden](media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-cert.png)
7.  Klik op **Maken**.
8.  Controleer of u de sleutel hebt gemaakt `B2C_1A_ADFSSamlCert`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Toevoegen van een claimprovider in de uitbreiding beleid
Als u wilt dat gebruikers zich aanmelden via AD FS-account, moet u AD FS-account als een claimprovider definiëren. Met andere woorden, moet u een eindpunt dat Azure AD B2C met communiceert opgeven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren dat een specifieke gebruiker is geverifieerd.

ADFS definiëren als een claimprovider door toe te voegen `<ClaimsProvider>` knooppunt in het beleidsbestand extensie:

1. Open het bestand met de extensie (TrustFrameworkExtensions.xml) van uw werkmap. Als u een XML-editor moet [Visual Studio Code probeert](https://code.visualstudio.com/download), een lichtgewicht platformoverschrijdende-editor.
2. Zoek de `<ClaimsProviders>` sectie
3. Voeg de volgende XML-fragment onder de `ClaimsProviders` element en vervang `identityProvider` met uw DNS (willekeurige waarde die aangeeft van uw domein) en sla het bestand. 

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

## <a name="register-the-adfs-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a>De claimprovider ADFS-account voor het ondertekenen van registreren of aanmelden gebruiker reis
Op dit moment is de identiteitsprovider ingesteld.  Het is echter niet beschikbaar in een van de schermen sign-up-to-date/aanmelden. Nu moet u de id-provider van de AD FS-account toevoegen aan de gebruiker `SignUpOrSignIn` traject van de gebruiker. Als u deze beschikbaar, maken we een duplicaat van een bestaande sjabloon gebruiker reis.  Vervolgens wordt deze wijzigen zodat het de ADFS-id-provider bevat:

>[!NOTE]
>Als u eerder hebt gekopieerd de `<UserJourneys>` element uit de base-bestand van uw beleid aan het extensiebestand (TrustFrameworkExtensions.xml), kunt u deze sectie overslaan.

1.  Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
2.  Zoek de `<UserJourneys>` element en kopieert u de volledige inhoud van `<UserJourneys>` knooppunt.
3.  Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml) en Ga naar de `<UserJourneys>` element. Als het element niet bestaat, Voeg een.
4.  Plak de volledige inhoud van `<UserJournesy>` knooppunt dat u hebt gekopieerd als een onderliggend element van de `<UserJourneys>` element.

### <a name="display-the-button"></a>De knop weergeven
De `<ClaimsProviderSelections>` element wordt de lijst met opties voor de selectie van claims provider en de volgorde gedefinieerd.  `<ClaimsProviderSelection>`element is vergelijkbaar met een knop identiteit provider op een pagina sign-up-to-date/aanmelden. Als u een `<ClaimsProviderSelection>` element voor AD FS-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt. Dit element toevoegen:

1.  Zoek de `<UserJourney>` knooppunt met `Id="SignUpOrSignIn"` in het traject gebruiker die u hebt gekopieerd.
2.  Zoek de `<OrchestrationStep>` knooppunt bevat`Order="1"`
3.  Plaats de volgende XML-fragment onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```
### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu u een knop hebt geïmplementeerd, moet u deze koppelen aan een actie. Er is in dit geval de actie voor Azure AD B2C om te communiceren met AD FS-account geen token ontvangen. De knop koppelen aan een actie door koppelen aan het technische profiel voor de claimprovider van uw AD FS-account:

1.  Zoek de `<OrchestrationStep>` die omvat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Plaats de volgende XML-fragment onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

> [!NOTE]
> * Zorg ervoor dat de `Id` heeft dezelfde waarde als die van `TargetClaimsExchangeId` in de vorige sectie.
> * Zorg ervoor dat `TechnicalProfileReferenceId` is ingesteld in het technische profiel u eerdere (Contoso SAML2) gemaakt.

## <a name="upload-the-policy-to-your-tenant"></a>Het beleid uploaden naar uw tenant
1.  In de [Azure-portal](https://portal.azure.com), schakel over naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en open de **Azure AD B2C** blade.
2.  Selecteer **identiteit ervaring Framework**.
3.  Open de **alle beleidsregels** blade.
4.  Selecteer **uploaden beleid**.
5.  Controleer **het beleid overschreven als deze bestaat** vak.
6.  **Uploaden** TrustFrameworkExtensions.xml en zorg ervoor dat deze niet de validatie mislukt

## <a name="test-the-custom-policy-by-using-run-now"></a>Het aangepaste beleid testen met behulp van nu uitvoeren
1.  Open **Azure AD B2C-instellingen** en Ga naar **identiteit ervaring Framework**.
2.  Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid die u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet zich aanmelden met AD FS-account.

## <a name="optional-register-the-adfs-account-claims-provider-to-profile-edit-user-journey"></a>[Optioneel] Registreren van de claimprovider ADFS-account om te bewerken van profielen gebruiker reis
U kunt ook de id-provider van de AD FS-account toevoegen aan de gebruiker `ProfileEdit` traject van de gebruiker. Als u deze beschikbaar, herhaalt u de laatste twee stappen:

### <a name="display-the-button"></a>De knop weergeven
1.  Open het extensiebestand van uw beleid (bijvoorbeeld TrustFrameworkExtensions.xml).
2.  Zoek de `<UserJourney>` knooppunt met `Id="ProfileEdit"` in het traject gebruiker die u hebt gekopieerd.
3.  Zoek de `<OrchestrationStep>` knooppunt bevat`Order="1"`
4.  Plaats de volgende XML-fragment onder `<ClaimsProviderSelections>` knooppunt:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie
1.  Zoek de `<OrchestrationStep>` die omvat `Order="2"` in de `<UserJourney>` knooppunt.
2.  Plaats de volgende XML-fragment onder `<ClaimsExchanges>` knooppunt:

```xml
<ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a>Het aangepaste profiel bewerken beleid testen met behulp van nu uitvoeren
1.  Open **Azure AD B2C-instellingen** en Ga naar **identiteit ervaring Framework**.
2.  Open **B2C_1A_ProfileEdit**, de relying party (RP) aangepast-beleid die u hebt geüpload. Selecteer **nu uitvoeren**.
3.  U moet zich aanmelden met AD FS-account.

## <a name="download-the-complete-policy-files"></a>De volledige beleidsbestanden downloaden
Optioneel: We raden aan bouwen van uw scenario met behulp van uw eigen aangepaste beleid bestanden na het voltooien van de aan de slag met aangepast beleid dat is doorlopen. [De voorbeeldbestanden beleid alleen ter informatie](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app)
