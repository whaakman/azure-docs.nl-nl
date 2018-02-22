---
title: Pushmeldingen toevoegen aan een Apache Cordova-app met de functie Mobile Apps van Azure App Service | Microsoft Docs
description: Informatie over het gebruik van mobiele Apps om pushmeldingen te verzenden aan uw Apache Cordova-app.
services: app-service\mobile
documentationcenter: javascript
manager: crdun
editor: 
author: conceptdev
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 6af5fa51f2e6553431b9f0aa2dbb368651e7e209
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Pushmeldingen toevoegen aan uw Apache Cordova-app
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie hebt u pushmeldingen toevoegen de [Apache Cordova-Quick Start] [ 5] project, zodat een pushmelding wordt verzonden naar het apparaat telkens wanneer een record wordt ingevoegd.

Als u het gedownloade Quick Start-serverproject niet gebruikt, moet u het push notification-uitbreidingspakket. Zie voor meer informatie [werken met de .NET-back-end-server SDK voor Mobile Apps][1].

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie wordt ervan uitgegaan dat u hebt een Apache Cordova-toepassing die is ontwikkeld met Visual Studio 2015. Dit apparaat moet uitvoeren op Google Android-Emulator, een Android-apparaat, een Windows-apparaat of een iOS-apparaat.

Voor deze zelfstudie hebt u het volgende nodig:

* Een PC met [Visual Studio Community 2015] [ 2] of hoger 
* [Visual Studio Tools voor Apache Cordova][4]
* Een [actief Azure-account][3]
* Een voltooide [Apache Cordova-Quick Start] [ 5] project
* (Android) Een [Google-account] [ 6] met een geverifieerde e-mailadres
* (iOS) Een [Apple Developer Program-lidmaatschap] [ 7] en een iOS-apparaat (iOS-Simulator biedt geen ondersteuning voor pushmeldingen)
* (Windows) Een [Windows Store-ontwikkelaarsaccount] [ 8] en een Windows 10-apparaat

## <a name="configure-hub"></a>Een notification hub configureren
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Bekijk een video waarin de stappen in deze sectie][9].

## <a name="update-the-server-project"></a>Het serverproject bijwerken
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="add-push-to-app"></a>Uw Cordova-app wijzigen
Om ervoor te zorgen dat uw Apache Cordova-app-project gereed is om pushmeldingen te verwerken, de Cordova-invoegtoepassing push plus de platform-specifieke pushservices te installeren.

#### <a name="update-the-cordova-version-in-your-project"></a>Werk de Cordova-versie in uw project.
Werk het clientproject als uw project gebruikt een versie van Apache Cordova die ouder is dan de versie 6.1.1. Voor het bijwerken van het project, moet u de volgende stappen uitvoeren: 

* De configuratie om designer te openen, met de rechtermuisknop op `config.xml`.
* Selecteer de **Platforms** tabblad.
* In de **Cordova CLI** in het tekstvak, selecteer **6.1.1**. 
* Voor het bijwerken van het project, selecteer **bouwen**, en selecteer vervolgens **Build Solution**.

#### <a name="install-the-push-plugin"></a>De push-invoegtoepassing installeren
Apache Cordova-toepassingen verwerken niet systeemeigen apparaat- of -mogelijkheden.  Deze mogelijkheden worden geleverd door invoegtoepassingen die zijn gepubliceerd op [npm] [ 10] of op GitHub. De `phonegap-plugin-push` invoegtoepassing netwerk pushmeldingen verwerkt.

U kunt de push-invoegtoepassing installeren in een van de volgende manieren:

**Vanaf de opdrachtprompt:**

Voer de volgende opdracht uit:

    cordova plugin add phonegap-plugin-push

**Uit vanuit Visual Studio:**

1. Open in Solution Explorer de `config.xml` bestand. Selecteer vervolgens **Plugins** > **aangepaste**. Selecteer vervolgens **Git** als de installatiebron. 
    
2. Voer `https://github.com/phonegap/phonegap-plugin-push` als de bron.

    ![Open het bestand config.xml in Solution Explorer][img1]

3. Selecteer de pijl naast de installatiebron.

4. In **SENDER_ID**, hebt u al een numerieke project-ID voor het project Google Developer-Console, u kunt u deze hier toevoegen. Anders, voer een aanduidingswaarde van de tijdelijke, zoals 777777. Als u voor Android ontwikkelt, kunt u deze waarde in het bestand config.xml later bijwerken.

    >[!NOTE]
    >Vanaf versie 2.0.0 moet google services.json worden geïnstalleerd in de hoofdmap van uw project voor het configureren van de afzender-ID. Zie voor meer informatie de [documentatie voor de installatie.](https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md)
5. Selecteer **Toevoegen**.

De push-invoegtoepassing is nu geïnstalleerd.

#### <a name="install-the-device-plugin"></a>De apparaat-invoegtoepassing installeren
Volg dezelfde procedure die u gebruikt voor het installeren van de push-invoegtoepassing. De apparaat-invoegtoepassing uit de lijst van de invoegtoepassingen Core toevoegen. (Als u wilt vinden, selecteer **Plugins** > **Core**.) U moet deze invoegtoepassing om de platformnaam te verkrijgen.

#### <a name="register-your-device-when-the-application-starts"></a>Registreer uw apparaat wanneer de toepassing wordt gestart 
We in eerste instantie opnemen minimale code voor Android. Later kunt u de app uit te voeren op iOS- of Windows 10.

1. Voeg een aanroep naar **registerForPushNotifications** tijdens de retouraanroep voor het proces aanmelden. U kunt ook kunt u deze toevoegen aan de onderkant van de **onDeviceReady** methode:

        // Log in to the service.
        client.login('google')
            .then(function () {
                // Create a table reference.
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems.
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item.
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    In dit voorbeeld ziet aanroepen **registerForPushNotifications** nadat verificatie is geslaagd. U kunt aanroepen `registerForPushNotifications()` zo vaak als nodig is.

2. Toevoegen van de nieuwe **registerForPushNotifications** methode als volgt:

        // Register for push notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }
3. (Android) Vervang in de bovenstaande code `Your_Project_ID` project met de numerieke ID voor uw app uit de [Google Developer-Console][18].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Optioneel) Configureren en uitvoeren van de app voor Android
Deze sectie om in te schakelen pushmeldingen voor Android worden voltooid.

#### <a name="enable-gcm"></a>Inschakelen van Firebase Cloud-berichten
Nadat u de Google Android-platform in eerste instantie ontwikkelt, moet u de Firebase Cloud Messaging inschakelen.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-backend"></a>De back-end voor mobiele App voor het verzenden van push-aanvragen via van FCM configureren
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Configureer uw Cordova-app voor Android
Open in uw Cordova-app config.xml. Vervang `Your_Project_ID` project met de numerieke ID voor uw app uit de [Google Developer-Console][18].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Open index.js. Werk vervolgens de code voor het gebruik van uw numerieke project-ID.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <a name="configure-device"></a>Uw Android-apparaat voor USB-foutopsporing configureren
Voordat u uw toepassing op uw Android-apparaat implementeren kunt, moet u de USB-foutopsporing inschakelen. Voer de volgende stappen uit op uw Android-telefoon:

1. Ga naar **instellingen** > **over telefoon**. Tik vervolgens op de **Build-nummer** totdat modus voor ontwikkelaars (ongeveer zeven maal) is ingeschakeld.
2. Terug in de **instellingen** > **opties voor ontwikkelaars**, inschakelen **USB-foutopsporing**. Sluit uw Android-telefoon aan de ontwikkeling van een PC met een USB-kabel.

We hebben getest dit met behulp van een Google-Nexus 5 X-apparaat met Android 6.0 (Marshmallow). De technieken zijn echter algemene binnen een moderne Android release.

#### <a name="install-google-play-services"></a>Installeer Google Play-Services
De push-invoegtoepassing is afhankelijk van Android Google Play Services voor pushmeldingen.

1. Selecteer in Visual Studio **extra** > **Android** > **Android SDK Manager**. Vouw de **extra's** map. De toepasselijke selectievakjes in om ervoor te zorgen dat elk van de volgende SDK's is geïnstalleerd:

   * Android 2.3 of hoger
   * Google-opslagplaats revisie 27 of hoger
   * Google Play Services 9.0.2 of hoger

2. Selecteer **pakketten installeren**. Wacht totdat de installatie is voltooid.

De huidige vereiste bibliotheken worden vermeld in de [phonegap-invoegtoepassing-push-installatie documentatie][19].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Pushmeldingen testen in de app voor Android
U kunt nu test pushmeldingen door het uitvoeren van de app en het invoegen van items in de takentabel. U kunt van hetzelfde apparaat of van een tweede apparaat testen, zolang u de dezelfde back-end gebruikt. Test uw Cordova-app op het Android-platform in een van de volgende manieren:

* *Op een fysiek apparaat:* uw Android-apparaat koppelen aan uw ontwikkelcomputer waarop een USB-kabel.  In plaats van **Google Android-Emulator**, selecteer **apparaat**. Visual Studio implementeert de toepassing op het apparaat en de toepassing uitvoert. U kunt vervolgens met de toepassing op het apparaat werken.

  Scherm delen toepassingen, zoals [Mobizen] [ 20] kan u helpen bij het ontwikkelen van Android-toepassingen. Het scherm van uw Android aan een webbrowser projecteert Mobizen op uw PC.

* *Op een Android-emulator:* er zijn extra configuratiestappen uitvoeren die vereist zijn wanneer u een emulator.

    Zorg ervoor dat u implementeert op een virtueel apparaat met Google APIs ingesteld als doel, zoals wordt weergegeven in de Android Virtual Device (AVD) manager.

    ![Android Virtual Device Manager](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Als u wilt gebruiken een snellere x86 emulator [Installeer het stuurprogramma HAXM][11], en configureer vervolgens de emulator om dit te gebruiken.

    Een Google-account toevoegen aan de Android-apparaat door te selecteren **Apps** > **instellingen** > **account toevoegen**. Volg de aanwijzingen.

    ![Een Google-account toevoegen aan de Android-apparaat](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Voer de todolist-app als voordat en een nieuwe taak invoegen. Dit moment wordt een pictogram weergegeven in het systeemvak. U kunt de meldingenlijst om weer te geven van de volledige tekst van de melding openen.

    ![melding weergeven](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(Optioneel) Configureren en uitvoeren op iOS
Deze sectie is voor het uitvoeren van de Cordova-project op iOS-apparaten. Als u met de iOS-apparaten worden niet werkt, kunt u deze sectie overslaan.

#### <a name="install-and-run-the-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>Installeren en uitvoeren van de iOS externe build-agent op een Mac- of cloud service
Voordat u een Cordova-app voor iOS met Visual Studio uitvoeren kunt, gaat u door de stappen in de [installatiehandleiding voor iOS] [ 12] wilt installeren en uitvoeren van de externe build-agent.

Zorg ervoor dat u de app voor iOS kunt samenstellen. De stappen in de setup guide zijn vereist voor het bouwen van de app voor iOS vanuit Visual Studio. Als u een Mac niet hebt, kunt u met behulp van de externe build-agent op een service, zoals MacInCloud voor iOS maken. Zie voor meer informatie [uitvoeren van uw iOS-app in de cloud][21].

> [!NOTE]
> XCode 7 of hoger is vereist voor de push-invoegtoepassing in iOS gebruiken.

#### <a name="find-the-id-to-use-as-your-app-id"></a>De ID te gebruiken als uw App-ID vinden
Voordat u uw app voor pushmeldingen, open config.xml in uw Cordova-app registreren, vindt de `id` kenmerk waarde in het element widget en kopieer deze vervolgens voor later gebruik. In het volgende XML-bestand, de ID is `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Deze id later gebruiken bij het maken van een App-ID op van Apple developer portal. Als u een andere App-ID in de portal voor ontwikkelaars maakt, moet u rekening houden met een paar extra stappen verderop in deze zelfstudie. De ID in het element widget moet overeenkomen met de App-ID op de portal voor ontwikkelaars.

#### <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>De app voor pushmeldingen op Apple developer-portal te registreren
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Bekijk een video van vergelijkbare stappen](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-to-send-push-notifications"></a>Configureren van Azure om pushmeldingen te verzenden
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a>Controleren of uw App-ID overeenkomt met uw Cordova-app
Als de App-ID die u hebt gemaakt in uw Apple Developer-Account al overeenkomt met de ID van de widget-element in het bestand config.xml, kunt u deze stap overslaan. Als de id's niet overeenkomen, voert u de volgende stappen uit:

1. Verwijder de map platforms van uw project.
2. Verwijder de map plugins van uw project.
3. Verwijder de map node_modules van uw project.
4. Bijwerken van het kenmerk id van de widget-element in het bestand config.xml om de app-ID die u hebt gemaakt in uw Apple developer-account te gebruiken.
5. Bouw het project opnieuw op.

##### <a name="test-push-notifications-in-your-ios-app"></a>Pushmeldingen testen in uw iOS-app
1. Controleer of in Visual Studio **iOS** als het implementatiedoel is geselecteerd. Selecteer vervolgens **apparaat** voor het uitvoeren van de pushmeldingen op het verbonden iOS-apparaat.

    U kunt de pushmeldingen uitvoeren op een iOS-apparaat verbonden met uw PC met iTunes. De iOS-Simulator biedt geen ondersteuning voor pushmeldingen.

2. Selecteer de **uitvoeren** knop of **F5** in Visual Studio om het project bouwen en starten van de app in een iOS-apparaat. Selecteer vervolgens **OK** pushmeldingen accepteren.

   > [!NOTE]
   > De app vraagt om bevestiging voor pushmeldingen tijdens de eerste keer uitvoert.

3. Typ een taak in de app en selecteer vervolgens het plusteken **(+)** pictogram.
4. Controleer of is een melding ontvangen. Selecteer vervolgens **OK** kunnen worden verwijderd van de melding.

## <a name="optional-configure-and-run-on-windows"></a>(Optioneel) Configureren en uitvoeren in Windows
Deze sectie beschrijft het uitvoeren van de Apache Cordova-app-project op Windows 10-apparaten (de PhoneGap-push-invoegtoepassing wordt ondersteund op Windows 10). Als u niet met Windows-apparaten werkt, kunt u deze sectie overslaan.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>Uw Windows-app voor pushmeldingen registreren met WNS
Als u de Store-opties in Visual Studio, selecteer een Windows-doel in de lijst oplossing Platforms zoals **Windows x64** of **Windows x86**. (Voorkomen **Windows AnyCPU** voor pushmeldingen.)

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Bekijk een video van gelijksoortige stappen][13]

#### <a name="configure-the-notification-hub-for-wns"></a>De notification hub configureren voor WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Configureer uw Cordova-app om ondersteuning voor pushmeldingen voor Windows
Open de configuration designer met de rechtermuisknop op **config.xml**. Selecteer vervolgens **ontwerper**. Selecteer vervolgens de **Windows** tabblad en selecteer vervolgens **Windows 10** onder **Windows doelversie**.

Ter ondersteuning van pushmeldingen in uw standaard (debug) bouwt, open het bestand build.json. Kopieer de "release"-configuratie aan uw configuratie voor foutopsporing.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Nadat de update, moet het bestand build.json bevatten de volgende code:

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

De app bouwen en te controleren of er geen fouten. Uw client-app moet zich nu registreren voor de meldingen van de back-end van Mobile Apps. Herhaal deze sectie voor elke Windows-project in uw oplossing.

#### <a name="test-push-notifications-in-your-windows-app"></a>Pushmeldingen testen in uw Windows-app
In Visual Studio, zorg ervoor dat een Windows-platform is geselecteerd als het implementatiedoel zoals **Windows x64** of **Windows x86**. Als u wilt uitvoeren op een Windows 10-PC die als host voor Visual Studio fungeert, kies **lokale Machine**.

1. Selecteer de **uitvoeren** om het project bouwen en de app te starten.

2. Typ een naam voor een nieuwe taken in de app en selecteer vervolgens het plusteken **(+)** pictogram toe te voegen.

Controleer of dat er een melding wordt ontvangen wanneer het item is toegevoegd.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Notification Hubs] [ 17] voor meer informatie over pushmeldingen.
* Als u dit nog niet hebt gedaan, blijven de zelfstudie door [toevoegen authentication] [ 14] aan uw Apache Cordova-app.

Meer informatie over het gebruik van de volgende SDK's:

* [Apache Cordova SDK][15]
* [ASP.NET Server SDK][1]
* [Node.js Server SDK][16]

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: http://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: http://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: http://taco.visualstudio.com/en-us/docs/build_ios_cloud/
