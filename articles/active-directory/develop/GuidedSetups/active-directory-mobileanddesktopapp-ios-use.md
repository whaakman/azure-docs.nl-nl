---
title: Azure AD v2 iOS Getting Started - gebruik | Microsoft Docs
description: Hoe iOS (Swift)-toepassingen met een API waarvoor toegangstokens door Azure Active Directory-v2-eindpunt kunnen aanroepen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 2ac1117a31a101705539a1f75520ce8de43809a2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>De Microsoft Authentication Library (MSAL) gebruiken voor het ophalen van een token voor de Microsoft Graph-API

Open `ViewController.swift` en vervang de code met:

```swift
import UIKit
import MSAL

class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {
    
    let kClientID = "Your_Application_Id_Here"
    let kAuthority = "https://login.microsoftonline.com/common/v2.0"

    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    
    var accessToken = String()
    var applicationContext = MSALPublicClientApplication.init()

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

     // This button will invoke the call to the Microsoft Graph API. It uses the
     // built in Swift libraries to create a connection.
    
    @IBAction func callGraphButton(_ sender: UIButton) {
        
        
        do {
            
            // We check to see if we have a current logged in user. If we don't, then we need to sign someone in.
            // We throw an interactionRequired so that we trigger the interactive signin.
            
            if  try self.applicationContext.users().isEmpty {
                throw NSError.init(domain: "MSALErrorDomain", code: MSALErrorCode.interactionRequired.rawValue, userInfo: nil)
            } else {
            
            // Acquire a token for an existing user silently
            
            try self.applicationContext.acquireTokenSilent(forScopes: self.kScopes, user: applicationContext.users().first) { (result, error) in
    
                    if error == nil {
                        self.accessToken = (result?.accessToken)!
                        self.loggingText.text = "Refreshing token silently)"
                        self.loggingText.text = "Refreshed Access token is \(self.accessToken)"
                        
                        self.signoutButton.isEnabled = true;
                        self.getContentWithToken()
    
                    } else {
                        self.loggingText.text = "Could not acquire token silently: \(error ?? "No error information" as! Error)"
    
                    }
                }
            }
        }  catch let error as NSError {
            
            // interactionRequired means we need to ask the user to sign-in. This usually happens
            // when the user's Refresh Token is expired or if the user has changed their password
            // among other possible reasons.
            
            if error.code == MSALErrorCode.interactionRequired.rawValue {
                
                self.applicationContext.acquireToken(forScopes: self.kScopes) { (result, error) in
                        if error == nil {
                            self.accessToken = (result?.accessToken)!
                            self.loggingText.text = "Access token is \(self.accessToken)"
                            self.signoutButton.isEnabled = true;
                            self.getContentWithToken()
                            
                        } else  {
                            self.loggingText.text = "Could not acquire token: \(error ?? "No error information" as! Error)"
                        }
                }
                
            }
            
        } catch {
            
            // This is the catch all error.
            
            self.loggingText.text = "Unable to acquire token. Got error: \(error)"
            
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        
        do {
             // Initialize a MSALPublicClientApplication with a given clientID and authority
            self.applicationContext = try MSALPublicClientApplication.init(clientId: kClientID, authority: kAuthority)
        } catch {
            self.loggingText.text = "Unable to create Application Context. Error: \(error)"
        }
    }


    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
    override func viewWillAppear(_ animated: Bool) {
        
        if self.accessToken.isEmpty {
            signoutButton.isEnabled = false; 
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie
#### <a name="getting-a-user-token-interactively"></a>Een gebruiker ophalen interactief token
Het aanroepen van de `acquireToken` methode resulteert in een browservenster vraagt de gebruiker aan te melden. Toepassingen vereisen meestal een gebruiker zich aanmelden interactief de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron, of wanneer een achtergrond-bewerking te verkrijgen van een token mislukt (bijvoorbeeld van de gebruiker het wachtwoord verlopen).

#### <a name="getting-a-user-token-silently"></a>Een gebruiker ophalen achtergrond token
De `acquireTokenSilent` methode verwerkt token acquisities van organisaties en verlenging zonder tussenkomst van de gebruiker. Na `acquireToken` wordt uitgevoerd voor de eerste keer `acquireTokenSilent` is de methode die vaak worden gebruikt voor het verkrijgen van tokens die worden gebruikt voor toegang tot beveiligde bronnen voor volgende aanroepen - aanroepen aan te vragen of vernieuwen van tokens op de achtergrond worden aangebracht.

Uiteindelijk `acquireTokenSilent` mislukt: bijvoorbeeld de gebruiker heeft zich afgemeld, of het wachtwoord op een ander apparaat is gewijzigd. Wanneer MSAL detecteert dat het probleem doordat een interactieve actie kan worden omgezet, wordt deze gebeurtenis wordt gestart een `MSALErrorCode.interactionRequired` uitzondering. Uw toepassing kan verwerken van deze uitzondering op twee manieren:

1.  Een aanroep tegen `acquireToken` onmiddellijk, wat ertoe leidt de gebruiker aan te melden. Dit patroon wordt meestal gebruikt in de on line toepassingen wanneer er geen offline inhoud in de toepassing beschikbaar is voor de gebruiker is. Dit patroon maakt gebruik van de voorbeeldtoepassing die is gegenereerd door deze Begeleide instelprocedure: u kunt deze bekijken in actie de eerste keer dat u de toepassing uitvoert. Omdat er geen gebruiker ooit de toepassing gebruikt `applicationContext.users().first` bevat een null-waarde en een ` MSALErrorCode.interactionRequired ` uitzondering gegenereerd. De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `acquireToken` waardoor vraagt de gebruiker aan te melden.

2.  Toepassingen kunnen ook een visuele indicatie maken voor de gebruiker die een interactief aanmelden is vereist, zodat de gebruiker het juiste moment aan te melden kunt selecteren of de toepassing opnieuw kunt `acquireTokenSilent` op een later tijdstip. Dit wordt meestal gebruikt wanneer de gebruiker andere functies van de toepassing gebruiken kunt zonder wordt onderbroken - bijvoorbeeld offline inhoud beschikbaar is in de toepassing is. In dit geval wordt de gebruiker kunt bepalen wanneer ze willen aanmelden voor toegang tot de beveiligde bron, of om de verouderde gegevens te vernieuwen of uw toepassing kunt ervoor kiezen om opnieuw te proberen `acquireTokenSilent` wanneer netwerk is hersteld na een tijdelijk niet beschikbaar.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>De Microsoft Graph API met behulp van het token dat u zojuist hebt verkregen aanroepen

Toevoegen van de nieuwe methode hieronder `ViewController.swift`. Deze methode wordt gebruikt om een `GET` -aanvraag in voor de Microsoft Graph API met behulp van een *HTTP-autorisatie-header*:

```swift
func getContentWithToken() {
    
    let sessionConfig = URLSessionConfiguration.default
    
    // Specify the Graph API endpoint
    let url = URL(string: kGraphURI)
    var request = URLRequest(url: url!)
    
    // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
    request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
    let urlSession = URLSession(configuration: sessionConfig, delegate: self, delegateQueue: OperationQueue.main)
    
    urlSession.dataTask(with: request) { data, response, error in
        let result = try? JSONSerialization.jsonObject(with: data!, options: [])
                    if result != nil {
                
                self.loggingText.text = result.debugDescription
            }
        }.resume()
}
```

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a>Maken van een REST-aanroep op basis van een beveiligde API

In deze voorbeeldtoepassing de `getContentWithToken()` methode wordt gebruikt voor het maken van een HTTP `GET` aanvragen op basis van een beveiligde bron die is een token vereist en vervolgens de inhoud naar de aanroeper wordt geretourneerd. Met deze methode wordt het token verkregen in de *HTTP-autorisatie-header*. Voor dit voorbeeld wordt de resource is de Microsoft Graph API *mij* eindpunt – waarin informatie over het profiel van de gebruiker.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Afmelden instellen

Voeg de volgende methode `ViewController.swift` afmelden van de gebruiker:

```swift 
@IBAction func signoutButton(_ sender: UIButton) {

    do {
        
        // Removes all tokens from the cache for this application for the provided user
        // first parameter:   The user to remove from the cache
        
        try self.applicationContext.remove(self.applicationContext.users().first)
        self.signoutButton.isEnabled = false;
        
    } catch let error {
        self.loggingText.text = "Received error signing user out: \(error)"
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Meer informatie over afmelden

De `signoutButton` methode wordt de gebruiker verwijderd uit de cache van de MSAL gebruiker – dit effectief laat weten MSAL vergeten van de huidige gebruiker, zodat een toekomstige aanvraag voor een token verkrijgen alleen lukt als het gaat interactief zijn.

Hoewel de toepassing in dit voorbeeld een enkele gebruiker ondersteunt, ondersteunt MSAL scenario's waarbij meerdere accounts kunnen worden aangemeld op hetzelfde moment: een voorbeeld is een e-mailtoepassing waar een gebruiker heeft meerdere accounts.
<!--end-collapse-->

## <a name="register-the-callback"></a>De callback registreren

Zodra de gebruiker zich verifieert, wordt de gebruiker omgeleid naar de toepassing van de browser. Volg onderstaande stappen voor het registreren van deze callback:

1.  Open `AppDelegate.swift` en MSAL importeren:

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Voeg de volgende methode voor uw <code>AppDelegate</code> klasse voor het afhandelen van retouraanroepen:
</li>
</ol>

```swift
// @brief Handles inbound URLs. Checks if the URL matches the redirect URI for a pending AppAuth
// authorization request and if so, will look for the code in the response.

func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    
    print("Received callback!")
    
    MSALPublicClientApplication.handleMSALResponse(url)
    
    return true
}
```

