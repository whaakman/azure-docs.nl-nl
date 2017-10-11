---
title: Aan de slag met verificatie voor mobiele Apps in Xamarin voor iOS
description: "Informatie over het verifiëren van gebruikers van uw Xamarin iOS-app via een groot aantal identiteitsproviders, waaronder AAD, Google, Facebook, Twitter en Microsoft met Mobile Apps."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 180cc61b-19c5-48bf-a16c-7181aef3eacc
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: glenga
ms.openlocfilehash: 454b2df5a9bf8cfba93befea54370957ab044d95
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-xamarinios-app"></a>Authenticatie toevoegen aan uw Xamarin.iOS-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Dit onderwerp leest u hoe u verifieert gebruikers van een App Service Mobile App van uw clienttoepassing. In deze zelfstudie kunt u verificatie toevoegen aan het Xamarin.iOS-Quick Start-project met een id-provider die wordt ondersteund door App Service. Nadat wordt is geverifieerd en geautoriseerd door uw mobiele App, de waarde van de gebruiker-ID wordt weergegeven en kunt u zich toegang kunnen krijgen tot gegevens in een tabel met beperkte toegang.

U moet eerst Voltooi de zelfstudie [een Xamarin.iOS-app maken]. Als u het gedownloade quick start-serverproject niet gebruikt, moet u het pakket voor verificatie-extensie toevoegen aan uw project. Zie voor meer informatie over server extensiepakketten [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Uw app registreren voor verificatie en App-Services configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie vereist dat u een nieuwe URL-schema voor uw app definiëren. Hierdoor kan de verificatiesysteem terug te keren naar uw app zodra het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. Selecteer in de [Azure-portal] uw App Service.

2. Klik op de **verificatie / autorisatie** menuoptie.

3. In de **toegestaan externe Omleidings-URL's**, voer `url_scheme_of_your_app://easyauth.callback`.  De **url_scheme_of_your_app** in deze tekenreeks wordt het URL-schema voor uw mobiele toepassing.  Deze moet voldoen aan de normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en begin met een letter).  U moet een notitie van de tekenreeks die u naar wens aanpassen van uw mobiele toepassingscode met het URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="restrict-permissions-to-authenticated-users"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. Voer het clientproject op een apparaat of emulator in Visual Studio en Xamarin Studio. Controleer of dat een niet-verwerkte uitzondering met een statuscode van 401 (niet-geautoriseerd) treedt op nadat de app wordt gestart. De fout wordt geregistreerd in de console van het foutopsporingsprogramma. Dus in Visual Studio ziet u de fout in het uitvoervenster.

&nbsp;&nbsp;Deze onbevoegde fout treedt op omdat de app probeert te krijgen tot uw back-end voor mobiele App als een niet-geverifieerde gebruiker. De *TodoItem* tabel nu is verificatie vereist.

U wordt vervolgens de client-app voor aanvragen voor resources bijwerken vanuit de back-end voor de mobiele App met een geverifieerde gebruiker.

## <a name="add-authentication-to-the-app"></a>Verificatie toevoegen aan de app.
In deze sectie wijzigt u de app voor een aanmeldingsscherm weergegeven voordat gegevens worden weergegeven. Wanneer de app wordt gestart, wordt geen geen verbinding maken met uw App Service en gegevens kunnen niet worden weergegeven. Nadat de eerste keer dat de gebruiker uitvoert de gebaar vernieuwen van het aanmeldingsscherm weergegeven; na een geslaagde aanmelding wordt de lijst met todo-items worden weergegeven.

1. Open het bestand in het clientproject **QSTodoService.cs** en voeg de volgende gebruiksinstructie en `MobileServiceUser` met accessor aan de klasse QSTodoService:
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. Met de naam van de nieuwe methode Add **verifiëren** naar **QSTodoService** met de definitie van de volgende:

        public async Task Authenticate(UIViewController view)
        {
            try
            {
                AppDelegate.ResumeWithURL = url => url.Scheme == "zumoe2etestapp" && client.ResumeWithURL(url);
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Als u van een id-provider dan een Facebook gebruikmaakt, wijzig de waarde die is doorgegeven aan **LoginAsync** boven op een van de volgende opties: _MicrosoftAccount_, _Twitter_, _Google_, of _WindowsAzureActiveDirectory_.

3. Open **QSTodoListViewController.cs**. Wijzig de methodedefinitie van **ViewDidLoad** verwijderen van de aanroep van **RefreshAsync()** in de buurt van het einde:
   
        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();
   
            todoService = QSTodoService.DefaultService;
            await todoService.InitializeStoreAsync();
   
            RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync();
            }
   
            // Comment out the call to RefreshAsync
            // await RefreshAsync();
        }
4. De methode wijzigen **RefreshAsync** om te verifiëren als de **gebruiker** eigenschap is null. Voeg de volgende code aan de bovenkant van de methodedefinitie van de:
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. Open **AppDelegate.cs**, voeg de volgende methode toe:

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. Open **Info.plist** bestand, gaat u naar **URL typen** in de **Geavanceerd** sectie. Nu configureren de **id** en de **URL-schema's** van uw URL-Type en klik op **URL-Type toevoegen**. **URL-schema's** moet hetzelfde zijn als uw {url_scheme_of_your_app}.
7. In Visual Studio of Xamarin Studio verbonden met uw Host Xamarin bouwen op uw Mac het clientproject die gericht is op een apparaat of emulator uitgevoerd. Controleer of dat de app geen gegevens weergegeven.
   
    Een gebaar van de vernieuwing uitvoeren met het binnenhalen van de lijst met items, waardoor het aanmeldingsscherm wordt weergegeven. Nadat u geldige referenties hebt, de app wordt weergegeven de lijst met todo-items en kunt u updates aanbrengen in de gegevens.

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[een Xamarin.iOS-app maken]: app-service-mobile-xamarin-ios-get-started.md