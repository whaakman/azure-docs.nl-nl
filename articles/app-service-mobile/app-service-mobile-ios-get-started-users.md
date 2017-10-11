---
title: Verificatie op iOS met Azure Mobile Apps toevoegen
description: "Informatie over het verifiëren van gebruikers van uw iOS-app via een groot aantal identiteitsproviders, waaronder AAD, Google, Facebook, Twitter en Microsoft met Azure Mobile Apps."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: ef3d3cbe-e7ca-45f9-987f-80c44209dc06
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: glenga
ms.openlocfilehash: 21a2cc6c1eaf4b34cbe8c2d7c4dbb69c8730cf32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-ios-app"></a>Verificatie toevoegen aan uw iOS-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In deze zelfstudie maakt u de verificatie van toevoegen de [iOS snel starten] project met een ondersteunde id-provider. Deze zelfstudie is gebaseerd op de [iOS snel starten] zelfstudie die u moet eerst te voltooien.

## <a name="register"></a>Uw app registreren voor verificatie en de App Service configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie vereist dat u een nieuwe URL-schema voor uw app definiëren.  Hierdoor kan de verificatiesysteem terug te keren naar uw app zodra het verificatieproces voltooid is.  In deze zelfstudie gebruiken we het URL-schema _appname_ in.  U kunt echter een URL-schema dat u kiest.  Deze moet uniek zijn voor uw mobiele App.  De omleiding aan serverzijde th inschakelen:

1. In de [Azure-portal], selecteer uw App Service.

2. Klik op de **verificatie / autorisatie** menuoptie.

3. Klik op **Azure Active Directory** onder de **verificatieproviders** sectie.

4. Stel de **beheermodus** naar **Geavanceerd**.

5. In de **toegestaan externe Omleidings-URL's**, voer `appname://easyauth.callback`.  De _appname_ in deze tekenreeks wordt het URL-schema voor uw mobiele toepassing.  Deze moet voldoen aan de normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en begin met een letter).  U moet een notitie van de tekenreeks die u naar wens aanpassen van uw mobiele toepassingscode met het URL-schema op verschillende plaatsen.

6. Klik op **OK**.

7. Klik op **Opslaan**.

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Druk in Xcode op **uitvoeren** om de app te starten. Er is een uitzondering opgetreden omdat de app probeert te krijgen tot de back-end als niet-geverifieerde gebruiker, maar de *TodoItem* tabel nu is verificatie vereist.

## <a name="add-authentication"></a>Verificatie toevoegen aan de app
**Objective-C**:

1. Open op uw Mac *QSTodoListViewController.m* in Xcode en voeg de volgende methode toe:

    ```Objective-C
    - (void)loginAndGetData
    {
        QSAppDelegate *appDelegate = (QSAppDelegate *)[UIApplication sharedApplication].delegate;
        appDelegate.qsTodoService = self.todoService;

        [self.todoService.client loginWithProvider:@"google" urlScheme:@"appname" controller:self animated:YES completion:^(MSUser * _Nullable user, NSError * _Nullable error) {
            if (error) {
                NSLog(@"Login failed with error: %@, %@", error, [error userInfo]);
            }
            else {
                self.todoService.client.currentUser = user;
                NSLog(@"User logged in: %@", user.userId);

                [self refresh];
            }
        }];
    }
    ```

    Wijziging *google* naar *microsoftaccount*, *twitter*, *facebook*, of *windowsazureactivedirectory* als u Google niet als id-provider gebruikt. Als u Facebook gebruikt, moet u [geaccepteerde Facebook domeinen] [ 1] in uw app.

    Vervang de **urlScheme** met een unieke naam voor uw toepassing.  De urlScheme moet hetzelfde zijn als de URL-schema-protocol dat u hebt opgegeven in de **toegestaan externe Omleidings-URL's** veld in de Azure portal. De urlScheme wordt gebruikt door de callback voor gebruikersverificatie overschakelen naar uw toepassing nadat de authenticatie-aanvraag voltooid is.

2. Vervang `[self refresh]` in `viewDidLoad` in *QSTodoListViewController.m* met de volgende code:

    ```Objective-C
    [self loginAndGetData];
    ```

3. Open de `QSAppDelegate.h` -bestand en voeg de volgende code:

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. Open de `QSAppDelegate.m` -bestand en voeg de volgende code:

    ```Objective-C
    - (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
    {
        if ([[url.scheme lowercaseString] isEqualToString:@"appname"]) {
            // Resume login flow
            return [self.qsTodoService.client resumeWithURL:url];
        }
        else {
            return NO;
        }
    }
    ```

   Voeg deze code direct voor de regel lezen `#pragma mark - Core Data stack`.  Vervang de _appname_ wih de urlScheme-waarde die u in stap 1 hebt gebruikt.

5. Open de `AppName-Info.plist` bestand (vervang AppName met de naam van uw app) en voeg de volgende code:

    ```XML
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    Deze code moet worden geplaatst in de `<dict>` element.  Vervang de _appname_ tekenreeks (binnen de matrix voor **CFBundleURLSchemes**) met de naam van de app die u in stap 1 hebt gekozen.  U kunt deze wijzigingen ook aanbrengen in de plist-editor - Klik op de `AppName-Info.plist` -bestand in XCode om de plist-editor te openen.

    Vervang de `com.microsoft.azure.zumo` de tekenreeks van **CFBundleURLName** bundel met uw Apple-id.

6. Druk op *uitvoeren* naar de app te starten en meld u vervolgens. Wanneer u bent aangemeld, moet u mogelijk zijn de takenlijst weergeven en bijwerken.

**SWIFT**:

1. Open op uw Mac *ToDoTableViewController.swift* in Xcode en voeg de volgende methode toe:

    ```swift
    func loginAndGetData() {

        guard let client = self.table?.client, client.currentUser == nil else {
            return
        }

        let appDelegate = UIApplication.shared.delegate as! AppDelegate
        appDelegate.todoTableViewController = self

        let loginBlock: MSClientLoginBlock = {(user, error) -> Void in
            if (error != nil) {
                print("Error: \(error?.localizedDescription)")
            }
            else {
                client.currentUser = user
                print("User logged in: \(user?.userId)")
            }
        }

        client.login(withProvider:"google", urlScheme: "appname", controller: self, animated: true, completion: loginBlock)

    }
    ```

    Wijziging *google* naar *microsoftaccount*, *twitter*, *facebook*, of *windowsazureactivedirectory* als u Google niet als id-provider gebruikt. Als u Facebook gebruikt, moet u [geaccepteerde Facebook domeinen] [ 1] in uw app.

    Vervang de **urlScheme** met een unieke naam voor uw toepassing.  De urlScheme moet hetzelfde zijn als de URL-schema-protocol dat u hebt opgegeven in de **toegestaan externe Omleidings-URL's** veld in de Azure portal. De urlScheme wordt gebruikt door de callback voor gebruikersverificatie overschakelen naar uw toepassing nadat de authenticatie-aanvraag voltooid is.

2. Verwijder de regels `self.refreshControl?.beginRefreshing()` en `self.onRefresh(self.refreshControl)` aan het einde van `viewDidLoad()` in *ToDoTableViewController.swift*. Voeg een aanroep naar `loginAndGetData()` op hun plaats:

    ```swift
    loginAndGetData()
    ```

3. Open de `AppDelegate.swift` -bestand en voeg de volgende regel om de `AppDelegate` klasse:

    ```swift
    var todoTableViewController: ToDoTableViewController?

    func application(_ application: UIApplication, openURL url: NSURL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
        if url.scheme?.lowercased() == "appname" {
            return (todoTableViewController!.table?.client.resume(with: url as URL))!
        }
        else {
            return false
        }
    }
    ```

    Vervang de _appname_ wih de urlScheme-waarde die u in stap 1 hebt gebruikt.

4. Open de `AppName-Info.plist` bestand (vervang AppName met de naam van uw app) en voeg de volgende code:

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLName</key>
            <string>com.microsoft.azure.zumo</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>appname</string>
            </array>
        </dict>
    </array>
    ```

    Deze code moet worden geplaatst in de `<dict>` element.  Vervang de _appname_ tekenreeks (binnen de matrix voor **CFBundleURLSchemes**) met de naam van de app die u in stap 1 hebt gekozen.  U kunt deze wijzigingen ook aanbrengen in de plist-editor - Klik op de `AppName-Info.plist` -bestand in XCode om de plist-editor te openen.

    Vervang de `com.microsoft.azure.zumo` de tekenreeks van **CFBundleURLName** bundel met uw Apple-id.

5. Druk op *uitvoeren* naar de app te starten en meld u vervolgens. Wanneer u bent aangemeld, moet u mogelijk zijn de takenlijst weergeven en bijwerken.

App Service-verificatie gebruikt appels Inter-App-communicatie.  Raadpleeg voor meer informatie over dit onderwerp, de [Apple-documentatie][2]
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[Azure-portal]: https://portal.azure.com

[iOS snel starten]: app-service-mobile-ios-get-started.md

