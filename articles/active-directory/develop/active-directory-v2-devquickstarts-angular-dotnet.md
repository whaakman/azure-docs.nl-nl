---
title: "Azure AD v2.0 .NET AngularJS één pagina-app aan de slag | Microsoft Docs"
description: "Het bouwen van een hoek JS één pagina app die gebruikers met beide persoonlijke Microsoft-accounts en werk-of schoolaccounts aanmeldt."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mtillman
editor: 
ms.assetid: 6a341781-278f-461b-92ca-7572a06e6852
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: d2a2108fb2205f646b2ac807922a90939bdee9f6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="add-sign-in-to-an-angularjs-single-page-app---net"></a>Aanmelden toevoegen aan een app met één pagina AngularJS - .NET
In dit artikel gaan we Meld u aan met ingeschakeld Microsoft-accounts toevoegen aan een AngularJS-app met behulp van het Azure Active Directory v2.0-eindpunt.  Het v2.0-eindpunt kunt u voor het uitvoeren van een enkele integratie in uw app en verifiëren van gebruikers met een persoonlijke en zakelijke/school-account.

Dit voorbeeld is een eenvoudige takenlijst één pagina app waarmee taken worden opgeslagen in een back-end REST API, geschreven met .NET 4.5 MVC framework en beveiligd met OAuth bearer-tokens van Azure AD.  De AngularJS-app gebruikt onze JavaScript-verificatiebibliotheek voor open-source [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) te verwerken van het hele proces aanmelden en tokens voor het aanroepen van de REST-API te verkrijgen.  Hetzelfde patroon kan worden toegepast om te verifiëren bij andere REST-API's, zoals de [Microsoft Graph](https://graph.microsoft.com).

> [!NOTE]
> Niet alle Azure Active Directory-scenario's en functies worden ondersteund door het v2.0-eindpunt.  Meer informatie over om te bepalen of moet u het v2.0-eindpunt, [v2.0 beperkingen](active-directory-v2-limitations.md).
> 
> 

## <a name="download"></a>Downloaden
Om te beginnen, moet u downloaden en installeren van Visual Studio.  Vervolgens kunt u een kloon of [downloaden](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) een geraamte app:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

De geraamte app bevat alle standaardcode voor een eenvoudige AngularJS-app, maar alle benodigde onderdelen identiteitsgerelateerde ontbreekt.  Als u niet volgen wilt, kunt u in plaats daarvan klonen of [downloaden](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) voltooide voorbeeld.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a>Een app registreren
Maak eerst een app in de [App-Portal voor Wachtwoordregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of als volgt [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

* Voeg de **Web** platform voor uw app.
* Voer de juiste **omleidings-URI**. De standaardwaarde voor dit voorbeeld is `https://localhost:44326/`.
* Laat de **impliciete stromen toestaan** selectievakje ingeschakeld. 

Noteer de **toepassings-ID** die is toegewezen aan uw app, moet u deze binnenkort. 

## <a name="install-adaljs"></a>Adal.js installeren
Als u wilt starten, navigeert u gedownload projecteren en adal.js installeren.  Als u hebt [bower](http://bower.io/) geïnstalleerd, kunt u alleen deze opdracht uitvoeren.  Voor afhankelijkheid versie verschillen worden ontdekt, kiest u de hogere versie.

```
bower install adal-angular#experimental
```

U kunt ook kunt u handmatig downloaden [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) en [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Beide bestanden toevoegt aan de `app/lib/adal-angular-experimental/dist` map van de `TodoSPA` project.

Nu opent u het project in Visual Studio en adal.js aan het einde van de hoofdpagina hoofdtekst laden:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>Instellen van de REST-API
Terwijl we alles wordt ingesteld, krijgen we de werking van de REST-API-back-end.  Open in de hoofdmap van het project `web.config` en vervang de `audience` waarde.  De REST-API gebruikt deze waarde om tokens die worden ontvangen van de hoek app op AJAX-aanvragen te valideren.

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>

...
```

Dat is de tijd die we willen besteden bespreking van de werking van de REST-API.  Maak rond in de code, maar als u meer informatie wilt over het beveiligen van web-API's in Azure AD, Bekijk gerust [in dit artikel](active-directory-v2-devquickstarts-dotnet-api.md). 

## <a name="sign-users-in"></a>Gebruikers aanmelden
Tijd voor de identiteitscode schrijven.  Hebt misschien u al opgemerkt dat adal.js bevat een AngularJS-provider, waarmee mooi met Hoekvormige routering mechanismen speelt.  Door de adal-module toe te voegen aan de app start:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

U kunt nu initialiseren de `adalProvider` met toepassings-ID:

```js
// app/scripts/app.js

...

adalProvider.init({

        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 

        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',

        // Your application id from the registration portal
        clientId: '<Your-application-id>',

        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',

    }, $httpProvider);
```

Goede, adal.js heeft nu alle informatie die nodig is voor het beveiligen van uw app en meld u aan gebruikers.  Als u wilt afdwingen voor een bepaalde route in de app aanmelden, is een kwestie van één regel code:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Nu wanneer een gebruiker klikt op de `TodoList` koppeling adal.js worden automatisch omgeleid naar Azure AD voor aanmelden indien nodig.  U kunt ook expliciet aanmelden en afmeldingsaanvragen te verzenden door aan te roepen adal.js in uw domeincontrollers verzenden:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {

        // Redirect the user to sign in
        adalService.login();

    };
    $scope.logout = function () {

        // Redirect the user to log out    
        adalService.logOut();

    };
...
```

## <a name="display-user-info"></a>Gebruikersgegevens weergeven
Nu dat de gebruiker is aangemeld, moet u waarschijnlijk voor toegang tot gegevens van de aangemelde gebruiker de verificatie in uw toepassing.  Adal.js beschrijft deze informatie voor u in de `userInfo` object.  Voor toegang tot dit object in een weergave, moet u eerst adal.js toevoegen aan het bereik van de hoofdmap van de bijbehorende controller:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

En u kunt rechtstreeks verband met de `userInfo` object in de weergave: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

U kunt ook de `userInfo` object om te bepalen of aan de gebruiker is aangemeld of niet.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>De REST-API aanroepen
Ten slotte is het tijd om sommige tokens verkrijgen en roept u de REST-API als u wilt maken, lezen, bijwerken en verwijderen van taken.  Ook goed nieuws!  U hoeft te doen *een ding*.  Adal.js zorgt automatisch voor het ophalen, opslaan in cache en vernieuwen van tokens.  Deze ook zorgt voor het koppelen van deze tokens aan uitgaande AJAX-aanvragen die u naar de REST-API verzendt.  

Hoe werkt dit? Het is alle dankzij de magie van [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), waardoor adal.js voor het transformeren van uitgaande en binnenkomende HTTP-berichten.  Bovendien adal.js wordt ervan uitgegaan dat alle aanvragen tot hetzelfde domein verzenden als tokens die zijn bedoeld voor dezelfde toepassings-ID als de AngularJS-app moet worden gebruikt in het venster.  Daarom hebben we gebruikt dezelfde toepassings-ID in de hoek app en in de NodeJS REST-API.  Natuurlijk kunt u dit gedrag negeren en vertel adal.js ophalen van tokens van andere REST-API's zo nodig - maar doet voor dit eenvoudige scenario de standaardwaarden.

Hier volgt een codefragment die laat hoe eenvoudig het is zien voor het verzenden van aanvragen met bearer-tokens van Azure AD:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Gefeliciteerd.  Uw Azure AD-geïntegreerde één pagina app is nu voltooid.  Opwekken op. een boeg.  U kunt verificatie van gebruikers, veilig aanroepen van de back-end REST-API met OpenID Connect en basisinformatie over de gebruiker ophalen.  Buiten het vak biedt ondersteuning voor elke gebruiker met een persoonlijk Microsoft-Account of een werk schoolaccount van Azure AD.  Voer de app en navigeer in een browser naar `https://localhost:44326/`.  Meld u aan met een persoonlijk Microsoft-account of een werk/school-account.  Taken toevoegen aan de takenlijst van de gebruiker en meld u af.  Probeer aanmelden met het andere type account. Als u een Azure AD-tenant maken werk/school gebruikers, moet [Lees hoe u een hier](active-directory-howto-tenant.md) (het is gratis).

Om door te gaan leren over het v2.0-eindpunt head terug naar onze [v2.0 ontwikkelaarshandleiding](active-directory-appmodel-v2-overview.md).  Voor aanvullende bronnen voor uitchecken:

* [Azure-voorbeelden op GitHub >>](https://github.com/Azure-Samples)
* [Azure AD op de Stack-overloop >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* Azure AD-documentatie op [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten downloaden
We raden u aan in te stellen dat u meldingen ontvangt wanneer er beveiligingsincidenten optreden. Ga hiervoor naar [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneer u op Security Advisory Alerts.

