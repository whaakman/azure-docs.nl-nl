---
title: Aan de slag met iOS - Microsoft identity-platform | Azure
description: Hoe iOS (Swift)-toepassingen met een API kunnen aanroepen die is vereist toegangstokens met behulp van Microsoft identity-platform
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06b4ecc151f8402bfb8f05968475b0305737ecd9
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406484"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-app"></a>Meld u aan gebruikers en de Microsoft Graph aanroepen vanuit een iOS-app

In deze zelfstudie leert u hoe u een iOS-toepassing bouwen en integreren met Microsoft identity-platform. Deze app wordt specifiek, meld u aan een gebruiker, een toegangstoken voor het aanroepen van de Microsoft Graph API en maken van een algemene aanvraag naar de Microsoft Graph API.  

Wanneer u de handleiding hebt voltooid, wordt uw toepassing accepteert aanmeldingen van persoonlijke Microsoft-accounts (waaronder outlook.com, live.com en anderen) en werk of school-accounts van een bedrijf of organisatie die gebruikmaakt van Azure Active Directory.

## <a name="how-this-guide-works"></a>De werking van deze handleiding

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze zelfstudie werkt](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

De app in dit voorbeeld wordt Meld u aan gebruikers en gegevens ophalen in hun naam.  Deze gegevens kan worden bereikt via een beveiligde API (Microsoft Graph-API in dit geval) die is autorisatie vereist en ook wordt beveiligd door Microsoft identity-platform.

Met name:

* Uw app, wordt de gebruiker via een browser of de Microsoft Authenticator aanmelden.
* De eindgebruiker accepteert de machtigingen die uw toepassing heeft aangevraagd. 
* Uw app worden een toegangstoken uitgegeven voor de Microsoft Graph API.
* Het toegangstoken worden opgenomen in de HTTP-aanvraag naar de web-API.
* Het antwoord van de Microsoft Graph worden verwerkt.

In dit voorbeeld maakt gebruik van de Microsoft Authentication library (MSAL) voor het implementeren van Auth. MSAL wordt automatisch vernieuwen van tokens, eenmalige aanmelding tussen andere apps op het apparaat leveren en beheren van het account (s).

## <a name="prerequisites"></a>Vereisten

- XCode-versie 10.x is vereist voor het voorbeeld dat is gemaakt in deze handleiding. U kunt downloaden XCode uit de [iTunes website](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode downloaden URL").
- Microsoft Authentication Library ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). U kunt afhankelijkheidsbeheerder gebruiken of handmatig toevoegen. Er is met de sectie hieronder [informatie](#add-msal). 

## <a name="set-up-your-project"></a>Uw project instellen

In deze zelfstudie wordt een nieuw project maken. Als u wilt downloaden van de voltooide zelfstudie in plaats daarvan [downloaden van de code](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip).

### <a name="create-a-new-project"></a>Een nieuw project maken

1. Xcode openen en selecteer **maken van een nieuwe Xcode-project**.
2. Selecteer **iOS > één weergave toepassing** en selecteer **volgende**.
3. Geef een productnaam en selecteer **volgende**.
4. Selecteer een map te maken van uw app en op *maken*.

## <a name="register-your-application"></a>Uw toepassing registreren 

U kunt uw toepassing registreren op twee manieren, zoals beschreven in de volgende twee secties.

### <a name="register-your-app"></a>Uw app registreren

1. Ga naar de [Azure-portal](https://aka.ms/MobileAppReg) > Selecteer `New registration`. 
2. Voer een **naam** voor uw app > `Register`. **Stel een omleidings-URI niet in deze fase**. 
3. In de `Manage` sectie, gaat u naar `Authentication` > `Add a platform` > `iOS`
    - Voer van uw project bundel-ID. Als u de code hebt gedownload, is dit `com.microsoft.identitysample.MSALiOS`.
4. Raak `Configure` en op te slaan de `MSAL Configuration` voor later. 

## <a name="add-msal"></a>MSAL toevoegen

### <a name="get-msal"></a>MSAL ophalen

#### <a name="cocoapods"></a>CocoaPods

U kunt [CocoaPods](https://cocoapods.org/) voor het installeren van `MSAL` door toe te voegen aan uw `Podfile` onder doel:

```
use_frameworks!

target '<your-target-here>' do
   pod 'MSAL', '~> 0.4.0'
end
```

#### <a name="carthage"></a>Carthage

U kunt [Carthage](https://github.com/Carthage/Carthage) voor het installeren van `MSAL` door toe te voegen aan uw `Cartfile`: 

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

#### <a name="manually"></a>Handmatig

U kunt ook Git Submodule gebruiken of bekijk de meest recente versie en gebruik als framework in uw toepassing.

### <a name="add-your-app-registration"></a>De registratie van uw app toevoegen

Voeg vervolgens de registratie van uw app toe aan uw code. Voeg uw ***Client / toepassings-ID*** naar `ViewController.swift`:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
let kAuthority = "https://login.microsoftonline.com/common"
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
```

### <a name="configure-url-schemes"></a>URL-schema's configureren

Registreren `CFBundleURLSchemes` om toe te staan een retouraanroep, zodat de gebruiker terug naar de app kan worden omgeleid na het aanmelden.

`LSApplicationQueriesSchemes` u kunt met behulp van uw app in de Microsoft Authenticator gebruiken indien beschikbaar. 

U doet dit door open `Info.plist` als een bron code en voeg het volgende toe, waarbij de ***BUNDLE_ID*** met wat u hebt geconfigureerd in Azure portal.

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauth</string>
    <string>msauthv2</string>
</array>
```

### <a name="import-msal"></a>MSAL importeren

Importeren MSAL framework in `ViewController.swift` en `AppDelegate.swift` bestanden:

```swift
import MSAL
```

## <a name="create-your-apps-ui"></a>De gebruikersinterface van uw app maken

Voor deze zelfstudie die u wilt maken:

- Knop voor Graph API aanroepen
- Knop Afmelden
- Logboekregistratie textview

In `ViewController.swift`, eigenschappen definiëren en `initUI()` als volgt:

```swift

var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = UIButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
        callGraphButton.setTitleColor(.blue, for: .normal)
        callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
        callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
        // Add sign out button
        signOutButton = UIButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.setTitle("Sign Out", for: .normal)
        signOutButton.setTitleColor(.blue, for: .normal)
        signOutButton.setTitleColor(.gray, for: .disabled)
        signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = UITextView()
        loggingText.isUserInteractionEnabled = false
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: 10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
    }
```

Voeg vervolgens toe aan `viewDidLoad()` van ViewController.swift:

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        do {
            try self.initMSAL()
        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }
```

## <a name="use-msal"></a>Gebruik MSAL

### <a name="initialize-msal"></a>MSAL initialiseren

U moet eerst maken een `MSALPublicClientApplication` met een exemplaar van `MSALPublicClientConfiguration` voor uw toepassing:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.loggingText.text = "Unable to create authority URL"
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
    }
```

### <a name="handle-the-callback"></a>De Callback verwerken

Voor het afhandelen van de callback na het aanmelden, toevoegen `MSALPublicClientApplication.handleMSALResponse` in `appDelegate`:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

#### <a name="acquire-tokens"></a>Tokens verkrijgen

Nu kunnen we de gebruikersinterface van de toepassing verwerking van logica en ophalen van tokens interactief via MSAL implementeren. 

MSAL bevat twee primaire methoden voor het ophalen van tokens: `acquireTokenSilently` en `acquireTokenInteractively`.  

`acquireTokenSilently()` probeert een gebruiker aanmelden en ophalen van tokens zonder tussenkomst van de gebruiker als een account aanwezig is.

`acquireTokenInteractively` de gebruikersinterface wordt altijd weergegeven wanneer u probeert de gebruiker aanmelden en ophalen van tokens. het kan echter sessiecookies in de browser of een account in de Microsoft authenticator gebruiken om te geven van een interactieve SSO-ervaring. 

```swift
    @objc func callGraphAPI(_ sender: UIButton) {
        
        guard let currentAccount = self.currentAccount() else {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            acquireTokenInteractively()
            return
        }
        
        acquireTokenSilently(currentAccount)
    }

    func currentAccount() -> MSALAccount? {
        
        guard let applicationContext = self.applicationContext else { return nil }
        
        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        
        do {
            let cachedAccounts = try applicationContext.allAccounts()
            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }
        } catch let error as NSError {
            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }
        
        return nil
    }
```

#### <a name="get-a-token-interactively"></a>Interactief een token verkrijgen

Als u wilt een token verkrijgen voor de eerste keer, moet u maken een `MSALInteractiveTokenParameters` en roep `acquireToken`.

1. Maak `MSALInteractiveTokenParameters` met een bereik.
2. Bel `acquireToken` met de parameters die zijn gemaakt.
3. Fout dienovereenkomstig afhandelen. Voor meer informatie raadpleegt u de [iOS foutafhandeling handleiding](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Error-Handling).
4. De geslaagde aanvraag worden verwerkt. 

```swift
    func acquireTokenInteractively() {
   
        guard let applicationContext = self.applicationContext else { return }
     // #1    
        let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
     // #2        
        applicationContext.acquireToken(with: parameters) { (result, error) in
     // #3            
            if let error = error {
                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }
            guard let result = result else {   
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
     // #4            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```



#### <a name="get-a-token-silently"></a>Op de achtergrond een token verkrijgen

Een bijgewerkte om token te verkrijgen op de achtergrond, moet u maken een `MSALSilentTokenParameters` en roep `acquireTokenSilent`:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        guard let applicationContext = self.applicationContext else { return }
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in    
            if let error = error {
                let nsError = error as NSError
                if (nsError.domain == MSALErrorDomain) {
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
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
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>De Microsoft Graph API aanroepen 

Zodra u een token via hebt `self.accessToken`, uw app kan deze waarde gebruiken in de HTTP-header waarmee een gemachtigde aanvraag naar de Microsoft Graph:

| Header-sleutel    | value                 |
| ------------- | --------------------- |
| Autorisatie | Bearer < toegangstoken > |

Het volgende toevoegen aan `ViewController.swift`:

```swift
    func getContentWithToken() {        
        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
               
        URLSession.shared.dataTask(with: request) { data, response, error in
               
        if let error = error {
            self.updateLogging(text: "Couldn't get graph result: \(error)")
            return
        }
               
        guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
               
        self.updateLogging(text: "Couldn't deserialize result JSON")
            return
        }
               
        self.updateLogging(text: "Result from Graph: \(result))")
        
        }.resume()
    }
```

Meer informatie over de [Microsoft Graph API](https://graph.microsoft.com)

### <a name="use-msal-for-sign-out"></a>Gebruik MSAL voor afmelding

Vervolgens, wordt ondersteuning voor toegevoegd aan onze app afmelden. 

Het is belangrijk te weten, afmelding met MSAL verwijdert alle bekende informatie over een gebruiker uit deze toepassing, maar de gebruiker heeft nog steeds een actieve sessie op hun apparaat. Als de gebruiker probeert aan te melden bij het opnieuw ze interactie mogelijk te zien, maar mogelijk niet naar hun referenties vanwege de apparaat-sessie actief opnieuw invoeren. 

Als u wilt toevoegen afmelden, kopieert u de volgende methode in uw `ViewController.swift`:

```swift 
    @objc func signOut(_ sender: UIButton) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache
             */
            
            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signOutButton.isEnabled = false
            
        } catch let error as NSError {
            
            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }
```

### <a name="enable-token-caching"></a>Token in cache opslaan inschakelen

Standaard caches MSAL van uw app-tokens in de iOS-sleutelhanger. 

Om in te schakelen token in cache opslaan, gaat u naar de instellingen van uw Xcode-Project > `Capabilities tab`  >  `Enable Keychain Sharing` > klikt u op `Plus` > Enter **com.microsoft.adalcache**.

### <a name="add-helper-methods"></a>Help-methoden toevoegen

Voeg deze Help-methoden voor het voltooien van het voorbeeld:

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
```

### <a name="multi-account-applications"></a>Account voor meerdere toepassingen

Deze app is gebouwd voor een scenario met één account. MSAL ondersteunt ook meerdere account-scenario's, maar hiervoor extra werk van apps. U moet de gebruikersinterface van de gebruiker selecteren welk account moet worden gebruikt voor elke actie waarvoor tokens te maken. U kunt ook uw app kunt implementeren om een heuristiek om te selecteren welk account moet worden gebruikt de `getAllAccounts(...)` methode.

## <a name="test-your-app"></a>Uw app testen

### <a name="run-locally"></a>Lokaal uitvoeren

Als u de bovenstaande code hebt gevolgd, probeert te bouwen en implementeren van de app op een testapparaat of emulator. U moet zich aanmelden en tokens verkrijgen voor Azure AD of persoonlijke Microsoft-accounts. Deze app verschijnt nadat een gebruiker die zich, de gegevens die zijn geretourneerd door de Microsoft Graph `/me` eindpunt. 

Als u problemen ondervindt, gerust een probleem in dit document of in de bibliotheek MSAL openen en laat het ons weten. 

### <a name="consent-to-your-app"></a>Toestemming geven voor uw app

De eerste keer dat een gebruiker zich in uw app, wordt ze gevraagd door Microsoft identity akkoord gaan met de machtigingen die zijn aangevraagd.  Hoewel de meeste gebruikers geschikt voor stemt ermee in dat zijn, zijn sommige Azure AD-tenants toestemming van de gebruiker - beheerders om in te stemmen namens alle gebruikers waarvoor uitgeschakeld.  Ter ondersteuning van dit scenario, zorg dat voor het registreren van uw app bereiken in de Azure-portal.

## <a name="help-and-support"></a>Help en ondersteuning

Problemen met deze zelfstudie of met het Microsoft identity-platform had? Zie [Help en ondersteuning](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options)

