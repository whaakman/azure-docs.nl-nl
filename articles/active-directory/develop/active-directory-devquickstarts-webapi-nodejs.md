---
title: Azure AD-Node.js-web-API aan de slag | Microsoft Docs
description: Over het bouwen van een web-API die kan worden geïntegreerd met Azure AD voor de verificatie van de Node.js-REST.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 1137e7164ac83a2ee0bf05804296aeeb5c3496fb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437444"
---
# <a name="azure-ad-nodejs-web-api-getting-started"></a>Azure AD-Node.js-web-API aan de slag

In dit artikel ziet u hoe u voor het beveiligen van een [Restify](http://restify.com/) API-eindpunt [Passport](http://passportjs.org/) met behulp van de [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) module voor het afhandelen van communicatie met Azure Active Directory (AAD). 

Het bereik van deze zelfstudie bevat informatie over de problemen met betrekking tot de API-eindpunten beveiligen. De bezorgdheid van het aanmelden en behoud van verificatietokens hier niet worden geïmplementeerd en zijn de verantwoordelijkheid van een clienttoepassing. Voor meer informatie rond een clientimplementatie bekijken [Node.js-web-app aanmelden en afmelden met Azure AD](active-directory-devquickstarts-openidconnect-nodejs.md).

Het volledige codevoorbeeld die zijn gekoppeld aan dit artikel vindt u op [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi-basic).

## <a name="create-the-sample-project"></a>Het voorbeeldproject maken
Deze servertoepassing moet een paar pakketafhankelijkheden te ondersteunen Restify en Passport, alsmede informatie die wordt doorgegeven aan de AAD-account.

Als u wilt, kunt u de volgende code toevoegen in een bestand met de naam `package.json`:

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

Eenmaal `package.json` is gemaakt, worden uitgevoerd `npm install` in de opdrachtprompt voor het installeren van de pakketafhankelijkheden. 

### <a name="configure-the-project-to-use-active-directory"></a>Het project voor het gebruik van Active Directory configureren

Om te beginnen de toepassing configureren, zijn er enkele accountspecifiek waarden die u via de Azure CLI verkrijgen kunt. De eenvoudigste manier om aan de slag met de CLI is met de Azure Cloud Shell.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Voer de volgende opdracht in de cloudshell: 

```azurecli-interactive
az ad app create --display-name node-aad-demo --homepage http://localhost --identifier-uris http://node-aad-demo
```

De [argumenten](/cli/azure/ad/app?view=azure-cli-latest#az-ad-app-create) voor de `create` opdracht:

| Argument  | Beschrijving |
|---------|---------|
|`display-name` | Beschrijvende naam van de registratie |
|`homepage` | De URL waar gebruikers kunnen zich aanmelden en uw toepassing gebruiken |
|`identifier-uris` | Ruimte gescheiden unieke URI's die Azure AD voor deze toepassing gebruiken kunt |

Voordat u verbinding met Azure Active Directory maken kunt, moet u de volgende informatie:

| Naam  | Beschrijving | De naam van de variabele in het configuratiebestand |
| ------------- | ------------- | ------------- |
| Tenantnaam  | [De naam van tenant](active-directory-howto-tenant.md) u wilt gebruiken voor verificatie | `tenantName`  |
| Client-id  | Client-ID is de OAuth-term die wordt gebruikt voor de AAD _toepassings-ID_. |  `clientID`  |

Kopiëren van het registratie-antwoord in de Azure Cloud Shell de `appId` waarde en maak een nieuw bestand met de naam `config.js`. Vervolgens voegen in de volgende code en de waarden vervangt door de tussen haakjes tokens:

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
Raadpleeg voor meer informatie over de afzonderlijke configuratie-instellingen, de [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) module-documentatie.

## <a name="implement-the-server"></a>De server implementeren
De [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) module biedt twee verificatiestrategieën: [OIDC](https://github.com/AzureAD/passport-azure-ad#51-oidcstrategy) en [Bearer](https://github.com/AzureAD/passport-azure-ad#52-bearerstrategy) strategieën. De server die is geïmplementeerd in dit artikel gebruikt de Bearer-strategie voor het beveiligen van de API-eindpunt.

### <a name="step-1-import-dependencies"></a>Stap 1: Importeren van afhankelijkheden
Maak een nieuw bestand met de naam `app.js` en plak deze in de volgende tekst:

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

In deze sectie van de code:

- De `restify` en `restify-plugins` modules om in te stellen van een Restify-server wordt verwezen.

- De `passport` en `passport-azure-ad` modules zijn verantwoordelijk voor de communicatie met AAD.

- De `config` variabele is geïnitialiseerd met waarden uit de `config.js` bestand in de vorige stap hebt gemaakt.

- Een matrix is gemaakt voor `authenticatedUserTokens` om op te slaan gebruikerstokens zoals ze worden doorgegeven aan de beveiligde eindpunten.

- De `serverPort` is een van de poort van de procesomgeving of van het configuratiebestand gedefinieerd.

### <a name="step-2-instantiate-an-authentication-strategy"></a>Stap 2: Instantieer een verificatiestrategie voor
Als u een eindpunt beveiligt, moet u een strategie voor verantwoordelijk voor het bepalen of de huidige aanvraag afkomstig is van een geverifieerde gebruiker al dan niet opgeven. Hier de `authenticatonStrategy` variabele is een exemplaar van de `passport-azure-ad` `BearerStrategy` klasse. Voeg de volgende code na de `require` instructies.

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
Deze implementatie maakt gebruik van automatische registratie door toe te voegen verificatietokens in de `authenticatedUserTokens` matrix als deze nog niet bestaan.

Zodra een nieuw exemplaar van de strategie is gemaakt, moet u deze doorgeven aan Passport via de `use` methode. Voeg de volgende code aan `app.js` het gebruik van de strategie in Passport.

```JavaScript
passport.use(authenticationStrategy);
```

### <a name="step-3-server-configuration"></a>Stap 3: Server-configuratie
Met de verificatiestrategie is gedefinieerd, kunt u nu de Restify-server met sommige basisinstellingen instellen en ingesteld voor het gebruik van Passport voor beveiliging.

```JavaScript
const server = restify.createServer({ name: 'Azure Active Directroy with Node.js Demo' });
server.use(restifyPlugins.authorizationParser());
server.use(passport.initialize());
server.use(passport.session());
```
Deze server is geïnitialiseerd en geconfigureerd voor het parseren van autorisatie-header en vervolgens ingesteld voor het gebruik van Passport.


### <a name="step-4-define-routes"></a>Stap 4: Definieert routes
U kunt nu definieert routes en bepalen die u wilt beveiligen met AAD. Dit project bevat twee routes waar het hoogste niveau doen geopend is en de `/api` route is ingesteld op verificatie vereist is.

In `app.js` Voeg de volgende code voor de route van hoofdmap-niveau:

```JavaScript
server.get('/', (req, res, next) => {
    res.send(200, 'Try: curl -isS -X GET http://127.0.0.1:3000/api');
    next();
});
```

De route van hoofdmap kunnen alle aanvragen via de route en retourneert een bericht met een opdracht voor het testen van de `/api` route. Daarentegen is de `/api` route is vergrendeld met behulp van [ `passport.authenticate` ](http://passportjs.org/docs/authenticate). Voeg de volgende code na de route van hoofdmap.

```JavaScript
server.get('/api', passport.authenticate('oauth-bearer', { session: false }), (req, res, next) => {
    res.json({ message: 'response from API endpoint' });
    return next();
});
```

Deze configuratie alleen kunt u geverifieerde aanvragen met een bearer-token toegang tot `/api`. De mogelijkheid om `session: false` wordt gebruikt voor het uitschakelen van sessies om ervoor te zorgen dat een token met elke aanvraag wordt doorgegeven aan de API.

Ten slotte de server is ingesteld om te luisteren op de geconfigureerde poort door het aanroepen van de `listen` methode.

```JavaScript
server.listen(serverPort);
```

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Nu dat de server is geïmplementeerd, kunt u de server niet starten via een opdrachtprompt en voer:

```Shell
npm start
```

Terwijl de server wordt uitgevoerd, kunt u een aanvraag naar de server voor het testen van de resultaten kunt indienen. Om te demonstreren het antwoord van de route van hoofdmap, open een bash-shell en voer de volgende code:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/
```

Als u uw server correct hebt geconfigureerd, het antwoord moet er ongeveer uitzien:

```Shell
HTTP/1.1 200 OK
Server: Azure Active Directroy with Node.js Demo
Content-Type: application/json
Content-Length: 49
Date: Tue, 10 Oct 2017 18:35:13 GMT
Connection: keep-alive

Try: curl -isS -X GET http://127.0.0.1:3000/api
```

Test vervolgens de route die moet worden geverifieerd met de volgende opdracht in de bash-shell:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/api
```

Als u de server correct hebt geconfigureerd, wordt de server met de status van reageren moet `Unauthorized`.

```Shell
HTTP/1.1 401 Unauthorized
Server: Azure Active Directroy with Node.js Demo
WWW-Authenticate: token is not found
Date: Tue, 10 Oct 2017 16:22:03 GMT
Connection: keep-alive
Content-Length: 12

Unauthorized
```
Nu dat u een beveiligde API hebt gemaakt, kunt u een client die kan verificatietokens doorgeven aan de API kunt implementeren.

## <a name="next-steps"></a>Volgende stappen
Zoals vermeld in de inleiding, moet u een equivalent van de client verbinding maken met de server die verantwoordelijk is voor het aanmelden, afmelden en beheren van tokens implementeren. Voor voorbeelden van code op basis van die u kunt verwijzen naar de clienttoepassingen in [iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) en [Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android). Raadpleeg het volgende artikel voor een stapsgewijze zelfstudie:

> [!div class="nextstepaction"]
> [Node.js-web-app aanmelden en afmelden met Azure AD](active-directory-devquickstarts-openidconnect-nodejs.md)