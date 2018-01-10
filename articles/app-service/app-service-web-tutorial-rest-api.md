---
title: Node.js-API-app in Azure App Service | Microsoft Docs
description: Informatie over het maken van een Node.js-RESTful-API en deze implementeren in een API-app in Azure App Service.
services: app-service\api
documentationcenter: node
author: bradygaster
manager: erikre
editor: 
ms.assetid: a820e400-06af-4852-8627-12b3db4a8e70
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/13/2017
ms.author: rachelap
ms.custom: mvc, devcenter
ms.openlocfilehash: 81d08e047a3689d110195f2325b52c6c0457e644
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Een Node.js-RESTful-API maken en deze implementeren in een API-app in Azure

In deze snelstartgids wordt uitgelegd hoe u een REST-API, geschreven in Node.js [Express](http://expressjs.com/), maakt met behulp van een [Swagger](http://swagger.io/)-definitie en deze implementeert in Azure. U maakt de app met opdrachtregelprogramma's, configureert resources met de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) en implementeert de app met Git.  Wanneer u klaar bent, hebt u een werkende voorbeeld-REST API die wordt uitgevoerd op Azure.

## <a name="prerequisites"></a>Vereisten

* [Git](https://git-scm.com/)
* [Node.js en NPM](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit onderwerp gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="prepare-your-environment"></a>Uw omgeving voorbereiden

1. Voer in een terminalvenster de volgende opdracht uit om het voorbeeld naar uw lokale machine te klonen.

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. Ga naar de map die de voorbeeldcode bevat.

    ```bash
    cd app-service-api-node-contact-list
    ```

3. Installeer [Swaggerize](https://www.npmjs.com/package/swaggerize-express) op uw lokale machine. Swaggerize is een hulpprogramma dat Node.js-code genereert voor uw REST API van een Swagger-definitie.

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a>Node.js-code genereren 

In deze sectie van de zelfstudie wordt een API-ontwikkelingswerkstroom getoond waarin u eerst Swagger-metagegevens maakt en deze vervolgens gebruikt om automatisch servercode voor de API te genereren. 

Wijzig de map naar de map *start* en voer `yo swaggerize` uit. Swaggerize maakt een Node.js-project voor uw API van de Swagger-definitie in *api.json*.

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

Wanneer in Swaggerize om een projectnaam wordt gevraagd, gebruikt u *ContactList*.
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like to call this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-the-project-code"></a>De projectcode aanpassen

1. Kopieer de map *lib* naar de map *ContactList* die door `yo swaggerize` is gemaakt en wijzig de map naar *ContactList*.

    ```bash
    cp -r lib ContactList/
    cd ContactList
    ```

2. Installeer de NPM-modules `jsonpath` en `swaggerize-ui`. 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. Vervang de code in het bestand *handlers/contacts.js* door de volgende code: 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    Deze code gebruikt de JSON-gegevens die zijn opgeslagen in het bestand *lib/contacts.json* dat gebruikmaakt van *lib/contactRepository.js*. De nieuwe code *contacts.js* retourneert alle contactpersonen in de opslagplaats als JSON-nettolading. 

4. Vervang de code in het bestand **handlers/contacts/{id}.js** door de volgende code:

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    Met deze code kunt u een padvariabele gebruiken om alleen de contactpersoon met een bepaalde id te retourneren.

5. Vervang de code in **server.js** door de volgende code:

    ```javascript
    'use strict';

    var port = process.env.PORT || 8000; 

    var http = require('http');
    var express = require('express');
    var bodyParser = require('body-parser');
    var swaggerize = require('swaggerize-express');
    var swaggerUi = require('swaggerize-ui'); 
    var path = require('path');
    var fs = require("fs");
    
    fs.existsSync = fs.existsSync || require('path').existsSync;

    var app = express();

    var server = http.createServer(app);

    app.use(bodyParser.json());

    app.use(swaggerize({
        api: path.resolve('./config/swagger.json'),
        handlers: path.resolve('./handlers'),
        docspath: '/swagger' 
    }));

    // change four
    app.use('/docs', swaggerUi({
        docs: '/swagger'  
    }));

    server.listen(port, function () { 
    });
    ```   

    Deze code brengt een aantal kleine wijzigingen aan zodat deze werkt met Azure App Service en maakt een interactieve webinterface voor uw API beschikbaar.

### <a name="test-the-api-locally"></a>De API lokaal testen

1. De Node.js-app starten
    ```bash
    npm start
    ```
    
2. Blader naar http://localhost:8000/contacts om de JSON voor de volledige lijst met contactpersonen weer te geven.
   
   ```json
    {
        "id": 1,
        "name": "Barney Poland",
        "email": "barney@contoso.com"
    },
    {
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    },
    {
        "id": 3,
        "name": "Lora Riggs",
        "email": "lora@contoso.com"
    }
   ```

3. Blader naar http://localhost:8000/contacts/2 om de contactpersoon met een `id` van twee weer te geven.
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. Test de API met behulp van de Swagger-webinterface op http://localhost:8000/docs.
   
    ![Swagger-webinterface](media/app-service-web-tutorial-rest-api/swagger-ui.png)

## <a id="createapiapp"></a> Een API-app maken

In deze sectie kunt u de Azure CLI 2.0 gebruiken om de resources te maken om de API op Azure App Service te hosten. 

1.  Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/?view=azure-cli-latest#az_login) en volg de instructies op het scherm.

    ```azurecli-interactive
    az login
    ```

2. Als u meerdere Azure-abonnementen hebt, wijzigt u het standaardabonnement naar het gewenste abonnement.

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-the-api-with-git"></a>De API implementeren met Git

Implementeer uw code in de API-app door doorvoeracties van uw lokale Git-opslagplaats naar Azure App Service te pushen.

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. Initialiseer een nieuwe opslagplaats in de map *ContactList*. 

    ```bash
    git init .
    ```

3. Sluit de map *node_modules* uit die via NPM is gemaakt in een eerdere stap in de zelfstudie over Git. Maak een nieuw `.gitignore`-bestand in de huidige map en voeg ergens in het bestand de volgende tekst toe op een nieuwe regel.

    ```
    node_modules/
    ```
    Bevestig met `git status` dat de `node_modules`-map wordt genegeerd.
    
4. Voeg de volgende regels in `package.json`. De code die wordt gegenereerd door Swaggerize opgeven niet een versie op voor de Node.js-engine. Zonder de specificatie versie gebruikt Azure de standaardversie van `0.10.18`, die niet compatibel is met de gegenereerde code.

    ```javascript
    "engines": {
        "node": "~0.10.22"
    },
    ```

5. Voer de wijzigingen door in de map.
    ```bash
    git add .
    git commit -m "initial version"
    ```

6. [!INCLUDE [Push to Azure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-the-api--in-azure"></a>De API testen in Azure

1. Open http://app_name.azurewebsites.net/contacts in een browservenster. Dezelfde JSON wordt geretourneerd als toen u de aanvraag eerder in de zelfstudie lokaal indiende.

   ```json
   {
       "id": 1,
       "name": "Barney Poland",
       "email": "barney@contoso.com"
   },
   {
       "id": 2,
       "name": "Lacy Barrera",
       "email": "lacy@contoso.com"
   },
   {
       "id": 3,
       "name": "Lora Riggs",
       "email": "lora@contoso.com"
   }
   ```

2. Ga in een browser naar het eindpunt `http://app_name.azurewebsites.net/docs` om de Swagger-UI uit te proberen die in Azure wordt uitgevoerd.

    ![Swagger-UI](media/app-service-web-tutorial-rest-api/swagger-azure-ui.png)

    U kunt nu updates voor de voorbeeld-API implementeren in Azure door doorvoeracties naar de Azure Git-opslagplaats te pushen.

## <a name="clean-up"></a>Opruimen

Voer de volgende Azure CLI-opdracht uit als u alle resources wilt verwijderen die door deze snelstartgids zijn gemaakt:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a>Volgende stap 
> [!div class="nextstepaction"]
> [Een bestaande aangepaste DNS-naam toewijzen aan Azure Web Apps](app-service-web-tutorial-custom-domain.md)

