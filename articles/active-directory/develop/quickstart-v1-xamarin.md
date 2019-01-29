---
title: Azure AD-Xamarin aan de slag | Microsoft Docs
description: Xamarin toepassingen bouwen die integreren met Azure AD voor aanmelding bij Azure AD-beveiligde API's en aanroepen met behulp van OAuth.
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
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 5b721dfd7a229220836f273be58c5ca74c4284d1
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55097926"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>Quickstart: Bouw een Xamarin-app die is geïntegreerd Microsoft-aanmelding

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Met Xamarin, kunt u mobiele apps schrijven in C# dat kan worden uitgevoerd op iOS, Android en Windows (mobiele apparaten en pc's). Als u een app met behulp van Xamarin, met Azure Active Directory (Azure AD) kunt eenvoudig gebruikers verifiëren met hun Azure AD-accounts kunnen worden gebruikt. De app kan ook veilig gebruiken voor elke web-API die wordt beveiligd door Azure AD, zoals de Office 365 API's of de API van Azure.

Azure AD biedt voor Xamarin-apps die toegang moeten krijgen tot beveiligde bronnen, de Active Directory Authentication Library (ADAL). Het enige doel van ADAL is gemakkelijker voor apps om toegangstokens. Als u wilt laten zien hoe eenvoudig het is, in dit artikel laat zien hoe DirectorySearcher apps bouwen die:

* Uitgevoerd op iOS, Android, Windows-bureaublad, Windows Phone en Windows Store.
* Een enkele draagbare klassenbibliotheek (PCL) gebruiken om te verifiëren van gebruikers en tokens verkrijgen voor de Azure AD Graph API.
* Zoeken naar een map voor gebruikers met een UPN die is opgegeven.

## <a name="prerequisites"></a>Vereisten

* Download de [basisproject](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), of download de [voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Elke download is een Visual Studio 2013-oplossing.
* U moet ook een Azure AD-tenant in voor het maken van gebruikers en de app te registreren. [Lees hier hoe u een tenant kunt verkrijgen](quickstart-create-new-tenant.md) als u er nog geen hebt.

Wanneer u klaar bent, volgt u de procedures in de volgende vier secties.

## <a name="step-1-set-up-your-xamarin-development-environment"></a>Stap 1: Uw Xamarin ontwikkelomgeving instellen

Omdat deze zelfstudie bevat de projecten voor iOS, Android en Windows, moet u zowel Visual Studio en Xamarin. Voor het maken van de noodzakelijke omgeving, voltooi het proces in [ingesteld omhoog en installeer Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) op MSDN. De instructies bevatten materiaal dat u bekijken kunt voor meer informatie over Xamarin terwijl u wacht voor installaties worden uitgevoerd.

Nadat u de installatie hebt voltooid, opent u de oplossing in Visual Studio. Hier vindt u zes projecten: vijf platform-specifieke projecten en één PCL, DirectorySearcher.cs, die wordt gedeeld met alle platforms.

## <a name="step-2-register-the-directorysearcher-app"></a>Stap 2: De DirectorySearcher-app te registreren

Als u wilt inschakelen voor de app tokens verkrijgen, moet u eerst deze te registreren in uw Azure AD-tenant en verleent deze machtiging voor toegang tot de Azure AD Graph API. Dit doet u al volgt:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Op de bovenste balk, klikt u op uw account. Klik vervolgens onder de **Directory** , selecteert u de Active Directory-tenant waar u de app te registreren.
3. Selecteer **Alle services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App-registraties** en selecteer vervolgens **Toevoegen**.
5. Maak een nieuwe **systeemeigen clienttoepassing**, volgt u de stappen.
  * **Naam** beschrijving van de app aan gebruikers.
  * **Omleidings-URI** is een combinatie van een schema en een tekenreeks die door Azure AD wordt gebruikt om tokenantwoorden te retourneren. Voer een waarde (bijvoorbeeld http://DirectorySearcher).
6. Nadat u de registratie hebt voltooid, wijst Azure AD de app een unieke toepassings-ID. Kopieer de waarde van de **toepassing** tabblad, omdat u hebt deze later nodig.
7. Op de **instellingen** weergeeft, schakelt **vereiste machtigingen**, en selecteer vervolgens **toevoegen**.
8. Selecteer **Microsoft Graph** als de API. Onder **gedelegeerde machtigingen**, voeg de **mapgegevens lezen** machtiging. Hierdoor is de app om op te vragen van de Graph-API voor gebruikers.

## <a name="step-3-install-and-configure-adal"></a>Stap 3: Installeren en configureren van ADAL

Nu dat u een app in Azure AD hebt, kunt u ADAL installeert en uw identiteit gerelateerde code te schrijven. Om in te schakelen van ADAL om te communiceren met Azure AD, geeft deze informatie over de app-registratie.

1. ADAL toevoegen aan het project DirectorySearcher met behulp van de Package Manager-Console.

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

    Houd er rekening mee dat twee bibliotheekverwijzingen worden toegevoegd aan elk project: het gedeelte PCL van ADAL en een platform-specifieke gedeelte.
2. Open in het project DirectorySearcherLib DirectorySearcher.cs.
3. De klasse waarden vervangen door de waarden die u hebt ingevoerd in de Azure-portal. Uw code zal naar deze waarden verwijzen wanneer deze gebruikmaakt van ADAL.

  * De *tenant* is het domein van uw Azure AD-tenant (bijvoorbeeld contoso.onmicrosoft.com).
  * De *clientId* is de client-ID van de app, die u hebt gekopieerd uit de portal.
  * De *returnUri* is de omleidings-URI die u hebt ingevoerd in de portal (bijvoorbeeld http://DirectorySearcher).

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>Stap 4: Gebruikmaken van ADAL om op te halen van tokens van Azure AD

Bijna alle van de app verificatielogica ligt in `DirectorySearcher.SearchByAlias(...)`. In de platform-specifieke projecten nodig is om door te geven van een contextuele parameter voor de `DirectorySearcher` PCL.

1. Open DirectorySearcher.cs en voegt u een nieuwe parameter voor de `SearchByAlias(...)` methode. `IPlatformParameters` is de contextuele parameter die kapselt de platform-specifieke objecten die ADAL moet de verificatie uitvoeren.

    ```csharp
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. Initialiseren `AuthenticationContext`, dit is de primaire klasse van ADAL. Deze actie wordt doorgegeven ADAL de coördinaten die nodig is om te communiceren met Azure AD.
3. Bel `AcquireTokenAsync(...)`, die accepteert de `IPlatformParameters` object en roept de verificatiestroom dat nodig is om een token terug naar de app.

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

    `AcquireTokenAsync(...)` eerst probeert te retourneren van een token voor de aangevraagde resource (in dit geval de Graph-API) zonder dat gebruikers hun referenties invoeren (via opslaan in cache of vernieuwen van oude tokens) wordt gevraagd. Indien nodig toont het gebruikers de Azure AD-aanmeldingspagina vóór het aangevraagde token verkrijgen.
4. Het toegangstoken koppelen aan de Graph API-aanvraag in de **autorisatie** header:

    ```csharp
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

Dat is alles voor de `DirectorySearcher` PCL en de app de identiteit gerelateerde code. Alles wat blijft is om aan te roepen de `SearchByAlias(...)` methode in de weergaven van elk platform en, indien nodig, voegt u code voor het correct verwerken van de levenscyclus van de gebruikersinterface toe.

### <a name="android"></a>Android
1. In MainActivity.cs, Voeg een aanroep naar `SearchByAlias(...)` op de knop klikt u op de handler:

    ```csharp
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. Overschrijf de `OnActivityResult` lifecycle-methode voor het doorsturen van de verificatie wordt omgeleid naar de juiste methode. ADAL biedt een Help-methode voor deze in Android:

    ```csharp
    ...
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ...
    ```

### <a name="windows-desktop"></a>Windows-bureaublad

Controleer in MainWindow.xaml.cs, een aanroep van `SearchByAlias(...)` door door te geven een `WindowInteropHelper` in van het bureaublad `PlatformParameters` object:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
In DirSearchClient_iOSViewController.cs, de iOS `PlatformParameters` object wordt een verwijzing naar de View-Controller:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Windows Universal
Open MainPage.xaml.cs in Windows Universal, en vervolgens implementeert u de `Search` methode. Deze methode maakt gebruik van een Help-methode in een gedeelde-project bij te werken van de gebruikersinterface als nodig.

```csharp
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

U hebt nu een werkende Xamarin-app die u kunt gebruikers verifiëren en veilig web-API's aanroepen met behulp van OAuth 2.0 op vijf verschillende platforms.

## <a name="step-5-populate-your-tenant"></a>Stap 5: Uw tenant vullen 

Als u al uw tenant met gebruikers nog niet hebt ingevuld, is nu tijd om dit te doen.

1. Voer uw app DirectorySearcher uit en meld u aan met een van de gebruikers.
2. Zoek andere gebruikers op basis van hun UPN.

## <a name="next-steps"></a>Volgende stappen

ADAL kunt eenvoudig algemene identiteitsfuncties opnemen in de app. Dit zorgt dat al het werk dirty voor u, zoals Cachebeheer, OAuth-protocolondersteuning, dat de gebruiker met een gebruikersinterface, en vernieuwen van tokens verlopen. U moet weten slechts één API-aanroep, `authContext.AcquireToken*(…)`.

* Download de [voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (zonder uw configuratiewaarden).
* Meer informatie over het [beveiligen van een .NET-Web-API met Azure AD](quickstart-v1-dotnet-webapi.md).
