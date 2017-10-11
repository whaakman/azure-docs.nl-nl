---
title: Azure AD v2 Windows Desktop ophalen gestart - gebruiken | Microsoft Docs
description: Hoe Windows Desktop .NET (XAML) toepassingen een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen
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
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 826ba0a00b26993d4f37f0a8ce587d7bb77e7eb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>De Microsoft Authentication Library (MSAL) gebruiken voor het ophalen van een token voor de Microsoft Graph-API

Deze sectie wordt beschreven hoe MSAL gebruiken voor een token de Microsoft Graph API.

1.  In `MainWindow.xaml.cs`, voeg de referentie voor MSAL bibliotheek toe aan de klasse:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Vervang <code>MainWindow</code> code die een klasse:
</li>
</ol>

```csharp
public partial class MainWindow : Window
{
    //Set the API Endpoint to Graph 'me' endpoint
    string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

    //Set the scope for API call to user.read
    string[] _scopes = new string[] { "user.read" };


    public MainWindow()
    {
        InitializeComponent();
    }

    /// <summary>
    /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
    /// </summary>
    private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
    {
        AuthenticationResult authResult = null;

        try
        {
            authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
        }
        catch (MsalUiRequiredException ex)
        {
            // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
            System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
            }
            catch (MsalException msalex)
            {
                ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
            }
        }
        catch (Exception ex)
        {
            ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
            return;
        }

        if (authResult != null)
        {
            ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie
#### <a name="getting-a-user-token-interactive"></a>Een token ophalen interactieve
Het aanroepen van de `AcquireTokenAsync` methode resulteert in een venster vraagt de gebruiker aan te melden. Toepassingen vereisen meestal een gebruiker zich aanmelden interactief de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron, of wanneer een achtergrond-bewerking te verkrijgen van een token mislukt (bijvoorbeeld van de gebruiker het wachtwoord verlopen).

#### <a name="getting-a-user-token-silently"></a>Een gebruiker ophalen achtergrond token
`AcquireTokenSilentAsync`token acquisities van organisaties en verlenging zonder tussenkomst van de gebruiker worden verwerkt. Na `AcquireTokenAsync` wordt uitgevoerd voor de eerste keer `AcquireTokenSilentAsync` is de gebruikelijke methode gebruikt voor het verkrijgen van tokens die worden gebruikt voor toegang tot beveiligde bronnen voor volgende aanroepen - aanroepen aan te vragen of vernieuwen van tokens op de achtergrond worden aangebracht.
Uiteindelijk `AcquireTokenSilentAsync` mislukt: bijvoorbeeld de gebruiker heeft zich afgemeld, of het wachtwoord op een ander apparaat is gewijzigd. Wanneer MSAL detecteert dat het probleem doordat een interactieve actie kan worden omgezet, wordt deze gebeurtenis wordt gestart een `MsalUiRequiredException`. Uw toepassing kan verwerken van deze uitzondering op twee manieren:

1.  Een aanroep tegen `AcquireTokenAsync` onmiddellijk, wat ertoe leidt de gebruiker om aan te melden. Dit patroon wordt meestal gebruikt in de on line toepassingen wanneer er geen offline inhoud in de toepassing beschikbaar is voor de gebruiker is. Dit patroon maakt gebruik van de steekproef die worden gegenereerd door deze Begeleide instelprocedure: u kunt deze bekijken in actie de eerste keer dat u het voorbeeld uitvoert: omdat er geen gebruiker ooit de toepassing gebruikt `PublicClientApp.Users.FirstOrDefault()` bevat een null-waarde en een `MsalUiRequiredException` uitzondering gegenereerd. De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `AcquireTokenAsync` waardoor vraagt de gebruiker om aan te melden.

2.  Toepassingen kunnen ook een visuele indicatie maken voor de gebruiker die een interactief aanmelden is vereist, zodat de gebruiker het juiste moment aan te melden kunt selecteren of de toepassing opnieuw kunt `AcquireTokenSilentAsync` op een later tijdstip. Dit wordt meestal gebruikt wanneer de gebruiker andere functies van de toepassing gebruiken kunt zonder wordt onderbroken - bijvoorbeeld offline inhoud beschikbaar is in de toepassing is. In dit geval wordt de gebruiker kunt bepalen wanneer ze willen aanmelden voor toegang tot de beveiligde bron, of om de verouderde gegevens te vernieuwen of uw toepassing kunt ervoor kiezen om opnieuw te proberen `AcquireTokenSilentAsync` wanneer netwerk is hersteld na een tijdelijk niet beschikbaar.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>De Microsoft Graph API met behulp van het token dat u zojuist hebt verkregen aanroepen

1. Toevoegen van de nieuwe methode hieronder uw `MainWindow.xaml.cs`. De methode wordt gebruikt om een `GET` -aanvraag in voor Graph API met behulp van een autoriseren-header:

```csharp
/// <summary>
/// Perform an HTTP GET request to a URL using an HTTP Authorization header
/// </summary>
/// <param name="url">The URL</param>
/// <param name="token">The token</param>
/// <returns>String containing the results of the GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add the token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```
<!--start-collapse-->
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Meer informatie over het maken van een REST-aanroep op basis van een beveiligde API

In deze voorbeeldtoepassing de `GetHttpContentWithToken` methode wordt gebruikt voor het maken van een HTTP `GET` aanvragen op basis van een beveiligde bron die is een token vereist en vervolgens de inhoud naar de aanroeper wordt geretourneerd. Met deze methode wordt het token verkregen in de *HTTP-autorisatie-header*. Voor dit voorbeeld wordt de resource is de Microsoft Graph API *mij* eindpunt – waarin informatie over het profiel van de gebruiker.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Een methode voor het ondertekenen van de gebruiker toevoegen

1. Voeg de volgende methode voor uw `MainWindow.xaml.cs` afmelden van de gebruiker:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    if (App.PublicClientApp.Users.Any())
    {
        try
        {
            App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Meer informatie over afmelden

`SignOutButton_Click`verwijdert de gebruiker uit MSAL gebruikerscache – dit effectief MSAL laat weten om de huidige gebruiker vergeet, zodat een toekomstige aanvraag voor een token verkrijgen alleen lukt als het gaat interactief zijn.
Hoewel de toepassing in dit voorbeeld een enkele gebruiker ondersteunt, ondersteunt MSAL scenario's waarbij meerdere accounts kunnen worden aangemeld op hetzelfde moment: een voorbeeld is een e-mailtoepassing waar een gebruiker heeft meerdere accounts.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Token standaardgegevens weergeven

1. Toevoegen van de volgende methode voor uw `MainWindow.xaml.cs` om algemene informatie over het token weer te geven:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
        TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```
<!--start-collapse-->
### <a name="more-information"></a>Meer informatie

Tokens die zijn verkregen *OpenID Connect* bevatten ook een kleine subset van de informatie die relevant zijn voor de gebruiker. `DisplayBasicTokenInfo`algemene informatie opgenomen in het token wordt weergegeven: bijvoorbeeld de weergavenaam van de gebruiker en-ID, evenals de vervaldatum van token en de toegang tot het type string token zelf. Deze informatie wordt weergegeven voor u om te zien. U kunt raakt de *Microsoft Graph-API aanroepen* meerdere keren knop en Zie dat hetzelfde token opnieuw is gebruikt voor volgende aanvragen. U ziet ook de verloopdatum wordt verlengd wanneer MSAL het beslist is tijd om het token te verlengen.
<!--end-collapse-->

