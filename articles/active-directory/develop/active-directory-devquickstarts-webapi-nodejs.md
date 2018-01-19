---
title: Azure AD Node.js-web-API aan de slag | Microsoft Docs
description: "Het bouwen van een REST Node.js-web-API die kan worden geïntegreerd met Azure AD voor verificatie."
services: active-directory
documentationcenter: nodejs
author: craigshoemaker
manager: mtillman
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 11/30/2017
ms.author: cshoe
ms.custom: aaddev
ms.openlocfilehash: 2ed0874b79601976e0d5a73fe82c7c03331d9bea
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-nodejs-web-api-getting-started"></a>Azure AD Node.js-web-API aan de slag

In dit artikel laat zien hoe u voor het beveiligen van een [Restify](http://restify.com/) API-eindpunt met [Passport](http://passportjs.org/) met behulp van de [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) module voor het afhandelen van communicatie met Azure Active Directory (AAD). 

Het bereik van deze handleiding bevat informatie over de problemen met betrekking tot de API-eindpunten beveiligen. De problemen van aanmelden en behoud verificatietokens hier niet worden geïmplementeerd en zijn de verantwoordelijkheid van een clienttoepassing. Bekijk voor meer informatie rondom de clientimplementatie van een [Node.js-web-app aanmelden en afmelden met Azure AD](active-directory-devquickstarts-openidconnect-nodejs.md).

Het volledige voorbeeld die zijn gekoppeld aan dit artikel is beschikbaar op [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi-basic).

## <a name="create-the-sample-project"></a>Het voorbeeldproject maken
Deze servertoepassing vereist een paar pakketafhankelijkheden naar Restify en Passport ondersteunen, evenals informatie die wordt doorgegeven aan de AAD-account.

Om te beginnen, kunt u de volgende code toevoegen in een bestand met de naam `package.json`:

```Shell
{
  "name": "node-aad-demo",
  "version": "0.0.1",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "passport": "0.4.0",
    "passport-azure-ad": "3.0.8",
    "restify": "6.0.1",
    "restify-plugins": "1.6.0"
  }
}
```

Eenmaal `package.json` is gemaakt, voer `npm install` in het opdrachtvenster voor het installeren van de pakketafhankelijkheden. 

### <a name="configure-the-project-to-use-active-directory"></a>Het project voor het gebruik van Active Directory configureren

Om te beginnen configureren van de toepassing, zijn er enkele account-specifieke waarden die kunt u met Azure CLI. De eenvoudigste manier om aan de slag met de CLI is met de Azure-Cloud-Shell.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Voer de volgende opdracht in de cloud-shell: 

```azurecli-interactive
az ad app create --display-name node-aad-demo --homepage http://localhost --identifier-uris http://node-aad-demo
```

De [argumenten](/cli/azure/ad/app?view=azure-cli-latest#az_ad_app_create) voor de `create` opdracht bevatten:

| Argument  | Beschrijving |
|---------|---------|
|`display-name` | Beschrijvende naam van de inschrijving |
|`homepage` | De URL waar gebruikers kunnen aanmelden en uw toepassing gebruiken |
|`identifier-uris` | Ruimte gescheiden unieke URI's die Azure AD voor deze toepassing gebruiken kunt |

Voordat u verbinding met Azure Active Directory maken kunt, moet u de volgende informatie:

| Naam  | Beschrijving | Naam van de variabele in het configuratiebestand |
| ------------- | ------------- | ------------- |
| De Tenantnaam van de  | [Tenantnaam](active-directory-howto-tenant.md) u wilt gebruiken voor verificatie | `tenantName`  |
| Client-id  | Client-ID is de OAuth-term die wordt gebruikt voor de AAD _toepassings-ID_. |  `clientID`  |

Kopiëren van het registratie-antwoord in de Azure-Cloud-Shell de `appId` waarde en maak een nieuw bestand met de naam `config.js`. Vervolgens toevoegen in de volgende code en de waarden vervangt door de tussen haakjes tokens:

```JavaScript
const tenantName    = //<YOUR_TENANT_NAME>;
const clientID      = //<YOUR_APP_ID_FROM_CLOUD_SHELL>;
const serverPort    = 3000;

module.exports.serverPort = serverPort;

module.exports.credentials = {
  identityMetadata: `https://login.microsoftonline.com/${tenantName}.onmicrosoft.com/.well-known/openid-configuration`, 
  clientID: clientID
};
```
Raadpleeg voor meer informatie over de afzonderlijke configuratie-instellingen de [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) module documentatie.

## <a name="implement-the-server"></a>De server implementeren
De [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) module biedt twee verificatiestrategieën: [OIDC](https://github.com/AzureAD/passport-azure-ad#51-oidcstrategy) en [Bearer](https://github.com/AzureAD/passport-azure-ad#52-bearerstrategy) strategieën. De server die is geïmplementeerd in dit artikel gebruikt de Bearer-strategie voor het beveiligen van de API-eindpunt.

### <a name="step-1-import-dependencies"></a>Stap 1: Import afhankelijkheden
Maak een nieuw bestand met de naam `app.js` en plak de volgende tekst:

```JavaScript
const
      restify = require('restify')
    , restifyPlugins = require('restify-plugins')
    , passport = require('passport')
    , BearerStrategy = require('passport-azure-ad').BearerStrategy
    , config = require('./config')
    , authenticatedUserTokens = []
    , serverPort = process.env.PORT || config.serverPort
;
```

In dit gedeelte van de code:

- De `restify` en `restify-plugins` modules wordt verwezen om een Restify-server instellen.

- De `passport` en `passport-azure-ad` modules verantwoordelijk zijn voor communicatie met AAD.

- De `config` variabele is geïnitialiseerd met waarden uit de `config.js` bestand in de vorige stap hebt gemaakt.

- Een matrix is gemaakt voor `authenticatedUserTokens` voor het opslaan van de gebruikerstokens zoals ze zijn doorgegeven aan beveiligde eindpunten.

- De `serverPort` is gedefinieerd van de procesomgeving poort of van het configuratiebestand.

### <a name="step-2-instantiate-an-authentication-strategy"></a>Stap 2: Een verificatiestrategie instantiëren
Als u een eindpunt beveiligt, moet u een strategie voor verantwoordelijk voor het bepalen of de huidige aanvraag afkomstig is van een geverifieerde gebruiker of niet opgeven. Hier de `authenticatonStrategy` variabele is een exemplaar van de `passport-azure-ad` `BearerStrategy` klasse. Voeg de volgende code na de `require` instructies.

```JavaScript
const authenticationStrategy = new BearerStrategy(config.credentials, (token, done) => {
    let currentUser = null;

    let userToken = authenticatedUserTokens.find((user) => {
        currentUser = user;
        user.sub === token.sub;
    });

    if(!userToken) {
        authenticatedUserTokens.push(token);
    }

    return done(null, currentUser, token);
});
```
Deze implementatie maakt gebruik van automatische registratie door toe te voegen verificatietokens in de `authenticatedUserTokens` matrix als deze niet al bestaan.

Wanneer een nieuw exemplaar van de strategie is gemaakt, moet u het doorgeven bij Passport via de `use` methode. Voeg de volgende code naar `app.js` gebruik van de strategie in Passport.

```JavaScript
passport.use(authenticationStrategy);
```

### <a name="step-3-server-configuration"></a>Stap 3: Server-configuratie
Met de verificatiestrategie is gedefinieerd, kunt u nu de Restify-server met een aantal basisinstellingen instellen en instellen op Passport gebruiken voor beveiliging.

```JavaScript
const server = restify.createServer({ name: 'Azure Active Directroy with Node.js Demo' });
server.use(restifyPlugins.authorizationParser());
server.use(passport.initialize());
server.use(passport.session());
```
Deze server is geïnitialiseerd en geconfigureerd voor het parseren van de autorisatie-header en stel vervolgens Passport gebruiken.


### <a name="step-4-define-routes"></a>Stap 4: Definieert routes
U kunt nu definiëren van routes en bepalen welke beveiligen met AAD. Dit project bevat twee routes waar het hoogste niveau geopend is en de `/api` route is ingesteld op verificatie vereisen.

In `app.js` Voeg de volgende code voor de hoofdmap niveau route:

```JavaScript
server.get('/', (req, res, next) => {
    res.send(200, 'Try: curl -isS -X GET http://127.0.0.1:3000/api');
    next();
});
```

De route root kunt u alle aanvragen via de route en retourneert een bericht met een opdracht voor het testen van de `/api` route. Daarentegen is de `/api` route is vergrendeld met behulp van [ `passport.authenticate` ](http://passportjs.org/docs/authenticate). Voeg de volgende code na de basis-route.

```JavaScript
server.get('/api', passport.authenticate('oauth-bearer', { session: false }), (req, res, next) => {
    res.json({ message: 'response from API endpoint' });
    return next();
});
```

Met deze configuratie kan alleen geverifieerde aanvragen met een bearer-token toegang tot `/api`. De optie `session: false` uitschakelen sessies om ervoor te zorgen dat een token wordt doorgegeven aan elke aanvraag naar de API wordt gebruikt.

Ten slotte de server is ingesteld om te luisteren op de geconfigureerde poort door het aanroepen van de `listen` methode.

```JavaScript
server.listen(serverPort);
```

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Nu dat de server is geïmplementeerd, kunt u de server niet starten via een opdrachtprompt en voer:

```Shell
npm start
```

U kunt met de server met een aanvraag voor de server kan de testresultaten verzenden. Om aan te tonen de reactie van de route hoofdmap, open een bash-shell en voer de volgende code:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/
```

Als u uw server juist hebt geconfigureerd, het antwoord moet er ongeveer uitzien:

```Shell
HTTP/1.1 200 OK
Server: Azure Active Directroy with Node.js Demo
Content-Type: application/json
Content-Length: 49
Date: Tue, 10 Oct 2017 18:35:13 GMT
Connection: keep-alive

Try: curl -isS -X GET http://127.0.0.1:3000/api
```

Vervolgens kunt u de route die moet worden geverifieerd door te voeren van de volgende opdracht in uw bash-shell testen:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/api
```

Als u de server juist hebt geconfigureerd, wordt de server met de status van reageren moet `Unauthorized`.

```Shell
HTTP/1.1 401 Unauthorized
Server: Azure Active Directroy with Node.js Demo
WWW-Authenticate: token is not found
Date: Tue, 10 Oct 2017 16:22:03 GMT
Connection: keep-alive
Content-Length: 12

Unauthorized
```
Nu dat u een beveiligde API hebt gemaakt, kunt u een client die verificatietokens doorgeven aan de API kan implementeren.

## <a name="next-steps"></a>Volgende stappen
Zoals vermeld in de introductie, moet u een exemplaar van de client verbinding maken met de server die verantwoordelijk is voor aanmelden, afmelden en het beheren van tokens implementeren. Voor voorbeelden op basis van code die u kunt verwijzen naar de clienttoepassingen in [iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) en [Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android). Raadpleeg het volgende artikel voor een stapsgewijze zelfstudie:

> [!div class="nextstepaction"]
> [Node.js-web-app aanmelden en afmelden met Azure AD](active-directory-devquickstarts-openidconnect-nodejs.md)