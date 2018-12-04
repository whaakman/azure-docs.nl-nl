---
title: De Azure Active Directory B2C-gebruikersinterface (UI) dynamisch aanpassen met behulp van aangepast beleid | Microsoft Docs
description: Ondersteuning voor meerdere huisstijl ervaringen met HTML5/CSS-inhoud die dynamisch worden gewijzigd tijdens runtime.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/20/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f078c1389e36b82f95b011ca1fbd7fbd1c4f895e
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834218"
---
# <a name="azure-active-directory-b2c-configure-the-ui-with-dynamic-content-by-using-custom-policies"></a>Azure Active Directory B2C: Het configureren van de gebruikersinterface met dynamische inhoud met behulp van aangepaste beleidsregels

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Met behulp van Azure Active Directory B2C (Azure AD B2C) aangepaste beleidsregels, kunt u een parameter in een queryreeks verzenden. Door de parameter door te geven aan uw HTML-eindpunt, kunt u de pagina-inhoud dynamisch wijzigen. U kunt bijvoorbeeld de achtergrondafbeelding op de registratie- of aanmeldingspagina van Azure AD B2C wijzigen, op basis van een parameter die u doorgeeft vanuit uw web- of mobiele toepassing. 

## <a name="prerequisites"></a>Vereisten
In dit artikel richt zich op het aanpassen van de gebruikersinterface van Azure AD B2C met *dynamische inhoud* met behulp van aangepast beleid. Als u wilt beginnen, Zie [in een aangepast beleid voor UI-aanpassing](active-directory-b2c-ui-customization-custom.md). 

>[!NOTE]
>Het Azure AD B2C-artikel [aanmeldbeleid configureren in een aangepast beleid](active-directory-b2c-ui-customization-custom.md), worden de volgende grondbeginselen besproken:
> * De pagina gebruiker gebruikersinterface (UI) functie aanpassen.
> * Essentiële hulpmiddelen voor het testen van de pagina UI-functie aanpassen met behulp van onze voorbeelden voor inhoud.
> * De core UI-elementen van elk paginatype.
> * Aanbevolen procedures voor het toepassen van deze functie.

## <a name="add-a-link-to-html5css-templates-to-your-user-journey"></a>Een koppeling naar uw gebruikersbeleving HTML5/CSS sjablonen toevoegen

In een aangepast beleid definieert een inhoudsdefinitie de HTML5-pagina URI die wordt gebruikt voor een opgegeven UI-stap (bijvoorbeeld de pagina aanmelden of registreren). Het Basisbeleid definieert het uiterlijk van de standaard door die verwijst naar een URI van HTML5-bestanden (in het CSS). In de uitbreiding beleid, kunt u het uiterlijk wijzigen door de LoadUri voor het HTML5-bestand te overschrijven. Inhoudsdefinities bevatten de URL's naar externe inhoud die is gedefinieerd door het samenstellen van HTML5/CSS-bestanden, waar nodig. 

De `ContentDefinitions` sectie bevat een reeks `ContentDefinition` XML-elementen. Het kenmerk ID van de `ContentDefinition` element Hiermee geeft u het type van de pagina die is gekoppeld aan de definitie van de inhoud. Het element bepaalt dat wil zeggen, de context die u een aangepaste HTML5/CSS-sjabloon wilt toepassen. De volgende tabel beschrijft de set met inhoud definitie-id's die worden herkend door de engine IEF en de typen van de pagina's die betrekking hebben op deze.

| De definitie van de inhoud-ID | Standaardsjabloon voor HTML5| Beschrijving | 
|-----------------------|--------|-------------|
| *api.error* | [Exception.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Foutpagina**. Deze pagina wordt weergegeven wanneer er een uitzondering of een fout is opgetreden. |
| *api.idpselections* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Pagina voor het id-provider selecteren**. Deze pagina geeft een lijst met id-providers die gebruikers uit tijdens het aanmelden kiezen kunnen. De opties zijn meestal enterprise id-providers, sociale id-providers, zoals Facebook en Google + of lokale accounts. |
| *api.idpselections.signup* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Selectie van de id-provider voor aanmelding bij**. Deze pagina geeft een lijst met id-providers die gebruikers uit tijdens de registratie kiezen kunnen. De opties zijn enterprise id-providers, sociale id-providers, zoals Facebook en Google + of lokale accounts. |
| *api.localaccountpasswordreset* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Pagina voor vergeten wachtwoorden**. Deze pagina bevat een formulier die gebruikers moeten worden voltooid voor het starten van een wachtwoord opnieuw instellen.  |
| *api.localaccountsignin* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Lokaal account aanmelden pagina**. Deze pagina bevat een formulier voor het aanmelden met een lokaal account dat gebaseerd op een e-mailadres of een gebruikersnaam opgeven. Een tekstvak voor invoer en wachtwoord vermelding in bevatten het formulier. |
| *api.localaccountsignup* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Aanmeldpagina voor lokaal account**. Deze pagina bevat een formulier om zich te registreren voor een lokaal account dat gebaseerd op een e-mailadres of een gebruikersnaam opgeven. Het formulier kan bevatten verschillende besturingselementen voor tekstinvoer, zoals: invoer een text box, een wachtwoordinvoervak, een keuzerondje, vervolgkeuzelijsten enkelvoudige selectie, en selectievakjes voor meervoudige selectie. |
| *api.phonefactor* | [multifactor-1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Multi-factor authentication-pagina**. Op deze pagina, kunnen gebruikers hun telefoonnummers (via SMS of spraak) controleren tijdens het registreren of aanmelden. |
| *api.selfasserted* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Aanmeldpagina voor sociaal account**. Deze pagina bevat een formulier die gebruikers moeten worden voltooid wanneer ze zich aanmelden met behulp van een bestaand account van een sociale id-provider. Deze pagina is vergelijkbaar met de voorgaande sociaal accountpagina voor het registreren, met uitzondering van de velden van de vermelding wachtwoord. |
| *api.selfasserted.profileupdate* | [updateprofile.HTML](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Update-profielpagina**. Deze pagina bevat een formulier dat gebruikers toegang hebben tot voor het bijwerken van het profiel. Deze pagina is vergelijkbaar met de sociaal account registratiepagina, met uitzondering van de velden van de vermelding wachtwoord. |
| *api.signuporsignin* | [Unified.HTML](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Uniforme pagina voor registreren of aanmelden**. Deze pagina zorgt voor de gebruiker zich kunnen registreren en aanmelden proces. Gebruikers kunnen enterprise id-providers, sociale id-providers zoals Facebook of Google + of lokale accounts gebruiken.  |

## <a name="serving-dynamic-content"></a>Dynamische inhoud
In de [aanmeldbeleid configureren in een aangepast beleid](active-directory-b2c-ui-customization-custom.md) artikel u HTML5-bestanden uploaden naar Azure Blob-opslag. Deze bestanden HTML5 statisch zijn en de dezelfde HTML-inhoud voor elke aanvraag weergegeven. 

In dit artikel gebruikt u een ASP.NET web-app, die u kunt queryreeksparameters accepteren en dienovereenkomstig reageren. 

In dit scenario maakt u:
* Een ASP.NET Core-webtoepassing die als host fungeert voor uw HTML5-sjablonen maken. 
* Toevoegen van een aangepaste sjabloon HTML5 _unified.cshtml_. 
* Publiceer uw web-app in Azure App Service. 
* Cross-origin resource sharing (CORS) voor uw web-app ingesteld.
* Overschrijf de `LoadUri` elementen om te verwijzen naar het HTML5-bestand.

## <a name="step-1-create-an-aspnet-web-app"></a>Stap 1: Een ASP.NET-web-app maken

1. Maak in Visual Studio een project door te selecteren **bestand** > **nieuw** > **Project**.

2. In de **nieuw Project** venster **Visual C#** > **Web** > **ASP.NET Core-webtoepassing (.NET Core)**.

3. Noem de toepassing (bijvoorbeeld *Contoso.AADB2C.UI*), en selecteer vervolgens **OK**.

    ![Nieuwe Visual Studio-project maken](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project1.png)

4. Selecteer de **webtoepassing** sjabloon.

5. De verificatie instellen voor **geen verificatie**.

    ![Web-App-sjabloon selecteren](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project2.png)

6. Selecteer **OK** om het project te maken.

## <a name="step-2-create-mvc-view"></a>Stap 2: MVC weergave maken
### <a name="step-21-download-the-b2c-built-in-html5-template"></a>Stap 2.1: De B2C-ingebouwde HTML5-sjabloon downloaden
Uw aangepaste HTML5-sjabloon is gebaseerd op de Azure AD B2C ingebouwde HTML5-sjabloon. U kunt downloaden de [unified.html bestand](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) of download de sjabloon in [beginnerspakket](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates/wingtip). U dit bestand HTML5 gebruiken om een uniforme pagina voor registreren of aanmelden te maken.

### <a name="step-22-add-the-mvc-view"></a>Stap 2.2: De MVC-weergave toevoegen
1. Met de rechtermuisknop op de map weergaven en thuis, en vervolgens **toevoegen** > **Nieuw Item**.

    ![MVC-nieuw item toevoegen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view1.png)

2. In de **Nieuw Item toevoegen - Contoso.AADB2C.UI** venster **Web > ASP.NET**.

3. Selecteer **MVC-pagina bekijkt**.

4. In de **naam** , wijzig de naam **unified.cshtml**.

5. Selecteer **Toevoegen**.

    ![MVC-weergave toevoegen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view2.png)

6. Als de *unified.cshtml* bestand nog niet open is, dubbelklik op het bestand om dit te openen en schakel vervolgens de inhoud van het bestand.

7. We verwijderen de verwijzing naar de lay-out-pagina voor dit scenario. Voeg het volgende codefragment aan _unified.cshtml_:

    ```csharp
    @{
        Layout = null;
    }
    ```

8. Open de _unified.cshtml_ dat u hebt gedownload van Azure AD B2C ingebouwde HTML5-sjabloon.

9. Vervang de één apenstaartjes (@) met dubbel van apenstaartjes (@).

10. Kopieer de inhoud van het bestand en plak deze onder de definitie van de lay-out. Uw code er moet uitzien:

    ![Unified.cshtml bestand na het toevoegen van het HTML5](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-edit-view1.png)

### <a name="step-23-change-the-background-image"></a>Stap 2.3: De achtergrondafbeelding wijzigen

Zoek de `<img>` -element waarin de `ID` waarde *background_background_image*, en vervang de `src` waarde met **https://kbdevstorage1.blob.core.windows.net/asset-blobs/19889_en_1** of andere de achtergrondafbeelding die u wilt gebruiken.

![De pagina-achtergrond wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-static-background.png)

### <a name="step-24-add-your-view-to-the-mvc-controller"></a>Stap 2.4: Uw weergave toevoegen aan de MVC-controller

1. Open **Controllers\HomeController.cs**, en voeg de volgende methode: 

    ```C
    public IActionResult unified()
    {
        return View();
    }
    ```
    Deze code geeft aan dat de methode moet een *weergave* sjabloonbestand om een antwoord naar de browser weer te geven. Omdat we niet expliciet met de naam van opgeven de *weergave* sjabloonbestand, MVC standaard gebruik van de _unified.cshtml_ bestand weergeven in de */weergaven en thuis* map.
    
    Nadat u hebt toegevoegd de _geïntegreerde_ methode, uw code er moet uitzien:
    
    ![De controller voor het renderen van de weergave wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-controller-view.png)

2. Fouten opsporen in uw web-app en zorg ervoor dat de _geïntegreerde_ pagina is toegankelijk (bijvoorbeeld `http://localhost:<Port number>/Home/unified`).

### <a name="step-25-publish-to-azure"></a>Stap 2.5: Publiceren naar Azure
1. In **Solution Explorer**, met de rechtermuisknop op de **Contoso.AADB2C.UI** project en selecteer vervolgens **publiceren**.

    ![Publiceren naar Microsoft Azure App Service](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish1.png)

2. Selecteer de **Microsoft Azure App Service** tegel en selecteer vervolgens **publiceren**.

    ![Nieuwe Microsoft Azure App Service maken](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish2.png)

    De **Create App Service** venster wordt geopend. In deze kunt u beginnen met het maken van alle benodigde Azure-resources voor het uitvoeren van de ASP.NET-web-app in Azure.

    > [!NOTE]
    > Zie voor meer informatie over publicatie [een ASP.NET-web-app maken in Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).

3. In de **Web-Appnaam** typt u een unieke app-naam (geldige tekens zijn a-z, A-Z, 0-9 en het koppelteken (-). De URL van de web-app is `http://<app_name>.azurewebsites.NET`, waarbij `<app_name>` de naam van uw web-app is. U kunt de automatisch gegenereerde naam accepteren, die uiteraard uniek is.

4. Selecteer **Maken** om de Azure-resources te gaan maken.

    ![Geef eigenschappen voor de App Service](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish3.png)

    Nadat het proces voor het maken voltooid is, wordt de wizard de ASP.NET-web-app gepubliceerd naar Azure en daarna wordt de app in de standaardbrowser gestart.

5. Kopieer de URL van de _geïntegreerde_ pagina (bijvoorbeeld _https://<app_name>.azurewebsites.net/home/unified_).

## <a name="step-3-configure-cors-in-azure-app-service"></a>Stap 3: Configureer CORS in Azure App Service
1. In de [Azure-portal](https://portal.azure.com/), selecteer **App Services**, en selecteer vervolgens de naam van uw API-app.

    ![API-app selecteren in de Azure-portal](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS1.png)

2. In de **instellingen** sectie onder **API** sectie, selecteer **CORS**.

    ![CORS-instellingen selecteren](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS2.png)

3. In de **CORS** venster in de **oorsprongen toegestaan** een van de volgende handelingen uit:

    * Voer de URL of URL's die u toestaan dat JavaScript-aanroepen afkomstig zijn wilt van. U moet alle kleine letters gebruiken in de URL's die u invoert.
    * Voer een sterretje (*) als u wilt opgeven dat alle domeinen worden geaccepteerd.

4. Selecteer **Opslaan**.

    ![De CORS-venster](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS3.png)

    Nadat u hebt geselecteerd **opslaan**, de API-app JavaScript-aanroepen vanuit de opgegeven URL's accepteert. 

## <a name="step-4-html5-template-validation"></a>Stap 4: HTML5 sjabloonvalidatie
De sjabloon HTML5 is klaar voor gebruik. Het is echter niet beschikbaar in de `ContentDefinition` code. Voordat u kunt toevoegen `ContentDefinition` aan uw aangepaste beleid, zorg ervoor dat:
* Uw inhoud is HTML5 compatibel is en toegankelijk is.
* Uw server voor webinhoud is ingeschakeld voor CORS.

    >[!NOTE]
    >Om te controleren dat de site waar u de inhoud van uw host CORS is ingeschakeld en CORS-aanvragen kunt testen, gaat u naar de [test cors.org](https://test-cors.org/) website. 

* Uw aangeboden inhoud wordt beveiligd via **HTTPS**.
* U gebruikt *absolute URL's*, zoals *https://yourdomain/content*, voor alle koppelingen, CSS-inhoud en afbeeldingen.

## <a name="step-5-configure-your-content-definition"></a>Stap 5: De definitie van de inhoud configureren
Het configureren van `ContentDefinition`, doet u het volgende:
1. Open het bestand basis van uw beleid (bijvoorbeeld *TrustFrameworkBase.xml*).

2. Zoek de `<ContentDefinitions>` -element, en kopieer de volledige inhoud van de `<ContentDefinitions>` knooppunt.

3. Open het extensiebestand (bijvoorbeeld *TrustFrameworkExtensions.xml*) en zoek vervolgens de `<BuildingBlocks>` element. Als het element niet bestaat, deze toevoegen.

4. Plak de volledige inhoud van de `<ContentDefinitions>` knooppunt dat u hebt gekopieerd als onderliggende site van de `<BuildingBlocks>` element. 

5. Zoek de `<ContentDefinition>` knooppunt dat bevat `Id="api.signuporsignin"` in het XML-bestand dat u hebt gekopieerd.

6. Wijzig de waarde van `LoadUri` van _~/tenant/default/unified_ naar _https://<app_name>.azurewebsites.net/home/unified_.  
    Het aangepaste beleid ziet er als volgt uit:
    
    ![De definitie van de inhoud](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-content-definition.png)

## <a name="step-6-upload-the-policy-to-your-tenant"></a>Stap 6: Het beleid voor uploaden naar uw tenant
1. In de [Azure-portal](https://portal.azure.com), Ga naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en selecteer vervolgens **Azure AD B2C**.

2. Selecteer **Identity-Ervaringsframework**.

3. Selecteer **alle beleidsregels**.

4. Selecteer **beleid uploaden**.

5. Selecteer de **het beleid overschrijven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkExtensions.xml* -bestand en zorg ervoor dat deze de validatietests doorstaat.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>Stap 7: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Selecteer **Azure AD B2C-instellingen**, en selecteer vervolgens **Identity-Ervaringsframework**.

    >[!NOTE]
    >Voer nu vereist dat ten minste één toepassing vooraf op de tenant worden geregistreerd. Zie voor meer informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast beleid u geüpload en selecteer vervolgens **nu uitvoeren**.  
    U zou het mogelijk om te zien van uw aangepaste HTML5 met de achtergrond die u eerder hebt gemaakt.

    ![Uw beleid voor registreren of aanmelden](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo1.png)

## <a name="step-8-add-dynamic-content"></a>Stap 8: Dynamische inhoud toevoegen
De achtergrond op basis van de queryreeks-parameter met de naam wijzigen _campaignId_. Uw toepassing RP (web en mobiele apps) verzendt de parameter naar Azure AD B2C. Uw beleid voor de parameter leest en de waarde ervan verzendt naar uw HTML5-sjabloon. 

### <a name="step-81-add-a-content-definition-parameter"></a>Stap 8.1: Een inhoudsdefinitie parameter toevoegen

Voeg de `ContentDefinitionParameters` element door de volgende te doen:
1. Open de *SignUpOrSignin* bestand van uw beleid (bijvoorbeeld *SignUpOrSignin.xml*).

2. Zoek de `<DefaultUserJourney>` knooppunt. 

3. In de `<DefaultUserJourney>` knooppunt, Voeg het volgende XML-fragment toe:  

    ```XML
    <UserJourneyBehaviors>
        <ContentDefinitionParameters>
            <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
        </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    ```

### <a name="step-82-change-your-code-to-accept-a-query-string-parameter-and-replace-the-background-image"></a>Stap 8.2: Uw code voor het accepteren van een queryreeks-parameter te wijzigen en de achtergrondafbeelding vervangen
Wijzigen van de HomeController `unified` methode voor het accepteren van de parameter campaignId. De methode controleert vervolgens of de parameter wordt ingesteld en waarde van de `ViewData["background"]` variabele dienovereenkomstig.

1. Open de *Controllers\HomeController.cs* bestand en wijzig vervolgens de `unified` methode door het volgende codefragment toe te voegen:

    ```csharp
    public IActionResult unified(string campaignId)
    {
        // If campaign ID is Hawaii, show Hawaii background
        if (campaignId != null && campaignId.ToLower() == "hawaii")
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/19889_en_1";
        }
        // If campaign ID is Tokyo, show Tokyo background
        else if (campaignId != null && campaignId.ToLower() == "tokyo")
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/19666_en_1";
        }
        // Default background
        else
        {
            ViewData["background"] = "https://kbdevstorage1.blob.core.windows.net/asset-blobs/18983_en_1";
        }

        return View();
    }

    ```

2. Zoek de `<img>` element met ID `background_background_image`, en vervang de `src` waarde met `@ViewData["background"]`.

    ![De pagina-achtergrond wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-dynamic-background.png)

### <a name="83-upload-the-changes-and-publish-your-policy"></a>8.3: de wijzigingen uploaden en publiceren van uw beleid
1. Uw Visual Studio-project kunt publiceren in Azure App Service.

2. Upload de *SignUpOrSignin.xml* Azure AD B2C-beleid.

3. Open **B2C_1A_signup_signin**, de RP aangepast beleid dat u geüpload en selecteer vervolgens **nu uitvoeren**.  
    Hier ziet u de dezelfde achtergrondafbeelding die eerder werd weergegeven.

4. Kopieer de URL van de adresbalk van de browser.

5. Voeg de _campaignId_ query-tekenreeksparameter naar de URI. Voeg bijvoorbeeld `&campaignId=hawaii`, zoals weergegeven in de volgende afbeelding:

    ![De pagina-achtergrond wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-campaignId-param.png)

6. Selecteer **Enter** om weer te geven van de achtergrondafbeelding Hawaï.

    ![De pagina-achtergrond wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo2.png)

7. Wijzig de waarde in *Tokio*, en selecteer vervolgens **Enter**.  
    De achtergrond Tokio weergegeven in de browser.

    ![De pagina-achtergrond wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo3.png)

## <a name="step-9-change-the-rest-of-the-user-journey"></a>Stap 9: De rest van de gebruikersbeleving wijzigen
Als u selecteert de **Meld u nu** koppeling op de aanmeldingspagina, de browser de standaardachtergrondafbeelding weergegeven, niet de installatiekopie die u hebt gedefinieerd. Dit probleem doet zich voor omdat u alleen de registratie- of aanmelden pagina hebt gewijzigd. De rest van de inhoud zelf Assert-definities wijzigen:
1. Ga terug naar 'Stap 2' en het volgende doen:

    a. Download de *selfasserted* bestand.

    b. Kopieer de inhoud van het bestand.

    c. Maak een nieuwe weergave *selfasserted*.

    d. Voeg *selfasserted* naar de **Start** controller.

2. Ga terug naar 'Stap 4' en het volgende doen: 

    a. Zoek in de uitbreiding beleid, de `<ContentDefinition>` knooppunt dat bevat `Id="api.selfasserted"`, `Id="api.localaccountsignup"`, en `Id="api.localaccountpasswordreset"`.

    b. Stel de `LoadUri` kenmerk aan uw *selfasserted* URI.

3. Ga terug naar 'Stap 8.2' en wijzigen van uw code voor het accepteren van queryreeksparameters, maar deze keer naar de *selfasserted* functie. 

4. Upload de *TrustFrameworkExtensions.xml* beleid, en zorg ervoor dat deze de validatietests doorstaat.

5. Het beleid test uitvoert, en selecteer vervolgens **Meld u nu** om het resultaat te bekijken.

## <a name="optional-download-the-complete-policy-files-and-code"></a>(Optioneel) De volledige beleidsbestanden en de code downloaden
* Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario bouwen met behulp van uw eigen aangepaste beleidsbestanden. Ter referentie, we hebben opgegeven [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).
* U kunt de code van de volledige downloaden [voorbeeld Visual Studio-oplossing voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).




