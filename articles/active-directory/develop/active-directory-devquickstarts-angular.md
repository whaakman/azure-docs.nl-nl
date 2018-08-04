---
title: Azure AD-AngularJS aan de slag | Microsoft Docs
description: Over het bouwen van een AngularJS single-page toepassing die kan worden geïntegreerd met Azure AD voor aanmelding bij en Azure AD-beveiligde API's aanroept met behulp van OAuth.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 0c7f6a0e447e3b48cdd1df684dc105ece1e98f66
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502046"
---
# <a name="azure-ad-angularjs-getting-started"></a>Azure AD-AngularJS aan de slag

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory (Azure AD) maakt het simpel waarmee u kunt toevoegen, aanmelding, afmelding en veilig OAuth-API-naar uw apps met één pagina aanroepen. Hiermee kunt uw apps verificatie van gebruikers met hun Windows Server Active Directory-accounts en een web-API waarmee u Azure AD beveiligen kunt, zoals de Office 365 API's of de API van Azure gebruiken.

Azure AD biedt voor JavaScript-toepassingen die worden uitgevoerd in een browser, de Active Directory Authentication Library (ADAL) of adal.js. Het enige doel van adal.js is gemakkelijker voor uw app om toegangstokens ophalen. Als u wilt laten zien hoe eenvoudig het is, bouwen hier we een AngularJS To Do List-toepassing die:

* De gebruiker zich aanmeldt bij de app met behulp van Azure AD als id-provider.

* Sommige geeft informatie weer over de gebruiker.
* Veilig van de app te doen lijst API aanroept met behulp van Azure AD-bearer-tokens.
* De gebruiker buiten de app ondertekent.

Voor het bouwen van de volledige, werkende toepassing, moet u naar:

1. Uw app registreren bij Azure AD.
2. ADAL installeren en configureren van de app met één pagina.
3. Gebruik ADAL voor beveiligde pagina's in de app met één pagina.

Aan de slag [het app-basisproject downloaden](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) of [downloaden van het voltooide voorbeeld](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip). U moet ook een Azure AD-tenant in die u kunt gebruikers maken en registreren van een toepassing. Als u nog een tenant hebt [informatie over het verkrijgen van een](quickstart-create-new-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>Stap 1: De toepassing DirectorySearcher registreren
Als u wilt inschakelen voor de app om te verifiëren van gebruikers en tokens verkrijgen, moet u eerst deze te registreren in uw Azure AD-tenant:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als u bent aangemeld bij meerdere mappen, moet u mogelijk om te controleren of dat u de juiste map bekijkt. Om dit te doen, op de bovenste balk, klikt u op uw account. Onder de **Directory** kiest u de Azure AD-tenant waar u uw toepassing registreren.
3. Klik op **alle services** in het linkerdeelvenster en selecteer vervolgens **Azure Active Directory**.
4. Klik op **App-registraties**, en selecteer vervolgens **toevoegen**.
5. Volg de aanwijzingen en maak een nieuwe webtoepassing en/of web-API:
  * **Naam** beschrijving van uw toepassing op gebruikers.
  * **Aanmeldings-URL** is de locatie waarop Azure AD tokens wordt geretourneerd. De standaardlocatie voor dit voorbeeld is `https://localhost:44326/`.
6. Nadat u de registratie hebt voltooid, wijst Azure AD een unieke toepassings-ID toe aan uw app. U moet deze waarde in de volgende secties, dus kopieer deze op het toepassingstabblad.
7. De impliciete flow OAuth Adal.js gebruikt om te communiceren met Azure AD. U moet de impliciete stroom inschakelen voor uw toepassing:
  1. Klik op de toepassing en selecteer **Manifest** om het manifest inline-editor te openen.
  2. Zoek de `oauth2AllowImplicitFlow` eigenschap. Stel de waarde op `true`.
  3. Klik op **opslaan** om op te slaan van het manifest.
8. Machtigingen verlenen binnen uw tenant voor uw toepassing. Ga naar **instellingen** > **vereiste machtigingen**, en klik op de **machtigingen verlenen** knop op de bovenste balk. Klik op **Ja** om te bevestigen.

## <a name="step-2-install-adal-and-configure-the-single-page-app"></a>Stap 2: Installeer ADAL en configureren van de app met één pagina
Nu dat u een toepassing in Azure AD hebt, kunt u adal.js installeert en uw identiteit gerelateerde code te schrijven.

### <a name="configure-the-javascript-client"></a>De JavaScript-client configureren
Beginnen met het adal.js toevoegen aan het project TodoSPA met behulp van de Package Manager-Console:
  1. Download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) en toe te voegen aan de `App/Scripts/` projectmap.
  2. Download [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) en toe te voegen aan de `App/Scripts/` projectmap.
  3. Laden van elk script voor het einde van de `</body>` in `index.html`:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-the-back-end-server"></a>De back-end-server configureren
Voor van de app met één pagina back-end te doen lijst API tokens van de browser accepteert, moet de back-end configuratie-informatie over de app-registratie. Open in het project TodoSPA `web.config`. Vervang de waarden van de elementen in de `<appSettings>` gedeelte in overeenstemming met de waarden die u in Azure portal gebruikt. Uw code zal naar deze waarden verwijzen wanneer deze gebruikmaakt van ADAL.
  * `ida:Tenant` is het domein van uw Azure AD-tenant bijvoorbeeld contoso.onmicrosoft.com.
  * `ida:Audience` is de client-ID van uw toepassing die u hebt gekopieerd uit de portal.

## <a name="step-3-use-adal-to-help-secure-pages-in-the-single-page-app"></a>Stap 3: Gebruik ADAL voor de beveiligde pagina's in de app met één pagina
Adal.js integreert met AngularJS route en HTTP-providers, zodat u beveiligde afzonderlijke weergaven in uw app met één pagina kunt.

1. In `App/Scripts/app.js`, brengen in de module adal.js:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. Initialiseren `adalProvider` met behulp van de configuratiewaarden van de registratie van uw toepassing, ook in `App/Scripts/app.js`:

    ```js
    adalProvider.init(
      {
          instance: 'https://login.microsoftonline.com/',
          tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
          clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
          extraQueryParameter: 'nux=1',
          //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
      },
      $httpProvider
    );
    ```
3. Beveiligen van de `TodoList` weergeven in de app met behulp van slechts één regel code: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Overzicht
U hebt nu een beveiligde app met één pagina die u kunt gebruikers aanmelden en verzenden van beveiligde met bearer-token aanvragen naar de back-end-API. Wanneer een gebruiker klikt op de **TodoList** koppeling adal.js worden automatisch omgeleid naar Azure AD voor aanmelding bij indien nodig. Adal.js wordt bovendien automatisch een toegangstoken koppelen aan een Ajax-aanvragen die worden verzonden naar de back-end van de app. 

De voorgaande stappen zijn de bare minimale die nodig is voor het bouwen van een app met één pagina met behulp van adal.js. Maar een aantal andere functies zijn nuttig in de app met één pagina:

* Voor het uitgeven van expliciet aanmelden en afmeldingsaanvragen te verzenden, kunt u functies in uw domeincontrollers die adal.js aanroepen. In `App/Scripts/homeCtrl.js`:

    ```js
    ...
    $scope.login = function () {
        adalService.login();
    };
    $scope.logout = function () {
        adalService.logOut();
    };
    ...
    ```
* Mogelijk wilt u de gebruikersgegevens die aanwezig zijn in de gebruikersinterface van de app. De ADAL-service al is toegevoegd aan de `userDataCtrl` -controller, zodat u toegang krijgt tot de `userInfo` object in de bijbehorende weergave `App/Views/UserData.html`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Er zijn veel scenario's waarin u weten wilt als de gebruiker is aangemeld of niet. U kunt ook de `userInfo` object informatie te verzamelen. Bijvoorbeeld in `index.html`, ziet u ofwel de **aanmelding** of **Afmelden** knop op basis van de status van verificatie:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

Uw Azure AD-integratie-app met één pagina kan gebruikers verifiëren, veilig aanroepen van de back-end met behulp van OAuth 2.0 en elementaire informatie over de gebruiker. Als u niet hebt gedaan, is nu de tijd voor het vullen van uw tenant waarbij sommige gebruikers. Voer uw takenlijst-app met één pagina uit en meld u aan met een van deze gebruikers. Taken toevoegen aan de takenlijst van de gebruiker, meldt u zich af en meld u opnieuw aan.

Adal.js kunt eenvoudig algemene identiteitsfuncties opnemen in uw toepassing. Dit zorgt dat al het werk dirty voor u: cachebeheer, ondersteuning voor OAuth protocol, dat de gebruiker een UI voor aanmelden, vervallen tokens en meer te vernieuwen.

Voor een verwijzing naar het voltooide voorbeeld (zonder uw configuratiewaarden) is beschikbaar in [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Volgende stappen
U kunt nu verder met aanvullende scenario's. U wilt proberen: [een CORS-web-API aanroepen vanuit een app met één pagina](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
