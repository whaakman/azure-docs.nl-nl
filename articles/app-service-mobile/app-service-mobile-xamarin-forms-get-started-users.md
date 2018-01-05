---
title: Aan de slag met verificatie voor mobiele Apps in app voor Xamarin Forms | Microsoft Docs
description: "Informatie over het verifiëren van gebruikers van uw app voor Xamarin Forms via een groot aantal identiteitsproviders, waaronder AAD, Google, Facebook, Twitter en Microsoft met Mobile Apps."
services: app-service\mobile
documentationcenter: xamarin
author: panarasi
manager: crdun
editor: 
ms.assetid: 9c55e192-c761-4ff2-8d88-72260e9f6179
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: panarasi
ms.openlocfilehash: e3e8c843437558c6d5d3a3c39bed1e647f852b18
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="add-authentication-to-your-xamarin-forms-app"></a>Verificatie toevoegen aan uw app voor Xamarin Forms
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Overzicht
Dit onderwerp leest u hoe u verifieert gebruikers van een App Service Mobile App van uw clienttoepassing. In deze zelfstudie kunt u verificatie toevoegen aan de Xamarin Forms Quick Start-project met behulp van een id-provider die wordt ondersteund door App Service. Nadat wordt is geverifieerd en geautoriseerd door uw mobiele App, de waarde van de gebruiker-ID wordt weergegeven en u kunt toegang krijgen tot gegevens in een beperkte tabel.

## <a name="prerequisites"></a>Vereisten
Voor het beste resultaat in deze zelfstudie, raden we u de [maken van een app voor Xamarin Forms] [ 1] zelfstudie. Nadat u deze zelfstudie hebt voltooid, hebt u een Xamarin Forms-project dat een TodoList-App voor meerdere platforms.

Als u het gedownloade quick start-serverproject niet gebruikt, moet u het pakket voor verificatie-extensie toevoegen aan uw project. Zie voor meer informatie over server extensiepakketten [werken met de .NET-back-endserver SDK voor Azure Mobile Apps][2].

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Uw app registreren voor verificatie en App-Services configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie vereist dat u een nieuwe URL-schema voor uw app definiëren. Hierdoor kan de verificatiesysteem terug te keren naar uw app zodra het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. In de [Azure-portal][8], selecteer uw App Service.

2. Klik op de **verificatie / autorisatie** menuoptie.

3. In de **toegestaan externe Omleidings-URL's**, voer `url_scheme_of_your_app://easyauth.callback`.  De **url_scheme_of_your_app** in deze tekenreeks wordt het URL-schema voor uw mobiele toepassing.  Deze moet voldoen aan de normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en begin met een letter).  U moet een notitie van de tekenreeks die u naar wens aanpassen van uw mobiele toepassingscode met het URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="restrict-permissions-to-authenticated-users"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-to-the-portable-class-library"></a>Verificatie toevoegen aan de draagbare klassebibliotheek
Maakt gebruik van mobiele Apps van de [LoginAsync] [ 3] extensiemethode op de [MobileServiceClient] [ 4] aanmelden met een gebruiker met App Service-verificatie. Dit voorbeeld maakt gebruik van een beheerde server authenticatiestroom die wordt weergegeven van de provider aanmelden interface in de app. Zie voor meer informatie [authentication Server beheerd][5]. Voor een betere gebruikerservaring in uw productie-app, moet u rekening houden met gebruik van [Client beheerd verificatie][6].

Om te verifiëren met een project Xamarin Forms, definiëren een **IAuthenticate** interface in de draagbare klassebibliotheek voor de app. Voeg vervolgens een **aanmelden** knop aan de gebruikersinterface is gedefinieerd in de draagbare klassebibliotheek u verificatie starten. Gegevens worden geladen vanuit de back-end voor de mobiele app na een geslaagde authenticatie.

Implementeer de **IAuthenticate** interface voor elk platform dat wordt ondersteund door uw app.

1. Open in Visual Studio of Xamarin Studio App.cs uit het project met **draagbare** in de naam die Portable Class Library-project, voegt u de volgende `using` instructie:

        using System.Threading.Tasks;
2. In App.cs, voeg de volgende `IAuthenticate` interface definition direct voor de `App` klasse definitie.

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
4. TodoList.xaml openen vanuit het Portable Class Library-project, voeg de volgende **knop** -element in de *buttonsPanel* lay-element, na de bestaande knop:

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    Deze knop activeert verificatie met uw mobiele app back-end server beheerd.
5. TodoList.xaml.cs openen vanuit het Portable Class Library-project, voeg dan het volgende veld naar de `TodoList` klasse:

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

    Deze code zorgt ervoor dat de gegevens alleen vernieuwd van de service nadat u hebt geverifieerd.
7. Voeg de volgende handler voor de **op wordt geklikt** gebeurtenis om de **TodoList** klasse:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. De wijzigingen opslaan en opnieuw opbouwen van het Portable Class Library-project zonder fouten te controleren.

## <a name="add-authentication-to-the-android-app"></a>Verificatie toevoegen aan de Android-app
Deze sectie wordt beschreven hoe u implementeert de **IAuthenticate** interface in het Android-app-project. Deze sectie overslaan als Android-apparaten worden niet ondersteund.

1. In Visual Studio of Xamarin Studio, met de rechtermuisknop op de **droid** project, klikt u vervolgens **instellen als opstartproject**.
2. Druk op F5 om het project in het foutopsporingsprogramma start en vervolgens controleren of een niet-verwerkte uitzondering met een statuscode van 401 (niet-geautoriseerd) wordt gegenereerd na de app wordt gestart. De 401 code wordt gemaakt, omdat de toegang op de back-end is beperkt tot alleen geautoriseerde gebruikers.
3. MainActivity.cs openen in de Android-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Update de **MainActivity** klasse voor het implementeren van de **IAuthenticate** interface, als volgt:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. Update de **MainActivity** klasse door toe te voegen een **MobileServiceUser** veld en een **verifiëren** methode die vereist wordt door de **IAuthenticate** interface, als volgt:

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

6. Update de **AndroidManifest.xml** bestand door het toevoegen van de volgende XML-code binnen de `<application>` element:

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
    Vervang `{url_scheme_of_your_app}` met URL-schema.
7. Voeg de volgende code naar de **OnCreate** methode van de **MainActivity** klasse voordat de aanroep van `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Deze code zorgt ervoor dat de verificator wordt geïnitialiseerd voordat de app-belastingen.
8. De app opnieuw samenstellen, uitvoeren en aanmelden met de verificatieprovider die u hebt gekozen en controleer of u kunt toegang krijgen tot gegevens als een geverifieerde gebruiker.

## <a name="add-authentication-to-the-ios-app"></a>Verificatie toevoegen aan de iOS-app
Deze sectie wordt beschreven hoe u implementeert de **IAuthenticate** interface in het iOS-app-project. Deze sectie overslaan als iOS-apparaten worden niet ondersteund.

1. In Visual Studio of Xamarin Studio, met de rechtermuisknop op de **iOS** project, klikt u vervolgens **instellen als opstartproject**.
2. Druk op F5 om het project in het foutopsporingsprogramma start en vervolgens controleren of een niet-verwerkte uitzondering met een statuscode van 401 (niet-geautoriseerd) wordt gegenereerd na de app wordt gestart. De 401-respons wordt gemaakt, omdat de toegang op de back-end is beperkt tot alleen geautoriseerde gebruikers.
3. AppDelegate.cs openen in de iOS-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Update de **AppDelegate** klasse voor het implementeren van de **IAuthenticate** interface, als volgt:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. Update de **AppDelegate** klasse door toe te voegen een **MobileServiceUser** veld en een **verifiëren** methode die vereist wordt door de **IAuthenticate** interface, als volgt:

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
    
6. Update de **AppDelegate** klasse door toe te voegen de **OpenUrl** methode overbelasting als volgt:

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }
   
7. Voeg de volgende regel code aan de **FinishedLaunching** methode voordat de aanroep van `LoadApplication()`:

        App.Init(this);

    Deze code zorgt ervoor dat de verificator is geïnitialiseerd voordat de app wordt geladen.

8. Open Info.plist en voeg een **URL-Type**. Instellen de **id** in een naam van uw keuze, de **URL-schema's** naar de URL-schema voor uw app en de **rol** op None.

9. De app opnieuw samenstellen, uitvoeren en aanmelden met de verificatieprovider die u hebt gekozen en controleer of u kunt toegang krijgen tot gegevens als een geverifieerde gebruiker.

## <a name="add-authentication-to-windows-10-including-phone-app-projects"></a>Verificatie toevoegen aan Windows 10 (inclusief telefoon) app-projecten
Deze sectie wordt beschreven hoe u implementeert de **IAuthenticate** interface in het Windows 10-app-projecten. De stappen die gelden voor Universal Windows Platform (UWP)-projecten, maar met behulp van de **UWP** project (met vermelde wijzigingen). Deze sectie overslaan als Windows-apparaten worden niet ondersteund.

1. In Visual Studio met de rechtermuisknop op de **UWP** project, klikt u vervolgens **instellen als opstartproject**.
2. Druk op F5 om het project in het foutopsporingsprogramma start en vervolgens controleren of een niet-verwerkte uitzondering met een statuscode van 401 (niet-geautoriseerd) wordt gegenereerd na de app wordt gestart. De 401-respons treedt op omdat de toegang op de back-end is beperkt tot alleen geautoriseerde gebruikers.
3. MainPage.xaml.cs openen voor het Windows-app-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Vervang `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare klassebibliotheek.
4. Update de **MainPage** klasse voor het implementeren van de **IAuthenticate** interface, als volgt:

        public sealed partial class MainPage : IAuthenticate
5. Update de **MainPage** klasse door toe te voegen een **MobileServiceUser** veld en een **verifiëren** methode die vereist wordt door de **IAuthenticate** interface, als volgt:

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

1. Voeg de volgende coderegel toe in de constructor voor het **MainPage** klasse voordat de aanroep van `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    Vervang `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare klassebibliotheek.

3. Als u **UWP**, voeg de volgende **OnActivated** methode overschrijven de **App** klasse:

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }
       }

3. Open Package.appxmanifest en voeg een **Protocol** declaratie. Instellen de **weergavenaam** in een naam van uw keuze en de **naam** naar de URL-schema van uw app.

4. De app opnieuw samenstellen, uitvoeren en aanmelden met de verificatieprovider die u hebt gekozen en controleer of u kunt toegang krijgen tot gegevens als een geverifieerde gebruiker.

## <a name="next-steps"></a>Volgende stappen
Nu dat u deze basisverificatie-zelfstudie hebt voltooid, overweeg dan u verder gaat u aan bij een van de volgende zelfstudies:

* [Pushmeldingen toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-push.md)

  Informatie over het toevoegen van ondersteuning van pushmeldingen aan uw app en het configureren van de backend voor mobiele apps voor gebruik van Azure Notification Hubs voor het verzenden van pushmeldingen.
* [Offlinesynchronisatie voor uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  Informatie over het toevoegen van Offlineondersteuning uw app met een back-end voor de mobiele App. Offlinesynchronisatie kan eindgebruikers werken met een mobiele app - weergeven, toevoegen of wijzigen van gegevens -, zelfs wanneer er geen netwerkverbinding.

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
