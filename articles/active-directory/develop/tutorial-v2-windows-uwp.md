---
title: Azure AD v2 UWP aan de slag | Microsoft Docs
description: Hoe Universal Windows Platform-toepassingen (UWP) een API die tokens voor toegang door de Azure Active Directory v2-eindpunt vereist kunnen aanroepen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/20/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 4afd4ce5b8a0ab4c076ebc3c587605dfe1204b8a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966381"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Microsoft Graph API aanroepen vanuit een Universal Windows Platform-toepassing (XAML)


> [!div renderon="docs"]
> [!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Deze handleiding wordt uitgelegd hoe een systeemeigen Universal Windows Platform (UWP)-toepassing kan aanvragen van een toegangstoken en roep vervolgens Microsoft Graph API. De handleiding geldt ook voor andere API's waarvoor toegangstokens van het Azure Active Directory v2-eindpunt.

Aan het einde van deze handleiding, worden in uw toepassing een beveiligde API aanroept met behulp van persoonlijke accounts. Voorbeelden zijn outlook.com, live.com en anderen. Werk-en schoolaccounts aanroepen uw toepassing ook van een bedrijf of organisatie die Azure Active Directory heeft.

>[!NOTE]
> Deze handleiding moet Visual Studio 2017 met Universal Windows Platform-ontwikkeling die zijn geïnstalleerd. Zie [instellen](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) voor instructies voor het downloaden en configureren van Visual Studio voor het ontwikkelen van Universal Windows Platform-apps.

### <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding op grafiek](./media/tutorial-v2-windows-uwp/uwp-intro.png)

Deze handleiding UWP met een voorbeeldtoepassing die query's van Microsoft Graph API of een Web-API die tokens van het Azure Active Directory v2-eindpunt accepteert gemaakt. Voor dit scenario wordt een token toegevoegd aan de HTTP-aanvragen via de autorisatie-header. Microsoft Authentication Library (MSAL) wordt gebruikt voor token-aankopen en vernieuwingen.

### <a name="nuget-packages"></a>NuGet-pakketten

Deze handleiding maakt gebruik van de volgende NuGet-pakketten:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library|


## <a name="set-up-your-project"></a>Instellen van uw project

In deze sectie biedt stapsgewijze instructies voor het integreren van een Windows Desktop .NET-toepassing (XAML) *aanmelden met Microsoft*. Vervolgens kan deze Web-API's waarvoor een token, zoals Microsoft Graph API opvragen.

Deze handleiding wordt gemaakt van een toepassing die wordt weergegeven een knop die query's Graph API, een knop Afmelden en tekstvakken die de resultaten van de aanroepen weergeven.

>[!NOTE]
> Wilt u dit voorbeeld van Visual Studio-project in plaats daarvan downloaden? [Een project hebt gedownload](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip) en gaat u naar de [toepassingsregistratie](#register-your-application "toepassing registratiestap") stap voor het configureren van de voorbeeldcode voordat deze wordt uitgevoerd.


### <a name="create-your-application"></a>Uw toepassing maken
1. Selecteer in Visual Studio, **bestand** > **nieuw** > **Project**.
2. Onder **sjablonen**, selecteer **Visual C#**.
3. Selecteer **Lege app (Universal Windows)**.
4. De naam van de app, en selecteer **OK**.
5. Als u hierom wordt gevraagd, selecteert u een willekeurige versie van **doel** en **Minimum** versies en selecteer **OK**.

    >![Minimum- en doel-versies](./media/tutorial-v2-windows-uwp/vs-minimum-target.png)

## <a name="add-microsoft-authentication-library-to-your-project"></a>Microsoft Authentication Library toevoegen aan uw project
1. Selecteer in Visual Studio, **extra** > **NuGet Package Manager** > **Package Manager Console**.
2. Kopieer en plak de volgende opdracht in de **Package Manager Console** venster:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre -Version 1.1.4-preview0002
    ```

> [!NOTE]
> Met deze opdracht installeert [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet). MSAL verkrijgt, in de cache opgeslagen en wordt vernieuwd gebruikerstokens die toegang API's die zijn beveiligd door Azure Active Directory v2 tot.

> [!NOTE]
> In deze zelfstudie niet gebruikt, maar de meest recente versie van MSAL.NET, maar er wordt gewerkt aan het bijwerken.

## <a name="initialize-msal"></a>MSAL initialiseren
Deze stap maakt u een klasse voor het afhandelen van interactie met MSAL, zoals het verwerken van tokens.

1. Open de **App.xaml.cs** -bestand en voeg de referentie voor MSAL toe aan de klasse:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. De volgende twee regels toevoegen aan de klasse van de app (binnen <code>sealed partial class App : Application</code> blokkeren):

    ```csharp
    // Below is the clientId of your app registration. 
    // You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";
    
    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
    ```

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
        // Set the API Endpoint to Graph 'me' endpoint
        string graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";
    
        // Set the scope for API call to user.read
        string[] scopes = new string[] { "user.read" };
    
        public MainPage()
        {
            this.InitializeComponent();
        }
    
        /// <summary>
        /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
        /// </summary>
        private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
        {
            AuthenticationResult authResult = null;
            ResultText.Text = string.Empty;
            TokenInfoText.Text = string.Empty;
    
            try
            {
                authResult = await App.PublicClientApp.AcquireTokenSilentAsync(scopes, App.PublicClientApp.Users.FirstOrDefault());
            }
            catch (MsalUiRequiredException ex)
            {
                // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
                System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");
    
                try
                {
                    authResult = await App.PublicClientApp.AcquireTokenAsync(scopes);
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
                ResultText.Text = await GetHttpContentWithToken(graphAPIEndpoint, authResult.AccessToken);
                DisplayBasicTokenInfo(authResult);
                this.SignOutButton.Visibility = Visibility.Visible;
            }
        }
    }
    ```

### <a name="more-information"></a>Meer informatie
#### <a name="get-a-user-token-interactively"></a>Een gebruiker interactief token ophalen
Een aanroep naar de `AcquireTokenAsync` methode resulteert in een venster waarin gebruikers zich aanmelden. Toepassingen vereisen meestal gebruikers zich interactief aanmelden de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron. Ze kunnen ook moet zich aanmelden als een bewerking op de achtergrond is een token ophalen is mislukt. Een voorbeeld daarvan is wanneer het wachtwoord van een gebruiker is verlopen.

#### <a name="get-a-user-token-silently"></a>Een gebruiker op de achtergrond token ophalen
De `AcquireTokenSilentAsync` methode wordt gebruikt voor token-aankopen en vernieuwingen zonder tussenkomst van de gebruiker. Na `AcquireTokenAsync` voor de eerste keer wordt uitgevoerd en de gebruiker wordt gevraagd om referenties, het `AcquireTokenSilentAsync` methode moet worden gebruikt voor het aanvragen van tokens voor volgende aanroepen omdat het verkrijgen van tokens op de achtergrond. MSAL wordt afgehandeld tokencache en verlenging.

Uiteindelijk de `AcquireTokenSilentAsync` methode mislukt. Oorzaak van de fout kunnen zijn dat gebruikers hebt zich afgemeld of het wachtwoord op een ander apparaat gewijzigd. Wanneer MSAL detecteert dat het probleem kan worden omgezet door verlangen dat ze een interactieve actie, wordt deze gebeurtenis wordt gestart een `MsalUiRequiredException` uitzondering. Uw toepassing kan verwerken deze uitzondering op twee manieren:

* Er kan een aanroep tegen `AcquireTokenAsync` onmiddellijk. Deze aanroep leidt de gebruiker zich aanmeldt. Normaal gesproken dit patroon in online toepassingen wordt gebruikt wanneer er geen beschikbare offline inhoud voor de gebruiker is. Het voorbeeld dat is gegenereerd door deze Begeleide instelling volgt het patroon. U wordt het in actie de eerste keer dat u het voorbeeld uitvoert. 
    * Omdat er geen gebruiker heeft de toepassing gebruikt `PublicClientApp.Users.FirstOrDefault()` bevat een null-waarde en een `MsalUiRequiredException` uitzondering is opgetreden.
    * De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `AcquireTokenAsync`. Deze aanroep leidt de gebruiker zich aanmeldt.

* Of in plaats daarvan geeft deze een visuele indicatie voor gebruikers die een interactieve aanmelding vereist is. Vervolgens kunnen ze het juiste moment aan te melden bij selecteren. Of de toepassing opnieuw kunt proberen `AcquireTokenSilentAsync` later opnieuw. Dit patroon wordt vaak gebruikt wanneer gebruikers de functionaliteit van andere toepassing zonder onderbreking kunnen gebruiken. Een voorbeeld is wanneer u offline inhoud is beschikbaar in de toepassing. Gebruikers kunnen in dit geval beslissen wanneer ze aanmelden willen bij de toegang tot de beveiligde bron of de verouderde gegevens vernieuwen. Of anders de toepassing kan beslissen om opnieuw te proberen `AcquireTokenSilentAsync` wanneer het netwerk is hersteld nadat deze tijdelijk niet beschikbaar is.

## <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Microsoft Graph API aanroepen met behulp van het token dat u zojuist hebt verkregen.

* Voeg de volgende nieuwe methode om te **MainPage.xaml.cs**. Deze methode wordt gebruikt om een `GET` aanvraag indient voor Graph API met behulp van een [autoriseren]-header:

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

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Meer informatie over het maken van een REST-aanroep op basis van een beveiligde API

In deze voorbeeldtoepassing de `GetHttpContentWithToken` methode wordt gebruikt om een HTTP `GET` aanvraag indient voor een beveiligde resource waarvoor een token. De methode retourneert vervolgens de inhoud voor de oproepende functie. Met deze methode wordt het verkregen token in de **HTTP-autorisatie** header. Voor dit voorbeeld is de bron de Microsoft Graph API **me** eindpunt, waarin de profielgegevens van de gebruiker worden weergegeven.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Toevoegen van een methode voor de gebruiker afmelden

* Als u wilt de gebruiker afmelden, toevoegen de volgende methode om te **MainPage.xaml.cs**:

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

### <a name="more-information-on-sign-out"></a>Meer informatie over het afmelden

De `SignOutButton_Click` methode wordt de gebruiker verwijderd uit de cache van de gebruiker MSAL. Deze methode weet MSAL vergeten van de huidige gebruiker. En vervolgens een toekomstige aanvraag om een token te verkrijgen slaagt alleen als deze is gemaakt interactief zijn.
De toepassing in dit voorbeeld biedt ondersteuning voor één gebruiker. Maar MSAL biedt ondersteuning voor scenario's waarbij meer dan één account kan worden aangemeld op hetzelfde moment. Een voorbeeld is een e-mailtoepassing waar een gebruiker heeft diverse accounts.

## <a name="display-basic-token-information"></a>Token basisinformatie weergeven

* Voeg de volgende methode om te **MainPage.xaml.cs** om algemene informatie over het token weer te geven:

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

### <a name="more-information"></a>Meer informatie

ID-tokens die zijn verkregen **OpenID Connect** bevatten ook een kleine subset van de informatie die relevant zijn voor de gebruiker. `DisplayBasicTokenInfo` algemene informatie die is opgenomen in het token wordt weergegeven. Voorbeelden zijn de weergavenaam van de gebruiker en -ID, de vervaldatum van het token en de tekenreeks met het toegangstoken zelf. Als u selecteert de **Microsoft Graph-API aanroepen** meerdere keren knop, ziet u dat hetzelfde token opnieuw werd gebruikt voor volgende aanvragen. U ziet ook de datum van afloop voor uitgebreide wanneer MSAL besluit is het tijd om het token te verlengen.

## <a name="register-your-application"></a>Uw toepassing registreren

Nu moet u uw toepassing registreren in de Portal voor Appregistratie Microsoft:
1. Ga naar de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/portal/register-app) om een toepassing te registreren.
2. Voer een naam voor uw toepassing.
3. Zorg ervoor dat de optie voor **begeleide installatie** is *niet geselecteerd*.
4. Selecteer **Platforms toevoegen**, selecteer **systeemeigen toepassing**, en selecteer vervolgens **opslaan**.
5. Kopieer de GUID in **toepassings-ID**, gaat u terug naar Visual Studio, open **App.xaml.cs**, en vervang `your_client_id_here` met de toepassings-ID die u zojuist hebt geregistreerd:

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Geïntegreerde verificatie inschakelen in federatieve domeinen (optioneel)

Om in te schakelen geïntegreerde Windows-verificatie als deze wordt gebruikt met een federatieve Azure Active Directory-domein, moet het toepassingsmanifest aanvullende mogelijkheden inschakelen:

1. Dubbelklik op **Package.appxmanifest**.
2. Selecteer de **mogelijkheden** tabblad en zorg ervoor dat de volgende instellingen zijn ingeschakeld:

    - Enterprise-verificatie
    - Particuliere netwerken (Client en Server)
    - Gedeelde gebruikerscertificaten

3. Open **App.xaml.cs** en voeg de volgende regel in de app-constructor:

    ```csharp
    App.PublicClientApp.UseCorporateNetwork = true;
    ```

> [!IMPORTANT]
> Geïntegreerde Windows-verificatie is niet geconfigureerd voor dit voorbeeld standaard. Toepassingen die aanvragen *Ondernemingsverificatie* of *gedeelde gebruikerscertificaten* mogelijkheden vereisen een hoger niveau van controle door de Windows Store. Niet alle ontwikkelaars willen ook het hogere niveau van controle uitvoeren. Schakel deze instelling alleen als u geïntegreerde Windows-verificatie met een federatieve Azure Active Directory-domein.


## <a name="test-your-code"></a>Testen van uw code

Als u wilt testen van uw toepassing, selecteer F5 om uit te voeren van uw project in Visual Studio. Het hoofdvenster wordt weergegeven:

![De gebruikersinterface van de toepassing](./media/tutorial-v2-windows-uwp/testapp-ui.png)

Wanneer u klaar om te testen bent, selecteert u **Microsoft Graph-API aanroepen**. Gebruik vervolgens een organisatieaccount voor Microsoft Azure Active Directory of een Microsoft-account, zoals live.com of outlook.com, aan te melden bij. Als de eerste keer is, ziet u een venster waarin de gebruiker zich aanmeldt:

![Aanmeldingspagina opgeven](./media/tutorial-v2-windows-uwp/sign-in-page.png)

### <a name="consent"></a>Akkoord
De eerste keer dat u zich aanmeldt bij uw toepassing, krijgt u een vergelijkbaar met de volgende instemmingsscherm. Selecteer **Ja** naar expliciet toestemming voor toegang tot:

![Het instemmingsscherm toegang](./media/tutorial-v2-windows-uwp/consentscreen.png)
### <a name="expected-results"></a>Verwachte resultaten
U ziet informatie uit gebruikersprofielen door de Microsoft Graph API-aanroep is geretourneerd op de **API Gespreksresultaten** scherm:

![API aanroepen resultaten scherm](./media/tutorial-v2-windows-uwp/uwp-results-screen.PNG)

U ziet ook basisinformatie over het token dat is verkregen `AcquireTokenAsync` of `AcquireTokenSilentAsync` in de **Token Info** vak:

|Eigenschap  |Indeling  |Beschrijving |
|---------|---------|---------|
|**Naam** |Volledige naam van gebruiker|En achternaam van eerst de gebruiker.|
|**Gebruikersnaam** |<span>user@domain.com</span> |De gebruikersnaam waarmee de gebruiker.|
|**Token is verlopen** |DateTime |De tijd wanneer het token is verlopen. MSAL vervolg op de vervaldatum vernieuwen van het token indien nodig.|
|**Toegangstoken** |Reeks |De token tekenreeks die wordt verzonden naar HTTP-aanvragen waarvoor een *autorisatie-header*.|

#### <a name="see-whats-in-the-access-token-optional"></a>Ontdek wat er in het toegangstoken (optioneel)
De waarde in (optioneel) kopiëren **toegangstoken** en plak deze in https://jwt.ms dit decoderen en bekijkt u de lijst met claims.

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereiken en gedelegeerde machtigingen

Microsoft Graph API vereist de *user.read* scope om te lezen van het gebruikersprofiel. Dit bereik wordt automatisch toegevoegd in elke toepassing die geregistreerd in de Portal voor Appregistratie standaard. Andere API's voor Microsoft Graph en de aangepaste API's voor uw back-endserver, mogelijk extra scopes. Microsoft Graph API vereist de *Calendars.Read* bereik om agenda's van de gebruiker weer te geven.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing moet toevoegen de *Calendars.Read* overgedragen machtigingen voor de registratiegegevens van de toepassing. Voeg de *Calendars.Read* bereik instellen op de `acquireTokenSilent` aanroepen. 

> [!NOTE]
> Gebruikers mogelijk gevraagd extra toestemmingen als u het aantal bereiken verhogen.

## <a name="known-issues"></a>Bekende problemen

### <a name="issue-1"></a>Probleem 1
U ontvangt een van de volgende foutberichten weergegeven wanneer u zich aanmelden op uw toepassing in een federatieve Azure Active Directory-domein:
 - Er is geen geldig clientcertificaat gevonden in de aanvraag.
 - Er is geen geldige certificaten gevonden in het certificaatarchief van de gebruiker.
 - Probeer opnieuw een andere verificatiemethode kiezen.

**Oorzaak:** mogelijkheden voor ondernemingen en het certificaat is niet ingeschakeld.

**Oplossing:** Volg de stappen in [geïntegreerde verificatie op federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional).

### <a name="issue-2"></a>Probleem 2
U hebt ingeschakeld [geïntegreerde verificatie op federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional) en probeer het Windows Hello gebruiken op een Windows 10-computer aan te melden voor een omgeving met meervoudige verificatie is geconfigureerd. De lijst van certificaten wordt weergegeven. Echter, als u ervoor kiest uw PINCODE moet gebruiken, het venster van de PINCODE wordt nooit weergegeven.

**Oorzaak:** dit probleem is een bekende beperking van de webauthenticatiebroker in UWP-toepassingen die worden uitgevoerd op Windows 10 desktop. Het werkt prima op Windows 10 Mobile.

**Tijdelijke oplossing:** Selecteer **Meld u aan met andere opties**. Selecteer vervolgens **Meld u aan met een gebruikersnaam en wachtwoord**. Selecteer **geeft u uw wachtwoord**. Ga via de telefoon verificatieproces uit.
