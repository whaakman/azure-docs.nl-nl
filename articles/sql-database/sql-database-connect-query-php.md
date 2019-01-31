---
title: PHP gebruiken om een query uit te voeren op een Azure SQL-database | Microsoft Docs
description: PHP gebruiken om een programma te maken dat is verbonden met een Azure SQL-database, en een query op deze database uitvoeren met behulp van T-SQL-instructies.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: php
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/28/2018
ms.openlocfilehash: b3fe6e0249143b27cb763401a8d328922ed1fe99
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173918"
---
# <a name="quickstart-use-php-to-query-an-azure-sql-database"></a>Snelstartgids: PHP gebruiken om een query uit te voeren voor een Azure SQL-database

In dit artikel ziet u hoe u [PHP](http://php.net/manual/en/intro-whatis.php) gebruikt om verbinding te maken met een Azure SQL-database. Vervolgens kunt u T-SQL-instructies gebruiken om een query uit te voeren voor de gegevens.

## <a name="prerequisites"></a>Vereisten

Zorg dat u aan de volgende vereisten voldoet als u dit voorbeeld wilt uitvoeren:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- U moet aan PHP verwante software hebben geïnstalleerd voor het besturingssysteem:

    - **MacOS**: installeer PHP, het ODBC-stuurprogramma en installeer vervolgens het PHP-stuurprogramma voor SQL Server. Zie [stap 1, 2 en 3](/sql/connect/php/installation-tutorial-linux-mac).

    - **Linux**: installeer PHP, het ODBC-stuurprogramma en installeer vervolgens het PHP-stuurprogramma voor SQL Server. Zie [stap 1, 2 en 3](/sql/connect/php/installation-tutorial-linux-mac).

    - **Windows**: installeer PHP voor IIS Express en Chocolatey, en installeer vervolgens het ODBC-stuurprogramma en SQLCMD. Zie [Stap 1.2 en 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).

## <a name="get-database-connection"></a>Verbinding maken met de database

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="add-code-to-query-database"></a>Code toevoegen om een query uit te voeren op de database

1. Maak een nieuw bestand in uw favoriete teksteditor *sqltest.php*.  

1. Vervang de inhoud ervan door de volgende code. Voeg vervolgens de juiste waarden toe voor uw server, database, gebruiker en wachtwoord.

   ```PHP
   <?php
       $serverName = "your_server.database.windows.net"; // update me
       $connectionOptions = array(
           "Database" => "your_database", // update me
           "Uid" => "your_username", // update me
           "PWD" => "your_password" // update me
       );
       //Establishes the connection
       $conn = sqlsrv_connect($serverName, $connectionOptions);
       $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
            FROM [SalesLT].[ProductCategory] pc
            JOIN [SalesLT].[Product] p
            ON pc.productcategoryid = p.productcategoryid";
       $getResults= sqlsrv_query($conn, $tsql);
       echo ("Reading data from table" . PHP_EOL);
       if ($getResults == FALSE)
           echo (sqlsrv_errors());
       while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
        echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
       }
       sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>De code uitvoeren

1. Voer de app uit vanaf de opdrachtprompt.

   ```bash
   php sqltest.php
   ```

1. Controleer de bovenste 20 rijen die worden geretourneerd en sluit het app-venster.

## <a name="next-steps"></a>Volgende stappen

- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)

- [Microsoft PHP-stuurprogramma's voor SQL Server](https://github.com/Microsoft/msphpsql/)

- [Problemen melden of vragen stellen](https://github.com/Microsoft/msphpsql/issues)

- [Voorbeeld van pogingslogica: Flexibel verbinding maken met SQL via PHP](/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)
