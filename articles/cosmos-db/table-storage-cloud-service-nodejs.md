---
title: 'Azure Table storage: een Node.js-web-app bouwen | Microsoft Docs'
description: Een zelfstudie bouwt op de Web-App met snelle zelfstudie voort door toevoeging van Azure Storage-services en de Azure-module.
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: e90959a2-4cb2-4b19-9bfb-aede15b18b1c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 9acd197c26e6365e396fd8f6321d764bba7bbb6c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-table-storage-nodejs-web-application"></a>Azure Table storage: Node.js-webtoepassing
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie, de toepassing u gemaakt in de [Node.js-webtoepassing met een snelle] zelfstudie wordt uitgebreid met de Microsoft Azure-clientbibliotheken voor Node.js gebruiken om te werken met data management-services. U kunt uw toepassing uitbreiden door een web gebaseerde takenlijst-toepassing die u naar Azure implementeren kunt te maken. De takenlijst kan een gebruiker taken ophalen, het toevoegen van nieuwe taken en taken te markeren als voltooid.

De items worden opgeslagen in Azure Storage. Azure Storage biedt niet-gestructureerde gegevensopslag die fouttolerante en maximaal beschikbaar is. Azure Storage bevat verschillende gegevensstructuren waar u kunt opslaan en toegang tot gegevens. U kunt de storage-services van de API's opgenomen in de Azure SDK voor Node.js of via de REST-API's gebruiken. Zie voor meer informatie [opslaan en toegang tot gegevens in Azure].

Deze zelfstudie wordt ervan uitgegaan dat u hebt voltooid de [Node.js-webtoepassing] en [Node.js snelle][Node.js-webtoepassing met een snelle] zelfstudies.

Bevat de volgende informatie:

* Werken met de engine Jade sjabloon
* Werken met Azure Data Management-services

De volgende schermafbeelding ziet de voltooide toepassing:

![De voltooide webpagina in internet explorer](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Storage-referenties instelling in het bestand Web.Config
U moet doorgeven in de storage-referenties voor toegang tot Azure Storage. Dit wordt gedaan door het gebruik van de toepassingsinstellingen web.config.
De web.config-instellingen worden doorgegeven als omgevingsvariabelen knooppunt, die vervolgens worden gelezen door de Azure SDK.

> [!NOTE]
> Storage-referenties worden alleen gebruikt wanneer de toepassing wordt geïmplementeerd naar Azure. Wanneer in de emulator wordt uitgevoerd, wordt de toepassing de opslagemulator gebruikt.
>
>

Voer de volgende stappen uit om de opslagaccountreferenties ophalen en toe te voegen aan het web.config-instellingen:

1. Als deze nog niet is geopend, start u de Azure PowerShell uit de **Start** menu door het uitbreiden van **alle programma's, Azure**, met de rechtermuisknop op **Azure PowerShell**, en selecteer vervolgens **als Administrator uitvoeren**.
2. Wijzig de mappen in de map met uw toepassing. Bijvoorbeeld: C:\\knooppunt\\tasklist\\WebRole1.
3. Voer de volgende cmdlet om op te halen van de accountgegevens voor de opslag van de Azure Powershell-venster:

    ```powershell
    PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts
    ```

   De voorgaande cmdlet haalt de lijst met opslagaccounts en sleutels die zijn gekoppeld aan de gehoste service-account.

   > [!NOTE]
   > Omdat de Azure SDK een opslagaccount maakt wanneer u een service implementeert, moet al een opslagaccount van het implementeren van uw toepassing in de vorige handleidingen bestaan.
   >
   >
4. Open de **ServiceDefinition.csdef** bestand met de omgevingsinstellingen die worden gebruikt wanneer de toepassing wordt geïmplementeerd naar Azure:

    ```powershell
    PS C:\node\tasklist> notepad ServiceDefinition.csdef
    ```

5. Voeg het volgende blok onder **omgeving** element, vervangen door {OPSLAGACCOUNT} en {TOEGANGSSLEUTEL voor opslag} met de accountnaam en de primaire sleutel voor het opslagaccount dat u wilt gebruiken voor implementatie:

  <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
  <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

   ![De inhoud van het bestand web.cloud.config](./media/table-storage-cloud-service-nodejs/node37.png)

6. Sla het bestand op en sluit Kladblok.

### <a name="install-additional-modules"></a>Aanvullende modules installeren
1. Gebruik de volgende opdracht voor het installeren van de [azure] [knooppunt-uuid] [nconf] en [asynchrone] modules lokaal ook om op te slaan een vermelding voor hen de **package.json** bestand:

  ```powershell
  PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save
  ```

  De uitvoer van deze opdracht ziet er ongeveer als volgt:

  ```
  node-uuid@1.4.1 node_modules\node-uuid

  nconf@0.6.9 node_modules\nconf
  ├── ini@1.1.0
  ├── async@0.2.9
  └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

  azure-storage@0.1.0 node_modules\azure-storage
  ├── extend@1.2.1
  ├── xmlbuilder@0.4.3
  ├── mime@1.2.11
  ├── underscore@1.4.4
  ├── validator@3.1.0
  ├── node-uuid@1.4.1
  ├── xml2js@0.2.7 (sax@0.5.2)
  └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)
  ```

## <a name="using-the-table-service-in-a-node-application"></a>De tabel-service in een knooppunttoepassing gebruiken
In deze sectie worden de basistoepassing gemaakt door de **snelle** opdracht wordt uitgebreid met het toevoegen van een **task.js** -bestand met het model voor uw taken. Wijzig de bestaande **app.js** -bestand en maak een nieuwe **tasklist.js** bestand dat gebruikmaakt van het model.

### <a name="create-the-model"></a>Het model maken
1. In de **WebRole1** directory, maak een nieuwe map met de naam **modellen**.
2. In de **modellen** directory, maak een nieuw bestand met de naam **task.js**. Dit bestand bevat het model voor de taken die zijn gemaakt door uw toepassing.
3. Aan het begin van de **task.js** bestand, voeg de volgende code om te verwijzen naar de vereiste bibliotheken:

    ```nodejs
    var azure = require('azure-storage');
    var uuid = require('node-uuid');
    var entityGen = azure.TableUtilities.entityGenerator;
    ```

4. Vervolgens voegt u code om te definiëren en exporteren van het taakobject. Het taakobject is verantwoordelijk voor het verbinding maken met de tabel.

    ```nodejs
    module.exports = Task;

    function Task(storageClient, tableName, partitionKey) {
      this.storageClient = storageClient;
      this.tableName = tableName;
      this.partitionKey = partitionKey;
      this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
        if(error) {
          throw error;
        }
      });
    };
    ```

5. Vervolgens voegt u de volgende code om aanvullende methoden voor het taakobject te definiëren waardoor er interactie met gegevens die zijn opgeslagen in de tabel:

    ```nodejs
    Task.prototype = {
      find: function(query, callback) {
        self = this;
        self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
          if(error) {
            callback(error);
          } else {
            callback(null, result.entries);
          }
        });
      },

      addItem: function(item, callback) {
        self = this;
        // use entityGenerator to set types
        // NOTE: RowKey must be a string type, even though
        // it contains a GUID in this example.
        var itemDescriptor = {
          PartitionKey: entityGen.String(self.partitionKey),
          RowKey: entityGen.String(uuid()),
          name: entityGen.String(item.name),
          category: entityGen.String(item.category),
          completed: entityGen.Boolean(false)
        };

        self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
          if(error){
            callback(error);
          }
          callback(null);
        });
      },

      updateItem: function(rKey, callback) {
        self = this;
        self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
          if(error) {
            callback(error);
          }
          entity.completed._ = true;
          self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
            if(error) {
              callback(error);
            }
            callback(null);
          });
        });
      }
    }
    ```

6. Sla op en sluit de **task.js** bestand.

### <a name="create-the-controller"></a>De controller maken
1. In de **WebRole1/routes** directory, maak een nieuw bestand met de naam **tasklist.js** en open het in een teksteditor.
2. Voeg de volgende code toe aan het bestand **tasklist.js**. Deze code wordt geladen voor de azure- en async-modules die worden gebruikt door **tasklist.js** en definieert de **TaskList** functie die wordt doorgegeven een exemplaar van de **taak** we object eerder hebt gedefinieerd:

    ```nodejs
    var azure = require('azure-storage');
    var async = require('async');

    module.exports = TaskList;

    function TaskList(task) {
      this.task = task;
    }
    ```

3. Blijven toevoegen aan de **tasklist.js** bestand door het toevoegen van de methoden voor **showTasks**, **addTask**, en **completeTasks**:

    ```nodejs
    TaskList.prototype = {
      showTasks: function(req, res) {
        self = this;
        var query = azure.TableQuery()
          .where('completed eq ?', false);
        self.task.find(query, function itemsFound(error, items) {
          res.render('index',{title: 'My ToDo List ', tasks: items});
        });
      },

      addTask: function(req,res) {
        var self = this
        var item = req.body.item;
        self.task.addItem(item, function itemAdded(error) {
          if(error) {
            throw error;
          }
          res.redirect('/');
        });
      },

      completeTask: function(req,res) {
        var self = this;
        var completedTasks = Object.keys(req.body);
        async.forEach(completedTasks, function taskIterator(completedTask, callback) {
          self.task.updateItem(completedTask, function itemsUpdated(error) {
            if(error){
              callback(error);
            } else {
              callback(null);
            }
          });
        }, function goHome(error){
          if(error) {
            throw error;
          } else {
            res.redirect('/');
          }
        });
      }
    }
    ```

4. Sla de **tasklist.js** bestand.

### <a name="modify-appjs"></a>App.js wijzigen
1. In de **WebRole1** directory, open de **app.js** bestand in een teksteditor.
2. Voeg de volgende voor het laden van de azure-module en stel de sleutel van naam en partitie in de tabel aan het begin van het bestand:

    ```nodejs
    var azure = require('azure-storage');
    var tableName = 'tasks';
    var partitionKey = 'hometasks';
    ```

3. In het bestand app.js, bladert u omlaag naar waar u de volgende regel zien:

    ```nodejs
    app.use('/', routes);
    app.use('/users', users);
    ```

    De voorgaande regels vervangen door de volgende code. Deze code initialiseert een exemplaar van <strong>taak</strong> met een verbinding met uw storage-account. De <strong>taak</strong> wordt doorgegeven aan de <strong>TaskList</strong>, waarbij wordt gebruikt om te communiceren met de tabel-service:

    ```nodejs
    var TaskList = require('./routes/tasklist');
    var Task = require('./models/task');
    var task = new Task(azure.createTableService(), tableName, partitionKey);
    var taskList = new TaskList(task);

    app.get('/', taskList.showTasks.bind(taskList));
    app.post('/addtask', taskList.addTask.bind(taskList));
    app.post('/completetask', taskList.completeTask.bind(taskList));
    ```

4. Sla de **app.js** bestand.

### <a name="modify-the-index-view"></a>Wijzig de weergave index
1. Wijzig de mappen op de **weergaven** directory en open de **index.jade** bestand in een teksteditor.
2. Vervang de inhoud van de **index.jade** bestand met de volgende code. Deze code definieert de weergave voor het weergeven van bestaande taken en definieert een formulier voor het toevoegen van nieuwe taken en bestaande bestanden markeren als voltooid.

    ```
    extends layout

    block content
      h1= title
      br

      form(action="/completetask", method="post")
        table.table.table-striped.table-bordered
          tr
            td Name
            td Category
            td Date
            td Complete
          if tasks != []
            tr
              td
          else
            each task in tasks
              tr
                td #{task.name._}
                td #{task.category._}
                - var day   = task.Timestamp._.getDate();
                - var month = task.Timestamp._.getMonth() + 1;
                - var year  = task.Timestamp._.getFullYear();
                td #{month + "/" + day + "/" + year}
                td
                  input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
        button.btn(type="submit") Update tasks
      hr
      form.well(action="/addtask", method="post")
        label Item Name:
        input(name="item[name]", type="textbox")
        label Item Category:
        input(name="item[category]", type="textbox")
        br
        button.btn(type="submit") Add item
    ```

3. Opslaan en sluiten **index.jade** bestand.

### <a name="modify-the-global-layout"></a>De indeling van de globale wijzigen
Het bestand **layout.jade** in de map **views** wordt gebruikt als een algemeen sjabloon voor andere **.jade**-bestanden. In deze stap, wijzigt u de **layout.jade** bestand om te gebruiken [Twitter Bootstrap](https://github.com/twbs/bootstrap), dit is een werkset waarmee u eenvoudig een aantrekkelijk ogende website ontwerpen kunt.

1. Downloaden en uitpakken van de bestanden voor [Twitter Bootstrap](http://getbootstrap.com/). Kopieer de **bootstrap.min.css** bestand van de **bootstrap\\verdeling\\css** map de **openbare\\stylesheets** map van uw toepassing tasklist.
2. Van de **weergaven** map, open de **layout.jade** -bestand in een teksteditor en vervang de inhoud door het volgende:

    DOCTYPE HTML-html head titel = Titelkoppeling (rel = 'stylesheet', href='/stylesheets/bootstrap.min.css') koppeling (rel = 'stylesheet', href='/stylesheets/style.css') body.app nav.navbar.navbar-standaard div.navbar-header a.navbar-brand(href='/') inhoud van mijn taken blokkeren

3. Sla de **layout.jade** bestand.

### <a name="running-the-application-in-the-emulator"></a>De toepassing wordt uitgevoerd in de Emulator
Gebruik de volgende opdracht voor het starten van de toepassing in de emulator.

```powershell
PS C:\node\tasklist\WebRole1> start-azureemulator -launch
```

De browser wordt geopend en de volgende pagina wordt weergegeven:

![Een web wisselbaar geheugen: met de titel van mijn takenlijst met een tabel met taken en velden in om een nieuwe taak toevoegen.](./media/table-storage-cloud-service-nodejs/node44.png)

Gebruik het formulier items toevoegen of verwijderen van bestaande items door deze te markeren als voltooid.

## <a name="publishing-the-application-to-azure"></a>Publiceren van de toepassing in Azure
Aanroepen in de Windows PowerShell-venster de volgende cmdlet als u wilt uw gehoste service in Azure implementeren.

```powershell
PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch
```

Vervang **myuniquename** met een unieke naam voor deze toepassing. Vervang **datacentername** met de naam van een Azure-Datacenter, zoals **VS-West**.

Nadat de implementatie voltooid is, ziet u een reactie vergelijkbaar met het volgende:

```
  PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
  WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
  WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
  WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
  WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
  WARNING: 2:19:01 PM - Connecting...
  WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
  WARNING: 2:19:40 PM - Upgrading...
  WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
  WARNING: 2:22:48 PM - Initializing...
  WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
  WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.
```

Door op te geven de **-starten** optie in de vorige cmdlet kan de browser wordt geopend en wordt uw toepassing in Azure wordt uitgevoerd als het publiceren is voltooid.

![Een browservenster met de pagina Mijn takenlijst. De URL geeft aan dat de pagina nu wordt gehost op Azure.](./media/table-storage-cloud-service-nodejs/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Stoppen en verwijderen van uw toepassing
Na implementatie van uw toepassing, wilt u mogelijk uitgeschakeld zodat u kunt kosten voorkomen of bouwen en implementeren van andere toepassingen binnen de periode voor de gratis proefversie.

Webrolexemplaren in Azure worden per uur van verbruikte servertijd in rekening gebracht.
Er wordt servertijd verbruikt zodra de toepassing is geïmplementeerd, zelfs als de exemplaren niet worden uitgevoerd en de gestopte status hebben.

De volgende stappen laten zien hoe om te stoppen en verwijderen van uw toepassing.

1. In het Windows PowerShell-venster stopt u de service-implementatie die u in de vorige sectie hebt gemaakt, met de volgende cmdlet:

    ```powershell
    PS C:\node\tasklist\WebRole1> Stop-AzureService
    ```

   Het kan enkele minuten duren voordat de service is gestopt. Als de service is gestopt, krijgt u een bericht waarin dit wordt aangegeven.

2. Als u de service wilt verwijderen, roept u de volgende cmdlet aan:

    ```powershell
    PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist
    ```

   Wanneer dit wordt gevraagd, typt u **Y** om de service te verwijderen.

   Het kan enkele minuten duren voordat de service is verwijderd. Nadat de service is verwijderd, ontvangt u een bericht weergegeven dat aangeeft dat de service is verwijderd.

[Node.js-webtoepassing met een snelle]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
[opslaan en toegang tot gegevens in Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
[Node.js-webtoepassing]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/


