---
title: Aan de slag met iOS - Microsoft identity-platform | Azure
description: Hoe iOS (Swift)-toepassingen met een API kunnen aanroepen die is vereist toegangstokens met behulp van Microsoft identity-platform
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.topic: tutorial
ms.workload: identity
ms.date: 07/15/2019
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 910069ab89cef18794e637b6bfbbc57fb732871c
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67872095"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-app"></a>Meld u aan gebruikers en de Microsoft Graph aanroepen vanuit een iOS-app

In deze zelfstudie leert u hoe u een iOS-app integreren met het Microsoft identity-platform. De app wordt een gebruiker, een toegangstoken voor het aanroepen van de Microsoft Graph API en te vragen de Microsoft Graph API.  

Wanneer u de handleiding hebt voltooid, wordt uw toepassing accepteert aanmeldingen van persoonlijke Microsoft-accounts (waaronder outlook.com, live.com en anderen) en werk of school-accounts van een bedrijf of organisatie die gebruikmaakt van Azure Active Directory.

## <a name="how-this-tutorial-works"></a>De werking van deze zelfstudie

![Laat zien hoe de voorbeeld-app die is gegenereerd door deze zelfstudie werkt](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

De app in deze zelfstudie wordt Meld u aan gebruikers en gegevens ophalen in hun naam.  Deze gegevens kan worden bereikt via een beveiligde API (Microsoft Graph-API in dit geval) die is autorisatie vereist en wordt beveiligd door de Microsoft identity-platform.

Met name:

* Uw app, wordt de gebruiker via een browser of de Microsoft Authenticator aanmelden.
* De eindgebruiker accepteert de machtigingen die uw toepassing heeft aangevraagd.
* Uw app worden een toegangstoken uitgegeven voor de Microsoft Graph API.
* Het toegangstoken worden opgenomen in de HTTP-aanvraag naar de web-API.
* Het antwoord van de Microsoft Graph worden verwerkt.

In dit voorbeeld maakt gebruik van de Microsoft Authentication library (MSAL) voor het implementeren van verificatie. MSAL wordt automatisch vernieuwen van tokens, eenmalige aanmelding (SSO) tussen de andere apps op het apparaat leveren en beheren van het account (s).

## <a name="prerequisites"></a>Vereisten

- XCode-versie 10.x is vereist voor het bouwen van de app in deze handleiding. U kunt downloaden XCode uit de [iTunes website](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode downloaden URL").
- Microsoft Authentication Library ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). U kunt een afhankelijkheidsbeheerder gebruiken of de tapewisselaar handmatig toevoegen. De onderstaande instructies laten u zien hoe.

In deze zelfstudie wordt een nieuw project maken. Als u wilt downloaden van de voltooide zelfstudie in plaats daarvan [downloaden van de code](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip).

## <a name="create-a-new-project"></a>Een nieuw project maken

1. Xcode openen en selecteer **maken van een nieuwe Xcode-project**.
2. Selecteer **iOS** > **één weergave App** en selecteer **volgende**.
3. Geef de productnaam van een.
4. Stel de **taal** naar **Swift** en selecteer **volgende**.
5. Selecteer een map te maken van uw app en op **maken**.

## <a name="register-your-application"></a>Uw toepassing registreren

1. Ga naar [Azure Portal](https://aka.ms/MobileAppReg)
2. Open de [blade App-registraties](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) en klikt u op **+ nieuwe registreren**.
3. Voer een **naam** voor uw app en vervolgens, zonder in te stellen van een omleidings-URI, klikt u op **registreren**.
4. In de **beheren** sectie van het deelvenster dat verschijnt, selecteer **verificatie**.
5. Klik op **probeer de nieuwe ervaring uit** in de buurt van de bovenkant van het scherm om de nieuwe app-registratie-ervaring te openen en klik vervolgens op **+ nieuwe registreren** >  **+ toevoegen van een platform**  >  **iOS**.
    - Voer van uw project bundel-ID. Als u de code hebt gedownload, is dit `com.microsoft.identitysample.MSALiOS`. Als u uw eigen project maakt, selecteert u uw project in Xcode en open de **algemene** tabblad. De bundel-id wordt weergegeven in de **identiteit** sectie.
6. Klik op `Configure` en sla de **MSAL configuratie** die wordt weergegeven in de **configuratie van iOS** pagina zodat u deze invoeren kunt bij het later opnieuw configureren van uw app.  Klik op **Gereed**.

## <a name="add-msal"></a>MSAL toevoegen

Kies een van de volgende manieren voor het installeren van de MSAL-bibliotheek in uw app:

### <a name="cocoapods"></a>CocoaPods

1. Als u [CocoaPods](https://cocoapods.org/), installeer `MSAL` door eerst het maken van een leeg bestand met de naam `podfile` in dezelfde map als van het project `.xcodeproj` bestand. Het volgende toevoegen aan `podfile`:

   ```
   use_frameworks!
   
   target '<your-target-here>' do
      pod 'MSAL', '~> 0.4.0'
   end
   ```

2. Vervang `<your-target-here>` met de naam van uw project.
3. Navigeer in een terminalvenster naar de map met de `podfile` u gemaakt en uitgevoerd `pod install` de MSAL-bibliotheek te installeren.
4. Xcode sluit en open `<your project name>.xcworkspace` laden van het project in Xcode.

### <a name="carthage"></a>Carthage

Als u [Carthage](https://github.com/Carthage/Carthage), installeer `MSAL` door toe te voegen aan uw `Cartfile`:

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

### <a name="manually"></a>Handmatig

U kunt ook Git Submodule gebruiken, of bekijk de nieuwste versie om te gebruiken als een framework in uw toepassing.

## <a name="add-your-app-registration"></a>De registratie van uw app toevoegen

Vervolgens gaan we de registratie van uw app toevoegen aan uw code. 

Voeg eerst de volgende importinstructie toe aan het begin van de `ViewController.swift` en `AppDelegate.swift` bestanden:

```swift
import MSAL
```

Voeg de volgende code om `ViewController.swift` voorafgaand aan `viewDidLoad()`:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
let kAuthority = "https://login.microsoftonline.com/common"
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
```

Wijzig de waarde die is toegewezen aan `kClientID`moet uw toepassings-ID. Deze waarde is onderdeel van de MSAL-configuratiegegevens die u tijdens de stap aan het begin van deze zelfstudie voor het registreren van de toepassing in Azure portal hebt opgeslagen.

## <a name="configure-url-schemes"></a>URL-schema's configureren

In deze stap registreert u `CFBundleURLSchemes` zodat de gebruiker terug naar de app kan worden omgeleid na het aanmelden. Door de manier `LSApplicationQueriesSchemes` ook kan uw app gebruikmaken van de Microsoft Authenticator.

Open in Xcode, `Info.plist` als een broncode bestand en voeg de volgende in de `<dict>` sectie. Vervang `[BUNDLE_ID]` met de waarde die u hebt gebruikt in de Azure portal, als u de code wordt gedownload `com.microsoft.identitysample.MSALiOS`. Als u uw eigen project maakt, selecteert u uw project in Xcode en open de **algemene** tabblad. De bundel-id wordt weergegeven in de **identiteit** sectie.

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

## <a name="create-your-apps-ui"></a>De gebruikersinterface van uw app maken

Maak nu een gebruikersinterface met een knop voor het aanroepen van de Microsoft Graph-API, een ander voor afmelden, en een SMS-bericht weergeven om te zien van bepaalde uitvoer door toe te voegen van de volgende code om de `ViewController`klasse:

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

Volgende ook binnen de `ViewController` klasse, vervangt de `viewDidLoad()` methode met:

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

Voeg de volgende `InitMSAL` methode de `ViewController` klasse:

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

### <a name="handle-the-sign-in-callback"></a>Verwerken van de callback aanmelden

Open het `AppDelegate.swift`-bestand. Voor het afhandelen van de callback na het aanmelden, toevoegen `MSALPublicClientApplication.handleMSALResponse` naar de `appDelegate` klasse als volgt:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

#### <a name="acquire-tokens"></a>Tokens verkrijgen

We kunnen nu implementeren van de toepassing gebruikersinterface verwerkingslogica en interactief tokens verkrijgen via MSAL.

MSAL bevat twee primaire methoden voor het ophalen van tokens: `acquireTokenSilently()` en `acquireTokenInteractively()`: 

- `acquireTokenSilently()` probeert een gebruiker aanmelden en ophalen van tokens zonder tussenkomst van de gebruiker als een account aanwezig is.

- `acquireTokenInteractively()` de gebruikersinterface worden altijd weergegeven wanneer u probeert aan te melden bij de gebruiker. Deze sessiecookies in de browser of een account in de Microsoft authenticator gebruiken om een interactieve SSO-ervaring te bieden.

Voeg de volgende code aan de `ViewController` klasse:

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

De onderstaande code wordt een token opgehaald voor het eerst door het maken van een `MSALInteractiveTokenParameters` object en roepen `acquireToken`. De volgende stap voegt u code die:

1. Hiermee maakt u `MSALInteractiveTokenParameters` met een bereik.
2. Aanroepen `acquireToken()` met de parameters gemaakt.
3. Fouten worden verwerkt. Voor meer informatie raadpleegt u de [iOS foutafhandeling handleiding](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Error-Handling).
4. Hiermee wordt de geslaagde aanvraag verwerkt.

Voeg de volgende code aan de `ViewController` klasse.

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

Een bijgewerkte om token te verkrijgen op de achtergrond, voeg de volgende code aan de `ViewController` klasse. Maakt een `MSALSilentTokenParameters` object en aanroepen `acquireTokenSilent()`:

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

Zodra u een token hebt, uw app kan worden gebruikt in de HTTP-header waarmee een gemachtigde aanvraag naar de Microsoft Graph:

| Header-sleutel    | value                 |
| ------------- | --------------------- |
| Authorization | Bearer \<toegangstoken > |

Voeg de volgende code aan de `ViewController` klasse:

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

Zie [Microsoft Graph API](https://graph.microsoft.com) voor meer informatie over de Microsoft Graph API.

### <a name="use-msal-for-sign-out"></a>Gebruik MSAL voor afmelding

Vervolgens is ondersteuning toegevoegd voor afmelden.

> [!Important]
> Afmelden met MSAL alle bekende informatie over een gebruiker verwijdert uit de toepassing, maar de gebruiker heeft nog steeds een actieve sessie op hun apparaat. Als de gebruiker probeert aan te melden bij het opnieuw ze UI voor aanmelden mogelijk te zien, maar mogelijk niet naar hun referenties opnieuw invoeren omdat de sessie van het apparaat nog steeds actief is.

Voeg de volgende code in om toe te voegen afmelding mogelijkheid, de `ViewController` klasse. Deze methode bladeren door alle accounts en verwijdert u deze:

```swift 
    @objc func signOut(_ sender: UIButton) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache */
            
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

In te schakelen token in:
1. Ga naar de instellingen van uw Xcode-Project > **mogelijkheden tabblad** > **delen van Sleutelketens inschakelen**
2. Klik op **+** en voer `com.microsoft.adalcache` als een **Sleutelketengroepen** vermelding.

### <a name="add-helper-methods"></a>Help-methoden toevoegen

Voeg de volgende methoden voor het `ViewController` klasse voor het voltooien van het voorbeeld:

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

Deze app is gebouwd voor een scenario met één account. MSAL biedt ook ondersteuning voor meerdere account scenario's, maar hiervoor extra werk van apps. U moet de gebruikersinterface van de gebruiker selecteren welk account moet worden gebruikt voor elke actie waarvoor tokens te maken. U kunt ook uw app kunt implementeren om een heuristiek om te selecteren welk account moet worden gebruikt de `getAccounts()` methode.

## <a name="test-your-app"></a>Uw app testen

### <a name="run-locally"></a>Lokaal uitvoeren

Bouw en implementeer de app voor een testapparaat of emulator. U moet zich aanmelden en tokens verkrijgen voor Azure AD of persoonlijke Microsoft-accounts.

De eerste keer dat een gebruiker zich bij uw app aanmeldt, wordt ze gevraagd door Microsoft identity akkoord gaan met de machtigingen die zijn aangevraagd.  Hoewel de meeste gebruikers geschikt voor stemt ermee in dat zijn, zijn sommige Azure AD-tenants toestemming van de gebruiker, waarvoor de beheerders om in te stemmen namens alle gebruikers uitgeschakeld. Ter ondersteuning van dit scenario, van uw app bereiken in de Azure-portal te registreren.

Nadat u zich hebt aangemeld, worden de app de gegevens die zijn geretourneerd door de Microsoft Graph weergegeven `/me` eindpunt.

## <a name="get-help"></a>Help opvragen

Ga naar [Help en ondersteuning](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) als u problemen ondervindt met deze zelfstudie of met het Microsoft identity-platform.
