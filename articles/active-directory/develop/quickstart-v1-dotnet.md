---
title: Gebruikers aanmelden en de Microsoft Graph API aanroepen vanuit een .NET Desktop-app (WPF) | Microsoft Docs
description: Meer informatie over het bouwen van een NET Windows Desktop-app die kan worden geïntegreerd met Azure Active Directory voor aanmelden en Azure Active Directory-beveiligde API-aanroepen met behulp van OAuth 2.0.
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
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 57c6b477057fb4100cff5726a4d13c6d24d80906
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55695267"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-a-net-desktop-wpf-app"></a>Snelstart: Gebruikers aanmelden en de Microsoft Graph API aanroepen vanuit een .NET Desktop-app (WPF)

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Azure Active Directory (Azure AD) biedt de ADAL (Active Directory Authentication Library) voor systeemeigen .NET-clients die toegang nodig hebben tot beveiligde bronnen. Met behulp van ADAL kan uw app eenvoudig toegangstokens ophalen. 

In deze quickstart leert u hoe een .NET WPF- takenlijsttoepassing bouwt die:

* Toegangstokens verkrijgt om de Azure Active Directory Graph API aan te roepen met behulp van het OAuth 2.0-verificatieprotocol.
* In een map zoekt naar gebruikers met een gegeven alias.
* Gebruikers afmeldt.

Om de volledige, werkende toepassing te compileren, moet u het volgende doen:

1. Registreer de toepassing bij Azure AD.
2. Installeer en configureer ADAL.
3. Gebruik ADAL om tokens op te halen uit Azure AD.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorgt u dat u aan deze vereisten voldoet:

* [Download het raamwerk van de app](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) of [download het voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)
* Zorg voor een Microsoft Azure Active Directory-tenant waarin u gebruikers kunt maken en een toepassing kunt registreren. [Lees hier hoe u een tenant kunt verkrijgen](quickstart-create-new-tenant.md) als u er nog geen hebt.

## <a name="step-1-register-the-directorysearcher-application"></a>Stap 1: De toepassing DirectorySearcher registreren

Als u de app wilt inschakelen voor het ophalen van tokens, moet u de app registreren in uw Azure Active Directory-tenant en de app toegang geven tot de Azure AD Graph API:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer uw account in de bovenste balk en kies onder **Directory** de Microsoft Active Directory-tenant waarin u uw toepassing wilt registreren.
3. Selecteer **Alle services** in het navigatievenster aan de linkerkant en kies **Azure Active Directory**.
4. Kies bij **App-registraties** de optie **Toevoegen**.
5. Volg de aanwijzingen en maak een nieuwe **systeemeigen** clienttoepassing.
    * De **Naam** van de toepassing beschrijft de toepassing voor gebruikers
    * De **Omleidings-URI** is een combinatie van een schema en een tekenreeks die door Azure Active Directory wordt gebruikt om tokenantwoorden te retourneren. Voer een waarde in die specifiek is voor uw toepassing, bijvoorbeeld `http://DirectorySearcher`.

6. Wanneer de registratie is voltooid, wijst Azure Active Directory een unieke toepassings-id toe aan uw app. U hebt deze waarde nodig in de volgende secties. Kopieer deze daarom vanaf de toepassingspagina.
7. Kies op de pagina **Instellingen** de optie **Vereiste machtigingen** en kies vervolgens **Toevoegen**. Selecteer **Microsoft Graph** als de API en voeg onder **Gedelegeerde machtigingen** de machtiging **Mapgegevens lezen** toe. Hiermee machtigt u de toepassing om gegevens uit de Graph API op te vragen voor gebruikers.

## <a name="step-2-install-and-configure-adal"></a>Stap 2: ADAL installeren en configureren

Nu u een toepassing hebt in Azure AD, kunt u ADAL installeren en uw aan identiteit gerelateerde code schrijven. Als u wilt dat ADAL kan communiceren met Azure AD, moet u ADAL voorzien van bepaalde informatie gegevens over uw app-registratie.

1. Begin met het toevoegen van ADAL aan het project `DirectorySearcher` met behulp van de Package Manager-Console.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

1. Open in het project `DirectorySearcher` `app.config`.
1. Vervang de waarden van de elementen in de sectie `<appSettings>` om de waarden die u hebt ingevoerd in Azure Portal weer te geven. Uw code verwijst naar deze waarden wanneer deze gebruikmaakt van ADAL.
  * De `ida:Tenant` is het domein van uw Microsoft Azure Active Directory-tenant, bijvoorbeeld contoso.onmicrosoft.com
  * De `ida:ClientId` is de client-id van uw toepassing die u hebt gekopieerd uit de portal.
  * De `ida:RedirectUri` is de omleidings-URL die u in de portal hebt geregistreerd.

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>Stap 3: ADAL gebruiken om tokens op te halen uit Azure AD

Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken nodig heeft, uw app eenvoudigweg `authContext.AcquireTokenAsync(...)`aanroept, waarna ADAL de rest doet.

1. Open in het project `DirectorySearcher` `MainWindow.xaml.cs`.
1. Zoek de methode `MainWindow()` op. 
1. Initialiseer de `AuthenticationContext` van uw app - de primaire klasse van ADAL. `AuthenticationContext` is waar u ADAL de coördinaten doorgeeft om te communiceren met Azure Active Directory en waar u opgeeft hoe deze tokens in de cache moeten worden opgeslagen.

    ```csharp
    public MainWindow()
    {
        InitializeComponent();

        authContext = new AuthenticationContext(authority, new FileCache());

        CheckForCachedToken();
    }
    ```

1. Zoek de methode `Search(...)` op, die wordt aangeroepen wanneer de gebruiker de knop **Zoeken** in de gebruikersinterface van de app selecteert. Met deze methode maakt u een GET-aanvraag voor de Azure AD Graph API om gebruikers op te vragen van wie de UPN begint met de opgegeven zoekterm.
1. Als u gegevens wilt opvragen uit de Graph API, moet u een access_token opnemen in de `Authorization`-header van de aanvraag. En hier komt ADAL in actie.

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

    Wanneer uw app een token aanvraagt door `AcquireTokenAsync(...)` aan te roepen, probeert ADAL een token te retourneren zonder de gebruiker om referenties te vragen.
    * Als ADAL bepaalt dat de gebruiker zich moet aanmelden om een token te verkrijgen, wordt er een aanmeldingsvenster weergegeven, worden de referenties van de gebruiker verzameld en wordt er na een geslaagde verificatie een token geretourneerd. 
    * Als ADAL om welke reden dan ook geen token kan retourneren, wordt er een `AdalException` gegenereerd.

1. Het object `AuthenticationResult` bevat een object `UserInfo` dat u kunt gebruiken om de informatie te verzamelen die uw app mogelijk nodig heeft. In de DirectorySearcher wordt `UserInfo` gebruikt om de gebruikersinterface van de app aan te passen met de id van de gebruiker.
1. Wanneer de gebruiker de knop **Afmelden** selecteert, controleert u of bij de volgende aanroep naar `AcquireTokenAsync(...)` de gebruiker wordt gevraagd zich aan te melden. U kunt dit eenvoudig doen met ADAL door de tokencache te wissen:

    ```csharp
    private void SignOut(object sender = null, RoutedEventArgs args = null)
    {
        // Clear the token cache
        authContext.TokenCache.Clear();

        ...
    }
    ```

    Als de gebruiker niet op de knop **Afmelden** klikt, moet u de sessie van de gebruiker behouden voor de volgende keer dat deze de DirectorySearcher uitvoert. Wanneer de app wordt gestart, kunt u de ADAL-tokencache controleren op een bestaand token en de gebruikersinterface dienovereenkomstig bijwerken.

1. Voer in de methode `CheckForCachedToken()` nog een aanroep naar `AcquireTokenAsync(...)` uit, waarbij u deze keer de parameter `PromptBehavior.Never` doorgeeft. `PromptBehavior.Never` vertelt ADAL dat de gebruiker niet moet worden gevraagd om zich aan te melden en ADAL moet in plaats daarvan een uitzondering genereren wanneer het niet lukt om een token te retourneren.

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

Gefeliciteerd! U hebt nu een werkende .NET WPF-toepassing die gebruikers kan verifiëren, Web-API's veilig kan aanroepen met behulp van OAuth 2.0, en basisinformatie over de gebruiker kan verkrijgen. Nu kunt u de tenant met gebruikers gaan vullen als u dat nog niet hebt gedaan. Voer uw DirectorySearcher-app met één pagina uit en meld u aan met een van deze gebruikers. Zoek andere gebruikers op basis van hun UPN. Sluit de app en voer deze opnieuw uit. U ziet hoe de sessie van de gebruiker intact blijft. Meld u af en meld u opnieuw aan als een andere gebruiker.

Met ADAL kunt u deze algemene identiteitsfuncties eenvoudig opnemen in uw toepassing. Het neemt de vervelende klusjes voor zijn rekening, waaronder cachebeheer, OAuth-protocolondersteuning, het aanbieden van een gebruikersinterface waarmee de gebruiker zich kan aanmelden, het vernieuwen van verlopen tokens en nog veel meer. Het enige dat u hoeft te weten, is één API-aanroep, `authContext.AcquireTokenAsync(...)`.

Zie het volledige voorbeeld (zonder uw configuratiewaarden) op [GitHub](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip) ter referentie.

## <a name="next-steps"></a>Volgende stappen

Leer hoe u een web-API beveiligt met behulp van OAuth 2.0-bearer-toegangstokens.
> [!div class="nextstepaction"]
> [Een .NET Web API beveiligen met Microsoft Azure AD >>](quickstart-v1-dotnet-webapi.md)
