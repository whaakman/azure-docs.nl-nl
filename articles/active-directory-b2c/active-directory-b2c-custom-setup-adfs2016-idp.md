---
title: AD FS toevoegen als een SAML-id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C | Microsoft Docs
description: Instellen van AD FS 2016 met de SAML-protocol en aangepaste beleidsregels in Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2c2e6861fda42a9e8c1aabcba303bfede47ac3c1
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669223"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>AD FS toevoegen als een SAML-id-provider met behulp van aangepaste beleidsregels in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Dit artikel leest u hoe u aanmelden voor een gebruikersaccount van de AD FS inschakelen met behulp van [aangepast beleid](active-directory-b2c-overview-custom.md) in Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Vereisten

Voer de stappen in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>De sleutel van het ADFS-toepassing toevoegen aan Azure AD B2C

Federatie met een AD FS-account vereist een clientgeheim voor het account aan Azure AD B2C-vertrouwensrelatie namens de toepassing. U moet uw AD FS-certificaat wordt opgeslagen in uw Azure AD B2C-tenant. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant gebruikt door hiernaar over te schakelen rechtsboven in de Azure Portal. Selecteer **schakelen tussen mappen**, en kies vervolgens de map met de tenant die u hebt gemaakt. In deze zelfstudie de *contoso* directory wordt gebruikt met de tenant met de naam *contoso0522Tenant.onmicrosoft.com*.

    ![Schakelen tussen mappen](./media/active-directory-b2c-custom-setup-adfs2016-idp/switch-directories.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze. U moet nu worden met behulp van uw tenant.
4. Selecteer op de pagina overzicht **Identity-Ervaringsframework**.
5. Selecteer **Beleidssleutels** weergeven van de sleutels die beschikbaar zijn in uw tenant, en klik vervolgens op **toevoegen**.
6. Kies **uploaden** als de optie.
7. Voer `ADFSSamlCert` voor de naam. Het voorvoegsel `B2C_1A_` mogelijk automatisch worden toegevoegd.
8. Blader naar en selecteer het PFX-certificaatbestand met de persoonlijke sleutel. Dit certificaat met de persoonlijke sleutel moet hetzelfde account dat is verleend en die wordt gebruikt voor de relying party van AD FS.
9. Klik op **maken** en Bevestig dat u hebt gemaakt de `B2C_1A_ADFSSamlCert` sleutel.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Toevoegen van een claimprovider in uw extensie-beleid

Als u wilt dat gebruikers zich aanmelden met een AD FS-account, moet u het account als een claimprovider definiëren. U doen dit door een eindpunt dat Azure AD B2C met communiceert op te geven. Het eindpunt biedt een set claims die worden gebruikt door Azure AD B2C om te controleren of dat een specifieke gebruiker is geverifieerd.

AD FS definiëren als een claimprovider door toe te voegen **ClaimsProvider** element in de uitbreiding beleid-bestand.

1. Open de *TrustFrameworkExtensions.xml* beleid-bestand in uw werkmap. Als u een XML-editor, moet [probeer Visual Studio Code](https://code.visualstudio.com/download), dit is een lichtgewicht editor voor meerdere platforms.
2. Voeg het volgende XML-bestand onder de **ClaimsProviders** -element en vervang **uw ADFS-domein** met uw ADFS-domein een naam geven en vervang de waarde van de **identityProvider** uitvoer claim met uw DNS (willekeurige waarde die aangeeft van uw domein) en sla het bestand. 

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
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
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

## <a name="register-the-claims-provider-for-sign-up-and-sign-in"></a>Registreren van de claimprovider voor registratie en aanmelding

Als u de AD FS-account-id-provider op de pagina's zich kunnen registreren en aanmelden, moet u deze toevoegen aan uw **SignUpOrSignIn** gebruikersbeleving. 

Maak een kopie van een bestaande sjabloon voor de gebruikersbeleving en wijzigen zodat de AD FS-id-provider zijn opgenomen:

>[!NOTE]
>Als u eerder hebt gekopieerd de **UserJourneys** element uit de base-bestand van uw beleid aan het extensiebestand (*TrustFrameworkExtensions.xml*) kunt u deze sectie overslaan.

1. Open het bestand basis van uw beleid. Bijvoorbeeld, *TrustFrameworkBase.xml*.
2. Kopieer de gehele inhoud van **UserJourneys** element.
3. Open het extensiebestand (*TrustFrameworkExtensions.xml*) en plak de volledige inhoud van **UserJourneys** element dat u in het extensiebestand hebt gekopieerd.

### <a name="display-the-button"></a>De knop weergeven

De **ClaimsProviderSelections** element wordt de lijst met claims provider selecties en de volgorde gedefinieerd.  De **ClaimsProviderSelection** element is vergelijkbaar met een knop identity provider op een pagina zich kunnen registreren en aanmelden. Als u een **ClaimsProviderSelection** element voor een AD FS-account, een nieuwe knop wordt weergegeven wanneer een gebruiker de pagina ziet. Dit element toevoegen:

1. In de **UserJourney** element met een id van `SignUpOrSignIn` Zoek in de trajecten van de gebruiker die u hebt gekopieerd, de **OrchestrationStep** element van `Order="1"`.
2. Voeg volgende **ClaimsProviderSelection** element onder de **ClaimsProviderSelections** element:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

Nu dat u een knop op locatie hebt, die u wilt koppelen aan een actie. De actie, wordt in dit geval is voor Azure AD B2C om te communiceren met AD FS-account voor het ontvangen van een token. De knop koppelen aan een actie door koppelen aan het technische profiel voor de claimprovider van uw AD FS-account:

1. Zoek de **OrchestrationStep** van `Order="2"` onder de **UserJourney** element.
2. Voeg volgende **ClaimsExchange** element onder de **ClaimsExchanges** element:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

> [!NOTE]
> * Zorg ervoor dat de `Id` heeft dezelfde waarde als `TargetClaimsExchangeId` in de vorige sectie.
> * Zorg ervoor dat de `TechnicalProfileReferenceId` is ingesteld in het technische profiel u eerdere (Contoso-SAML2) hebt gemaakt.


## <a name="optional-register-the-claims-provider-for-profile-edit"></a>[Optioneel] Registreren van de claimprovider voor profiel bewerken

U kunt ook de id-provider van de AD FS-account toevoegen aan uw profiel bewerken de gebruikersbeleving.

### <a name="display-the-button"></a>De knop weergeven

1. Open het bestand uitbreiding van uw beleid. Bijvoorbeeld, *TrustFrameworkExtensions.xml*.
2. In de **UserJourney** element met een id `ProfileEdit` Zoek in de trajecten van de gebruiker die u hebt gekopieerd, de **OrchestrationStep** element van `Order="1"`.
3. Voeg volgende **ClaimsProviderSelection** element onder **ClaimsProviderSelections** element:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>De knop koppelen aan een actie

1. Zoek de **OrchestrationStep** van `Order="2"` onder de **UserJourney** element.
2. Voeg volgende **ClaimsExchange** element onder de **ClaimsExchanges** element:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

## <a name="upload-the-policy-to-your-tenant"></a>Uploaden van het beleid aan uw tenant

1. Selecteer in de Azure portal, **alle beleidsregels**.
2. Selecteer **beleid uploaden**.
3. Schakel **het beleid overschrijven als deze bestaat**.
4. Blader naar en selecteer uw *TrustFrameworkExtensions.xml* beleid-bestand en selecteer vervolgens **uploaden**. Zorg ervoor dat de validatie voltooid is.


## <a name="configure-an-adfs-relying-party-trust"></a>Een AD FS Relying Party-vertrouwensrelatie configureren

Voor het gebruik van ADFS als id-provider in Azure AD B2C, moet u een AD FS Relying Party Trust maken met de Azure AD B2C-SAML-metagegevens. Het volgende voorbeeld ziet u een URL-adres in de SAML-metagegevens van het technische profiel van een Azure AD B2C:

```
https://login.microsoftonline.com/te/your-tenant/your-policy/samlp/metadata?idptp=your-technical-profile
```

Vervang de volgende waarden:

- **uw tenant** met de naam van uw tenant, zoals uw tenant.onmicrosoft.com.
- **uw beleid** met de naam van uw beleid. Gebruik het beleid dat is waar u het technische SAML-provider-profiel configureren, of een beleidsregel die eigenschappen overneemt van dat beleid.
- **uw op technisch profiel** met de naam van uw id-provider technische SAML-profiel.
 
Open een browser en navigeer naar de URL. Controleer of u typt u de juiste URL en dat u toegang tot de XML-bestand met metagegevens hebt.

Als u wilt toevoegen van een nieuwe relying party trust met behulp van de AD FS-beheermodule en de instellingen handmatig configureren, moet u de volgende procedure uitvoeren op een federatieserver. Lidmaatschap van **beheerders** of gelijkwaardig op de lokale computer is de minimale vereiste om deze procedure te voltooien. Lees de informatie over het gebruik van de juiste accounts en groepslidmaatschappen op [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477).

1. Selecteer in Serverbeheer **extra**, en selecteer vervolgens **AD FS Management**.
2. Selecteer **toevoegen vertrouwensrelatie van Relying Party**.
3. Op de **Welkom** pagina, kies **Claims bewuste**, en klik vervolgens op **Start**.
4. Op de **gegevensbron selecteren** pagina, selecteert u **importeren van gegevens over de relying party gepubliceerd online of op een lokaal netwerk**, geef de URL van uw Azure AD B2C-metagegevens en klik vervolgens op **volgende**.
5. Op de **weergavenaam opgeven** pagina een **weergavenaam**onder **opmerkingen bij de**, voer een beschrijving voor deze relying partyvertrouwensrelatie, en klik vervolgens op **volgende**.
6. Op de **kiezen toegangscontrolebeleid** pagina, selecteer een beleid en klik vervolgens op **volgende**.
7. Op de **gereed voor toevoegen vertrouwensrelatie** pagina, Controleer de instellingen en klik vervolgens op **volgende** om op te slaan, uw relying party trust informatie.
8. Op de **voltooien** pagina, klikt u op **sluiten**, deze actie wordt automatisch de **Claimregels bewerken** in het dialoogvenster.
9. Selecteer **regel toevoegen**.  
10. In **claimregelsjabloon**, selecteer **verzenden LDAP-kenmerken als claims**.
11. Geef een **naam van Claimregel**. Voor de **kenmerkopslag**, selecteer **Active Directory selecteren**, voegt u de volgende claims en klik vervolgens op **voltooien** en **OK**.

    ![Eigenschappen van de regel instellen](./media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)

12.  Op basis van het certificaattype, wellicht u stelt het HASH-algoritme. Selecteer op de relying party trust (B2C-Demo) eigenschappenvenster, de **Geavanceerd** tabblad en wijzig de **veilige hash-algoritme** naar `SHA-1` of `SHA-256`, en klikt u op **Ok**.  

### <a name="update-the-relying-party-metadata"></a>De metagegevens van de relying party bijwerken

Wijzigen van het technische SAML-profiel, moet u AD FS bijwerken met de versie van de bijgewerkte metagegevens. U hoeft niet te worden de metagegevens bijwerken bij het maken van de relying party-toepassing, maar wanneer u een wijziging aanbrengt, bij te werken van de metagegevens in AD FS.

1. Selecteer in Serverbeheer **extra**, en selecteer vervolgens **AD FS Management**.
2. Selecteer de relying party trust die u hebt gemaakt, selecteer **Update van Federatiemetagegevens**, en klik vervolgens op **Update**. 

### <a name="test-the-policy-by-using-run-now"></a>Het beleid testen met behulp van nu uitvoeren

1.  Open **Azure AD B2C-instellingen** en Ga naar **Identity-Ervaringsframework**.
2.  Open **B2C_1A_ProfileEdit**, de relying party (RP) aangepast-beleid dat u hebt geüpload. Selecteer **nu uitvoeren**. U moet aanmelden met AD FS-account.

## <a name="download-the-complete-policy-files"></a>De volledige bestanden downloaden

Optioneel: U kunt uw scenario met behulp van uw eigen aangepaste beleidsbestanden na het voltooien van de stappen in bouwen [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md). Bijvoorbeeld bestanden, Zie [voorbeeldbestanden beleid alleen ter informatie](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app).
