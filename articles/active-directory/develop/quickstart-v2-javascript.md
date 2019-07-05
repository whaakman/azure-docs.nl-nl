---
title: Microsoft identity platform JavaScript quickstart - Azure
description: Meer informatie over hoe een API die is vereist toegangstokens met behulp van Microsoft identity-platform kunnen aanroepen in JavaScript-toepassingen.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2182708ea459b578a2a9a94213ab41e76821aefc
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514352"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-single-page-application"></a>Quickstart: Meld u aan gebruikers en het verkrijgen van een toegangstoken van een JavaScript-toepassing met één pagina

In deze snelstartgids leert u hoe u een codevoorbeeld die u laat zien hoe een toepassing in JavaScript één pagina (SPA) gebruikers van persoonlijke accounts, werkaccounts en schoolaccounts kan aanmelden. Een JavaScript-SPA krijgt ook een toegangstoken voor het aanroepen van de Microsoft Graph API of een web-API.

![De werking van de voorbeeld-app in deze Quick Start](media/quickstart-v2-javascript/javascriptspa-intro.svg)

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart moet de volgende instellingen:
* Als u wilt het project met een Node.js-server uitvoert, download en installeer [Node.js](https://nodejs.org/en/download/).
* Als u wilt bewerken in de project-bestanden, download en installeer [Visual Studio Code](https://code.visualstudio.com/download).
* Als u wilt uitvoeren het project als een Visual Studio-oplossing, download en installeer [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>De quickstart-toepassing registreren en downloaden
> Voor het starten van uw toepassing Quick Start, moet u een van de volgende opties gebruiken.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1 (snelle): registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
>
> 1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een werk of school-account of een persoonlijk Microsoft-account.
> 1. Als uw account hebt u toegang tot meer dan één tenant, selecteert u het account in de rechterbovenhoek, en stel uw portal-sessie op de tenant Azure Active Directory (Azure AD) die u wilt gebruiken.
> 1. Ga naar de nieuwe [Azure portal - App-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) deelvenster.
> 1. Voer een naam voor uw toepassing en selecteer **registreren**.
> 1. Volg de instructies om te downloaden en uw nieuwe toepassing automatisch te configureren.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2 (handmatig): registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
>
> 1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met behulp van een werk of school-account of een persoonlijk Microsoft-account.
>
> 1. Als uw account hebt u toegang tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stel uw portal-sessie met de Azure AD-tenant die u wilt gebruiken.
> 1. Ga naar de Microsoft identity-platform voor ontwikkelaars [App-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) pagina.
> 1. Selecteer **registratie van nieuwe**.
> 1. Wanneer de pagina **Een toepassing registreren** wordt weergegeven, voert u een naam in voor de toepassing.
> 1. Selecteer onder **Ondersteunde accounttypen** de optie **Accounts in een organisatieadreslijst en persoonlijke Microsoft-account**.
> 1. Onder de **omleidings-URI** sectie in de vervolgkeuzelijst, selecteer de **Web** platform, en stel de waarde in op `http://localhost:30662/`.
> 1. Selecteer **Registreren**. Op de app **overzicht** pagina, houd er rekening mee de **(client) toepassings-ID** waarde voor later gebruik.
> 1. Voor deze quickstart moet de [Impliciete toekenningsstroom](v2-oauth2-implicit-grant-flow.md) zijn ingeschakeld. Selecteer in het linkerdeelvenster van de geregistreerde toepassing **verificatie**.
> 1. In de **geavanceerde instellingen** sectie onder **impliciete**, selecteer de **ID-tokens** en **toegangstokens** selectievakjes. ID-tokens en toegangstokens zijn vereist, omdat deze app moet het aanmelden van gebruikers en aanroepen van een API.
> 1. Selecteer aan de bovenkant van het deelvenster **opslaan**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Stap 1: uw toepassing configureren in Azure Portal
> Voor het codevoorbeeld voor deze Quick Start om te werken, moet u Voeg een omleidings-URI als `http://localhost:30662/` en in te schakelen **impliciete**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Breng deze wijzigingen voor mij aan]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-javascript/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken.

#### <a name="step-2-download-the-project"></a>Stap 2: Het project downloaden

Selecteer de optie die geschikt is voor uw ontwikkelomgeving:

* Het project wilt uitvoeren met een webserver met behulp van Node.js, [downloaden van de projectbestanden core](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip). Gebruiken voor het openen van de bestanden, een editor zoals [Visual Studio Code](https://code.visualstudio.com/).

* (Optioneel) Het project wilt uitvoeren met de IIS-server, [downloaden van het Visual Studio-project](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip). Pak het zip-bestand naar een lokale map (bijvoorbeeld *C:\Azure-Samples*).



#### <a name="step-3-configure-your-javascript-app"></a>Stap 3: Configureer de JavaScript-app

> [!div renderon="docs"]
> In de *JavaScriptSPA* map bewerken *index.html*, en stel de `clientID` en `authority` waarden onder `msalConfig`.

> [!div class="sxs-lookup" renderon="portal"]
> In de *JavaScriptSPA* map bewerken *index.html*, en vervang `msalConfig` door de volgende code:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_info_here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

```
> [!div renderon="portal"]
> > [!NOTE]
> > In deze snelstartgids ondersteunt Enter_the_Supported_Account_Info_Here.


> [!div renderon="docs"]
>
> Waar:
> - *\<Enter_the_Application_Id_here >* is de **(client) toepassings-ID** voor de toepassing die u hebt geregistreerd.
> - *\<Enter_the_Tenant_info_here >* is ingesteld op een van de volgende opties:
>    - Als uw toepassing ondersteunt *accounts in deze organisatie-map*, vervang deze waarde met de **Tenant-ID** of **tenantnaam** (bijvoorbeeld  *Contoso.Microsoft.com*).
>    - Als uw toepassing ondersteunt *accounts in een organisatie-map*, vervang deze waarde met **organisaties**.
>    - Als uw toepassing ondersteunt *accounts in een organisatie-map en de persoonlijke Microsoft-accounts*, vervang deze waarde met **algemene**. De ondersteuning om te beperken *persoonlijke Microsoft-accounts alleen*, vervang deze waarde met **consumenten**.
>
> > [!TIP]
> > Om de waarden van **Toepassings-id (client-id)** , **Map-id (tenant-id)** en **Ondersteunde accounttypen** te achterhalen, gaat u naar de **Overzichtspagina** van de app in de Azure-portal.
>

#### <a name="step-4-run-the-project"></a>Stap 4: Het project uitvoeren

* Als u [Node.js](https://nodejs.org/en/download/):

    1. Voer de volgende opdracht uit de projectmap voor het starten van de server:

        ```batch
        npm install
        node server.js
        ```

    1. Open een webbrowser en Ga naar `http://localhost:30662/`.
    1. Selecteer **aanmelden** starten van de aanmelding en roept u vervolgens Microsoft Graph API.


* Als u [Visual Studio](https://visualstudio.microsoft.com/downloads/), selecteert u de projectoplossing en selecteer vervolgens F5 het project wilt uitvoeren.

Nadat de toepassing in de browser geladen, selecteert u **aanmelden**. De eerste keer is dat u zich aanmeldt, moet u gevraagd voor toestemming van de toepassing om toegang tot uw profiel en u zich aanmeldt. Nadat u bent aangemeld, moet de gegevens van uw gebruikersprofiel worden weergegeven op de pagina.

## <a name="more-information"></a>Meer informatie

### <a name="msaljs"></a>msal.js

De MSAL-bibliotheek gebruikers worden aangemeld en vraagt de tokens die worden gebruikt voor toegang tot een API die wordt beveiligd door Microsoft identity-platform. De snelstartgids *index.html* bestand bevat een verwijzing naar de bibliotheek:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.min.js"></script>
```
> [!TIP]
> U kunt de vorige versie vervangen door de meest recente releaseversie onder [MSAL.js releases](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).


Als u Node.js geïnstalleerd hebt, kunt u ook de meest recente versie via Node.js Package Manager (npm) downloaden:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL initialiseren

De snelstartcode laat ook zien hoe de MSAL-bibliotheek initialiseren:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

var myMSALObj = new Msal.UserAgentApplication(msalConfig);
```

> |Waar  |  |
> |---------|---------|
> |`ClientId`     | De toepassings-ID van de toepassing die geregistreerd in Azure portal.|
> |`authority`    | (Optioneel) De URL van de instantie die ondersteuning biedt voor accounttypen, zoals eerder is beschreven in de configuratiesectie. De standaard-instantie is `https://login.microsoftonline.com/common`. |
> |`cacheLocation`  | (Optioneel) Hiermee stelt u de browser-opslag voor de verificatie-status. De standaardwaarde is sessionStorage.   |
> |`storeAuthStateInCookie`  | (Optioneel) De bibliotheek waarin de status van de authenticatie-aanvraag die is vereist voor de validatie van de verificatiestromen in de browsercookies. Dankzij deze cookie is ingesteld voor Internet Explorer en Microsoft Edge browser om bepaalde [bekende problemen](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues). |

Zie voor meer informatie over beschikbare configureerbare opties, [initialiseren clienttoepassingen](msal-js-initializing-client-applications.md).

### <a name="sign-in-users"></a>Gebruikers aanmelden

In het volgende codefragment wordt weergegeven hoe u gebruikers kunt aanmelden:

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
    //Login Success callback code here
}).catch(function (error) {
    console.log(error);
});
```

> |Waar  |  |
> |---------|---------|
> | `scopes`   | (Optioneel) Bevat de bereiken die worden aangevraagd voor toestemming van de gebruiker op de tijd van aanmelden. Bijvoorbeeld, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste web-API’s (`api://<Application ID>/access_as_user`). |

> [!TIP]
> U kunt ook u mogelijk wilt gebruiken de `loginRedirect` methode zodat de huidige pagina worden omgeleid naar de aanmeldingspagina in plaats van een pop-upvenster.

### <a name="request-tokens"></a>Tokens aanvragen

MSAL maakt gebruik van drie methoden om te verkrijgen van tokens: `acquireTokenRedirect`, `acquireTokenPopup`, en `acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

Met de methode `acquireTokenSilent` wordt het verkrijgen en vernieuwen van tokens verwerkt zonder tussenkomst van de gebruiker. Nadat de methode `loginRedirect` of `loginPopup` de eerste keer is uitgevoerd, wordt voor volgende aanroepen meestal de methode `acquireTokenSilent` gebruikt om tokens te verkrijgen die worden gebruikt voor toegang tot beveiligde resources. Aanroepen voor het aanvragen of vernieuwen van tokens worden op de achtergrond uitgevoerd.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> |Waar  |  |
> |---------|---------|
> | `scopes`   | Bevat bereiken die worden aangevraagd om te worden geretourneerd in het toegangstoken voor de API. Bijvoorbeeld, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste web-API’s (`api://<Application ID>/access_as_user`).|

#### <a name="get-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

Er zijn situaties waarin u wilt afdwingen dat gebruikers om te communiceren met het eindpunt van de Microsoft identity-platform. Bijvoorbeeld:
* Gebruikers moeten mogelijk hun referenties opnieuw invoeren omdat het wachtwoord is verlopen.
* Uw toepassing is toegang tot aanvullende resources bereiken die de gebruiker toestemming geven moet voor aanvragen.
* Tweeledige verificatie is vereist.

De gebruikelijke aanbevolen patroon voor de meeste toepassingen is om aan te roepen `acquireTokenSilent` eerst, klikt u vervolgens de uitzondering te ontdekken en deze vervolgens aanroepen `acquireTokenPopup` (of `acquireTokenRedirect`) om te beginnen een interactieve-aanvraag.

Aanroepen van de `acquireTokenPopup` resulteert in een pop-upvenster voor het aanmelden. (Of `acquireTokenRedirect` leidt gebruikers omleiden naar het eindpunt van de Microsoft identity-platform.) In dit venster moeten gebruikers communiceren door hun referenties bevestigen, de toestemming verlenen aan de vereiste resource of voltooien van de verificatie met twee factoren.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> [!NOTE]
> Deze snelstartgids maakt gebruik van de `loginRedirect` en `acquireTokenRedirect` methoden met Microsoft Internet Explorer, vanwege een [bekende probleem](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) met betrekking tot de verwerking van pop-upvensters door Internet Explorer.

## <a name="next-steps"></a>Volgende stappen

Zie voor een meer gedetailleerde stapsgewijze handleiding voor het bouwen van de toepassing voor deze Quick Start:

> [!div class="nextstepaction"]
> [Zelfstudie voor het aanmelden en aanroepen van MS Graph](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

Als u de MSAL-opslagplaats voor documentatie, veelgestelde vragen over, problemen en informatie, Zie:

> [!div class="nextstepaction"]
> [MSAL.js GitHub repo](https://github.com/AzureAD/microsoft-authentication-library-for-js)
