---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 0f4f890afd8c698bec8dbd76e8bb8dca710f4d6c
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203305"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Gebruik de Microsoft Authentication Library (MSAL) aan te melden bij de gebruiker

1. Voeg de volgende code aan uw `index.html` binnen het bestand de `<script></script>` tags:

```javascript
//Pass null for default authority (https://login.microsoftonline.com/common)
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, null, acquireTokenRedirectCallBack,
    {storeAuthStateInCookie: true, cacheLocation: "localStorage"});

function signIn() {
    myMSALObj.loginPopup(applicationConfig.graphScopes).then(function (idToken) {
        //Login Success
        showWelcomeMessage();
        acquireTokenPopupAndCallMSGraph();
    }, function (error) {
        console.log(error);
    });
}

function acquireTokenPopupAndCallMSGraph() {
    //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
    myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
        callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
    }, function (error) {
        console.log(error);
        // Call acquireTokenPopup (popup window) in case of acquireTokenSilent failure due to consent or interaction required ONLY
        if (error.indexOf("consent_required") !== -1 || error.indexOf("interaction_required") !== -1 || error.indexOf("login_required") !== -1) {
            myMSALObj.acquireTokenPopup(applicationConfig.graphScopes).then(function (accessToken) {
                callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
            }, function (error) {
                console.log(error);
            });
        }
    });
}

function graphAPICallback(data) {
    //Display user data on DOM
    var divWelcome = document.getElementById('WelcomeMessage');
    divWelcome.innerHTML += " to Microsoft Graph API!!";
    document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
}

function showWelcomeMessage() {
    var divWelcome = document.getElementById('WelcomeMessage');
    divWelcome.innerHTML += 'Welcome ' + myMSALObj.getUser().name;
    var loginbutton = document.getElementById('SignIn');
    loginbutton.innerHTML = 'Sign Out';
    loginbutton.setAttribute('onclick', 'signOut();');
}

// This function can be removed if you do not need to support IE
function acquireTokenRedirectAndCallMSGraph() {
    //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
    myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
      callMSGraph(applicationConfig.graphEndpoint, accessToken, graphAPICallback);
    }, function (error) {
        console.log(error);
        //Call acquireTokenRedirect in case of acquireToken Failure
        if (error.indexOf("consent_required") !== -1 || error.indexOf("interaction_required") !== -1 || error.indexOf("login_required") !== -1) {
            myMSALObj.acquireTokenRedirect(applicationConfig.graphScopes);
        }
    });
}

function acquireTokenRedirectCallBack(errorDesc, token, error, tokenType) {
    if (tokenType === "access_token") {
        callMSGraph(applicationConfig.graphEndpoint, token, graphAPICallback);
    } else {
        console.log("token type is:"+tokenType);
    }
}


// Browser check variables
var ua = window.navigator.userAgent;
var msie = ua.indexOf('MSIE ');
var msie11 = ua.indexOf('Trident/');
var msedge = ua.indexOf('Edge/');
var isIE = msie > 0 || msie11 > 0;
var isEdge = msedge > 0;

//If you support IE, our recommendation is that you sign-in using Redirect APIs
//If you as a developer are testing using Microsoft Edge InPrivate mode, please add "isEdge" to the if check
if (!isIE) {
    if (myMSALObj.getUser()) {// avoid duplicate code execution on page load in case of iframe and popup window.
        showWelcomeMessage();
        acquireTokenPopupAndCallMSGraph();
    }
} else {
    document.getElementById("SignIn").onclick = function () {
        myMSALObj.loginRedirect(applicationConfig.graphScopes);
    };
    if (myMSALObj.getUser() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
        showWelcomeMessage();
        acquireTokenRedirectAndCallMSGraph();
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie

Nadat een gebruiker op de **aanmelden** knop voor het eerst de `signIn` methodeaanroepen `loginPopup` aan te melden bij de gebruiker. Resultaat van deze methode is bij het openen van een pop-upvenster met de *Microsoft Azure Active Directory v2.0-eindpunt* te vragen en referenties van de gebruiker te valideren. Als gevolg van een geslaagde aanmelding wordt de gebruiker wordt omgeleid naar de oorspronkelijke *index.html* pagina en een token wordt ontvangen, verwerkt door `msal.js` en de informatie in het token in de cache is opgeslagen. Dit token wordt ook wel de *ID-token* en bevat algemene informatie over de gebruiker, zoals de naam van de gebruiker weergegeven. Als u alle gegevens die door dit token voor andere doeleinden gebruiken wilt, moet u om ervoor te zorgen dat dit token wordt gevalideerd door uw back-endserver om te waarborgen dat het token is uitgegeven voor een geldige gebruiker voor uw toepassing.

De beveiligd-WACHTWOORDVERIFICATIE die worden gegenereerd door deze handleiding aanroepen `acquireTokenSilent` en/of `acquireTokenPopup` aan te schaffen een *toegangstoken* gebruikt om te vragen van de Microsoft Graph-API voor profielgegevens van de gebruiker. Als u een voorbeeld waarin het ID-token valideert, kijk dan eens [dit](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "active-directory-javascript-singlepageapp-dotnet-webapi-v2 voorbeeld GitHub") voorbeeldtoepassing in GitHub – het voorbeeld maakt gebruik van een ASP .NET-web-API voor validatie van tokens.

#### <a name="getting-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

Na de eerste aanmelding wordt u niet wilt gebruikers vragen om te verifiëren telkens wanneer ze nodig hebben om aan te vragen van een token te krijgen tot een resource – dus *acquireTokenSilent* de meeste gevallen moet worden gebruikt voor het verkrijgen van tokens. Er zijn echter situaties die u wilt afdwingen dat gebruikers om te communiceren met Azure Active Directory v2.0-eindpunt: enkele voorbeelden zijn:

- Gebruikers moeten mogelijk hun referenties opnieuw opgeven omdat het wachtwoord is verlopen
- Uw toepassing vraagt toegang tot een resource waarvoor de gebruiker toestemming moet geven
- Wanneer verificatie in twee stappen is vereist

Aanroepen van de *acquireTokenPopup(scope)* resulteert in een pop-upvenster (of *acquireTokenRedirect(scope)* leidt gebruikers omleiden naar de Azure Active Directory v2.0-eindpunt) waar gebruikers moeten communiceren door hun referenties bevestigen, de toestemming verlenen aan de vereiste resource of voltooien van de verificatie met twee factoren.

#### <a name="getting-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

Met de methode ` acquireTokenSilent` wordt het verkrijgen en vernieuwen van tokens verwerkt zonder tussenkomst van de gebruiker. Na `loginPopup` (of `loginRedirect`) wordt uitgevoerd voor de eerste keer `acquireTokenSilent` is de methode die vaak worden gebruikt om te verkrijgen van tokens gebruikt voor toegang tot beveiligde resources voor volgende aanroepen - aanroepen aan te vragen of vernieuwen van tokens op de achtergrond worden gemaakt.
`acquireTokenSilent` mislukken in sommige gevallen, bijvoorbeeld van de gebruiker het wachtwoord is verlopen. Uw toepassing kan verwerken deze uitzondering op twee manieren:

1. Aanroepen van `acquireTokenPopup` onmiddellijk, wat ertoe leidt dat u wordt gevraagd de gebruiker zich aanmeldt. Dit patroon wordt vaak gebruikt in online toepassingen waarbij er geen niet-geverifieerde inhoud in de toepassing beschikbaar voor de gebruiker. Dit patroon maakt gebruik van de steekproef die worden gegenereerd door deze Begeleide installatie.

2. Toepassingen kunnen ook een visuele indicatie maken voor de gebruiker dat een interactief aanmelden is vereist, zodat de gebruiker kan het juiste moment aan te melden bij selecteren, of de toepassing opnieuw kunt `acquireTokenSilent` op een later tijdstip. Dit wordt meestal gebruikt wanneer de gebruiker andere functionaliteit van de toepassing gebruiken kunt zonder onderbroken: er bijvoorbeeld niet-geverifieerde inhoud beschikbaar in de toepassing is. In dit geval kan de gebruiker beslissen wanneer ze willen Meld u aan voor toegang tot de beveiligde bron of om de verouderde gegevens te vernieuwen.

> [!NOTE]
> De bovenstaande code wordt gebruikgemaakt van de `loginRedirect` en `acquireTokenRedirect` methoden als de browser die wordt gebruikt Internet Explorer vanwege is een [bekende probleem](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser) met betrekking tot de verwerking van pop-upvensters door Internet Explorer-browser.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Aanroepen van de Microsoft Graph-API met behulp van het token dat u zojuist hebt verkregen.

Voeg de volgende code aan uw `index.html` binnen het bestand de `<script></script>` tags:

```javascript
function callMSGraph(theUrl, accessToken, callback) {
    var xmlHttp = new XMLHttpRequest();
    xmlHttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200)
            callback(JSON.parse(this.responseText));
    }
    xmlHttp.open("GET", theUrl, true); // true for asynchronous
    xmlHttp.setRequestHeader('Authorization', 'Bearer ' + accessToken);
    xmlHttp.send();
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Meer informatie over het maken van een REST-aanroep op basis van een beveiligde API

In de voorbeeldtoepassing die is gemaakt door deze handleiding worden de `callMSGraph()` methode wordt gebruikt om een HTTP `GET` aanvragen op basis van een beveiligde resource waarvoor een token en vervolgens de inhoud wordt geretourneerd voor de oproepende functie. Met deze methode wordt het verkregen token in de *HTTP-autorisatie-header*. Voor de voorbeeldtoepassing in deze handleiding is gemaakt, de bron is de Microsoft Graph API *me* eindpunt – Hiermee worden de profielgegevens van de gebruiker.

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Toevoegen van een methode voor de gebruiker afmelden

Voeg de volgende code aan uw `index.html` binnen het bestand de `<script></script>` tags:

```javascript
/**
 * Sign out the user
 */
 function signOut() {
     myMSALObj.logout();
 }
```
