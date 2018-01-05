---
title: Aan de slag met verificatie voor mobiele Apps in Xamarin Android
description: "Informatie over het verifiëren van gebruikers van uw Xamarin.android-app via een groot aantal identiteitsproviders, waaronder AAD, Google, Facebook, Twitter en Microsoft met Mobile Apps."
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: panarasi
editor: 
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 97207b722b65ccf98c57304cd559b0927aacd5a4
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="add-authentication-to-your-xamarinandroid-app"></a>Verificatie toevoegen aan uw Xamarin.Android-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Dit onderwerp leest u hoe u verifieert gebruikers van een mobiele App van uw clienttoepassing. In deze zelfstudie kunt u verificatie toevoegen aan de Quick Start-project met behulp van een id-provider die wordt ondersteund door Azure Mobile Apps. Nadat wordt hebt geverifieerd en gemachtigd in de mobiele App, wordt de waarde van de gebruiker-ID weergegeven.

Deze zelfstudie is gebaseerd op de mobiele App Quick Start. U moet ook eerst Voltooi de zelfstudie [een Xamarin.Android-app maken]. Als u het gedownloade quick start-serverproject niet gebruikt, moet u het pakket voor verificatie-extensie toevoegen aan uw project. Zie voor meer informatie over server extensiepakketten [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register"></a>Uw app registreren voor verificatie en App-Services configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie vereist dat u een nieuwe URL-schema voor uw app definiëren. Hierdoor kan de verificatiesysteem terug te keren naar uw app zodra het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. Selecteer in de [Azure-portal] uw App Service.

2. Klik op de **verificatie / autorisatie** menuoptie.

3. In de **toegestaan externe Omleidings-URL's**, voer `url_scheme_of_your_app://easyauth.callback`.  De **url_scheme_of_your_app** in deze tekenreeks wordt het URL-schema voor uw mobiele toepassing.  Deze moet voldoen aan de normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en begin met een letter).  U moet een notitie van de tekenreeks die u naar wens aanpassen van uw mobiele toepassingscode met het URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Voer het clientproject op een apparaat of emulator in Visual Studio en Xamarin Studio. Controleer of dat een niet-verwerkte uitzondering met een statuscode van 401 (niet-geautoriseerd) treedt op nadat de app wordt gestart. Dit gebeurt omdat de app probeert te krijgen tot uw back-end voor mobiele App als een niet-geverifieerde gebruiker. De *TodoItem* tabel nu is verificatie vereist.

U wordt vervolgens de client-app voor aanvragen voor resources bijwerken vanuit de back-end voor de mobiele App met een geverifieerde gebruiker.

## <a name="add-authentication"></a>Verificatie toevoegen aan de app.
De app is bijgewerkt om te vereisen dat gebruikers tikt u op de **aanmelden** knop en worden geverifieerd voordat gegevens worden weergegeven.

1. Voeg de volgende code naar de **TodoActivity** klasse:
   
        // Define a authenticated user.
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
   
    Hiermee maakt u een nieuwe methode voor het verifiëren van een gebruiker en een methode-handler voor een nieuwe **aanmelden** knop. De gebruiker in de bovenstaande voorbeeldcode is geverifieerd met behulp van een Facebook-aanmelding. Een dialoogvenster wordt gebruikt voor het weergeven van de gebruikers-ID eenmaal is geverifieerd.
   
   > [!NOTE]
   > Als u van een id-provider dan Facebook gebruikmaakt, wijzig de waarde die is doorgegeven aan **LoginAsync** boven op een van de volgende opties: *MicrosoftAccount*, *Twitter*,  *Google*, of *WindowsAzureActiveDirectory*.
   > 
   > 
2. In de **OnCreate** methode, verwijderen of commentarieer de volgende regel code:
   
        OnRefreshItemsSelected ();
3. Voeg de volgende in het bestand Activity_To_Do.axml *LoginUser* knop definitie voordat u de bestaande *AddItem* knop:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Voeg het volgende element naar het bronbestand Strings.xml:
   
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

6. In Visual Studio en Xamarin Studio het clientproject uitvoeren op een apparaat of emulator en meld u aan met uw gekozen id-provider. Wanneer u is aangemeld bent, wordt de app uw aanmeldings-ID en de lijst met todo-items worden weergegeven en kunt u updates aanbrengen in de gegevens.

<!-- URLs. -->
[een Xamarin.Android-app maken]: app-service-mobile-xamarin-android-get-started.md