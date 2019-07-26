---
title: Mobiele app die web-Api's aanroept-een Token ophalen voor de app | Micro soft Identity-platform
description: Meer informatie over het bouwen van een mobiele app die web-Api's aanroept (een token voor de app ophalen)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d49717355cab5441d26608fa12333bd1b8b73d44
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68413532"
---
# <a name="mobile-app-that-calls-web-apis---get-a-token"></a>Mobiele app die web-Api's aanroept-een Token ophalen

Voordat u beveiligde web-Api's kunt aanroepen, moet uw app een toegangs token hebben. Dit artikel begeleidt u bij het proces voor het ophalen van een token met behulp van de micro soft Authentication Library (MSAL).

## <a name="scopes-to-request"></a>Te aanvragen scopes

Wanneer u een token aanvraagt, moet u een bereik definiëren. Het bereik bepaalt welke gegevens uw app kan gebruiken.  

De eenvoudigste benadering is het combi neren van `App ID URI` de gewenste Web-API met de scope. `.default` Op die manier krijgt u het micro soft-identiteits platform dat voor uw app alle scopes in de portal zijn vereist.

#### <a name="android"></a>Android
```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
```

#### <a name="ios"></a>iOS
```swift
let scopes: [String] = ["https://graph.microsoft.com/.default"]
```

#### <a name="xamarin"></a>Xamarin
```CSharp 
var scopes = new [] {"https://graph.microsoft.com/.default"};
```

## <a name="get-tokens"></a>Tokens ophalen

### <a name="via-msal"></a>Via MSAL

MSAL staat apps toe om tokens op de achtergrond en interactief te verkrijgen. U hoeft deze methoden alleen aan te roepen en MSAL retourneert een toegangs token voor de aangevraagde bereiken. Het juiste patroon is om een stille aanvraag uit te voeren en terug te vallen op een interactieve aanvraag.

#### <a name="android"></a>Android

```Java
String[] SCOPES = {"https://graph.microsoft.com/.default"};
PublicClientApplication sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    R.raw.auth_config);

// Check if there are any accounts we can sign in silently.
// Result is in the silent callback (success or error).
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {

        if (accounts.isEmpty() && accounts.size() == 1) {
            // TODO: Create a silent callback to catch successful or failed request.
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts or > 1 account. */
        }
    }
});    

[...]

// No accounts found. Interactively request a token.
// TODO: Create an interactive callback to catch successful or failed request.
sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());        
```

#### <a name="ios"></a>iOS

```swift
// Initialize the app.
guard let authorityURL = URL(string: kAuthority) else {
    self.loggingText.text = "Unable to create authority URL"
    return
}
let authority = try MSALAADAuthority(url: authorityURL)
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)

// Get tokens.
let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
applicationContext.acquireTokenSilent(with: parameters) { (result, error) in
    if let error = error {
        let nsError = error as NSError

        // interactionRequired means you need to ask the user to sign in. This usually happens
        // when the user's refresh token is expired or when the user has changed the password,
        // among other possible reasons.
        if (nsError.domain == MSALErrorDomain) {
            if (nsError.code == MSALError.interactionRequired.rawValue) {    
                DispatchQueue.main.async {
                    guard let applicationContext = self.applicationContext else { return }
                    let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
                    applicationContext.acquireToken(with: parameters) { (result, error) in
                        if let error = error {
                            self.updateLogging(text: "Could not acquire token: \(error)")
                            return
                        }

                        guard let result = result else {
                            self.updateLogging(text: "Could not acquire token: No result returned")
                            return
                        }
                        
                        // Token is ready via interaction!
                        self.accessToken = result.accessToken
                    }
                }
                return
            }
        }

        self.updateLogging(text: "Could not acquire token silently: \(error)")
        return
    }
    guard let result = result else {
        self.updateLogging(text: "Could not acquire token: No result returned")
        return
    }

    // Token is ready via silent acquisition.
    self.accessToken = result.accessToken
}
```

#### <a name="xamarin"></a>Xamarin

In het volgende voor beeld wordt de minimale code weer gegeven voor het interactief ophalen van een token voor het lezen van het profiel van de gebruiker met Microsoft Graph.

```CSharp
string[] scopes = new string[] {"user.read"};
var app = PublicClientApplicationBuilder.Create(clientId).Build();
var accounts = await app.GetAccountsAsync();
AuthenticationResult result;
try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
             .ExecuteAsync();
}
catch(MsalUiRequiredException)
{
 result = await app.AcquireTokenInteractive(scopes)
             .ExecuteAsync();
}
```

### <a name="mandatory-parameters"></a>Verplichte para meters

`AcquireTokenInteractive`heeft slechts één verplichte para ``scopes``meter, die een opsomming bevat van teken reeksen die de bereiken definiëren waarvoor een token is vereist. Als het token voor de Microsoft Graph is, kunt u de vereiste bereiken vinden in API-verwijzing van elke micro soft Graph API in de sectie met de naam ' permissions '. Als u bijvoorbeeld [de contact personen van de gebruiker wilt weer geven](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/user_list_contacts), moet de scope ' gebruiker. read ', ' Contacts. read ' worden gebruikt. Zie ook [Microsoft Graph permissions Reference](https://developer.microsoft.com/graph/docs/concepts/permissions_reference)(Engelstalig).

Als u deze niet hebt opgegeven bij het maken van de app op Android, moet u ook de bovenliggende activiteit (met behulp `.WithParentActivityOrWindow`van hieronder) opgeven, zodat het token na de interactie terugkeert naar die bovenliggende activiteit. Als u deze niet opgeeft, wordt er een uitzonde ring gegenereerd `.ExecuteAsync()`bij het aanroepen van.

### <a name="specific-optional-parameters"></a>Specifieke optionele para meters

#### <a name="withprompt"></a>WithPrompt

`WithPrompt()`wordt gebruikt om de interactiviteit met de gebruiker te beheren door een prompt op te geven

<img src="https://user-images.githubusercontent.com/13203188/53438042-3fb85700-39ff-11e9-9a9e-1ff9874197b3.png" width="25%" />

De klasse definieert de volgende constanten:

- ``SelectAccount``: Hiermee wordt de STS gedwongen het dialoog venster voor account selectie te presen teren met accounts waarvoor de gebruiker een sessie heeft. Deze optie is handig wanneer ontwikkel aars van toepassingen willen laten kiezen uit verschillende identiteiten. Deze optie verstuurt MSAL ``prompt=select_account`` om naar de ID-provider te verzenden. Deze optie is de standaard instelling en het is een goede taak om de best mogelijke ervaring te bieden op basis van de beschik bare informatie (account, aanwezigheid van een sessie voor de gebruiker, enzovoort). ...). Wijzig deze alleen als u er geen goede reden voor hebt.
- ``Consent``: Hiermee kan de ontwikkelaar van de toepassing afdwingen dat de gebruiker om toestemming wordt gevraagd, zelfs als toestemming werd verleend. In dit geval verzendt `prompt=consent` MSAL naar de ID-provider. Deze optie kan worden gebruikt in sommige toepassingen met een gerichte beveiliging waarbij het beheer van de organisatie vereist dat de gebruiker het dialoog venster voor toestemming wordt weer gegeven telkens wanneer de toepassing wordt gebruikt.
- ``ForceLogin``: de ontwikkelaar van de toepassing kan de gebruiker door de service om referenties wordt gevraagd, zelfs als deze prompt niet nodig is. Deze optie kan nuttig zijn als het ophalen van een token mislukt, zodat de gebruiker zich opnieuw aanmeldt. In dit geval verzendt `prompt=login` MSAL naar de ID-provider. We hebben de IT-afdeling weer gegeven in sommige toepassingen met een gerichte beveiliging waarbij het beheer van de organisatie vereist dat de gebruiker zich opnieuw aanmeldt, telkens wanneer ze toegang hebben tot specifieke delen van een toepassing.
- ``Never``(alleen voor .NET 4,5 en WinRT) wordt de gebruiker niet gevraagd, maar probeert in plaats daarvan de cookie te gebruiken die is opgeslagen in de verborgen Inge sloten webweergave (zie hieronder): Webweergaven in MSAL.NET). Het gebruik van deze optie kan mislukken en in dat `AcquireTokenInteractive` geval wordt een uitzonde ring gegenereerd om aan te geven dat een UI-interactie nodig is. u `Prompt` moet een andere para meter gebruiken.
- ``NoPrompt``: Er wordt geen prompt verzonden naar de ID-provider. Deze optie is alleen nuttig voor het Azure AD B2C bewerken van profiel beleid (Zie [B2C-specifiek](https://aka.ms/msal-net-b2c-specificities)).

#### <a name="withextrascopetoconsent"></a>WithExtraScopeToConsent

Deze wijzigings functie wordt gebruikt in een geavanceerd scenario waarbij de gebruiker vooraf toestemming moet geven aan verschillende bronnen (en niet de incrementele toestemming wilt gebruiken, die normaal gesp roken wordt gebruikt met MSAL.NET/de micro soft Identity platform v 2.0). Zie [How to: de gebruiker vooraf toestemming geven voor verschillende bronnen](scenario-desktop-production.md#how-to-have--the-user-consent-upfront-for-several-resources)voor meer informatie.

```CSharp
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

#### <a name="other-optional-parameters"></a>Andere optionele para meters

Meer informatie over de andere optionele para meters `AcquireTokenInteractive` voor in de referentie documentatie voor [AcquireTokenInteractiveParameterBuilder](/dotnet/api/microsoft.identity.client.acquiretokeninteractiveparameterbuilder?view=azure-dotnet-preview#methods)

### <a name="via-the-protocol"></a>Via het Protocol

Het is niet raadzaam het protocol rechtstreeks te gebruiken. Als u dat wel doet, ondersteunt de app geen enkele eenmalige aanmelding (SSO), Apparaatbeheer en scenario's voor voorwaardelijke toegang.

Wanneer u het protocol gebruikt om tokens voor mobiele apps op te halen, moet u twee aanvragen doen: een autorisatie code ophalen en deze voor een token uitwisselen.

#### <a name="get-authorization-code"></a>Autorisatiecode ophalen

```Text
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=<CLIENT_ID>
&response_type=code
&redirect_uri=<ENCODED_REDIRECT_URI>
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2F.default
&state=12345
```

#### <a name="get-access-and-refresh-token"></a>Token voor toegang en vernieuwing ophalen

```Text
POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=<CLIENT_ID>
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=<ENCODED_REDIRECT_URI>
&grant_type=authorization_code
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een web-API aanroepen](scenario-mobile-call-api.md)
