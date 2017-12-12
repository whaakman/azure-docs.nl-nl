---
title: 'Azure Active Directory B2C: Een Salesforce SAML-provider toevoegen met behulp van aangepaste beleid | Microsoft Docs'
description: Meer informatie over het maken en beheren van Azure Active Directory B2C aangepast beleid.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: mtillman
editor: parakhj
ms.assetid: d7f4143f-cd7c-4939-91a8-231a4104dc2c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/11/2017
ms.author: parakhj
ms.openlocfilehash: 16f7c5708b479f18de17a612a733a2be6e97ad01
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a>Azure Active Directory B2C: Meld u aan met Salesforce accounts via SAML

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel laat zien hoe u [aangepast beleid](active-directory-b2c-overview-custom.md) voor het instellen van de aanmeldingspagina voor gebruikers van een specifieke Salesforce-organisatie.

## <a name="prerequisites"></a>Vereisten

### <a name="azure-ad-b2c-setup"></a>Installatie van de Azure AD B2C

Zorg ervoor dat u alle stappen die laten hoe u zien hebt uitgevoerd naar [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) in Azure Active Directory B2C (Azure AD B2C).

Deze omvatten:

* Een Azure AD B2C-tenant maken.
* Een Azure AD B2C-toepassing maken.
* Twee toepassingen van de groepsbeleid-engine registreren.
* Instellen van sleutels.
* Het starter pack instellen.

### <a name="salesforce-setup"></a>SalesForce setup

In dit artikel gaan we ervan uit dat u al het volgende hebt gedaan:

* Aangemeld bij een Salesforce-account. U kunt zich aanmelden voor een [gratis account ontwikkelaarsversie](https://developer.salesforce.com/signup).
* [Stel een mijn domein](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) voor uw Salesforce-organisatie.

## <a name="set-up-salesforce-so-users-can-federate"></a>Salesforce instellen zodat gebruikers communiceren kunnen.

Om te communiceren met Salesforce van Azure AD B2C, moet u de URL van de Salesforce-metagegevens.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Salesforce instellen als een id-provider

> [!NOTE]
> In dit artikel gaan we ervan uit u [Salesforce Lightning ervaring](https://developer.salesforce.com/page/Lightning_Experience_FAQ).

1. [Aanmelden bij Salesforce](https://login.salesforce.com/). 
2. In het menu links onder **instellingen**, vouw **identiteit**, en klik vervolgens op **identiteitsprovider**.
3. Klik op **inschakelen identiteitsprovider**.
4. Onder **selecteert u het certificaat**, selecteert u het certificaat dat u wilt dat Salesforce gebruiken om te communiceren met Azure AD B2C. (U kunt het standaardcertificaat gebruiken.) Klik op **Opslaan**. 

### <a name="create-a-connected-app-in-salesforce"></a>Een verbonden app maken in Salesforce

1. Op de **identiteitsprovider** pagina, gaat u naar **serviceproviders**.
2. Klik op **serviceproviders worden nu gemaakt via verbonden Apps. Klik hier.**
3. Onder **basisinformatie**, geef de vereiste waarden voor uw verbonden app.
4. Onder **Web-Appinstellingen**, selecteer de **SAML inschakelen** selectievakje.
5. In de **entiteit-ID** en voer de volgende URL. Zorg ervoor dat u de waarde voor vervangen `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. In de **ACS URL** en voer de volgende URL. Zorg ervoor dat u de waarde voor vervangen `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Laat de standaardwaarden voor alle andere instellingen.
8. Ga naar de onderkant van de lijst en klik vervolgens op **opslaan**.

### <a name="get-the-metadata-url"></a>De metagegevens-URL ophalen

1. Klik op de overzichtspagina van uw verbonden app **beheren**.
2. Kopieer de waarde voor **metagegevens detectie-eindpunt**, en vervolgens opslaan. U gebruikt deze verderop in dit artikel.

### <a name="set-up-salesforce-users-to-federate"></a>Salesforce-gebruikers instellen om te federeren

1. Op de **beheren** pagina van de verbonden app, gaat u naar **profielen**.
2. Klik op **profielen beheren**.
3. Selecteer de profielen (of groepen gebruikers) die u wilt federeren met Azure AD B2C. Als systeembeheerder, selecteer de **systeembeheerder** selectievakje, zodat u kunt federeren met uw Salesforce-account.

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a>Een handtekeningcertificaat voor Azure AD B2C genereren

Aanvragen verzonden naar Salesforce moeten zijn ondertekend door Azure AD B2C. Open Azure PowerShell en voer de volgende opdrachten voor het genereren van een certificaat voor ondertekening.

> [!NOTE]
> Zorg ervoor dat u de tenantnaam en het wachtwoord in de regels van de twee bovenste bijwerken.

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-the-saml-signing-certificate-to-azure-ad-b2c"></a>Het handtekeningcertificaat SAML toevoegen aan Azure AD B2C

Het handtekeningcertificaat uploaden naar uw Azure AD B2C-tenant: 

1. Ga naar uw Azure AD B2C-tenant. Klik op **instellingen** > **identiteit ervaring Framework** > **beleid sleutels**.
2. Klik op **+ toevoegen**, en vervolgens:
    1. Klik op **opties** > **uploaden**.
    2. Voer een **naam** (bijvoorbeeld SAMLSigningCert). Het voorvoegsel *B2C_1A_* wordt automatisch toegevoegd aan de naam van uw sleutel.
    3. Als uw certificaat selecteren, schakelt **uploaden bestand besturingselement**. 
    4. Voer wachtwoord in van het certificaat dat u in het PowerShell-script instellen.
3. Klik op **Create**.
4. Controleer of u een sleutel (bijvoorbeeld B2C_1A_SAMLSigningCert) hebt gemaakt. Noteer de volledige naam (inclusief *B2C_1A_*). U wordt naar deze sleutel later in het beleid verwijzen.

## <a name="create-the-salesforce-saml-claims-provider-in-your-base-policy"></a>Maken van de claimprovider Salesforce SAML in uw base beleid

U moet Salesforce definiëren als een claimprovider, zodat gebruikers zich aanmelden kunnen met behulp van Salesforce. Met andere woorden, moet u het eindpunt dat Azure AD B2C met communiceren zullen opgeven. Het eindpunt wordt *bieden* een reeks *claims* die gebruikmaakt van Azure AD B2C om te controleren dat een specifieke gebruiker is geverifieerd. Om dit te doen, Voeg een `<ClaimsProvider>` voor Salesforce in het extensiebestand van het beleid:

1. Open het extensiebestand (TrustFrameworkExtensions.xml) in uw werkmap.
2. Zoek de `<ClaimsProviders>` sectie. Als deze niet bestaat nog, maakt u dit onder het hoofdknooppunt.
3. Voeg een nieuwe `<ClaimsProvider>`:

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="RequestsSigned">false</Item>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="externalIdp"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
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

Onder de `<ClaimsProvider>` knooppunt:

1. Wijzig de waarde voor `<Domain>` naar een unieke waarde die wordt onderscheiden `<ClaimsProvider>` van andere id-providers.
2. Werk de waarde voor `<DisplayName>` naar een weergavenaam voor de claimprovider. Deze waarde wordt momenteel niet gebruikt.

### <a name="update-the-technical-profile"></a>Het technische profiel bijwerken

Als u een SAML-token van Salesforce, definiëren de protocollen die Azure AD B2C wordt gebruikt voor communicatie met Azure Active Directory (Azure AD). Dit doen in de `<TechnicalProfile>` element van `<ClaimsProvider>`:

1. Bijwerken van de ID van de `<TechnicalProfile>` knooppunt. Deze ID wordt gebruikt om te verwijzen naar dit technische profiel van andere onderdelen van het beleid.
2. Werk de waarde voor `<DisplayName>`. Deze waarde wordt weergegeven op de knop aanmelden op de aanmeldingspagina.
3. Werk de waarde voor `<Description>`.
4. SalesForce maakt gebruik van het SAML 2.0-protocol. Zorg ervoor dat de waarde voor `<Protocol>` is **SAML2**.

Update de `<Metadata>` sectie in de voorgaande XML in overeenstemming met de instellingen voor uw specifieke Salesforce-account. Werk de metagegevenswaarden in de XML:

1. Werk de waarde van `<Item Key="PartnerEntity">` met de Salesforce metagegevens-URL die u eerder hebt gekopieerd. Deze heeft de volgende indeling: 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. In de `<CryptographicKeys>` sectie, werk de waarde voor beide exemplaren van `StorageReferenceId` op de naam van de sleutel van het handtekeningcertificaat (bijvoorbeeld B2C_1A_SAMLSigningCert).

### <a name="upload-the-extension-file-for-verification"></a>Upload het extensiebestand voor verificatie

Het beleid is nu geconfigureerd zodat Azure AD B2C weet hoe om te communiceren met Salesforce. Upload het extensiebestand van het beleid, om te controleren of er geen problemen die tot nu toe. Het bestand te uploaden uitbreiding van uw beleid:

1. In uw Azure AD B2C-tenant, gaat u naar de **alle beleidsregels** blade.
2. Selecteer de **het beleid overschreven als deze bestaat** selectievakje.
3. Upload het extensiebestand (TrustFrameworkExtensions.xml). Zorg ervoor dat deze validatie niet is mislukt.

## <a name="register-the-salesforce-saml-claims-provider-to-a-user-journey"></a>Registreren van de claimprovider Salesforce SAML aan een gebruiker reis

Voeg vervolgens de Salesforce SAML-identiteitsprovider naar een van de gebruiker trajecten. Op dit moment wordt de id-provider is ingesteld, maar is niet beschikbaar is op een van de gebruiker registreren of aanmelden pagina's. Als de id-provider toevoegen aan een aanmeldingspagina, maak eerst een duplicaat van een bestaande sjabloon gebruiker reis. De sjabloon vervolgens wijzigen zodat er ook de Azure AD-id-provider.

1. Open het bestand basis van uw beleid (bijvoorbeeld TrustFrameworkBase.xml).
2. Zoek de `<UserJourneys>` -element en kopieer de gehele `<UserJourney>` waarde, inclusief Id = 'SignUpOrSignIn'.
3. Open het extensiebestand (bijvoorbeeld TrustFrameworkExtensions.xml). Zoek de `<UserJourneys>` element. Als het element niet bestaat, maakt u een.
4. Plak de gekopieerde gehele `<UserJourney>` als een onderliggend element van de `<UserJourneys>` element.
5. Wijzig de naam van de ID van de nieuwe `<UserJourney>` (bijvoorbeeld SignUpOrSignUsingContoso).

### <a name="display-the-identity-provider-button"></a>De knop identiteit provider weergeven

De `<ClaimsProviderSelection>` element is vergelijkbaar met een knop identiteit provider op een pagina registreren of aanmelden. Door het toevoegen van een `<ClaimsProviderSelection>` -element voor Salesforce, een nieuwe knop wordt weergegeven wanneer een gebruiker aan deze pagina. De knop identiteit provider weergeven:

1. In de `<UserJourney>` die u hebt gemaakt, zoek de `<OrchestrationStep>` met `Order="1"`.
2. Voeg de volgende XML-code:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. Stel `TargetClaimsExchangeId` naar een logische waarde. Het is raadzaam na de dezelfde overeenkomst als anderen (bijvoorbeeld  *\[ClaimProviderName\]Exchange*).

### <a name="link-the-identity-provider-button-to-an-action"></a>De knop van de provider identiteit koppelen aan een actie

Nu dat u een knop van de provider identiteit opgesteld hebben, kunt u deze aan een actie koppelen. In dit geval is de actie voor Azure AD B2C om te communiceren met Salesforce voor het ontvangen van een SAML-token. U kunt dit doen door koppelen aan het technische profiel voor uw Salesforce SAML claimprovider:

1. In de `<UserJourney>` knooppunt, vinden de `<OrchestrationStep>` met `Order="2"`.
2. Voeg de volgende XML-code:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. Update `Id` op dezelfde waarde die u eerder hebt gebruikt voor `TargetClaimsExchangeId`.
4. Update `TechnicalProfileReferenceId` naar de `Id` profiel van de technische u eerder hebt gemaakt (bijvoorbeeld ContosoProfile).

### <a name="upload-the-updated-extension-file"></a>Het bijgewerkte extensiebestand uploaden

U klaar bent met wijzigen van het extensiebestand. Sla en dit bestand niet uploaden. Zorg ervoor dat alle validaties slaagt.

### <a name="update-the-relying-party-file"></a>De relying party-bestand bijwerken

Werk vervolgens de relying party (RP)-bestand waarmee de gebruiker reis die u hebt gemaakt:

1. Maak een kopie van SignUpOrSignIn.xml in uw werkmap. Wijzig de naam (bijvoorbeeld SignUpOrSignInWithAAD.xml).
2. Open het nieuwe bestand en update de `PolicyId` kenmerk voor `<TrustFrameworkPolicy>` met een unieke waarde. Dit is de naam van uw beleid (bijvoorbeeld SignUpOrSignInWithAAD).
3. Wijzig de `ReferenceId` kenmerk in `<DefaultUserJourney>` overeenkomen met de `Id` van de nieuwe gebruiker reis die u hebt gemaakt (bijvoorbeeld SignUpOrSignUsingContoso).
4. De wijzigingen opslaan en vervolgens te uploaden.

## <a name="test-and-troubleshoot"></a>Testen en problemen oplossen

Test het aangepaste beleid dat u zojuist hebt geüpload, in de Azure-portal, gaat u naar de blade beleid op en klik vervolgens op **nu uitvoeren**. Als dit mislukt, Zie [aangepaste beleidsproblemen oplossen](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Volgende stappen

Feedback geven aan [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).
