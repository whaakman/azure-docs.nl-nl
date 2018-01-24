---
title: Azure AD Windows Phone-aan de slag | Microsoft Docs
description: "Het bouwen van een Windows Phone-toepassing die kan worden geïntegreerd met Azure AD voor aanmelden en Azure AD-aanroepen beveiligd met OAuth API's."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 66f5ac20-5e1f-4b9d-bb99-9b3305e26416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/30/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c078ae22255190a37d75a4100ebfffcb6288c4cb
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-ad-windows-phone-getting-started"></a>Azure AD Windows Phone aan de slag
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Projecten met Windows Phone 8.1 en een eerdere versie worden niet ondersteund in Visual Studio 2017.  Zie [Geschikte platforms voor Visual Studio 2017 en compatibiliteit](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) voor meer informatie.

Als u een Windows Phone 8.1-app ontwikkelt, kunt Azure AD u eenvoudig en snel voor u om uw gebruikers met hun Active Directory-account te verifiëren.  Ook kunt uw toepassing veilig gebruiken voor een web-API die zijn beveiligd door Azure AD, zoals de Office 365-API of de Azure-API.

> [!NOTE]
> Deze voorbeeldcode maakt gebruik van ADAL versie 2.0.  Voor de nieuwste technologie mogelijk wilt u in plaats daarvan onze [universele Windows-zelfstudie met behulp van ADAL v3.0](active-directory-devquickstarts-windowsstore.md).  Als u een app voor Windows Phone 8.1 inderdaad maakt, is dit de juiste plaats.  ADAL v2.0 wordt nog steeds volledig ondersteund en is de aanbevolen manier van het ontwikkelen van apps agianst Windows Phone 8.1 gebruikmaken van Azure AD.
> 
> 

Voor .NET systeemeigen clients die toegang moeten krijgen tot beveiligde bronnen, levert Azure AD de Active Directory Authentication Library of ADAL.  Enig doel in het leven van ADAL is gemakkelijker voor uw app toegangstokens ophalen.  Om te demonstreren hoe eenvoudig het is, je hier we bouwt u een 'Directory Searcher' Windows Phone 8.1-app die:

* Krijgt toegang tot tokens voor het aanroepen van de Azure AD Graph API met behulp van de [OAuth 2.0-verificatieprotocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Zoekt een directory voor gebruikers met een opgegeven UPN.
* Tekenen gebruikers uit.

De volledige werkende toepassing bouwen, moet u het:

1. Uw toepassing registreren met Azure AD.
2. Installeren en configureren van ADAL.
3. ADAL gebruikt om tokens van Azure AD.

Aan de slag [een basisproject downloaden](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) of [het voltooide voorbeeld downloaden](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Elk is een Visual Studio 2013-oplossing.  U moet ook een Azure AD-tenant kunt u gebruikers maken en een toepassing registreren.  Als u niet al een tenant [Lees hoe u een](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>1. De Directory Searcher toepassing registreren
Als u wilt inschakelen voor uw app tokens krijgen, moet u eerst registreren in uw Azure AD-tenant en verleent deze machtiging voor toegang tot de Azure AD Graph API:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op de bovenste balk op je account en klikt u onder de **Directory** kiest u de Active Directory-tenant waar u wilt uw toepassing registreren.
3. Klik op **meer Services** in de nav linkerkant en kies **Azure Active Directory**.
4. Klik op **App registraties** en kies **toevoegen**.
5. Volg de aanwijzingen en maak een nieuwe **systeemeigen clienttoepassing**.
  * De **naam** van de toepassing bevat een beschrijving van uw toepassing aan eindgebruikers
  * De **omleidings-Uri** is een combinatie schema en de tekenreeks die door Azure AD wordt gebruikt om te retourneren van reacties token.  Een tijdelijke aanduidingswaarde opgeven voor nu, bijvoorbeeld `http://DirectorySearcher`.  We Vervang deze waarde hoger.
6. Zodra u inschrijving hebt voltooid, toewijst AAD uw app een unieke id  U moet deze waarde in de volgende secties, dus kopiëren vanaf het toepassingstabblad.
7. Van de **instellingen** pagina **Required Permissions** en kies **toevoegen**. Selecteer de **Microsoft Graph** als de API en voeg de **Directory-gegevens lezen** machtiging onder **gedelegeerde machtigingen**.  Hiermee schakelt u uw toepassing query uitvoeren op de Graph-API voor gebruikers.

## <a name="2-install--configure-adal"></a>2. Installeren en configureren van ADAL
Nu dat u een toepassing in Azure AD hebt, kunt u ADAL installeert en uw identiteitsgerelateerde code schrijven.  In de volgorde voor ADAL om te communiceren met Azure AD, moet u voorzien enige informatie over de registratie van uw app.

* Beginnen met het ADAL toevoegen aan het DirectorySearcher-project met de Package Manager-Console.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

* Open in het project DirectorySearcher `MainPage.xaml.cs`.  Vervang de waarden in de `Config Values` regio in overeenstemming met de waarden die u in de Azure-portal hebt ingevoerd.  Uw code zal naar deze waarden verwijzen wanneer deze gebruikmaakt van ADAL.
  * De `tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com
  * De `clientId` is de clientId van uw toepassing die u hebt gekopieerd uit de portal.
* U moet nu voor het detecteren van de callback-uri voor uw Windows Phone-app.  Stel een onderbrekingspunt in op deze regel in de `MainPage` methode:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
* Voer de app en kopieer gereserveerd de waarde van `redirectUri` wanneer het onderbrekingspunt is bereikt.  Moet er ongeveer als

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

* Terug op de **configureren** tabblad van uw toepassing in de Azure-beheerportal, vervang de waarde van de **RedirectUri** met deze waarde.  

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. Gebruikmaken van ADAL Tokens van AAD ophalen
Het basisprincipe achter ADAL is dat wanneer uw app een toegangstoken moet, aanroept `authContext.AcquireToken(…)`, en doet de rest van ADAL.  

* De eerste stap is het initialiseren van uw app `AuthenticationContext` -ADAL de primaire klasse.  Dit is waar het doorgeven van ADAL de coördinaten nodig om te communiceren met Azure AD en hoe deze tokens in de cache.

```csharp
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

* Nu zoeken de `Search(...)` methode, die wordt aangeroepen wanneer de gebruiker cliks de "zoeken" in de gebruikersinterface van de app.  Deze methode maakt een GET-aanvraag voor Azure AD Graph API aan query voor gebruikers wiens UPN met de opgegeven zoekterm begint.  Maar om de Graph API een query uitvoert, moet u een access_token in de `Authorization` header van de aanvraag - dit is waar de ADAL wordt geleverd.

```csharp
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
* Als interactieve verificatie nodig is, ADAL van Windows Phone Web Authentication Broker (Windows-Adresboek) wordt gebruikt en [voortzetting model](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) om de Azure AD-aanmelding op de pagina weer te geven.  Wanneer de gebruiker zich aanmeldt, uw app moet ADAL geeft de resultaten van de Windows-Adresboek interactie.  Dit is net zo eenvoudig als het implementeren van de `ContinueWebAuthentication` interface:

```csharp
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

* Nu is het tijd om het gebruik van de `AuthenticationResult` die ADAL geretourneerd aan uw app.  In de `QueryGraph(...)` retouraanroep, de access_token die u hebt verkregen koppelen aan de GET-aanvraag in de autorisatie-header:

```csharp
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
* U kunt ook de `AuthenticationResult` object om informatie over de gebruiker in uw app weer te geven. In de `QueryGraph(...)` methode resultaat gebruiken om de gebruikers-ID op de pagina weer te geven:

```csharp
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
* Ten slotte kunt u ADAL voor het ondertekenen van de gebruiker buiten de toepassing ook.  Wanneer de gebruiker op de knop 'Afmelden', willen we ervoor te zorgen dat de volgende aanroep `AcquireTokenSilentAsync(...)` zal mislukken.  Met ADAL, is dit is net zo eenvoudig als het token cache wissen:

```csharp
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Gefeliciteerd! U nu beschikken over een werkende Windows Phone-app met de mogelijkheid om te verifiëren van gebruikers, veilig aanroepen van Web-API's met behulp van OAuth 2.0 en algemene informatie over de gebruiker ophalen.  Als u nog niet gedaan hebt, is nu de tijd voor het vullen van uw tenant waarbij sommige gebruikers.  Uitvoeren van uw app DirectorySearcher en meld u aan met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van de UPN.  Sluit de app en voer deze opnieuw uit.  U ziet hoe de gebruikerssessie blijft intact.  Meld u af en meld u opnieuw aan als een andere gebruiker.

ADAL kunt eenvoudig gebruikmaken van al deze algemene identiteit functies in uw toepassing.  Dit zorgt voor al het werk dirty voor u - Cachebeheer, OAuth-protocolondersteuning, dat de gebruiker een aanmelding-gebruikersinterface vernieuwen van tokens verlopen en meer.  Alles wat u moet weten één API-aanroep is `authContext.AcquireToken*(…)`.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) is opgegeven [hier](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  U kunt nu verder met de identiteit van de aanvullende scenario's.  U wilt proberen:

[Een .NET-Web-API met Azure AD beveiligen >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

