---
title: 'Azure Active Directory B2C: Gebruiker migratie benaderingen'
description: Bespreek core en geavanceerde concepten voor gebruikersmigratie met Graph API en eventueel Azure AD B2C aangepast beleid.
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
ms.date: 10/04/2017
ms.author: yoelh
ms.openlocfilehash: 25023359e3f1eeb241f6f0e70bcb179aa32974af
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-user-migration"></a>Azure Active Directory B2C: Migratie gebruiker
Wanneer u uw identiteitsprovider migreert naar Azure Active Directory B2C (Azure AD B2C), moet u mogelijk ook het migreren van het gebruikersaccount. In dit artikel wordt uitgelegd hoe u bestaande gebruikersaccounts migreert van een identiteitsprovider naar Azure AD B2C. Het artikel is niet bedoeld als richtlijnen, maar in plaats daarvan deze twee verschillende manieren worden beschreven. De ontwikkelaar is verantwoordelijk voor de geschiktheid van elke methode.

## <a name="user-migration-flows"></a>Gebruiker migratiestromen
Met Azure AD B2C, kunt u gebruikers via migreren [Graph API](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet). Het migratieproces van de gebruiker worden onderverdeeld in twee stromen:

* **Vóór de migratie**: deze stroom is van toepassing wanneer u ofwel wissen toegang tot de referenties van een gebruiker (gebruikersnaam en wachtwoord hebben) of de referenties zijn versleuteld, maar u kunt ze ontsleutelen. Het proces vóór de migratie omvat het lezen van de gebruikers van de oude id-provider en het maken van nieuwe accounts in de Azure AD B2C-directory.

* **Vóór de migratie en het wachtwoord opnieuw instellen**: deze stroom is van toepassing wanneer het wachtwoord van een gebruiker niet toegankelijk is. Bijvoorbeeld:
    * Het wachtwoord wordt opgeslagen in de hash-indeling.
    * Het wachtwoord wordt opgeslagen in een id-provider die u niet kunt openen. Uw oude identiteitsprovider valideert de gebruikersreferenties door het aanroepen van een webservice.

In beide stroomt u eerst het proces vóór de migratie uitvoert, de gebruikers van uw oude identiteitsprovider lezen en nieuwe accounts in de Azure AD B2C-directory maken. Als u het wachtwoord niet hebt, kunt u het account maken met een wachtwoord dat willekeurig wordt gegenereerd. Vervolgens vraagt u de gebruiker het wachtwoord te wijzigen of wanneer de gebruiker zich aanmeldt voor de eerste keer, Azure AD B2C wordt gevraagd of de gebruiker opnieuw in te stellen.

## <a name="password-policy"></a>Wachtwoordbeleid
Het Azure AD B2C-wachtwoordbeleid (voor lokale accounts) is gebaseerd op Azure Active Directory-beleid. De Azure AD B2C registreren of aanmelden en het wachtwoord opnieuw instellen van beleidsregels gebruiken de sterkte 'sterk' wachtwoord en wachtwoorden niet verloopt. Zie voor meer informatie [Azure AD-wachtwoordbeleid](https://msdn.microsoft.com/library/azure/jj943764.aspx).

Als de accounts die u wilt migreren, een zwakkere Wachtwoordsterkte dan gebruiken de [sterk Wachtwoordsterkte afgedwongen door Azure AD B2C](https://msdn.microsoft.com/library/azure/jj943764.aspx), kunt u de vereiste sterke wachtwoorden uitschakelen. Als u wilt het standaardbeleid wachtwoorden wijzigen, de `passwordPolicies` eigenschap `DisableStrongPassword`. U kunt de aanvraag voor het maken van gebruiker bijvoorbeeld als volgt wijzigen: 

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-graph-api-to-migrate-users"></a>Stap 1: Gebruik Graph API om gebruikers te migreren
U kunt de Azure AD B2C-gebruikersaccount via Graph API maken (het wachtwoord of met een willekeurig wachtwoord). Deze sectie beschrijft het proces van het maken van gebruikersaccounts in de Azure AD B2C-directory met behulp van Graph API.

### <a name="step-11-register-your-application-in-your-tenant"></a>Stap 1.1: Uw toepassing registreren in uw tenant
Om te communiceren met de Graph API, moet u eerst een service-account met beheerdersbevoegdheden hebben. In Azure AD registreert u een toepassing en verificatie met Azure AD. De referenties van toepassing zijn **toepassings-ID** en **Toepassingsgeheim**. De toepassing fungeert als zichzelf niet als een gebruiker, de API van de grafiek aan te roepen.

De migratietoepassing eerst registreren in Azure AD. Vervolgens maakt u een Toepassingssleutel (toepassingsgeheim) en de toepassing met schrijfbevoegdheden instellen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Kies uw Azure AD **B2C** tenant door het selecteren van uw account aan de bovenkant van het venster.

3. Selecteer in het linkerdeelvenster **Azure Active Directory** (geen Azure AD B2C). Als u wilt vinden, moet u mogelijk selecteren **meer Services**.

4. Selecteer **App registraties**.

5. Selecteer **registratie van de nieuwe toepassing**.

    ![Nieuwe toepassing registreren](media/active-directory-b2c-user-migration/pre-migration-app-registration.png)

6. Maak een nieuwe toepassing als volgt:
    * Voor **naam**, gebruik **B2CUserMigration** of elke gewenste naam.
    * Voor **toepassingstype**, gebruik **Web-app/API**.
    * Voor **aanmeldings-URL**, gebruik **https://localhost** (zoals dit niet relevant zijn voor deze toepassing is).
    * Selecteer **Maken**.

7. Nadat de toepassing is gemaakt, in de **toepassingen** , selecteert u het zojuist gemaakte **B2CUserMigration** toepassing.

8. Selecteer **eigenschappen**, Kopieer de **toepassings-ID**, en op te slaan voor later.

### <a name="step-12-create-the-application-secret"></a>Stap 1.2: Het toepassingsgeheim maken
1. In de Azure portal **App geregistreerd** Selecteer **sleutels**.

2. Voeg een nieuwe sleutel (ook wel bekend als een clientgeheim) en kopieer de sleutel voor later gebruik.

    ![Toepassings-ID en -sleutels](media/active-directory-b2c-user-migration/pre-migration-app-id-and-key.png)

### <a name="step-13-grant-administrative-permission-to-your-application"></a>Stap 1.3: Toestemming beheerdersrechten voor uw toepassing
1. In de Azure portal **App geregistreerd** Selecteer **vereist machtigingen**.

2. Selecteer **Windows Azure Active Directory**.

3. In de **toegang inschakelen** deelvenster onder **Toepassingsmachtigingen**, selecteer **lezen en schrijven directorygegevens**, en selecteer vervolgens **opslaan**.

4. In de **vereist machtigingen** deelvenster **machtiging verlenen**.

    ![Toepassingsmachtigingen](media/active-directory-b2c-user-migration/pre-migration-app-registration-permissions.png)

U hebt nu een toepassing met machtigingen voor het maken, lezen en bijwerken van gebruikers van uw Azure AD B2C-tenant.

### <a name="step-14-optional-environment-cleanup"></a>Stap 1.4: (Optioneel) omgeving opschonen
Lezen en schrijven van gegevens mapmachtigingen doen *niet* omvatten het recht om gebruikers te verwijderen. Om uw toepassing de mogelijkheid om gebruikers (voor het opschonen van uw omgeving) te verwijderen, moet u een extra stap waarbij wordt uitgevoerd, PowerShell accountbeheerder gebruiker machtigingen instellen uitvoeren. Anders kunt u doorgaan met de volgende sectie.

> [!IMPORTANT]
> Moet u een B2C-tenant administrator-account dat is *lokale* aan de B2C-tenant. De syntaxis van de naam van account is  *admin@contosob2c.onmicrosoft.com* .

>[!NOTE]
> De volgende PowerShell-script vereist [Azure Active Directory PowerShell versie 2](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0).

In deze PowerShell-script wordt het volgende doen:
1. Verbinding maken met uw online service. Voer hiertoe de `Connect-AzureAD` cmdlet achter de Windows PowerShell opdrachtprompt en geef uw referenties. 

2. Gebruik de **toepassings-ID** de toepassing van de gebruikersrol van de account-beheerder toewijzen. Deze rollen hebben een bekende id's, hoeft u is, Voer uw **toepassings-ID** in het script.

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

## <a name="step-2-pre-migration-application-sample"></a>Stap 2: De toepassing vóór de migratie-voorbeeld
[Downloaden en uitvoeren van de voorbeeldcode](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). U kunt dit downloaden als ZIP-bestand.

### <a name="step-21-edit-the-migration-data-file"></a>Stap 2.1: Het bestand met migratiegegevens bewerken
De voorbeeld-app maakt gebruik van een JSON-bestand dat dummy gebruikergegevens bevat. Nadat u het voorbeeld met succes is uitgevoerd, kunt u de code voor het verbruik van de gegevens van uw eigen database wijzigen. Of u kunt het gebruikersprofiel exporteren naar een JSON-bestand en stel vervolgens de app te gebruiken dat bestand.

Het JSON-bestand bewerken, opent u de `AADB2C.UserMigration.sln` Visual Studio-oplossing. In de `AADB2C.UserMigration` project, open de `UsersData.json` bestand.

![Gegevensbestand van de gebruiker](media/active-directory-b2c-user-migration/pre-migration-data-file.png)

Zoals u ziet, is het bestand bevat een lijst van entiteiten van de gebruiker. Elke gebruikersentiteit heeft de volgende eigenschappen:
* e-mailen
* Weergavenaam
* Voornaam
* Achternaam
* wachtwoord (kan niet leeg zijn)

> [!NOTE]
> Tijdens de compilatie, kopieert de Visual Studio het bestand naar de `bin` directory.

### <a name="step-22-configure-the-application-settings"></a>Step 2.2: De toepassing configureren
Onder de `AADB2C.UserMigration` project, open de *App.config* bestand. De volgende appinstellingen vervangen door uw eigen waarden:

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
    
</appSettings>
```

> [!NOTE]
> * Het gebruik van een verbindingsreeks voor Azure-tabel wordt in de volgende secties beschreven.
> * De naam van uw B2C-tenant is het domein dat u hebt opgegeven tijdens het maken van de tenant en deze wordt weergegeven in de Azure-portal. Naam van de tenant heeft meestal het achtervoegsel *. onmicrosoft.com* (bijvoorbeeld *contosob2c.onmicrosoft.com*).
>

### <a name="step-23-run-the-pre-migration-process"></a>Stap 2.3: Het proces vóór de migratie uitvoeren
Met de rechtermuisknop op de `AADB2C.UserMigration` oplossing en klik vervolgens opnieuw maken in het voorbeeld. Als u geslaagde, u hebt nu een `UserMigration.exe` uitvoerbare bestand zich in `AADB2C.UserMigration\bin\Debug`. Voor het uitvoeren van het migratieproces, moet u een van de volgende opdrachtregelparameters gebruiken:

* Naar **migreren van gebruikers met een wachtwoord**, gebruiken de `UserMigration.exe 1` opdracht.

* Naar **migreren van gebruikers met een willekeurig wachtwoord**, gebruiken de `UserMigration.exe 2` opdracht. Deze bewerking maakt ook een Azure-Tabelentiteit. Later, configureert u het beleid voor het aanroepen van de REST-API-service. De service gebruikt een Azure-tabel om te houden en beheren van het migratieproces.

![Demo voor migratie-proces](media/active-directory-b2c-user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>Stap 2.4: Het proces vóór de migratie controleren
Gebruik een van de volgende twee methoden voor het valideren van de migratie:

* Als u wilt zoeken naar een gebruiker op weergavenaam, de Azure portal te gebruiken:
    
    a. Open **Azure AD B2C**, en selecteer vervolgens **gebruikers en groepen**.

    b. In het zoekvak, typ de weergavenaam van de gebruiker en bekijk vervolgens het profiel van de gebruiker. 

* Voor het ophalen van een gebruiker door aanmelden e-mailadres, moet u deze voorbeeldtoepassing gebruiken:

    a. Voer de volgende opdracht uit:

    ```Console
        UserMigration.exe 3 {email address}
    ```
        
    > [!TIP]
    > U kunt ook de uitvoer naar een JSON-bestand opslaan met behulp van de volgende opdracht:
    >
    >```Console
    >    UserMigration.exe 3 {email address} >> UserProfile.json
    >```

    > [!TIP]
    > U kunt ook een gebruiker op weergavenaam ophalen met behulp van de volgende opdracht: `UserMigration.exe 4 <Display name>`.

    b. Open de *UserProfile.json* bestand in een JSON-editor. In Visual Studio Code, kunt u een JSON-document opmaken Shift + Alt + F selecteren of selecteren **Format Document** in het contextmenu.

    ![Het bestand UserProfile.json](media/active-directory-b2c-user-migration/pre-migration-get-by-email2.png)

### <a name="step-25-optional-environment-cleanup"></a>Stap 2.5: (Optioneel) omgeving opschonen
Als u wilt opruimen-up maken van uw Azure AD-tenant en gebruikers verwijderen uit de Azure AD-directory, voer de `UserMigration.exe 5` opdracht.

> [!NOTE]
> * Als u wilt opschonen uw tenant, gebruiker accountbeheerder machtigingen configureren voor uw toepassing.
> * De voorbeeld-app voor migratie ruimt op alle gebruikers die worden vermeld in het JSON-bestand.

### <a name="step-26-sign-in-with-migrated-users-with-password"></a>Stap 2.6: Aanmelden met de gemigreerde gebruikers (met een wachtwoord)
Nadat u het proces vóór de migratie uitvoert met wachtwoorden van gebruikers, de accounts zijn gereed om te gebruiken en gebruikers zich aanmelden bij uw toepassing met behulp van Azure AD B2C. Als u geen toegang tot wachtwoorden, blijven de volgende sectie.

## <a name="step-3-help-users-reset-their-password"></a>Stap 3: Zodat gebruikers hun wachtwoord opnieuw instellen
Als u gebruikers met een willekeurig wachtwoord migreert, moeten ze hun wachtwoord opnieuw instellen. Stuur een welkomstbericht met een koppeling naar het wachtwoord opnieuw instellen zodat ze het wachtwoord opnieuw instellen.

Als u de koppeling naar uw wachtwoord opnieuw instellen van beleid, het volgende doen:

1. Selecteer **Azure AD B2C-instellingen**, en selecteer vervolgens **wachtwoord opnieuw instellen** eigenschappen van beleid.

2. Selecteer uw toepassing.

    >[!NOTE]
    >Uitvoeren is nu vereist ten minste één toepassing om te worden preregistered op de tenant. Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel. 

3. Selecteer **nu uitvoeren**, en controleert u het beleid.

4. In de **uitvoeren nu endpoint** vak, Kopieer de URL en vervolgens naar uw gebruikers te verzenden.

    ![Verzameling diagnostische logboeken](media/active-directory-b2c-user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-the-user-migration-status"></a>Stap 4: (Optioneel) Wijzig uw beleid om te controleren en de status van de migratie gebruiker instellen

> [!NOTE]
> Als u wilt controleren en wijzigen van de gebruikersstatus van de migratie, moet u een aangepast beleid gebruiken. Zie voor meer informatie [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md).
>

Wanneer gebruikers proberen aan te melden zonder eerst het wachtwoord opnieuw instellen, moet uw beleid voor een uitgebreide foutbericht retourneren. Bijvoorbeeld: 
>*Uw wachtwoord is verlopen. Als u deze herstellen, selecteert u de koppeling wachtwoord opnieuw instellen.* 

Deze optionele stap vereist het gebruik van aangepaste beleidsregels voor Azure AD B2C, zoals beschreven in de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) artikel.

In deze sectie door het beleid om te controleren van de gebruikersstatus van de migratie op de aanmeldingspagina te wijzigen. Als de gebruiker het wachtwoord niet wijzigen, een HTTP-409 foutbericht weergegeven waarin u wordt gevraagd de gebruiker te selecteren retourneert de **wachtwoord vergeten?** koppeling. 

Om te wijzigen van het wachtwoord houden, moet u een Azure-tabel gebruiken. Wanneer u het proces vóór de migratie uitvoert met de opdrachtregelparameter `2`, u een gebruikersentiteit in een Azure-tabel maken. Uw service doet het volgende:

* Op aanmelden, wordt de migratie van RESTful-service, een e-mailbericht verzenden als een invoerclaim opgeroepen door het beleid van Azure AD B2C. De service zoekt naar het e-mailadres in de Azure-tabel. Als het adres bestaat, de service een foutbericht genereert: *moet u wachtwoord wijzigen*.

* Nadat de gebruiker is het wachtwoord wijzigt, moet u de entiteit verwijderen uit de Azure-tabel.

>[!NOTE]
>We een Azure-tabel gebruiken voor het vereenvoudigen van het voorbeeld. U kunt de migratiestatus van de opslaan in een database of als een aangepaste eigenschap in de Azure AD B2C-account.

### <a name="41-update-your-application-setting"></a>4.1: uw toepassingsinstelling bijwerken
1. Als u wilt testen van de RESTful-API-demo, opent u de `AADB2C.UserMigration.sln` Visual Studio-oplossing in Visual Studio. 

2. In de `AADB2C.UserMigration.API` project, open de *App.config* bestand. De app-instelling vervangen door uw eigen waarde:

    ```XML
    <appSettings>
        <add key="BlobStorageConnectionString" value="{The Azure Blob storage connection string"} />
    </appSettings>
    ```

### <a name="step-42-deploy-your-web-application-to-azure-app-service"></a>Stap 4.2: Uw webtoepassing in Azure App Service implementeren
Uw API-service publiceren in Azure App Service. Zie voor meer informatie [uw app implementeren in Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy).

### <a name="step-43-add-a-technical-profile-and-technical-profile-validation-to-your-policy"></a>4.3 stap: Een profiel voor technische en technische profiel validatie toevoegen aan uw beleid 
1. Open in uw werkmap, de *TrustFrameworkExtensions.xml* extensiebestand beleid. 

2. Zoeken naar de `<ClaimsProviders>` knooppunt en voegt u het volgende XML-fragment in het knooppunt. Wijzig de waarde van `ServiceUrl` om te verwijzen naar de eindpunt-URL. 

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
    
    <ClaimsProvider>
        <DisplayName>Local Account</DisplayName>
        <TechnicalProfiles>
    
        <!-- This technical profile uses a validation technical profile to authenticate the user. -->
        <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountSignIn" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        <TechnicalProfile Id="LocalAccountWritePasswordUsingObjectId">
            <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="LocalAccountPasswordReset" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    
        </TechnicalProfiles>
    </ClaimsProvider>
    ```

De voorgaande technische profiel definieert een invoerclaim: `signInName` (verzenden als e-mailadres). Op aanmelden, wordt de claim verzonden naar uw RESTful-eindpunt.

Nadat u het profiel voor technische gedefinieerd voor de RESTful-API, laat u uw Azure AD B2C-beleid aan te roepen van het technische profiel. Het XML-fragment overschrijft `SelfAsserted-LocalAccountSignin-Email`, die is gedefinieerd in het basis-beleid. Het XML-fragment voegt ook `ValidationTechnicalProfile`, met ReferenceId die verwijst naar uw technische profiel `LocalAccountUserMigration`. 

### <a name="step-44-upload-the-policy-to-your-tenant"></a>Stap 4.4: Het beleid uploaden naar uw tenant
1. In de [Azure-portal](https://portal.azure.com), overschakelen naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en selecteer vervolgens **Azure AD B2C**.

2. Selecteer **identiteit ervaring Framework**.

3. Selecteer **alle beleidsregels**.

4. Selecteer **uploaden beleid**.

5. Selecteer de **het beleid overschreven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkExtensions.xml* bestand en zorg ervoor dat deze gevalideerd wordt.

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a>Stap 4.5: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Selecteer **Azure AD B2C-instellingen**, en ga vervolgens naar **identiteit ervaring Framework**.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid die u hebt geüpload en selecteer vervolgens **nu uitvoeren**.

3. Aanmelden met een van de gemigreerde gebruikers referenties en selecteer vervolgens **aanmelden**. REST-API moet genereert het volgende foutbericht weergegeven:

    ![Verzameling diagnostische logboeken](media/active-directory-b2c-user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshoot-your-rest-api"></a>Stap 4.6: (Optioneel) uw REST API oplossen
U kunt weergeven en logboekinformatie in near-realtime controleren.

1. Menu instellingen van uw RESTful toepassing onder **bewaking**, selecteer **diagnostische logboeken**. 

2. Stel **toepassingslogboeken (bestandssysteem)** naar **op**. 

3. Stel de **niveau** naar **uitgebreide**.

4. Selecteer **opslaan**

    ![Verzameling diagnostische logboeken](media/active-directory-b2c-user-migration/pre-migration-diagnostic-logs.png)

5. Op de **instellingen** selecteert u **logboekstream**.

6. Controleer de uitvoer van de RESTful-API.

Zie voor meer informatie [Streaming-logboeken en de console](https://docs.microsoft.com/azure/app-service-web/web-sites-streaming-logs-and-console).

> [!IMPORTANT]
> Gebruik de logboeken met diagnostische gegevens alleen tijdens het ontwikkelen en testen. De uitvoer van de RESTful-API kan vertrouwelijke gegevens bevatten die niet in de productieomgeving worden blootgesteld.
>

## <a name="optional-download-the-complete-policy-files"></a>(Optioneel) De volledige beleidsbestanden downloaden
Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario maken met behulp van uw eigen aangepaste beleidsbestanden. We hebben opgegeven voor eigen referentie [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-user-migration). 
