---
title: Azure Active Directory v2.0 .NET systeemeigen App | Microsoft Docs
description: Het bouwen van een systeemeigen .NET-app die gebruikers met beide persoonlijke Microsoft-Account en werk-of schoolaccounts ondertekent.
services: active-directory
documentationcenter: 
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 46d81e09-bad0-44ce-9026-881805976e72
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/30/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 88679e7dd71011f767cbe4de295c284516375d20
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="add-sign-in-to-a-windows-desktop-app"></a>Aanmelden toevoegen aan een Windows-bureaublad-app
Met het v2.0-eindpunt kunt u snel verificatie op uw bureaublad apps met ondersteuning voor beide persoonlijke Microsoft-accounts en werk-of schoolaccount toevoegen.  Bovendien kunnen uw app zodat deze veilig te communiceren met een back-end web-api, evenals [Microsoft Graph](https://graph.microsoft.io) en enkele van de [Office 365 Unified API](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [!NOTE]
> Niet alle Azure Active Directory (AD)-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

Voor [systeemeigen .NET-toepassingen die worden uitgevoerd op een apparaat](active-directory-v2-flows.md#mobile-and-native-apps), Azure AD levert de Verificatiebibliotheek van Microsoft Identity of MSAL.  MSAL van enig doel in leven is gemakkelijker voor uw app om op te halen van tokens voor het aanroepen van webservices.  Om te demonstreren hoe eenvoudig het is, je hier we bouwt u een .NET WPF-takenlijst-app die:

* De gebruiker zich aanmeldt & opgehaald tokens met toegang tot de [OAuth 2.0-verificatieprotocol](active-directory-v2-protocols.md).
* Veilig een back-end takenlijst-webservice, die ook is beveiligd met OAuth 2.0-aanroepen.
* De gebruiker zich afmeldt.

## <a name="download-sample-code"></a>Voorbeeldcode downloaden
De code voor deze zelfstudie wordt onderhouden in [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  Als u wilt volgen, kunt u [basis van de app downloaden als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) of het geraamte:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

De voltooide app is verstrekt aan het einde van deze zelfstudie ook.

## <a name="register-an-app"></a>Een app registreren
Maakt een nieuwe app op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of als volgt [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

* Noteer de **toepassings-Id** toegewezen aan uw app, moet u deze snel.
* Voeg de **Mobile** platform voor uw app.

## <a name="install--configure-msal"></a>Installeren en configureren van MSAL
Nu dat u een app geregistreerd bij Microsoft hebt, kunt u MSAL installeert en uw identiteitsgerelateerde code schrijven.  In de volgorde voor MSAL naar het v2.0-eindpunt te communiceren, moet u voorzien enige informatie over de registratie van uw app.

* Beginnen met het MSAL toevoegen aan het TodoListClient-project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

* Open in het project TodoListClient `app.config`.  Vervang de waarden van de elementen in de `<appSettings>` sectie in overeenstemming met de waarden die u in de portal van de registratie van de app hebt ingevoerd.  Uw code zal naar deze waarden verwijzen wanneer MSAL wordt gebruikt.
  
  * De `ida:ClientId` is de **toepassings-Id** van uw app die u hebt gekopieerd uit de portal.
* Open in het project TodoList-Service `web.config` in de hoofdmap van het project.  
  
  * Vervang de `ida:Audience` waarde met dezelfde **toepassings-Id** vanuit de portal.

## <a name="use-msal-to-get-tokens"></a>Gebruik MSAL tokens ophalen
Het basisprincipe achter MSAL is dat wanneer uw app een toegangstoken moet, u gewoon belt `app.AcquireToken(...)`, en MSAL doet de rest.  

* In de `TodoListClient` project, open `MainWindow.xaml.cs` en zoek de `OnInitialized(...)` methode.  De eerste stap is het initialiseren van uw app `PublicClientApplication` -MSAL van primaire klasse die systeemeigen toepassingen vertegenwoordigt.  Dit is waar u MSAL de coördinaten die voor de communicatie met Azure AD en hoe deze tokens in de cache moet doorgeven.

```csharp
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

* Wanneer de app wordt gestart, die we willen te zien als de gebruiker al in de app is ondertekend.  Echter niet willen we nu nog een gebruikersinterface aanmelden invoke - maken we de gebruiker klikt u op 'Aanmelden' om dit te doen.  Ook in de `OnInitialized(...)` methode:

```csharp
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.

    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

* Als de gebruiker niet is aangemeld en ze op de knop 'Aanmelden', die we willen aanroepen van een UI-aanmelding en hebben de gebruikers hun referenties invoeren.  De knop aanmelden handler implementeren:

```csharp
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

* Als de gebruiker heeft zich aanmeldt, MSAL ontvangt en een token voor u in de cache en kunt u doorgaan om aan te roepen de `GetTodoList()` methode met vertrouwen.  Alle die altijd ingeschakeld als u de taken van een gebruiker is het implementeren van de `GetTodoList()` methode.

```csharp
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```csharp
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Voer
Gefeliciteerd! U hebt nu een werkende .NET WPF-app met de mogelijkheid om te verifiëren van gebruikers & veilig aanroepen van Web-API's met behulp van OAuth 2.0.  Voer beide projecten uit en meld u aan met een persoonlijk Microsoft-account of een account voor werk of school.  Taken toevoegen aan de takenlijst van de gebruiker.  Meld u af en meld u opnieuw aan als een andere gebruiker om hun takenlijst weer te geven.  Sluit de app en voer deze opnieuw uit.  U ziet hoe de gebruikerssessie blijft intact - gebeurt dit omdat de app in de cache opgeslagen tokens in een lokaal bestand.

MSAL gemakkelijk veelvoorkomende identiteit functies opnemen in uw app met behulp van zowel privé- als -accounts.  Dit zorgt voor al het werk dirty voor u - Cachebeheer, OAuth-protocolondersteuning, dat de gebruiker een aanmelding-gebruikersinterface vernieuwen van tokens verlopen en meer.  Alles wat u moet weten één API-aanroep is `app.AcquireTokenAsync(...)`.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) [is opgegeven als een ZIP hier](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), of u kunt dit ook klonen vanuit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Volgende stappen
U kunt nu verplaatsen naar geavanceerdere onderwerpen.  U wilt proberen:

* [Beveiliging van de Web-API TodoListService met het v2.0-eindpunt](active-directory-v2-devquickstarts-dotnet-api.md)

Voor aanvullende bronnen voor uitchecken:  

* [De ontwikkelaarshandleiding v2.0 >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow 'msal' tag >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten downloaden
We raden u aan in te stellen dat u meldingen ontvangt wanneer er beveiligingsincidenten optreden. Ga hiervoor naar [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneer u op Security Advisory Alerts.

