---
title: Instellen van aanmelding met een Salesforce-SAML-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Instellen van aanmelding met een Salesforce-SAML-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 920d5924271d31848b455ebece9e4832548e1b06
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54846401"
---
# <a name="set-up-sign-in-with-a-salesforce-saml-provider-by-using-custom-policies-in-azure-active-directory-b2c"></a>Instellen van aanmelding met een Salesforce-SAML-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelding voor gebruikers via een Salesforce organisatie inschakelen [aangepast beleid](active-directory-b2c-overview-custom.md) in Azure Active Directory (Azure AD) B2C. U aanmelden inschakelen door toe te voegen een [technische SAML-profiel](saml-technical-profile.md) naar een aangepast beleid.

## <a name="prerequisites"></a>Vereisten

- Voer de stappen in [aan de slag met aangepaste beleidsregels in Azure Active Directory B2C](active-directory-b2c-get-started-custom.md).
- Als u dit nog niet hebt gedaan, kunt u zich aanmelden voor een [gratis account van de Developer Edition](https://developer.salesforce.com/signup). In dit artikel wordt de [Salesforce Lightning ervaring](https://developer.salesforce.com/page/Lightning_Experience_FAQ).
- [Stel een mijn domein](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) voor uw Salesforce-organisatie.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Salesforce instellen als een id-provider

1. [Aanmelden bij Salesforce](https://login.salesforce.com/). 
2. In het menu links onder **instellingen**, vouw **identiteit**, en selecteer vervolgens **id-Provider**.
3. Selecteer **id-Provider inschakelen**.
4. Onder **selecteert u het certificaat**, selecteert u het certificaat dat u wilt dat Salesforce gebruiken om te communiceren met Azure AD B2C. U kunt het standaard-certificaat gebruiken. 
5. Klik op **Opslaan**. 

### <a name="create-a-connected-app-in-salesforce"></a>Een verbonden app maken in Salesforce

1. Op de **id-Provider** weergeeft, schakelt **serviceproviders worden nu gemaakt via Apps die zijn verbonden. Klik hier.**
2. Onder **basisinformatie**, voer de vereiste waarden voor uw verbonden app.
3. Onder **Web-Appinstellingen**, Controleer de **SAML inschakelen** vak.
4. In de **entiteit-ID** en voer de volgende URL. Zorg ervoor dat u de waarde voor vervangen `your-tenant` met de naam van uw Azure AD B2C-tenant.

      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```

6. In de **ACS URL** en voer de volgende URL. Zorg ervoor dat u de waarde voor vervangen `your-tenant` met de naam van uw Azure AD B2C-tenant.
      
      ```
      https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Ga naar de onderkant van de lijst en klik vervolgens op **opslaan**.

### <a name="get-the-metadata-url"></a>De metagegevens-URL ophalen

1. Klik op de overzichtspagina van uw verbonden app **beheren**.
2. Kopieer de waarde voor **detectie-eindpunt voor metagegevens**, en vervolgens op te slaan. U gebruikt deze verderop in dit artikel.

### <a name="set-up-salesforce-users-to-federate"></a>Salesforce-gebruikers instellen om te federeren

1. Op de **beheren** pagina van uw verbonden app, klikt u op **Spravovat Profily**.
2. Selecteer de profielen (of groepen gebruikers) die u wilt federeren met Azure AD B2C. Als een systeembeheerder bent, selecteert u de **systeembeheerder** selectievakje, zodat u kunt federeren met uw Salesforce-account.

## <a name="generate-a-signing-certificate"></a>Een handtekeningcertificaat genereren

Aanvragen verzonden naar de Salesforce moeten worden ondertekend door Azure AD B2C. Voor het genereren van een ondertekend certificaat, opent u Azure PowerShell en voer de volgende opdrachten.

> [!NOTE]
> Zorg ervoor dat u de naam van tenant en het wachtwoord in de twee belangrijkste regels bijwerken.

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="create-a-policy-key"></a>De beleidssleutel van een maken

U moet voor het opslaan van het certificaat dat u hebt gemaakt in uw Azure AD B2C-tenant.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer op de pagina overzicht **Identiteitsfunctie: PREVIEW**.
5. Selecteer **Beleidssleutels** en selecteer vervolgens **toevoegen**.
6. Voor **opties**, kiest u `Upload`.
7. Voer een **naam** in voor het beleid. Bijvoorbeeld: SAMLSigningCert. Het voorvoegsel `B2C_1A_` wordt automatisch toegevoegd aan de naam van uw sleutel.
8. Blader naar en selecteer het B2CSigningCert.pfx-certificaat dat u hebt gemaakt. 
9. Voer de **wachtwoord** voor het certificaat.
3. Klik op **Create**.

## <a name="add-a-claims-provider"></a>Toevoegen van een claimprovider

Als u wilt dat gebruikers zich aanmelden met een Salesforce-account, moet u het account als een claimprovider waarmee Azure AD B2C via een eindpunt communiceren kunnen definiëren. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd. 

U kunt een Salesforce-account als een claimprovider definiëren door toe te voegen aan de **ClaimsProviders** element in het bestand uitbreiding van uw beleid.

1. Open de *TrustFrameworkExtensions.xml*.
2. Zoek de **ClaimsProviders** element. Als deze niet bestaat, kunt u deze onder het root-element toevoegen.
3. Toevoegen van een nieuwe **ClaimsProvider** als volgt:

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
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
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

4. Werk de waarde van **PartnerEntity** met de metagegevens-URL voor Salesforce die u eerder hebt gekopieerd.
5. Werk de waarde van beide exemplaren van **StorageReferenceId** op de naam van de sleutel van het certificaat voor ondertekening. Bijvoorbeeld: B2C_1A_SAMLSigningCert.

### <a name="upload-the-extension-file-for-verification"></a>Upload het extensiebestand voor verificatie

U hebt nu uw beleid geconfigureerd zodat Azure AD B2C weet hoe om te communiceren met uw Salesforce-account. Upload het bestand uitbreiding van uw beleid om te bevestigen dat er geen problemen met tot nu toe.

1. Op de **aangepast beleid** pagina in uw Azure AD B2C-tenant, selecteer **uploaden beleid**.
2. Schakel **het beleid overschrijven als deze bestaat**, en blader vervolgens naar en selecteer de *TrustFrameworkExtensions.xml* bestand.
3. Klik op **Uploaden**.

## <a name="register-the-claims-provider"></a>De claimprovider registreren

Op dit moment wordt de id-provider is ingesteld, maar het is niet beschikbaar zijn in elk van de schermen registreren of aanmelden. Om het beschikbaar maken, kunt u een duplicaat van een bestaande sjabloon voor de gebruikersbeleving maken en wijzigen zodat het bevat ook de Salesforce-id-provider.

1. Open de *TrustFrameworkBase.xml* -bestand van het starter-pack.
2. Zoeken en kopieer de gehele inhoud van de **UserJourney** element met `Id="SignUpOrSignIn"`.
3. Open de *TrustFrameworkExtensions.xml* en zoek de **UserJourneys** element. Als het element niet bestaat, Voeg een.
4. Plak de volledige inhoud van de **UserJourney** element dat u hebt gekopieerd als onderliggende site van de **UserJourneys** element.
5. Wijzig de naam van de ID van de gebruikersbeleving. Bijvoorbeeld `SignUpSignInSalesforce`.

### <a name="display-the-button"></a>De knop weergeven

De **ClaimsProviderSelection** element is vergelijkbaar met een id-provider-knop op het scherm registreren of aanmelden. Als u een **ClaimsProviderSelection** -element voor een LinkedIn-account, een nieuwe knop wordt weergegeven wanneer een gebruiker op de pagina terechtkomt.

1. Zoek de **OrchestrationStep** element met `Order="1"` in de gebruikersbeleving die u zojuist hebt gemaakt.
2. Onder **ClaimsProviderSelects**, voegt u het volgende element toe. Stel de waarde van **TargetClaimsExchangeId** naar een geschikte waarde, bijvoorbeeld `SalesforceExchange`:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="SalesforceExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met een Salesforce-account voor het ontvangen van een token.

1. Zoek de **OrchestrationStep** die bevat `Order="2"` in de gebruikersbeleving.
2. Voeg de volgende **ClaimsExchange** element ervoor te zorgen dat u dezelfde waarde voor **Id** die u hebt gebruikt voor **TargetClaimsExchangeId**:

    ```XML
    <ClaimsExchange Id="SalesforceExchange" TechnicalProfileReferenceId="salesforce" />
    ```
    
    Werk de waarde van **TechnicalProfileReferenceId** naar de **Id** van het technische profiel dat u eerder hebt gemaakt. Bijvoorbeeld `LinkedIn-OAUTH`.

3. Sla de *TrustFrameworkExtensions.xml* -bestand en upload het opnieuw om te verifiëren.

## <a name="create-an-azure-ad-b2c-application"></a>Een Azure AD B2C-toepassing maken

Communicatie met Azure AD B2c vindt plaats via een toepassing die u in uw tenant maakt. Deze sectie vindt u optionele stappen die u uitvoeren kunt voor het maken van een testtoepassing als u dat nog niet hebt gedaan.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
5. Voer een naam voor de toepassing, bijvoorbeeld *testapp1*.
6. Voor **Web-App / Web-API**, selecteer `Yes`, en voer vervolgens `https://jwt.ms` voor de **antwoord-URL**.
7. Klik op **Create**.

## <a name="update-and-test-the-relying-party-file"></a>Bijwerken en testen van de relying party-bestand

Update voor de relying party (RP)-bestand dat initieert de gebruikersbeleving die u zojuist hebt gemaakt:

1. Maak een kopie van *SignUpOrSignIn.xml* in uw werkmap en wijzig de naam. Bijvoorbeeld, wijzig de naam *SignUpSignInSalesforce.xml*.
2. Open het nieuwe bestand en werk de waarde van de **PolicyId** voor het kenmerk **TrustFrameworkPolicy** met een unieke waarde. Bijvoorbeeld `SignUpSignInSalesforce`.
3. Werk de waarde van **PublicPolicyUri** aan de URI voor het beleid. Bijvoorbeeld:`http://contoso.com/B2C_1A_signup_signin_salesforce`
4. Werk de waarde van de **ReferenceId** kenmerk in **DefaultUserJourney** zodat deze overeenkomen met de ID van de nieuwe gebruikersbeleving die u hebt gemaakt (SignUpSignInSalesforce).
5. Sla uw wijzigingen, upload het bestand en selecteer vervolgens het nieuwe beleid in de lijst.
6. Zorg ervoor dat Azure AD B2C-toepassing die u hebt gemaakt is geselecteerd de **toepassing selecteren** veld en vervolgens testen door te klikken op **nu uitvoeren**.
