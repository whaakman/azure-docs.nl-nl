---
title: Aan de slag met verificatie voor Mobile Apps in Xamarin Android
description: Informatie over het gebruik van Mobile Apps om gebruikers van uw Xamarin.android-app via een groot aantal id-providers, met inbegrip van AAD, Google, Facebook, Twitter en Microsoft te verifiëren.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: panarasi
editor: ''
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 09/24/2018
ms.author: panarasi
ms.openlocfilehash: 0a2d964d60d13f0e71de5776112a4edbe3cdcc45
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57993914"
---
# <a name="add-authentication-to-your-xamarinandroid-app"></a>Verificatie toevoegen aan uw Xamarin.Android-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Dit onderwerp ziet u hoe u verifieert gebruikers van een mobiele App van uw clienttoepassing. In deze zelfstudie, kunt u verificatie toevoegen aan de Quick Start-project met behulp van een id-provider die wordt ondersteund door Azure Mobile Apps. Na wordt is geverifieerd en toegestaan in de mobiele App, wordt de waarde van de gebruiker-ID weergegeven.

In deze zelfstudie is gebaseerd op de mobiele App Quick Start. U moet ook eerst de zelfstudie hebt voltooid [Een Xamarin.Android-app maken]. Als u het gedownloade quick start-serverproject niet gebruikt, moet u het pakket voor verificatie-extensie toevoegen aan uw project. Zie voor meer informatie over het server-extensiepakketten [werken met de .NET back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register"></a>Uw app registreren voor verificatie en App-Services configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie is vereist dat u een nieuwe URL-schema voor uw app definiëren. Hiermee kunt het verificatiesysteem terug te keren naar uw app nadat het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. Selecteer uw App Service in de [Azure-portal].

2. Klik op de **verificatie / autorisatie** menu-optie.

3. In de **toegestane externe Omleidings-URL's**, voer `url_scheme_of_your_app://easyauth.callback`.  De **url_scheme_of_your_app** in deze reeks wordt het URL-schema voor uw mobiele toepassing.  Het moet normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en beginnen met een letter) volgen.  U moet een notitie van de tekenreeks die u kiest, aangezien u nodig hebt om aan te passen van de code van uw mobiele toepassing met de URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Voer het clientproject op een apparaat of emulator in Visual Studio of Xamarin Studio. Controleer of dat een niet-verwerkte uitzondering met een statuscode 401 (niet-gemachtigd) is gegenereerd nadat de app wordt gestart. Dit komt doordat de app probeert te krijgen tot uw back-end van mobiele App als een niet-geverifieerde gebruiker. De *TodoItem* tabel nu verificatie is vereist.

U wordt vervolgens de client-app op aanvraag resources bijwerken van de back-end van de mobiele App met een geverifieerde gebruiker.

## <a name="add-authentication"></a>Verificatie toevoegen aan de app.
De app wordt bijgewerkt om te vereisen dat gebruikers tikken op de **aanmelden** knop en verifiëren voordat gegevens worden weergegeven.

1. Voeg de volgende code aan de **TodoActivity** klasse:
   
        // Define an authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");
   
                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }
   
        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load the data.
                OnRefreshItemsSelected();
            }
        }
   
    Hiermee maakt u een nieuwe methode voor het verifiëren van een gebruiker en een methode-handler voor een nieuwe **aanmelden** knop. De gebruiker in de bovenstaande voorbeeldcode wordt geverifieerd met behulp van een Facebook-aanmelding. Een dialoogvenster wordt gebruikt om de gebruikers-ID eenmaal geverifieerd weer te geven.
   
   > [!NOTE]
   > Als u een id-provider dan Facebook gebruikt, wijzigt u de waarde die is doorgegeven aan **LoginAsync** boven op een van de volgende: *MicrosoftAccount*, *Twitter*, *Google*, or *WindowsAzureActiveDirectory*.
   > 
   > 
2. In de **OnCreate** methode, verwijderen of opmerking-out van de volgende coderegel:
   
        OnRefreshItemsSelected ();
3. Voeg het volgende toe in het bestand Activity_To_Do.axml *LoginUser* knop definitie voordat u de bestaande *AddItem* knop:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Voeg het volgende element in het bestand van de resources Strings.xml:
   
        <string name="login_button_text">Sign in</string>
5. Open het bestand AndroidManifest.xml, voeg de volgende code in `<application>` XML-element:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. Voer het clientproject uit op een apparaat of emulator in Visual Studio of Xamarin Studio en meld u aan met uw gekozen identiteitsprovider. Wanneer u is aangemeld bent, wordt de app uw aanmeldings-ID en de lijst met todo-items worden weergegeven en kunt u updates aanbrengen in de gegevens.

## <a name="troubleshooting"></a>Problemen oplossen

**De toepassing is vastgelopen met `Java.Lang.NoSuchMethodError: No static method startActivity`**

In sommige gevallen, veroorzaakt een conflict in de voor ondersteuningspakketten die weergegeven als slechts een waarschuwing in de Visual studio, maar het vastlopen van de toepassing met deze uitzondering tijdens runtime. In dit geval moet u ervoor zorgen dat alle ondersteuningspakketten waarnaar wordt verwezen in uw project beschikt over dezelfde versie. Het [Azure Mobile Apps NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) heeft `Xamarin.Android.Support.CustomTabs`-afhankelijkheid voor het Android-platform, dus als uw project gebruikmaakt van nieuwere ondersteuningspakketten, moet u dit pakket met de vereiste versie rechtstreeks installeren om conflicten te voorkomen.

<!-- URLs. -->
[Een Xamarin.Android-app maken]: app-service-mobile-xamarin-android-get-started.md
