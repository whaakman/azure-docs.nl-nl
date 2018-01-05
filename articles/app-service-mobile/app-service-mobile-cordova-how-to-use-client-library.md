---
title: Het gebruik van Apache Cordova-invoegtoepassing voor Azure Mobile Apps
description: Het gebruik van Apache Cordova-invoegtoepassing voor Azure Mobile Apps
services: app-service\mobile
documentationcenter: javascript
author: conceptdev
manager: crdun
editor: 
ms.assetid: a56a1ce4-de0c-4f3c-8763-66252c52aa59
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: f166d2e533dc49ca7779b45f3dec57a53c22fc40
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Het gebruik van Apache Cordova-clientbibliotheek voor Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u veelvoorkomende scenario's met behulp van de meest recente uitvoeren [Apache Cordova-invoegtoepassing voor Azure Mobile Apps]. Als u niet bekend met Azure Mobile Apps bent, eerst voltooien [Azure Mobile Apps Quick Start] voor het maken van een back-end van een tabel maken en een vooraf samengestelde Apache Cordova-project hebt gedownload. We richten op de client-side Apache Cordova-invoegtoepassing in deze handleiding.

## <a name="supported-platforms"></a>Ondersteunde platforms
Deze SDK biedt ondersteuning voor Apache Cordova-v6.0.0 en later op iOS, Android en Windows apparaten.  De beschikbare platformondersteuning is als volgt:

* Android-API 19-24 (KitKat via deze).
* iOS 8.0 en hoger.
* Windows Phone 8.1.
* Universele Windows-Platform.

## <a name="Setup"></a>Het installatieprogramma en vereisten
Deze handleiding wordt ervan uitgegaan dat u een back-end hebt gemaakt met een tabel. Deze handleiding wordt ervan uitgegaan dat de tabel heeft van hetzelfde schema als de tabellen in deze zelfstudies. Deze handleiding wordt ervan uitgegaan dat u de Apache Cordova-invoegtoepassing hebt toegevoegd aan uw code.  Als u dit niet gedaan hebt, kunt u de Apache Cordova-invoegtoepassing kunt toevoegen aan uw project op de opdrachtregel:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Voor meer informatie over het maken van [uw eerste Apache Cordova-app], Zie de documentatie.

## <a name="ionic"></a>Instellen van een app ionische v2

Voor een juiste configuratie een ionische v2-project, eerst een eenvoudige app maken en toevoegen van de Cordova-invoegtoepassing:

```
ionic start projectName --v2
cd projectName
ionic plugin add cordova-plugin-ms-azure-mobile-apps
```

Voeg de volgende regels voor `app.component.ts` om de clientobject te maken:

```
declare var WindowsAzure: any;
var client = new WindowsAzure.MobileServiceClient("https://yoursite.azurewebsites.net");
```

U kunt nu bouwen en uitvoeren van het project in de browser:

```
ionic platform add browser
ionic run browser
```

De Azure Mobile Apps Cordova-invoegtoepassing biedt ondersteuning voor beide ionische v1- en v2-apps.  Alleen de apps ionische v2 nodig aanvullende-declaratie voor de `WindowsAzure` object.

[!INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Procedure: verificatie van gebruikers
Azure App Service biedt ondersteuning voor verificatie en autorisatie van app-gebruikers met behulp van verschillende externe id-providers: Facebook, Google, Microsoft-Account en Twitter. U kunt machtigingen instellen voor tabellen toegang voor specifieke bewerkingen voor alleen geverifieerde gebruikers te beperken. U kunt ook de identiteit van de geverifieerde gebruikers gebruiken voor het implementeren van autorisatieregels in server-scripts. Zie voor meer informatie de [aan de slag met verificatie] zelfstudie.

Wanneer u verificatie gebruikt in een Apache Cordova-app, moeten de volgende Cordova-invoegtoepassingen beschikbaar zijn:

* [cordova-invoegtoepassing-apparaat]
* [cordova-invoegtoepassing-inappbrowser]

Twee verificatie stromen worden ondersteund: de stroom van een server en een client-stroom.  De stroom van de server biedt de eenvoudigste verificatie-ervaring, is afhankelijk van de webinterface voor verificatie van de provider. De stroom kunt u betere integratie met apparaatspecifieke mogelijkheden, zoals single-sign-on zoals is afhankelijk van de provider-specifieke apparaat-specifieke SDK's.

[!INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Procedure: het configureren van uw mobiele App Service voor externe Omleidings-URL's.
Verschillende soorten Apache Cordova-toepassingen gebruiken een loopback-functie voor het OAuth UI stromen.  OAuth UI-overdrachten op localhost veroorzaken problemen omdat alleen weet hoe uw service gebruiken standaard de verificatieservice.  Voorbeelden van problematisch OAuth UI stromen zijn:

* De emulator rimpel.
* Live opnieuw laden met ionische.
* De mobiele back-end lokaal uitgevoerd
* De mobiele back-end in een andere Azure App Service dan de één verstrekken verificatie uitgevoerd.

Volg deze instructies voor uw lokale instellingen toevoegen aan de configuratie:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw mobiele App.
3. Klik op **hulpprogramma's**
4. Klik op **resourceverkenner** Klik in het menu OBSERVE **gaat**.  Een nieuw venster of tabblad wordt geopend.
5. Vouw de **config**, **authsettings** knooppunten voor uw site in de linkernavigatiebalk.
6. Klik op **bewerken**
7. Zoek naar het element 'allowedExternalRedirectUrls'.  Het kan worden ingesteld op null of een matrix met waarden.  Wijzig de waarde in de volgende waarde:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    De URL's vervangen door de URL's van uw service.  Voorbeelden zijn "http://localhost: 3000' (voor de Node.js-voorbeeld-service) of 'http://localhost:4400' (voor de service Rimpel).  Deze URL's zijn echter voorbeelden - uw situatie, met inbegrip van voor de services die worden vermeld in de voorbeelden, kunnen afwijken.
8. Klik op de **lezen/schrijven** knop in de rechterbovenhoek van het scherm.
9. Klik op de groene **plaatsen** knop.

De instellingen worden op dit moment opgeslagen.  Het browservenster niet sluiten totdat de instellingen hebt opgeslagen.
Ook deze loopback-URL's toevoegen aan de CORS-instellingen voor uw App Service:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw mobiele App.
3. De blade instellingen automatisch geopend.  Als dit niet zo is, klikt u op **alle instellingen**.
4. Klik op **CORS** in het menu API.
5. Voer de URL die u wilt toevoegen in het vak opgegeven en druk op Enter.
6. Geef extra URL's desgewenst.
7. Klik op **opslaan** de instellingen op te slaan.

Het duurt ongeveer 10-15 seconden voordat de nieuwe instellingen van kracht te laten worden.

## <a name="register-for-push"></a>Hoe: registreren voor pushmeldingen
Installeer de [phonegap-invoegtoepassing-push] voor het afhandelen van pushmeldingen.  Deze invoegtoepassing gemakkelijk kan worden toegevoegd met de `cordova plugin add` opdracht op de opdrachtregel of via het installatieprogramma van de invoegtoepassing Git vanuit Visual Studio.  De volgende code in uw Apache Cordova-app registreert uw apparaat voor pushmeldingen:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

De Notification Hubs SDK gebruiken om pushmeldingen te verzenden van de server.  Nooit pushmeldingen rechtstreeks vanuit clients verzenden. In dat geval kan worden gebruikt voor het activeren van een denial of service-aanval tegen Notification Hubs of de PNS.  De PNS kan verkeer als gevolg van dergelijke aanvallen been.

## <a name="more-information"></a>Meer informatie

U vindt gedetailleerde informatie voor API in onze [API-documentatie](http://azure.github.io/azure-mobile-apps-js-client/).

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com
[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md
[aan de slag met verificatie]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Apache Cordova-invoegtoepassing voor Azure Mobile Apps]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[uw eerste Apache Cordova-app]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-invoegtoepassing-push]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-invoegtoepassing-apparaat]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-invoegtoepassing-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
