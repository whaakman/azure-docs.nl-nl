---
title: Een Node.js-web-app bouwen voor Azure Cosmos DB | Microsoft Docs
description: Voor deze zelfstudie behandelt het gebruik van Microsoft Azure Cosmos DB voor het opslaan van en toegang tot gegevens in een Node.js Express-webtoepassing gehost op Azure Websites.
keywords: Toepassingsontwikkeling, zelfstudie, node.js, node.js-zelfstudie leren
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 9da9e63b-e76a-434e-96dd-195ce2699ef3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: mimig
ms.openlocfilehash: 043de0e8a934a2fd92522eeb70261203afac180e
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="_Toc395783175"></a>Een Node.js-webtoepassing bouwen met Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Java](sql-api-java-application.md)
> * [Python](sql-api-python-application.md)
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Voor deze zelfstudie ziet u hoe u Azure Cosmos DB en de SQL-API voor het opslaan van en toegang tot gegevens uit een Node.js Express-toepassing die wordt gehost op Azure Websites. U bouwt een eenvoudige webtoepassing voor taakbeheer, een taken-app, waarmee u taken kunt maken, ophalen en voltooien. De taken worden opgeslagen als JSON-documenten in Azure Cosmos DB. Deze zelfstudie begeleidt u bij het maken en implementeren van de app en legt uit wat er gebeurt in elk fragment.

![Schermopname van de toepassing My Todo List die in deze zelfstudie voor Node.js wordt gemaakt](./media/sql-api-nodejs-application/cosmos-db-node-js-mytodo.png)

Hebt u geen tijd om de zelfstudie te voltooien en wilt u gewoon de volledige oplossing downloaden? Geen probleem, u kunt de complete voorbeeldoplossing downloaden van [GitHub][GitHub]. Lees het [Leesmij](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md)-bestand voor instructies over het uitvoeren van de app.

## <a name="_Toc395783176"></a>Vereisten
> [!TIP]
> Voor deze zelfstudie wordt ervan uitgegaan dat u ervaring hebt met Node.js en Azure Websites
> 
> 

Voordat u de instructies in dit artikel uitvoert, moet u beschikken over het volgende:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js][Node.js] versie v0.10.29 of hoger.
* [Express generator](http://www.expressjs.com/starter/generator.html) (te installeren via `npm install express-generator -g`)
* [Git][Git].

## <a name="_Toc395637761"></a>Stap 1: een Azure Cosmos DB-databaseaccount maken
Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt of de Azure Cosmos DB-emulator gebruikt voor deze zelfstudie, kunt u direct doorgaan naar [Stap 2: een nieuwe Node.js-toepassing maken](#_Toc395783178).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>Stap 2: Maak een nieuwe Node.js-toepassing
Laten we eens kijken hoe u het [Express](http://expressjs.com/)-framework gebruikt om een eenvoudig Hello World Node.js-project te maken.

1. Open een terminal waarmee u graag werkt, zoals de Node.js-opdrachtprompt.
2. Navigeer naar de map waarin u de nieuwe toepassing wilt opslaan.
3. Gebruik de express-generator om een toepassing met de naam **todo** te maken.
   
        express todo
4. Open de nieuwe map **todo** en installeer afhankelijkheden.
   
        cd todo
        npm install
5. Voer uw nieuwe toepassing uit.
   
        npm start
6. U kunt uw nieuwe toepassing weergeven door in uw browser naar [http://localhost:3000](http://localhost:3000) te gaan.
   
    ![Node.js leren - Schermopname van de toepassing Hello World in een browservenster](./media/sql-api-nodejs-application/cosmos-db-node-js-express.png)

    Klik vervolgens in het terminalvenster op CTRL + C om de toepassing te stoppen. Klik daarna op **y** om de batchtaak te beëindigen.

## <a name="_Toc395783179"></a>Stap 3: Aanvullende modules installeren
Het bestand **package.json** is een van de bestanden die zijn gemaakt in de hoofdmap van het project. Dit bestand bevat een lijst met aanvullende modules die u nodig hebt voor uw Node.js-toepassing. Later, wanneer u deze toepassing naar Azure Websites implementeert, wordt dit bestand gebruikt om te bepalen welke modules moeten worden geïnstalleerd op Azure ter ondersteuning van uw toepassing. Voor deze zelfstudie moeten u nog twee pakketten installeren.

1. Keer terug naar de terminal en installeer de **async**-module via npm.
   
        npm install async --save
2. Installeer de **DocumentDB**-module via NPM. Dit is de module waar alle van de Azure DB die Cosmos-magie plaatsvindt.
   
        npm install documentdb --save
3. Een snelle controle van het bestand **package.json** van de toepassing toont u de aanvullende modules. Dit bestand vertelt Azure welke pakketten moeten worden gedownload en geïnstalleerd wanneer uw toepassing wordt uitgevoerd. Het bestand is vergelijkbaar met het onderstaande voorbeeld.
   
        {
          "name": "todo",
          "version": "0.0.0",
          "private": true,
          "scripts": {
            "start": "node ./bin/www"
          },
          "dependencies": {
            "async": "^2.1.4",
            "body-parser": "~1.15.2",
            "cookie-parser": "~1.4.3",
            "debug": "~2.2.0",
            "documentdb": "^1.10.0",
            "express": "~4.14.0",
            "jade": "~1.11.0",
            "morgan": "~1.7.0",
            "serve-favicon": "~2.3.0"
          }
        }
   
    Hiermee wordt bij Node (en later bij Azure) aangegeven dat de toepassing afhankelijk is van deze aanvullende modules.

## <a name="_Toc395783180"></a>Stap 4: De Azure Cosmos DB-service in een knooppunttoepassing gebruiken
Hiermee is de installatie en eerste configuratie voltooid en kunnen we aan het echte werk beginnen, namelijk het schrijven van code met Azure Cosmos DB.

### <a name="create-the-model"></a>Het model maken
1. Maak in de projectmap een nieuwe map met de naam **models**. Deze map moet in dezelfde map staan als het package.json-bestand.
2. Maak in de map **models** een nieuw bestand met de naam **taskDao.js**. Dit bestand bevat het model voor de taken die met onze toepassing worden gemaakt.
3. Maak in dezelfde map **models** nog een bestand met de naam **docdbUtils.js**. Dit bestand bevat enkele handige, opnieuw bruikbare codefragmenten die we voor de hele toepassing zullen gebruiken. 
4. Kopieer de volgende code naar het bestand **docdbUtils.js**.
   
        var DocumentDBClient = require('documentdb').DocumentClient;
   
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id= @id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
   
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
   
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
   
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };               
   
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
   
                            client.createCollection(databaseLink, collectionSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
   
        module.exports = DocDBUtils;
   
5. Sla het bestand **docdbUtils.js** op en sluit het bestand.
6. Voeg aan het begin van het bestand **taskDao.js** de volgende code toe om te verwijzen naar de **DocumentDBClient** en **docdbUtils.js** die eerder is gemaakt:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. Vervolgens voegt u code toe om het taakobject te definiëren en te exporteren. Hiermee wordt het taakobject geïnitialiseerd en wordt de database- en documentverzameling ingesteld die we zullen gebruiken.
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. Voeg vervolgens de volgende code toe om de aanvullende methoden voor het taakobject te definiëren, waardoor er interactie mogelijk is met gegevens die zijn opgeslagen in Azure Cosmos DB.
   
        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
   
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);
                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
   
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
   
            find: function (querySpec, callback) {
                var self = this;
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results);
                    }
                });
            },
   
            addItem: function (item, callback) {
                var self = this;
   
                item.date = Date.now();
                item.completed = false;
   
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, doc);
                    }
                });
            },
   
            updateItem: function (itemId, callback) {
                var self = this;
   
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        doc.completed = true;
   
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
   
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
   
            getItem: function (itemId, callback) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id = @id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };
9. Sla het bestand **taskDao.js** op en sluit het bestand. 

### <a name="create-the-controller"></a>De controller maken
1. Maak in de map **routes** van uw project een nieuw bestand met de naam **tasklist.js**. 
2. Voeg de volgende code toe aan het bestand **tasklist.js**. Hiermee worden de DocumentDBClient- en async-modules geladen die door het bestand **tasklist.js** worden gebruikt. Met deze code wordt ook de functie **TaskList** gedefinieerd, waarnaar een exemplaar van het eerder gemaakte object **Task** wordt doorgegeven:
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. Voeg vervolgens methoden toe aan het bestand **tasklist.js** door de methoden voor **showTasks, addTask** en **completeTasks** toe te voegen:
   
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
   
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
   
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
   
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
   
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
   
                    res.redirect('/');
                });
            },
   
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
   
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };
4. Sla het bestand **tasklist.js** op en sluit het bestand.

### <a name="add-configjs"></a>Config.js toevoegen
1. Maak een nieuw bestand met de naam **config.js** in uw projectmap.
2. Voeg het volgende toe aan het bestand **config.js**. Hiermee definieert u configuratie-instellingen en waarden die nodig zijn voor de toepassing.
   
        var config = {}
   
        config.host = process.env.HOST || "[the URI value from the Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. Werk in het bestand **config.js** de waarden voor HOST en AUTH_KEY bij door gebruik te maken van de waarden op de blade Sleutels van uw Azure Cosmos DB-account in [Microsoft Azure Portal](https://portal.azure.com).
4. Sla het bestand **config.js** op en sluit het bestand.

### <a name="modify-appjs"></a>App.js wijzigen
1. Ga naar de projectmap en open het bestand **app.js**. Dit bestand is gemaakt toen de Express-webtoepassing werd gemaakt.
2. Voeg de volgende code toe boven aan het bestand **app.js**.
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. Deze code definieert het configuratiebestand dat moet worden gebruikt en leest waarden uit dit bestand naar enkele variabelen die we zo dadelijk gaan gebruiken.
4. Vervang de volgende twee regels in het bestand **app.js**:
   
        app.use('/', index);
        app.use('/users', users); 
   
      door het volgende codefragment:
   
        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
        app.set('view engine', 'jade');
5. Deze regels definiëren een nieuw exemplaar van het object **TaskDao** met een nieuwe verbinding met Azure Cosmos DB (door gebruik te maken van de gelezen waarden uit het bestand **config.js**), initialiseren het taakobject en binden de formulieracties aan de methoden op de **TaskList**-controller. 
6. Tot slot slaat u het bestand **app.js** op en sluit u het bestand.

## <a name="_Toc395783181"></a>Stap 5: Een gebruikersinterface maken
U kunt zich nu concentreren op het bouwen van de gebruikersinterface, zodat gebruikers kunnen communiceren met de toepassing. De Express-toepassing die is gemaakt, gebruikt **Jade** als weergave-engine. Raadpleeg [http://jade-lang.com/](http://jade-lang.com/) voor meer informatie over Jade.

1. Het bestand **layout.jade** in de map **views** wordt gebruikt als een algemeen sjabloon voor andere **.jade**-bestanden. In deze stap wordt het bestand aangepast voor het gebruik van [Twitter Bootstrap](https://github.com/twbs/bootstrap). Dit is een werkset waarmee u eenvoudig een aantrekkelijk ogende website kunt maken. 
2. Open het bestand **layout.jade** in de map **views** en vervang de inhoud door het volgende:

    ```
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
                       input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
             button.btn.btn-primary(type="submit") Update tasks
           hr
           form.well(action="/addtask", method="post")
             .form-group
               label(for="name") Item Name:
               input.form-control(name="name", type="textbox")
             .form-group
               label(for="category") Item Category:
               input.form-control(name="category", type="textbox")
             br
             button.btn(type="submit") Add item
   

Hiermee breidt u de lay-out uit en levert u inhoud voor de tijdelijke aanduiding **content** die we eerder in het bestand **layout.jade** zijn tegengekomen.
   
In deze lay-out hebben we twee HTML-formulieren gemaakt.

Het eerste formulier bevat een tabel voor onze gegevens en een knop waarmee items kunnen worden bijgewerkt door ze naar de methode **/completetask** van de controller te verzenden.
    
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
4. Als u een taak wilt voltooien, schakelt u het selectievakje in de kolom Complete (Voltooid) in en klikt u vervolgens op **Update tasks** (Taken bijwerken). Hiermee wordt het document dat u al hebt gemaakt, bijgewerkt.

5. Klik om de toepassing te stoppen op CTRL + C in het terminalvenster en klik vervolgens op **y** om de batchtaak te beëindigen.

## <a name="_Toc395783182"></a>Stap 7: Uw project voor de ontwikkeling van een toepassing implementeren op Azure Websites
1. Als dit nog niet hebt gedaan, schakelt u een git-opslagplaats voor uw Azure-website in. In het onderwerp [Local Git Deployment to Azure App Service](../app-service/app-service-deploy-local-git.md) (Lokale Git-implementatie naar Azure App Service) vindt u hiervoor de instructies.
2. Voeg uw Azure-website toe als een externe git.
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. Implementeer door naar de externe git te pushen.
   
        git push azure master
4. Binnen een paar seconden zal git publicatie van uw webtoepassing voltooien en een browser starten waarin u kunt zien uw werk in Azure wordt uitgevoerd!

    Gefeliciteerd. U hebt zojuist uw eerste Node.js Express-webtoepassing met Azure Cosmos DB gemaakt en gepubliceerd naar Azure Websites.

    Als u de volledige referentietoepassing voor deze zelfstudie wilt downloaden of raadplegen, kunt u dit doen op [GitHub][GitHub].

## <a name="_Toc395637775"></a>Volgende stappen

* Wilt u de schaal en prestaties testen met Azure Cosmos DB? Zie [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md) (Prestaties en schaal testen met Azure Cosmos DB)
* Leer hoe het [bewaken van een Azure Cosmos DB-account](monitor-accounts.md) werkt.
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).
* Bekijk de [documentatie voor Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/).

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

