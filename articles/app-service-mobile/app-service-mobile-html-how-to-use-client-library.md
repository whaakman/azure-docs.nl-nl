---
title: Het gebruik van de JavaScript-SDK voor Azure Mobile Apps
description: Hoe gebruik v voor Azure Mobile Apps
services: app-service\mobile
documentationcenter: javascript
author: conceptdev
manager: crdun
editor: 
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 9494e40a27895def450de856bbf806573daedfdb
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Het gebruik van de JavaScript-clientbibliotheek voor Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Deze handleiding leert u veelvoorkomende scenario's met behulp van de meest recente uitvoeren [JavaScript SDK voor Azure Mobile Apps]. Als u niet bekend met Azure Mobile Apps bent, eerst voltooien [Azure Mobile Apps Quick Start] voor het maken van een back-end en een tabel maken. In deze handleiding richten we op het gebruik van de mobiele back-end in de HTML/JavaScript-webtoepassingen.

## <a name="supported-platforms"></a>Ondersteunde platforms
We beperken browserondersteuning van de huidige en de laatste versie van de belangrijke browsers: Google Chrome, Microsoft Edge Microsoft Internet Explorer en Mozilla Firefox.  We verwachten dat de SDK met de functie met relatief moderne browsers.

Het pakket wordt gedistribueerd als een universeel JavaScript-Module, zodat deze globale variabelen, AMD, ondersteunt en CommonJS indelingen.

## <a name="Setup"></a>Het installatieprogramma en vereisten
Deze handleiding wordt ervan uitgegaan dat u een back-end hebt gemaakt met een tabel. Deze handleiding wordt ervan uitgegaan dat de tabel heeft van hetzelfde schema als de tabellen in deze zelfstudies.

Installeren van de Azure Mobile Apps JavaScript SDK kan worden uitgevoerd via de `npm` opdracht:

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

U kunt ook een vooraf samengestelde versie van de SDK door rechtstreeks uit onze CDN te downloaden:

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Procedure: verificatie van gebruikers
Azure App Service biedt ondersteuning voor verificatie en autorisatie van app-gebruikers met behulp van verschillende externe id-providers: Facebook, Google, Microsoft-Account en Twitter. U kunt machtigingen instellen voor tabellen toegang voor specifieke bewerkingen voor alleen geverifieerde gebruikers te beperken. U kunt ook de identiteit van de geverifieerde gebruikers gebruiken voor het implementeren van autorisatieregels in server-scripts. Zie voor meer informatie de [aan de slag met verificatie] zelfstudie.

Twee verificatie stromen worden ondersteund: de stroom van een server en een client-stroom.  De stroom van de server biedt de eenvoudigste verificatie-ervaring, is afhankelijk van de webinterface voor verificatie van de provider. De stroom kunt u betere integratie met apparaatspecifieke mogelijkheden, zoals single-sign-on zoals is afhankelijk van de provider-specifieke SDK's.

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Procedure: het configureren van uw mobiele App Service voor externe Omleidings-URL's.
Verschillende soorten toepassingen van JavaScript gebruiken een loopback-functie voor het OAuth UI stromen.  Deze mogelijkheden zijn:

* Uw service lokaal uitgevoerd
* Met behulp van Live opnieuw laden met ionische Framework
* Omleiden naar App Service voor verificatie.

Die lokaal wordt uitgevoerd kan problemen veroorzaken omdat App Service-verificatie is standaard alleen geconfigureerd voor toegang vanuit uw back-end voor de mobiele App. Gebruik de volgende stappen uit om te wijzigen van instellingen van de App Service-verificatie inschakelen wanneer de server lokaal wordt uitgevoerd:

1. Meld u aan bij [Azure-portal].
2. Navigeer naar uw mobiele App back-end.
3. Selecteer **resourceverkenner** in de **ONTWIKKELINGSPROGRAMMA's** menu.
4. Klik op **gaat** resource explorer voor back-end van uw mobiele App in een nieuw tabblad of het venster te openen.
5. Vouw de **config** > **authsettings** knooppunt voor uw app.
6. Klik op de **bewerken** knop bewerken van de resource inschakelen.
7. Zoek de **allowedExternalRedirectUrls** -element moet null zijn. De URL's in een matrix toevoegen:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Vervang de URL's in de matrix door de URL's van uw service, die in dit voorbeeld is `http://localhost:3000` voor de lokale service van de Node.js-voorbeeld. U kunt ook `http://localhost:4400` voor de service Rimpel of een andere URL, afhankelijk van hoe uw app is geconfigureerd.
8. Klik boven aan de pagina op **lezen/schrijven**, klikt u vervolgens op **plaatsen** om op te slaan van de updates.

U moet ook de dezelfde loopback-URL's toevoegen aan de CORS-instellingen voor goedgekeurde IP-adressen:

1. Ga terug naar de [Azure-portal].
2. Navigeer naar uw mobiele App back-end.
3. Klik op **CORS** in de **API** menu.
4. Elke URL opgeven in de lege **toegestane oorsprongen** in het tekstvak.  Een nieuw tekstvak wordt gemaakt.
5. Klik op **opslaan**

Na de back-end-updates, kunt u zich de loopback-URL's in uw app gebruiken.

<!-- URLs. -->
[Azure Mobile Apps Quick Start]: app-service-mobile-cordova-get-started.md
[aan de slag met verificatie]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure-portal]: https://portal.azure.com/
[JavaScript SDK voor Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
