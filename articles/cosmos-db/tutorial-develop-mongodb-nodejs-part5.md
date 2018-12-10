---
title: Zelfstudie voor MongoDB, Angular en Node voor Azure - deel 5
description: Deel 5 van de serie zelfstudies voor het maken van een MongoDB-app met Angular en Node op Azure Cosmos DB, waarbij gebruik wordt gemaakt van dezelfde API's als voor MongoDB
services: cosmos-db
author: johnpapa
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/05/2017
ms.author: jopapa
ms.custom: mvc
ms.openlocfilehash: ce4ad1c069d34e79c81b8335bf2d58cb95068b9a
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52866173"
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-5-use-mongoose-to-connect-to-azure-cosmos-db"></a>Een MongoDB-app maken met Angular en Azure Cosmos DB - deel 5: Mongoose gebruiken om verbinding te maken met Azure Cosmos DB

In deze meerdelige zelfstudie leert u hoe u een nieuwe Node.js-app maakt met Express, Angular en een verbinding maakt tussen de app en een [Azure Cosmos DB MongoDB API](mongodb-introduction.md)-account.

Deel 5 van de zelfstudie bouwt voort op [deel 4](tutorial-develop-mongodb-nodejs-part4.md) en beschrijft de volgende taken:

> [!div class="checklist"]
> * Mongoose gebruiken om verbinding te maken met Azure Cosmos DB
> * Cosmos DB-verbindingsreeksgegevens ophalen
> * Het hero-model maken
> * De hero-service maken voor het ophalen van hero-gegevens
> * De app lokaal uitvoeren

## <a name="video-walkthrough"></a>Video-overzicht

Bekijk de volgende video om snel de stappen te leren die worden beschreven in dit document: 

> [!VIDEO https://www.youtube.com/embed/sI5hw6KPPXI]


## <a name="prerequisites"></a>Vereisten

Voltooi de stappen in [deel 4](tutorial-develop-mongodb-nodejs-part4.md) van de zelfstudie voordat u aan dit deel begint.

> [!TIP]
> Aan de hand van deze zelfstudie wordt u stapsgewijs begeleid bij het maken van de toepassing. Als u het voltooide project wilt downloaden, kunt u de voltooide app vinden in de [angular-cosmosdb-opslagplaats](https://github.com/Azure-Samples/angular-cosmosdb) op GitHub.

## <a name="use-mongoose-to-connect-to-azure-cosmos-db"></a>Mongoose gebruiken om verbinding te maken met Azure Cosmos DB

1. Installeer de module Mongoose npm, een API die wordt gebruikt om te praten met MongoDB.

    ```bash
    npm i mongoose --save
    ```

2. Maak nu een nieuw bestand met de naam **mongo.js** in de map **server**. U gaat de verbindingsgegevens van uw Cosmos DB-account toevoegen aan dit bestand.

3. Kopieer de volgende code in **mongo.js**. Deze code:

    * Vereist Mongoose.

    * Overschrijft de Mongo-promise om de basis-promise te gebruiken die is ingebouwd in ES6/ES2015 en een hoger versie.

    * Roept een env-bestand aan waarmee u bepaalde dingen kunt instellen op basis van de fase waarin u zich bevindt: fasering, productie of ontwikkeling. U maakt dit bestand in de volgende sectie.

    * Neem de MongoDB-verbindingsreeks op, die wordt ingesteld in het env-bestand.

    * Maakt een verbindingsfunctie die Mongoose aanroept.

    ```javascript
    const mongoose = require('mongoose');
    /**
     * Set to Node.js native promises
     * Per http://mongoosejs.com/docs/promises.html
     */
    mongoose.Promise = global.Promise;

    const env = require('./env/environment');

    // eslint-disable-next-line max-len
    const mongoUri = `mongodb://${env.accountName}:${env.key}@${env.accountName}.documents.azure.com:${env.port}/${env.databaseName}?ssl=true`;

    function connect() {
     mongoose.set('debug', true);
     return mongoose.connect(mongoUri, { useMongoClient: true });
    }

    module.exports = {
      connect,
      mongoose
    };
    ```
    
4. Maak in het deelvenster Explorer, onder **server**, een map met de naam **environment** en maak in de map **environment** een nieuw bestand met de naam **environment.js**.

5. Volgens het bestand mongo.js weten we dat we `dbName`, `key` en `cosmosPort` moeten opnemen, dus kopieer de volgende code in **environment.js**.

    ```javascript
    // TODO: replace if yours are different
    module.exports = {
      accountName: 'your-cosmosdb-account-name-goes-here',
      databaseName: 'admin', 
      key: 'your-key-goes-here',
      port: 10255
    };
    ```

## <a name="get-the-connection-string-information"></a>Verbindingsreeksgegevens ophalen

1. Wijzig in **environment.js** de waarde van `port` in 10255. (U vindt de Cosmos DB-poort in de Azure-portal)

    ```javascript
    const port = 10255;
    ```

2. Wijzig in **environment.js** de waarde van `accountName` in de naam van het Azure Cosmos DB-account dat u in [stap 4](tutorial-develop-mongodb-nodejs-part4.md) hebt gemaakt. 

3. Haal de primaire sleutel voor het Azure Cosmos DB-account op via de volgende CLI-opdracht in het terminalvenster: 

    ```azure-cli-interactive
    az cosmosdb list-keys --name <cosmosdb-name> -g myResourceGroup
    ```    
    
    * `<cosmosdb-name>` is de naam van het Cosmos Azure DB-account dat u hebt gemaakt in [stap 4](tutorial-develop-mongodb-nodejs-part4.md).

4. Kopieer de primaire sleutel in het bestand environment.js als de `key`-waarde.

    De app heeft nu alle gegevens die nodig zijn om verbinding te maken met Azure Cosmos DB. Deze informatie kan ook worden opgehaald in Azure Portal. Zie [Get the MongoDB connection string to customize](connect-mongodb-account.md#GetCustomConnection) (MongoDB-verbindingsreeks ophalen om aan te passen) voor meer informatie. De gebruikersnaam in Azure Portal is gelijk aan dbName in environments.js. 

## <a name="create-a-hero-model"></a>Een Hero-model maken

1. Maak in het deelvenster Explorer het bestand **hero.model.js** onder de map **server**.

2. Kopieer de volgende code in **hero,model.js**. Deze code biedt de volgende functionaliteit:

   * Vereist Mongoose.
   * Maakt een nieuw schema met een id, een naam en een uitspraak.
   * Maakt een model aan de hand van het schema.
   * Exporteert het model. 
   * Noem de verzameling Heroes (in plaats van Hero's, wat de standaardnaam van de verzameling is op basis van de regels voor namen in meervoud in Mongoose).

   ```javascript
   const mongoose = require('mongoose');

   const Schema = mongoose.Schema;

   const heroSchema = new Schema(
     {
       id: { type: Number, required: true, unique: true },
       name: String,
       saying: String
     },
     {
       collection: 'Heroes'
     }
   );

   const Hero = mongoose.model('Hero', heroSchema);

   module.exports = Hero;
   ```

## <a name="create-a-hero-service"></a>Een Hero-service maken

1. Maak in het deelvenster Explorer het bestand **hero.service.js** onder de map **server**.

2. Kopieer de volgende code in **hero.service.js**. Deze code:

   * Haalt het zojuist gemaakte model op
   * Maakt verbinding met de database
   * Maakt een docquery-variabele die de methode hero.find gebruikt voor het definiëren van een query waarmee alle hero's worden geretourneerd.
   * Voert een query uit met docquery.exec met een promise een lijst met alle hero's op te halen, waarvan de responsstatus 200 is. 
   * Stuurt het foutbericht terug als de status 500 is
   * De hero's worden opgehaald omdat we modules gebruiken. 

   ```javascript
   const Hero = require('./hero.model');

   require('./mongo').connect();

   function getHeroes() {
     const docquery = Hero.find({});
     docquery
       .exec()
       .then(heroes => {
         res.status(200).json(heroes);
       })
       .catch(error => {
         res.status(500).send(error);
         return;
       });
   }

   module.exports = {
     getHeroes
   };
   ```

## <a name="add-the-hero-service-to-routesjs"></a>De hero-service toevoegen aan routes.js

1. In Visual Studio Code, commentarieert u in **routes.js** de functie `res.send` uit waarmee de hero-voorbeeldgegevens worden verzonden en voegt u in plaats daarvan een regel toe om de functie `heroService.getHeroes` aan te roepen.

    ```javascript
    router.get('/heroes', (req, res) => {
      heroService.getHeroes(req, res);
    //  res.send(200, [
    //      {"id": 10, "name": "Starlord", "saying": "oh yeah"}
    //  ])
    });
    ```

2. In **routes.js** moet u de hero-service gebruiken:

    ```javascript
    const heroService = require('./hero.service'); 
    ```

3. Werk in **hero.service.js** de functie getHeroes bij, zodat de parameters `req` en `res` als volgt worden opgenomen:

    ```javascript
    function getHeroes(req, res) {
    ```

    We staan nu even stil bij de aanroepreeks. Eerst komen we terecht bij `index.js`, waarmee de knooppuntserver wordt ingesteld. Merk op dat hiermee de routes worden ingesteld en gedefinieerd. Het bestand routes.js communiceert vervolgens met de hero-service. Deze krijgt de opdracht functies op te halen, bijvoorbeeld getHeroes, en de aanvraag en respons door te geven. Hier pakt hero.service.js het model en maakt verbinding met Mongo. Vervolgens wordt getHeroes uitgevoerd als het wordt aangeroepen en geeft een respons terug van 200. Hierna verloopt het via de keten. 

## <a name="run-the-app"></a>De app uitvoeren

1. We gaan nu de app opnieuw uitvoeren. Sla uw wijzigingen op in Visual Studio Code, klik aan de linkerkant op de knop **Debug** ![pictogram Debug in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part5/debug-button.png) en klik vervolgens op de knop **Start Debugging** ![pictogram Debug in Visual Studio Code](./media/tutorial-develop-mongodb-nodejs-part5/start-debugging-button.png).

3. Open in de browser ontwikkelhulpprogramma's en het tabblad Netwerk, en ga naar http://localhost:3000. Hier ziet u de toepassing.

    ![Nieuw Azure Cosmos DB-account in Azure Portal](./media/tutorial-develop-mongodb-nodejs-part5/azure-cosmos-db-heroes-app.png)

   In de app zijn nog geen hero's opgeslagen, maar in de volgende stap van de zelfstudie gaan we de functies Put, Push en Delete toevoegen, zodat we hero's vanuit de gebruikersinterface kunnen toevoegen en bijwerken of eruit verwijderen met behulp van Mongoose-verbindingen met de Azure Cosmos DB-database. 

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u de volgende taken uitgevoerd:

> [!div class="checklist"]
> * Mongoose API's gebruikt om de hero’s-app verbinding te laten maken met Azure Cosmos DB 
> * De functionaliteit getHeroes aan de app toegevoegd

U kunt doorgaan met het volgende deel van de zelfstudie om de functies Post, Put en Delete aan de app toe te voegen.

> [!div class="nextstepaction"]
> [De functies Post, Put en Delete aan de app toevoegen](tutorial-develop-mongodb-nodejs-part6.md)
