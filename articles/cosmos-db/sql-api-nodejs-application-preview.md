---
title: Een Node.js-web-app maken voor Azure Cosmos DB | Microsoft Docs
description: In deze zelfstudie over Node.js wordt uitgelegd hoe u Microsoft Azure Cosmos DB gebruikt voor het opslaan van en de toegang tot gegevens van een Node.js Express-webtoepassing die wordt gehost op Azure Websites.
keywords: Ontwikkelen van toepassingen, zelfstudie over databases, Node.js leren, zelfstudie voor Node.js
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 07/31/2018
ms.author: sngun
ms.openlocfilehash: 9f7744c7743107b3587bd63a6e7681a6f1e42c00
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39608967"
---
# <a name="_Toc395783175"></a>Een Node.js-webtoepassing bouwen met Azure Cosmos DB en Node.js SDK (preview)
> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Node.js - v2.0 Preview](sql-api-nodejs-application-preview.md)
> * [Java](sql-api-java-application.md)
> * [Python](sql-api-python-application.md)
> 
> 

In deze zelfstudie over Node.js wordt uitgelegd hoe u met een Azure Cosmos DB SQL API-account toegang kunt krijgen tot gegevens uit een Node.js Express-toepassing die wordt gehost op Azure Websites en hoe u deze gegevens kunt opslaan. In deze zelfstudie bouwt u een eenvoudige webtoepassing (Todo-app) waarmee u taken kunt maken, ophalen en voltooien. De taken worden opgeslagen als JSON-documenten in Azure Cosmos DB. De volgende afbeelding toont een schermafbeelding van de Todo-toepassing:

![Schermopname van de toepassing My Todo List die in deze zelfstudie voor Node.js wordt gemaakt](./media/sql-api-nodejs-application/cosmos-db-node-js-mytodo.png)

Deze zelfstudie laat zien hoe u een Azure Cosmos DB SQL API-account kunt maken met behulp van de Azure-portal. Vervolgens bouwt u een web-app die op de Node.js SDK is gebouwd om een database en container te maken en items aan de container toe te voegen. Daarna kunt u de web-app uitvoeren. In deze zelfstudie wordt gebruikgemaakt van versie 2.0 van de Node.js SDK, momenteel in preview.

U kunt het volledige voorbeeld ook downloaden op [GitHub][GitHub]. Lees het [Leesmij](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md)-bestand voor instructies over het uitvoeren van de app.

## <a name="_Toc395783176"></a>Vereisten

Voordat u de instructies in dit artikel uitvoert, moet u beschikken over het volgende:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js][Node.js], versie 6.10 of hoger.
* [Express generator](http://www.expressjs.com/starter/generator.html) (te installeren via `npm install express-generator -g`)
* [Git][Git].

## <a name="_Toc395637761"></a>Stap 1: een Azure Cosmos DB-databaseaccount maken
Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt of de Azure Cosmos DB-emulator gebruikt voor deze zelfstudie, kunt u direct doorgaan naar [Stap 2: een nieuwe Node.js-toepassing maken](#_Toc395783178).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>Stap 2: een nieuwe Node.js-toepassing maken
Laten we eens kijken hoe u het [Express](http://expressjs.com/)-framework gebruikt om een eenvoudig Hello World Node.js-project te maken.

1. Open een terminal waarmee u graag werkt, zoals de Node.js-opdrachtprompt.
2. Navigeer naar de map waarin u de nieuwe toepassing wilt opslaan.
3. Gebruik de express-generator om een toepassing met de naam **todo** te maken.

   ```bash
   express todo
   ```
4. Open de nieuwe map **todo** en installeer afhankelijkheden.

   ```bash
   cd todo
   npm install
   ```
5. Voer uw nieuwe toepassing uit.

   ```bash
   npm start
   ```

6. U kunt uw nieuwe toepassing bekijken door in uw browser naar [http://localhost:3000](http://localhost:3000) te gaan.
   
    ![Node.js leren - Schermopname van de toepassing Hello World in een browservenster](./media/sql-api-nodejs-application/cosmos-db-node-js-express.png)

 Stop de toepassing door middel van Ctrl+C in het terminalvenster en klik op **v** om de batchtaak te beëindigen.

## <a name="_Toc395783179"></a>Stap 3: de vereiste modules installeren

Het bestand **package.json** is een van de bestanden die zijn gemaakt in de hoofdmap van het project. Dit bestand bevat een lijst met aanvullende modules die u nodig hebt voor uw Node.js-toepassing. Later, wanneer u deze toepassing implementeert op Azure Websites, wordt dit bestand gebruikt om te bepalen welke modules ter ondersteuning van uw toepassing in Azure moeten worden geïnstalleerd. Voor deze zelfstudie moet u twee extra pakketten installeren.

1. Open de terminal en installeer de **async**-module via npm.

   ```bash
   npm install async --save
   ```

2. Installeer de **@azure/cosmos**-module via npm. 

   ```bash
   npm install @azure/cosmos
   ```

## <a name="_Toc395783180"></a>Stap 4: de Azure Cosmos DB-service in een Node-toepassing gebruiken
U hebt nu de initiële setup en configuratie voltooid. U gaat nu code schrijven die voor de Todo-toepassing is vereist om met Azure Cosmos DB te kunnen communiceren.

### <a name="create-the-model"></a>Het model maken
1. Maak in de hoofdmap van uw projectmap een nieuwe map met de naam **models**  

2. Maak in de map **models** een nieuw bestand met de naam **taskDao.js**. Dit bestand bevat code die vereist is voor het maken van de database en de container en definieert methoden voor het lezen, bijwerken, maken en zoeken van taken in Azure Cosmos DB. 

3. Kopieer de volgende code naar het bestand **taskDao.js**

   ```nodejs
   // @ts-check
   const CosmosClient = require("@azure/cosmos").CosmosClient;
   const debug = require("debug")("todo:taskDao");
   class TaskDao {
     /**
      * Manages reading, adding, and updating Tasks in Cosmos DB
      * @param {CosmosClient} cosmosClient
      * @param {string} databaseId
      * @param {string} containerId
      */
     constructor(cosmosClient, databaseId, containerId) {
       this.client = cosmosClient;
       this.databaseId = databaseId;
       this.collectionId = containerId;

       this.database = null;
       this.container = null;
     }

     async init() {
       debug("Setting up the database...");
       const dbResponse = await this.client.databases.createIfNotExists({
         id: this.databaseId
       });
       this.database = dbResponse.database;
       debug("Setting up the database...done!");
       debug("Setting up the container...");
       const coResponse = await this.database.containers.createIfNotExists({
         id: this.collectionId
       });
       this.container = coResponse.container;
       debug("Setting up the container...done!");
     }

     async find(querySpec) {
       debug("Querying for items from the database");
       if (!this.container) {
         throw new Error("Collection is not initialized.");
       }
       const { result: results } = await this.container.items
        .query(querySpec)
        .toArray();
      return results;
    }

    async addItem(item) {
      debug("Adding an item to the database");
      item.date = Date.now();
      item.completed = false;
      const { body: doc } = await this.container.items.create(item);
      return doc;
    }

    async updateItem(itemId) {
      debug("Update an item in the database");
      const doc = await this.getItem(itemId);
      doc.completed = true;

      const { body: replaced } = await this.container.item(itemId).replace(doc);
      return replaced;
    }

    async getItem(itemId) {
      debug("Getting an item from the database");
      const { body } = await this.container.item(itemId).read();
      return body;
    }
  }

   module.exports = TaskDao;
   ```
4. Sla het bestand **taskDao.js** op en sluit het bestand.  

### <a name="create-the-controller"></a>De controller maken

1. Maak in de map **routes** van uw project een nieuw bestand met de naam **tasklist.js**.  

2. Voeg de volgende code toe aan het bestand **tasklist.js**. Hiermee worden de CosmosClient- en async-modules geladen die door **tasklist.js** worden gebruikt. Met deze code wordt ook de klasse **TaskList** gedefinieerd, die als een exemplaar van het eerder gedefinieerde object **TaskDao** wordt doorgegeven:
   
   ```nodejs
   const TaskDao = require("../models/TaskDao");

   class TaskList {
     /**
      * Handles the various APIs for displaying and managing tasks
      * @param {TaskDao} taskDao
     */
    constructor(taskDao) {
    this.taskDao = taskDao;
    }
    async showTasks(req, res) {
      const querySpec = {
        query: "SELECT * FROM root r WHERE r.completed=@completed",
        parameters: [
          {
            name: "@completed",
            value: false
          }
        ]
      };

      const items = await this.taskDao.find(querySpec);
      res.render("index", {
        title: "My ToDo List ",
        tasks: items
      });
    }

    async addTask(req, res) {
      const item = req.body;

      await this.taskDao.addItem(item);
      res.redirect("/");
    }

    async completeTask(req, res) {
      const completedTasks = Object.keys(req.body);
      const tasks = [];

      completedTasks.forEach(task => {
        tasks.push(this.taskDao.updateItem(task));
      });

      await Promise.all(tasks);

      res.redirect("/");
    }
  }

  module.exports = TaskList;
   ```

3. Sla het bestand **tasklist.js** op en sluit het bestand.

### <a name="add-configjs"></a>Config.js toevoegen

1. Maak in de hoofdmap van uw projectmap een nieuw bestand met de naam **config.js**. 

2. Voeg de volgende code toe aan het bestand **config.js**. Met deze code definieert u configuratie-instellingen en waarden die voor de toepassing zijn vereist.
   
   ```nodejs
   const config = {};

   config.host = process.env.HOST || "[the endpoint URI of your Azure Cosmos DB account]";
   config.authKey =
     process.env.AUTH_KEY || "[the PRIMARY KEY value of your Azure Cosmos DB account";
   config.databaseId = "ToDoList";
   config.containerId = "Items";

   if (config.host.includes("https://localhost:")) {
     console.log("Local environment detected");
     console.log("WARNING: Disabled checking of self-signed certs. Do not have this code in production.");
     process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";
     console.log(`Go to http://localhost:${process.env.PORT || '3000'} to try the sample.`);
   }

   module.exports = config;
   ```

3. Werk in het bestand **config.js** de waarden voor HOST en AUTH_KEY bij met de waarden op de pagina Sleutels van uw Azure Cosmos DB-account in [Microsoft Azure Portal](https://portal.azure.com). 

4. Sla het bestand **config.js** op en sluit het bestand.

### <a name="modify-appjs"></a>App.js wijzigen
1. Ga naar de projectmap en open het bestand **app.js**. Dit bestand is gemaakt toen de Express-webtoepassing werd gemaakt.  

2. Voeg de volgende code toe aan het bestand **app.js**. Deze code definieert het configuratiebestand dat moet worden gebruikt en leest waarden uit dit bestand naar enkele variabelen die we zo dadelijk gaan gebruiken. 
   
   ```nodejs
   const CosmosClient = require("@azure/cosmos").CosmosClient;
   const config = require("./config");
   const TaskList = require("./routes/tasklist");
   const TaskDao = require("./models/taskDao");

   const express = require("express");
   const path = require("path");
   const logger = require("morgan");
   const cookieParser = require("cookie-parser");
   const bodyParser = require("body-parser");

   const app = express();

   // view engine setup
   app.set("views", path.join(__dirname, "views"));
   app.set("view engine", "jade");

   // uncomment after placing your favicon in /public
   //app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
   app.use(logger("dev"));
   app.use(bodyParser.json());
   app.use(bodyParser.urlencoded({ extended: false }));
   app.use(cookieParser());
   app.use(express.static(path.join(__dirname, "public")));

   //Todo App:
   const cosmosClient = new CosmosClient({
     endpoint: config.host,
     auth: {
       masterKey: config.authKey
     }
   });
   const taskDao = new TaskDao(cosmosClient, config.databaseId, config.containerId);
   const taskList = new TaskList(taskDao);
   taskDao
     .init(err => {
       console.error(err);
     })
     .catch(err => {
       console.error(err);
       console.error("Shutting down because there was an error settinig up the database.");
       process.exit(1);
     });

   app.get("/", (req, res, next) => taskList.showTasks(req, res).catch(next));
   app.post("/addtask", (req, res, next) => taskList.addTask(req, res).catch(next));
   app.post("/completetask", (req, res, next) => taskList.completeTask(req, res).catch(next));
   app.set("view engine", "jade");

   // catch 404 and forward to error handler
   app.use(function(req, res, next) {
     const err = new Error("Not Found");
     err.status = 404;
     next(err);
   });

   // error handler
   app.use(function(err, req, res, next) {
     // set locals, only providing error in development
     res.locals.message = err.message;
     res.locals.error = req.app.get("env") === "development" ? err : {};

     // render the error page
     res.status(err.status || 500);
     res.render("error");
   });

   module.exports = app;
   ```

3. Tot slot slaat u het bestand **app.js** op en sluit u het bestand.

## <a name="_Toc395783181"></a>Stap 5: Een gebruikersinterface maken
U kunt zich nu concentreren op het bouwen van de gebruikersinterface, zodat gebruikers kunnen communiceren met de toepassing. De Express-toepassing die is gemaakt, gebruikt **Jade** als weergave-engine. Raadpleeg [http://jade-lang.com/](http://jade-lang.com/) voor meer informatie over Jade.

1. Het bestand **layout.jade** in de map **views** wordt gebruikt als een algemeen sjabloon voor andere **.jade**-bestanden. In deze stap wordt het bestand aangepast voor het gebruik van [Twitter Bootstrap](https://github.com/twbs/bootstrap). Dit is een werkset waarmee u eenvoudig een aantrekkelijk ogende website kunt maken.  

2. Open het bestand **layout.jade** in de map **views** en vervang de inhoud door het volgende:

   ```html
   doctype html
   html
     head
       title= title
       link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
       link(rel='stylesheet', href='/stylesheets/style.css')
     body
       nav.navbar.navbar-inverse.navbar-fixed-top
         div.navbar-header
           a.navbar-brand(href='#') My Tasks
       block content
       script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
       script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
   ```

    Zodoende weet de **Jade**-engine dat er bepaalde HTML-code voor onze toepassing moet worden weergegeven en wordt er een **blok** met de naam **content** gemaakt waar we lay-out kunnen opgeven voor onze inhoudspagina's.

    Sla het bestand **layout.jade** op en sluit het bestand.

3. Open nu het bestand **index.jade**, de weergave die door de toepassing wordt gebruikt, en vervang de inhoud van het bestand door het volgende:

   ```html
   extends layout
   block content
        h1 #{title}
        br
        
        form(action="/completetask", method="post")
         table.table.table-striped.table-bordered
            tr
              td Name
              td Category
              td Date
              td Complete
            if (typeof tasks === "undefined")
              tr
                td
            else
              each task in tasks
                tr
                  td #{task.name}
                  td #{task.category}
                  - var date  = new Date(task.date);
                  - var day   = date.getDate();
                  - var month = date.getMonth() + 1;
                  - var year  = date.getFullYear();
                  td #{month + "/" + day + "/" + year}
                  td
                   if(task.completed) 
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
                   else
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
          button.btn.btn-primary(type="submit") Update tasks
        hr
       form.well(action="/addtask", method="post")
        label Item Name:
        input(name="name", type="textbox")
        label Item Category:
        input(name="category", type="textbox")
        br
        button.btn(type="submit") Add item
   ```

Hiermee breidt u de lay-out uit en levert u inhoud voor de tijdelijke aanduiding **content** die we eerder in het bestand **layout.jade** zijn tegengekomen.
   
In deze lay-out hebben we twee HTML-formulieren gemaakt.

Het eerste formulier bevat een tabel voor onze gegevens en een knop waarmee items kunnen worden bijgewerkt door ze naar de methode **/completeTask** van de controller te verzenden.
    
Het tweede formulier bevat twee invoervelden en een knop waarmee een nieuw item kan worden gemaakt door ze naar de methode **/addtask** van de controller te verzenden.

Dit is alles wat we nodig hebben voor een goed werkende toepassing.

## <a name="_Toc395783181"></a>Stap 6: De toepassing lokaal uitvoeren
1. Als u de toepassing wilt testen op uw lokale machine, voert u `npm start` uit in de terminal om uw toepassing te starten. Vernieuw vervolgens de browserpagina [http://localhost:3000](http://localhost:3000). De pagina zou er nu moeten uitzien als in onderstaande afbeelding:
   
    ![Schermopname van de toepassing MyTodo List in een browservenster](./media/sql-api-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > Als u een fout ontvangt over de inspringing in het layout.jade- of index.jade-bestand, moet u ervoor zorgen dat de eerste twee regels in beide bestanden links worden uitgelijnd, zonder spaties. Als er vóór de eerste twee regels spaties staan, verwijdert u deze, slaat u beide bestanden op en vernieuwt u het browservenster. 

2. Gebruik de velden Item, Itemnaam en Categorie om een nieuwe taak in te voeren en klik daarna op **Item toevoegen**. Hiermee maakt u in Azure Cosmos DB een document met deze eigenschappen. 
3. De pagina moet worden bijgewerkt met het nieuwe item in de takenlijst.
   
    ![Schermopname van de toepassing met een nieuw item in de takenlijst](./media/sql-api-nodejs-application/cosmos-db-node-js-added-task.png)
4. Als u een taak wilt voltooien, schakelt u het selectievakje in de kolom Complete (Voltooid) in en klikt u vervolgens op **Update tasks** (Taken bijwerken). Hiermee wordt het door u gemaakte document bijgewerkt en uit de weergave verwijderd.

5. Klik om de toepassing te stoppen op CTRL + C in het terminalvenster en klik vervolgens op **y** om de batchtaak te beëindigen.

## <a name="_Toc395783182"></a>Stap 7: Uw project voor de ontwikkeling van een toepassing implementeren op Azure Websites
1. Als dit nog niet hebt gedaan, schakelt u een git-opslagplaats voor uw Azure-website in. In het onderwerp [Local Git Deployment to Azure App Service](../app-service/app-service-deploy-local-git.md) (Lokale Git-implementatie naar Azure App Service) vindt u hiervoor de instructies.
2. Voeg uw Azure-website toe als een externe git.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Implementeer door naar de externe git te pushen.
   
        git push azure master
4. Over een paar seconden zal GIT de publicatie van uw webtoepassing voltooien en een browser starten waarin u kunt zien hoe uw werk in Azure wordt uitgevoerd.

    Gefeliciteerd! U hebt zojuist uw eerste Node.js Express-webtoepassing met Azure Cosmos DB gemaakt en gepubliceerd naar Azure Websites.

    Als u de volledige referentietoepassing voor deze zelfstudie wilt downloaden of raadplegen, kunt u dit doen op [GitHub][GitHub].

## <a name="_Toc395637775"></a>Volgende stappen

* Wilt u de schaal en prestaties testen met Azure Cosmos DB? Zie [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) (Prestaties en schaal testen met Azure Cosmos DB)
* Leer hoe het [bewaken van een Azure Cosmos DB-account](monitor-accounts.md) werkt.
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).
* Bekijk de [documentatie voor Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/).

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-todo-app

