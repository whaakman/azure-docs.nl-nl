---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/18/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: d4ba15e4ad46044c04c242c8805af9f320e95150
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368443"
---
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>MSAL gebruiken om op te halen van een token voor de Microsoft Graph API

In deze sectie maakt u MSAL gebruiken om op te halen van een token voor de Microsoft Graph API.

1.  In de *MainWindow.xaml.cs* bestand, voeg de referentie voor MSAL toe aan de klasse:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Vervang de `MainWindow` klasse met de volgende code:

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

            var app = App.PublicClientApp;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;

            var accounts = await app.GetAccountsAsync();

            try
            {
                authResult = await app.AcquireTokenSilentAsync(_scopes, accounts.FirstOrDefault());
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

#### <a name="get-a-user-token-interactively"></a>Een gebruiker interactief token ophalen

Aanroepen van de `AcquireTokenAsync` methode resulteert in een venster waarin gebruikers zich aanmelden. Toepassingen vereisen meestal gebruikers zich interactief aanmelden de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron. Ze kunnen ook moet zich aanmelden als een bewerking op de achtergrond is een token ophalen is mislukt (bijvoorbeeld wanneer het wachtwoord van een gebruiker is verlopen).

#### <a name="get-a-user-token-silently"></a>Een gebruiker op de achtergrond token ophalen

De `AcquireTokenSilentAsync` methode wordt gebruikt voor token-aankopen en vernieuwingen zonder tussenkomst van de gebruiker. Na `AcquireTokenAsync` wordt uitgevoerd voor de eerste keer `AcquireTokenSilentAsync` is de gebruikelijke methode om te gebruiken om te verkrijgen van tokens die toegang krijgen beveiligde bronnen voor volgende aanroepen tot omdat aanroepen aan te vragen of vernieuwen van tokens op de achtergrond worden gemaakt.

Uiteindelijk de `AcquireTokenSilentAsync` methode mislukken. Oorzaak van de fout kunnen zijn dat de gebruiker heeft zich afgemeld of het wachtwoord op een ander apparaat gewijzigd. Wanneer MSAL detecteert dat het probleem kan worden omgezet door verlangen dat ze een interactieve actie, wordt deze gebeurtenis wordt gestart een `MsalUiRequiredException` uitzondering. Uw toepassing kan verwerken deze uitzondering op twee manieren:

* Er kan een aanroep tegen `AcquireTokenAsync` onmiddellijk. Deze aanroep leidt de gebruiker zich aanmeldt. Dit patroon wordt meestal gebruikt in online toepassingen waarbij er geen beschikbaar offline inhoud voor de gebruiker. Het voorbeeld dat is gegenereerd door deze Begeleide instelling volgt dit patroon, die u kunt zien in actie de eerste keer dat u het voorbeeld uitvoert. 

* Omdat er geen gebruiker heeft de toepassing gebruikt `PublicClientApp.Users.FirstOrDefault()` bevat een null-waarde en een `MsalUiRequiredException` uitzondering is opgetreden. 

* De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `AcquireTokenAsync`, wat ertoe leidt dat u wordt gevraagd de gebruiker zich aanmeldt.

* Het kan in plaats daarvan een visuele indicatie opleveren voor gebruikers die een interactieve aanmelding vereist is, zodat ze het juiste moment aan te melden bij kunnen selecteren. Of de toepassing opnieuw kunt proberen `AcquireTokenSilentAsync` later opnieuw. Dit patroon wordt vaak gebruikt wanneer gebruikers de functionaliteit van andere toepassing zonder onderbreking--bijvoorbeeld gebruiken kunnen bij offline-inhoud beschikbaar in de toepassing is. Gebruikers kunnen in dit geval beslissen wanneer ze aanmelden willen bij de toegang tot de beveiligde bron of de verouderde gegevens vernieuwen. U kunt ook de toepassing kunt bepalen om opnieuw te proberen `AcquireTokenSilentAsync` wanneer het netwerk is hersteld na zijn tijdelijk niet beschikbaar.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>De Microsoft Graph-API aanroepen met behulp van het token dat u zojuist hebt verkregen

Voeg de volgende nieuwe methode voor uw `MainWindow.xaml.cs`. De methode wordt gebruikt om een `GET` aanvraag indient voor Graph API met behulp van een autoriseren-header:

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
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Meer informatie over het maken van een REST-aanroep op basis van een beveiligde API

In deze voorbeeldtoepassing gebruikt u de `GetHttpContentWithToken` methode voor het maken van een HTTP `GET` aanvragen op basis van een beveiligde resource waarvoor een token en vervolgens de inhoud wordt geretourneerd voor de oproepende functie. Deze methode wordt het verkregen token in de HTTP-autorisatie-header. Voor dit voorbeeld is de bron de Microsoft Graph API *me* eindpunt, waarin de profielgegevens van de gebruiker worden weergegeven.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Een methode voor het afmelden van een gebruiker toevoegen

Toevoegen als u wilt zich afmelden van een gebruiker, de volgende methode aan uw `MainWindow.xaml.cs` bestand:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private async void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    var accounts = await App.PublicClientApp.GetAccountsAsync(); 

    if (accounts.Any())
    {
        try
        {
            await App.PublicClientApp.RemoveAsync(accounts.FirstOrDefault()); 
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
### <a name="more-information-about-user-sign-out"></a>Meer informatie over de gebruiker afmelden

De `SignOutButton_Click` methode gebruikers worden verwijderd uit de cache van de gebruiker MSAL, die MSAL weet vergeten van de huidige gebruiker, zodat een toekomstige aanvraag om een token te verkrijgen slaagt alleen als deze is gemaakt interactief zijn.

Hoewel de toepassing in dit voorbeeld biedt ondersteuning voor afzonderlijke gebruikers, ondersteunt MSAL scenario's waarbij meerdere accounts kunnen worden aangemeld op hetzelfde moment. Een voorbeeld is een e-mailtoepassing waar een gebruiker meerdere accounts heeft.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Token basisinformatie weergeven

Als algemene informatie over het token weergeven, toevoegen de volgende methode aan uw *MainWindow.xaml.cs* bestand:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Username: {authResult.Account.Username}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie

Naast het toegangstoken dat wordt gebruikt voor de Microsoft Graph-API aanroepen nadat de gebruiker zich aanmeldt bij, verkrijgt MSAL ook een ID-token. Dit token bevat een kleine subset van de informatie die relevant zijn voor gebruikers. De `DisplayBasicTokenInfo` de algemene informatie die opgenomen in het token wordt weergegeven. Bijvoorbeeld, wordt de weergavenaam van de gebruiker en -ID, evenals de vervaldatum van token en de tekenreeks die het toegangstoken zelf. U kunt selecteren de *Microsoft Graph-API aanroepen* meerdere keren knop en Zie dat hetzelfde token opnieuw werd gebruikt voor volgende aanvragen. U ziet ook de verloopdatum wordt verlengd wanneer MSAL het besluit is tijd om het token te verlengen.
<!--end-collapse-->

