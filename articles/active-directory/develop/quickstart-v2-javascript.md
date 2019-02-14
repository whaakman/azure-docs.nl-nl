---
title: Quickstart voor Azure AD v2 JavaScript | Microsoft Docs
description: Informatie over hoe met JavaScript-toepassingen een API kan worden aangeroepen die toegangstokens nodig heeft van Azure Active Directory v2.0-eindpunt
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6a97e03f3c195b9fbd0ee7a09950414b7a940c7c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217476"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Quickstart: Gebruikers aanmelden en een toegangstoken verkrijgen van een JavaScript-toepassing

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

In deze quickstart leert u hoe u een codevoorbeeld gebruikt waarmee wordt gedemonstreerd hoe via een toepassing met één pagina (SPA) van JavaScript persoonlijke accounts, en werk- en schoolaccounts kunnen worden aangemeld, en een toegangstoken kan worden opgehaald om de Microsoft Graph API of een andere web-API aan te roepen.

![Hoe de voorbeeld-app werkt die is gegenereerd door deze snelstart](media/quickstart-v2-javascript/javascriptspa-intro.png)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>De quickstart-toepassing registreren en downloaden
> U hebt twee opties voor het starten van de snelstarttoepassing:
> * [Express] [Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [Handmatig] [Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: registreer de toepassing en laat deze automatisch configureren. Download vervolgens het codevoorbeeld
>
> 1. Ga naar de [Azure Portal - Toepassingsregistratie (preview)](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs).
> 1. Voer een naam in voor de toepassing en klik op **Registreren**.
> 1. Volg de instructies om de nieuwe toepassing met slechts één klik te downloaden en automatisch te configureren.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
>
> 1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) om een toepassing te registreren.
> 1. Als u via uw account toegang hebt tot meer dan één tenant, selecteert u uw account in de rechterbovenhoek en stelt u de portalsessie in op de gewenste Azure Active Directory-tenant.
> 1. Selecteer in het linkernavigatiedeelvenster de **Azure Active Directory**-service en selecteer vervolgens **App-registraties (preview) > Nieuwe registratie**.
> 1. Wanneer de pagina **Een toepassing registreren** wordt weergegeven, voert u een naam in voor de toepassing.
> 1. Selecteer onder **Ondersteunde accounttypen** de optie **Accounts in een organisatieadreslijst en persoonlijke Microsoft-account**.
> 1. Selecteer in de sectie **Omleidings-URL** het **Webplatform** en stel de waarde in op `http://localhost:30662/`.
> 1. Selecteer **Registreren** wanneer u klaar bent.  Noteer de waarde **Toepassings-id (client)** op de app-pagina **Overzicht**.
> 1. Voor deze quickstart moet de [Impliciete toekenningsstroom](v2-oauth2-implicit-grant-flow.md) zijn ingeschakeld. Selecteer in het deelvenster aan de linkerkant van de geregistreerde toepassing de optie **Verificatie**.
> 1. Schakel in **Geavanceerde instellingen**, onder **Impliciete toekenning**, de selectievakjes **Id-tokens** en **toegangstoken** in. Id-tokens en toegangstokens zijn vereist, omdat via de app gebruikers moeten worden aangemeld en een API moet worden aangeroepen.
> 1. Selecteer **Opslaan**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Stap 1: uw toepassing configureren in Azure Portal
> Voor een juiste werking van het codevoorbeeld uit deze quickstart moet u een omleidings-URI toevoegen zoals `http://localhost:30662/` en **Impliciete toekenning** inschakelen.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Breng deze wijzigingen voor mij aan]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-javascript/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken.

#### <a name="step-2-download-the-project"></a>Stap 2: Het project downloaden

U kunt een van deze opties kiezen die geschikt is voor uw ontwikkelomgeving.
* [Download de core projectbestanden - voor een webserver, zoals Node.js](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)
* [Download het Visual Studio-project](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)

Pak het ZIP-bestand uit in een lokale map, bijvoorbeeld **C:\Azure-Samples**.

#### <a name="step-3-configure-your-javascript-app"></a>Stap 3: Configureer de JavaScript-app

> [!div renderon="docs"]
> Bewerk `index.html` en stel de waarden `clientID` en `authority` in onder `applicationConfig`.

> [!div class="sxs-lookup" renderon="portal"]
> Bewerk `index.html` en vervang `applicationConfig` door:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
> [!div renderon="docs"]
>
> Waar:
> - `Enter_the_Application_Id_here`: is de **toepassings-id (client-id)** voor de toepassing die u hebt geregistreerd.
> - `Enter_the_Tenant_Info_Here`: is ingesteld op een van de volgende opties:
>   - Als uw toepassing **Alleen accounts in deze organisatiemap** ondersteunt, vervang deze waarde dan door de **Tenant-id** of **Tenantnaam** (bijvoorbeeld contoso.microsoft.com)
>   - Als uw toepassing **Accounts in elke organisatiemap** ondersteunt, vervang deze waarde dan door `organizations`
>   - Als uw toepassing **Accounts in elke organisatiemap en persoonlijke Microsoft-accounts** ondersteunt, vervang deze waarde dan door `common`
>
> > [!TIP]
> > Om de waarden van **Toepassings-id (client-id)**, **Map-id (tenant-id)** en **Ondersteunde accounttypen** te achterhalen, gaat u naar de **Overzichtspagina** van de app in de Azure-portal.

> [!NOTE]
> De server is geconfigureerd om te luisteren op poort 30662 in het *server.js*-bestand in het project [Node.js](https://nodejs.org/en/download/) en het *.csproj*-bestand in het project [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
>

#### <a name="step-4-run-the-project"></a>Stap 4: Het project uitvoeren

* Als u Node.js gebruikt:

    1. Voer de volgende opdracht uit vanuit de projectmap om de server te starten:

        ```batch
        npm install
        node server.js
        ```

    1. Open een webbrowser en ga naar `http://localhost:30662/`.
    1. Klik op de knop **Aanmelden** om het aanmelden te starten en roep vervolgens de Microsoft Graph API aan.


* Als u Visual Studio gebruikt, moet u de projectoplossing selecteren en vervolgens op **F5** drukken om het project uit te voeren.

## <a name="more-information"></a>Meer informatie

### <a name="msaljs"></a>*msal.js*

MSAL is de bibliotheek die wordt gebruikt voor het aanmelden van gebruikers en de aanvraagtokens die worden gebruikt voor toegang tot een API die is beveiligd via Microsoft Azure AD (Azure Active Directory). De *index.html* van de quickstart bevat een referentie naar de bibliotheek:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.min.js"></script>
```

Als u Node hebt geïnstalleerd, kunt u er ook voor kiezen om deze te downloaden via npm:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL initialiseren

De quickstart-code laat ook zien hoe u de bibliotheek kunt initialiseren:

```javascript
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, applicationConfig.authority, acquireTokenRedirectCallBack, {storeAuthStateInCookie: true, cacheLocation: "localStorage"});
```

> |Waar  |  |
> |---------|---------|
> |`ClientId`     |Toepassings-id van de toepassing die is geregistreerd in Azure Portal|
> |`authority`    |Het is de URL van de instantie. Doorgeven van *null* stelt de standaardinstantie in op `https://login.microsoftonline.com/common`. Als de app één tenant heeft (doelaccounts in één richting), stelt u deze waarde in op `https://login.microsoftonline.com/<tenant name or ID>`|
> |`tokenReceivedCallback`| Aanroepmethode wordt aangeroepen wanneer u, na de verificatie, terug naar de app wordt geleid. Hier wordt `acquireTokenRedirectCallBack` doorgegeven. Dit is null als loginPopup wordt gebruikt.|
> |`options`  |Een verzameling met optionele parameters. In dit geval zijn `storeAuthStateInCookie` en `cacheLocation` optionele configuraties. Raadpleeg de [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-basics#configuration-options) voor meer details over de opties. |

### <a name="sign-in-users"></a>Gebruikers aanmelden

In het volgende codefragment wordt weergegeven hoe u gebruikers kunt aanmelden:

```javascript
myMSALObj.loginPopup(applicationConfig.graphScopes).then(function (idToken) {
    //Callback code here
})
```

> |Waar  |  |
> |---------|---------|
> | `scopes`   | Bevat bereiken die worden aangevraagd voor gebruikerstoestemming tijdens het aanmelden (optioneel). Bijvoorbeeld, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste web-API’s (`api://<Application ID>/access_as_user`). Hier wordt `applicationConfig.graphScopes` doorgegeven. |

> [!TIP]
> U kunt er ook voor kiezen om de methode `loginRedirect` te gebruiken om de huidige pagina om te leiden naar de aanmeldingspagina, in plaats van een pop-upvenster.

### <a name="request-tokens"></a>Tokens aanvragen

MSAL biedt drie methoden om tokens te verkrijgen: `acquireTokenRedirect`, `acquireTokenPopup` en `acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Een gebruikerstoken op de achtergrond ophalen

Met de methode `acquireTokenSilent` wordt het verkrijgen en vernieuwen van tokens verwerkt zonder tussenkomst van de gebruiker. Nadat de methode `loginRedirect` of `loginPopup` de eerste keer is uitgevoerd, wordt voor volgende aanroepen meestal de methode `acquireTokenSilent` gebruikt om tokens te verkrijgen die worden gebruikt voor toegang tot beveiligde resources. Aanroepen voor het aanvragen of vernieuwen van tokens worden op de achtergrond uitgevoerd.

```javascript
myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> |Waar  |  |
> |---------|---------|
> | `scopes`   | Bevat bereiken die worden aangevraagd om te worden geretourneerd in het toegangstoken voor de API. Bijvoorbeeld, `[ "user.read" ]` voor Microsoft Graph of `[ "<Application ID URL>/scope" ]` voor aangepaste web-API’s (`api://<Application ID>/access_as_user`). Hier wordt `applicationConfig.graphScopes` doorgegeven.|

#### <a name="get-a-user-token-interactively"></a>Een gebruikerstoken interactief ophalen

Er zijn situaties waarin u de gebruikers moet dwingen om te werken met het Azure AD v2.0-eindpunt. Bijvoorbeeld:
* Gebruikers moeten mogelijk hun referenties opnieuw opgeven omdat het wachtwoord is verlopen
* De toepassing vraagt toegang tot aanvullende resourcebereiken waarvoor de gebruiker toestemming moet geven
* Wanneer verificatie in twee stappen is vereist

Het patroon dat gewoonlijk wordt aanbevolen voor de meeste toepassingen: eerst `acquireTokenSilent` aanroepen, vervolgens de uitzondering detecteren, en daarna `acquireTokenRedirect` (of `acquireTokenPopup`0) aanroepen om een interactieve aanvraag te starten.

Als u `acquireTokenPopup(scope)` aanroept, wordt er een pop-upvenster weergegeven (of als u `acquireTokenRedirect(scope)` aanroept, wordt gebruikers naar het Azure AD v.2.0-eindpunt geleid). In dit venster moeten gebruikers hun referenties bevestigen, waarmee ze de vereiste resource toestemming geven, of ze moeten verificatie in twee stappen voltooien.

```javascript
myMSALObj.acquireTokenPopup(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> [!NOTE]
> In deze quickstart wordt gebruikgemaakt van de methoden `loginRedirect` en `acquireTokenRedirect`, wanneer de gebruikte browser Internet Explorer is, vanwege een [bekend probleem](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) dat te maken heeft met het verwerken van pop-upvensters in de Internet Explorer-browser.

## <a name="next-steps"></a>Volgende stappen

Probeer onze JavaScript-zelfstudie hieronder voor een meer gedetailleerde stapsgewijze handleiding voor het bouwen van de toepassing in deze quickstart.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Leer de stappen voor het maken van de toepassing die wordt gebruikt in deze quickstart

> [!div class="nextstepaction"]
> [Zelfstudie voor het aanroepen van Graph API](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>Verken de MSAL-opslagplaats voor documentatie, veelgestelde vragen, problemen, en meer

> [!div class="nextstepaction"]
> [msal.js GitHub-opslagplaats](https://github.com/AzureAD/microsoft-authentication-library-for-js)
