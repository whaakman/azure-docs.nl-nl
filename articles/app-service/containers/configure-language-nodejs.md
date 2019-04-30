---
title: Node.js-apps - Azure App Service configureren | Microsoft Docs
description: Informatie over het configureren van Node.js-apps werken in Azure App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: 43dc76e6d1e1ec2a6167f1d3e3cc7b8780f843db
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850235"
---
# <a name="configure-a-linux-nodejs-app-for-azure-app-service"></a>Een Linux-Node.js-app configureren voor Azure App Service

Node.js-apps moeten worden geïmplementeerd met alle vereiste NPM-afhankelijkheden. De App Service-implementatie-engine (Kudu) wordt automatisch uitgevoerd `npm install --production` voor u wanneer u implementeert een [Git-opslagplaats](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), of een [Zip-pakket](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) met buildprocessen die zijn ingeschakeld. Als u uw bestanden met behulp van implementeert [FTP/S](../deploy-ftp.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), u moet echter handmatig uploaden van de vereiste pakketten.

Deze handleiding bevat de belangrijkste concepten en instructies voor het Node.js-ontwikkelaars die gebruikmaken van een ingebouwde Linux-container in App Service. Als u Azure App Service nog nooit hebt gebruikt, volgt u de [Node.js Quick Start](quickstart-nodejs.md) en [Node.js met MongoDB zelfstudie](tutorial-nodejs-mongodb-app.md) eerste.

## <a name="show-nodejs-version"></a>Node.js versie weergeven

Als u wilt weergeven van de huidige versie van Node.js, kunt u de volgende opdracht uitvoeren de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Om weer te geven voor alle ondersteunde versies van Node.js, voert u de volgende opdracht uit de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep NODE
```

## <a name="set-nodejs-version"></a>Node.js-versie instellen

Om uw app in te stellen een [ondersteunde versie van Node.js](#show-nodejs-version), voer de volgende opdracht uit de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "NODE|10.14"
```

Deze instelling geeft u de Node.js-versie moet worden gebruikt, zowel tijdens runtime als tijdens het terugzetten in Kudu geautomatiseerd pakket.

> [!NOTE]
> U moet de Node.js-versie instellen in van uw project `package.json`. De implementatie-engine wordt uitgevoerd in een afzonderlijke container met alle ondersteunde versies van Node.js.

## <a name="configure-nodejs-server"></a>Node.js-server configureren

De Node.js-containers worden geleverd met [PM2](http://pm2.keymetrics.io/), de manager van een productie-proces. U kunt uw app om te beginnen met PM2, of met NPM, of met een aangepaste opdracht configureren.

- [Aangepaste opdracht uitvoeren](#run-custom-command)
- [Uitvoering starten npm](#run-npm-start)
- [Uitvoeren met PM2](#run-with-pm2)

### <a name="run-custom-command"></a>Aangepaste opdracht uitvoeren

App Service kan uw app met behulp van een aangepaste opdracht te starten, zoals een uitvoerbaar bestand, zoals *run.sh*. Bijvoorbeeld, om uit te voeren `npm run start:prod`, voer de volgende opdracht uit de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "npm run start:prod"
```

### <a name="run-npm-start"></a>Uitvoering starten npm

Om te beginnen met uw app met `npm start`, zorg er wel een `start` script is in de *package.json* bestand. Bijvoorbeeld:

```json
{
  ...
  "scripts": {
    "start": "gulp",
    ...
  },
  ...
}
```

Gebruik een aangepaste *package.json* in uw project, voert u de volgende opdracht uit de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filename>.json"
```

### <a name="run-with-pm2"></a>Uitvoeren met PM2

Uw app in de container automatisch gestart met PM2 wanneer een van de algemene Node.js-bestanden wordt gevonden in uw project:

- *bin/www*
- *server.js*
- *app.js*
- *index.js*
- *hostingstart.js*
- Een van de volgende [PM2 bestanden](http://pm2.keymetrics.io/docs/usage/application-declaration/#process-file): *process.json* en *ecosystem.config.js*

U kunt ook een aangepaste opstartbestand configureren met de volgende extensies:

- Een *.js* bestand
- Een [PM2 bestand](http://pm2.keymetrics.io/docs/usage/application-declaration/#process-file) met de extensie *.json*, *. config.js*, *yaml*, of *.yml*

Als u wilt toevoegen van een aangepaste start-bestand, kunt u de volgende opdracht uitvoeren de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filname-with-extension>"
```

## <a name="debug-remotely"></a>Foutopsporing op afstand

> [!NOTE]
> Foutopsporing op afstand is momenteel in Preview.

U kunt fouten opsporen in uw Node.js-app op afstand in [Visual Studio Code](https://code.visualstudio.com/) als u deze configureert [uitvoeren met PM2](#run-with-pm2), behalve wanneer u uitvoert met behulp van een *. config.js, *.yml, of *yaml*.

In de meeste gevallen is er geen extra configuratie vereist voor uw app. Als uw app wordt uitgevoerd met een *process.json* bestand (standaard of aangepast), moet er een `script` eigenschap in de JSON-hoofdmap. Bijvoorbeeld:

```json
{
  "name"        : "worker",
  "script"      : "./index.js",
  ...
}
```

Als u Visual Studio Code instelt voor foutopsporing op afstand, installatie van de [App Service-extensie](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Volg de instructies op de pagina met extensies en aanmelden bij Azure in Visual Studio Code.

Zoek in de Azure explorer, de fouten opsporen, met de rechtermuisknop en selecteer de gewenste app **externe foutopsporing starten**. Klik op **Ja** inschakelt voor uw app. App Service een tunnel-proxy voor u begint en koppelt u het foutopsporingsprogramma. Vervolgens kunt u aanvragen versturen naar de app en ziet het foutopsporingsprogramma onderbreken op onderbreking.

Wanneer u klaar bent met het opsporen van fouten, het foutopsporingsprogramma stopt met het selecteren van **verbinding verbreken**. Wanneer u hierom wordt gevraagd, klikt u op **Ja** om uit te schakelen foutopsporing op afstand. Als u wilt deze later uitschakelen met de rechtermuisknop op uw app opnieuw in de Azure explorer en selecteer **externe foutopsporing uitschakelen**.

## <a name="access-environment-variables"></a>Toegang tot omgevingsvariabelen

In App Service, kunt u [app-instellingen](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#app-settings) buiten de app-code. Vervolgens kunt u ze met het standaardpatroon voor Node.js openen. Voor toegang tot bijvoorbeeld de app-instelling `NODE_ENV` gebruikt u de volgende code:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Bower-Grunt/Gulp uitvoeren

Kudu wordt standaard uitgevoerd `npm install --production` wordt gedetecteerd wanneer een Node.js-app wordt geïmplementeerd. Als uw app nodig een van de populaire hulpprogramma's voor werkstroomautomatisering zoals Grunt, Bower of Gulp heeft, moet u om op te geven een [aangepaste implementatiescript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) uit te voeren.

Als u wilt uw opslag om uit te voeren van deze hulpprogramma's inschakelen, moet u toe te voegen aan de afhankelijkheden in *package.json.* Bijvoorbeeld:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Wijzig de directory naar de hoofdmap van uw opslagplaats en voer de volgende opdrachten uit vanuit een lokale terminalvenster:

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

Hoofdmap van uw opslagplaats bevat nu twee extra bestanden: *.deployment* en *deploy.sh*.

Open *deploy.sh* en zoek de `Deployment` sectie, welke ziet er als volgt:

```bash
##################################################################################################################################
# Deployment
# ----------
```

In deze sectie wordt beëindigd met uitgevoerd `npm install --production`. Voeg de sectie met voorbeeldcode moet u de vereiste hulpprogramma uitvoeren *aan het einde* van de `Deployment` sectie:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Zie een [voorbeeld in het voorbeeld MEAN.js](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135), waarop het script voor implementatie ook wordt uitgevoerd een aangepaste `npm install` opdracht.

### <a name="bower"></a>Bower

Dit fragment wordt uitgevoerd `bower install`.

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Dit fragment wordt uitgevoerd `gulp imagemin`.

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Dit fragment wordt uitgevoerd `grunt`.

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>HTTPS-sessie detecteren

In App Service vindt [SSL-beëindiging](https://wikipedia.org/wiki/TLS_termination_proxy) plaats in de load balancers voor het netwerk, zodat alle HTTPS-aanvragen uw app bereiken als niet-versleutelde HTTP-aanvragen. Inspecteer de header `X-Forwarded-Proto` als de app-logica moet controleren of de aanvragen van gebruikers al dan niet zijn versleuteld.

Populaire webframeworks bieden toegang tot de `X-Forwarded-*`-informatie in het patroon van de standaard-app. In [Express](https://expressjs.com/), kunt u [proxy's vertrouwen](http://expressjs.com/guide/behind-proxies.html). Bijvoorbeeld:

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Toegang tot diagnostische logboeken

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH-sessie openen in browser

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer een werkende Node.js-app zich anders in App Service gedraagt of fouten heeft, probeert u het volgende:

- [Toegang tot de logboekstream](#access-diagnostic-logs).
- De app lokaal testen in productiemodus. App Service voert uw Node.js-apps in de productiemodus voor, dus u ervoor zorgen moet dat uw project werkt zoals verwacht in productiemodus lokaal. Bijvoorbeeld:
    - Afhankelijk van uw *package.json*, verschillende pakketten kunnen worden geïnstalleerd voor de productiemodus (`dependencies` versus `devDependencies`).
    - Bepaalde web-frameworks kunnen statische bestanden in productiemodus anders implementeren.
    - Bepaalde web-frameworks kunnen aangepaste opstartscripts gebruiken bij het uitvoeren in productiemodus.
- Het uitvoeren van uw app in App Service in de Ontwikkelingsmodus. Bijvoorbeeld, in [MEAN.js](http://meanjs.org/), kunt u uw app instellen op Ontwikkelingsmodus in runtime door [instelling de `NODE_ENV` app-instelling](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Node.js-app met MongoDB](tutorial-nodejs-mongodb-app.md)

> [!div class="nextstepaction"]
> [App Service Linux Veelgestelde vragen](app-service-linux-faq.md)