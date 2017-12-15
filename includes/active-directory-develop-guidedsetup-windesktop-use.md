
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Gebruik MSAL voor een token voor de Microsoft Graph-API

In deze sectie gebruikt u MSAL ophalen van een token voor de Microsoft Graph API.

1.  In de *MainWindow.xaml.cs* bestand, de verwijzing voor MSAL toevoegen aan de klasse:

    ```csharp
    using Microsoft.Identity.Client;
    ```
<!-- Workaround for Docs conversion bug -->

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
#### <a name="get-a-user-token-interactively"></a>Een gebruiker interactief token ophalen
Het aanroepen van de `AcquireTokenAsync` methode resulteert in een venster waarin gebruikers zich aanmelden. Toepassingen vereisen meestal gebruikers zich aanmelden in interactief de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron. Ze wellicht ook aan te melden bij een achtergrond te verkrijgen van een token mislukt (bijvoorbeeld wanneer een gebruiker het wachtwoord is verlopen).

#### <a name="get-a-user-token-silently"></a>Een gebruiker achtergrond token ophalen
De `AcquireTokenSilentAsync` methode token acquisities van organisaties en vernieuwingen zonder tussenkomst van de gebruiker worden verwerkt. Na `AcquireTokenAsync` wordt uitgevoerd voor de eerste keer `AcquireTokenSilentAsync` is de gebruikelijke methode gebruiken om toegang te verkrijgen van tokens die toegang beveiligde bronnen voor volgende aanroepen tot omdat aanroepen naar aanvragen of vernieuwen van tokens op de achtergrond worden gemaakt.

Uiteindelijk de `AcquireTokenSilentAsync` methode mislukken. Oorzaak van de fout is mogelijk dat de gebruiker heeft zich afgemeld of het wachtwoord op een ander apparaat gewijzigd. Wanneer MSAL detecteert dat het probleem doordat een interactieve actie kan worden omgezet, wordt deze gebeurtenis wordt gestart een `MsalUiRequiredException` uitzondering. Uw toepassing kan verwerken van deze uitzondering op twee manieren:

* Er kan een aanroep van tegen `AcquireTokenAsync` onmiddellijk. Deze aanroep resulteert in vraagt de gebruiker aan te melden. Dit patroon wordt meestal gebruikt in de on line toepassingen wanneer er geen beschikbare offline inhoud voor de gebruiker is. De steekproef die worden gegenereerd door deze Begeleide instelprocedure volgt dit patroon die u kunt zien in actie de eerste keer dat u het voorbeeld uitvoert. 
    * Omdat er geen gebruiker heeft de toepassing gebruikt `PublicClientApp.Users.FirstOrDefault()` bevat een null-waarde en een `MsalUiRequiredException` uitzondering gegenereerd. 
    * De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `AcquireTokenAsync`, wat ertoe leidt de gebruiker aan te melden.

* Het kan in plaats daarvan een visuele indicatie opleveren voor gebruikers die een interactief aanmelden vereist is, zodat ze het juiste moment aan te melden kunnen selecteren. Of de toepassing opnieuw kunt `AcquireTokenSilentAsync` later. Dit patroon wordt vaak gebruikt als gebruikers andere toepassingsfunctionaliteit van de zonder onderbreking--bijvoorbeeld gebruiken kunnen wanneer u offline inhoud is beschikbaar in de toepassing. In dit geval bepalen gebruikers wanneer ze aanmelden willen bij de toegang tot de beveiligde bron of de verouderde gegevens te vernieuwen. U kunt ook de toepassing opnieuw kunt bepalen `AcquireTokenSilentAsync` wanneer het netwerk is hersteld nadat u hebt tijdelijk niet beschikbaar zijn.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>De Microsoft Graph-API aanroepen via het token dat u zojuist hebt verkregen

Voeg de volgende nieuwe methode voor uw `MainWindow.xaml.cs`. De methode wordt gebruikt om een `GET` aanvraag tegen Graph API met behulp van een autoriseren-header:

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

In deze voorbeeldtoepassing gebruikt u de `GetHttpContentWithToken` methode voor het maken van een HTTP `GET` aanvragen op basis van een beveiligde bron die is een token vereist en vervolgens de inhoud naar de aanroeper wordt geretourneerd. Deze methode wordt het token verkregen in de HTTP-autorisatie-header. Voor dit voorbeeld wordt de resource is de Microsoft Graph API *mij* eindpunt waarin informatie over het profiel van de gebruiker.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-a-user"></a>Een methode voor het ondertekenen van een gebruiker toevoegen

Als u wilt een gebruiker afmelden, voegt u de volgende methode voor uw `MainWindow.xaml.cs` bestand:

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
### <a name="more-information-about-user-sign-out"></a>Meer informatie over gebruikers afmelden

De `SignOutButton_Click` methode gebruikers verwijdert uit de cache van de gebruiker MSAL, waardoor effectief MSAL vergeten van de huidige gebruiker, zodat een toekomstige aanvraag voor een token verkrijgen slaagt alleen als interactief zijn wordt gemaakt.

Hoewel de toepassing in dit voorbeeld één gebruikers ondersteunt, ondersteunt MSAL scenario's waarbij meerdere accounts kunnen worden ondertekend op hetzelfde moment. Een voorbeeld is een e-mailtoepassing waar een gebruiker heeft meerdere accounts.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Token standaardgegevens weergeven

Voor algemene informatie over het token wordt weergegeven, voegt u de volgende methode voor uw *MainWindow.xaml.cs* bestand:

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

Naast het toegangstoken dat wordt gebruikt voor de Microsoft Graph-API aanroepen nadat de gebruiker zich aanmeldt, verkrijgt MSAL ook een token ID. Dit token bevatten een kleine subset van de informatie die relevant zijn voor gebruikers. De `DisplayBasicTokenInfo` de basisgegevens die opgenomen in het token wordt weergegeven. Bijvoorbeeld, geeft de weergavenaam van de gebruiker en -ID, evenals de vervaldatum van token en het type string het toegangstoken zelf. U kunt selecteren de *Microsoft Graph-API aanroepen* meerdere keren knop en Zie dat hetzelfde token opnieuw is gebruikt voor volgende aanvragen. U ziet ook de verloopdatum wordt verlengd wanneer MSAL het beslist is tijd om het token te verlengen.
<!--end-collapse-->

