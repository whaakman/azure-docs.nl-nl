---
title: Verbinding maken met Azure SQL Database via Node.js | Microsoft Docs
description: Dit is een Node.js-codevoorbeeld dat u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit Azure SQL Database.
services: sql-database
documentationcenter: 
author: LuisBosquez
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: lbosq
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: a7b1a504c6ae7850e5f986895adcf4d92fd5adc1
ms.contentlocale: nl-nl
ms.lasthandoff: 06/15/2017


---
<a id="azure-sql-database-use-nodejs-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Database: Node.js gebruiken om verbinding te maken en query's uit te voeren op gegevens

In deze Quick Start ziet u hoe u verbinding maakt met een Azure SQL-database met [Node.js](https://nodejs.org/en/) en vervolgens Transact-SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen vanaf Windows-, Ubuntu Linux- en Mac-platforms.

<a id="prerequisites" class="xliff"></a>

## Vereisten

In deze Quick Start wordt dit gebruikt als basis voor het maken van de resources die u hebt gemaakt in een van deze Quick Starts:

- [Database maken - Portal](sql-database-get-started-portal.md)
- [Database maken - CLI](sql-database-get-started-cli.md)
- [Database maken - PowerShell](sql-database-get-started-powershell.md)

<a id="install-nodejs" class="xliff"></a>

## Node.js installeren 

Voor de stappen in dit gedeelte wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met Node.js, maar geen ervaring hebt met het werken met Azure SQL Database. Als u niet bekend bent met ontwikkelen met Node.js, gaat u naar [Een app maken met SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) en selecteert u **Node.js**. Selecteer vervolgens uw besturingssysteem.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Voer de volgende opdrachten in om **brew** te installeren, een gebruiksvriendelijk pakketbeheerprogramma voor Mac OS X en **Node.js**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install node
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Voer de volgende opdrachten in om **Node.js** en **npm**, het pakketbeheerprogramma voor Node.js, te installeren.

```bash
sudo apt-get install -y nodejs npm
```

<a id="windows" class="xliff"></a>

### **Windows**
Ga naar de [downloadpagina van Node.js](https://nodejs.org/en/download/) en selecteer het gewenste Windows-installatieprogramma.


<a id="install-the-tedious-sql-server-driver-for-nodejs" class="xliff"></a>

## Het SQL Server-stuurprogramma Tedious installeren voor Node.js
Het aanbevolen stuurprogramma voor Node.js is **[tedious](https://github.com/tediousjs/tedious)**. Tedious is een open source-initiatief dat Microsoft aanbiedt voor Node.js-toepassingen op elk platform. Voor deze zelfstudie hebt u een lege map nodig voor uw code en de `npm`-afhankelijkheden die worden geïnstalleerd.

Voor de installatie van het **tedious**-stuurprogramma voert u de volgende opdracht in uw map uit:

```cmd
npm install tedious
```

<a id="get-connection-information" class="xliff"></a>

## Verbindingsgegevens ophalen

Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure SQL-database. U hebt de volledig gekwalificeerde servernaam, databasenaam en aanmeldingsgegevens in de volgende procedures nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. Op de pagina **Overzicht** voor de database controleert u de volledig gekwalificeerde servernaam zoals in de onderstaande afbeelding wordt weergegeven. U kunt de cursor boven de servernaam houden om de optie **Klik om te kopiëren** naar boven te halen. 

   ![servernaam](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Als u de aanmeldingsgegevens voor uw Azure SQL Database-server bent vergeten, gaat u naar de SQL Database-serverpagina om de beheerdersnaam voor de server weer te geven en, indien nodig, het wachtwoord opnieuw in te stellen.
    
<a id="select-data" class="xliff"></a>

## Gegevens selecteren

Gebruik de volgende code om in uw Azure SQL-database te zoeken naar de 20 populairste producten per categorie. Importeer eerst de verbindings- en aanvraagklasse voor het stuurprogramma vanuit de Tedious-stuurprogrammabibliotheek. Maak daarna het configuratieobject en vervang de variabelen **gebruikersnaam**, **wachtwoord**, **server** en **database** door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. Maak een `Connection`-object met het opgegeven `config`-object. Definieer vervolgens de retouraanroep voor de `connect`-gebeurtenis van het `connection`-object om de functie `queryDatabase()` uit te voeren.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}
var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        queryDatabase()
    }
});

function queryDatabase(){
    console.log('Reading rows from the Table...');

    // Read all rows from table
    request = new Request(
        "SELECT TOP 1 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
        }
    );
    
    request.on('row', function(columns) {
        columns.forEach(function(column) {
            console.log("%s\t%s", column.metadata.colName, column.value);
        });
    });

    connection.execSql(request);
}
```

<a id="insert-data-into-the-database" class="xliff"></a>

## Gegevens invoegen in de database
Gebruik de volgende code om een nieuw product in te voegen in de tabel SalesLT.Product. Gebruik hiervoor de functie `insertIntoDatabase()` met de Transact-SQL-instructie [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Vervang de variabelen **gebruikersnaam**, **wachtwoord**, **server** en **database** door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. 

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        insertIntoDatabase()
    }
});

function insertIntoDatabase(){
    console.log("Inserting a brand new product into database...");
    request = new Request(
        "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) inserted');
        }
    );
    connection.execSql(request);
}
```

<a id="update-data-in-the-database" class="xliff"></a>

## Gegevens in de database bijwerken
Gebruik de volgende code om het nieuwe product te verwijderen dat u eerder hebt toegevoegd. Gebruik de functie `updateInDatabase()` en de Transact-SQL-instructie [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Vervang de variabelen **gebruikersnaam**, **wachtwoord**, **server** en **database** door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. In dit voorbeeld wordt de productnaam gebruikt die in het vorige voorbeeld is ingevoegd.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        updateInDatabase()
    }
});

function updateInDatabase(){
    console.log("Updating the price of the brand new product in database...");
    request = new Request(
        "UPDATE SalesLT.Product SET ListPrice = 50 WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) updated');
        }
    );
    connection.execSql(request);
}
```

<a id="delete-data-from-the-database" class="xliff"></a>

## Gegevens uit de database verwijderen
Gebruik de volgende code om gegevens uit de database te verwijderen. Vervang de variabelen **gebruikersnaam**, **wachtwoord**, **server** en **database** door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. Gebruik deze keer een **DELETE-instructie** in de functie `deleteFromDatabase()`. In dit voorbeeld wordt ook de productnaam gebruikt die in het vorige voorbeeld is ingevoegd.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        deleteFromDatabase()
    }
});

function deleteFromDatabase(){
    console.log("Deleting the brand new product in database...");
    request = new Request(
        "DELETE FROM SalesLT.Product WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
        }
    );
    connection.execSql(request);
}
```


<a id="next-steps" class="xliff"></a>

## Volgende stappen
- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [Microsoft-stuurprogramma Node.js voor SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)
- [Verbinding maken en query's uitvoeren met SSMS](sql-database-connect-query-ssms.md)
- [Verbinding maken en query's uitvoeren met Visual Studio Code](sql-database-connect-query-vscode.md).



