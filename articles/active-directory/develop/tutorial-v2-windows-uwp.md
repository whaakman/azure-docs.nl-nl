---
title: Aan de slag met micro soft Identity platform UWP | Azure
description: Hoe Universeel Windows-platform-toepassingen (UWP) een API kunnen aanroepen die toegangs tokens vereist voor het micro soft Identity platform-eind punt.
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
ms.openlocfilehash: dae3f9f2f4a9f6966d3453123773887c244cf4e2
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334070"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Microsoft Graph-API aanroepen vanuit een Universeel Windows-platform toepassing (XAML)

> [!div renderon="docs"]

In deze hand leiding wordt uitgelegd hoe een systeem eigen Universeel Windows-platform-toepassing (UWP) een toegangs token kan aanvragen en vervolgens Microsoft Graph API aanroept. De hand leiding is ook van toepassing op andere Api's waarvoor toegangs tokens van het micro soft Identity platform-eind punt zijn vereist.

Aan het einde van deze hand leiding roept uw toepassing een beveiligde API aan met persoonlijke accounts. Voor beelden zijn outlook.com, live.com en anderen. Uw toepassing roept ook werk-en school accounts aan van een bedrijf of organisatie met Azure Active Directory (Azure AD).

>[!NOTE]
> Voor deze hand leiding is Visual Studio 2017 met Universeel Windows-platform-ontwikkeling geïnstalleerd. Zie [instellen](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) voor instructies voor het downloaden en configureren van Visual Studio voor het ontwikkelen van universeel Windows-platform-apps.

## <a name="how-this-guide-works"></a>De werking van deze handleiding

![Toont hoe de voor beeld-app die door deze zelf studie wordt gegenereerd, werkt](./media/tutorial-v2-windows-uwp/uwp-intro.svg)

Deze hand leiding maakt een voor beeld van een UWP-toepassing die query's uitvoert Microsoft Graph-API of een web-API die tokens accepteert van het micro soft Identity platform-eind punt. Voor dit scenario wordt een token toegevoegd aan HTTP-aanvragen via de autorisatie-header. Micro soft Authentication Library (MSAL) verwerkt verwervingen en vernieuwingen van tokens.

## <a name="nuget-packages"></a>NuGet-pakketten

Deze hand leiding maakt gebruik van de volgende NuGet-pakketten:

|Bibliotheek|Description|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library|

## <a name="set-up-your-project"></a>Uw project instellen

In deze sectie vindt u stapsgewijze instructies voor het integreren van een Windows Desktop .NET-toepassing (XAML) met *Aanmelden bij micro soft*. Vervolgens kunt u een query uitvoeren op Web-Api's waarvoor een token is vereist, zoals Microsoft Graph-API.

In deze hand leiding wordt een toepassing gemaakt waarmee een knop wordt weer gegeven die Graph API, een knop voor het afmelden en tekst vakken waarin de resultaten van de aanroepen worden weer gegeven.

> [!NOTE]
> Wilt u in plaats daarvan het Visual Studio-project van dit voor beeld downloaden? [Down load een project](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/msal3x.zip) en ga naar [de stap]toepassings registratie(#register-your-application "toepassings") registratie om het code voorbeeld te configureren voordat deze wordt uitgevoerd.

### <a name="create-your-application"></a>Uw toepassing maken

1. Selecteer in Visual Studio **Bestand** > **Nieuw** > **Project**.
2. Onder **sjablonen**selecteert u **Visual C#** .
3. Selecteer **Lege app (Universal Windows)** .
4. Geef de app een naam en selecteer **OK**.
5. Als hierom wordt gevraagd, selecteert u een wille keurige versie voor **doel** en **minimum** versie en selecteert u **OK**.

    >![Minimum-en doel versies](./media/tutorial-v2-windows-uwp/vs-minimum-target.png)

## <a name="add-microsoft-authentication-library-to-your-project"></a>Micro soft-verificatie bibliotheek toevoegen aan uw project
1. Selecteer in Visual Studio **Tools** > **NuGet Package Manager** > **Package Manager Console**.
2. Kopieer en plak de volgende opdracht in het venster **Package Manager-console** :

    ```powershell
    Install-Package Microsoft.Identity.Client -IncludePrerelease
    ```

> [!NOTE]
> Met deze opdracht [wordt de micro soft-verificatie bibliotheek](https://aka.ms/msal-net)geïnstalleerd. MSAL verkrijgt, bewaart en vernieuwt gebruikers tokens die toegang hebben tot Api's die worden beveiligd door het micro soft Identity-platform.

## <a name="create-your-applications-ui"></a>De gebruikers interface van uw toepassing maken

Er wordt automatisch een **MainPage. xaml** -bestand gemaakt als onderdeel van de project sjabloon. Open dit bestand en volg de instructies:

* Vervang het **raster** knooppunt van uw toepassing door de volgende code:

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
    
## <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>MSAL gebruiken om een token op te halen voor de Microsoft Graph-API

In deze sectie wordt uitgelegd hoe u MSAL gebruikt om een token op te halen voor Microsoft Graph-API.

1.  Voeg in **MainPage.xaml.cs**de verwijzing voor MSAL toe aan de klasse:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. Vervang de code van uw <code>MainPage</code> klasse door de volgende code:

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

Een aanroep van de `AcquireTokenInteractive` methode resulteert in een venster waarin de gebruiker wordt gevraagd zich aan te melden. Toepassingen vereisen doorgaans dat gebruikers zich interactief aanmelden wanneer ze voor het eerst toegang hebben tot een beveiligde bron. Ze moeten zich mogelijk ook aanmelden wanneer een stille bewerking voor het verkrijgen van een token mislukt. Een voor beeld hiervan is wanneer het wacht woord van een gebruiker is verlopen.

#### <a name="get-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

De `AcquireTokenSilent` -methode verwerkt verwervingen en vernieuwingen van tokens zonder tussen komst van de gebruiker. Nadat `AcquireTokenInteractive` het voor de eerste keer is uitgevoerd en de gebruiker om referenties wordt gevraagd, `AcquireTokenSilent` moet de methode worden gebruikt voor het aanvragen van tokens voor daaropvolgende aanroepen omdat de tokens op de achtergrond worden opgehaald. MSAL verwerkt de token cache en de verlenging.

Uiteindelijk mislukt de `AcquireTokenSilent` methode. De oorzaak van de fout is dat gebruikers zich afmelden of hun wacht woord hebben gewijzigd op een ander apparaat. Wanneer MSAL detecteert dat het probleem kan worden opgelost door een interactieve actie te vereisen, wordt `MsalUiRequiredException` er een uitzonde ring geactiveerd. Uw toepassing kan deze uitzonde ring op twee manieren afhandelen:

* Dit kan `AcquireTokenInteractive` direct een gesprek voeren. Met deze aanroep wordt de gebruiker gevraagd zich aan te melden. Normaal gesp roken wordt dit patroon gebruikt in online toepassingen waarbij er geen offline-inhoud beschikbaar is voor de gebruiker. Het voor beeld dat door deze begeleide installatie wordt gegenereerd, volgt het patroon. De eerste keer dat u het voor beeld uitvoert, ziet u dat deze in actie is.
  * Omdat geen enkele gebruiker de toepassing heeft gebruikt `accounts.FirstOrDefault()` , bevat een null-waarde en `MsalUiRequiredException` er wordt een uitzonde ring gegenereerd.
  * De code in het voor beeld behandelt vervolgens de uitzonde `AcquireTokenInteractive`ring door aan te roepen. Met deze aanroep wordt de gebruiker gevraagd zich aan te melden.

* In plaats daarvan wordt een visuele indicatie gegeven aan gebruikers die een interactieve aanmelding vereist. Vervolgens kunnen ze het juiste tijdstip selecteren om zich aan te melden. Of de toepassing kan het `AcquireTokenSilent` later opnieuw proberen. Dit patroon wordt vaak gebruikt wanneer gebruikers de functionaliteit van andere toepassingen zonder onderbreking kunnen gebruiken. Een voor beeld hiervan is wanneer offline-inhoud beschikbaar is in de toepassing. In dit geval kunnen gebruikers bepalen wanneer ze zich willen aanmelden om toegang te krijgen tot de beveiligde resource of de verouderde informatie te vernieuwen. Of de toepassing kan er ook voor kiezen `AcquireTokenSilent` om het opnieuw te proberen wanneer het netwerk wordt hersteld nadat het tijdelijk niet beschikbaar was.

## <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Microsoft Graph-API aanroepen met behulp van het token dat u zojuist hebt verkregen

* Voeg de volgende nieuwe methode toe aan **MainPage.xaml.cs**. Deze methode wordt gebruikt om een `GET` aanvraag te doen tegen Graph API met behulp van een `Authorization` header:

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

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Meer informatie over het maken van een REST-aanroep voor een beveiligde API

In deze voorbeeld toepassing wordt de `GetHttpContentWithToken` methode gebruikt om een http- `GET` aanvraag te doen tegen een beveiligde bron waarvoor een token is vereist. Vervolgens retourneert de methode de inhoud naar de aanroeper. Met deze methode wordt het verkregen token toegevoegd aan de **http-autorisatie-** header. Voor dit voor beeld is de resource het Microsoft Graph API **me** -eind punt, waarin de profiel gegevens van de gebruiker worden weer gegeven.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Een methode voor het afmelden van de gebruiker toevoegen

* Als u de gebruiker wilt afmelden, voegt u de volgende methode toe aan **MainPage.xaml.cs**:

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
> MSAL.net maakt gebruik van asynchrone methoden voor het verkrijgen van tokens of het manipuleren van accounts, en daarom moet u ervoor zorgen dat u in de UI-thread acties `Dispatcher.RunAsync`voor de gebruikers interface uitvoert, dus de en de voorzorgsmaatregelen die moeten worden aangeroepen`ConfigureAwait(false)`

### <a name="more-information-on-sign-out"></a>Meer informatie over afmelden

De `SignOutButton_Click` -methode verwijdert de gebruiker uit de MSAL-gebruikers cache. Deze methode vertelt MSAL de huidige gebruiker te verg eten. Een toekomstige aanvraag voor het verkrijgen van een token slaagt alleen als deze is gemaakt om interactief te worden.
De toepassing in dit voor beeld ondersteunt één gebruiker. MSAL ondersteunt echter scenario's waarbij meerdere accounts tegelijkertijd kunnen worden ondertekend. Een voor beeld is een e-mail toepassing waarbij een gebruiker meerdere accounts heeft.

## <a name="display-basic-token-information"></a>Elementaire token gegevens weer geven

* Voeg de volgende methode toe aan **MainPage.xaml.cs** om basis informatie over het token weer te geven:

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

ID-tokens die zijn verkregen via **OpenID Connect Connect** bevatten ook een kleine subset van informatie die betrekking heeft op de gebruiker. `DisplayBasicTokenInfo`geeft basis informatie weer die in het token is opgenomen. Voor beelden zijn de weergave naam en-ID van de gebruiker, de verval datum van het token en de teken reeks die het toegangs token zelf vertegenwoordigt. Als u de knop **Call Microsoft Graph API** meerdere keren selecteert, ziet u dat hetzelfde token opnieuw is gebruikt voor volgende aanvragen. U kunt ook zien hoe lang de verloop datum moet worden verlengd wanneer MSAL beslist dat het token wordt vernieuwd.

## <a name="register-your-application"></a>Uw toepassing registreren

U moet uw toepassing nu registreren in de micro soft Application Registration-portal:

1. Meld u bij de [Azure-portal](https://portal.azure.com) aan met een werk- of schoolaccount of een persoonlijk Microsoft-account.
1. Als uw account zich in meer dan één Azure AD-Tenant bevindt, selecteert u `Directory + Subscription` in de rechter bovenhoek van het menu boven aan de pagina en schakelt u uw portal sessie over naar de gewenste Azure AD-Tenant.
1. Navigeer naar de pagina micro soft-identiteits platform voor ontwikkel aars [app-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) .
1. Selecteer **nieuwe registratie**.
   - Voer in de sectie **Naam** een beschrijvende toepassingsnaam. Deze wordt zichtbaar voor gebruikers van de app. Bijvoorbeeld: `UWP-App-calling-MSGraph`.
   - Selecteer in de sectie **ondersteunde account typen** de optie **accounts in een organisatorische map en persoonlijke micro soft-accounts (bijvoorbeeld Skype, Xbox, Outlook.com)** .
   - Selecteer **Registreren** om de toepassing te maken.
1. Zoek op de pagina app- **overzicht** de waarde van de **toepassing (client)** en noteer deze voor later. Ga terug naar Visual Studio, open **MainPage.xaml.cs**en vervang de waarde van ClientId door de id van de toepassing die u zojuist hebt geregistreerd:
1. Selecteer in de lijst met pagina’s voor de app de optie **Verificatie**.
   1. In de sectie omleidings- **uri's** in de lijst omleidings-uri's:
   1. Selecteer in de kolom **type** de optie **open bare client (mobiele & bureau blad)** .
   1. Voer `urn:ietf:wg:oauth:2.0:oob` in de kolom omleidings- **URI** in.
1. Selecteer **Opslaan**.
1. Selecteer in de lijst met pagina's voor de app **API-machtigingen**
   - Klik op de knop **een machtiging toevoegen** en vervolgens op
   - Controleer of het tabblad **van de micro soft API** is geselecteerd
   - Klik in de sectie *veelgebruikte micro soft-api's* op **Microsoft Graph**
   - Controleer in de sectie **gedelegeerde machtigingen** of de juiste machtigingen zijn ingeschakeld: **Gebruiker. Read**. Gebruik het zoekvak als dat nodig is.
   - Selecteer de knop **machtigingen toevoegen**

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Geïntegreerde verificatie inschakelen voor federatieve domeinen (optioneel)

Als u geïntegreerde Windows-verificatie wilt inschakelen wanneer deze wordt gebruikt met een federatief Azure AD-domein, moet het toepassings manifest extra mogelijkheden inschakelen:

1. Dubbel klik op **package. appxmanifest**.
2. Selecteer het tabblad **mogelijkheden** en controleer of de volgende instellingen zijn ingeschakeld:

    - Ondernemings verificatie
    - Particuliere netwerken (client & Server)
    - Gedeelde gebruikers certificaten

> [!IMPORTANT]
> [Geïntegreerde Windows-verificatie](https://aka.ms/msal-net-iwa) is niet standaard geconfigureerd voor dit voor beeld. Toepassingen die de mogelijkheden voor *ondernemings verificatie* of *gedeelde gebruikers certificaten* aanvragen, vereisen een hoger verificatie niveau door de Windows Store. Het is ook niet alle ontwikkel aars nodig om het hogere verificatie niveau uit te voeren. Schakel deze instelling alleen in als u geïntegreerde Windows-verificatie met een federatief Azure AD-domein nodig hebt.

## <a name="test-your-code"></a>Uw code testen

Als u uw toepassing wilt testen, selecteert u F5 om uw project uit te voeren in Visual Studio. Het hoofd venster wordt weer gegeven:

![Gebruikers interface van de toepassing](./media/tutorial-v2-windows-uwp/testapp-ui.png)

Wanneer u klaar bent om te testen, selecteert u **Microsoft Graph-API aanroepen**. Gebruik vervolgens een Azure AD-organisatie account of een Microsoft-account, zoals live.com of outlook.com, om u aan te melden. Als dit de eerste keer is, ziet u een venster waarin de gebruiker wordt gevraagd zich aan te melden:

![Aanmeldings pagina](./media/tutorial-v2-windows-uwp/sign-in-page.png)

### <a name="consent"></a>Vergunning

De eerste keer dat u zich bij uw toepassing aanmeldt, wordt er een venster voor toestemming weer gegeven dat vergelijkbaar is met het volgende. Selecteer **Ja** om expliciet toestemming te geven voor toegang:

![Venster toegangs bevestiging](./media/tutorial-v2-windows-uwp/consentscreen.png)

### <a name="expected-results"></a>Verwachte resultaten

U ziet de gebruikers profiel gegevens die worden geretourneerd door de API-aanroep van Microsoft Graph op het scherm met de **API-aanroep resultaten** :

![Scherm met resultaten van API-aanroep](./media/tutorial-v2-windows-uwp/uwp-results-screen.PNG)

U ziet ook algemene informatie over het token dat u `AcquireTokenInteractive` hebt `AcquireTokenSilent` verkregen via of in het vak **token info** :

|Eigenschap  |Indeling  |Description |
|---------|---------|---------|
|**Gebruikersnaam** |<span>user@domain.com</span> |De gebruikers naam waarmee de gebruiker wordt geïdentificeerd.|
|**Token verloopt** |Datetime |Het tijdstip waarop het token verloopt. MSAL breidt de verloop datum uit door de token indien nodig te vernieuwen.|

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereiken en gedelegeerde machtigingen

Voor de Microsoft Graph-API is het bereik *User. Read* vereist om het profiel van een gebruiker te lezen. Deze scope wordt standaard automatisch toegevoegd in elke toepassing die is geregistreerd in de portal voor toepassings registratie. Andere Api's voor Microsoft Graph en aangepaste Api's voor uw back-endserver hebben mogelijk extra scopes nodig. Microsoft Graph-API vereist het bereik *Calendars. Read* om de agenda's van de gebruiker weer te geven.

Als u toegang wilt krijgen tot de agenda's van de gebruiker in de context van een toepassing, voegt u de *agenda's toe.* gedelegeerde machtigingen lezen voor de registratie gegevens van de toepassing. Voeg de *agenda's* vervolgens toe aan de `acquireTokenSilent` aanroep.

> [!NOTE]
> Gebruikers kunnen worden gevraagd om aanvullende toestemmingen wanneer u het aantal bereiken verhoogt.

## <a name="known-issues"></a>Bekende problemen

### <a name="issue-1"></a>Probleem 1

Er wordt een van de volgende fout berichten weer gegeven wanneer u zich aanmeldt bij uw toepassing in een federatief Azure AD-domein:

* Er is geen geldig client certificaat gevonden in de aanvraag.
* Er zijn geen geldige certificaten gevonden in het certificaat archief van de gebruiker.
* Probeer opnieuw een andere verificatie methode te kiezen.

**Wordt** De mogelijkheden voor ondernemings en certificaten zijn niet ingeschakeld.

**Oplossen** Volg de stappen in [geïntegreerde verificatie voor federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional).

### <a name="issue-2"></a>Probleem 2

U schakelt [geïntegreerde verificatie in voor federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional) en probeert Windows hello te gebruiken op een Windows 10-computer om u aan te melden bij een omgeving waarvoor multi-factor Authentication is geconfigureerd. De lijst met certificaten wordt weer gegeven. Als u echter kiest voor het gebruik van uw pincode, wordt het venster vastmaken nooit weer gegeven.

**Wordt** Dit probleem is een bekende beperking van de Web authentication Broker in UWP-toepassingen die worden uitgevoerd op Windows 10 Desktop. Het werkt prima op Windows 10 Mobile.

**Enkele** Selecteer **Aanmelden met andere opties**. Selecteer vervolgens **Aanmelden met een gebruikers naam en wacht woord**. Selecteer **uw wacht woord opgeven**. Ga vervolgens door met het verificatie proces voor de telefoon.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Help ons het micro soft Identity-platform te verbeteren. Vertel ons wat u denkt door een korte enquête met twee vragen te volt ooien.

> [!div class="nextstepaction"]
> [Micro soft Identity platform-enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)
