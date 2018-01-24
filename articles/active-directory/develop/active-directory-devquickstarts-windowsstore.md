---
title: Azure AD Windows universele-Platform (UWP/XAML) aan de slag | Microsoft Docs
description: Build Windows Store-apps die integreren met Azure AD voor aanmelden en roept u Azure AD beveiligd met OAuth API's.
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8bc8c3a897363da2a8ebe7ac6bd8798c8e22ba04
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-ad-windows-universal-platform-uwpxaml-getting-started"></a>Azure AD Windows universele-Platform (UWP/XAML) aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Windows Store 8.1 en projecten met een eerdere versie worden niet ondersteund in Visual Studio 2017.  Zie [Geschikte platforms voor Visual Studio 2017 en compatibiliteit](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) voor meer informatie.

Als u apps voor de Windows Store ontwikkelt, kunt Azure Active Directory (Azure AD) u eenvoudig en snel om uw gebruikers met hun Active Directory-account te verifiëren. Een app door te integreren met Azure AD, kan veilig een web-API die wordt beveiligd door Azure AD, zoals de Office 365-API of de Azure-API gebruiken.

Voor Windows Store-desktoptoepassingen die toegang moeten krijgen tot beveiligde bronnen, levert Azure AD de Active Directory Authentication Library (ADAL). Het enige doel van ADAL is gemakkelijker voor de app toegangstokens ophalen. Als u wilt laten zien hoe eenvoudig het is, in dit artikel laat zien hoe DirectorySearcher Windows Store-Apps bouwen die:

* Krijgt toegang tot tokens voor de Azure AD Graph-API aanroept met behulp van de [OAuth 2.0-verificatieprotocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Zoekt een directory voor gebruikers met een opgegeven user principal name (UPN).
* Tekenen gebruikers uit.

## <a name="before-you-get-started"></a>Voordat u aan de slag gaat
* Download de [basisproject](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip), of download de [voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip). Elke download is een Visual Studio 2015-oplossing.
* U moet ook een Azure AD-tenant waarin gebruikers maken en de app te registreren. Als u niet al een tenant [Lees hoe u een](active-directory-howto-tenant.md).

Wanneer u klaar bent, voert u de procedures in de volgende drie secties.

## <a name="step-1-register-the-directorysearcher-app"></a>Stap 1: De app DirectorySearcher te registreren
Als u wilt inschakelen voor de app tokens krijgen, moet u eerst registreren in uw Azure AD-tenant en verleent deze machtiging voor toegang tot de Azure AD Graph API. Dit doet u al volgt:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op uw account op de bovenste balk. Klik vervolgens onder de **Directory** , selecteert u de Active Directory-tenant waar u de app te registreren.
3. Klik op **meer Services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App registraties**, en selecteer vervolgens **toevoegen**.
5. Volg de aanwijzingen voor het maken van een **systeemeigen clienttoepassing**.
  * **Naam** beschrijving van de app voor gebruikers.
  * **Omleidings-URI** is een combinatie schema en de tekenreeks die gebruikmaakt van Azure AD token antwoorden retourneren. Voer een aanduidingswaarde van de tijdelijke voor nu (bijvoorbeeld **http://DirectorySearcher**). U moet de waarde later vervangen.
6. Nadat u de registratie hebt voltooid, wijst Azure AD de app een unieke toepassings-ID. Kopieer de waarde op de **toepassing** tabblad omdat u hebt deze later nodig.
7. Op de **instellingen** pagina **Required Permissions**, en selecteer vervolgens **toevoegen**.
8. Voor de **Azure Active Directory** app, selecteer **Microsoft Graph** als de API.
9. Onder **gedelegeerde machtigingen**, voeg de **toegang tot de map als de gebruiker is aangemeld** machtiging. In dat geval kan de app om op te vragen de Graph-API voor gebruikers.

## <a name="step-2-install-and-configure-adal"></a>Stap 2: Installeren en configureren van ADAL
Nu dat u een app in Azure AD hebt, kunt u ADAL installeert en uw identiteitsgerelateerde code schrijven. Om in te schakelen ADAL om te communiceren met Azure AD, geef deze de enige informatie over de registratie van de app.

1. ADAL toevoegen aan het project DirectorySearcher met behulp van de Package Manager-Console.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. Open in het project DirectorySearcher MainPage.xaml.cs.
3. Vervang de waarden in de **configuratiewaarden** regio met de waarden die u hebt ingevoerd in de Azure-portal. Uw code verwijst naar deze waarden wanneer deze gebruikmaakt van ADAL.
  * De *tenant* is het domein van uw Azure AD-tenant (bijvoorbeeld: contoso.onmicrosoft.com).
  * De *clientId* is de client-ID van de app, die u hebt gekopieerd uit de portal.
4. U moet nu voor het detecteren van de callback-URI voor uw Windows Store-app. Stel een onderbrekingspunt in op deze regel in de `MainPage` methode:
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. Bouw de oplossing, om ervoor te zorgen dat alle pakket-verwijzingen zijn hersteld. Als er pakketten ontbreken, opent u NuGet Package Manager en deze terugzetten.
6. Voer de app en kopieer de waarde van `redirectUri` wanneer het onderbrekingspunt is bereikt. De waarde moet er ongeveer als volgt uitzien:

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. Terug op de **instellingen** tabblad toevoegen van de app in de Azure portal een **RedirectUri** met de vorige waarde.  

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>Stap 3: Gebruik ADAL tokens ophalen uit Azure AD
Het basisprincipe achter ADAL is dat wanneer de app een toegangstoken moet, aanroept `authContext.AcquireToken(…)`, en doet de rest van ADAL.  

1. Initialiseren van de app `AuthenticationContext`, dit is de primaire klasse van ADAL. Deze actie geeft ADAL de coördinaten nodig om te communiceren met Azure AD en hoe deze tokens in de cache.

    ```csharp
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. Zoek de `Search(...)` methode die wordt opgeroepen wanneer gebruikers op de **Search** knop op de gebruikersinterface van de app. Deze methode maakt een get-aanvraag voor Azure AD Graph API aan query voor gebruikers wiens UPN met de opgegeven zoekterm begint. Om te vragen de Graph API, een toegangstoken in de aanvraag opnemen **autorisatie** header. Dit is waar de ADAL wordt geleverd.

    ```csharp
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    Wanneer de app een token aanvragen door het aanroepen van `AcquireTokenAsync(...)`, ADAL probeert te retourneren van een token zonder dat de gebruiker om referenties gevraagd. Als ADAL wordt vastgesteld dat de gebruiker zich aanmelden moet bij een token verkrijgen, het geeft een dialoogvenster aanmelden, verzamelt de referenties van de gebruiker en geeft aan dat een token na een verificatie geslaagde. Als ADAL kunnen niet worden geretourneerd van een token voor een of andere reden is de *AuthenticationResult* status is een fout opgetreden.
3. Nu is het tijd om het gebruik van het toegangstoken dat u zojuist hebt opgehaald. Ook in de `Search(...)` methode, het token koppelen aan de Graph API get-aanvraag in de **autorisatie** header:

    ```csharp
    // Add the access token to the Authorization header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. U kunt de `AuthenticationResult` object om informatie over de gebruiker in de app, zoals ID van de gebruiker weer te geven:

    ```csharp
    // Update the page UI to represent the signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. U kunt ook ADAL voor het ondertekenen van gebruikers buiten de app. Wanneer de gebruiker klikt op de **Afmelden** knop, zorg ervoor dat de volgende aanroep `AcquireTokenAsync(...)` ziet u een weergave aanmelden. Deze actie is met ADAL, net zo eenvoudig als het token cache wissen:

    ```csharp
    private void SignOut()
    {
        // Clear session state from the token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a>Volgend onderwerp
U hebt nu een werkende Windows Store-app die u kunt verificatie van gebruikers, veilig aanroepen van web-API's met behulp van OAuth 2.0 en algemene informatie over de gebruiker ophalen.

Als u al uw tenant met gebruikers nog niet hebt ingevuld, is nu tijd om dit te doen.
1. Uitvoeren van uw app DirectorySearcher en meld u aan met een van de gebruikers.
2. Zoeken naar andere gebruikers op basis van de UPN.
3. De toepassing sluiten en opnieuw te starten. U ziet hoe de gebruikerssessie blijft intact.
4. Meld u af met de rechtermuisknop op de balk onderaan weergeven en vervolgens weer aanmelden als een andere gebruiker.

ADAL kunt eenvoudig deze algemene identiteit functies opnemen in de app. Zorgt het alle dirty werk voor u, zoals Cachebeheer, OAuth-protocolondersteuning, dat de gebruiker een aanmelding-gebruikersinterface en vernieuwen van tokens verlopen. U moet weten slechts één API-aanroep, `authContext.AcquireToken*(…)`.

Ter referentie: download de [voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (zonder uw configuratiewaarden).

U kunt nu verder met de identiteit van de aanvullende scenario's. Probeer bijvoorbeeld [beveiligen van een .NET-Web-API met Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
