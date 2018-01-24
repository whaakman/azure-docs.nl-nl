---
title: Aanmelden, verificatie bewerken profiel .NET Azure Active Directory B2C | Microsoft Docs
description: Het bouwen van een Windows-bureaubladtoepassing met aanmelden, aanmelding en Profielbeheer met behulp van Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 9da14362-8216-4485-960e-af17cd5ba3bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: seohack1
ms.openlocfilehash: 5d4664e87ca0a45d59d976f6415fce858bc51dcd
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Een Windows desktop app bouwen
U kunt met behulp van Azure Active Directory (Azure AD) B2C beheerfuncties krachtige Self-service identiteitsbeheer toevoegen aan uw bureaublad-app in slechts enkele korte stappen. In dit artikel wordt beschreven hoe u een .NET Windows Presentation Foundation (WPF) 'takenlijst' u app maakt die gebruikersregistratie, aanmelding en Profielbeheer bevat. De app biedt ondersteuning voor aanmelden en aanmelden met een gebruikersnaam of e-mailbericht. Het biedt ook ondersteuning voor zich kunnen registreren en aanmelden via sociale accounts zoals Facebook en Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Een Azure AD B2C-directory maken
Voordat u Azure AD B2C kunt gebruiken, moet u een directory, of tenant, maken.  Een directory is een container voor alle gebruikers, apps, groepen en meer. Als u nog geen directory hebt, [maakt u een B2C-directory](active-directory-b2c-get-started.md) voordat u doorgaat in deze handleiding.

## <a name="create-an-application"></a>Een app maken
Vervolgens maakt u een app in uw B2C-directory. Hiermee geeft u informatie door aan Azure AD die nodig is om veilig te communiceren met uw app. Volg [deze instructies](active-directory-b2c-app-registration.md) om een app te maken.  Zorg ervoor dat:

* Omvatten een **native client** in de toepassing.
* Kopieer de **omleidings-URI** `urn:ietf:wg:oauth:2.0:oob`. Dit is de standaard-URL voor dit codevoorbeeld.
* U de **toepassings-id** kopieert die is toegewezen aan uw app. U hebt dit later nodig.

## <a name="create-your-policies"></a>Het beleid maken
In Azure AD B2C wordt elke gebruikerservaring gedefinieerd door [beleid](active-directory-b2c-reference-policies.md). Dit codevoorbeeld bevat drie identiteitservaringen: registreren, aanmelden en profiel bewerken. U moet een beleid voor elk type maken, zoals beschreven in de [naslagartikel](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Wanneer u uw drie beleidsregels maakt:

* Kiest u **Registratie met gebruikers-id** of **Registratie via e-mail** op de blade met identiteitsproviders.
* Kiest u **Weergavenaam** en andere registratiekenmerken in het registratiebeleid.
* Kiest u **Weergavenaam**- en **Object-id**-claims als toepassingsclaims voor elk beleid. U kunt ook andere claims kiezen.
* Kopieert u de **naam** van elk beleid nadat u dit hebt gemaakt. Deze moet het voorvoegsel `b2c_1_` bevatten.  U hebt deze beleidsnamen later nodig.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, kunt u uw app maken.

## <a name="download-the-code"></a>De code downloaden
De code voor deze zelfstudie [wordt bewaard in GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Als u het voorbeeld wilt maken terwijl u aan het werk bent, kunt u [een basisproject downloaden als zip-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). U kunt het basisproject ook klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

De voltooide app is ook [beschikbaar als zip-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) of in de `complete`-vertakking van dezelfde opslagplaats.

Nadat u de voorbeeldcode hebt gedownload, opent u het SLN-bestand in Visual Studio om aan de slag te gaan. De `TaskClient` project is de WPF-bureaubladtoepassing die de gebruiker werkt. Voor de doeleinden van deze zelfstudie wordt een taak back-end-web-API, gehost in Azure die de takenlijst van elke gebruiker opslaat aangeroepen.  U hoeft niet voor het bouwen van de web-API, we hebben al actief voor u.

Bekijk voor meer informatie over hoe een web-API veilig wordt geverifieerd aanvragen met behulp van Azure AD B2C, de [web-API aan de slag artikel](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Uitvoeren van beleid
Uw app communiceert met Azure AD B2C door te sturen verificatieberichten die het beleid dat hij of zij wil uit te voeren als onderdeel van de HTTP-aanvraag opgeven. Voor .NET-desktoptoepassingen, kunt u de voorbeeld Microsoft Authentication Library (MSAL) gebruiken om te verzenden van berichten van OAuth 2.0-verificatie, voert u beleidsregels en ophalen van tokens die aanroepen van web-API's.

### <a name="install-msal"></a>MSAL installeren
Toevoegen van MSAL naar de `TaskClient` project met de Visual Studio Package Manager-Console.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Uw B2C-gegevens invoeren
Open het bestand `Globals.cs` en elk van de eigenschapswaarden vervangt door uw eigen. Deze klasse wordt gebruikt in de gehele `TaskClient` verwijzing gebruikte waarden.

```csharp
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="create-the-publicclientapplication"></a>De PublicClientApplication maken
De primaire klasse van MSAL is `PublicClientApplication`. Deze klasse vertegenwoordigt de toepassing in het Azure AD B2C-systeem. Wanneer de initalizes app maakt voor een exemplaar van `PublicClientApplication` in `MainWindow.xaml.cs`. Dit kan worden gebruikt in het venster.

```csharp
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app,
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### <a name="initiate-a-sign-up-flow"></a>Een registratie stroom initiëren
Wanneer een gebruiker ervoor om te tekenen van kiest, die u wilt starten een aanmelding stroom die gebruikmaakt van het registratiebeleid die u hebt gemaakt. Met behulp van MSAL die u zojuist hebt aanroepen `pca.AcquireTokenAsync(...)`. De parameters die u doorgeeft aan `AcquireTokenAsync(...)` bepalen welk token wordt weergegeven, het beleid op waarmee de verificatieaanvraag, enzovoort.

```csharp
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a>Starten van een stroom aanmelden
U kunt een stroom aanmelden op dezelfde manier als dat u een aanmelding stroom initiëren initiëren. Wanneer een gebruiker zich aanmeldt, moet u dezelfde aanroep MSAL, ditmaal met behulp van uw beleid voor aanmelden:

```csharp
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Starten van een stroom profiel bewerken
U kunt een beleid voor het profiel bewerken opnieuw uitvoeren op dezelfde manier:

```csharp
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

In al deze gevallen retourneert MSAL ofwel een token in `AuthenticationResult` of er een uitzondering gegenereerd. Telkens wanneer u een token via MSAL krijgen, kunt u de `AuthenticationResult.User` object bijwerken van de gebruikersgegevens in de app, zoals de gebruikersinterface. Het token voor gebruik in andere onderdelen van de toepassing worden ook opgeslagen in ADAL.

### <a name="check-for-tokens-on-app-start"></a>Controleer voor tokens op app starten
U kunt ook MSAL gebruiken om-in status van de gebruiker bij te houden.  In deze app willen we de gebruiker blijft aangemeld nadat ze de app sluiten en opnieuw openen.  Terug in de `OnInitialized` overschrijven, gebruikt u de MSAL `AcquireTokenSilent` methode om te controleren of tokens in de cache opgeslagen:

```csharp
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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
```

## <a name="call-the-task-api"></a>De takenlijst-API aanroepen
U hebt nu MSAL gebruikt om beleidsregels uitvoeren en tokens verkrijgen.  Wanneer u een deze tokens aan te roepen de takenlijst-API gebruiken wilt, kunt u opnieuw gebruiken van MSAL `AcquireTokenSilent` methode om te controleren of tokens in de cache opgeslagen:

```csharp
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

Bij het aanroepen van `AcquireTokenSilentAsync(...)` is gelukt en een token gevonden in de cache, kunt u het token voor toevoegen de `Authorization` koptekst van de HTTP-aanvraag. De web-API van de taak wordt deze header gebruiken om te verifiëren van de aanvraag voor het lezen van de takenlijst van de gebruiker:

```csharp
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>De gebruiker afmelden
Ten slotte kunt u MSAL naar einde van een gebruikerssessie met de app wanneer de gebruiker selecteert **Afmelden**.  Wanneer u MSAL gebruikt, wordt dit wordt bereikt door het wissen van alle van de tokens van de tokencache:

```csharp
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren
Ten slotte bouwen en uitvoeren van het voorbeeld.  Zich registreren voor de app met behulp van de naam van een e-adres of de gebruiker. Meld u af en meld u opnieuw aan als dezelfde gebruiker. Profiel van de gebruiker bewerken. Meld u af en meld u aan met behulp van een andere gebruiker.

## <a name="add-social-idps"></a>Sociale IDPs toevoegen
Op dit moment wordt de app ondersteunt alleen gebruiker registreren en aanmelden met **lokale accounts**. Dit zijn opgeslagen in uw B2C-directory accounts die gebruikmaken van een gebruikersnaam en wachtwoord. U kunt met behulp van Azure AD B2C, ondersteuning voor andere id-providers (IDPs) toevoegen zonder uw code.

Volg de gedetailleerde instructies in deze artikelen wilt sociale IDPs toevoegen aan uw app, eerst. Voor elke IDP die u wilt ondersteunen, moet u een toepassing registreren in dat systeem en het verkrijgen van een client-ID.

* [Facebook ingesteld als een IDP](active-directory-b2c-setup-fb-app.md)
* [Google ingesteld als een IDP](active-directory-b2c-setup-goog-app.md)
* [Amazon ingesteld als een IDP](active-directory-b2c-setup-amzn-app.md)
* [LinkedIn instellen als een IDP](active-directory-b2c-setup-li-app.md)

Nadat u de id-providers aan uw B2C-directory toevoegen, moet u elk van de drie beleidsregels om op te nemen van de nieuwe IDPs bewerken, zoals beschreven in de [naslagartikel](active-directory-b2c-reference-policies.md). Nadat u uw beleid opslaat, voer de app opnieuw. U ziet nu de nieuwe IDPs toegevoegd als aanmelden en aanmeldingsopties in elk van uw identiteit ervaringen.

U kunt experimenteren met het beleid en houd rekening met de gevolgen voor uw voorbeeld-app. Toevoegen of verwijderen van IDPs, toepassingsclaims bewerken of registratiekenmerken wijzigen. Experiment totdat u kunt zien hoe beleidsregels, verificatieaanvragen en MSAL met elkaar verbinden.

Voor een verwijzing naar het voltooide voorbeeld [wordt geleverd als ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). U kunt dit ook klonen van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
