---
title: Azure AD v1 ASP.NET webserver aan de slag | Microsoft Docs
description: Implementeren van Microsoft-aanmeldingspagina op een ASP.NET-oplossing met een traditioneel browser gebaseerde webtoepassing met behulp van standaard OpenID Connect
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/08/2017
ms.author: andret
ms.openlocfilehash: b23afd26f7ac1828381a0410d2455206c8f43c88
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
<!--start-intro-->
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Aanmelden met Microsoft toevoegen aan een ASP.NET-web-app

Deze handleiding wordt uitgelegd hoe aanmelden bij Microsoft met behulp van een ASP.NET MVC-oplossing met een traditioneel browser gebaseerde webtoepassing met OpenID Connect implementeren. 

Aan het einde van deze handleiding, uw toepassing accepteert aanmeldingen van werk- en schoolaccounts accounts van organisaties die zijn geïntegreerd met Azure Active Directory.

> [!NOTE]
> Deze begeleide setup helpt u bij het inschakelen van de aanmeldingen van werk- en schoolaccounts accounts in uw ASP.NET-toepassing. Als u geïnteresseerd om in te schakelen aanmeldingen voor persoonlijke accounts naast werk-en schoolaccounts bent, kunt u de [v2 eindpunt](../active-directory-v2-compare.md). Zie [deze ASP.NET-instelling voor het eindpunt v2 begeleide](./active-directory-aspnetwebapp.md) , evenals [dit document](../active-directory-v2-limitations.md) uitleg over de huidige beperkingen van het v2-eindpunt.
<br/><br/>

<!--separator-->

> Deze handleiding is Visual Studio 2015 Update 3 of Visual Studio 2017 vereist.  Hebt u geen deze?  [Visual Studio 2017 gratis downloaden](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding](media/active-directory-aspnetwebapp-v1/aspnet-intro.png)

Deze handleiding is gebaseerd op het scenario waarbij een browser toegang heeft tot een ASP.NET-website aanvragen van een gebruiker worden geverifieerd via een knop aanmelden. In dit scenario treedt de meeste werk voor het weergeven van de webpagina op aan de serverzijde.

> [!NOTE]
> Deze Begeleide instelprocedure laat zien hoe melden gebruikers op een ASP.NET-webtoepassing die beginnen met een lege sjabloon en stappen zoals het toevoegen van een teken in de knop en elke domeincontroller en de methoden, terwijl het ook uitleg over enkele concepten bevatten. U kunt ook kunt u ook een project voor aanmelding bij Azure Active Directory maakt gebruikers (werk- en schoolaccounts-accounts) met behulp van de [Visual Studio-web-sjabloon](https://docs.microsoft.com/aspnet/visual-studio/overview/2013/creating-web-projects-in-visual-studio#organizational-account-authentication-options) en het selecteren van *Organisatieaccounts* en vervolgens een van de opties voor cloud - deze optie maakt gebruik van een uitgebreidere sjabloon met extra domeincontrollers, methoden en weergaven.

## <a name="libraries"></a>Bibliotheken

Deze handleiding bevat de volgende pakketten:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware waarmee een toepassing wilt OpenIdConnect gebruiken voor verificatie|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware waarmee een toepassing te onderhouden met behulp van cookies gebruikerssessie|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Kunnen worden uitgevoerd op IIS met de ASP.NET-aanvraag voor pipeline-toepassingen op basis van een OWIN|


<!--end-intro-->

<!--start-setup-->

## <a name="set-up-your-project"></a>Instellen van uw project

Deze sectie bevat de stappen voor het installeren en configureren van de verificatiepijplijn via OWIN middleware op een ASP.NET-project met OpenID Connect. 

> Voorkeur voor het downloaden van dit voorbeeld Visual Studio-project in plaats daarvan? [Downloaden van een project](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/GuidedSetup.zip) en doorgaan met de [configuratiestap](#configure-your-webconfig-and-register-an-application) voor het configureren van het codevoorbeeld voordat wordt uitgevoerd.

## <a name="create-your-aspnet-project"></a>Uw ASP.NET-project maken
1. In Visual Studio:`File` > `New` > `Project`<br/>
2. Onder *Visual C# \Web*, selecteer `ASP.NET Web Application (.NET Framework)`.
3. Naam van uw toepassing en klik op *OK*
4. Selecteer `Empty` en selecteer vervolgens het selectievakje in om toe te voegen `MVC` verwijzingen

## <a name="add-authentication-components"></a>Verificatieonderdelen toevoegen

1. In Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Voeg *OWIN middleware NuGet-pakketten* door het volgende te typen in het venster Package Manager-Console:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-packages"></a>Over deze pakketten
>De bovenstaande bibliotheken Schakel eenmalige aanmelding (SSO) met OpenID Connect via authenticatie op basis van een cookie. Nadat de verificatie is voltooid en het token voor de gebruiker wordt verzonden naar uw toepassing, maakt OWIN middleware een sessiecookie. De browser gebruikt deze cookie vervolgens bij volgende aanvragen, zodat de gebruiker hoeft niet te verifiëren en geen aanvullende verificatie nodig is.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>De verificatiepijplijn configureren
De volgende stappen gebruikt bij het maken van een OWIN-middleware *Opstartklasse* OpenID Connect verificatie configureren. Deze klasse wordt automatisch uitgevoerd.

> [!TIP]
> Als uw project beschikt niet over een `Startup.cs` bestand in de hoofdmap:<br/>
> 1. Met de rechtermuisknop op de hoofdmap van het project: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Geef deze de naam`Startup.cs`<br/>
>
>> Zorg ervoor dat de geselecteerde klasse is een OWIN-Opstartklasse en niet een standaard C#-klasse. Dit controleren door te controleren of er `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` boven de naamruimte.


1. Voeg *OWIN* en *Microsoft.IdentityModel* naamruimten aan `Startup.cs`:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=AddedNameSpaces "Startup.cs")]

2. Opstartklasse vervangen door de volgende code:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=Startup "Startup.cs")]
    
<!--start-collapse-->
> [!NOTE]
> De parameters die u opgeeft in *OpenIDConnectAuthenticationOptions* fungeren als coördinaten voor de toepassing om te communiceren met Azure AD. Omdat de middleware OpenID Connect worden cookies gebruikt, moet u ook cookie-verificatie instellen als de voorgaande code wordt getoond. De *ValidateIssuer* waarde vertelt OpenIdConnect niet toegang te beperken tot een specifieke organisatie.
<!--end-collapse-->

<!--end-setup-->

<!--start-use-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Toevoegen van een domeincontroller voor het afhandelen van aanmelden en afmeldingsaanvragen te verzenden

Deze stap ziet een nieuwe domeincontroller om aan- en afmeldingsaanvragen methoden zichtbaar te maken.

1.  Met de rechtermuisknop op de `Controllers` map en selecteer`Add` > `Controller`
2.  Selecteer `MVC (.NET version) Controller – Empty`.
3.  Klik op *toevoegen*
4.  Naam `HomeController` en klik op *toevoegen*
5.  Voeg *OWIN* naamruimten aan de klasse:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=AddedNameSpaces "HomeController.cs")]

6. Voeg de volgende methoden voor het initiëren van een verificatievraag via code voor het afhandelen van aanmelden en afmelden bij de domeincontroller:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=SigInAndSignOut "HomeController.cs")]
    
## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>De startpagina van de app aan te melden gebruikers via een knop maken

Maak een nieuwe weergave voor het toevoegen van de knop aanmelden en gebruikersgegevens na verificatie weergeven in Visual Studio:

1.  Met de rechtermuisknop op de `Views\Home` map en selecteer`Add View`
2.  Noem deze `Index`.
3.  De volgende HTML-code, waaronder de knop aanmelden toevoegen aan het bestand:

    [!code-html[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Home/Index.cshtml "Index.cshtml")]

<!--start-collapse-->
> [!NOTE]
> Deze pagina wordt de knop aanmelden in SVG-indeling met een zwarte achtergrond toegevoegd:<br/>![Aanmelden met Microsoft](media/active-directory-aspnetwebapp-v1/aspnetsigninbuttonsample.png)<br/> Ga voor meer aanmelden knoppen naar [deze pagina](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "richtlijnen huisstijl").
<!--end-collapse-->

## <a name="display-users-claims-by-adding-a-controller"></a>Claims van de gebruiker weergeven door het toevoegen van een domeincontroller
Deze domeincontroller ziet u het gebruik van de `[Authorize]` kenmerk voor het beveiligen van een domeincontroller. Dit kenmerk wordt toegang tot de controller beperken door alleen geverifieerde gebruikers. De volgende code maakt gebruik van het kenmerk om claims van de gebruiker die zijn opgehaald als onderdeel van de aanmeldingspagina weer te geven.

1.  Met de rechtermuisknop op de `Controllers` map:`Add` > `Controller`
2.  Selecteer `MVC {version} Controller – Empty`.
3.  Klik op *toevoegen*
4.  Geef deze de naam`ClaimsController`
5.  Vervang de code van de klasse van uw domeincontroller met de volgende code - Hiermee voegt u de `[Authorize]` kenmerk aan de klasse:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\ClaimsController.cs?name=ClaimsController "ClaimsController.cs")]

<!--start-collapse-->
> [!NOTE]
> Vanwege het gebruik van de `[Authorize]` kenmerk, alle methoden van deze domeincontroller kan alleen worden uitgevoerd als de gebruiker is geverifieerd. Als de gebruiker niet is geverifieerd en probeert te krijgen tot de domeincontroller, OWIN een verificatievraag Start en het afdwingen van de gebruiker te verifiëren. De bovenstaande code gekeken naar de verzameling claims van de gebruiker naar specifieke kenmerken die zijn opgenomen in het token van de gebruiker. Deze kenmerken zijn de volledige naam van de gebruiker en gebruikersnaam, evenals het onderwerp globale gebruiker-id. Bevat ook de *Tenant-ID*, die staat voor de ID voor de organisatie van de gebruiker. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Een weergave voor de claims van de gebruiker maken

Maak een nieuwe weergave voor de claims van de gebruiker in een webpagina in Visual Studio:

1.  Met de rechtermuisknop op de `Views\Claims` map en:`Add View`
2.  Noem deze `Index`.
3.  De volgende HTML-code toevoegen aan het bestand:

    [!code-html[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Claims/Index.cshtml "Index.cshtml")]
    
<!--end-use-->

<!--start-configure-->
## <a name="configure-your-webconfig-and-register-an-application"></a>Configureer uw *web.config* en een toepassing registreren

1. Voeg het volgende wanneer u in Visual Studio `web.config` (te vinden in de hoofdmap) onder de sectie `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="RedirectUrl" value="Enter_the_Redirect_Url_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
    ```
2. Selecteer het project in Solution Explorer en bekijk de <i>eigenschappen</i> venster (als er geen een venster met eigenschappen, druk op F4)
3. SSL ingeschakeld om te wijzigen<code>True</code>
4. SSL-URL van het project naar het Klembord kopiëren:<br/><br/>![Projecteigenschappen](media/active-directory-aspnetwebapp-v1/visual-studio-project-properties.png)<br />
5. In <code>web.config</code>, Vervang <code>Enter_the_Redirect_URL_here</code> met de SSL-URL van uw project 

### <a name="register-your-application-in-the-azure-portal-then-add-its-information-to-webconfig"></a>Uw toepassing registreren in de Azure Portal, en voeg vervolgens de informatie naar *web.config*

1. Ga naar de [Microsoft Azure Portal - App registraties](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) een toepassing registreren
2. Selecteer `New application registration`
3. Voer een naam voor uw toepassing
4. Visual Studio-project plakken *SSL URL* in `Sign-on URL` (deze URL wordt ook automatisch toegevoegd aan de lijst met antwoord-URL's voor de toepassing die u registreert)
5. Klik op `Create` om de toepassing te registreren. Deze actie keert u terug naar de lijst met toepassingen
6. Nu zoeken en/of Selecteer de toepassing die u zojuist hebt gemaakt om de eigenschappen te openen
7. Kopieer de guid onder `Application ID` naar het Klembord
8. Ga terug naar Visual Studio en, in `web.config`, Vervang `Enter_the_Application_Id_here` met de toepassings-ID van de toepassing die u zojuist hebt geregistreerd

> [!TIP]
> Als uw account is geconfigureerd voor toegang tot meerdere mappen, zorg ervoor dat u hebt geselecteerd de juiste map voor de organisatie u wilt dat de toepassing worden geregistreerd door te klikken op de accountnaam van uw in de rechterbovenhoek rechts in de Azure Portal en vervolgens verifiëren van de geselecteerde map aangegeven:<br/>![De juiste map selecteren](media/active-directory-aspnetwebapp-v1/tenantselector.png)

## <a name="configure-sign-in-options"></a>Aanmeldingsopties configureren

U kunt uw toepassing configureren voor dat alleen gebruikers die deel uitmaken van een organisatie Azure Active Directory-exemplaar aan te melden, of accepteer de aanmeldingen van gebruikers die deel uitmaken van een organisatie. Volg de instructies van een van de volgende opties:

### <a name="configure-your-application-to-allow-sign-ins-of-work-and-school-accounts-from-any-company-or-organization-multi-tenant"></a>Uw toepassing configureren voor het toestaan aanmeldingen van werk- en schoolaccounts accounts van een bedrijf of organisatie (multitenant)

Volg de volgende stappen uit als u wilt accepteren aanmeldingen van werk- en schoolaccounts accounts van een bedrijf of organisatie die is geïntegreerd met Azure Active Directory. Dit is een veelvoorkomend scenario voor *SaaS-toepassingen*:

1. Ga terug naar [Microsoft Azure Portal - App registraties](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) en zoek de toepassing die u zojuist hebt geregistreerd
2. Onder `All Settings` selecteren`Properties`
3. Wijziging `Multi-tenanted` eigenschap `Yes` en klikt u op`Save`

Zie voor meer informatie over deze instelling en het concept van multitenant-toepassingen [in dit artikel](../active-directory-devhowto-multi-tenant-overview.md "multitenant overzicht").

### <a name="restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Voorkomen dat gebruikers slechts één organisatie Active Directory-exemplaar aan te melden bij uw toepassing (single-tenant)

Deze optie is een veelvoorkomend scenario voor *LOB-toepassingen*: als u wilt dat uw toepassing accepteren aanmeldingen alleen van accounts die deel uitmaken van een specifiek exemplaar van Azure Active Directory (inclusief *gastaccounts*van dat exemplaar), vervang de `Tenant` parameter in *web.config* van `Common` met de naam van de tenant van de organisatie – bijvoorbeeld *contoso.onmicrosoft.com*. Daarna wijzigen de `ValidateIssuer` argument in uw [ *OWIN-Opstartklasse* ](#configure-the-authentication-pipeline) naar `true`.

Instellen zodat gebruikers alleen een lijst met specifieke organisaties `ValidateIssuer` op waar en gebruik de `ValidIssuers` parameter om een lijst met organisaties.

Er is een andere optie voor het implementeren van een aangepaste methode voor het valideren van de uitgevers van certificaten met behulp van *IssuerValidator* parameter. Voor meer informatie over `TokenValidationParameters`, Zie [deze MSDN-artikel](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN-artikel").

<!--end-configure-->

<!--start-configure-arp-->
<!--
## Configure your ASP.NET Web App with the application's registration information

In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information. After this, add the application’ registration information to your solution via *web.config*.

1.  In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)
2.  Change `SSL Enabled` to `True`
3.  Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:<br/><br/>![Project properties](media/active-directory-aspnetwebapp-v1/vsprojectproperties.png)<br />
4.  Add the following in `web.config` file located in root’s folder, under section `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
```
-->
<!--end-configure-arp-->
<!--start-test-->
## <a name="test-your-code"></a>Testen van uw code

Druk op `F5` uw project in Visual Studio wilt uitvoeren. De browser wordt geopend en wordt u verwezen *http://localhost: {poort}* waarin u zien hoe de *aanmelden met Microsoft* knop. Opwekken en klik op aan te melden.

Als u klaar bent om te testen, gebruikt een werkaccount (Azure Active Directory) aan te melden. 

![Meld u aan met de Microsoft-browservenster](media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin.png)

![Meld u aan met de Microsoft-browservenster](media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Verwachte resultaten
Na het aanmelden, wordt de gebruiker omgeleid naar de startpagina van uw website, de HTTPS-URL die is opgegeven in de registratiegegevens van uw toepassing in de Registratieportal voor Microsoft-toepassing wordt. Deze pagina wordt nu *Hallo {User}* en een koppeling om af te melden en een koppeling om te zien van claims van de gebruiker – dit is een koppeling naar de controller autoriseren eerder hebt gemaakt.

### <a name="see-users-claims"></a>Zie de claims van de gebruiker
Selecteer de hyperlink om te zien van claims van de gebruiker. Deze actie leidt u naar de domeincontroller en de weergave die alleen beschikbaar voor gebruikers die zijn geverifieerd.

#### <a name="expected-results"></a>Verwachte resultaten
 Hier ziet u een tabel met de basiseigenschappen van de aangemelde gebruiker:

| Eigenschap | Waarde | Beschrijving|
|---|---|---|
| Naam | {De volledige gebruikersnaam} | De voor- en achternaam van de gebruiker
|Gebruikersnaam | <span>user@domain.com</span>| De gebruikersnaam die wordt gebruikt voor het identificeren van de aangemelde gebruiker
| Onderwerp| {Onderwerp}|Een tekenreeks als unieke identificatie van de aanmelding van de gebruiker op het web|
| Tenant-id| {Guid}| Een *guid* uniek vertegenwoordigt de organisatie Azure Active Directory van de gebruiker.|

Bovendien ziet u een tabel met inbegrip van alle gebruikersclaims in verificatieaanvraag opgenomen. Zie voor een lijst van alle claims in een ID Token en de bijbehorende uitleg [artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lijst van Claims in ID Token").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Test een methode die heeft toegang tot een *[autoriseren]* kenmerk (optioneel)
In deze stap test u toegang tot de controller Claims als anonieme gebruiker:<br/>
Selecteer de koppeling voor afmelden van de gebruiker en de afmelden proces te voltooien.<br/>
Typ nu in uw browser http://localhost: {poort} / claims voor toegang tot uw domeincontroller die is beveiligd met de `[Authorize]` kenmerk

#### <a name="expected-results"></a>Verwachte resultaten
U ontvangt de prompt dat u hoeft te worden geverifieerd om te zien van de weergave.

## <a name="additional-information"></a>Aanvullende informatie

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Beveiligen van uw hele website
Omwille van de gehele website toevoegen de `AuthorizeAttribute` naar `GlobalFilters` in `Global.asax` `Application_Start` methode:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

<!--end-test-->