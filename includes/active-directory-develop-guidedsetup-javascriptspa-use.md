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
ms.openlocfilehash: 6f6318f2b4386118d83a72d667e63f88327e240b
ms.sourcegitcommit: abeefca6cd5ca01c3e0b281832212aceff08bf3e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "64993292"
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>Gebruik de Microsoft Authentication Library (MSAL) aan te melden bij de gebruiker

1. Voeg de volgende code aan uw `index.html` binnen het bestand de `<script></script>` tags:

    ```javascript
    var msalConfig = {
        auth: {
            clientId: "Enter_the_Application_Id_here"
            authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    
    var graphConfig = {
        graphMeEndpoint: "https://graph.microsoft.com/v1.0/me"
    };

    // this can be used for login or token request, however in more complex situations
    // this can have diverging options
    var request = {
        scopes: ["user.read"]
    };

    var myMSALObj = new Msal.UserAgentApplication(msalConfig);
    // Register Callbacks for redirect flow
    myMSALObj.handleRedirectCallbacks(acquireTokenRedirectCallBack, acquireTokenErrorRedirectCallBack);

    function signIn() {

        myMSALObj.loginPopup(request).then(function (loginResponse) {
            //Login Success
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }).catch(function (error) {
            console.log(error);
        });
    }


    function acquireTokenPopupAndCallMSGraph() {
        //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
        myMSALObj.acquireTokenSilent(request).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
        }).catch(function (error) {
            console.log(error.errorCode);
            // Call acquireTokenPopup (popup window) in case of acquireTokenSilent failure due to consent or interaction required ONLY
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenPopup(request).then(function (tokenResponse) {
                    callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
                }).catch(function (error) {
                    console.log(error);
                });
            }
        });
    }


    function graphAPICallback(data) {
        document.getElementById("json").innerHTML = JSON.stringify(data, null, 2);
    }


    function showWelcomeMessage() {
        var divWelcome = document.getElementById('WelcomeMessage');
        divWelcome.innerHTML = 'Welcome ' + myMSALObj.getAccount().userName + "to Microsoft Graph API";
        var loginbutton = document.getElementById('SignIn');
        loginbutton.innerHTML = 'Sign Out';
        loginbutton.setAttribute('onclick', 'signOut();');
    }


   function acquireTokenRedirectAndCallMSGraph() {
        //Call acquireTokenSilent (iframe) to obtain a token for Microsoft Graph
        myMSALObj.acquireTokenSilent(request).then(function (tokenResponse) {
            callMSGraph(graphConfig.graphMeEndpoint, tokenResponse.accessToken, graphAPICallback);
        }).catch(function (error) {
            console.log(error);
            //Call acquireTokenRedirect in case of acquireToken Failure
            if (requiresInteraction(error.errorCode)) {
                myMSALObj.acquireTokenRedirect(request);
            }
        });
    }


    function acquireTokenRedirectCallBack(response) {
        if (response.tokenType === "access_token") {
            callMSGraph(graphConfig.graphEndpoint, response.accessToken, graphAPICallback);
        } else {
            console.log("token type is:" + response.tokenType);
        }
    }


    function  acquireTokenErrorRedirectCallBack(error) {
        console.log(error);
    }

    function requiresInteraction(errorCode) {
        if (!errorCode || !errorCode.length) {
            return false;
        }
        return errorCode === "consent_required" ||
            errorCode === "interaction_required" ||
            errorCode === "login_required";
    }


    if (loginType === 'POPUP') {
        if (myMSALObj.getAccount()) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenPopupAndCallMSGraph();
        }
    }
    else if (loginType === 'REDIRECT') {
        document.getElementById("SignIn").onclick = function () {
            myMSALObj.loginRedirect(request);
        };
        if (myMSALObj.getAccount() && !myMSALObj.isCallback(window.location.hash)) {// avoid duplicate code execution on page load in case of iframe and popup window.
            showWelcomeMessage();
            acquireTokenRedirectAndCallMSGraph();
        }
    } else {
        console.error('Please set a valid login type');
    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Meer informatie

Nadat een gebruiker op de **aanmelden** knop voor het eerst de `signIn` methodeaanroepen `loginPopup` aan te melden bij de gebruiker. Resultaat van deze methode is bij het openen van een pop-upvenster met de *Microsoft identity platform endpoint* te vragen en referenties van de gebruiker te valideren. Als gevolg van een geslaagde aanmelding wordt de gebruiker wordt omgeleid naar de oorspronkelijke *index.html* pagina en een token wordt ontvangen, verwerkt door `msal.js` en de informatie in het token in de cache is opgeslagen. Dit token wordt ook wel de *ID-token* en bevat algemene informatie over de gebruiker, zoals de naam van de gebruiker weergegeven. Als u alle gegevens die door dit token voor andere doeleinden gebruiken wilt, moet u om ervoor te zorgen dat dit token wordt gevalideerd door uw back-endserver om te waarborgen dat het token is uitgegeven voor een geldige gebruiker voor uw toepassing.

De beveiligd-WACHTWOORDVERIFICATIE die worden gegenereerd door deze handleiding aanroepen `acquireTokenSilent` en/of `acquireTokenPopup` aan te schaffen een *toegangstoken* gebruikt om te vragen van de Microsoft Graph-API voor profielgegevens van de gebruiker. Als u een voorbeeld waarin het ID-token valideert, kijk dan eens [dit](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "active-directory-javascript-singlepageapp-dotnet-webapi-v2 voorbeeld GitHub") voorbeeldtoepassing in GitHub – het voorbeeld maakt gebruik van een ASP .NET-web-API voor validatie van tokens.

#### <a name="getting-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

Na de eerste aanmelding wordt u niet wilt gebruikers vragen om te verifiëren telkens wanneer ze nodig hebben om aan te vragen van een token te krijgen tot een resource – dus *acquireTokenSilent* de meeste gevallen moet worden gebruikt voor het verkrijgen van tokens. Er zijn echter situaties die u wilt afdwingen dat gebruikers om te communiceren met Microsoft identity platform endpoint – enkele voorbeelden zijn:

- Gebruikers moeten mogelijk hun referenties opnieuw opgeven omdat het wachtwoord is verlopen
- Uw toepassing vraagt toegang tot een resource waarvoor de gebruiker toestemming moet geven
- Wanneer verificatie in twee stappen is vereist

Aanroepen van de *acquireTokenPopup* resulteert in een pop-upvenster (of *acquireTokenRedirect* leidt gebruikers omleiden naar het eindpunt van de Microsoft identity-platform) waar gebruikers nodig hebben om te communiceren door bevestiging van hun referenties, de toestemming verlenen aan de vereiste resource of voltooien van de verificatie met twee factoren.

#### <a name="getting-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

Met de methode `acquireTokenSilent` wordt het verkrijgen en vernieuwen van tokens verwerkt zonder tussenkomst van de gebruiker. Na `loginPopup` (of `loginRedirect`) wordt uitgevoerd voor de eerste keer `acquireTokenSilent` is de methode die vaak worden gebruikt om te verkrijgen van tokens gebruikt voor toegang tot beveiligde resources voor volgende aanroepen - aanroepen aan te vragen of vernieuwen van tokens op de achtergrond worden gemaakt.
`acquireTokenSilent` mislukken in sommige gevallen, bijvoorbeeld van de gebruiker het wachtwoord is verlopen. Uw toepassing kan verwerken deze uitzondering op twee manieren:

1. Aanroepen van `acquireTokenPopup` onmiddellijk, wat ertoe leidt dat u wordt gevraagd de gebruiker zich aanmeldt. Dit patroon wordt vaak gebruikt in online toepassingen waarbij er geen niet-geverifieerde inhoud in de toepassing beschikbaar voor de gebruiker. Dit patroon maakt gebruik van de steekproef die worden gegenereerd door deze Begeleide installatie.

2. Toepassingen kunnen ook een visuele indicatie maken voor de gebruiker dat een interactief aanmelden is vereist, zodat de gebruiker kan het juiste moment aan te melden bij selecteren, of de toepassing opnieuw kunt `acquireTokenSilent` op een later tijdstip. Dit wordt meestal gebruikt wanneer de gebruiker andere functionaliteit van de toepassing gebruiken kunt zonder onderbroken: er bijvoorbeeld niet-geverifieerde inhoud beschikbaar in de toepassing is. In dit geval kan de gebruiker beslissen wanneer ze willen Meld u aan voor toegang tot de beveiligde bron of om de verouderde gegevens te vernieuwen.

> [!NOTE]
> Deze snelstartgids maakt gebruik van de `loginRedirect` en `acquireTokenRedirect` methoden als de browser die wordt gebruikt Internet Explorer vanwege is een [bekende probleem](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) met betrekking tot de verwerking van pop-upvensters door Internet Explorer-browser.
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
