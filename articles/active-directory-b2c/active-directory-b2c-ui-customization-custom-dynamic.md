---
title: 'Azure Active Directory B2C: De Azure AD B2C-gebruikersinterface (UI) dynamisch aanpassen met behulp van aangepaste beleid'
description: Ondersteuning voor meerdere huisstijl ervaringen met HTML5/CSS-inhoud die dynamisch worden gewijzigd tijdens runtime.
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
ms.date: 09/20/2017
ms.author: yoelh
ms.openlocfilehash: 3a2310ae6266709df6677c55f11b15239c0425a2
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-active-directory-b2c-configure-the-ui-with-dynamic-content-by-using-custom-policies"></a>Azure Active Directory B2C: Het configureren van de gebruikersinterface met dynamische inhoud met behulp van aangepast beleid
Met behulp van Azure Active Directory B2C (Azure AD B2C) aangepast beleid, kunt u een parameter in een queryreeks verzenden. Door de parameter wordt doorgegeven aan uw HTML-eindpunt, kunt u de pagina-inhoud dynamisch wijzigen. U kunt bijvoorbeeld de achtergrondafbeelding op de Azure AD B2C registreren of aanmelden pagina op basis van een parameter die u van uw web- of mobiele toepassing doorgeeft wijzigen. 

## <a name="prerequisites"></a>Vereisten
In dit artikel is gericht op het aanpassen van de Azure AD B2C-gebruikersinterface met *dynamische inhoud* met behulp van aangepast beleid. Om te beginnen, Zie [aanpassen in een aangepast beleid](active-directory-b2c-ui-customization-custom.md). 

>[!NOTE]
>Het Azure AD B2C-artikel [configureren aanpassen in een aangepast beleid](active-directory-b2c-ui-customization-custom.md), worden de volgende grondbeginselen besproken:
> * De pagina gebruiker gebruikersinterface (UI) aanpassing functie.
> * Essentiële hulpmiddelen voor het testen van de functie pagina gebruikersinterface aanpassen met behulp van onze voorbeelden.
> * De core UI-elementen van elk paginatype.
> * Aanbevolen procedures voor het toepassen van deze functie.

## <a name="add-a-link-to-html5css-templates-to-your-user-journey"></a>Een koppeling toevoegen aan de gebruiker reis HTML5/CSS-sjablonen

In een aangepast beleid definieert een definitie van de inhoud de HTML5-pagina URI die wordt gebruikt voor een specifieke gebruikersinterface-stap (bijvoorbeeld, de pagina aanmelden of registreren). Het basistype beleid bepaalt het standaarduiterlijk door te verwijzen naar een URI van HTML5-bestanden (in het CSS). In de uitbreiding beleid, kunt u het uiterlijk wijzigen door de LoadUri voor het HTML5-bestand te overschrijven. Inhoud definities bevatten URL's naar externe inhoud die is gedefinieerd door HTML5/CSS-bestanden naar gelang van toepassing. 

De `ContentDefinitions` sectie bevat een reeks `ContentDefinition` XML-elementen. Het kenmerk ID van de `ContentDefinition` element geeft het type van de pagina die is gekoppeld aan de definitie van de inhoud. Het element bepaalt dat wil zeggen, de context die u een aangepaste HTML5/CSS-sjabloon wilt toepassen. De volgende tabel beschrijft de set van inhoud definitie-id's die worden herkend door de engine voor het IEF en de paginatypen die betrekking hebben op deze.

| De definitie van de inhoud-ID | Standaardsjabloon voor HTML5| Beschrijving | 
|-----------------------|--------|-------------|
| *api.error* | [exception.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Foutpagina**. Deze pagina wordt weergegeven wanneer een uitzondering of een fout is opgetreden. |
| *api.idpselections* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Id-provider selectiepagina**. Deze pagina geeft een lijst van de id-providers die gebruikers tijdens het aanmelden kiezen kunnen. De opties zijn meestal enterprise identiteitsproviders, sociale id-providers zoals Facebook en Google + of lokale accounts. |
| *api.idpselections.signup* | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Selectie van de id-provider voor registratie**. Deze pagina geeft een lijst van de id-providers die gebruikers tijdens de registratie kiezen kunnen. De opties zijn enterprise identiteitsproviders, sociale id-providers zoals Facebook en Google + of lokale accounts. |
| *api.localaccountpasswordreset* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Wachtwoordpagina vergeten**. Deze pagina bevat een formulier die gebruikers moeten worden voltooid voor het initiëren van een wachtwoord opnieuw instellen.  |
| *api.localaccountsignin* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Aanmeldingspagina voor lokaal account**. Deze pagina bevat een formulier voor het aanmelden met een lokaal account dat gebaseerd op een e-mailadres of een gebruikersnaam. Het formulier kan een tekstinvoervak en wachtwoordvak vermelding bevatten. |
| *api.localaccountsignup* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Lokaal account registratiepagina**. Deze pagina bevat een formulier voor het aanmelden voor een lokaal account dat gebaseerd op een e-mailadres of een gebruikersnaam. Het formulier kan verschillende invoer besturingselementen bevatten, zoals: invoer voor een text vak, een wachtwoordinvoervak, een keuzerondje, één vervolgkeuzelijsten en meerdere Schakel selectievakjes. |
| *api.phonefactor* | [multifactor-1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Multi-factor authentication-pagina**. Op deze pagina, kunnen gebruikers hun telefoonnummers (met behulp van tekst of stem) controleren tijdens het registreren of aanmelden. |
| *api.selfasserted* | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **De aanmeldpagina sociale account**. Deze pagina bevat een formulier dat gebruikers wanneer ze zich registreren met behulp van een bestaand account van een identiteitsprovider van sociale moeten voltooien. Deze pagina is vergelijkbaar met de voorgaande sociale account aanmeldpagina, met uitzondering van de invoervelden wachtwoord. |
| *api.selfasserted.profileupdate* | [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Update profielpagina**. Deze pagina bevat een formulier waarop gebruikers kunnen hun profiel bijwerken. Deze pagina is vergelijkbaar met de sociale account aanmeldpagina, met uitzondering van de invoervelden wachtwoord. |
| *api.signuporsignin* | [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Unified registreren of aanmelden pagina**. Deze pagina wordt de gebruiker zich kunnen registreren en aanmelden proces. Gebruikers kunnen enterprise identiteitsproviders, sociale id-providers zoals Facebook of Google + of lokale accounts gebruiken.  |

## <a name="serving-dynamic-content"></a>Dynamische inhoud
In de [configureren aanpassen in een aangepast beleid](active-directory-b2c-ui-customization-custom.md) artikel leert u HTML5 bestanden uploaden naar Azure Blob-opslag. Deze bestanden HTML5 statisch zijn en dat de dezelfde HTML inhoud voor elke aanvraag. 

In dit artikel gebruikt u een ASP.NET-webtoepassing die u kunt queryreeksparameters accepteren en dienovereenkomstig reageren. 

In dit scenario maakt u:
* Een ASP.NET Core-webtoepassing die als host fungeert voor uw HTML5-sjablonen maken. 
* Voeg een aangepaste sjabloon voor het HTML5 _unified.cshtml_. 
* Uw web-app publiceren in Azure App Service. 
* Cross-origin-resource delen (CORS) voor uw web-app ingesteld.
* Overschrijf de `LoadUri` elementen verwijzen naar uw HTML5-bestand.

## <a name="step-1-create-an-aspnet-web-app"></a>Stap 1: Een ASP.NET-web-app maken

1. Maak in Visual Studio een project door te selecteren **bestand** > **nieuw** > **Project**.

2. In de **nieuw Project** Selecteer **Visual C#** > **Web** > **ASP.NET Core-webtoepassing (.NET Core)**.

3. Naam van de toepassing (bijvoorbeeld *Contoso.AADB2C.UI*), en selecteer vervolgens **OK**.

    ![Nieuw Visual Studio-project maken](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project1.png)

4. Selecteer de **webtoepassing** sjabloon.

5. De verificatie instellen voor het **geen verificatie**.

    ![Selecteer de sjabloon webtoepassing](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-create-project2.png)

6. Selecteer **OK** om het project te maken.

## <a name="step-2-create-mvc-view"></a>Stap 2: MVC weergave maken
### <a name="step-21-download-the-b2c-built-in-html5-template"></a>Stap 2.1: Download de B2C ingebouwde HTML5-sjabloon
De aangepaste HTML5-sjabloon is gebaseerd op de Azure AD B2C ingebouwde HTML5-sjabloon. U kunt downloaden via de [unified.html bestand](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) of downloaden van de sjabloon [starter pack](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates/wingtip). Dit bestand HTML5 u maakt een uniforme registreren of aanmelden pagina.

### <a name="step-22-add-the-mvc-view"></a>Step 2.2: De MVC-weergave toevoegen
1. Met de rechtermuisknop op de map weergaven/Home en vervolgens **toevoegen** > **Nieuw Item**.

    ![MVC nieuw item toevoegen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view1.png)

2. In de **Nieuw Item toevoegen - Contoso.AADB2C.UI** Selecteer **Web > ASP.NET**.

3. Selecteer **MVC-pagina bekijkt**.

4. In de **naam** Wijzig de naam **unified.cshtml**.

5. Selecteer **Toevoegen**.

    ![MVC-weergave toevoegen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-view2.png)

6. Als de *unified.cshtml* bestand nog niet open is, dubbelklik op het bestand om dit te openen en schakel vervolgens de inhoud van het bestand.

7. Voor dit scenario moet verwijderen we de verwijzing naar de pagina-indeling. Voeg het volgende codefragment aan _unified.cshtml_:

    ```csharp
    @{
        Layout = null;
    }
    ```

8. Open de _unified.cshtml_ bestand dat u hebt gedownload van Azure AD B2C ingebouwde HTML5-sjabloon.

9. Vervang het enkelvoudige apenstaartjes (@) met dubbel apenstaartjes (@).

10. Kopieer de inhoud van het bestand en plak deze onder de indelingsdefinitie. Uw code moet eruitzien als:

    ![Unified.cshtml bestand na het toevoegen van de HTML5](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-edit-view1.png)

### <a name="step-23-change-the-background-image"></a>Stap 2.3: De achtergrondafbeelding wijzigen

Zoek de `<img>` element met de `ID` waarde *background_background_image*, en vervang de `src` waarde met **https://kbdevstorage1.blob.core.windows.net/ Asset-blobs/19889_en_1** of andere achtergrondafbeelding die u wilt gebruiken.

![De pagina-achtergrond wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-add-static-background.png)

### <a name="step-24-add-your-view-to-the-mvc-controller"></a>Stap 2.4: Uw weergave toevoegen aan de MVC-controller

1. Open **Controllers\HomeController.cs**, en plaats de volgende methode: 

    ```C
    public IActionResult unified()
    {
        return View();
    }
    ```
    Deze code geeft aan dat de methode moet een *weergave* sjabloonbestand voor het weergeven van een antwoord naar de browser. Omdat we niet expliciet met de naam van opgeven de *weergave* sjabloonbestand MVC gebruikt standaard de _unified.cshtml_ weergave-bestand in de */weergaven/Home* map.
    
    Nadat u hebt toegevoegd de _unified_ methode, uw code moet eruitzien als:
    
    ![De domeincontroller voor het weergeven van de weergave wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-controller-view.png)

2. Fouten opsporen in uw web-app en zorg ervoor dat de _unified_ pagina toegankelijk is (bijvoorbeeld `http://localhost:<Port number>/Home/unified`).

### <a name="step-25-publish-to-azure"></a>Stap 2.5: Publiceren naar Azure
1. In **Solution Explorer**, met de rechtermuisknop op de **Contoso.AADB2C.UI** project en selecteer vervolgens **publiceren**.

    ![Publiceren naar Microsoft Azure App Service](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish1.png)

2. Selecteer de **Microsoft Azure App Service** tegel en selecteer vervolgens **publiceren**.

    ![Nieuwe Microsoft Azure App Service maken](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish2.png)

    De **Create App Service** venster wordt geopend. In dit kunt u beginnen met het maken van alle benodigde Azure-resources voor het uitvoeren van de ASP.NET-web-app in Azure.

    > [!NOTE]
    > Zie voor meer informatie over publiceren [een ASP.NET-web-app maken in Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).

3. In de **Web-Appnaam** typt u een unieke app-naam (geldige tekens zijn a-z, A-Z, 0-9 en het koppelteken (-). De URL van de web-app is `http://<app_name>.azurewebsites.NET`, waarbij `<app_name>` de naam van uw web-app is. U kunt de automatisch gegenereerde naam accepteren, die uiteraard uniek is.

4. Selecteer **Maken** om de Azure-resources te gaan maken.

    ![Geef eigenschappen voor de App Service](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-publish3.png)

    Nadat het maken van het proces voltooid is, wordt de wizard de ASP.NET-web-app publiceert naar Azure en vervolgens de app in de standaardbrowser start.

5. Kopieer de URL van de _unified_ pagina (bijvoorbeeld _https://<app_name>.azurewebsites.net/home/unified_).

## <a name="step-3-configure-cors-in-azure-app-service"></a>Stap 3: CORS configureren in Azure App Service
1. In de [Azure-portal](https://portal.azure.com/), selecteer **App Services**, en selecteer vervolgens de naam van uw API-app.

    ![API-app selecteren in de Azure portal](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS1.png)

2. In de **instellingen** onder sectie **API** sectie **CORS**.

    ![Selecteer de CORS-instellingen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS2.png)

3. In de **CORS** venster in de **toegestane oorsprongen** een van de volgende handelingen uit:

    * Geef de URL of URL's die u toestaan van JavaScript-aanroepen afkomstig zijn wilt van.
    * Geef een sterretje (*) als u wilt opgeven dat alle domeinen worden geaccepteerd.

4. Selecteer **Opslaan**.

    ![Het venster CORS](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-CORS3.png)

    Nadat u hebt geselecteerd **opslaan**, de API-app JavaScript-aanroepen vanuit de opgegeven URL's accepteert. 

## <a name="step-4-html5-template-validation"></a>Stap 4: HTML5 sjabloon validatie
Uw HTML5-sjabloon is klaar voor gebruik. Het is echter niet beschikbaar in de `ContentDefinition` code. Voordat u kunt toevoegen `ContentDefinition` naar het aangepaste beleid ervoor dat:
* De inhoud is HTML5 compatibel is en toegankelijk is.
* De inhoudsserver is voor CORS ingeschakeld.

    >[!NOTE]
    >Om te controleren of de site waar u uw inhoud host CORS is ingeschakeld en aanvragen van CORS kunt testen, gaat u naar de [test cors.org](http://test-cors.org/) website. 

* Uw aangeboden inhoud wordt beveiligd via **HTTPS**.
* U gebruikt *absolute URL's*, zoals *https://yourdomain/content*, voor alle koppelingen, CSS-inhoud en afbeeldingen.

## <a name="step-5-configure-your-content-definition"></a>Stap 5: De definitie van de inhoud configureren
Voor het configureren van `ContentDefinition`, het volgende doen:
1. Open het bestand basis van uw beleid (bijvoorbeeld *TrustFrameworkBase.xml*).

2. Zoeken naar de `<ContentDefinitions>` -element en kopieert u de volledige inhoud van de `<ContentDefinitions>` knooppunt.

3. Open het extensiebestand (bijvoorbeeld *TrustFrameworkExtensions.xml*) en zoek vervolgens naar de `<BuildingBlocks>` element. Als het element niet bestaat, moet u deze toevoegen.

4. Plak de volledige inhoud van de `<ContentDefinitions>` knooppunt dat u hebt gekopieerd als een onderliggend element van de `<BuildingBlocks>` element. 

5. Zoeken naar de `<ContentDefinition>` knooppunt met `Id="api.signuporsignin"` in de XML-bestand dat u hebt gekopieerd.

6. Wijzig de waarde van `LoadUri` van _~/tenant/default/unified_ naar _https://<app_name>.azurewebsites.net/home/unified_.  
    Het aangepaste beleid moet eruitzien als in het volgende:
    
    ![De definitie van de inhoud](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-content-definition.png)

## <a name="step-6-upload-the-policy-to-your-tenant"></a>Stap 6: Het beleid uploaden naar uw tenant
1. In de [Azure-portal](https://portal.azure.com), overschakelen naar de [context van uw Azure AD B2C-tenant](active-directory-b2c-navigate-to-b2c-context.md), en selecteer vervolgens **Azure AD B2C**.

2. Selecteer **identiteit ervaring Framework**.

3. Selecteer **alle beleidsregels**.

4. Selecteer **uploaden beleid**.

5. Selecteer de **het beleid overschreven als deze bestaat** selectievakje.

6. Upload de *TrustFrameworkExtensions.xml* bestand en zorg ervoor dat deze gevalideerd wordt.

## <a name="step-7-test-the-custom-policy-by-using-run-now"></a>Stap 7: Het aangepaste beleid testen met behulp van nu uitvoeren
1. Selecteer **Azure AD B2C-instellingen**, en selecteer vervolgens **identiteit ervaring Framework**.

    >[!NOTE]
    >Uitvoeren is nu vereist ten minste één toepassing om te worden preregistered op de tenant. Zie voor informatie over het registreren van toepassingen, de Azure AD B2C [aan de slag](active-directory-b2c-get-started.md) artikel of de [toepassingsregistratie](active-directory-b2c-app-registration.md) artikel.

2. Open **B2C_1A_signup_signin**, de relying party (RP) aangepast-beleid die u hebt geüpload en selecteer vervolgens **nu uitvoeren**.  
    U moet mogelijk zijn om te zien van uw aangepaste HTML5 met de achtergrond die u eerder hebt gemaakt.

    ![Uw beleid registreren of aanmelden](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo1.png)

## <a name="step-8-add-dynamic-content"></a>Stap 8: Dynamische inhoud toevoegen
De achtergrond op basis van Querytekenreeksparameter met de naam wijzigen _campaignId_. Uw toepassing RP (web- en mobiele apps) verzendt de parameter naar Azure AD B2C. Uw beleid voor de parameter leest en de waarde ervan verzendt naar uw HTML5-sjabloon. 

### <a name="step-81-add-a-content-definition-parameter"></a>Stap 8.1: Een parameter inhoudsdefinitie toevoegen

Voeg de `ContentDefinitionParameters` element door het volgende te doen:
1. Open de *SignUpOrSignin* -bestand van het beleid (bijvoorbeeld *SignUpOrSignin.xml*).

2. Zoeken naar de `<DefaultUserJourney>` knooppunt. 

3. In de `<DefaultUserJourney>` knooppunt, voeg de volgende XML-fragment:  

    ```XML
    <UserJourneyBehaviors>
        <ContentDefinitionParameters>
            <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
        </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    ```

### <a name="step-82-change-your-code-to-accept-a-query-string-parameter-and-replace-the-background-image"></a>Stap 8.2: Uw code voor het accepteren van een queryreeksparameter wijzigen en vervang de achtergrondafbeelding
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

### <a name="83-upload-the-changes-and-publish-your-policy"></a>8.3: de wijzigingen te uploaden en publiceren van uw beleid
1. Visual Studio-project publiceren in Azure App Service.

2. Upload de *SignUpOrSignin.xml* beleid voor het Azure AD B2C.

3. Open **B2C_1A_signup_signin**, het aangepaste RP-beleid die u hebt geüpload en selecteer vervolgens **nu uitvoeren**.  
    Hier ziet u de dezelfde achtergrondafbeelding die eerder is weergegeven.

4. Kopieer de URL van de adresbalk.

5. Voeg de _campaignId_ Querytekenreeksparameter met de URI. Bijvoorbeeld, Voeg `&campaignId=hawaii`, zoals weergegeven in de volgende afbeelding:

    ![De pagina-achtergrond wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-campaignId-param.png)

6. Selecteer **Enter** om de achtergrondafbeelding Hawaï weer te geven.

    ![De pagina-achtergrond wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo2.png)

7. Wijzig de waarde in *Tokio*, en selecteer vervolgens **Enter**.  
    De achtergrond Tokio weergegeven in de browser.

    ![De pagina-achtergrond wijzigen](media/active-directory-b2c-ui-customization-custom-dynamic/aadb2c-ief-ui-customization-demo3.png)

## <a name="step-9-change-the-rest-of-the-user-journey"></a>Stap 9: De rest van de gebruiker reis wijzigen
Als u selecteert de **nu aanmelden** koppeling op de aanmeldingspagina, de browser de standaardachtergrondafbeelding weergegeven, is niet de installatiekopie die u hebt gedefinieerd. Dit probleem doet zich voor omdat u alleen op de pagina registreren of aanmelden hebt gewijzigd. De rest van de inhoud zelf Assert-definities wijzigen:
1. Ga terug naar 'Stap 2' en het volgende doen:

    a. Download de *selfasserted* bestand.

    b. Kopieer de inhoud van het bestand.

    c. Maak een nieuwe weergave *selfasserted*.

    d. Voeg *selfasserted* naar de **Start** controller.

2. Ga terug naar 'Stap 4' en het volgende doen: 

    a. Zoek in de uitbreiding beleid, de `<ContentDefinition>` knooppunt met `Id="api.selfasserted"`, `Id="api.localaccountsignup"`, en `Id="api.localaccountpasswordreset"`.

    b. Stel de `LoadUri` kenmerk uw *selfasserted* URI.

3. Ga terug naar 'Stap 8.2' en wijzigen van uw code voor het accepteren van queryreeksparameters, maar deze tijd de *selfasserted* functie. 

4. Upload de *TrustFrameworkExtensions.xml* beleid, en zorg ervoor dat deze gevalideerd wordt.

5. Uitvoeren van het beleid testen en selecteer vervolgens **nu aanmelden** om het resultaat te bekijken.

## <a name="optional-download-the-complete-policy-files-and-code"></a>(Optioneel) De volledige-beleidsbestanden en code downloaden
* Na het voltooien van de [aan de slag met aangepaste beleidsregels](active-directory-b2c-get-started-custom.md) scenario, het is raadzaam dat u uw scenario maken met behulp van uw eigen aangepaste beleidsbestanden. We hebben opgegeven voor eigen referentie [beleid voorbeeldbestanden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).
* U kunt de code van de volledige downloaden [voorbeeld Visual Studio-oplossing voor verwijzing](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-ui-customization).




