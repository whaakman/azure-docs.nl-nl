---
title: Migratie van gebruikers nadert in Azure Active Directory B2C | Microsoft Docs
description: Bespreken core en geavanceerde concepten voor gebruikersmigratie met behulp van Graph API en eventueel met aangepaste beleidsregels voor Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7805b238d42201b791e038964985f784fcf8d4ce
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054869"
---
# <a name="azure-active-directory-b2c-user-migration"></a>Azure Active Directory B2C: Gebruikersmigratie
Wanneer u uw id-provider migreert naar Azure Active Directory B2C (Azure AD B2C), moet u mogelijk ook het gebruikersaccount te migreren. In dit artikel wordt uitgelegd hoe u bestaande gebruikersaccounts migreren van een id-provider naar Azure AD B2C. Het artikel is niet bedoeld om te worden duidelijk, maar in plaats daarvan worden enkele scenario's beschreven. De ontwikkelaar is verantwoordelijk voor de geschiktheid van elke methode.

## <a name="user-migration-flows"></a>Migratie-gebruikersstromen
Met Azure AD B2C, kunt u gebruikers via migreren [Azure AD Graph API][B2C-GraphQuickStart]. Het migratieproces van de gebruiker worden onderverdeeld in twee stromen:

* **Vóór de migratie**: met deze stroom is van toepassing wanneer u duidelijk toegang tot de referenties van een gebruiker (gebruikersnaam en wachtwoord) hebben of de referenties zijn versleuteld, maar kunt u ze kunt ontsleutelen. Het proces vóór de migratie omvat het lezen van de gebruikers van de oude id-provider en het maken van nieuwe accounts in de Azure AD B2C-directory.

* **Vóór de migratie en het wachtwoord opnieuw instellen**: met deze stroom is van toepassing wanneer het wachtwoord van een gebruiker niet toegankelijk is. Bijvoorbeeld:
    * Het wachtwoord wordt opgeslagen in de hash-indeling.
    * Het wachtwoord wordt opgeslagen in een id-provider die u geen toegang krijgt. Uw oude id-provider valideert de referenties van de gebruiker door een webservice aan te roepen.

In beide stromen u eerst het proces vóór de migratie uitvoeren, lezen van de gebruikers van uw oude id-provider en nieuwe accounts maken in de Azure AD B2C-directory. Als u het wachtwoord hebt, kunt u het account maken met behulp van een wachtwoord dat willekeurig wordt gegenereerd. Vervolgens vraagt u de gebruiker het wachtwoord te wijzigen of wanneer de gebruiker zich aanmeldt voor de eerste keer, de gebruiker opnieuw in te stellen in Azure AD B2C wordt gevraagd.

## <a name="password-policy"></a>Wachtwoordbeleid
Het wachtwoordbeleid van Azure AD B2C (voor lokale accounts) is gebaseerd op Azure Active Directory-beleid. De Azure AD B2C registreren of aanmelden en het wachtwoord opnieuw instellen van beleidsregels gebruikt de sterkte 'sterk' wachtwoord en wachtwoorden niet verloopt. Zie voor meer informatie, [Azure AD-wachtwoordbeleid][AD-PasswordPolicies].

Als de accounts die u wilt migreren gebruikt u een zwakkere Wachtwoordsterkte dan de [sterke Wachtwoordsterkte afgedwongen door Azure AD B2C][AD-PasswordPolicies], kunt u de vereiste sterk wachtwoord uitschakelen. Als u wilt het standaardbeleid voor wachtwoord wijzigen, stelt de `passwordPolicies` eigenschap `DisableStrongPassword`. Bijvoorbeeld, kunt u de aanvraag voor het maken van gebruiker als volgt wijzigen:

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-azure-ad-graph-api-to-migrate-users"></a>Stap 1: Gebruik Azure AD Graph API om gebruikers te migreren
U maakt het Azure AD B2C-gebruikersaccount via Graph API (met het wachtwoord of met een willekeurig wachtwoord). Deze sectie beschrijft het proces voor het maken van gebruikersaccounts in de Azure AD B2C-directory met behulp van Graph API.

### <a name="step-11-register-your-application-in-your-tenant"></a>Stap 1.1: Registreer uw toepassing in uw tenant
Om te communiceren met de Graph API, moet u eerst een service-account met beheerdersbevoegdheden hebt. In Azure AD registreert u een toepassing en verificatie met Azure AD. De referenties van toepassing zijn **toepassings-ID** en **Toepassingsgeheim**. De toepassing fungeert als zelf, niet als een gebruiker, de Graph-API aan te roepen.

Uw migratietoepassing eerst registreren in Azure AD. Vervolgens een Toepassingssleutel (toepassingsgeheim) maken en de toepassing met schrijfmachtigingen instellen.

1. Meld u aan bij [Azure Portal][Portal].

2. Kies uw Azure AD **B2C** tenant door het selecteren van uw account aan de bovenkant van het venster.

3. Selecteer in het linkerdeelvenster **Azure Active Directory** (geen Azure AD B2C). Om te zoeken, moet u mogelijk selecteren **meer Services**.

4. Selecteer **App-registraties**.

5. Selecteer **Nieuwe toepassing registreren**.

    ![Nieuwe toepassing registreren](media/active-directory-b2c-user-migration/pre-migration-app-registration.png)

6. Maak een nieuwe toepassing door de volgende te doen:
    * Voor **naam**, gebruikt u **B2CUserMigration** of een andere naam die u wilt.
    * Voor **toepassingstype**, gebruikt u **Web-app/API**.
    * Voor **aanmeldings-URL**, gebruikt u **https://localhost** (zoals dit niet relevant zijn voor deze toepassing is).
    * Selecteer **Maken**.

7. Nadat de toepassing is gemaakt, in de **toepassingen** , selecteert u het zojuist gemaakte **B2CUserMigration** toepassing.

8. Selecteer **eigenschappen**, Kopieer de **toepassings-ID**, en bewaar het voor later.

### <a name="step-12-create-the-application-secret"></a>Stap 1.2: De toepassing het clientgeheim maken
1. In de Azure-portal **App geregistreerd** venster **sleutels**.

2. Voeg een nieuwe sleutel (ook wel bekend als een clientgeheim) en kopieer vervolgens de sleutel voor later gebruik.

    ![Toepassings-ID en -sleutels](media/active-directory-b2c-user-migration/pre-migration-app-id-and-key.png)

### <a name="step-13-grant-administrative-permission-to-your-application"></a>Stap 1.3: Een beheerdersmachtiging nodig voor uw toepassing verlenen
1. In de Azure-portal **App geregistreerd** venster **vereiste machtigingen**.

2. Selecteer **Windows Azure Active Directory**.

3. In de **toegang inschakelen** deelvenster onder **Toepassingsmachtigingen**, selecteer **mapgegevens lezen en schrijven**, en selecteer vervolgens **opslaan**.

4. In de **vereiste machtigingen** venster **machtigingen verlenen**.

    ![Machtigingen van de toepassing](media/active-directory-b2c-user-migration/pre-migration-app-registration-permissions.png)

U hebt nu een toepassing met machtigingen voor het maken, lezen en bijwerken van gebruikers van uw Azure AD B2C-tenant.

### <a name="step-14-optional-environment-cleanup"></a>Stap 1.4: (Optioneel) omgeving opschonen
Lezen en schrijven gegevens mapmachtigingen doen *niet* omvatten het recht om gebruikers te verwijderen. Om te geven van uw toepassing de mogelijkheid om gebruikers (voor het opschonen van uw omgeving) te verwijderen, moet u een extra stap die wordt uitgevoerd van PowerShell om in te stellen van de beheerder van gebruikersaccounts machtigingen uitvoeren. Anders kunt u doorgaan met de volgende sectie.

> [!IMPORTANT]
> Moet u een B2C-tenant administrator-account dat is *lokale* voor de B2C-tenant. De syntaxis van de naam van het account is *admin@contosob2c.onmicrosoft.com*.

>[!NOTE]
> De volgende PowerShell-script vereist [Azure Active Directory PowerShell versie 2][AD-Powershell].

In dit PowerShell-script, het volgende doen:
1. Verbinding maken met uw online service. Om dit te doen, voer de `Connect-AzureAD` achter de Windows PowerShell-cmdlet opdrachtprompt, en geef uw referenties.

2. Gebruik de **toepassings-ID** toewijzen de toepassing de gebruikersrol administrator-account. Deze rollen zijn bekende id's, dus u hoeft alleen Voer uw **toepassings-ID** in het script.

```PowerShell
Connect-AzureAD

$AppId = "<Your application ID>"

# Fetch Azure AD application to assign to role
$roleMember = Get-AzureADServicePrincipal -Filter "AppId eq '$AppId'"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add application to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
```

Wijzig de `$AppId` waarde met uw Azure AD **toepassings-ID**.

## <a name="step-2-pre-migration-application-sample"></a>Stap 2: Vóór de migratie-voorbeeldtoepassing
[Downloaden en uitvoeren van de voorbeeldcode][UserMigrationSample]. U kunt dit downloaden als ZIP-bestand.

### <a name="step-21-edit-the-migration-data-file"></a>Stap 2.1: Bewerk het bestand met migratiegegevens
De voorbeeld-app maakt gebruik van een JSON-bestand met gegevens van de dummygebruiker. Nadat u het voorbeeld met succes is uitgevoerd, kunt u de code voor de gegevens uit uw eigen database gebruiken. Of u kunt het gebruikersprofiel exporteren naar een JSON-bestand en stel vervolgens de app om dat bestand te gebruiken.

Als u wilt het JSON-bestand bewerken, opent u de `AADB2C.UserMigration.sln` Visual Studio-oplossing. In de `AADB2C.UserMigration` project, open de `UsersData.json` bestand.

![Gegevensbestand van de gebruiker](media/active-directory-b2c-user-migration/pre-migration-data-file.png)

Zoals u ziet, betekent dit dat het bestand een lijst met gebruikersentiteiten bevat. Elke gebruikersentiteit heeft de volgende eigenschappen:
* e-mailen
* displayName
* Voornaam
* lastName
* wachtwoord (mag leeg zijn)

> [!NOTE]
> Bij het compileren, Visual Studio het bestand kopieert de `bin` directory.

### <a name="step-22-configure-the-application-settings"></a>Stap 2.2: De toepassingsinstellingen configureren
Onder de `AADB2C.UserMigration` project, open de *App.config* bestand. Vervang de volgende appinstellingen door uw eigen waarden:

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Client Secret Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
</appSettings>
```

> [!NOTE]
> * Het gebruik van de verbindingsreeks voor een Azure-tabel wordt beschreven in de volgende secties.
> * De naam van uw B2C-tenant is het domein dat u hebt opgegeven tijdens het maken van de tenant, deze wordt weergegeven in de Azure-portal. Naam van de tenant heeft meestal het achtervoegsel *. onmicrosoft.com* (bijvoorbeeld *contosob2c.onmicrosoft.com*).
>

### <a name="step-23-run-the-pre-migration-process"></a>Stap 2.3: Het proces vóór de migratie uitvoeren
Met de rechtermuisknop op de `AADB2C.UserMigration` oplossing en klik vervolgens opnieuw maken in het voorbeeld. Als u geslaagde, u hebt nu een `UserMigration.exe` uitvoerbare bestand zich bevindt in `AADB2C.UserMigration\bin\Debug\net461`. Voor het uitvoeren van het migratieproces, moet u een van de volgende opdrachtregelparameters gebruiken:

* Naar **migreren van gebruikers met een wachtwoord**, gebruikt u de `UserMigration.exe 1` opdracht.

* Naar **migreren van gebruikers met een willekeurig wachtwoord**, gebruikt u de `UserMigration.exe 2` opdracht. Met deze bewerking maakt ook een Azure-Tabelentiteit. Later configureert u het beleid voor het aanroepen van de REST-API-service. De service maakt gebruik van een Azure-tabel om te houden en beheren van het migratieproces.

![Demo voor migratie van proces](media/active-directory-b2c-user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>Stap 2.4: Het proces vóór de migratie controleren
Gebruik een van de volgende twee methoden voor het valideren van de migratie:

* Als u wilt zoeken naar een gebruiker op weergavenaam, gebruikt u de Azure-portal:

    a. Open **Azure AD B2C**, en selecteer vervolgens **gebruikers en groepen**.

    b. In het zoekvak typt u de weergavenaam van de gebruiker en bekijk vervolgens het profiel van de gebruiker.

* Als u wilt ophalen van een gebruiker op aanmelden e-mailadres, gebruikt u deze voorbeeldtoepassing:

    a. Voer de volgende opdracht uit:

    ```Console
        UserMigration.exe 3 {email address}
    ```

    > [!TIP]
    > U kunt ook een gebruiker op weergavenaam ophalen met behulp van de volgende opdracht uit: `UserMigration.exe 4 "<Display name>"`.

    b. Open het bestand UserProfile.json in een JSON-editor om te zien van de gegevens van gebruiker.

    ![Het bestand UserProfile.json](media/active-directory-b2c-user-migration/pre-migration-get-by-email2.png)

### <a name="step-25-optional-environment-cleanup"></a>Stap 2.5: (Optioneel) omgeving opschonen
Als u wilt opschonen-up van uw Azure AD-tenant en gebruikers verwijderen uit de Azure AD-directory, voer de `UserMigration.exe 5` opdracht.

> [!NOTE]
> * Als u wilt opschonen van uw tenant, beheerder van gebruikersaccounts machtigingen configureren voor uw toepassing.
> * De voorbeeld-app voor migratie opschonen van alle gebruikers die worden vermeld in het JSON-bestand.

### <a name="step-26-sign-in-with-migrated-users-with-password"></a>Stap 2.6: Aanmelden bij gemigreerde gebruikers (met een wachtwoord)
Nadat u het proces vóór de migratie uitvoert met wachtwoorden van gebruikers, de accounts zijn klaar voor gebruik en gebruikers kunnen zich aanmelden bij uw toepassing met behulp van Azure AD B2C. Als u geen toegang tot de wachtwoorden van gebruikers, gaat u verder met de volgende sectie.

## <a name="step-3-help-users-reset-their-password"></a>Stap 3: Kunnen gebruikers hun wachtwoord opnieuw instellen
Als u gebruikers met een willekeurig wachtwoord migreert, moeten ze hun wachtwoord opnieuw instellen. Verzenden om hen te helpen het wachtwoord opnieuw instellen, een welkomstbericht per e-mail met een koppeling naar het wachtwoord opnieuw instellen.

Als u de koppeling naar uw wachtwoord opnieuw instellen van beleid, het volgende doen:

1. Selecteer **Azure AD B2C-instellingen**, en selecteer vervolgens **wachtwoord opnieuw instellen** eigenschappen van beleid.

2. Selecteer uw toepassing.

    > [!NOTE]
    > Voer nu vereist dat ten minste één toepassing vooraf op de tenant worden geregistreerd. Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag] [ B2C-GetStarted] artikel of de [toepassingsregistratie] [ B2C-AppRegister] artikel.

3. Selecteer **nu uitvoeren**, en controleer vervolgens of het beleid.

4. In de **eindpunt nu uitvoeren** vak, Kopieer de URL en vervolgens naar uw gebruikers te verzenden.

    ![Logboeken met diagnostische gegevens instellen](media/active-directory-b2c-user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-the-user-migration-status"></a>Stap 4: (Optioneel) Wijzig het beleid om te controleren en instellen van de migratie van gebruikersstatus

> [!NOTE]
> Als u wilt controleren en wijzigen van de gebruikersstatus van de migratie, moet u een aangepast beleid gebruiken. De installatie-instructies uit [aan de slag met aangepaste beleidsregels] [ B2C-GetStartedCustom] moeten worden uitgevoerd.
>

Wanneer gebruikers proberen aan te melden bij zonder eerst het wachtwoord opnieuw instellen, moet uw beleid een beschrijvende foutbericht retourneren. Bijvoorbeeld:
>*Uw wachtwoord is verlopen. Om deze te herstellen, selecteert u de koppeling wachtwoord opnieuw instellen.*

Deze optionele stap vereist het gebruik van aangepaste Azure AD B2C-beleid, zoals beschreven in de [aan de slag met aangepaste beleidsregels] [ B2C-GetStartedCustom] artikel.

In deze sectie maakt u het beleid om te controleren of de migratiestatus van de gebruiker op aanmelden. Als de gebruiker het wachtwoord niet wijzigen, retourneren een HTTP 409 foutbericht weergegeven waarin u wordt gevraagd de gebruiker selecteert de **wachtwoord vergeten?** koppeling.

Als u wilt wijzigen van het wachtwoord bijhouden, moet u een Azure-tabel gebruiken. Wanneer u het proces vóór de migratie uitvoert met de opdrachtregelparameter `2`, u een gebruikersentiteit maken in een Azure-tabel. Uw service doet het volgende:

* Het Azure AD B2C-beleid roept op aanmelden, migratie van uw RESTful-service, een e-mailbericht verzenden als een invoer claim. De service gezocht naar het e-mailadres in de Azure-tabel. Als het adres bestaat, de service genereert een foutbericht weergegeven: *moet u wachtwoord wijzigen*.

* Nadat de gebruiker is het wachtwoord wijzigt, moet u de entiteit verwijderen uit de Azure-tabel.

>[!NOTE]
>Voor het vereenvoudigen van het voorbeeld gebruiken we een Azure-tabel. U kunt de migratiestatus van de opslaan in een database of als een aangepaste eigenschap in de Azure AD B2C-account.

### <a name="41-update-your-application-setting"></a>4.1: de toepassingsinstelling van de bijwerken
1. Als u wilt testen van de RESTful-API-demo, open `AADB2C.UserMigration.sln` in Visual Studio.

2. In de `AADB2C.UserMigration.API` project, open de *appsettings.json* bestand. De instelling vervangen door het geconfigureerd in [stap 2.2](#step-22-configure-the-application-settings):

    ```json
    {
        "BlobStorageConnectionString": "{The Azure Blob storage connection string}",
        ...
    }
    ```

### <a name="step-42-deploy-your-web-application-to-azure-app-service"></a>Stap 4.2: Uw web-App in Azure App Service implementeren
Klik in Solution Explorer met de rechtermuisknop op de `AADB2C.UserMigration.API`, selecteert u 'Publiceren...'. Volg de instructies voor het publiceren naar Azure App Service. Zie voor meer informatie, [uw app implementeren in Azure App Service][AppService-Deploy].

### <a name="step-43-add-a-technical-profile-and-technical-profile-validation-to-your-policy"></a>Stap 4.3: Een technisch profiel en de validatie van het technische profiel toevoegen aan uw beleid
1. Klik in Solution Explorer 'Oplossingsitems' uitvouwen en open de *TrustFrameworkExtensions.xml* beleid-bestand.
2. Wijziging `TenantId`, `PublicPolicyUri` en `<TenantId>` velden uit `yourtenant.onmicrosoft.com` op de naam van uw tenant.
3. Onder de `<TechnicalProfile Id="login-NonInteractive">` -element, Vervang alle exemplaren van `ProxyIdentityExperienceFrameworkAppId` en `IdentityExperienceFrameworkAppId` met de toepassings-id's geconfigureerd in [aan de slag met aangepaste beleidsregels][B2C-GetStartedCustom].
4. Onder de `<ClaimsProviders>` knooppunt, vinden de volgende XML-fragment. Wijzig de waarde van `ServiceUrl` om te verwijzen naar uw Azure App Service-URL.

    ```XML
    <ClaimsProvider>
      <DisplayName>REST APIs</DisplayName>
      <TechnicalProfiles>

        <TechnicalProfile Id="LocalAccountSignIn">
          <DisplayName>Local account just in time migration</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/LoalAccountSignIn</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="email" />
          </InputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>

        <TechnicalProfile Id="LocalAccountPasswordReset">
          <DisplayName>Local account just in time migration</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/PasswordUpdated</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
          </InputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

De voorgaande technisch profiel definieert een invoer claim: `signInName` (verzenden als e-mailadres). Op aanmelden, wordt de claim verzonden naar uw RESTful-eindpunt.

Nadat u het technische profiel gedefinieerd voor uw RESTful-API, laat u uw Azure AD B2C-beleid voor het aanroepen van het technische profiel. De XML-fragment onderdrukt `SelfAsserted-LocalAccountSignin-Email`, die is gedefinieerd in het Basisbeleid. De XML-fragment wordt ook toegevoegd `ValidationTechnicalProfile`, met ReferenceId die verwijst naar het technische profiel `LocalAccountUserMigration`.

### <a name="step-44-upload-the-policy-to-your-tenant"></a>Stap 4.4: Het beleid voor uploaden naar uw tenant
1. In de [Azure-portal][Portal], Ga naar de [context van uw Azure AD B2C-tenant][B2C-NavContext], en selecteer vervolgens **Azure AD B2C**.

2. Selecteer **Identity-Ervaringsframework**.

3. Selecteer **alle beleidsregels**.

4. Selecteer **beleid uploaden**.

5. Selecteer de **het beleid overschrijven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkExtensions.xml* -bestand en zorg ervoor dat deze de validatietests doorstaat.

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a>Stap 4.5: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Selecteer **Azure AD B2C-instellingen**, en ga vervolgens naar **Identity-Ervaringsframework**.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast beleid u geüpload en selecteer vervolgens **nu uitvoeren**.

3. Meld u aan met een van de gemigreerde gebruikers referenties op en selecteer vervolgens **aanmelden**. De REST-API moet genereert de volgende strekking weergegeven:

    ![Logboeken met diagnostische gegevens instellen](media/active-directory-b2c-user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshoot-your-rest-api"></a>Stap 4.6: (Optioneel) voor het oplossen van de REST-API
U kunt bekijken en controleren van logboekgegevens in near-real-time.

1. Menu instellingen van uw RESTful toepassing onder **bewaking**, selecteer **diagnostische logboeken**.

2. Stel **toepassingslogboeken (bestandssysteem)** naar **op**.

3. Stel de **niveau** naar **uitgebreide**.

4. Selecteer **opslaan**

    ![Logboeken met diagnostische gegevens instellen](media/active-directory-b2c-user-migration/pre-migration-diagnostic-logs.png)

5. Op de **instellingen** in het menu **logboekstream**.

6. Controleer de uitvoer van de RESTful-API.

> [!IMPORTANT]
> Gebruik de diagnostische logboeken alleen tijdens het ontwikkelen en testen. De uitvoer van de RESTful-API mogelijk vertrouwelijke informatie die niet moet worden weergegeven in de productieomgeving.
>

## <a name="optional-download-the-complete-policy-files"></a>(Optioneel) De volledige bestanden downloaden
Na het voltooien van de [aan de slag met aangepaste beleidsregels] [ B2C-GetStartedCustom] scenario, het is raadzaam dat u uw scenario bouwen met behulp van uw eigen aangepaste beleidsbestanden. Ter referentie, we hebben opgegeven [beleid voorbeeldbestanden][UserMigrationSample].

[AD-PasswordPolicies]: https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy
[AD-Powershell]: https://docs.microsoft.com/powershell/azure/active-directory/install-adv2
[AppService-Deploy]: https://docs.microsoft.com/aspnet/core/tutorials/publish-to-azure-webapp-using-vs
[B2C-AppRegister]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration
[B2C-GetStarted]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started
[B2C-GetStartedCustom]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom
[B2C-GraphQuickStart]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet
[B2C-NavContext]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-navigate-to-b2c-context
[Portal]: https://portal.azure.com/
[UserMigrationSample]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration