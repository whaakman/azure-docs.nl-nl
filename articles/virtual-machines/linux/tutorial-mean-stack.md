---
title: Maken van een gemiddelde stack op een Linux VM in Azure | Microsoft Docs
description: Informatie over het maken van een stack MongoDB, snelle AngularJS en Node.js (gemiddelde) op een Linux VM in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 1d74ead08dfb63276afb08bdcb7f4e3e3db5bfd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a>Maak een stack MongoDB, snelle AngularJS en Node.js (gemiddelde) op een Linux VM in Azure

Deze zelfstudie laat zien hoe u een stack MongoDB, snelle AngularJS en Node.js (gemiddelde) implementeren op een Linux VM in Azure. De gemiddelde stack die u maakt kan toevoegen, verwijderen en het weergeven van rapporten in een database. Procedures voor:

> [!div class="checklist"]
> * Een Linux-VM maken
> * Node.js installeren
> * MongoDB installeren en instellen van de server
> * Express installeren en instellen van de routes naar de server
> * Toegang tot de routes met AngularJS
> * De toepassing uitvoeren

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).


## <a name="create-a-linux-vm"></a>Een Linux-VM maken

Maak een resourcegroep met de [az groep maken](https://docs.microsoft.com/cli/azure/group#az_group_create) opdracht en maak een Linux-VM met de [az vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Het volgende voorbeeld wordt de Azure CLI voor het maken van een resourcegroep met de naam *myResourceGroupMEAN* in de *eastus* locatie. Een virtuele machine gemaakt met de naam *myVM* met SSH-sleutels als deze niet al bestaan op de standaardlocatie van de sleutel. Als u een specifieke set van sleutels, gebruikt de--ssh-sleutel / waarde-optie.

```azurecli-interactive
az group create --name myResourceGroupMEAN --location eastus
az vm create \
    --resource-group myResourceGroupMEAN \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password 'Azure12345678!' \
    --generate-ssh-keys
az vm open-port --port 3300 --resource-group myResourceGroupMEAN --name myVM
```

Wanneer de virtuele machine is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld: 

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroupMEAN/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.72.77.9",
  "resourceGroup": "myResourceGroupMEAN"
}
```
Noteer het `publicIpAddress`. Dit adres wordt gebruikt voor toegang tot de virtuele machine.

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de virtuele machine. Zorg ervoor dat de juiste openbare IP-adres gebruiken. In ons voorbeeld boven onze IP is adres 13.72.77.9.

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a>Node.js installeren

[Node.js](https://nodejs.org/en/) is een JavaScript-runtime gebouwd op van de Chrome V8 JavaScript-engine. Node.js wordt gebruikt in deze zelfstudie voor het instellen van de Express-routes en de AngularJS-domeincontrollers.

Installeer op de virtuele machine met behulp van de bash-shell die u hebt geopend met SSH, Node.js.

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-the-server"></a>MongoDB installeren en instellen van de server
[MongoDB](http://www.mongodb.com) -gegevens opslaat in flexibele, zoals JSON-documenten. Velden in een database document naar kunnen variëren en gegevensstructuur na verloop van tijd kan worden gewijzigd. Voor onze voorbeeldtoepassing toevoegen we adresboekrecords aan MongoDB die rapportnaam, isbn nummer, auteur en het aantal pagina's bevatten. 

1. Stel op de virtuele machine met behulp van de bash-shell die u hebt geopend met SSH, de MongoDB-sleutel.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. De package manager bijwerken met de sleutel.
  
    ```bash
    sudo apt-get update
    ```

3. MongoDB installeren.

    ```bash
    sudo apt-get install -y mongodb
    ```

4. Start de server.

    ```bash
    sudo service mongodb start
    ```

5. Er moet ook installeren de [hoofdtekst parser](https://www.npmjs.com/package/body-parser-json) pakket om ons verwerken van de JSON aanvragen doorgegeven aan de server te helpen.

    Installeer de npm package manager.

    ```bash
    sudo apt-get install npm
    ```

    Installeer het pakket van de parser hoofdtekst.
    
    ```bash
    sudo npm install body-parser
    ```

6. Maak een map met de naam *Books* en voeg een bestand toe met de naam *server.js* die de configuratie voor de webserver bevat.

    ```node.js
    var express = require('express');
    var bodyParser = require('body-parser');
    var app = express();
    app.use(express.static(__dirname + '/public'));
    app.use(bodyParser.json());
    require('./apps/routes')(app);
    app.set('port', 3300);
    app.listen(app.get('port'), function() {
        console.log('Server up: http://localhost:' + app.get('port'));
    });
    ```

## <a name="install-express-and-set-up-routes-to-the-server"></a>Express installeren en instellen van de routes naar de server

[Express](https://expressjs.com) is een minimaal en flexibel Node.js web application framework met functies voor webtoepassingen en mobiele toepassingen. Snelle in deze zelfstudie wordt gebruikt om door te geven voor het adresboek van gegevens van en naar onze MongoDB-database. [Mongoose](http://mongoosejs.com) biedt een ongecompliceerde, schema's gebaseerde oplossing om te modelleren uw toepassingsgegevens. Mongoose wordt gebruikt in deze zelfstudie voor het bieden van een boek-schema voor de database.

1. Snelle en Mongoose installeren.

    ```bash
    sudo npm install express mongoose
    ```

2. In de *Books* map, maak een map met de naam *apps* en toevoegen van een bestand met de naam *routes.js* van de expliciete routes gedefinieerd.

    ```node.js
    var Book = require('./models/book');
    module.exports = function(app) {
      app.get('/book', function(req, res) {
        Book.find({}, function(err, result) {
          if ( err ) throw err;
          res.json(result);
        });
      }); 
      app.post('/book', function(req, res) {
        var book = new Book( {
          name:req.body.name,
          isbn:req.body.isbn,
          author:req.body.author,
          pages:req.body.pages
        });
        book.save(function(err, result) {
          if ( err ) throw err;
          res.json( {
            message:"Successfully added book",
            book:result
          });
        });
      });
      app.delete("/book/:isbn", function(req, res) {
        Book.findOneAndRemove(req.query, function(err, result) {
          if ( err ) throw err;
          res.json( {
            message: "Successfully deleted the book",
            book: result
          });
        });
      });
      var path = require('path');
      app.get('*', function(req, res) {
        res.sendfile(path.join(__dirname + '/public', 'index.html'));
      });
    };
    ```

3. In de *apps* map, maak een map met de naam *modellen* en toevoegen van een bestand met de naam *book.js* met de adresboek Modelconfiguratie gedefinieerd.  

    ```node.js
    var mongoose = require('mongoose');
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost);
    mongoose.connection;
    mongoose.set('debug', true);
    var bookSchema = mongoose.Schema( {
      name: String,
      isbn: {type: String, index: true},
      author: String,
      pages: Number
    });
    var Book = mongoose.model('Book', bookSchema);
    module.exports = mongoose.model('Book', bookSchema); 
    ```

## <a name="access-the-routes-with-angularjs"></a>Toegang tot de routes met AngularJS

[AngularJS](https://angularjs.org) biedt een webframework voor het maken van weergaven in uw webtoepassingen. In deze zelfstudie gebruiken we AngularJS om verbinding te onze webpagina snelle en acties uitvoeren op onze adresboek-database.

1. Wijzig de map back-up naar *Books* (`cd ../..`), en maak vervolgens een map met de naam *openbare* en toevoegen van een bestand met de naam *script.js* met de configuratie van de domeincontroller gedefinieerd.

    ```node.js
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
      $http( {
        method: 'GET',
        url: '/book'
      }).then(function successCallback(response) {
        $scope.books = response.data;
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });
      $scope.del_book = function(book) {
        $http( {
          method: 'DELETE',
          url: '/book/:isbn',
          params: {'isbn': book.isbn}
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
      $scope.add_book = function() {
        var body = '{ "name": "' + $scope.Name + 
        '", "isbn": "' + $scope.Isbn +
        '", "author": "' + $scope.Author + 
        '", "pages": "' + $scope.Pages + '" }';
        $http({
          method: 'POST',
          url: '/book',
          data: body
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
    });
    ```
    
2. In de *openbare* map, maakt u een bestand met de naam *index.html* met de webpagina die is gedefinieerd.

    ```html
    <!doctype html>
    <html ng-app="myApp" ng-controller="myCtrl">
      <head>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="script.js"></script>
      </head>
      <body>
        <div>
          <table>
            <tr>
              <td>Name:</td> 
              <td><input type="text" ng-model="Name"></td>
            </tr>
            <tr>
              <td>Isbn:</td>
              <td><input type="text" ng-model="Isbn"></td>
            </tr>
            <tr>
              <td>Author:</td> 
              <td><input type="text" ng-model="Author"></td>
            </tr>
            <tr>
              <td>Pages:</td>
              <td><input type="number" ng-model="Pages"></td>
            </tr>
          </table>
          <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
          <table>
            <tr>
              <th>Name</th>
              <th>Isbn</th>
              <th>Author</th>
              <th>Pages</th>
            </tr>
            <tr ng-repeat="book in books">
              <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
              <td>{{book.name}}</td>
              <td>{{book.isbn}}</td>
              <td>{{book.author}}</td>
              <td>{{book.pages}}</td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    ```

##  <a name="run-the-application"></a>De toepassing uitvoeren

1. Wijzig de map back-up naar *Books* (`cd ..`) en start de server door het uitvoeren van deze opdracht:

    ```bash
    nodejs server.js
    ```

2. Open een webbrowser naar het adres dat u hebt vastgelegd voor de virtuele machine. Bijvoorbeeld: *http://13.72.77.9:3300*. U ziet dat lijkt op de volgende pagina:

    ![Book record](media/tutorial-mean/meanstack-init.png)

3. Gegevens invoeren in de tekstvakken en klik op **toevoegen**. Bijvoorbeeld:

    ![Book record toevoegen](media/tutorial-mean/meanstack-add.png)

4. Na de pagina te vernieuwen, ziet u dat lijkt op deze pagina:

    ![Lijst adresboekrecords](media/tutorial-mean/meanstack-list.png)

5. U kunt klikken **verwijderen** en de book record verwijderen uit de database.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een webtoepassing waarin wordt bijgehouden adresboek registreert met behulp van een gemiddelde gemaakt stack op een Linux-VM. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Linux-VM maken
> * Node.js installeren
> * MongoDB installeren en instellen van de server
> * Express installeren en instellen van de routes naar de server
> * Toegang tot de routes met AngularJS
> * De toepassing uitvoeren

Ga naar de volgende zelfstudie voor meer informatie over het beveiligen van webservers met SSL-certificaten.

> [!div class="nextstepaction"]
> [Webserver met SSL beveiligde](tutorial-secure-web-server.md)
