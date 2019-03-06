---
title: Verificatie op iOS met Azure Mobile Apps toevoegen
description: Informatie over het gebruik van Azure Mobile Apps om gebruikers van uw iOS-app via een groot aantal id-providers, met inbegrip van AAD, Google, Facebook, Twitter en Microsoft te verifiëren.
services: app-service\mobile
documentationcenter: ios
author: conceptdev
manager: crdun
editor: ''
ms.assetid: ef3d3cbe-e7ca-45f9-987f-80c44209dc06
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: crdun
ms.openlocfilehash: 8c1c52790065015977add7e32a06063057b24dad
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57445905"
---
# <a name="add-authentication-to-your-ios-app"></a>Verificatie toevoegen aan uw iOS-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In deze zelfstudie voegt u verificatie van de [Snelstartgids voor iOS] project met behulp van een ondersteunde id-provider. In deze zelfstudie is gebaseerd op de [Snelstartgids voor iOS] van de zelfstudie moet u eerst uitvoeren.

## <a name="register"></a>Uw app registreren voor verificatie en de App Service configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie is vereist dat u een nieuwe URL-schema voor uw app definiëren.  Hiermee kunt het verificatiesysteem terug te keren naar uw app nadat het verificatieproces voltooid is.  In deze zelfstudie gebruiken we het URL-schema _appname_ in.  U kunt echter een URL-schema dat u kiest.  Deze moet uniek zijn voor uw mobiele App.  De omleiding aan serverzijde th inschakelen:

1. In de [Azure-portal], selecteert u uw App Service.

2. Klik op de **verificatie / autorisatie** menu-optie.

3. Klik op **Azure Active Directory** onder de **verificatieproviders** sectie.

4. Stel de **beheermodus** naar **Geavanceerd**.

5. In de **toegestane externe Omleidings-URL's**, voer `appname://easyauth.callback`.  De _appname_ in deze reeks wordt het URL-schema voor uw mobiele toepassing.  Het moet normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en beginnen met een letter) volgen.  U moet een notitie van de tekenreeks die u kiest, aangezien u nodig hebt om aan te passen van de code van uw mobiele toepassing met de URL-schema op verschillende plaatsen.

6. Klik op **OK**.

7. Klik op **Opslaan**.

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

In Xcode, drukt u op **uitvoeren** om de app te starten. Een uitzondering is opgetreden omdat de app probeert te krijgen tot de back-end als niet-geverifieerde gebruiker, maar de *TodoItem* tabel nu verificatie is vereist.

## <a name="add-authentication"></a>Verificatie toevoegen aan app
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

    Wijziging *google* naar *microsoftaccount*, *twitter*, *facebook*, of *windowsazureactivedirectory* als u geen van Google als id-provider gebruikmaakt. Als u Facebook gebruikt, moet u [whitelist Facebook domeinen] [ 1] in uw app.

    Vervang de **urlScheme** met een unieke naam voor uw toepassing.  De urlScheme moet gelijk zijn aan het URL-schema-protocol dat u hebt opgegeven in de **toegestane externe Omleidings-URL's** veld in de Azure-portal. De urlScheme wordt gebruikt door de callback voor gebruikersverificatie wilt terugkeren naar uw toepassing nadat de verificatieaanvraag voltooid is.

2. Vervang `[self refresh]` in `viewDidLoad` in *QSTodoListViewController.m* door de volgende code:

    ```Objective-C
    [self loginAndGetData];
    ```

3. Open de `QSAppDelegate.h` bestand en voeg de volgende code toe:

    ```Objective-C
    #import "QSTodoService.h"

    @property (strong, nonatomic) QSTodoService *qsTodoService;
    ```

4. Open de `QSAppDelegate.m` bestand en voeg de volgende code toe:

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

   Voeg deze code direct vóór het lezen van de regel toe `#pragma mark - Core Data stack`.  Vervang de _appname_ met de urlScheme-waarde die u in stap 1 hebt gebruikt.

5. Open de `AppName-Info.plist` bestand (vervangen AppName met de naam van uw app) en voeg de volgende code toe:

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

    Deze code moet worden geplaatst in de `<dict>` element.  Vervang de _appname_ tekenreeks (binnen de matrix voor **CFBundleURLSchemes**) met de naam van de app die u in stap 1 hebt gekozen.  U kunt ook deze wijzigingen aanbrengt in de plist-editor - Klik op de `AppName-Info.plist` -bestand in XCode om de plist-editor te openen.

    Vervang de `com.microsoft.azure.zumo` verbindingsreeks gebruiken voor **CFBundleURLName** bundel met uw Apple-id.

6. Druk op *uitvoeren* naar de app te starten en meld u vervolgens. Wanneer u bent aangemeld, moet u rekening kan de Todo-lijst bekijken en updates.

**Swift**:

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

    Wijziging *google* naar *microsoftaccount*, *twitter*, *facebook*, of *windowsazureactivedirectory* als u geen van Google als id-provider gebruikmaakt. Als u Facebook gebruikt, moet u [whitelist Facebook domeinen] [ 1] in uw app.

    Vervang de **urlScheme** met een unieke naam voor uw toepassing.  De urlScheme moet gelijk zijn aan het URL-schema-protocol dat u hebt opgegeven in de **toegestane externe Omleidings-URL's** veld in de Azure-portal. De urlScheme wordt gebruikt door de callback voor gebruikersverificatie wilt terugkeren naar uw toepassing nadat de verificatieaanvraag voltooid is.

2. Verwijder de regels `self.refreshControl?.beginRefreshing()` en `self.onRefresh(self.refreshControl)` aan het einde van `viewDidLoad()` in *ToDoTableViewController.swift*. Voeg een aanroep naar `loginAndGetData()` in plaats daarvan:

    ```swift
    loginAndGetData()
    ```

3. Open de `AppDelegate.swift` -bestand en voeg de volgende regel aan de `AppDelegate` klasse:

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

    Vervang de _appname_ met de urlScheme-waarde die u in stap 1 hebt gebruikt.

4. Open de `AppName-Info.plist` bestand (vervangen AppName met de naam van uw app) en voeg de volgende code toe:

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

    Deze code moet worden geplaatst in de `<dict>` element.  Vervang de _appname_ tekenreeks (binnen de matrix voor **CFBundleURLSchemes**) met de naam van de app die u in stap 1 hebt gekozen.  U kunt ook deze wijzigingen aanbrengt in de plist-editor - Klik op de `AppName-Info.plist` -bestand in XCode om de plist-editor te openen.

    Vervang de `com.microsoft.azure.zumo` verbindingsreeks gebruiken voor **CFBundleURLName** bundel met uw Apple-id.

5. Druk op *uitvoeren* naar de app te starten en meld u vervolgens. Wanneer u bent aangemeld, moet u rekening kan de Todo-lijst bekijken en updates.

App Service-verificatie wordt gebruikt voor communicatie van appels Inter-App.  Raadpleeg voor meer informatie over dit onderwerp, de [Apple-documentatie][2]
<!-- URLs. -->

[1]: https://developers.facebook.com/docs/ios/ios9#whitelist
[2]: https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html
[Azure-portal]: https://portal.azure.com

[Snelstartgids voor iOS]: app-service-mobile-ios-get-started.md

