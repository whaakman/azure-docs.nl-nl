---
title: Microsoft identity platform iOS quickstart | Azure
description: Meer informatie over het aanmelden gebruikers- en Microsoft Graph-query in een iOS-toepassing.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: brandwe
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b3802d8f92913e416cc6a80f899179fde80cec30
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65962588"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app"></a>Quickstart: Gebruikers aanmelden en de Microsoft Graph API aanroepen vanuit een iOS-app

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Deze snelstart bevat een codevoorbeeld die u laat zien hoe een systeemeigen iOS-toepassing persoonlijke, werk- en schoolaccounts kan aanmelden, een toegangstoken kan ophalen en de Microsoft Graph API kan aanroepen.

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze Quick Start werkt](media/quickstart-v2-ios/ios-intro.svg)

> [!NOTE]
> **Vereisten**
> * XCode 10 +
> * iOS 10+ 

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>De quickstart-app registreren en downloaden
> U hebt twee opties voor het starten van de snelstarttoepassing:
> * [Express] [Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Handmatig] [Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Uw app registreren
> 1. Ga naar de nieuwe [Azure portal - App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/IosQuickstartPage/sourceType/docs) deelvenster.
> 1. Voer een naam in voor de toepassing en selecteer **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:
>
> 1. Navigeer naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://aka.ms/MobileAppReg) pagina.
> 1. Selecteer **registratie van nieuwe**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>      - In de **naam** sectie, voer de toepassingsnaam van een zinvolle die zal worden getoond aan gebruikers van de app wanneer ze zich aanmelden of toestemming voor uw app, bijvoorbeeld geven `iOSQuickstart`.
>      - Andere configuraties overslaan op deze pagina. 
>      - Klik op de `Register` knop.
> 1. Klik op de nieuwe app > Ga naar `Authentication`  >  `Add Platform`  >  `iOS`.    
>      - Voer de ***bundel-id*** voor uw toepassing. 
> 1. Selecteer `Configure` en sla de ***MSAL configuratie*** details voor later. 

> [!div renderon="portal" class="sxs-lookup"]
>
> #### <a name="step-1-configure-your-application"></a>Stap 1: Uw toepassing configureren
> Voor het codevoorbeeld voor deze Quick Start om te werken, moet u een omleidings-URI die compatibel is met de broker Auth toevoegen. 
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging voor mij maken]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-ios/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken

#### <a name="step-2-download-your-web-server-or-project"></a>Stap 2: Uw webserver of project downloaden

- [De voorbeeldcode downloaden](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Stap 3: Het project configureren

> [!div renderon="docs"]
> Als u de optie 1 hierboven hebt geselecteerd, kunt u deze stappen overslaan. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Pak het zip-bestand uit en open het project in XCode.
> 1. Bewerk **ViewController.swift** en vervang de regel die begint met 'let kClientID' door het volgende codefragment:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_here"
>    let kAuthority = "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
>
>    ```
> 1. Klik met de rechtermuisknop **Info.plist** en selecteer **openen als** > **broncode**.
> 1. Vervang onder het hoofdknooppunt dict door uw ***bundel-Id***:
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.Enter_the_Bundle_Id_Here</string>
>          </array>
>       </dict>
>    </array>
> 
>    ```
> 1. Bouwen en uitvoeren van de app! 

> [!div renderon="docs"]
>
> 1. Pak het zip-bestand uit en open het project in XCode.
> 1. Bewerken **ViewController.swift** en vervang de regel die begint met 'laten kClientID' met het volgende codefragment:
>
>    ```swift
>    let kClientID = "<ENTER_YOUR_APPLICATION/CLIENT_ID>"
> 
>    ```
> 1. Klik met de rechtermuisknop **Info.plist** en selecteer **openen als** > **broncode**.
> 1. Vervang onder het hoofdknooppunt dict door uw ***bundel-Id***:
>
>    ```xml
>    <key>CFBundleURLTypes</key>
>    <array>
>       <dict>
>          <key>CFBundleURLSchemes</key>
>          <array>
>             <string>msauth.<ENTER_YOUR_BUNDLE_ID></string>
>          </array>
>       </dict>
>    </array>
>
>    ```
> 1. Bouwen en uitvoeren van de app! 

## <a name="more-information"></a>Meer informatie

Lees deze secties voor meer informatie over deze snelstart.

### <a name="getting-msal"></a>MSAL ophalen

MSAL ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)) is de bibliotheek gebruikt voor het aanmelden van gebruikers en aanvragen van tokens die worden gebruikt voor toegang tot een API die wordt beveiligd door Microsoft identity-platform. U kunt MSAL toevoegen aan uw toepassing met behulp van het volgende proces:

```
$ vi Podfile

```
Voeg het volgende toe aan de podfile (met doel van uw project):

```
use_frameworks!

target 'MSALiOS' do
   pod 'MSAL', '~> 0.4.0'
end

```

### <a name="msal-initialization"></a>MSAL initialiseren

U kunt de verwijzing voor MSAL toevoegen door de volgende code toe te voegen:

```swift
import MSAL
```

Vervolgens initialiseert u MSAL met de volgende code:

```swift
let authority = try MSALAADAuthority(url: URL(string: kAuthority)!)
            
let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)

```

> |Waar: ||
> |---------|---------|
> | `clientId` | De toepassings-id van de toepassing die is geregistreerd in *portal.azure.com* |
> | `authority` | Het eindpunt van Microsoft identity-platform. In de meeste gevallen is dit *https<span/>://login.microsoftonline.com/common* |
> | `redirectUri` | De omleidings-URI van de toepassing. U kunt doorgeven 'nil' als de standaardwaarde of uw aangepaste omleidings-URI wilt gebruiken. |

### <a name="additional-app-requirements"></a>Als u meer App-vereisten  

Uw app moet ook beschikken over de volgende uw `AppDelegate`. Hiermee kunt MSAL SDK afhandelt token reactie van de broker-app voor verificatie bij het uitvoeren van verificatie.

 ```swift
 func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
         guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
             return false
         }
         
         return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
     }

```

Ten slotte uw app moet heeft een `LSApplicationQueriesSchemes` vermelding in uw ***Info.plist*** naast de `CFBundleURLTypes`. Het voorbeeld wordt geleverd met dit opgenomen. 

   ```xml 
   <key>LSApplicationQueriesSchemes</key>
   <array>
      <string>msauth</string>
      <string>msauthv2</string>
   </array>
   ```

### <a name="sign-in-users--request-tokens"></a>Meld u aan gebruikers en tokens aanvragen

MSAL biedt twee methoden voor het verkrijgen van tokens: `acquireToken` en `acquireTokenSilent`.

#### <a name="acquiretoken-getting-a-token-interactively"></a>acquireToken: Ophalen van een token interactief

In sommige situaties moet gebruikers kunnen communiceren met Microsoft identity-platform. In dergelijke gevallen de eindgebruiker mogelijk vereist zijn voor hun account selecteren of toestemming geven voor uw app machtigingen zijn referenties invoeren. Bijvoorbeeld: 

* De eerste keer dat gebruikers zich aanmelden bij de toepassing
* Als een gebruiker hun wachtwoord opnieuw instellen, moet ze hun referenties invoeren 
* Wanneer uw toepassing toegang aanvraagt tot een resource voor de eerste keer
* Als MFA of andere beleidsregels voor voorwaardelijke toegang vereist zijn

```swift
let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
applicationContext.acquireToken(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Waar:||
> |---------|---------|
> | `scopes` | Bevat de bereiken die worden aangevraagd (dat wil zeggen, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste Web-API's (`api://<Application ID>/access_as_user`) |

#### <a name="acquiretokensilent-getting-an-access-token-silently"></a>acquireTokenSilent: Een toegangstoken op de achtergrond verkrijgen

Apps al dan niet mogen moeten gebruikers melden telkens wanneer ze een token aanvragen. Als de gebruiker al heeft aangemeld, kan deze methode apps aanvragen tokens op de achtergrond. 

```swift
let parameters = MSALSilentTokenParameters(scopes: kScopes, account: applicationContext.allAccounts().first)
applicationContext.acquireTokenSilent(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Waar: ||
> |---------|---------|
> | `scopes` | Bevat de bereiken die worden aangevraagd (dat wil zeggen, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste Web-API's (`api://<Application ID>/access_as_user`) |
> | `account` | Voor wordt het account een token aangevraagd. In deze snelstartgids is een toepassing één account, als u maken van een app met meerdere account wilt u de logica moet, voor het identificeren van welk account moet worden gebruikt voor token aanvragen definiëren `applicationContext.account(forHomeAccountId: self.homeAccountId)` |

## <a name="next-steps"></a>Volgende stappen

Probeer uit de iOS-zelfstudie voor een volledige stapsgewijze handleiding voor het bouwen van toepassingen, met inbegrip van een volledige uitleg van deze quickstart.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>De stappen voor het maken van de toepassing die wordt gebruikt in deze snelstart

> [!div class="nextstepaction"]
> [iOS-zelfstudie voor Graph API-aanroepen](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]