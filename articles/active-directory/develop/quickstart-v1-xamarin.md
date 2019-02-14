---
title: Aan de slag met Azure AD Xamarin | Microsoft Docs
description: Bouw Xamarin-toepassingen die integreren met Azure AD voor aanmelding en roep Azure AD-beveiligde API's aan met behulp van OAuth.
services: active-directory
documentationcenter: xamarin
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6266ec1f01a50756f745c3e8185c9fe34e102b4a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56196192"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>Quickstart: Bouw een Xamarin-app die Microsoft-aanmelding integreert

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Met Xamarin kunt u mobiele apps schrijven in C# die kunnen worden uitgevoerd in iOS, Android en Windows (mobiele apparaten en pc's). Als u een app bouwt met behulp van Xamarin, kunt u met Azure Active Directory (Azure AD) op eenvoudige wijze gebruikers verifiëren met behulp van hun Azure AD-accounts. De app kan ook veilig gebruikmaken van web-API's die worden beveiligd door Azure AD, zoals de Office 365 API's of de Azure API.

Azure AD biedt de ADAL (Active Directory Authentication Library) voor Xamarin-apps die toegang nodig hebben tot beveiligde bronnen. Het enige doel van ADAL is het ophalen van toegangstokens gemakkelijker te maken voor apps. Om te laten zien hoe eenvoudig dat is, laat dit artikel zien hoe DirectorySearcher-apps kunnen worden gebouwd die:

* Worden uitgevoerd op iOS, Android, Windows-pc, Windows Phone en Windows Store.
* Gebruikmaken van één overdraagbare klassenbibliotheek (PCL) om gebruikers te verifiëren en tokens op te halen voor de Azure AD Graph API.
* Zoeken naar een map voor gebruikers met een gegeven UPN.

## <a name="prerequisites"></a>Vereisten

* Download het [raamwerk van het project](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) of download het [voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Elke download is een Visual Studio 2013-oplossing.
* U hebt ook een Azure AD-tenant nodig waarin u gebruikers kunt maken en de toepassing kunt registreren. [Lees hier hoe u een tenant kunt verkrijgen](quickstart-create-new-tenant.md) als u er nog geen hebt.

Wanneer u klaar bent, volgt u de procedures in de volgende vier gedeelten.

## <a name="step-1-set-up-your-xamarin-development-environment"></a>Stap 1: De Xamarin-ontwikkelomgeving instellen

Omdat deze zelfstudie projecten voor iOS, Android en Windows bevat, hebt u zowel Visual Studio als Xamarin nodig. Voor het maken van de noodzakelijke omgeving voert u het proces uit in [Visual Studio en Xamarin instellen en installeren](https://msdn.microsoft.com/library/mt613162.aspx) op MSDN. De instructies bevatten materiaal met meer informatie over Xamarin dat u kunt doornemen terwijl u wacht totdat de volledige installatie is uitgevoerd.

Nadat de installatie is voltooid, opent u de oplossing in Visual Studio. Daar vindt u zes projecten: vijf platform-specifieke projecten en één PCL, DirectorySearcher.cs, die wordt gedeeld met alle platforms.

## <a name="step-2-register-the-directorysearcher-app"></a>Stap 2: De DirectorySearcher-app registreren

Als u de app wilt instellen voor het ophalen van tokens, moet u de app eerst registreren in uw Azure AD-tenant en de app toegang geven tot de Azure AD Graph API. Dit doet u al volgt:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op uw account op de bovenste balk. Kies vervolgens onder **Directory** de Active Directory-tenant waar u de app wilt registreren.
3. Selecteer **Alle services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App-registraties** en selecteer vervolgens **Toevoegen**.
5. Volg de aanwijzingen voor het maken van een nieuwe **systeemeigen clienttoepassing**.
  * **Naam** beschrijft de app voor gebruikers.
  * **Omleidings-URI** is een combinatie van een schema en een tekenreeks die door Azure AD wordt gebruikt om tokenantwoorden te retourneren. Voer een waarde in, bijvoorbeeld http://DirectorySearcher).
6. Wanneer de registratie is voltooid, wijst Azure AD een unieke toepassings-id toe aan uw app. Kopieer de waarde vanaf het tabblad **Toepassing**, omdat u die later nodig hebt.
7. Selecteer op de pagina **Instellingen** de **Vereiste machtigingen** en selecteer vervolgens **Toevoegen**.
8. Selecteer **Microsoft Graph** als API. Voeg onder **Gedelegeerde machtigingen** de machtiging **Mapgegevens lezen** toe. Hierdoor kan de app de Graph API voor gebruikers doorzoeken.

## <a name="step-3-install-and-configure-adal"></a>Stap 3: ADAL installeren en configureren

Nu u een toepassing hebt in Azure AD, kunt u ADAL installeren en uw aan identiteit gerelateerde code schrijven. Als u wilt dat ADAL kan communiceren met Azure AD, moet u ADAL voorzien van bepaalde informatie gegevens over de app-registratie.

1. Voeg ADAL toe aan het DirectorySearcher-project met behulp van de Package Manager Console.

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
    `

    `
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
    `

    Er worden aan elk project twee bibliotheekverwijzingen toegevoegd: het PCL-gedeelte van ADAL en een platform-specifiek gedeelte.
2. Open DirectorySearcher.cs in het project DirectorySearcherLib.
3. Vervang de waarden van de klasseleden door de waarden die u hebt ingevoerd in de Azure-portal. Uw code verwijst naar deze waarden wanneer deze gebruikmaakt van ADAL.

  * De *tenant* is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com.
  * De *clientId* is de client-id van de app, die u vanuit de portal hebt gekopieerd.
  * De *returnUri* is de omleidings-URI die u in de portal hebt ingevoerd, bijvoorbeeld http://DirectorySearcher).

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>Stap 4: ADAL gebruiken om tokens op te halen uit Azure AD

Bijna alle verificatielogica van de app ligt opgesloten in `DirectorySearcher.SearchByAlias(...)`. In de platform-specifieke projecten hoeft alleen maar een contextuele parameter te worden doorgegeven aan de `DirectorySearcher`-PCL.

1. Open DirectorySearcher.cs en voeg een nieuwe parameter toe aan de `SearchByAlias(...)`-methode. `IPlatformParameters` is de contextuele parameter waarin de platform-specifieke objecten zijn opgenomen die ADAL nodig heeft om de verificatie uit te voeren.

    ```csharp
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. Initialiseer `AuthenticationContext`, de primaire klasse van ADAL. Deze actie geeft aan ADAL de coördinaten door die nodig zijn om te communiceren met Azure AD.
3. Roep `AcquireTokenAsync(...)` aan om het `IPlatformParameters`-object te accepteren en de verificatiestroom aan te roepen die nodig is om een token naar de app te retourneren.

    ```csharp
    ...
        AuthenticationResult authResult = null;
        try
        {
            AuthenticationContext authContext = new AuthenticationContext(authority);
            authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
        }
        catch (Exception ee)
        {
            results.Add(new User { error = ee.Message });
            return results;
        }
    ...
    ```

    `AcquireTokenAsync(...)` probeert eerst een token voor de aangevraagde resource te retourneren (in dit geval de Graph API) zonder dat gebruikers hun referenties hoeven in te voeren (via caching of het vernieuwen van oude tokens). Gebruikers krijgen zo nodig de Azure AD-aanmeldingspagina te zien voordat het aangevraagde token wordt verkregen.
4. Koppel het toegangstoken aan de Graph API-aanvraag in de kop **Autorisatie**:

    ```csharp
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

Dat was het wat betreft de `DirectorySearcher`-PCL en de aan identiteit gerelateerde code van de app. Het enige wat nog te doen staat, is het aanroepen van de `SearchByAlias(...)`-methode in de weergaven van elk platform en, waar nodig, het toevoegen van code voor het correct verwerken van de levenscyclus van de gebruikersinterface.

### <a name="android"></a>Android
1. Voeg in MainActivity.cs een aanroep toe aan `SearchByAlias(...)` in de handler voor klikken:

    ```csharp
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. Overschrijf de `OnActivityResult`-levenscyclusmethode voor het terugsturen van omgeleide verificaties naar de juiste methode. ADAL biedt daarvoor een Help-methode in Android:

    ```csharp
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a>Windows-pc

Voer in MainWindow.xaml.cs een aanroep uit naar `SearchByAlias(...)` door een `WindowInteropHelper` door te geven in het `PlatformParameters`-object op de pc:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
In DirSearchClient_iOSViewController.cs verwijst het iOS `PlatformParameters`-object naar de View Controller:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Windows Universal
Open MainPage.xaml.cs in Windows Universal en implementeer vervolgens de `Search`-methode. Deze methode maakt gebruik van een Help-methode in een gedeeld project om de gebruikersinterface waar nodig bij te werken.

```csharp
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

U hebt nu een werkende Xamarin-app waarmee u gebruikers kunt verifiëren en veilig web-API's kunt aanroepen met behulp van OAuth 2.0 op vijf verschillende platforms.

## <a name="step-5-populate-your-tenant"></a>Stap 5: Uw tenant vullen 

Nu kunt u de tenant met gebruikers gaan vullen als u dat nog niet hebt gedaan.

1. Voer uw DirectorySearcher-app uit en meld u aan met een van deze gebruikers.
2. Zoek andere gebruikers op basis van hun UPN.

## <a name="next-steps"></a>Volgende stappen

ADAL maakt het opnemen van algemene identiteitsfuncties in de app eenvoudig. ADAL neemt alle vervelende klusjes voor zijn rekening, zoals cachebeheer, OAuth-protocolondersteuning, het aanbieden van een gebruikersinterface waarmee de gebruiker zich kan aanmelden, en het vernieuwen van verlopen tokens. U hoeft slechts één API-aanroep te kennen, `authContext.AcquireToken*(…)`.

* Download het [volledige voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (zonder uw configuratiewaarden).
* Ga naar [Een .NET Web API beveiligen met Azure AD ](quickstart-v1-dotnet-webapi.md).
