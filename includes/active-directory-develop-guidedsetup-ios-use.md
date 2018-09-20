---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: c6d5fab6ff065dee336c510e3f94583cb0c4960b
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46466149"
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Microsoft Authentication Library (MSAL) gebruiken om op te halen van een token voor de Microsoft Graph API

Open `ViewController.swift` en vervang de code met:

```swift
import UIKit
import MSAL

/// 😃 A View Controller that will respond to the events of the Storyboard.
class ViewController: UIViewController, UITextFieldDelegate, URLSessionDelegate {
    
    // Update the below to your client ID you received in the portal. The below is for running the demo only
    let kClientID = "Your_Application_Id_Here"
    
    // These settings you don't need to edit unless you wish to attempt deeper scenarios with the app.
    let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
    let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
    let kAuthority = "https://login.microsoftonline.com/common"
    
    var accessToken = String()
    var applicationContext : MSALPublicClientApplication?

    @IBOutlet weak var loggingText: UITextView!
    @IBOutlet weak var signoutButton: UIButton!

    /**
        Setup public client application in viewDidLoad
    */

    override func viewDidLoad() {

        super.viewDidLoad()

        do {

            /**
             Initialize a MSALPublicClientApplication with a given clientID and authority
             - clientId:            The clientID of your application, you should get this from the app portal.
             - authority:           A URL indicating a directory that MSAL can use to obtain tokens. In Azure AD
                                    it is of the form https://<instance/<tenant>, where <instance> is the
                                    directory host (e.g. https://login.microsoftonline.com) and <tenant> is a
                                    identifier within the directory itself (e.g. a domain associated to the
                                    tenant, such as contoso.onmicrosoft.com, or the GUID representing the
                                    TenantID property of the directory)
             - error                The error that occurred creating the application object, if any, if you're
                                    not interested in the specific error pass in nil.
             */

            guard let authorityURL = URL(string: kAuthority) else {
                self.loggingText.text = "Unable to create authority URL"
                return
            }

            let authority = try MSALAuthority(url: authorityURL)
            self.applicationContext = try MSALPublicClientApplication(clientId: kClientID, authority: authority)

        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }

    override func viewWillAppear(_ animated: Bool) {

        super.viewWillAppear(animated)
        signoutButton.isEnabled = !self.accessToken.isEmpty
    }
    
    /**
     This button will invoke the authorization flow.
    */

    @IBAction func callGraphButton(_ sender: UIButton) {

        if self.currentAccount() == nil {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            self.acquireTokenInteractively()
        } else {
            self.acquireTokenSilently()
        }
    }

    func acquireTokenInteractively() {

        guard let applicationContext = self.applicationContext else { return }

        applicationContext.acquireToken(forScopes: kScopes) { (result, error) in

            if let error = error {

                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }

            guard let result = result else {

                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }

            self.accessToken = result.accessToken!
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignoutButton(enabled: true)
            self.getContentWithToken()
        }
    }

    func acquireTokenSilently() {

        guard let applicationContext = self.applicationContext else { return }

        /**
         Acquire a token for an existing account silently
         - forScopes:           Permissions you want included in the access token received
                                in the result in the completionBlock. Not all scopes are
                                guaranteed to be included in the access token returned.
         - account:             An account object that we retrieved from the application object before that the
                                authentication flow will be locked down to.
         - completionBlock:     The completion block that will be called when the authentication
                                flow completes, or encounters an error.
         */

        applicationContext.acquireTokenSilent(forScopes: kScopes, account: self.currentAccount()) { (result, error) in

            if let error = error {

                let nsError = error as NSError

                // interactionRequired means we need to ask the user to sign-in. This usually happens
                // when the user's Refresh Token is expired or if the user has changed their password
                // among other possible reasons.
                if (nsError.domain == MSALErrorDomain
                    && nsError.code == MSALErrorCode.interactionRequired.rawValue) {

                    DispatchQueue.main.async {
                        self.acquireTokenInteractively()
                    }

                } else {
                    self.updateLogging(text: "Could not acquire token silently: \(error)")
                }

                return
            }

            guard let result = result else {

                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }

            self.accessToken = result.accessToken!
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignoutButton(enabled: true)
            self.getContentWithToken()
        }
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

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie
#### <a name="getting-a-user-token-interactively"></a>Een gebruiker ophalen interactief token
Aanroepen van de `acquireToken` methode resulteert in een browservenster geopend waarin wordt gevraagd de gebruiker zich aanmeldt. Toepassingen vereisen meestal dat een gebruiker zich interactief aanmelden de eerste keer dat ze nodig hebben voor toegang tot een beveiligde bron, of wanneer een bewerking op de achtergrond te verkrijgen van een token mislukt (bijvoorbeeld van de gebruiker het wachtwoord verlopen).

#### <a name="getting-a-user-token-silently"></a>Een gebruiker ophalen op de achtergrond token
De `acquireTokenSilent` methode wordt gebruikt voor token-aankopen en verlenging zonder tussenkomst van de gebruiker. Na `acquireToken` wordt uitgevoerd voor de eerste keer `acquireTokenSilent` is de methode die vaak worden gebruikt om te verkrijgen van tokens gebruikt voor toegang tot beveiligde resources voor volgende aanroepen - aanroepen aan te vragen of vernieuwen van tokens op de achtergrond worden gemaakt.

Uiteindelijk `acquireTokenSilent` mislukken: bijv. de gebruiker heeft zich afgemeld, of het wachtwoord op een ander apparaat heeft gewijzigd. Wanneer MSAL detecteert dat het probleem kan worden omgezet door verlangen dat ze een interactieve actie, wordt deze gebeurtenis wordt gestart een `MSALErrorCode.interactionRequired` uitzondering. Uw toepassing kan verwerken deze uitzondering op twee manieren:

1.  Aanroepen op basis van `acquireToken` onmiddellijk, wat ertoe leidt dat u wordt gevraagd de gebruiker zich aanmeldt. Dit patroon wordt meestal gebruikt in online toepassingen waarbij er geen offline inhoud in de toepassing beschikbaar is voor de gebruiker. De voorbeeldtoepassing die is gegenereerd door deze Begeleide instelling maakt gebruik van dit patroon: u kunt deze zien in actie de eerste keer dat u de toepassing uitvoert. Omdat er geen gebruiker heeft het ooit de toepassing gebruikt `applicationContext.allAccounts().first` bevat een null-waarde en een ` MSALErrorCode.interactionRequired ` uitzondering gegenereerd. De code in het voorbeeld de uitzondering wordt verwerkt door het aanroepen van `acquireToken` leidt de gebruiker zich aanmeldt.

2.  Toepassingen kunnen ook een visuele indicatie maken voor de gebruiker dat een interactief aanmelden is vereist, zodat de gebruiker kan het juiste moment aan te melden bij selecteren, of de toepassing opnieuw kunt `acquireTokenSilent` op een later tijdstip. Dit wordt meestal gebruikt wanneer de gebruiker andere functionaliteit van de toepassing gebruiken kunt zonder onderbroken - bijvoorbeeld: er offline inhoud beschikbaar in de toepassing is. In dit geval de gebruiker kunt bepalen wanneer ze willen Meld u aan voor toegang tot de beveiligde bron of om de verouderde gegevens te vernieuwen of uw toepassing kunt bepalen om opnieuw te proberen `acquireTokenSilent` wanneer het netwerk is hersteld na een tijdelijk niet beschikbaar.

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Aanroepen van de Microsoft Graph-API met behulp van het token dat u zojuist hebt verkregen.

Toevoegen van de nieuwe methode hieronder om te `ViewController.swift`. Deze methode wordt gebruikt om een `GET` aanvraag indient voor de Microsoft Graph API met behulp van een *HTTP-autorisatie-header*:

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

<!--start-collapse-->
### <a name="making-a-rest-call-against-a-protected-api"></a>Een REST-aanroep op basis van een beveiligde API maken

In deze voorbeeldtoepassing de `getContentWithToken()` methode wordt gebruikt om een HTTP `GET` aanvragen op basis van een beveiligde resource waarvoor een token en vervolgens de inhoud wordt geretourneerd voor de oproepende functie. Met deze methode wordt het verkregen token in de *HTTP-autorisatie-header*. Voor dit voorbeeld is de bron de Microsoft Graph API *me* eindpunt – Hiermee worden de profielgegevens van de gebruiker.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Instellen van afmelding

Voeg de volgende methode om te `ViewController.swift` afmelden van de gebruiker:

```swift 
 @IBAction func signoutButton(_ sender: UIButton) {

        guard let applicationContext = self.applicationContext else { return }

        guard let account = self.currentAccount() else { return }

        do {

            /**
             Removes all tokens from the cache for this application for the provided account
             - account:    The account to remove from the cache
             */

            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signoutButton.isEnabled = false

        } catch let error as NSError {

            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }

}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Meer informatie over het afmelden

De `signoutButton` methode wordt de gebruiker verwijderd uit de cache van MSAL gebruiker – dit effectief vertelt MSAL vergeten van de huidige gebruiker, zodat een toekomstige aanvraag om een token te verkrijgen alleen slaagt als deze is gemaakt interactief zijn.

Hoewel de toepassing in dit voorbeeld biedt ondersteuning voor één gebruiker, ondersteunt MSAL-scenario's waarbij meerdere accounts kunnen worden aangemeld op hetzelfde moment: een voorbeeld is een e-mailtoepassing waar een gebruiker meerdere accounts heeft.
<!--end-collapse-->

## <a name="register-the-callback"></a>De callback te registreren

Zodra de gebruiker zich verifieert, wordt de gebruiker omgeleid naar de toepassing van de browser. Volg de stappen hieronder om deze callback te registreren:

1.  Open `AppDelegate.swift` en MSAL importeren:

```swift
import MSAL
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Toevoegen van de volgende methode aan uw <code>AppDelegate</code> klasse voor het afhandelen van callbacks:
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
