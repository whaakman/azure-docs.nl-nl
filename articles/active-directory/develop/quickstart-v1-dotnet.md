---
title: Azure AD .NET Desktop (WPF) is aan de slag | Microsoft Docs
description: Over het bouwen van een .NET Windows-Desktop-toepassing die kan worden geïntegreerd met Azure AD voor aanmelding en roept Azure AD beveiligd met OAuth API's.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: e7a662afd78ff053c60aeb5de3af9f0070a7c458
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506526"
---
# <a name="azure-ad-net-desktop-wpf-getting-started"></a>Azure AD .NET Desktop (WPF) is aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Als u een bureaubladtoepassing ontwikkelt, heel Azure AD simpel kunt u uw gebruikers verifiëren met hun Active Directory-accounts. Ook kunt uw toepassing veilig gebruiken voor een web-API die wordt beveiligd door Azure AD, zoals de Office 365 API's of de API van Azure.

Azure AD biedt voor .NET native clients die toegang moeten krijgen tot beveiligde bronnen, de Active Directory Authentication Library of ADAL. Uitsluitend in het leven van ADAL is het gemakkelijk voor uw app om toegangstokens ophalen. Als u wilt laten zien hoe eenvoudig het is, bouwen hier we een To-Do-lijst van .NET-WPF-toepassing die:

* Hiermee toegang tot tokens voor het aanroepen van de Azure AD Graph API met behulp van de [OAuth 2.0-verificatieprotocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Zoekt naar een map voor gebruikers met een opgegeven alias.
* Gebruikers van de tekens uit.

Voor het bouwen van de volledige werkende toepassing, moet u het:

1. Uw toepassing registreren met Azure AD.
2. Installeren en configureren van ADAL.
3. Gebruikmaken van ADAL om op te halen van tokens van Azure AD.

Aan de slag [het app-basisproject downloaden](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) of [downloaden van het voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). U moet ook een Azure AD-tenant in die u kunt gebruikers maken en registreren van een toepassing. Als u nog een tenant hebt [informatie over het verkrijgen van een](quickstart-create-new-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>1. De toepassing DirectorySearcher registreren
Als u wilt inschakelen voor uw app tokens verkrijgen, moet u eerst deze te registreren in uw Azure AD-tenant en verleent deze machtiging voor toegang tot de Azure AD Graph-API:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op de bovenste balk van uw account en klikt u onder de **Directory** kiest u de Active Directory-tenant waar u wilt registreren van uw toepassing.
3. Klik op **alle services** in het navigatievenster aan de linkerkant, en kies **Azure Active Directory**.
4. Klik op **App-registraties** en kies **toevoegen**.
5. Volg de aanwijzingen en maak een nieuwe **systeemeigen clienttoepassing**.
  * De **naam** van de toepassing wordt beschreven voor uw eindgebruikers
  * De **omleidings-Uri** is een combinatie van schema en de tekenreeks die Azure AD wordt gebruikt om tokenantwoorden te retourneren. Voer een specifieke waarde aan uw toepassing, bijvoorbeeld `http://DirectorySearcher`.
6. Nadat u de registratie hebt voltooid, AAD wordt uw app toewijzen een unieke toepassings-ID. U moet deze waarde in de volgende secties, dus te kopiëren uit de toepassingspagina.
7. Uit de **instellingen** pagina, kies **vereiste machtigingen** en kies **toevoegen**. Selecteer de **Microsoft Graph** als de API en voeg de **mapgegevens lezen** machtiging onder **gedelegeerde machtigingen**. Hierdoor kan uw toepassing om op te vragen van de Graph-API voor gebruikers.

## <a name="2-install--configure-adal"></a>2. Installeren en configureren van ADAL
Nu dat u een toepassing in Azure AD hebt, kunt u ADAL installeert en uw identiteit gerelateerde code te schrijven. In de volgorde van ADAL om te communiceren met Azure AD, moet u deze voorzien van enkele gegevens over uw app-registratie.

* Beginnen met het toevoegen van ADAL voor het DirectorySearcher-project met behulp van de Package Manager-Console.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* Open in het project DirectorySearcher `app.config`. Vervang de waarden van de elementen in de `<appSettings>` gedeelte in overeenstemming met de waarden die u in de Azure-Portal hebt ingevoerd. Uw code zal naar deze waarden verwijzen wanneer deze gebruikmaakt van ADAL.
  * De `ida:Tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com
  * De `ida:ClientId` is de clientId van uw toepassing die u hebt gekopieerd uit de portal.
  * De `ida:RedirectUri` is de omleiding url die u in de portal hebt geregistreerd.

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. Gebruikmaken van ADAL Tokens ophalen uit AAD
Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken moet, deze roept `authContext.AcquireTokenAsync(...)`, en doet de rest van ADAL. 

* In de `DirectorySearcher` project, open `MainWindow.xaml.cs` en zoek de `MainWindow()` methode. De eerste stap is het initialiseren van uw app `AuthenticationContext` -ADAL de primaire klasse. Dit is waar het doorgeven van ADAL de coördinaten die nodig is om te communiceren met Azure AD en hoe deze tokens in de cache.

```csharp
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* Zoek nu de `Search(...)` methode, die wordt aangeroepen wanneer de gebruiker klikt op de knop "Zoeken" in de gebruikersinterface van de app. Deze methode maakt een GET-aanvraag voor de Azure AD Graph-API aan query voor gebruikers wiens UPN met de opgegeven zoekterm begint. Maar als u wilt de Graph API een query uitvoert, moet u een access_token in de `Authorization` koptekst van de aanvraag - dit is waar de ADAL wordt geleverd.

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
* Wanneer uw app een token aanvraagt door het aanroepen van `AcquireTokenAsync(...)`, ADAL zal proberen om te herstellen van een token zonder dat de gebruiker om referenties wordt gevraagd. Als de ADAL wordt vastgesteld dat de gebruiker moet zich aanmelden bij een token verkrijgen, wordt een dialoogvenster voor aanmelding weergeven, verzamelen van de referenties van de gebruiker en retourneert een token bij een geslaagde verificatie. Als ADAL niet kan een token terug voor een bepaalde reden, genereert deze een `AdalException`.
* U ziet dat de `AuthenticationResult` -object bevat een `UserInfo` -object dat kan worden gebruikt voor het verzamelen van informatie die uw app mogelijk nodig hebt. In de DirectorySearcher `UserInfo` wordt gebruikt voor het aanpassen van de gebruikersinterface van de app met de id van de gebruiker.
* Wanneer de gebruiker klikt op de knop 'Afmelden', willen we ervoor zorgen dat de volgende aanroep `AcquireTokenAsync(...)` vraagt de gebruiker zich aanmeldt. Met ADAL is dit net zo gemakkelijk als het token cache wissen:

```csharp
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* Echter, als de gebruiker niet de knop 'Afmelden' klikt heeft, wordt u onderhouden van de sessie van de gebruiker voor de volgende keer dat ze de DirectorySearcher worden uitgevoerd. Wanneer de app wordt gestart, kunt u controleren van de ADAL-tokencache voor een bestaande token en dienovereenkomstig bijwerken van de gebruikersinterface. In de `CheckForCachedToken()` methode, voer een andere aanroep naar `AcquireTokenAsync(...)`, ditmaal doorgeven in de `PromptBehavior.Never` parameter. `PromptBehavior.Never` vertelt ADAL dat de gebruiker niet moet worden gevraagd voor aanmelden en ADAL in plaats daarvan een uitzondering genereert moet wanneer het niet lukt om terug te keren een token.

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

Gefeliciteerd. U nu beschikken over een werkende .NET WPF-toepassing met de mogelijkheid om te verifiëren van gebruikers, veilig aanroepen van Web-API's met behulp van OAuth 2.0 en elementaire informatie over de gebruiker. Als u niet hebt gedaan, is nu de tijd voor het vullen van uw tenant waarbij sommige gebruikers. Voer uw app DirectorySearcher uit en meld u aan met een van deze gebruikers. Zoeken naar andere gebruikers op basis van hun UPN. Sluit de app en voer deze opnieuw uit. U ziet hoe de sessie van de gebruiker intact blijft. Meld u af en meld u opnieuw aan als een andere gebruiker.

ADAL kunt eenvoudig opnemen van al deze algemene identiteitsfuncties in uw toepassing. Dit zorgt dat al het werk dirty voor u - Cachebeheer, ondersteuning voor OAuth protocol, dat de gebruiker met een gebruikersinterface, vervallen tokens en meer te vernieuwen. Alles wat u moet weten is dat één API-aanroep, `authContext.AcquireTokenAsync(...)`.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) is opgegeven [hier](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip). U kunt nu verder met aanvullende scenario's. U wilt proberen:

[Een .NET-Web-API met Azure AD beveiligen >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

