---
title: Azure AD v2 JavaScript-quickstart | Microsoft Docs
description: Informatie over hoe een API waarvoor toegangstokens door Azure Active Directory v2.0-eindpunt kunnen aanroepen in JavaScript-toepassingen
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 69c77896f894201d1419aaef33470a02ac45ff91
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986285"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Snelstartgids: Meld u aan gebruikers en een toegangstoken van een JavaScript-toepassing te verkrijgen

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

In deze snelstartgids leert u hoe u een codevoorbeeld van die u laat hoe een JavaScript-toepassing met één pagina (SPA zien) kunt aanmelden met persoonlijke accounts, werk en schoolaccounts, een toegangstoken voor het aanroepen van de Microsoft Graph API of een web-API.

![Hoe de voorbeeld-app werkt die is gegenereerd door deze snelstart](media/quickstart-v2-javascript/javascriptspa-intro.png)

> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Uw toepassing registreren en de app uit de snelstart downloaden
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
>
> 1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) om een toepassing te registreren.
> 1. Als uw account u toegang tot meer dan één tenant hebt, selecteert u uw account in de rechterbovenhoek hoek en instellen van uw portal-sessie op de gewenste Azure AD-tenant.
> 1. Selecteer in het navigatiedeelvenster links het **Azure Active Directory** service en selecteer vervolgens **App-registraties (Preview) > nieuwe registratie**.
> 1. Wanneer de **registreren van een toepassing** pagina wordt weergegeven, voer een naam in voor uw toepassing.
> 1. Onder **ondersteund accounttypen**, selecteer **Accounts in een organisatie-map en de persoonlijke Microsoft-accounts**.
> 1. Selecteer de **Web** platform onder de **omleidings-URI** sectie en stel de waarde voor `http://localhost:30662/`.
> 1. Wanneer u klaar bent, selecteert u **registreren**.  Op de app **overzicht** pagina, noteer de **(client) toepassings-ID** waarde.
> 1. Deze snelstartgids moet de [impliciet verlenen stroom](v2-oauth2-implicit-grant-flow.md) zijn ingeschakeld. Selecteer in het navigatiedeelvenster links van de geregistreerde toepassing, **verificatie**.
> 1. In **geavanceerde instellingen**onder **impliciete**, schakel beide **ID-tokens** en **toegangstokens** selectievakjes. ID-tokens en toegangstokens zijn vereist, omdat deze app moet het aanmelden van gebruikers en aanroepen van een API.
> 1. Selecteer **Opslaan**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Stap 1: Uw toepassing in Azure portal configureren
> Voor het codevoorbeeld voor deze Quick Start om te werken, moet u Voeg een omleidings-URI als `http://localhost:30662/` en in te schakelen **impliciete**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Deze wijzigingen aanbrengt voor mij]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-javascript/green-check.png) uw toepassing is geconfigureerd met deze kenmerken.

#### <a name="step-2-download-the-project"></a>Stap 2: Het project downloaden

U kunt een van deze opties die geschikt is voor uw ontwikkelomgeving.
* [Download de projectbestanden core - voor een webserver, zoals Node.js](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)
* [Het Visual Studio-project downloaden](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)

Pak het zip-bestand naar een lokale map, bijvoorbeeld **C:\Azure-Samples**.

#### <a name="step-3-configure-your-javascript-app"></a>Stap 3: Uw JavaScript-app configureren

> [!div renderon="docs"]
> Bewerken `index.html` en vervang `Enter_the_Application_Id_here` onder `applicationConfig` met toepassings-ID van de app die u zojuist hebt geregistreerd.

> [!div class="sxs-lookup" renderon="portal"]
> Bewerken `index.html` en vervang `applicationConfig` met:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
> [!NOTE]
>Als u [Node.js](https://nodejs.org/en/download/), wordt de *server.js* bestand is geconfigureerd voor de server te luisteren op poort 30662 starten.
> Als u [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), het codevoorbeeld *.csproj* bestand is geconfigureerd voor de server te luisteren op poort 30662 starten.
>

#### <a name="step-4-run-the-project"></a>Stap 4: Voer het project

* Als u Node.js:

    1. Voer de volgende opdracht uit de directory van het project om de server te starten:

        ```batch
        npm install
        node server.js
        ```

    1. Open een webbrowser en navigeer naar `http://localhost:30662/`.
    1. Klik op **aanmelden** knop aanmelden starten en vervolgens Microsoft Graph API aanroepen.


* Als u Visual Studio, zorg ervoor dat u selecteert de project-oplossing en druk vervolgens op **F5** om uit te voeren van uw project.

## <a name="more-information"></a>Meer informatie

### <a name="msaljs"></a>*msal.js*

MSAL is de library gebruikt voor aanmelding bij gebruikers en tokens gebruikt voor toegang tot een API die wordt beveiligd door Microsoft Azure Active Directory (Azure AD)-aanvraag. Van de snelstartgids *index.html* bevat een verwijzing naar de bibliotheek:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.min.js"></script>
```

U kunt ook hebt u een knooppunt is geïnstalleerd, kunt u downloaden via npm:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL initialiseren

De snelstartcode laat ook zien hoe initialiseren van de bibliotheek:

```javascript
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, null, acquireTokenRedirectCallBack, {storeAuthStateInCookie: true, cacheLocation: "localStorage"});
```

> |Waar  |  |
> |---------|---------|
> |`ClientId`     |Toepassings-id van de toepassing die is geregistreerd in Azure Portal|
> |`authority`    |URL van de instantie is. Doorgeven van *null* wordt de standaard-instantie ingesteld op `https://login.microsoftonline.com/common`. Als uw app is één tenant (doelitems accounts in slechts één map), instelt deze waarde op `https://login.microsoftonline.com/<tenant name or ID>`|
> |`tokenReceivedCallback`| Callback-methode wordt aangeroepen nadat de verificatie wordt omgeleid naar de app. Hier `acquireTokenRedirectCallBack` wordt doorgegeven. Dit is null als loginPopup.|
> |`options`  |Een verzameling van de volgende optionele parameters. In dit geval `storeAuthStateInCookie` en `cacheLocation` zijn optionele configuratie. Zie de [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-basics#configuration-options) voor meer informatie over de opties. |

### <a name="sign-in-users"></a>Meld u aan gebruikers

Het volgende codefragment toont hoe u aan te melden bij gebruikers:

```javascript
myMSALObj.loginPopup(applicationConfig.graphScopes).then(function (idToken) {
    //Callback code here
})
```

> |Waar  |  |
> |---------|---------|
> | `scopes`   | (Optioneel) Bevat bereiken voor toestemming van de gebruiker tijdens het aanmelden wordt aangevraagd. Bijvoorbeeld, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste Web-API's (dat wil zeggen, `api://<Application ID>/access_as_user` ). Hier `applicationConfig.graphScopes` wordt doorgegeven. |

> [!TIP]
> U wilt ook gebruiken de `loginRedirect` methode zodat de huidige pagina worden omgeleid naar de aanmeldingspagina in plaats van een pop-upvenster.

### <a name="request-tokens"></a>Aanvragen van tokens

MSAL heeft drie methoden voor het verkrijgen van tokens: `acquireTokenRedirect`, `acquireTokenPopup` en `acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

De `acquireTokenSilent` methode wordt gebruikt voor token-aankopen en verlenging zonder tussenkomst van de gebruiker. Na de `loginRedirect` of `loginPopup` methode wordt uitgevoerd voor de eerste keer `acquireTokenSilent` is de methode die vaak worden gebruikt om te verkrijgen van tokens die worden gebruikt voor toegang tot beveiligde resources voor volgende aanroepen. Oproepen aan te vragen of vernieuwen van tokens worden op de achtergrond gemaakt.

```javascript
myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> |Waar  |  |
> |---------|---------|
> | `scopes`   | Bevat bereiken worden weergegeven in het toegangstoken voor de API wordt aangevraagd. Bijvoorbeeld, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste Web-API's (dat wil zeggen, `api://<Application ID>/access_as_user`). Hier `applicationConfig.graphScopes` wordt doorgegeven.|

#### <a name="get-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

Er zijn situaties waarin u wilt afdwingen dat gebruikers om te communiceren met Azure AD v2.0-eindpunt. Bijvoorbeeld:
* Gebruikers willen hun referenties opnieuw invoeren omdat het wachtwoord is verlopen
* Uw toepassing is toegang tot aanvullende resources bereiken die de gebruiker toestemming geven moet voor aanvragen
* Wanneer verificatie in twee stappen is vereist

De gebruikelijke aanbevolen patroon voor de meeste toepassingen is om aan te roepen `acquireTokenSilent` eerst vervolgens catch de uitzondering en roep vervolgens `acquireTokenRedirect` (of `acquireTokenPopup`) om te beginnen een interactieve-aanvraag.

Aanroepen van de `acquireTokenPopup(scope)` resulteert in een pop-upvenster aan te melden bij (of `acquireTokenRedirect(scope)` leidt gebruikers omleiden naar de Azure AD v2.0-eindpunt) wanneer gebruikers nodig hebben om te communiceren met een bevestiging van hun referenties de toestemming te verlenen tot de resource vereist, of voltooien van de verificatie met twee factoren.

```javascript
myMSALObj.acquireTokenPopup(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> [!NOTE]
> Deze snelstartgids maakt gebruik van de `loginRedirect` en `acquireTokenRedirect` methoden als de browser die wordt gebruikt Internet Explorer vanwege is een [bekende probleem](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) met betrekking tot de verwerking van pop-upvensters door Internet Explorer-browser.

## <a name="next-steps"></a>Volgende stappen

Probeer de volgende JavaScript-zelfstudie voor een meer gedetailleerde stapsgewijze handleiding voor het maken van de toepassing voor deze Quick Start.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Meer informatie over de stappen voor het maken van de toepassing voor deze Quick Start

> [!div class="nextstepaction"]
> [Zelfstudie voor het aanroepen van Graph API](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>De MSAL-opslagplaats voor documentatie, veelgestelde vragen en problemen bladeren

> [!div class="nextstepaction"]
> [msal.js GitHub-opslagplaats](https://github.com/AzureAD/microsoft-authentication-library-for-js)
