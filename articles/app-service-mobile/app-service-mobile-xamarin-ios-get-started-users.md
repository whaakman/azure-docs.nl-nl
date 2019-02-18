---
title: Aan de slag met verificatie voor Mobile Apps in Xamarin iOS
description: Informatie over het gebruik van Mobile Apps om gebruikers van uw Xamarin iOS-app via een groot aantal id-providers, met inbegrip van AAD, Google, Facebook, Twitter en Microsoft te verifiëren.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 180cc61b-19c5-48bf-a16c-7181aef3eacc
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: crdun
ms.openlocfilehash: 132909931291daf3aefddd5e1a44273050d98e06
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56326167"
---
# <a name="add-authentication-to-your-xamarinios-app"></a>Authenticatie toevoegen aan uw Xamarin.iOS-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Dit onderwerp ziet u hoe u verifieert gebruikers van een App Service Mobile App van uw clienttoepassing. In deze zelfstudie, kunt u verificatie toevoegen aan de Xamarin.iOS Quick Start-project met behulp van een id-provider die wordt ondersteund door App Service. Nadat wordt is geverifieerd en geautoriseerd door uw mobiele App, de waarde van de gebruiker-ID wordt weergegeven en kunt u zich voor toegang tot gegevens in een tabel met beperkte toegang.

U moet eerst de zelfstudie hebt voltooid [Een Xamarin.iOS-app maken]. Als u het gedownloade quick start-serverproject niet gebruikt, moet u het pakket voor verificatie-extensie toevoegen aan uw project. Zie voor meer informatie over het server-extensiepakketten [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Uw app registreren voor verificatie en App-Services configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie is vereist dat u een nieuwe URL-schema voor uw app definiëren. Hiermee kunt het verificatiesysteem terug te keren naar uw app nadat het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. In de [Azure-portal](https://portal.azure.com/), selecteert u uw App Service.

2. Klik op de **verificatie / autorisatie** menu-optie.

3. In de **toegestane externe Omleidings-URL's**, voer `url_scheme_of_your_app://easyauth.callback`.  De **url_scheme_of_your_app** in deze reeks wordt het URL-schema voor uw mobiele toepassing.  Het moet normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en beginnen met een letter) volgen.  U moet een notitie van de tekenreeks die u kiest, aangezien u nodig hebt om aan te passen van de code van uw mobiele toepassing met de URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="restrict-permissions-to-authenticated-users"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* Voer het clientproject op een apparaat of emulator in Visual Studio of Xamarin Studio. Controleer of dat een niet-verwerkte uitzondering met een statuscode 401 (niet-gemachtigd) is gegenereerd nadat de app wordt gestart. De fout wordt geregistreerd in de console van het foutopsporingsprogramma. Dus in Visual Studio ziet u de fout in het uitvoervenster weergegeven.

    Deze niet-geautoriseerde fout doet zich voor omdat de app probeert te krijgen tot uw back-end van mobiele App als een niet-geverifieerde gebruiker. De *TodoItem* tabel nu verificatie is vereist.

U wordt vervolgens de client-app op aanvraag resources bijwerken van de back-end van de mobiele App met een geverifieerde gebruiker.

## <a name="add-authentication-to-the-app"></a>Verificatie toevoegen aan de app.
In deze sectie wijzigt u de app een aanmeldingsscherm weergegeven voordat gegevens worden weergegeven. Wanneer de app wordt gestart, wordt geen verbinding maken met uw App Service en worden geen gegevens weergegeven. Nadat de eerste keer dat de gebruiker uitvoert de beweging vernieuwen, wordt het aanmeldingsscherm weergegeven; na geslaagde aanmelding wordt de lijst met todo-items worden weergegeven.

1. Open het bestand in het clientproject **QSTodoService.cs** en voeg de volgende gebruiksinstructie en `MobileServiceUser` met accessor aan de klasse QSTodoService:

    ```csharp
    using UIKit;

    // Logged in user
    private MobileServiceUser user;
    public MobileServiceUser User { get { return user; } }
    ```

2. Toevoegen van nieuwe methode met de naam **verifiëren** naar **QSTodoService** met de definitie van de volgende:

    ```csharp
    public async Task Authenticate(UIViewController view)
    {
        try
        {
            AppDelegate.ResumeWithURL = url => url.Scheme == "{url_scheme_of_your_app}" && client.ResumeWithURL(url);
            user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
        }
    }
    ```

    > [!NOTE]
    > Als u een id-provider dan een Facebook gebruikt, wijzigt u de waarde die is doorgegeven aan **LoginAsync** boven op een van de volgende: _MicrosoftAccount_, _Twitter_, _Google_, or _WindowsAzureActiveDirectory_.

3. Open **QSTodoListViewController.cs**. Wijzigen van de methodedefinitie van de van **ViewDidLoad** verwijderen van de aanroep van **RefreshAsync()** in de buurt van het einde:

    ```csharp
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
    ```

4. Wijzigen van de methode **RefreshAsync** om te verifiëren als de **gebruiker** eigenschap null is. Voeg de volgende code toe aan de bovenkant van de methodedefinitie van de:

    ```csharp
    // start of RefreshAsync method
    if (todoService.User == null) {
        await QSTodoService.DefaultService.Authenticate(this);
        if (todoService.User == null) {
            Console.WriteLine("couldn't login!!");
            return;
        }
    }
    // rest of RefreshAsync method
    ```

5. Open **AppDelegate.cs**, voeg de volgende methode toe:

    ```csharp
    public static Func<NSUrl, bool> ResumeWithURL;

    public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
    {
        return ResumeWithURL != null && ResumeWithURL(url);
    }
    ```

6. Open **Info.plist** bestand, gaat u naar **URL typen** in de **Geavanceerd** sectie. Nu configureren voor de **id** en de **URL-schema's** van uw Type URL en klik op **URL-Type toevoegen**. **URL-schema's** moet gelijk zijn aan uw {url_scheme_of_your_app}.
7. Voer het clientproject die gericht is op een apparaat of emulator in Visual Studio, verbonden met uw Mac-Host of Visual Studio voor Mac. Controleer of dat de app geen gegevens weergegeven.

    Het gebaar vernieuwen uitvoeren door het binnenhalen van de lijst met items, waardoor het aanmeldingsscherm wordt weergegeven. Nadat u hebt geldige referenties is opgegeven, de app de lijst met todo-items worden weergegeven en kunt u updates aanbrengen in de gegevens.

<!-- URLs. -->
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Een Xamarin.iOS-app maken]: app-service-mobile-xamarin-ios-get-started.md
