---
title: Node.js gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: Node.js gebruiken om een programma te maken dat is verbonden met een Azure SQL-database, en een query voor deze database uitvoeren met behulp van T-SQL-instructies.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: nodejs
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/26/2018
ms.openlocfilehash: c6e518f644e8fc43db58ef481312f44b2071a0dc
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194301"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>Snelstart: Node.js gebruiken om een query uit te voeren voor een Azure SQL-database

In dit artikel ziet u hoe u [Node.js](https://nodejs.org) gebruikt om verbinding te maken met een Azure SQL-database. Vervolgens kunt u T-SQL-instructies gebruiken om een query uit te voeren voor de gegevens.

## <a name="prerequisites"></a>Vereisten

Zorg dat u aan de volgende vereisten voldoet als u dit voorbeeld wilt uitvoeren:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Node.js-software voor uw besturingssysteem:

  - **MacOS**: installeer Homebrew en Node.js, en installeer vervolgens het ODBC-stuurprogramma en SQLCMD. Zie [Stap 1.2 en 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
  
  - **Ubuntu**: installeer Node.js en installeer vervolgens het ODBC-stuurprogramma en SQLCMD. Zie [Stap 1.2 en 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
  
  - **Windows**: installeer Chocolatey en Node.js, en installeer vervolgens het ODBC-stuurprogramma en SQLCMD. Zie [Stap 1.2 en 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="get-database-connection"></a>Verbinding maken met de database

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

> [!IMPORTANT]
> U moet een firewallregel hebben ingesteld voor het openbare IP-adres van de computer waarop u deze zelfstudie uitvoert. Als u een andere computer gebruikt of een ander openbaar IP-adres hebt, maakt u een [firewallregel op serverniveau met behulp van de Azure-portal](sql-database-get-started-portal-firewall.md).

## <a name="create-the-project"></a>Het project maken

Open een opdrachtprompt en maak een map met de naam *sqltest*. Navigeer naar de map die u hebt gemaakt, en voer de volgende opdracht uit:

  ```bash
  npm init -y
  npm install tedious
  npm install async
  ```

## <a name="add-code-to-query-database"></a>Code toevoegen om een query uit te voeren op de database

1. Maak in uw favoriete teksteditor een nieuw bestand *sqltest.js*.

1. Vervang de inhoud ervan door de volgende code. Voeg vervolgens de juiste waarden toe voor uw server, database, gebruiker en wachtwoord.

    ```js
    var Connection = require('tedious').Connection;
    var Request = require('tedious').Request;

    // Create connection to database
    var config =
    {
        userName: 'your_username', // update me
        password: 'your_password', // update me
        server: 'your_server.database.windows.net', // update me
        options:
        {
            database: 'your_database', //update me
            encrypt: true
        }
    }
    var connection = new Connection(config);

    // Attempt to connect and execute queries if connection goes through
    connection.on('connect', function(err)
        {
            if (err)
            {
                console.log(err)
            }
            else
            {
                queryDatabase()
            }
        }
    );

    function queryDatabase()
    {
        console.log('Reading rows from the Table...');

        // Read all rows from table
        var request = new Request(
            "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc "
                + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
            function(err, rowCount, rows)
            {
                console.log(rowCount + ' row(s) returned');
                process.exit();
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

> [!NOTE]
> In het codevoorbeeld wordt de voorbeelddatabase **AdventureWorksLT** voor Azure SQL gebruikt.

## <a name="run-the-code"></a>De code uitvoeren

1. Voer het programma uit bij de opdrachtprompt.

    ```bash
    node sqltest.js
    ```

1. Controleer de bovenste 20 rijen die worden geretourneerd en sluit het toepassingsvenster.

## <a name="next-steps"></a>Volgende stappen

- [Microsoft-stuurprogramma Node.js voor SQL Server](/sql/connect/node-js/node-js-driver-for-sql-server)

- Verbinding maken en query's uitvoeren in Windows/Linux/macOS met [.NET core](sql-database-connect-query-dotnet-core.md), [Visual Studio Code](sql-database-connect-query-vscode.md) of [SSMS](sql-database-connect-query-ssms.md) (alleen Windows)

- [Aan de slag met .NET Core in Windows/Linux/macOS met behulp van de opdrachtregel](/dotnet/core/tutorials/using-with-xplat-cli)

- Uw eerste Azure SQL-database ontwerpen met [.NET](sql-database-design-first-database-csharp.md) of [SSMS](sql-database-design-first-database.md)
