---
title: Azure AD v2 UWP aan de slag | Microsoft Docs
description: Toegangstokens hoe Universal Windows Platform-toepassingen (UWP) een API kunnen aanroepen die worden vereist door het Azure Active Directory-v2-eindpunt
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/20/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: c2d5681e30651aac7a09a8ead923015e9a892d42
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34796611"
---
# <a name="call-microsoft-graph-api-from-a-universal-windows-platform-application-xaml"></a>Microsoft Graph API aanroepen vanuit een Universal Windows Platform-toepassing (XAML)

Deze handleiding wordt uitgelegd hoe een systeemeigen Universal Windows Platform (UWP)-toepassing kan aanvragen van een toegangstoken en vervolgens Microsoft Graph API aanroepen. De handleiding geldt ook voor andere API's waarvoor toegangstokens van het Azure Active Directory-v2-eindpunt.

Aan het einde van deze handleiding wordt in uw toepassing een beveiligde API aanroept met behulp van persoonlijke accounts. Voorbeelden zijn outlook.com, live.com en anderen. Werk-en schoolaccounts aanroepen uw toepassing ook van een bedrijf of organisatie die Azure Active Directory heeft.

>[!NOTE]
> Deze handleiding vereist 2017 voor Visual Studio met de ontwikkeling van universele Windows-Platform geïnstalleerd. Zie [ophalen instellen](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) voor instructies voor het downloaden en configureren van Visual Studio voor het ontwikkelen van apps voor universele Windows-Platform.

### <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding op de grafiek](media/active-directory-mobileanddesktopapp-windowsuniversalplatform-introduction/uwp-intro.png)

Deze handleiding wordt een voorbeeld UWP-toepassing Microsoft Graph API query of een Web-API die tokens van het Azure Active Directory-v2-eindpunt accepteert gemaakt. Voor dit scenario wordt een token toegevoegd om HTTP-aanvragen via de autorisatie-header. Microsoft Authentication Library (MSAL) verwerkt token acquisities van organisaties en vernieuwingen.

### <a name="nuget-packages"></a>NuGet-pakketten

Deze handleiding bevat de volgende NuGet-pakketten:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft-Verificatiebibliotheek|


## <a name="set-up-your-project"></a>Instellen van uw project

Deze sectie bevat stapsgewijze instructies voor het integreren van een Windows Desktop .NET-toepassing (XAML) met *aanmelden met Microsoft*. Klik op te vragen Web-API's waarvoor een token, zoals Microsoft Graph API.

Deze handleiding maakt een toepassing die wordt weergegeven een knop die query's Graph API, een knop Afmelden en tekstvakken zijn die de resultaten van de oproepen weergeven.

>[!NOTE]
> Wilt u dit voorbeeld Visual Studio-project in plaats daarvan downloaden? [Downloaden van een project](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip) en doorgaan met de [toepassingsregistratie](#register-your-application "toepassing registratiestap") stap voor het configureren van het codevoorbeeld voordat deze wordt uitgevoerd.


### <a name="create-your-application"></a>Uw toepassing maken
1. Selecteer in Visual Studio **bestand** > **nieuw** > **Project**.
2. Onder **sjablonen**, selecteer **Visual C#**.
3. Selecteer **Lege app (Universal Windows)**.
4. De naam van de app, en selecteer **OK**.
5. Als u wordt gevraagd, selecteert u een willekeurige versie voor **doel** en **Minimum** versies en selecteer **OK**.

    >![Minimum- en doel-versies](media/active-directory-uwp-v2.md/vs-minimum-target.png)

## <a name="add-microsoft-authentication-library-to-your-project"></a>Microsoft-Verificatiebibliotheek toevoegen aan uw project
1. Selecteer in Visual Studio **extra** > **NuGet Package Manager** > **Package Manager Console**.
2. Kopieer en plak de volgende opdracht in de **Package Manager Console** venster:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

> [!NOTE]
> Deze opdracht wordt geïnstalleerd [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet). MSAL verkrijgt, in de cache opslaat en gebruikerstokens die toegang API's die zijn beveiligd door Azure Active Directory-v2 tot wordt vernieuwd.

## <a name="initialize-msal"></a>MSAL initialiseren
Deze stap maakt u een klasse voor het afhandelen van interactie met MSAL, zoals het verwerken van tokens.

1. Open de **App.xaml.cs** -bestand en voeg de referentie voor MSAL toe aan de klasse:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. De volgende twee regels toevoegen aan de klasse van de app (binnen <code>sealed partial class App : Application</code> blok):

    ```csharp
    // Below is the clientId of your app registration. 
    // You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";
    
    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
    ```

## <a name="create-your-applications-ui"></a>De gebruikersinterface van uw toepassing maken

Een **MainPage.xaml** bestand wordt automatisch gemaakt als onderdeel van uw project-sjabloon. Dit bestand openen en volg de instructies:

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
    
## <a name="use-msal-to-get-a-token-for-microsoft-graph-api"></a>Gebruik MSAL voor een token voor Microsoft Graph API

Deze sectie wordt beschreven hoe MSAL gebruiken voor een token voor Microsoft Graph API.

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
Een aanroep van de `AcquireTokenAsync` methode resulteert in een venster waarin gebruikers zich aanmelden. Toepassingen vereisen meestal gebruikers zich aanmelden in interactief de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron. Ze wellicht ook aan te melden bij een achtergrond te verkrijgen van een token mislukt. Een voorbeeld is wanneer een gebruiker het wachtwoord is verlopen.

#### <a name="get-a-user-token-silently"></a>Een gebruiker achtergrond token ophalen
De `AcquireTokenSilentAsync` methode token acquisities van organisaties en vernieuwingen zonder tussenkomst van de gebruiker worden verwerkt. Na `AcquireTokenAsync` voor het eerst wordt uitgevoerd en de gebruiker wordt gevraagd om referenties, het `AcquireTokenSilentAsync` methode moet worden gebruikt voor het aanvragen van tokens voor volgende aanroepen omdat het achtergrond tokens verkrijgen. MSAL wordt afgehandeld tokencache en verlenging.

Uiteindelijk de `AcquireTokenSilentAsync` methode mislukt. Oorzaak van de fout is mogelijk dat gebruikers hebt zich afgemeld of het wachtwoord op een ander apparaat gewijzigd. Wanneer MSAL detecteert dat het probleem doordat een interactieve actie kan worden omgezet, wordt deze gebeurtenis wordt gestart een `MsalUiRequiredException` uitzondering. Uw toepassing kan verwerken van deze uitzondering op twee manieren:

* Er kan een aanroep van tegen `AcquireTokenAsync` onmiddellijk. Deze aanroep resulteert in vraagt de gebruiker aan te melden. Normaal gesproken dit patroon wordt gebruikt in toepassingen met online wanneer er geen beschikbare offline inhoud voor de gebruiker is. Het voorbeeld dat is gegenereerd door deze begeleide setup heeft het patroon. U ziet deze in actie de eerste keer dat u het voorbeeld uitvoert. 
    * Omdat er geen gebruiker heeft de toepassing gebruikt `PublicClientApp.Users.FirstOrDefault()` bevat een null-waarde en een `MsalUiRequiredException` uitzondering gegenereerd.
    * De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `AcquireTokenAsync`. Deze aanroep resulteert in vraagt de gebruiker aan te melden.

* Of in plaats daarvan wordt een visuele indicatie biedt aan gebruikers dat een interactieve aanmelding vereist is. Vervolgens kunnen ze het juiste moment aan te melden selecteren. Of de toepassing opnieuw kunt `AcquireTokenSilentAsync` later. Dit patroon wordt vaak gebruikt als gebruikers andere toepassingsfunctionaliteit van de zonder onderbreking kunnen gebruiken. Een voorbeeld is wanneer u offline inhoud is beschikbaar in de toepassing. In dit geval bepalen gebruikers wanneer ze aanmelden willen bij de toegang tot de beveiligde bron of de verouderde gegevens te vernieuwen. Of anders de toepassing opnieuw kunt bepalen `AcquireTokenSilentAsync` wanneer het netwerk wordt teruggezet nadat deze tijdelijk niet beschikbaar is.

## <a name="call-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Microsoft Graph API aanroepen via het token dat u zojuist hebt verkregen

* Voeg de volgende nieuwe methode voor **MainPage.xaml.cs**. Deze methode wordt gebruikt om een `GET` aanvraag tegen Graph API met behulp van een [autoriseren]-header:

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

In deze voorbeeldtoepassing de `GetHttpContentWithToken` methode wordt gebruikt voor het maken van een HTTP `GET` -aanvraag in voor een beveiligde bron die is een token vereist. De methode retourneert vervolgens de inhoud naar de aanroeper. Met deze methode wordt het token verkregen in de **HTTP Authorization** header. Voor dit voorbeeld wordt de resource is de Microsoft Graph API **mij** eindpunt waarin informatie over het profiel van de gebruiker.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Een methode voor het ondertekenen van de gebruiker toevoegen

* Om de gebruiker afmelden, voegt u de volgende methode voor **MainPage.xaml.cs**:

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

De `SignOutButton_Click` methode wordt de gebruiker verwijderd uit de cache van de gebruiker MSAL. Deze methode vertelt effectief MSAL vergeten van de huidige gebruiker. Vervolgens slaagt een toekomstige aanvraag voor een token verkrijgen alleen als aan interactief heeft verstrekt.
De toepassing in dit voorbeeld ondersteunt een enkele gebruiker. Maar MSAL ondersteunt scenario's waarbij meer dan één account kan worden ondertekend op hetzelfde moment. Een voorbeeld is een e-mailtoepassing waar een gebruiker heeft diverse accounts.

## <a name="display-basic-token-information"></a>Token standaardgegevens weergeven

* Voeg de volgende methode **MainPage.xaml.cs** om algemene informatie over het token weer te geven:

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

ID-tokens die zijn verkregen **OpenID Connect** bevatten ook een kleine subset van de informatie die relevant zijn voor de gebruiker. `DisplayBasicTokenInfo` bevat basisgegevens opgenomen in het token. Voorbeelden zijn de weergavenaam van de gebruiker en -ID, de vervaldatum van het token en de tekenreeks met het toegangstoken zelf. Als u selecteert de **Microsoft Graph-API aanroepen** meerdere keren knop, ziet u dat hetzelfde token opnieuw is gebruikt voor volgende aanvragen. U ziet ook de vervaldatum uitgebreid wanneer MSAL beslist is het tijd om het token te verlengen.

## <a name="register-your-application"></a>Uw toepassing registreren

U moet nu uw toepassing registreren in de Registratieportal voor Microsoft-toepassing:
1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app) om een toepassing te registreren.
2. Voer een naam voor uw toepassing.
3. Zorg ervoor dat de optie voor **Begeleide instelprocedure** is *niet geselecteerd*.
4. Selecteer **Platforms toevoegen**, selecteer **systeemeigen toepassing**, en selecteer vervolgens **opslaan**.
5. Kopieer de GUID in **toepassings-ID**, gaat u terug naar Visual Studio, open **App.xaml.cs**, en vervang `your_client_id_here` met de toepassings-ID die u zojuist hebt geregistreerd:

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Geïntegreerde verificatie inschakelen op federatieve domeinen (optioneel)

Als u wilt geïntegreerde Windows-verificatie inschakelen als deze wordt gebruikt met een federatieve Azure Active Directory-domein, moet het toepassingsmanifest aanvullende mogelijkheden inschakelen:

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
> Geïntegreerde Windows-verificatie is niet geconfigureerd voor dit voorbeeld standaard. Toepassingen die aanvragen *Ondernemingsverificatie* of *gedeelde gebruikerscertificaten* mogelijkheden vereist een hogere mate van verificatie door de Windows Store. Niet alle ontwikkelaars willen ook de hogere mate van verificatie. Schakel deze instelling alleen als u geïntegreerde Windows-verificatie met een federatieve Azure Active Directory-domein moet.


## <a name="test-your-code"></a>Testen van uw code

Selecteer F5 uw project in Visual Studio wilt uitvoeren voor het testen van uw toepassing. Het hoofdvenster wordt weergegeven:

![De gebruikersinterface van de toepassing](media/active-directory-uwp-v2.md/testapp-ui.png)

Wanneer u klaar bent om te testen, selecteert u **Microsoft Graph-API aanroepen**. Gebruik vervolgens een organisatieaccount voor Microsoft Azure Active Directory of een Microsoft-account, zoals live.com of outlook.com, aan te melden. Als de eerste keer is, ziet u een venster waarin de gebruiker aan te melden:

![Aanmeldingspagina](media/active-directory-uwp-v2.md/sign-in-page.png)

### <a name="consent"></a>Akkoord
De eerste keer dat u zich aanmeldt bij uw toepassing krijgt u een toestemming van de volgende strekking scherm. Selecteer **Ja** om expliciet toestemming voor toegang:

![Toegang toestemming scherm](media/active-directory-uwp-v2.md/consentscreen.png)
### <a name="expected-results"></a>Verwachte resultaten
U ziet gebruikersprofielgegevens geretourneerd door de Microsoft Graph API-aanroep op de **API aanroepen resultaten** scherm:

![API-aanroepen resultaten scherm](media/active-directory-uwp-v2.md/uwp-results-screen.PNG)

Zie ook basisinformatie over het token dat is verkregen `AcquireTokenAsync` of `AcquireTokenSilentAsync` in de **Token Info** vak:

|Eigenschap  |Indeling  |Beschrijving |
|---------|---------|---------|
|**Naam** |Volledige naam van gebruiker|De gebruiker eerst de- en achternaam.|
|**Gebruikersnaam** |<span>user@domain.com</span> |De gebruikersnaam die de gebruiker identificeert.|
|**Token is verlopen** |DateTime |De tijd waarop het token verloopt. MSAL breidt de verloopdatum te vernieuwen van het token indien nodig.|
|**Toegangstoken** |Reeks |De token tekenreeks die wordt verzonden naar HTTP-aanvragen waarvoor een *autorisatie-header*.|

#### <a name="see-whats-in-the-access-token-optional"></a>Ontdek wat er in het toegangstoken (optioneel)
Eventueel, kopieert u de waarde in **Access Token** en plak deze in https://jwt.ms dit decoderen en het bekijken van de lijst met claims.

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

Microsoft Graph API vereist de *user.read* bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard toegepast in elke toepassing die geregistreerd in de Portal van de registratie van de toepassing. Andere voor Microsoft Graph-API's en aangepaste API's voor uw back-end-server mogelijk extra scopes. Microsoft Graph API vereist de *Calendars.Read* bereik voor een lijst met agenda's van de gebruiker.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing, voeg de *Calendars.Read* overgedragen machtigingen voor de toepassing registratie-informatie. Voeg vervolgens de *Calendars.Read* bereik voor de `acquireTokenSilent` aanroepen. 

> [!NOTE]
> Gebruikers mogelijk gevraagd om extra toestemmingen als u het aantal scopes verhogen.

## <a name="known-issues"></a>Bekende problemen

### <a name="issue-1"></a>1 probleem
U ontvangt een van de volgende foutberichten weergegeven wanneer u zich op uw toepassing in een federatieve Azure Active Directory-domein aanmelden:
 - Er is geen geldig clientcertificaat gevonden in de aanvraag.
 - Er is geen geldige certificaten gevonden in het certificaatarchief van de gebruiker.
 - Probeer opnieuw het kiezen van een andere verificatiemethode.

**Oorzaak:** mogelijkheden voor ondernemingen en certificaat is niet ingeschakeld.

**Oplossing:** Volg de stappen in [geïntegreerde verificatie op federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional).

### <a name="issue-2"></a>Probleem 2
U inschakelen [geïntegreerde verificatie op federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional) en proberen te gebruiken Windows Hello op een Windows 10-computer zich in een omgeving met meervoudige verificatie is geconfigureerd. De lijst met certificaten is opgenomen. Als u gebruiken, uw PINCODE wilt, wordt het venster PINCODE echter nooit gepresenteerd.

**Oorzaak:** dit probleem is een bekende beperking van de webauthenticatiebroker in UWP-toepassingen die worden uitgevoerd op Windows 10 desktop. Het werkt probleemloos op Windows 10 Mobile.

**Tijdelijke oplossing:** Selecteer **aanmelden met andere opties**. Selecteer vervolgens **aanmelden met een gebruikersnaam en wachtwoord**. Selecteer **geeft u uw wachtwoord**. Doorloop vervolgens naar het authenticatieproces telefoon.
