---
title: Azure AD-Xamarin aan de slag | Microsoft Docs
description: Xamarin-toepassingen die integreren met Azure AD voor aanmelden en roept u Azure AD-beveiligde API's met OAuth bouwen.
services: active-directory
documentationcenter: xamarin
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 94a7d35115420d455fe94e1173abf76622172f6f
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-ad-xamarin-getting-started"></a>Azure AD-Xamarin aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Met Xamarin, kunt u mobiele apps in C# die kunnen worden uitgevoerd op iOS, Android en Windows (mobiele apparaten en pc's). Als u een app met Xamarin maakt, met Azure Active Directory (Azure AD) kunt u eenvoudig gebruikers worden geverifieerd met hun Azure AD-accounts kunnen worden gebruikt. De app kan ook veilig een web-API die wordt beveiligd door Azure AD, zoals de Office 365-API of de Azure-API gebruiken.

Azure AD levert voor Xamarin-apps die toegang moeten krijgen tot beveiligde bronnen, de Active Directory Authentication Library (ADAL). Het enige doel van ADAL is gemakkelijk om apps te toegangstokens ophalen. Als u wilt laten zien hoe eenvoudig het is, in dit artikel laat zien hoe DirectorySearcher apps bouwen die:

* Voer op iOS, Android, Windows-bureaublad, Windows Phone en Windows Store.
* Een enkele draagbare klassebibliotheek (PCL) gebruiken voor het verifiëren van gebruikers en tokens krijgen voor de Azure AD Graph API.
* Zoeken in een map voor gebruikers met een opgegeven UPN.

## <a name="before-you-get-started"></a>Voordat u aan de slag gaat
* Download de [basisproject](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip), of download de [voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Elke download is een Visual Studio 2013-oplossing.
* U moet ook een Azure AD-tenant waarin gebruikers maken en de app te registreren. Als u niet al een tenant [Lees hoe u een](active-directory-howto-tenant.md).

Wanneer u klaar bent, voert u de procedures in de volgende vier secties.

## <a name="step-1-set-up-your-xamarin-development-environment"></a>Stap 1: Uw Xamarin-ontwikkelomgeving instellen
Omdat deze zelfstudie projecten voor iOS, Android en Windows omvat, moet u zowel Visual Studio en Xamarin. Voltooi het proces in voor het maken van de benodigde omgeving [ingesteld omhoog en installeer Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) op MSDN. De instructies bevatten materiaal die u bekijken kunt voor meer informatie over Xamarin terwijl u voor de installatie wacht te voltooien.

Nadat u de installatie hebt voltooid, opent u de oplossing in Visual Studio. Daar vindt u zes projecten: vijf platform-specifieke projecten en één PCL, DirectorySearcher.cs, die wordt gedeeld met alle platforms.

## <a name="step-2-register-the-directorysearcher-app"></a>Stap 2: De app DirectorySearcher te registreren
Als u wilt inschakelen voor de app tokens krijgen, moet u eerst registreren in uw Azure AD-tenant en verleent deze machtiging voor toegang tot de Azure AD Graph API. Dit doet u al volgt:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op uw account op de bovenste balk. Klik vervolgens onder de **Directory** , selecteert u de Active Directory-tenant waar u de app te registreren.
3. Klik op **meer Services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App registraties**, en selecteer vervolgens **toevoegen**.
5. Maak een nieuwe **systeemeigen clienttoepassing**, volg de aanwijzingen.
  * **Naam** beschrijving van de app voor gebruikers.
  * **Omleidings-URI** is een combinatie schema en de tekenreeks die gebruikmaakt van Azure AD token antwoorden retourneren. Voer een waarde (bijvoorbeeld http://DirectorySearcher).
6. Nadat u de registratie hebt voltooid, wijst Azure AD de app een unieke toepassings-ID. Kopieer de waarde van de **toepassing** tabblad omdat u hebt deze later nodig.
7. Op de **instellingen** pagina **Required Permissions**, en selecteer vervolgens **toevoegen**.
8. Selecteer **Microsoft Graph** als de API. Onder **gedelegeerde machtigingen**, voeg de **Directory-gegevens lezen** machtiging.  
Deze actie kan de app om op te vragen de Graph-API voor gebruikers.

## <a name="step-3-install-and-configure-adal"></a>Stap 3: Installeren en configureren van ADAL
Nu dat u een app in Azure AD hebt, kunt u ADAL installeert en uw identiteitsgerelateerde code schrijven. Om in te schakelen ADAL om te communiceren met Azure AD, geef deze de enige informatie over de registratie van de app.

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

    Houd er rekening mee dat de twee verwijzingen van de tapewisselaar worden toegevoegd aan elk project: het gedeelte PCL van ADAL en een deel van de platform-specifieke.
2. Open in het project DirectorySearcherLib DirectorySearcher.cs.
3. Vervang de waarden van de lid klasse met de waarden die u hebt ingevoerd in de Azure-portal. Uw code verwijst naar deze waarden wanneer deze gebruikmaakt van ADAL.

  * De *tenant* is het domein van uw Azure AD-tenant (bijvoorbeeld: contoso.onmicrosoft.com).
  * De *clientId* is de client-ID van de app, die u hebt gekopieerd uit de portal.
  * De *returnUri* is de omleidings-URI die u hebt ingevoerd in de portal (bijvoorbeeld http://DirectorySearcher).

## <a name="step-4-use-adal-to-get-tokens-from-azure-ad"></a>Stap 4: Gebruik ADAL tokens ophalen uit Azure AD
Bijna alle van de app verificatielogica ligt in `DirectorySearcher.SearchByAlias(...)`. In de platform-specifieke projecten nodig is om door te geven van een contextuele parameter voor de `DirectorySearcher` PCL.

1. Open DirectorySearcher.cs en voeg vervolgens een nieuwe parameter voor de `SearchByAlias(...)` methode. `IPlatformParameters`de contextuele parameter die kapselt de platform-specifieke objecten die ADAL nodig zijn voor de verificatie is.

    ```csharp
    public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
    {
    ```

2. Initialiseren `AuthenticationContext`, dit is de primaire klasse van ADAL.  
Deze actie geeft ADAL de coördinaten die kan communiceren met Azure AD.
3. Roep `AcquireTokenAsync(...)`, die accepteert de `IPlatformParameters` object en roept de authenticatiestroom dat nodig is om een token terug naar de app.

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

    `AcquireTokenAsync(...)`eerst probeert te retourneren van een token voor de aangevraagde bron (in dit geval de Graph-API) zonder gebruikers hun referenties invoeren (via caching of oude tokens vernieuwen). Zo nodig toont het gebruikers de aanmeldingspagina van Azure AD voordat de aangevraagde token ophalen.
4. Het toegangstoken koppelen aan de Graph API-aanvraag in de **autorisatie** header:

    ```csharp
    ...
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
    ...
    ```

Dit is alles wat voor de `DirectorySearcher` PCL en de app de identiteitsgerelateerde code. Alles wat u hoeft alleen nog aan te roepen de `SearchByAlias(...)` methode in elk platform weergaven en, indien nodig, voeg code toe voor de levenscyclus van de gebruikersinterface correct verwerkt.

### <a name="android"></a>Android
1. In MainActivity.cs, Voeg een aanroep naar `SearchByAlias(...)` op de knop klikt u op de handler:

    ```csharp
    List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
    ```
2. Overschrijf de `OnActivityResult` lifecycle-methode voor het doorsturen van de verificatie die wordt omgeleid naar de juiste methode. ADAL biedt een Help-methode voor dit in Android:

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
Controleer in MainWindow.xaml.cs, een aanroep van `SearchByAlias(...)` door een `WindowInteropHelper` op het bureaublad `PlatformParameters` object:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS
In DirSearchClient_iOSViewController.cs, het bestand iOS `PlatformParameters` object krijgt een verwijzing naar de weergavebesturing:

```csharp
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

### <a name="windows-universal"></a>Windows Universal
In Windows universele MainPage.xaml.cs openen en vervolgens implementeert u de `Search` methode. Deze methode maakt gebruik van een Help-methode in een gedeeld project UI indien nodig bijwerken.

```csharp
...
List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

## <a name="whats-next"></a>Volgend onderwerp
U hebt nu een werkende Xamarin-app die u kunt verificatie van gebruikers en veilig aanroepen van web-API's met behulp van OAuth 2.0 op vijf verschillende platforms.

Als u al uw tenant met gebruikers nog niet hebt ingevuld, is nu tijd om dit te doen.

1. Uitvoeren van uw app DirectorySearcher en meld u aan met een van de gebruikers.
2. Zoeken naar andere gebruikers op basis van de UPN.

ADAL kunt gemakkelijk veelvoorkomende identiteit functies opnemen in de app. Zorgt het alle dirty werk voor u, zoals Cachebeheer, OAuth-protocolondersteuning, dat de gebruiker een aanmelding-gebruikersinterface en vernieuwen van tokens verlopen. U moet weten slechts één API-aanroep, `authContext.AcquireToken*(…)`.

Ter referentie: download de [voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip) (zonder uw configuratiewaarden).

U kunt nu verder met de identiteit van de aanvullende scenario's. Probeer bijvoorbeeld [beveiligen van een .NET-Web-API met Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
