---
title: Vanuit Node.js verbinding maken met Azure Database voor MySQL | Microsoft Docs
description: Deze snelstartgids bevat enkele voorbeelden van Node.js-code die u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit Azure Database voor MySQL.
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 2f18016614b229273aa4d661991149be949ce238
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-database-for-mysql-use-nodejs-to-connect-and-query-data"></a>Azure Database voor MySQL: Node.js gebruiken om verbinding te maken en query's uit te voeren op gegevens
In deze snelstartgids ziet u hoe u vanuit de platformen Windows, Ubuntu Linux, en Mac met behulp van [Node.js](https://nodejs.org/) verbinding maakt met een Azure Database voor MySQL. U ziet hier hoe u SQL-instructies gebruikt om gegevens in de database op te vragen, in te voegen, bij te werken en te verwijderen. In dit onderwerp wordt ervan uitgegaan dat u bekend bent met behulp van Node.js ontwikkelen en dat u niet bekend bent met werken met Azure-Database voor MySQL.

## <a name="prerequisites"></a>Vereisten
In deze snelstartgids worden de resources die in een van deze handleidingen zijn gemaakt, als uitgangspunt gebruikt:
- [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Een Azure-database voor een MySQL-server maken met behulp van Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

U moet ook het volgende doen:
- De [Node.js](https://nodejs.org)-runtime installeren.
- Het [mysql2](https://www.npmjs.com/package/mysql2)-pakket installeren om vanuit de Node.js-toepassing verbinding te maken met MySQL. 

## <a name="install-nodejs-and-the-mysql-connector"></a>Node.js en de MySQL-connector installeren
Volg de instructies in het desbetreffende gedeelte voor het installeren van Node.js afhankelijk van uw platform. Gebruik NPM voor het installeren van het mysql2-pakket en de bijbehorende afhankelijkheden in de projectmap.

### <a name="windows"></a>**Windows**
1. Ga naar de [Node.js pagina met downloads](https://nodejs.org/en/download/), en selecteer vervolgens de gewenste optie voor Windows installer.
2. Maak een lokale projectmap, zoals `nodejsmysql`. 
3. Start de opdrachtprompt en wijzig vervolgens de directory in de projectmap zoals`cd c:\nodejsmysql\`
4. Voer het hulpprogramma NPM uit om de mysql2-bibliotheek in de projectmap te installeren.

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. Controleer de installatie door de `npm list`-uitvoertekst voor `mysql2@1.3.5` te controleren.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
1. Voer de volgende opdrachten uit om **Node.js** en **NPM**, het pakketbeheerprogramma voor Node.js, te installeren.

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. Voer de volgende opdrachten voor het maken van een projectmap `mysqlnodejs` en installeer het pakket mysql2 in die map.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. Controleer de installatie door de uitvoertekst van de npm-lijst te controleren voor `mysql2@1.3.5`.

### <a name="mac-os"></a>**Mac OS**
1. Voer de volgende opdrachten in om **brew** te installeren, een gebruiksvriendelijk pakketbeheerprogramma voor Mac OS X en **Node.js**.

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. Voer de volgende opdrachten voor het maken van een projectmap `mysqlnodejs` en installeer het pakket mysql2 in die map.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. Controleer de installatie door de `npm list`-uitvoertekst voor `mysql2@1.3.6` te controleren. Het versienummer kan variëren als nieuwe patches worden vrijgegeven.

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure Database voor MySQL. U hebt de volledig gekwalificeerde servernaam en aanmeldingsreferenties nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het linkerdeelvenster op **Alle resources** en zoek vervolgens naar de server die u hebt gemaakt (bijvoorbeeld **myserver4demo**).
3. Klik op de servernaam **myserver4demo**.
4. Selecteer de server **eigenschappen** pagina en maak een notitie van de **servernaam** en **aanmeldingsnaam van Server-beheerder**.
 ![Azure Database voor MySQL - Aanmeldgegevens van de serverbeheerder](./media/connect-nodejs/1_server-properties-name-login.png)
5. Als u uw aanmeldingsgegevens server bent vergeten, gaat u naar de **overzicht** pagina om de aanmeldingsnaam voor Server-beheerder weer te geven en zo nodig het wachtwoord opnieuw instellen.

## <a name="running-the-javascript-code-in-nodejs"></a>De JavaScript-code in Node.js uitvoeren
1. Plak de JavaScript-code in tekstbestanden en vervolgens opslaan in een projectmap met bestand extensie .js (zoals C:\nodejsmysql\createtable.js of /home/username/nodejsmysql/createtable.js).
2. Start de opdrachtprompt of bash-shell en wijzig vervolgens de directory in de projectmap `cd nodejsmysql`.
3. Typ de knooppuntopdracht, gevolgd door de bestandsnaam `node createtable.js` om de toepassing uit te voeren.
4. In Windows moet u als de knooppunttoepassing zich niet in uw omgevingsvariabelepad bevindt mogelijk het volledige pad gebruiken om de knooppunttoepassing te starten, zoals `"C:\Program Files\nodejs\node.exe" createtable.js`

## <a name="connect-create-table-and-insert-data"></a>Verbinden, tabel maken en gegevens invoegen
De volgende code gebruiken om verbinding te en de gegevens laden met behulp van **CREATE TABLE** en **INSERT INTO** SQL-instructies.

De methode [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) wordt gebruikt om een koppeling te maken met de MySQL-server. De functie [connect()](https://github.com/mysqljs/mysql#establishing-connections) wordt gebruikt om verbinding met de server te maken. De functie [query()](https://github.com/mysqljs/mysql#performing-queries) wordt gebruikt om de SQL-query uit te voeren op de MySQL-database. 

Vervang de parameters `host`, `user`, `password` en `database` door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
    if (err) { 
        console.log("!!! Cannot connect !!! Error:");
        throw err;
    }
    else
    {
       console.log("Connection established.");
           queryDatabase();
    }   
});

function queryDatabase(){
       conn.query('DROP TABLE IF EXISTS inventory;', function (err, results, fields) { 
            if (err) throw err; 
            console.log('Dropped inventory table if existed.');
        })
       conn.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);', 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Created inventory table.');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['banana', 150], 
            function (err, results, fields) {
                if (err) throw err;
            else console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['orange', 154], 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['apple', 100], 
        function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.end(function (err) { 
        if (err) throw err;
        else  console.log('Done.') 
        });
};
```

## <a name="read-data"></a>Gegevens lezen
De volgende code gebruiken om verbinding te en de gegevens niet lezen via een **Selecteer** SQL-instructie. 

De methode [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) wordt gebruikt om een koppeling te maken met de MySQL-server. De methode [connect()](https://github.com/mysqljs/mysql#establishing-connections) wordt gebruikt om verbinding met de server te maken. De methode [query()](https://github.com/mysqljs/mysql#performing-queries) wordt gebruikt om de SQL-query uit te voeren op de MySQL-database. De resultatenmatrix wordt gebruikt om de resultaten van de query op te slaan.

Vervang de parameters `host`, `user`, `password` en `database` door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            readData();
        }   
    });

function readData(){
        conn.query('SELECT * FROM inventory', 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Selected ' + results.length + ' row(s).');
                for (i = 0; i < results.length; i++) {
                    console.log('Row: ' + JSON.stringify(results[i]));
                }
                console.log('Done.');
            })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Closing connection.') 
        });
};
```

## <a name="update-data"></a>Gegevens bijwerken
De volgende code gebruiken om verbinding te en de gegevens niet lezen via een **UPDATE** SQL-instructie. 

De methode [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) wordt gebruikt om een koppeling te maken met de MySQL-server. De methode [connect()](https://github.com/mysqljs/mysql#establishing-connections) wordt gebruikt om verbinding met de server te maken. De methode [query()](https://github.com/mysqljs/mysql#performing-queries) wordt gebruikt om de SQL-query uit te voeren op de MySQL-database. 

Vervang de parameters `host`, `user`, `password` en `database` door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            updateData();
        }   
    });

function updateData(){
       conn.query('UPDATE inventory SET quantity = ? WHERE name = ?', [200, 'banana'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Updated ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="delete-data"></a>Gegevens verwijderen
De volgende code gebruiken om verbinding te en de gegevens niet lezen via een **verwijderen** SQL-instructie. 

De methode [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) wordt gebruikt om een koppeling te maken met de MySQL-server. De methode [connect()](https://github.com/mysqljs/mysql#establishing-connections) wordt gebruikt om verbinding met de server te maken. De methode [query()](https://github.com/mysqljs/mysql#performing-queries) wordt gebruikt om de SQL-query uit te voeren op de MySQL-database. 

Vervang de parameters `host`, `user`, `password` en `database` door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            deleteData();
        }   
    });

function deleteData(){
       conn.query('DELETE FROM inventory WHERE name = ?', ['orange'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Deleted ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./concepts-migrate-import-export.md)
