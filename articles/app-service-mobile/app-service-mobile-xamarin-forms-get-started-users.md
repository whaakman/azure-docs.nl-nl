---
title: Aan de slag met verificatie voor Mobile Apps in Xamarin Forms-app | Microsoft Docs
description: Informatie over het gebruik van Mobile Apps om gebruikers van uw Xamarin Forms-app via een groot aantal id-providers, met inbegrip van AAD, Google, Facebook, Twitter en Microsoft te verifiëren.
services: app-service\mobile
documentationcenter: xamarin
author: panarasi
manager: crdun
editor: ''
ms.assetid: 9c55e192-c761-4ff2-8d88-72260e9f6179
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 09/24/2018
ms.author: panarasi
ms.openlocfilehash: f7e500fb5856c7eec48a371042244b44dd944779
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063765"
---
# <a name="add-authentication-to-your-xamarin-forms-app"></a>Verificatie toevoegen aan uw Xamarin Forms-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Overzicht
Dit onderwerp ziet u hoe u verifieert gebruikers van een App Service Mobile App van uw clienttoepassing. In deze zelfstudie, kunt u verificatie toevoegen aan de Xamarin Forms Quick Start-project met behulp van een id-provider die wordt ondersteund door App Service. Nadat wordt is geverifieerd en geautoriseerd door uw mobiele App, de waarde van de gebruiker-ID wordt weergegeven en kunt u zich voor toegang tot gegevens in een tabel met beperkte toegang.

## <a name="prerequisites"></a>Vereisten
Voor het beste resultaat met deze zelfstudie, het beste voert u de [een Xamarin Forms-app maken] [ 1] zelfstudie. Nadat u deze zelfstudie hebt voltooid, hebt u een Xamarin Forms-project dat is een TodoList-app voor meerdere platforms.

Als u het gedownloade quick start-serverproject niet gebruikt, moet u het pakket voor verificatie-extensie toevoegen aan uw project. Zie voor meer informatie over het server-extensiepakketten [werken met de .NET back-endserver SDK voor Azure Mobile Apps][2].

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Uw app registreren voor verificatie en App-Services configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie is vereist dat u een nieuwe URL-schema voor uw app definiëren. Hiermee kunt het verificatiesysteem terug te keren naar uw app nadat het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. In de [Azure-portal][8], selecteert u uw App Service.

2. Klik op de **verificatie / autorisatie** menu-optie.

3. In de **toegestane externe Omleidings-URL's**, voer `url_scheme_of_your_app://easyauth.callback`.  De **url_scheme_of_your_app** in deze reeks wordt het URL-schema voor uw mobiele toepassing.  Het moet normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en beginnen met een letter) volgen.  U moet een notitie van de tekenreeks die u kiest, aangezien u nodig hebt om aan te passen van de code van uw mobiele toepassing met de URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="restrict-permissions-to-authenticated-users"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-to-the-portable-class-library"></a>Verificatie toevoegen aan de draagbare klassebibliotheek
Mobile Apps maken gebruik van de [LoginAsync] [ 3] uitbreidingsmethode op de [MobileServiceClient] [ 4] te melden bij een gebruiker met App Service -verificatie. In dit voorbeeld wordt een verificatiestroom server beheerd die van de provider in de aanmeldingsinterface in de app wordt weergegeven. Zie voor meer informatie, [authentication-Server beheerd][5]. Voor een betere ervaring voor eindgebruikers in uw app in productie, moet u rekening houden in plaats daarvan [Client beheerde verificatie][6].

Als u wilt verifiëren met een project Xamarin Forms, definieert een **IAuthenticate** interface in de draagbare klassenbibliotheek vormt voor de app. Voeg een **aanmelden** knop aan de gebruikersinterface is gedefinieerd in de draagbare klassenbibliotheek, die u op te starten verificatie. Gegevens worden geladen vanuit de mobiele back-end na een geslaagde authenticatie.

Implementeer de **IAuthenticate** interface voor elk platform wordt ondersteund door uw app.

1. Open in Visual Studio of Xamarin Studio App.cs uit het project met **draagbare** in de naam, die Portable Class Library-project, voegt u de volgende `using` instructie:

        using System.Threading.Tasks;
2. In App.cs, voeg de volgende `IAuthenticate` interface definition direct voor de `App` klassedefinitie.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. Voor het initialiseren van de interface met een platform-specifieke implementatie, de volgende statische leden toevoegen aan de **App** klasse.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. TodoList.xaml openen vanuit het project draagbare klassenbibliotheek, voeg de volgende **knop** -element in de *buttonsPanel* lay-out-element, na de bestaande knop:

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    Deze knop wordt verificatie server beheerd met de back-end van uw mobiele app geactiveerd.
5. TodoList.xaml.cs openen vanuit het project draagbare klassenbibliotheek, en vervolgens het volgende veld toevoegen aan de `TodoList` klasse:

        // Track whether the user has authenticated.
        bool authenticated = false;
6. Vervang de **OnAppearing** methode met de volgende code:

        protected override async void OnAppearing()
        {
            base.OnAppearing();

            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Deze code zorgt ervoor dat gegevens alleen worden vernieuwd vanuit de service nadat u hebt geverifieerd.
7. Voeg de volgende-handler voor de **op wordt geklikt** gebeurtenis om de **TodoList** klasse:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. Uw wijzigingen opslaan en opnieuw maken van het draagbare klassebibliotheek project zonder fouten controleren.

## <a name="add-authentication-to-the-android-app"></a>Verificatie toevoegen aan de Android-app
Deze sectie wordt beschreven hoe u implementeert de **IAuthenticate** interface in het Android-app-project. Deze sectie overslaan als u geen ondersteuning voor Android-apparaten bieden.

1. In Visual Studio of Xamarin Studio, met de rechtermuisknop op de **droid** project, klikt u vervolgens **Set as StartUp Project**.
2. Druk op F5 om het project in het foutopsporingsprogramma start en vervolgens controleren of dat een niet-verwerkte uitzondering met een statuscode 401 (niet-gemachtigd) wordt gegenereerd na de app wordt gestart. De 401 code wordt gemaakt, omdat toegang op de back-end beperkt tot alleen geautoriseerde gebruikers is.
3. Open MainActivity.cs in het Android-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Update de **MainActivity** klasse voor het implementeren van de **IAuthenticate** interface, als volgt:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. Update de **MainActivity** klasse door toe te voegen een **MobileServiceUser** veld en een **verifiëren** methode, die vereist wordt door de **IAuthenticate** interface, als volgt:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this, 
                    MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }

    Als u van een id-provider dan Facebook gebruikmaakt, kiest u een andere waarde voor [MobileServiceAuthenticationProvider][7].

6. Update de **AndroidManifest.xml** bestand door toe te voegen van de volgende XML-code binnen de `<application>` element:

    ```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
      </intent-filter>
    </activity>
    ```
    Vervang `{url_scheme_of_your_app}` met uw URL-schema.
7. Voeg de volgende code aan de **OnCreate** -methode van de **MainActivity** klasse voordat de aanroep van `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Deze code zorgt ervoor dat de verificator wordt geïnitialiseerd voordat de belasting voor de app.
8. Opnieuw opbouwen van de app, uitvoeren en meld u aan met de verificatieprovider die u hebt gekozen en controleer of u kunt toegang tot gegevens als een geverifieerde gebruiker.

### <a name="troubleshooting"></a>Problemen oplossen

**De toepassing is vastgelopen met `Java.Lang.NoSuchMethodError: No static method startActivity`**

In sommige gevallen, veroorzaakt een conflict in de voor ondersteuningspakketten die weergegeven als slechts een waarschuwing in de Visual studio, maar het vastlopen van de toepassing met deze uitzondering tijdens runtime. In dit geval moet u ervoor zorgen dat alle ondersteuningspakketten waarnaar wordt verwezen in uw project beschikt over dezelfde versie. De [Azure Mobile Apps NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) heeft `Xamarin.Android.Support.CustomTabs` afhankelijkheid voor Android-platform, dus als uw project gebruikmaakt van nieuwere ondersteuning u pakketten moet dit pakket installeren met de vereiste versie rechtstreeks op het voorkomen van conflicten.

## <a name="add-authentication-to-the-ios-app"></a>Verificatie toevoegen aan de iOS-app
Deze sectie wordt beschreven hoe u implementeert de **IAuthenticate** interface in het iOS-app-project. Als u iOS-apparaten niet ondersteunt, kunt u deze sectie overslaan.

1. In Visual Studio of Xamarin Studio, met de rechtermuisknop op de **iOS** project, klikt u vervolgens **Set as StartUp Project**.
2. Druk op F5 om het project in het foutopsporingsprogramma start en vervolgens controleren of dat een niet-verwerkte uitzondering met een statuscode 401 (niet-gemachtigd) wordt gegenereerd na de app wordt gestart. De 401-respons is gemaakt, omdat toegang op de back-end beperkt tot alleen geautoriseerde gebruikers is.
3. Open AppDelegate.cs in het iOS-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Update de **AppDelegate** klasse voor het implementeren van de **IAuthenticate** interface, als volgt:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. Update de **AppDelegate** klasse door toe te voegen een **MobileServiceUser** veld en een **verifiëren** methode, die vereist wordt door de **IAuthenticate** interface, als volgt:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;
                    }
                }
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();

            return success;
        }

    Als u van een id-provider dan Facebook gebruikmaakt, kiest u een andere waarde voor [MobileServiceAuthenticationProvider].
    
6. Update de **AppDelegate** klasse door toe te voegen de **OpenUrl** methode overbelasten als volgt:

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }
   
7. Voeg de volgende regel code aan de **FinishedLaunching** methode voordat de aanroep van `LoadApplication()`:

        App.Init(this);

    Deze code zorgt ervoor dat de verificator is geïnitialiseerd voordat de app wordt geladen.

8. Open de Info.plist en voeg een **URL-Type**. Instellen de **id** naar een naam van uw keuze, de **URL-schema's** naar de URL-schema voor uw app en de **rol** op None.

9. Opnieuw opbouwen van de app, uitvoeren en meld u aan met de verificatieprovider die u hebt gekozen en controleer of u kunt toegang tot gegevens als een geverifieerde gebruiker.

## <a name="add-authentication-to-windows-10-including-phone-app-projects"></a>Verificatie toevoegen aan projecten voor apps van Windows 10 (inclusief Phone)
Deze sectie wordt beschreven hoe u implementeert de **IAuthenticate** interface in de Windows 10-app-projecten. De stappen die gelden voor Universal Windows Platform (UWP)-projecten, maar met behulp van de **UWP** project (met de vermelde wijzigingen). Deze sectie overslaan als Windows-apparaten worden niet ondersteund.

1. In Visual Studio met de rechtermuisknop op de **UWP** project, klikt u vervolgens **Set as StartUp Project**.
2. Druk op F5 om het project in het foutopsporingsprogramma start en vervolgens controleren of dat een niet-verwerkte uitzondering met een statuscode 401 (niet-gemachtigd) wordt gegenereerd na de app wordt gestart. De 401-respons komt doordat de toegang tot de back-end is beperkt tot alleen geautoriseerde gebruikers.
3. MainPage.xaml.cs openen voor het Windows-app-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Vervang `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare klassenbibliotheek vormt.
4. Update de **MainPage** klasse voor het implementeren van de **IAuthenticate** interface, als volgt:

        public sealed partial class MainPage : IAuthenticate
5. Update de **MainPage** klasse door toe te voegen een **MobileServiceUser** veld en een **verifiëren** methode, die vereist wordt door de **IAuthenticate**interface, als volgt:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }

            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }

    Als u van een id-provider dan Facebook gebruikmaakt, kiest u een andere waarde voor [MobileServiceAuthenticationProvider][7].

1. Voeg de volgende regel code toe in de constructor voor het **MainPage** klasse voordat de aanroep van `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    Vervang `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare klassenbibliotheek vormt.

3. Als u **UWP**, voeg de volgende **OnActivated** methode overschrijven naar de **App** klasse:

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }
       }

3. Open Package.appxmanifest en voeg een **Protocol** declaratie. Stelt de **weergavenaam** naar een naam van uw keuze, en de **naam** naar de URL-schema van uw app.

4. Opnieuw opbouwen van de app, uitvoeren en meld u aan met de verificatieprovider die u hebt gekozen en controleer of u kunt toegang tot gegevens als een geverifieerde gebruiker.

## <a name="next-steps"></a>Volgende stappen
Nu dat u basisverificatie-zelfstudie hebt voltooid, kunt u doorgaan naar een van de volgende zelfstudies:

* [Pushmeldingen toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-push.md)

  Informatie over het toevoegen van ondersteuning van pushmeldingen aan uw app en het configureren van de backend voor mobiele apps voor gebruik van Azure Notification Hubs voor het verzenden van pushmeldingen.
* [Offlinesynchronisatie voor uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  Informatie over het toevoegen van offlineondersteuning aan uw app met een back-end voor mobiele apps. Offlinesynchronisatie kunnen eindgebruikers om te communiceren met een mobiele app - weergeven, toevoegen of wijzigen van gegevens -, zelfs wanneer er geen netwerkverbinding.

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
[8]: https://portal.azure.com
