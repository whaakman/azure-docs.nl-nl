---
title: Microsoft identity-platform UWP aan de slag | Azure
description: Toegangstokens hoe Universal Windows Platform-toepassingen (UWP) een API kunnen aanroepen die worden vereist door het eindpunt van Microsoft identity-platform.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/16/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff2089e8abdde8e6a99de1be2be070fb457fa632
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68276645"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Microsoft Graph API aanroepen vanuit een Universal Windows Platform-toepassing (XAML)

> [!div renderon="docs"]

Deze handleiding wordt uitgelegd hoe een systeemeigen Universal Windows Platform (UWP)-toepassing kan aanvragen van een toegangstoken en roep vervolgens Microsoft Graph API. De handleiding geldt ook voor andere API's waarvoor toegangstokens van het eindpunt van Microsoft identity-platform.

Aan het einde van deze handleiding, worden in uw toepassing een beveiligde API aanroept met behulp van persoonlijke accounts. Voorbeelden zijn outlook.com, live.com en anderen. Werk-en schoolaccounts aanroepen uw toepassing ook van een bedrijf of organisatie die Azure Active Directory (Azure AD) is.

>[!NOTE]
> Deze handleiding moet Visual Studio 2017 met Universal Windows Platform-ontwikkeling die zijn geïnstalleerd. Zie [instellen](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) voor instructies voor het downloaden en configureren van Visual Studio voor het ontwikkelen van Universal Windows Platform-apps.

## <a name="how-this-guide-works"></a>De werking van deze handleiding

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze zelfstudie werkt](./media/tutorial-v2-windows-uwp/uwp-intro.svg)

In deze handleiding wordt gemaakt voor een UWP-voorbeeldtoepassing die query's van Microsoft Graph API of een Web-API die tokens van het eindpunt van de Microsoft identity-platform accepteert. Voor dit scenario wordt een token toegevoegd aan de HTTP-aanvragen via de autorisatie-header. Microsoft Authentication Library (MSAL) wordt gebruikt voor token-aankopen en vernieuwingen.

## <a name="nuget-packages"></a>NuGet-pakketten

Deze handleiding maakt gebruik van de volgende NuGet-pakketten:

|Bibliotheek|Description|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library|

## <a name="set-up-your-project"></a>Uw project instellen

In deze sectie biedt stapsgewijze instructies voor het integreren van een Windows Desktop .NET-toepassing (XAML) *aanmelden met Microsoft*. Vervolgens kan deze Web-API's waarvoor een token, zoals Microsoft Graph API opvragen.

Deze handleiding wordt gemaakt van een toepassing die wordt weergegeven een knop die query's Graph API, een knop Afmelden en tekstvakken die de resultaten van de aanroepen weergeven.

> [!NOTE]
> Wilt u dit voorbeeld van Visual Studio-project in plaats daarvan downloaden? [Een project hebt gedownload](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip) en gaat u naar de [toepassingsregistratie](#register-your-application "toepassing registratiestap") stap voor het configureren van de voorbeeldcode voordat deze wordt uitgevoerd.

### <a name="create-your-application"></a>Uw toepassing maken

1. Selecteer in Visual Studio **Bestand** > **Nieuw** > **Project**.
2. Onder **sjablonen**, selecteer **Visual C#** .
3. Selecteer **Lege app (Universal Windows)** .
4. De naam van de app, en selecteer **OK**.
5. Als u hierom wordt gevraagd, selecteert u een willekeurige versie van **doel** en **Minimum** versies en selecteer **OK**.

    >![Minimum- en doel-versies](./media/tutorial-v2-windows-uwp/vs-minimum-target.png)

## <a name="add-microsoft-authentication-library-to-your-project"></a>Microsoft Authentication Library toevoegen aan uw project
1. Selecteer in Visual Studio **Tools** > **NuGet Package Manager** > **Package Manager Console**.
2. Kopieer en plak de volgende opdracht in de **Package Manager Console** venster:

    ```powershell
    Install-Package Microsoft.Identity.Client -IncludePrerelease
    ```

> [!NOTE]
> Met deze opdracht installeert [Microsoft Authentication Library](https://aka.ms/msal-net). MSAL verkrijgt, in de cache opgeslagen en wordt vernieuwd gebruikerstokens die toegang API's die zijn beveiligd door Microsoft identity-platform tot.

## <a name="create-your-applications-ui"></a>Gebruikersinterface van uw toepassing maken

Een **MainPage.xaml** bestand wordt automatisch gemaakt als onderdeel van de projectsjabloon, maken. Dit bestand openen en volg de instructies:

* Vervangen van uw toepassing **raster** knooppunt met de volgende code:

    ```xml
    <Grid>
        <StackPanel Background="Azure">
            <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
                <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
                <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
            </StackPanel>
            <TextBlock Text="API Call Results" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
            <TextBlock Text="Token Info" Margin="2,0,0,-5" FontFamily="Segoe Ui" />
            <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
        </StackPanel>
    </Grid>
    ```
    
## <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>MSAL gebruiken om op te halen van een token voor Microsoft Graph API

In deze sectie ziet u hoe u MSAL om op te halen van een token voor Microsoft Graph API.

1.  In **MainPage.xaml.cs**, voeg de referentie voor MSAL toe aan de klasse:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Vervang de code van uw <code>MainPage</code> klasse met de volgende code:

    ```csharp
    public sealed partial class MainPage : Page
    {
        //Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

        //Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };

        // Below are the clientId (Application Id) of your app registration and the tenant information. 
        // You have to replace:
        // - the content of ClientID with the Application Id for your app registration
        // - Te content of Tenant by the information about the accounts allowed to sign-in in your application:
        //   - For Work or School account in your org, use your tenant ID, or domain
        //   - for any Work or School accounts, use organizations
        //   - for any Work or School accounts, or Microsoft personal account, use common
        //   - for Microsoft Personal account, use consumers
        private const string ClientId = "0b8b0665-bc13-4fdc-bd72-e0227b9fc011";        

        public IPublicClientApplication PublicClientApp { get; } 

        public MainPage()
        {
          this.InitializeComponent();

          PublicClientApp = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(AadAuthorityAudience.AzureAdAndPersonalMicrosoftAccount)
                .WithLogging((level, message, containsPii) =>
                {
                    Debug.WriteLine($"MSAL: {level} {message} ");
                }, LogLevel.Warning, enablePiiLogging:false,enableDefaultPlatformLogging:true)
                .WithUseCorporateNetwork(true)
                .Build();
        }

        /// <summary>
        /// Call AcquireTokenInteractive - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
         AuthenticationResult authResult = null;
         ResultText.Text = string.Empty;
         TokenInfoText.Text = string.Empty;

         // It's good practice to not do work on the UI thread, so use ConfigureAwait(false) whenever possible.            
         IEnumerable<IAccount> accounts = await PublicClientApp.GetAccountsAsync().ConfigureAwait(false); 
         IAccount firstAccount = accounts.FirstOrDefault();

         try
         {
          authResult = await PublicClientApp.AcquireTokenSilent(scopes, firstAccount)
                                                  .ExecuteAsync();
         }
         catch (MsalUiRequiredException ex)
         {
          // A MsalUiRequiredException happened on AcquireTokenSilent.
          // This indicates you need to call AcquireTokenInteractive to acquire a token
          System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

          try
          {
           authResult = await PublicClientApp.AcquireTokenInteractive(scopes)
                                                      .ExecuteAsync()
                                                      .ConfigureAwait(false);
           }
           catch (MsalException msalex)
           {
            await DisplayMessageAsync($"Error Acquiring Token:{System.Environment.NewLine}{msalex}");
           }
          }
          catch (Exception ex)
          {
           await DisplayMessageAsync($"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}");
           return;
          }

          if (authResult != null)
          {
           var content = await GetHttpContentWithToken(graphAPIEndpoint,
                                                       authResult.AccessToken).ConfigureAwait(false);

           // Go back to the UI thread to make changes to the UI
           await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
           {
            ResultText.Text = content;
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
           });
          }
        }
    ```

### <a name="more-information"></a>Meer informatie

#### <a name="get-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

Een aanroep naar de `AcquireTokenInteractive` methode resulteert in een venster waarin gebruikers zich aanmelden. Toepassingen vereisen meestal gebruikers zich interactief aanmelden de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron. Ze kunnen ook moet zich aanmelden als een bewerking op de achtergrond is een token ophalen is mislukt. Een voorbeeld daarvan is wanneer het wachtwoord van een gebruiker is verlopen.

#### <a name="get-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

De `AcquireTokenSilent` methode wordt gebruikt voor token-aankopen en vernieuwingen zonder tussenkomst van de gebruiker. Na `AcquireTokenInteractive` voor de eerste keer wordt uitgevoerd en de gebruiker wordt gevraagd om referenties, het `AcquireTokenSilent` methode moet worden gebruikt voor het aanvragen van tokens voor volgende aanroepen omdat deze tokens op de achtergrond verkrijgt. MSAL wordt afgehandeld tokencache en verlenging.

Uiteindelijk de `AcquireTokenSilent` methode mislukt. Oorzaak van de fout kunnen zijn dat gebruikers hebt zich afgemeld of het wachtwoord op een ander apparaat gewijzigd. Wanneer MSAL detecteert dat het probleem kan worden omgezet door verlangen dat ze een interactieve actie, wordt deze gebeurtenis wordt gestart een `MsalUiRequiredException` uitzondering. Uw toepassing kan verwerken deze uitzondering op twee manieren:

* Er kan een aanroep tegen `AcquireTokenInteractive` onmiddellijk. Deze aanroep leidt de gebruiker zich aanmeldt. Normaal gesproken dit patroon in online toepassingen wordt gebruikt wanneer er geen beschikbare offline inhoud voor de gebruiker is. Het voorbeeld dat is gegenereerd door deze Begeleide instelling volgt het patroon. U wordt het in actie de eerste keer dat u het voorbeeld uitvoert.
  * Omdat er geen gebruiker heeft de toepassing gebruikt `accounts.FirstOrDefault()` bevat een null-waarde en een `MsalUiRequiredException` uitzondering is opgetreden.
  * De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `AcquireTokenInteractive`. Deze aanroep leidt de gebruiker zich aanmeldt.

* Of in plaats daarvan geeft deze een visuele indicatie voor gebruikers die een interactieve aanmelding vereist is. Vervolgens kunnen ze het juiste moment aan te melden bij selecteren. Of de toepassing opnieuw kunt proberen `AcquireTokenSilent` later opnieuw. Dit patroon wordt vaak gebruikt wanneer gebruikers de functionaliteit van andere toepassing zonder onderbreking kunnen gebruiken. Een voorbeeld is wanneer u offline inhoud is beschikbaar in de toepassing. Gebruikers kunnen in dit geval beslissen wanneer ze aanmelden willen bij de toegang tot de beveiligde bron of de verouderde gegevens vernieuwen. Of anders de toepassing kan beslissen om opnieuw te proberen `AcquireTokenSilent` wanneer het netwerk is hersteld nadat deze tijdelijk niet beschikbaar is.

## <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Microsoft Graph API aanroepen met behulp van het token dat u zojuist hebt verkregen.

* Voeg de volgende nieuwe methode om te **MainPage.xaml.cs**. Deze methode wordt gebruikt om een `GET` aanvraag indient voor Graph API met behulp van een `Authorization` header:

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
            // Add the token in Authorization header
            request.Headers.Authorization = 
              new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
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

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Meer informatie over het maken van een REST-aanroep op basis van een beveiligde API

In deze voorbeeldtoepassing de `GetHttpContentWithToken` methode wordt gebruikt om een HTTP `GET` aanvraag indient voor een beveiligde resource waarvoor een token. De methode retourneert vervolgens de inhoud voor de oproepende functie. Met deze methode wordt het verkregen token in de **HTTP-autorisatie** header. Voor dit voorbeeld is de bron de Microsoft Graph API **me** eindpunt, waarin de profielgegevens van de gebruiker worden weergegeven.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Toevoegen van een methode voor de gebruiker afmelden

* Als u wilt de gebruiker afmelden, toevoegen de volgende methode om te **MainPage.xaml.cs**:

    ```csharp
    /// <summary>
    /// Sign out the current user
    /// </summary>
    private async void SignOutButton_Click(object sender, RoutedEventArgs e)
    {
        IEnumerable<IAccount> accounts = await PublicClientApp.GetAccountsAsync
                                                              .ConfigureAwait(false);
        IAccount firstAccount = accounts.FirstOrDefault();

        try
        {
            await PublicClientApp.RemoveAsync(firstAccount).ConfigureAwait(false);
            await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
            {
                ResultText.Text = "User has signed-out";
                this.CallGraphButton.Visibility = Visibility.Visible;
                    this.SignOutButton.Visibility = Visibility.Collapsed;
                });
            }
            catch (MsalException ex)
            {
                ResultText.Text = $"Error signing-out user: {ex.Message}";
            }
        }
    ```

> [!NOTE]
> MSAL.NET asynchrone methoden gebruikt om tokens verkrijgen of accounts te manipuleren, en daarom moet u zorgen voor het doen van UI-ed-acties in de UI-thread, dus de `Dispatcher.RunAsync`, en de voorzorgsmaatregelen om aan te roepen `ConfigureAwait(false)`

### <a name="more-information-on-sign-out"></a>Meer informatie over het afmelden

De `SignOutButton_Click` methode wordt de gebruiker verwijderd uit de cache van de gebruiker MSAL. Deze methode weet MSAL vergeten van de huidige gebruiker. En vervolgens een toekomstige aanvraag om een token te verkrijgen slaagt alleen als deze is gemaakt interactief zijn.
De toepassing in dit voorbeeld biedt ondersteuning voor één gebruiker. Maar MSAL biedt ondersteuning voor scenario's waarbij meer dan één account kan worden aangemeld op hetzelfde moment. Een voorbeeld is een e-mailtoepassing waar een gebruiker heeft diverse accounts.

## <a name="display-basic-token-information"></a>Token basisinformatie weergeven

* Voeg de volgende methode om te **MainPage.xaml.cs** om algemene informatie over het token weer te geven:

    ```csharp
    /// <summary>
    /// Display basic information contained in the token. Needs to be called from the UI thead.
    /// </summary>
    private void DisplayBasicTokenInfo(AuthenticationResult authResult)
    {
        TokenInfoText.Text = "";
        if (authResult != null)
        {
            TokenInfoText.Text += $"User Name: {authResult.Account.Username}" + Environment.NewLine;
            TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        }
    }
    ```

### <a name="more-information"></a>Meer informatie

ID-tokens die zijn verkregen **OpenID Connect** bevatten ook een kleine subset van de informatie die relevant zijn voor de gebruiker. `DisplayBasicTokenInfo` algemene informatie die is opgenomen in het token wordt weergegeven. Voorbeelden zijn de weergavenaam van de gebruiker en -ID, de vervaldatum van het token en de tekenreeks met het toegangstoken zelf. Als u selecteert de **Microsoft Graph-API aanroepen** meerdere keren knop, ziet u dat hetzelfde token opnieuw werd gebruikt voor volgende aanvragen. U ziet ook de datum van afloop voor uitgebreide wanneer MSAL besluit is het tijd om het token te verlengen.

## <a name="register-your-application"></a>Uw toepassing registreren

Nu moet u uw toepassing registreren in de Portal voor Appregistratie Microsoft:

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Als uw account in meer dan één Azure AD-tenant aanwezig is, selecteert u `Directory + Subscription` in de hoek rechtsboven in het menu boven op de pagina en switch uw portal-sessie met de gewenste Azure AD-tenant.
1. Navigeer naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) pagina.
1. Selecteer **registratie van nieuwe**.
   - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `UWP-App-calling-MSGraph`.
   - In de **ondersteund accounttypen** sectie, selecteer **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts (zoals Skype, Xbox, Outlook.com)** .
   - Selecteer **Registreren** om de toepassing te maken.
1. Op de app **overzicht** pagina, zoek de **(client) toepassings-ID** waarde en leg deze vast voor later. Ga terug naar Visual Studio, open **MainPage.xaml.cs**, en vervang de waarde van ClientId met de toepassings-ID die u zojuist hebt geregistreerd:
1. Selecteer in de lijst met pagina’s voor de app de optie **Verificatie**.
   1. In de **omleidings-URI's** sectie in de lijst van de omleidings-URI's:
   1. In de **TYPE** kolomselectie **openbare client (mobiele en desktop)** .
   1. Voer `urn:ietf:wg:oauth:2.0:oob` in de **OMLEIDINGS-URI** kolom.
1. Selecteer **Opslaan**.
1. Selecteer in de lijst van pagina's voor de app **API-machtigingen**
   - Klik op de **toevoegen van een machtiging** knop en klikt u vervolgens
   - Zorg ervoor dat de **Microsoft API's** tabblad is geselecteerd
   - In de *gebruikte Microsoft APIs* sectie, klikt u op **Microsoft Graph**
   - In de **overgedragen machtigingen** sectie, zorgt u ervoor dat de juiste machtigingen worden gecontroleerd: **User.Read**. Gebruik het zoekvak indien nodig.
   - Selecteer de **machtigingen toevoegen** knop

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Geïntegreerde verificatie inschakelen in federatieve domeinen (optioneel)

Windows-Integrated-verificatie inschakelen als deze wordt gebruikt met een federatieve Azure AD-domein, het toepassingsmanifest moet extra mogelijkheden inschakelen:

1. Dubbelklik op **Package.appxmanifest**.
2. Selecteer de **mogelijkheden** tabblad en zorg ervoor dat de volgende instellingen zijn ingeschakeld:

    - Enterprise-verificatie
    - Particuliere netwerken (Client en Server)
    - Gedeelde gebruikerscertificaten

> [!IMPORTANT]
> [Geïntegreerde Windows-authenticatie](https://aka.ms/msal-net-iwa) niet standaard ingeschakeld voor dit voorbeeld is geconfigureerd. Toepassingen die aanvragen *Ondernemingsverificatie* of *gedeelde gebruikerscertificaten* mogelijkheden vereisen een hoger niveau van controle door de Windows Store. Niet alle ontwikkelaars willen ook het hogere niveau van controle uitvoeren. Schakel deze instelling alleen als u geïntegreerde Windows-verificatie met een federatieve Azure AD-domein.

## <a name="test-your-code"></a>Uw code testen

Als u wilt testen van uw toepassing, selecteer F5 om uit te voeren van uw project in Visual Studio. Het hoofdvenster wordt weergegeven:

![De gebruikersinterface van de toepassing](./media/tutorial-v2-windows-uwp/testapp-ui.png)

Wanneer u klaar om te testen bent, selecteert u **Microsoft Graph-API aanroepen**. Gebruik vervolgens een Azure AD-organisatieaccount of een Microsoft-account, zoals live.com of outlook.com, aan te melden bij. Als de eerste keer is, ziet u een venster waarin de gebruiker zich aanmeldt:

![Aanmeldingspagina opgeven](./media/tutorial-v2-windows-uwp/sign-in-page.png)

### <a name="consent"></a>Toestemming geven

De eerste keer dat u zich aanmeldt bij uw toepassing, krijgt u een vergelijkbaar met de volgende instemmingsscherm. Selecteer **Ja** naar expliciet toestemming voor toegang tot:

![Het instemmingsscherm toegang](./media/tutorial-v2-windows-uwp/consentscreen.png)

### <a name="expected-results"></a>Verwachte resultaten

U ziet informatie uit gebruikersprofielen door de Microsoft Graph API-aanroep is geretourneerd op de **API Gespreksresultaten** scherm:

![API aanroepen resultaten scherm](./media/tutorial-v2-windows-uwp/uwp-results-screen.PNG)

U ziet ook basisinformatie over het token dat is verkregen `AcquireTokenInteractive` of `AcquireTokenSilent` in de **Token Info** vak:

|Eigenschap  |Indeling  |Description |
|---------|---------|---------|
|**Gebruikersnaam** |<span>user@domain.com</span> |De gebruikersnaam waarmee de gebruiker.|
|**Token is verlopen** |Datetime |De tijd wanneer het token is verlopen. MSAL vervolg op de vervaldatum vernieuwen van het token indien nodig.|

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereiken en gedelegeerde machtigingen

Microsoft Graph API vereist de *user.read* scope om te lezen van het gebruikersprofiel. Dit bereik wordt automatisch toegevoegd in elke toepassing die geregistreerd in de Portal voor Appregistratie standaard. Andere API's voor Microsoft Graph en de aangepaste API's voor uw back-endserver, mogelijk extra scopes. Microsoft Graph API vereist de *Calendars.Read* bereik om agenda's van de gebruiker weer te geven.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing moet toevoegen de *Calendars.Read* overgedragen machtigingen voor de registratiegegevens van de toepassing. Voeg de *Calendars.Read* bereik instellen op de `acquireTokenSilent` aanroepen.

> [!NOTE]
> Gebruikers mogelijk gevraagd extra toestemmingen als u het aantal bereiken verhogen.

## <a name="known-issues"></a>Bekende problemen

### <a name="issue-1"></a>Probleem 1

U ontvangt een van de volgende foutberichten weergegeven wanneer u zich aanmelden op uw toepassing in een federatieve Azure AD-domein:

* Er is geen geldig clientcertificaat gevonden in de aanvraag.
* Er is geen geldige certificaten gevonden in het certificaatarchief van de gebruiker.
* Probeer opnieuw een andere verificatiemethode kiezen.

**Oorzaak:** Enterprise- en certificaat-mogelijkheden zijn niet ingeschakeld.

**Oplossing:** Volg de stappen in [geïntegreerde verificatie op federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional).

### <a name="issue-2"></a>Probleem 2

U hebt ingeschakeld [geïntegreerde verificatie op federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional) en probeer het Windows Hello gebruiken op een Windows 10-computer aan te melden voor een omgeving met multi-factor authentication is geconfigureerd. De lijst van certificaten wordt weergegeven. Echter, als u ervoor kiest uw PINCODE moet gebruiken, het venster van de PINCODE wordt nooit weergegeven.

**Oorzaak:** Dit probleem is een bekende beperking van de webauthenticatiebroker in UWP-toepassingen die worden uitgevoerd op Windows 10 desktop. Het werkt prima op Windows 10 Mobile.

**Tijdelijke oplossing:** Selecteer **Meld u aan met andere opties**. Selecteer vervolgens **Meld u aan met een gebruikersnaam en wachtwoord**. Selecteer **geeft u uw wachtwoord**. Ga via de telefoon verificatieproces uit.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
