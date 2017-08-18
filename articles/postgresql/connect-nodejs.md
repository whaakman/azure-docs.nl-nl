---
title: Vanuit Node.js verbinding maken met Azure Database voor PostgreSQL | Microsoft Docs
description: Deze snelstartgids bevat een voorbeeld van Node.js-code die u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit een Azure Database voor PostgreSQL.
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.custom: mvc
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 06/23/2017
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: ddc364f2a0b8a8bb0a4a2c3a563c007470415991
ms.contentlocale: nl-nl
ms.lasthandoff: 08/10/2017

---

# <a name="azure-database-for-postgresql-use-nodejs-to-connect-and-query-data"></a>Azure Database voor PostgreSQL: Node.js gebruiken om verbinding te maken en query's uit te voeren op gegevens
In deze snelstartgids ziet u hoe u vanuit de platformen Windows, Ubuntu Linux, en Mac met behulp van [Node.js](https://nodejs.org/) verbinding maakt met een Azure Database voor PostgreSQL. U ziet hier hoe u SQL-instructies gebruikt om gegevens in de database op te vragen, in te voegen, bij te werken en te verwijderen. In de stappen van dit artikel wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met behulp van Node.js, maar geen ervaring hebt met het werken met Azure Database voor PostgreSQL.

## <a name="prerequisites"></a>Vereisten
In deze snelstartgids worden de resources die in een van deze handleidingen zijn gemaakt, als uitgangspunt gebruikt:
- [Database maken - Portal](quickstart-create-server-database-portal.md)
- [Database maken - CLI](quickstart-create-server-database-azure-cli.md)

U moet ook het volgende doen:
- [Node.js](https://nodejs.org) installeren

## <a name="install-pg-client"></a>pg-client installeren
Installeer [pg](https://www.npmjs.com/package/pg), een niet-blokkerende JavaScript-client voor Node.js die handig is om verbinding maken met en query's uit te voeren in PostgreSQL.

Voer de Node Package Manager (NPM) voor JavaScript uit vanaf de opdrachtregel om de pg-client te installeren.
```bash
npm install pg
```

Controleer de installatie door de geïnstalleerde pakketten weer te geven.
```bash
npm list
```
De uitvoer van de lijstopdracht bevestigt de versie van elk onderdeel. 
```
`-- pg@6.2.3
  +-- buffer-writer@1.0.1
  +-- packet-reader@0.3.1
etc...
```

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure Database voor PostgreSQL. U hebt de volledig gekwalificeerde servernaam en aanmeldingsreferenties nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. In het menu links in Azure Portal klikt u op **Alle resources** en zoekt u naar de server die u zojuist hebt gemaakt: **mypgserver-20170401**.
3. Klik op de servernaam **mypgserver-20170401**.
4. Selecteer de pagina **Overzicht** van de server. Noteer de **servernaam** en de **gebruikersnaam van de serverbeheerder**.
 ![Azure Database voor PostgreSQL - Aanmeldgegevens van de serverbeheerder](./media/connect-nodejs/1-connection-string.png)
5. Als u uw aanmeldingsgegevens voor de server bent vergeten, gaat u naar de pagina **Overzicht** om de aanmeldingsnaam van de serverbeheerder weer te geven en indien nodig het wachtwoord opnieuw in te stellen.

## <a name="running-the-javascript-code-in-nodejs"></a>De JavaScript-code in Node.js uitvoeren
U kunt Node.js starten vanuit de Bash-shell of opdrachtprompt van Windows door `node` in te voeren. Voer vervolgens de JavaScript-voorbeeldcode interactief uit door deze te kopiëren en in de prompt te plakken. U kunt de JavaScript-code ook in een tekstbestand opslaan en `node filename.js` starten met de bestandsnaam als parameter om de code uit te voeren.

## <a name="connect-create-table-and-insert-data"></a>Verbinden, tabel maken en gegevens invoegen
Gebruik de volgende code om verbinding te maken en de gegevens te laden met de SQL-instructies **CREATE TABLE** EN **INSERT INTO**.
Het object [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) wordt gebruikt voor interactie met de PostgreSQL-server. De functie [pg.Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wordt gebruikt om verbinding met de server te maken. De functie [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wordt gebruikt om de SQL-query uit te voeren op de PostgreSQL-database. 

Vervang de parameters host, dbname, user en password door de waarden die u hebt opgegeven tijdens het maken van de server en database.

```javascript
const pg = require('pg');

const config = {
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: '<name_of_database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DROP TABLE IF EXISTS inventory;
        CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
        INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
        INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
        INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    `;

    client
        .query(query)
        .then(() => {
            console.log('Table created successfully!');
            client.end(console.log('Closed client connection'));
        })
        .catch(err => console.log(err))
        .then(() => {
            console.log('Finished execution, exiting now');
            process.exit();
        });
}
```

## <a name="read-data"></a>Gegevens lezen
Gebruik de volgende code om verbinding te maken en de gegevens te lezen met de SQL-instructie **SELECT**. Het object [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) wordt gebruikt voor interactie met de PostgreSQL-server. De functie [pg.Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wordt gebruikt om verbinding met de server te maken. De functie [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wordt gebruikt om de SQL-query uit te voeren op de PostgreSQL-database. 

Vervang de parameters host, dbname, user en password door de waarden die u hebt opgegeven tijdens het maken van de server en database. 

```javascript
const pg = require('pg');

const config = {
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: '<name_of_database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else { queryDatabase(); }
});

function queryDatabase() {
  
    console.log(`Running query to PostgreSQL server: ${config.host}`);

    const query = 'SELECT * FROM inventory;';

    client.query(query)
        .then(res => {
            const rows = res.rows;

            rows.map(row => {
                console.log(`Read: ${JSON.stringify(row)}`);
            });

            process.exit();
        })
        .catch(err => {
            console.log(err);
        });
}
```

## <a name="update-data"></a>Gegevens bijwerken
Gebruik de volgende code om verbinding te maken en de gegevens te lezen met de SQL-instructie **UPDATE**. Het object [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) wordt gebruikt voor interactie met de PostgreSQL-server. De functie [pg.Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wordt gebruikt om verbinding met de server te maken. De functie [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wordt gebruikt om de SQL-query uit te voeren op de PostgreSQL-database. 

Vervang de parameters host, dbname, user en password door de waarden die u hebt opgegeven tijdens het maken van de server en database. 

```javascript
const pg = require('pg');

const config = {
    host: 'mypgserver-20170401.postgres.database.azure.com',
    user: 'mylogin@mypgserver-20170401',
    password: '<server_admin_password>',
    database: '<name_of_database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `UPDATE inventory 
                   SET quantity= 1000 WHERE name='banana';`;

    client
        .query(query)
        .then(() => {
            console.log('Update completed succesfully!');
            client.end(console.log('Closed client connection'));
        })
        .catch(err => console.log(err))
        .then(() => {
            console.log('Finished execution, exiting now');
            process.exit();
        });
}
```

## <a name="delete-data"></a>Gegevens verwijderen
Gebruik de volgende code om verbinding te maken en de gegevens te lezen met de SQL-instructie **DELETE**. Het object [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) wordt gebruikt voor interactie met de PostgreSQL-server. De functie [pg.Client.Connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) wordt gebruikt om verbinding met de server te maken. De functie [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) wordt gebruikt om de SQL-query uit te voeren op de PostgreSQL-database. 

Vervang de parameters host, dbname, user en password door de waarden die u hebt opgegeven tijdens het maken van de server en database. 

```javascript
const pg = require('pg');

const config = {
  host: '<your-db-server-name>.postgres.database.azure.com',
  // Do not hard code your username and password.
  // Consider using Node environment variables.
  user: '<your-db-username>',     
  password: '<your-password>',
  database: '<name-of-database>',
  port: 5432,
  ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
  if (err) {
    throw err;
  } else {
    queryDatabase();
  }
});

function queryDatabase() {
  const query = `
    DELETE FROM inventory 
    WHERE name = 'apple';
  `;

  client
    .query(query)
    .then(result => {
      console.log('Delete completed');
      console.log(`Rows affected: ${result.rowCount}`);
    })
    .catch(err => {
      console.log(err);
      throw err;
    });
}
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een database migreren met behulp van Exporteren en importeren](./howto-migrate-using-export-and-import.md)

