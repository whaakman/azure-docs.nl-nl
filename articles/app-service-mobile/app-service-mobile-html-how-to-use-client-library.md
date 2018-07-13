---
title: Over het gebruik van de JavaScript-SDK voor Azure Mobile Apps
description: Over het gebruik v voor Azure Mobile Apps
services: app-service\mobile
documentationcenter: javascript
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: ba2eb5a7f888e4cffcd798259afa8194b4021025
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38488889"
---
# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Het gebruik van de JavaScript-clientbibliotheek voor Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u hoe u algemene scenario's met behulp van de meest recente uitvoert [JavaScript-SDK voor Azure Mobile Apps]. Als u niet bekend bent met Azure Mobile Apps, voltooi eerst [Azure Mobile Apps snel starten] om te maken van een back-end en een tabel maken. In deze handleiding richten we over het gebruik van de mobiele back-end in HTML/JavaScript-Web-Apps.

## <a name="supported-platforms"></a>Ondersteunde platforms
We beperken browserondersteuning naar de huidige en de laatste versie van de belangrijke browsers: Google Chrome, Microsoft Edge, Microsoft Internet Explorer en Mozilla Firefox.  We verwachten dat de SDK aan de functie met een relatief moderne browser.

Het pakket wordt gedistribueerd als een universeel JavaScript-Module, zodat het globals, AMD, ondersteunt en CommonJS indelingen.

## <a name="Setup"></a>Installatie en vereisten
Deze handleiding wordt ervan uitgegaan dat u een back-end hebt gemaakt met een tabel. Deze handleiding wordt ervan uitgegaan dat de tabel hetzelfde schema als de tabellen in deze zelfstudies heeft.

Installatie van de Azure Mobile Apps JavaScript SDK kan worden gedaan de `npm` opdracht:

```
npm install azure-mobile-apps-client --save
```

De bibliotheek kan ook worden gebruikt als een module ES2015 binnen CommonJS omgevingen zoals Browserify en Webpack en als een AMD-bibliotheek.  Bijvoorbeeld:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

U kunt ook een vooraf gemaakte versie van de SDK door het downloaden van rechtstreeks vanuit het CDN:

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Hoe: verificatie van gebruikers
Azure App Service ondersteunt verificatie en autorisatie van app-gebruikers met behulp van verschillende externe id-providers: Facebook, Google, Microsoft-Account en Twitter. U kunt machtigingen instellen voor tabellen toegang voor specifieke bewerkingen alleen geverifieerde gebruikers te beperken. U kunt ook de identiteit van de geverifieerde gebruikers gebruiken voor het implementeren van autorisatieregels in server-scripts. Zie voor meer informatie de [aan de slag met verificatie] zelfstudie.

Twee verificatiestromen worden ondersteund: de stroom van een server en een clientstroom.  De stroom van de server bevat de meest eenvoudige verificatie-ervaring, afhankelijk van de provider de webinterface voor verificatie. De stroom kunt diepere integratie met de apparaat-specifieke mogelijkheden zoals single-sign-on, afhankelijk van provider-specifieke SDK's.

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Hoe: configureren van uw mobiele App-Service voor externe Omleidings-URL's.
Verschillende typen van JavaScript-toepassingen gebruiken een loopback-mogelijkheid voor het afhandelen van OAuth UI stromen.  Deze mogelijkheden omvatten:

* Uw service lokaal worden uitgevoerd
* Met behulp van Live opnieuw laden met de ionische Framework
* Omleiden naar App Service voor verificatie.

Lokaal wordt uitgevoerd kan problemen veroorzaken omdat App Service-verificatie is standaard alleen geconfigureerd voor toegang vanuit uw mobiele back-end. Gebruik de volgende stappen uit om te wijzigen van de App Service-instellingen voor verificatie wanneer de server lokaal wordt uitgevoerd:

1. Meld u aan bij [Azure Portal].
2. Navigeer naar de back-end van uw mobiele App.
3. Selecteer **Resource explorer** in de **ONTWIKKELTOOLS** menu.
4. Klik op **gaat** resource explorer voor de back-end van uw mobiele App in een nieuw tabblad of venster te openen.
5. Vouw de **config** > **authsettings** knooppunt voor uw app.
6. Klik op de **bewerken** knop om in te schakelen de bron te bewerken.
7. Zoek de **allowedExternalRedirectUrls** -element mag niet null zijn. De URL's in een matrix toevoegen:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Vervang de URL's in de matrix door de URL's van uw service, die in dit voorbeeld is `http://localhost:3000` voor de lokale Node.js-voorbeeld-service. U kunt ook `http://localhost:4400` voor de Ripple-service of een andere URL, afhankelijk van hoe uw app is geconfigureerd.
8. Aan de bovenkant van de pagina, klikt u op **lezen/schrijven**, klikt u vervolgens op **plaatsen** om op te slaan van de updates.

Ook moet u dezelfde loopback-URL's toevoegen aan de lijst met toegestane adressen CORS-instellingen:

1. Ga terug naar de [Azure Portal].
2. Navigeer naar de back-end van uw mobiele App.
3. Klik op **CORS** in de **API** menu.
4. Elke URL opgeven in de lege **oorsprongen toegestaan** in het tekstvak.  Een nieuwe tekstvak wordt gemaakt.
5. Klik op **opslaan**

Nadat de back-end is bijgewerkt, kunt u zich aan de nieuwe loopback-URL's in uw app gebruiken.

<!-- URLs. -->
[Azure Mobile Apps snel starten]: app-service-mobile-cordova-get-started.md
[Aan de slag met verificatie]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure Portal]: https://portal.azure.com/
[JavaScript-SDK voor Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/library/azure/jj613353.aspx
