---
title: Micro soft Identity platform iOS Quick Start | Azure
description: Meer informatie over het aanmelden van gebruikers en het opvragen van Microsoft Graph in een iOS-toepassing.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: twhitney
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e6a9dab8a2a37d7fa312b525453683fe789269b
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852935"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-ios-app"></a>Quickstart: Gebruikers aanmelden en de Microsoft Graph API aanroepen vanuit een iOS-app

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Deze snelstart bevat een codevoorbeeld die u laat zien hoe een systeemeigen iOS-toepassing persoonlijke, werk- en schoolaccounts kan aanmelden, een toegangstoken kan ophalen en de Microsoft Graph API kan aanroepen.

![Toont hoe de voor beeld-app die door deze Quick start is gegenereerd, werkt](media/quickstart-v2-ios/ios-intro.svg)

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
> Als u uw app wilt registreren,
> 1. Ga naar het deel venster nieuwe [Azure Portal-app-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/IosQuickstartPage/sourceType/docs) .
> 1. Voer een naam in voor de toepassing en selecteer **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
> Volg deze stappen om de toepassing te registreren en de registratiegegevens van de app handmatig toe te voegen aan uw oplossing:
>
> 1. Navigeer naar de pagina micro soft-identiteits platform voor ontwikkel aars [app-registraties](https://aka.ms/MobileAppReg) .
> 1. Selecteer **nieuwe registratie**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>      - Voer in de sectie **naam** een zinvolle toepassings naam in die wordt weer gegeven voor gebruikers van de app wanneer deze zich aanmelden of toestemming voor uw app geven, `iOSQuickstart`bijvoorbeeld.
>      - Andere configuraties op deze pagina overs Laan. 
>      - Klik op `Register` de knop.
> 1. Klik op de nieuwe app > Ga naar `Authentication`.  >  `Add Platform`  >  `iOS`    
>      - Voer de ***bundel-id*** in voor uw toepassing. 
> 1. Selecteer `Configure` de details van de ***MSAL-configuratie*** en sla deze op voor later. 

> [!div renderon="portal" class="sxs-lookup"]
>
> #### <a name="step-1-configure-your-application"></a>Stap 1: Uw toepassing configureren
> Het code voorbeeld voor deze Quick Start werkt alleen als u een omleidings-URI toevoegt die compatibel is met de auth Broker. 
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijziging voor mij maken]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-ios/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken

#### <a name="step-2-download-your-web-server-or-project"></a>Stap 2: Uw webserver of project downloaden

- [Het code voorbeeld downloaden](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip)

#### <a name="step-3-configure-your-project"></a>Stap 3: Het project configureren

> [!div renderon="docs"]
> Als u optie 1 hierboven hebt geselecteerd, kunt u deze stappen overs Laan. 

> [!div renderon="portal" class="sxs-lookup"]
> 1. Pak het zip-bestand uit en open het project in XCode.
> 1. Bewerk **ViewController.swift** en vervang de regel die begint met 'let kClientID' door het volgende codefragment:
>    ```swift
>    let kClientID = "Enter_the_Application_Id_here"
>    let kAuthority = "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
>    ``` 
> 1. Klik met de rechter muisknop op **info. plist** en selecteer **openen als** > **bron code**.
> 1. Vervang onder het hoofd knooppunt dict door de ***bundel-id***:
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
> 1. Bouw & de app uit te voeren. 

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > Deze Quick Start biedt ondersteuning voor Enter_the_Supported_Account_Info_Here.

> [!div renderon="docs"]
>
> 1. Pak het zip-bestand uit en open het project in XCode.
> 1. Bewerk **View Controller. Swift** en vervang de regel die begint met ' laat kClientID ' door het volgende code fragment:
>
>    ```swift
>    let kClientID = "<ENTER_YOUR_APPLICATION/CLIENT_ID>"
> 
>    ```
> 1. Klik met de rechter muisknop op **info. plist** en selecteer **openen als** > **bron code**.
> 1. Vervang onder het hoofd knooppunt dict door de ***bundel-id***:
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
> 1. Bouw & de app uit te voeren. 

## <a name="more-information"></a>Meer informatie

Lees deze secties voor meer informatie over deze snelstart.

### <a name="getting-msal"></a>MSAL ophalen

MSAL ([MSAL. Framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)) is de bibliotheek die wordt gebruikt voor het aanmelden van gebruikers en het aanvragen van tokens die worden gebruikt om toegang te krijgen tot een API die wordt beveiligd door micro soft Identity platform. U kunt MSAL toevoegen aan uw toepassing met behulp van het volgende proces:

```
$ vi Podfile

```
Voeg het volgende toe aan deze podfile (met het doel van het project):

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
> | `authority` | Het micro soft Identity platform-eind punt. In de meeste gevallen is dit *https<span/>://login.microsoftonline.com/common* |
> | `redirectUri` | De omleidings-URI van de toepassing. U kunt Nil door geven om de standaard waarde of uw aangepaste omleidings-URI te gebruiken. |

### <a name="additional-app-requirements"></a>Aanvullende app-vereisten  

Uw app moet ook het volgende hebben in uw `AppDelegate`. Hiermee kan de MSAL SDK-reactie van de auth Broker-app worden verwerkt wanneer u verificatie uitvoert.

 ```swift
 func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
         guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
             return false
         }
         
         return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
     }

```

Ten slotte moet uw app een `LSApplicationQueriesSchemes` vermelding hebben in uw ***info. plist*** naast de `CFBundleURLTypes`. Dit is inclusief het voor beeld. 

   ```xml 
   <key>LSApplicationQueriesSchemes</key>
   <array>
      <string>msauth</string>
      <string>msauthv2</string>
   </array>
   ```

### <a name="sign-in-users--request-tokens"></a>Aanmeldings tokens voor gebruikers & aanvragen

MSAL biedt twee methoden voor het verkrijgen van tokens: `acquireToken` en `acquireTokenSilent`.

#### <a name="acquiretoken-getting-a-token-interactively"></a>acquireToken: Een token interactief ophalen

In sommige situaties moeten gebruikers communiceren met het micro soft Identity-platform. In dergelijke gevallen kan de eind gebruiker verplicht zijn of haar account selecteren, hun referenties invoeren of toestemming geven voor de machtigingen van uw app. Bijvoorbeeld: 

* De eerste keer dat gebruikers zich aanmelden bij de toepassing
* Als een gebruiker het wacht woord opnieuw instelt, moeten de referenties worden ingevoerd 
* Wanneer uw toepassing voor de eerste keer toegang tot een resource vraagt
* Wanneer MFA of een ander beleid voor voorwaardelijke toegang is vereist

```swift
let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
applicationContext.acquireToken(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Waar:||
> |---------|---------|
> | `scopes` | Bevat de bereiken die worden aangevraagd (dat wil zeggen `[ "user.read" ]` , voor Microsoft Graph `[ "<Application ID URL>/scope" ]` of voor aangepaste web-`api://<Application ID>/access_as_user`api's () |

#### <a name="acquiretokensilent-getting-an-access-token-silently"></a>acquireTokenSilent: Een toegangstoken op de achtergrond verkrijgen

Voor apps mogen gebruikers zich niet elke keer aanmelden wanneer ze een token aanvragen. Als de gebruiker al is aangemeld, kunnen apps tokens op de achtergrond aanvragen. 

```swift
let parameters = MSALSilentTokenParameters(scopes: kScopes, account: applicationContext.allAccounts().first)
applicationContext.acquireTokenSilent(with: parameters) { (result, error) in /* Add your handling logic */}
```

> |Waar: ||
> |---------|---------|
> | `scopes` | Bevat de bereiken die worden aangevraagd (dat wil zeggen `[ "user.read" ]` , voor Microsoft Graph `[ "<Application ID URL>/scope" ]` of voor aangepaste web-`api://<Application ID>/access_as_user`api's () |
> | `account` | Het account waarvoor een token wordt aangevraagd. Deze Quick start is een enkele account toepassing. Als u een app met meerdere accounts wilt maken, moet u de logica definiëren om te bepalen welk account moet worden gebruikt voor token aanvragen`applicationContext.account(forHomeAccountId: self.homeAccountId)` |

## <a name="next-steps"></a>Volgende stappen

Probeer de iOS-zelf studie voor een volledige stapsgewijze hand leiding voor het bouwen van toepassingen, inclusief een volledige uitleg van deze Snelstartgids.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>De stappen voor het maken van de toepassing die wordt gebruikt in deze snelstart

> [!div class="nextstepaction"]
> [iOS-zelfstudie voor Graph API-aanroepen](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-ios)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Help ons het micro soft Identity-platform te verbeteren. Vertel ons wat u denkt door een korte enquête met twee vragen te volt ooien.

> [!div class="nextstepaction"]
> [Micro soft Identity platform-enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)