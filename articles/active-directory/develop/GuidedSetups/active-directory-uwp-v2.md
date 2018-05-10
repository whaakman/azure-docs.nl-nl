---
title: Azure AD v2 UWP aan de slag | Microsoft Docs
description: Hoe toepassingen Universal Windows Platform (XAML) een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen
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
ms.openlocfilehash: 390559922b3b8fb293d1c8b38f36dfd0a1df9ebd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>De Microsoft Graph-API aanroepen vanuit een Universal Windows Platform (UWP)-toepassing

Deze handleiding wordt uitgelegd hoe een systeemeigen Universal Windows Platform (XAML)-toepassing kunt ophalen van een toegangstoken en gebruik vervolgens deze toegangstoken om aan te roepen Microsoft Graph API of andere API's waarvoor toegangstokens van Azure Active Directory-v2-eindpunt.

Aan het einde van deze handleiding, uw toepassing kan worden aanroepen van een beveiligde API met persoonlijke accounts (inclusief outlook.com, live.com en anderen) en de werk- en schoolaccounts van een bedrijf of organisatie die Azure Active Directory heeft.  

> Deze handleiding vereist 2017 voor Visual Studio met de ontwikkeling van universele Windows-Platform geïnstalleerd. Dit selectievakje inschakelt [artikel](https://docs.microsoft.com/windows/uwp/get-started/get-set-up "Visual Studio instellen voor UWP") voor instructies over het downloaden en configureren van Visual Studio om Universal Windows Platform-Apps te ontwikkelen.

### <a name="how-this-guide-works"></a>De werking van deze handleiding

![De werking van deze handleiding](media/active-directory-mobileanddesktopapp-windowsuniversalplatform-introduction/uwp-intro.png)

De voorbeeldtoepassing die is gemaakt door deze handleiding kunt een UWP-app om op te vragen voor Microsoft Graph API of een Web-API die tokens van het Azure Active Directory-v2-eindpunt accepteert. Voor dit scenario wordt een token toegevoegd om HTTP-aanvragen via de autorisatie-header. Token acquisities van organisaties en vernieuwingen worden afgehandeld door de Microsoft Authentication Library (MSAL).

### <a name="nuget-packages"></a>NuGet-pakketten

Deze handleiding bevat de volgende NuGet-pakketten:

|Bibliotheek|Beschrijving|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft-Verificatiebibliotheek (MSAL)|


## <a name="set-up-your-project"></a>Instellen van uw project

Deze sectie bevat stapsgewijze instructies voor het integreren van een Windows Desktop .NET-toepassing (XAML) met *aanmelden met Microsoft* zodat Web-API's waarvoor een token, zoals Microsoft Graph API op te vragen.

De toepassing gemaakt door deze handleiding wordt weergegeven een knop waarmee een query Graph API, een knop Afmelden en tekstvakken die het resultaat van de oproepen weergeven.

> Voorkeur voor het downloaden van dit voorbeeld Visual Studio-project in plaats daarvan? [Downloaden van een project](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip) en doorgaan met de [toepassingsregistratie](#register-your-application "toepassing registratiestap") stap voor het configureren van het codevoorbeeld voordat wordt uitgevoerd.


### <a name="create-your-application"></a>Uw toepassing maken
1. In Visual Studio: **bestand** > **nieuwe** > **Project**<br/>
2. Onder *sjablonen*, selecteer **Visual C#**
3. Selecteer **lege App (universeel Windows)**
4. Een naam geven en klik op 'Ok'.
5. Als u wordt gevraagd, afgenomen tot minder vrije elke versie van selecteren *doel* en *Minimum* versie en klik op 'Ok':<br/><br/>![Minimum- en doel-versies](media/active-directory-uwp-v2.md/vs-minimum-target.png)

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>De Microsoft Authentication Library (MSAL) toevoegen aan uw project
1. In Visual Studio: **extra** > **NuGet-Pakketbeheer** > **Package Manager-Console**
2. De volgende opdracht in het venster Package Manager Console kopiëren en plakken:

    ```powershell
    Install-Package Microsoft.Identity.Client -Pre
    ```

> [!NOTE]
> Het pakket hierboven installeert de [Microsoft Authentication Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet). MSAL verwerkt ophalen, opslaan in cache en gebruikt voor toegang tot API's die zijn beveiligd door Azure Active Directory-v2 gebruikerstokens vernieuwen.

## <a name="initialize-msal"></a>MSAL initialiseren
Deze stap maakt u een klasse voor het afhandelen van interactie met MSAL bibliotheek, zoals de verwerking van tokens.

1. Open de **App.xaml.cs** -bestand en voeg de referentie voor MSAL bibliotheek toe aan de klasse:

    ```csharp
    using Microsoft.Identity.Client;
    ```

2. De volgende twee regels toevoegen aan de klasse van de App (binnen <code>sealed partial class App : Application</code> blok):

    ```csharp
    // Below is the clientId of your app registration. 
    // You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";
    
    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
    ```

## <a name="create-your-applications-ui"></a>De gebruikersinterface van uw toepassing maken

Een **MainPage.xaml** bestand automatisch moet worden gemaakt als onderdeel van uw project-sjabloon. Dit bestand openen en volg de instructies:

1.  Vervangen van uw toepassing **<Grid>** knooppunt met:

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
    
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>De Microsoft Authentication Library (MSAL) gebruiken voor het ophalen van een token voor de Microsoft Graph-API

Deze sectie wordt beschreven hoe MSAL gebruiken voor een token voor de Microsoft Graph API.

1.  In **MainPage.xaml.cs**, voeg de referentie voor MSAL bibliotheek toe aan de klasse:

    ```csharp
    using Microsoft.Identity.Client;
    ```
2. Vervang de code van uw <code>MainPage</code> klasse met:

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
Het aanroepen van de `AcquireTokenAsync` methode resulteert in een venster waarin gebruikers zich aanmelden. Toepassingen vereisen meestal gebruikers zich aanmelden in interactief de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron. Ze wellicht ook aan te melden bij een achtergrond te verkrijgen van een token mislukt (bijvoorbeeld wanneer een gebruiker het wachtwoord is verlopen).

#### <a name="get-a-user-token-silently"></a>Een gebruiker achtergrond token ophalen
De `AcquireTokenSilentAsync` methode token acquisities van organisaties en vernieuwingen zonder tussenkomst van de gebruiker worden verwerkt. Na `AcquireTokenAsync` wordt uitgevoerd voor de eerste keer `AcquireTokenSilentAsync` is de gebruikelijke methode gebruiken om toegang te verkrijgen van tokens die toegang beveiligde bronnen voor volgende aanroepen tot omdat aanroepen naar aanvragen of vernieuwen van tokens op de achtergrond worden gemaakt.

Uiteindelijk de `AcquireTokenSilentAsync` methode mislukken. Oorzaak van de fout is mogelijk dat de gebruiker heeft zich afgemeld of het wachtwoord op een ander apparaat gewijzigd. Wanneer MSAL detecteert dat het probleem doordat een interactieve actie kan worden omgezet, wordt deze gebeurtenis wordt gestart een `MsalUiRequiredException` uitzondering. Uw toepassing kan verwerken van deze uitzondering op twee manieren:

* Er kan een aanroep van tegen `AcquireTokenAsync` onmiddellijk. Deze aanroep resulteert in vraagt de gebruiker aan te melden. Dit patroon wordt gewoonlijk in onlinetoepassingen gebruikt wanneer er geen beschikbare offline inhoud voor de gebruiker is. De steekproef die worden gegenereerd door deze Begeleide instelprocedure volgt dit patroon die u kunt zien in actie de eerste keer dat u het voorbeeld uitvoert. 
    * Omdat er geen gebruiker heeft de toepassing gebruikt `PublicClientApp.Users.FirstOrDefault()` bevat een null-waarde en een `MsalUiRequiredException` uitzondering gegenereerd. 
    * De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `AcquireTokenAsync`, wat ertoe leidt de gebruiker aan te melden.

* Het kan in plaats daarvan een visuele indicatie opleveren voor gebruikers die een interactief aanmelden vereist is, zodat ze het juiste moment aan te melden kunnen selecteren. Of de toepassing opnieuw kunt `AcquireTokenSilentAsync` later. Dit patroon wordt vaak gebruikt als gebruikers andere toepassingsfunctionaliteit van de zonder onderbreking--bijvoorbeeld gebruiken kunnen wanneer u offline inhoud is beschikbaar in de toepassing. In dit geval bepalen gebruikers wanneer ze aanmelden willen bij de toegang tot de beveiligde bron of de verouderde gegevens te vernieuwen. U kunt ook de toepassing opnieuw kunt bepalen `AcquireTokenSilentAsync` wanneer het netwerk is hersteld nadat u hebt tijdelijk niet beschikbaar zijn.

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>De Microsoft Graph API met behulp van het token dat u zojuist hebt verkregen aanroepen

1. Voeg de volgende nieuwe methode voor uw **MainPage.xaml.cs**. De methode wordt gebruikt om een `GET` -aanvraag in voor Graph API met behulp van een autoriseren-header:

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

In deze voorbeeldtoepassing de `GetHttpContentWithToken` methode wordt gebruikt voor het maken van een HTTP `GET` aanvragen op basis van een beveiligde bron die is een token vereist en vervolgens de inhoud naar de aanroeper wordt geretourneerd. Met deze methode wordt het token verkregen in de *HTTP-autorisatie-header*. Voor dit voorbeeld wordt de resource is de Microsoft Graph API *mij* eindpunt – waarin informatie over het profiel van de gebruiker.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Een methode voor het ondertekenen van de gebruiker toevoegen

1. Om de gebruiker afmelden, voegt u de volgende methode voor **MainPage.xaml.cs**:

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

### <a name="more-info-on-sign-out"></a>Meer informatie over afmelden

De methode `SignOutButton_Click` verwijdert u de gebruiker uit cache van de gebruiker MSAL – MSAL Zodoende weet om de huidige gebruiker vergeet, zodat een toekomstige aanvraag voor het verkrijgen van een token alleen slagen kan als deze wordt doorgevoerd interactief zijn.
Hoewel de toepassing in dit voorbeeld een enkele gebruiker ondersteunt, ondersteunt MSAL scenario's waarbij meerdere accounts kunnen worden aangemeld op hetzelfde moment: een voorbeeld is een e-mailtoepassing waar een gebruiker heeft meerdere accounts.

## <a name="display-basic-token-information"></a>Token standaardgegevens weergeven

1. Voeg de volgende methode voor uw **MainPage.xaml.cs** om algemene informatie over het token weer te geven:

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

ID-tokens die zijn verkregen *OpenID Connect* bevatten ook een kleine subset van de informatie die relevant zijn voor de gebruiker. `DisplayBasicTokenInfo` algemene informatie opgenomen in het token wordt weergegeven: bijvoorbeeld de weergavenaam van de gebruiker en-ID, evenals de vervaldatum van token en de toegang tot het type string token zelf. Deze informatie wordt weergegeven voor u om te zien. U kunt raakt de **Microsoft Graph-API aanroepen** meerdere keren knop en Zie dat hetzelfde token opnieuw is gebruikt voor volgende aanvragen. U ziet ook de verloopdatum wordt verlengd wanneer MSAL het beslist is tijd om het token te verlengen.

## <a name="register-your-application"></a>Uw toepassing registreren

Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app) een toepassing registreren
2. Voer een naam voor uw toepassing 
3. Zorg ervoor dat de optie voor begeleide Setup is uitgeschakeld
4. Klik op **Platforms toevoegen**, selecteer daarna **systeemeigen toepassing** en klik op Opslaan
5. Kopiëren van de GUID in toepassings-ID, gaat u terug naar Visual Studio, open **App.xaml.cs** en vervang `your_client_id_here` met de toepassings-ID die u zojuist hebt geregistreerd:

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```

## <a name="enable-integrated-authentication-on-federated-domains-optional"></a>Geïntegreerde verificatie inschakelen op federatieve domeinen (optioneel)

Om in te schakelen geïntegreerde Windows-verificatie gebruikt in combinatie met een federatieve Azure Active Directory-domein, moet het toepassingsmanifest aanvullende mogelijkheden inschakelen:

1. Dubbelklik op **Package.appxmanifest**
2. Selecteer **mogelijkheden** tabblad en zorg ervoor dat de volgende instellingen zijn ingeschakeld:

    - Enterprise-verificatie
    - Particuliere netwerken (Client en Server)
    - Gedeelde gebruikerscertificaten 

3. Open vervolgens **App.xaml.cs**, en voeg de volgende regel in de App-constructor:

    ```csharp
    App.PublicClientApp.UseCorporateNetwork = true;
    ```

> [!IMPORTANT]
> Geïntegreerde Windows-verificatie is niet geconfigureerd voor dit voorbeeld standaard omdat toepassingen aanvragen van de *Ondernemingsverificatie* of *gedeelde gebruikerscertificaten* mogelijkheden vereisen een hogere mate van verificatie door de Windows Store en niet alle ontwikkelaars wilt uitvoeren van de hogere mate van verificatie. Schakel deze instelling alleen als u geïntegreerde Windows-verificatie met een federatieve Azure Active Directory-domein moet.


## <a name="test-your-code"></a>Testen van uw code

Testen van uw toepassing, drukt u op `F5` uw project in Visual Studio wilt uitvoeren. Het hoofdvenster moet worden weergegeven:

![De gebruikersinterface van de toepassing](media/active-directory-uwp-v2.md/testapp-ui.png)

Wanneer u klaar bent om te testen, klikt u op *Microsoft Graph-API aanroepen* en gebruik van een Microsoft Azure Active Directory (organisatieaccount) of een Microsoft-Account (live.com, outlook.com)-account aan te melden. Als dit de eerste keer is, ziet u een venster waarin de gebruiker aan te melden:

![Aanmeldingspagina](media/active-directory-uwp-v2.md/sign-in-page.png)

### <a name="consent"></a>Toestemming
De eerste keer dat u zich aanmeldt bij uw toepassing krijgt u een toestemming van de volgende strekking scherm waarin u moet expliciet accepteren:

![Toestemming scherm](media/active-directory-uwp-v2.md/consentscreen.png)
### <a name="expected-results"></a>Verwachte resultaten
U ziet gebruikersprofielgegevens geretourneerd door de Microsoft Graph API-aanroep op het scherm API aanroepen resultaten:

![Scherm](media/active-directory-uwp-v2.md/uwp-results-screen.PNG)

U ziet ook basisinformatie over het token dat is verkregen `AcquireTokenAsync` of `AcquireTokenSilentAsync` in het vak Token Info:

|Eigenschap  |Indeling  |Beschrijving |
|---------|---------|---------|
|**Naam** |Volledige naam van gebruiker |De gebruiker eerst de- en achternaam.|
|**Gebruikersnaam** |<span>user@domain.com</span> |De gebruikersnaam die wordt gebruikt om de gebruiker te identificeren.|
|**Token is verlopen** |DateTime |De tijd waarop het token verloopt. MSAL breidt de verloopdatum te vernieuwen van het token indien nodig.|
|**Toegangstoken** |Tekenreeks |De token tekenreeks die wordt verzonden naar HTTP-aanvragen waarvoor een *autorisatie-header*.|

#### <a name="see-what-is-in-the-access-token-optional"></a>Ontdek wat er in het toegangstoken (optioneel)
Desgewenst kunt u de waarde in Access Token kopiëren en plakken in https://jwt.ms dit decoderen en het bekijken van de lijst met claims.

### <a name="more-information-about-scopes-and-delegated-permissions"></a>Meer informatie over bereikwaarden en gedelegeerde machtigingen

De Microsoft Graph API vereist de *user.read* bereik van het gebruikersprofiel lezen. Deze scope wordt automatisch toegevoegd standaard toegepast in elke toepassing die geregistreerd in de Portal van de registratie van de toepassing. Andere voor Microsoft Graph-API's, evenals aangepaste API's voor uw back-end-server mogelijk extra scopes. De Microsoft Graph API vereist de *Calendars.Read* bereik voor een lijst met agenda's van de gebruiker.

Voor toegang tot agenda's van de gebruiker in de context van een toepassing, voeg de *Calendars.Read* overgedragen machtigingen voor de toepassing registratie-informatie. Voeg vervolgens de *Calendars.Read* bereik voor de `acquireTokenSilent` aanroepen. 

> [!NOTE]
> Gebruiker kan worden gevraagd om extra toestemmingen als u het aantal scopes verhogen.

## <a name="known-issues"></a>Bekende problemen

### <a name="issue-1"></a>1 probleem:
U mogelijk een van de volgende fouten bij het aanmelden op uw toepassing op een federatieve Azure Active Directory-domein:
 - Er is geen geldig clientcertificaat gevonden in de aanvraag.
 - Er is geen geldige certificaten gevonden in het certificaatarchief van de gebruiker.
 - Probeer opnieuw het kiezen van een andere verificatiemethode.

**Oorzaak:** mogelijkheden voor ondernemingen en certificaten zijn niet ingeschakeld

**Oplossing:** Volg de stappen in [geïntegreerde verificatie op federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional)

### <a name="issue-2"></a>Probleem 2:
Nadat u hebt ingeschakeld [geïntegreerde verificatie op federatieve domeinen](#enable-integrated-authentication-on-federated-domains-optional) en proberen te gebruiken Windows Hello op een Windows 10-computer zich in een omgeving met meerdere-multi-factor-verificatie geconfigureerd, wordt de lijst met certificaten wordt gepresenteerd, maar als u gebruiken, uw PINCODE wilt, het venster PINCODE wordt nooit weergegeven.

**Oorzaak:** bekende beperking met de webauthenticatiebroker in UWP toepassingen die worden uitgevoerd op Windows 10-bureaublad (werkt prima uit op Windows 10 Mobile)

**Tijdelijke oplossing:** gebruikers moeten selecteren om te aanmelden met andere opties en kies vervolgens *aanmelden met een gebruikersnaam en wachtwoord* in plaats daarvan, selecteer uw wachtwoord opgeven en gaat u via de telefoon-verificatie.

