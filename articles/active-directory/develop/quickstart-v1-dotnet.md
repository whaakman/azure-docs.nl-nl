---
title: Meld u aan gebruikers en de Microsoft Graph-API aanroepen vanuit een .NET-Desktop (WPF)-app | Microsoft Docs
description: Informatie over het bouwen van een .NET Windows-Desktop-toepassing die kan worden geïntegreerd met Azure AD voor aanmelden en aanroepen van Azure AD met behulp van OAuth 2.0 API's beveiligd.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 968afcba8b0a6ab9d46c5582eecbb4901975257c
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55101132"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-a-net-desktop-wpf-app"></a>Quickstart: Meld u aan gebruikers en de Microsoft Graph-API aanroepen vanuit een .NET-Desktop (WPF)-app

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Voor .NET native clients die toegang moeten krijgen tot beveiligde bronnen, biedt Azure Active Directory (Azure AD) de Active Directory Authentication Library (ADAL). ADAL kunt eenvoudig uw app toegangstokens ophalen. 

In deze snelstartgids leert u hoe u om een takenlijst voor .NET-WPF-toepassing te bouwen die:

* Hiermee toegang tot tokens voor het aanroepen van de Azure AD Graph-API met behulp van de OAuth 2.0-protocol voor verificatie.
* Zoekt naar een map voor gebruikers met een opgegeven alias.
* Gebruikers van de tekens uit.

Om de volledige, werkende toepassing te compileren, moet u het volgende doen:

1. Registreer de toepassing bij Azure AD.
2. Installeer en configureer ADAL.
3. Gebruik ADAL om tokens op te halen uit Azure AD.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorgt u dat u aan deze vereisten voldoet:

* [Het app-basisproject downloaden](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) of [het voltooide voorbeeld downloaden](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)
* Een Azure AD-tenant waar u kunt gebruikers maken en registreren van een toepassing hebben. [Lees hier hoe u een tenant kunt verkrijgen](quickstart-create-new-tenant.md) als u er nog geen hebt.

## <a name="step-1-register-the-directorysearcher-application"></a>Stap 1: De toepassing DirectorySearcher registreren

Als u wilt inschakelen voor de app om op te halen van tokens, uw app registreren in uw Azure AD-tenant en verleent deze machtiging voor toegang tot de Azure AD Graph-API:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Op de bovenste balk, selecteert u uw account en klikt u onder de **Directory** kiest u de Active Directory-tenant waar u wilt registreren van uw toepassing.
3. Selecteer op **alle services** in het navigatievenster aan de linkerkant, en kies **Azure Active Directory**.
4. Op **App-registraties**, kiest u **toevoegen**.
5. Volg de aanwijzingen en maak een nieuwe **systeemeigen** clienttoepassing.
    * De **naam** van de toepassing wordt beschreven voor uw eindgebruikers
    * De **omleidings-Uri** is een combinatie van schema en de tekenreeks die Azure AD wordt gebruikt om tokenantwoorden te retourneren. Voer een specifieke waarde aan uw toepassing, bijvoorbeeld `http://DirectorySearcher`.

6. Nadat u de registratie hebt voltooid, AAD wordt uw app toewijzen een unieke toepassings-ID. U moet deze waarde in de volgende secties, dus te kopiëren uit de toepassingspagina.
7. Uit de **instellingen** pagina, kies **vereiste machtigingen** en kies **toevoegen**. Selecteer **Microsoft Graph** als de API, en klikt u onder **overgedragen machtigingen** toevoegen de **mapgegevens lezen** machtiging. Instellen van deze machtiging kan uw toepassing om op te vragen van de Graph-API voor gebruikers.

## <a name="step-2-install-and-configure-adal"></a>Stap 2: Installeren en configureren van ADAL

Nu u een toepassing hebt in Azure AD, kunt u ADAL installeren en uw aan identiteit gerelateerde code schrijven. Als u wilt dat ADAL kan communiceren met Azure AD, moet u ADAL voorzien van bepaalde informatie gegevens over uw app-registratie.

1. Begin door toe te voegen ADAL de `DirectorySearcher` project met behulp van de Package Manager-Console.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

1. In de `DirectorySearcher` project, open `app.config`.
1. Vervang de waarden van de elementen in de `<appSettings>` gedeelte in overeenstemming met de waarden die u in de Azure-portal hebt ingevoerd. Uw code verwijst naar deze waarden wanneer deze gebruikmaakt van ADAL.
  * De `ida:Tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com
  * De `ida:ClientId` is de client-ID van uw toepassing die u hebt gekopieerd uit de portal.
  * De `ida:RedirectUri` is de omleidings-URL die u in de portal hebt geregistreerd.

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>Stap 3: Gebruikmaken van ADAL om op te halen van tokens van Azure AD

Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken moet, uw app roept `authContext.AcquireTokenAsync(...)`, en doet de rest van ADAL.

1. In de `DirectorySearcher` project, open `MainWindow.xaml.cs`.
1. Zoek de `MainWindow()` methode. 
1. Initialiseren van uw app `AuthenticationContext` -ADAL de primaire klasse. `AuthenticationContext` is waar het doorgeven van ADAL de coördinaten die nodig is om te communiceren met Azure AD en hoe deze tokens in de cache.

    ```csharp
    public MainWindow()
    {
        InitializeComponent();

        authContext = new AuthenticationContext(authority, new FileCache());

        CheckForCachedToken();
    }
    ```

1. Zoek de `Search(...)` methode die wordt aangeroepen wanneer de gebruiker selecteert de **zoeken** knop in de gebruikersinterface van de app. Met deze methode maakt u een GET-aanvraag voor de Azure AD Graph API om gebruikers op te vragen van wie de UPN begint met de opgegeven zoekterm.
1. Om te vragen de Graph API, omvatten een access_token in de `Authorization` -header van de aanvraag, dit is waar ADAL is beschikbaar in.

    ```csharp
    private async void Search(object sender, RoutedEventArgs e)
    {
        // Validate the Input String
        if (string.IsNullOrEmpty(SearchText.Text))
        {
            MessageBox.Show("Please enter a value for the To Do item name");
            return;
        }

        // Get an Access Token for the Graph API
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
            UserNameLabel.Content = result.UserInfo.DisplayableId;
            SignOutButton.Visibility = Visibility.Visible;
        }
        catch (AdalException ex)
        {
            // An unexpected error occurred, or user canceled the sign in.
            if (ex.ErrorCode != "access_denied")
                MessageBox.Show(ex.Message);

            return;
        }

        ...
    }
    ```

    Wanneer uw app een token aanvraagt door het aanroepen van `AcquireTokenAsync(...)`, ADAL zal proberen om te herstellen van een token zonder dat de gebruiker om referenties wordt gevraagd.
    * Als de ADAL wordt vastgesteld dat de gebruiker moet zich aanmelden bij een token verkrijgen, wordt een dialoogvenster voor aanmelding weergeven, verzamelen van de referenties van de gebruiker en retourneert een token bij een geslaagde verificatie. 
    * Als ADAL niet kan een token terug voor een bepaalde reden, genereert deze een `AdalException`.

1. U ziet dat de `AuthenticationResult` -object bevat een `UserInfo` -object dat kan worden gebruikt voor het verzamelen van informatie die uw app mogelijk nodig hebt. In de DirectorySearcher `UserInfo` wordt gebruikt voor het aanpassen van de gebruikersinterface van de app met de id van de gebruiker.
1. Wanneer de gebruiker selecteert de **Afmelden** knop, zorg ervoor dat de volgende aanroep aan `AcquireTokenAsync(...)` vraagt de gebruiker zich aanmeldt. U kunt dit eenvoudig doen met ADAL door de tokencache uit te schakelen:

    ```csharp
    private void SignOut(object sender = null, RoutedEventArgs args = null)
    {
        // Clear the token cache
        authContext.TokenCache.Clear();

        ...
    }
    ```

    Als de gebruiker is niet op de **Afmelden** knop, die u wilt behouden van de sessie van de gebruiker voor de volgende keer dat ze de DirectorySearcher worden uitgevoerd. Wanneer de app wordt gestart, kunt u controleren van de ADAL-tokencache voor een bestaande token en dienovereenkomstig bijwerken van de gebruikersinterface.

1. In de `CheckForCachedToken()` methode, voer een andere aanroep naar `AcquireTokenAsync(...)`, ditmaal doorgeven in de `PromptBehavior.Never` parameter. `PromptBehavior.Never` vertelt ADAL dat de gebruiker niet moet worden gevraagd voor aanmelden en ADAL in plaats daarvan een uitzondering genereert moet wanneer het niet lukt om terug te keren een token.

    ```csharp
    public async void CheckForCachedToken() 
    {
        // As the application starts, try to get an access token without prompting the user. If one exists, show the user as signed in.
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "user_interaction_required")
            {
                // An unexpected error occurred.
                MessageBox.Show(ex.Message);
            }

            // If user interaction is required, proceed to main page without singing the user in.
            return;
        }

        // A valid token is in the cache
        SignOutButton.Visibility = Visibility.Visible;
        UserNameLabel.Content = result.UserInfo.DisplayableId;
    }
    ```

Gefeliciteerd! U hebt nu een werkende .NET WPF-toepassing die u kunt gebruikers verifiëren, veilig aanroepen van Web-API's met behulp van OAuth 2.0 en elementaire informatie over de gebruiker. Nu kunt u de tenant met gebruikers gaan vullen als u dat nog niet hebt gedaan. Voer uw app DirectorySearcher uit en meld u aan met een van deze gebruikers. Zoek andere gebruikers op basis van hun UPN. De app sluit en opnieuw te starten. U ziet hoe de sessie van de gebruiker intact blijft. Meld u af en meld u opnieuw aan als een andere gebruiker.

ADAL kunt eenvoudig deze algemene identiteitsfuncties opnemen in uw toepassing. Dit zorgt dat het werk dirty bij u past, met inbegrip van Cachebeheer, ondersteuning voor OAuth protocol, dat de gebruiker met een gebruikersinterface, vervallen tokens en meer te vernieuwen. Het enige dat u hoeft te weten, is één API-aanroep, `authContext.AcquireTokenAsync(...)`.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) [op GitHub](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Volgende stappen

Leer hoe u een web-API beveiligen met behulp van OAuth 2.0-bearer-toegangstokens.
> [!div class="nextstepaction"]
> [Een .NET-Web-API met Azure AD beveiligen >>](quickstart-v1-dotnet-webapi.md)
