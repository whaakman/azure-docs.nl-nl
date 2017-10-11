---
title: Azure AD .NET aan de slag | Microsoft Docs
description: "Het bouwen van een toepassing .NET Windows-bureaublad, die kan worden geïntegreerd met Azure AD voor aanmelden en Azure AD-aanroepen beveiligd met OAuth API's."
services: active-directory
documentationcenter: .net
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: ed33574f-6fa3-402c-b030-fae76fba84e1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 7a252e0e5243c7b7489373845531cb913ca1f6aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Azure AD integreren met een Windows-bureaublad WPF-App
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Als u een bureaubladtoepassing ontwikkelt, kunt Azure AD u eenvoudig en snel voor u om uw gebruikers met hun Active Directory-account te verifiëren.  Ook kunt uw toepassing veilig gebruiken voor een web-API die zijn beveiligd door Azure AD, zoals de Office 365-API of de Azure-API.

Voor .NET systeemeigen clients die toegang moeten krijgen tot beveiligde bronnen, levert Azure AD de Active Directory Authentication Library of ADAL.  Enig doel in het leven van ADAL is gemakkelijker voor uw app toegangstokens ophalen.  Als u wilt laten zien hoe eenvoudig het is, hier we je een .NET WPF-takenlijst-toepassing bouwt die:

* Krijgt toegang tot tokens voor het aanroepen van de Azure AD Graph API met behulp van de [OAuth 2.0-verificatieprotocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Zoekt een directory voor gebruikers met een alias voor een gegeven.
* Tekenen gebruikers uit.

De volledige werkende toepassing bouwen, moet u het:

1. Uw toepassing registreren met Azure AD.
2. Installeren en configureren van ADAL.
3. ADAL gebruikt om tokens van Azure AD.

Aan de slag [de basis van de app downloaden](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) of [het voltooide voorbeeld downloaden](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  U moet ook een Azure AD-tenant kunt u gebruikers maken en een toepassing registreren.  Als u niet al een tenant [Lees hoe u een](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>1. De toepassing DirectorySearcher registreren
Als u wilt inschakelen voor uw app tokens krijgen, moet u eerst registreren in uw Azure AD-tenant en verleent deze machtiging voor toegang tot de Azure AD Graph API:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op de bovenste balk op je account en klikt u onder de **Directory** kiest u de Active Directory-tenant waar u wilt uw toepassing registreren.
3. Klik op **meer Services** in de nav linkerkant en kies **Azure Active Directory**.
4. Klik op **App registraties** en kies **toevoegen**.
5. Volg de aanwijzingen en maak een nieuwe **systeemeigen clienttoepassing**.
  * De **naam** van de toepassing bevat een beschrijving van uw toepassing aan eindgebruikers
  * De **omleidings-Uri** is een combinatie schema en de tekenreeks die door Azure AD wordt gebruikt om te retourneren van reacties token.  Voer van een specifieke waarde in voor uw toepassing bijvoorbeeld `http://DirectorySearcher`.
6. Zodra u inschrijving hebt voltooid, toewijst AAD uw app een unieke id  U moet deze waarde in de volgende secties, dus kopiëren van de toepassingspagina.
7. Van de **instellingen** pagina **Required Permissions** en kies **toevoegen**. Selecteer de **Microsoft Graph** als de API en voeg de **Directory-gegevens lezen** machtiging onder **gedelegeerde machtigingen**.  Hiermee schakelt u uw toepassing query uitvoeren op de Graph-API voor gebruikers.

## <a name="2-install--configure-adal"></a>2. Installeren en configureren van ADAL
Nu dat u een toepassing in Azure AD hebt, kunt u ADAL installeert en uw identiteitsgerelateerde code schrijven.  In de volgorde voor ADAL om te communiceren met Azure AD, moet u voorzien enige informatie over de registratie van uw app.

* Beginnen met het ADAL toevoegen aan het DirectorySearcher-project met de Package Manager-Console.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* Open in het project DirectorySearcher `app.config`.  Vervang de waarden van de elementen in de `<appSettings>` sectie in overeenstemming met de waarden die u in de Azure-Portal hebt ingevoerd.  Uw code zal naar deze waarden verwijzen wanneer deze gebruikmaakt van ADAL.
  * De `ida:Tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com
  * De `ida:ClientId` is de clientId van uw toepassing die u hebt gekopieerd uit de portal.
  * De `ida:RedirectUri` is de omleiding url die u hebt geregistreerd in de portal.

## <a name="3----use-adal-to-get-tokens-from-aad"></a>3.    Gebruikmaken van ADAL Tokens van AAD ophalen
Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken moet, aanroept `authContext.AcquireTokenAsync(...)`, en doet de rest van ADAL.  

* In de `DirectorySearcher` project, open `MainWindow.xaml.cs` en zoek de `MainWindow()` methode.  De eerste stap is het initialiseren van uw app `AuthenticationContext` -ADAL de primaire klasse.  Dit is waar het doorgeven van ADAL de coördinaten nodig om te communiceren met Azure AD en hoe deze tokens in de cache.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

* Nu zoeken de `Search(...)` methode, die wordt aangeroepen wanneer de gebruiker cliks de "zoeken" in de gebruikersinterface van de app.  Deze methode maakt een GET-aanvraag voor Azure AD Graph API aan query voor gebruikers wiens UPN met de opgegeven zoekterm begint.  Maar om de Graph API een query uitvoert, moet u een access_token in de `Authorization` header van de aanvraag - dit is waar de ADAL wordt geleverd.

```C#
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
* Wanneer uw app een token aanvragen door het aanroepen van `AcquireTokenAsync(...)`, ADAL probeert te retourneren van een token zonder dat de gebruiker om referenties gevraagd.  Als ADAL wordt vastgesteld dat de gebruiker zich aanmelden moet bij een token verkrijgen, wordt het een aanmelding weergegeven, verzamelen van referenties van de gebruiker en retourneren een token na verificatie is geslaagd.  Als ADAL kunnen niet worden geretourneerd van een token voor een of andere reden is, genereert het een `AdalException`.
* U ziet dat de `AuthenticationResult` object bevat een `UserInfo` -object dat kan worden gebruikt voor het verzamelen van informatie die uw app mogelijk nodig hebt.  In de DirectorySearcher `UserInfo` wordt gebruikt voor het aanpassen van de gebruikersinterface van de app met de id van de gebruiker.
* Wanneer de gebruiker op de knop 'Afmelden', willen we ervoor te zorgen dat de volgende aanroep `AcquireTokenAsync(...)` vraagt de gebruiker aan te melden.  Met ADAL, is dit is net zo eenvoudig als het token cache wissen:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

* Echter, als de gebruiker heeft niet op de knop 'Afmelden', wilt u de sessie van de gebruiker voor de volgende keer dat ze de DirectorySearcher worden uitgevoerd.  Wanneer de app opent, kunt u controleren van de ADAL-tokencache voor een bestaande token en de gebruikersinterface worden dienovereenkomstig bijgewerkt.  In de `CheckForCachedToken()` methode, moet u een andere aanroep van `AcquireTokenAsync(...)`, ditmaal doorgeven in de `PromptBehavior.Never` parameter.  `PromptBehavior.Never`vertelt ADAL dat de gebruiker niet moet worden gevraagd voor aanmelding en ADAL moet in plaats daarvan Veroorzaak een exception wanneer het niet lukt om te retourneren van een token.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
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

Gefeliciteerd. U nu beschikken over een werkende .NET WPF-toepassing met de mogelijkheid om te verifiëren van gebruikers, veilig aanroepen van Web-API's met behulp van OAuth 2.0 en algemene informatie over de gebruiker ophalen.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van uw tenant waarbij sommige gebruikers.  Uitvoeren van uw app DirectorySearcher en meld u aan met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van de UPN.  Sluit de app en voer deze opnieuw uit.  U ziet hoe de gebruikerssessie blijft intact.  Meld u af en meld u opnieuw aan als een andere gebruiker.

ADAL kunt eenvoudig gebruikmaken van al deze algemene identiteit functies in uw toepassing.  Dit zorgt voor al het werk dirty voor u - Cachebeheer, OAuth-protocolondersteuning, dat de gebruiker een aanmelding-gebruikersinterface vernieuwen van tokens verlopen en meer.  Alles wat u moet weten één API-aanroep is `authContext.AcquireTokenAsync(...)`.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) is opgegeven [hier](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  U kunt nu verder met aanvullende scenario's.  U wilt proberen:

[Een .NET-Web-API met Azure AD beveiligen >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

