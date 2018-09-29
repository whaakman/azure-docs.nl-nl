---
title: PHP gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: In dit onderwerp ziet u hoe u PHP gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u een query voor deze database uitvoert met behulp van Transact-SQL-instructies.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: php
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 2fc7b961df003fe05bc4ad4e49b9debb74c952fd
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063231"
---
# <a name="use-php-to-query-an-azure-sql-database"></a>PHP gebruiken om een query uit te voeren voor een Azure SQL-database

In deze snelstart wordt gedemonstreerd hoe u [PHP](http://php.net/manual/en/intro-whatis.php) gebruikt om een programma te maken dat verbinding maakt met een Azure SQL-database, en hoe u Transact-SQL-instructies gebruikt om gegevens te doorzoeken.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Een [firewallregel op serverniveau](sql-database-get-started-portal-firewall.md) voor het openbare IP-adres van de computer die u gebruikt voor deze snelstart.

- U hebt PHP en verwante software voor uw besturingssysteem geïnstalleerd:

    - **MacOS**: installeer Homebrew en PHP, installeer het ODBC-stuurprogramma en SQLCMD, en installeer vervolgens het PHP-stuurprogramma voor SQL Server. Zie [stap 1.2, 1.3 en 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/mac/).
    - **Ubuntu**: installeer PHP en andere vereiste pakketten, en installeer vervolgens het PHP-stuurprogramma voor SQL Server. Zie [stap 1.2 en 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).
    - **Windows**: installeer de nieuwste versie van PHP voor IIS Express, de nieuwste versie van Microsoft-stuurprogramma's voor SQL Server in IIS Express, Chocolatey, het ODBC-stuurprogramma en SQLCMD. Zie [stap 1.2 en 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).    

## <a name="sql-server-connection-information"></a>SQL Server-verbindingsgegevens

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="insert-code-to-query-sql-database"></a>Code invoegen om een query uit te voeren voor een SQL-database

1. Maak een nieuw bestand in uw favoriete teksteditor **sqltest.php**.  

2. Vervang de inhoud door de volgende code en voeg de juiste waarden toe voor de server, de database, de gebruiker en het wachtwoord.

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
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

1. Voer bij de opdrachtprompt de volgende opdrachten uit:

   ```php
   php sqltest.php
   ```

2. Controleer of de bovenste 20 rijen worden geretourneerd, en sluit vervolgens het toepassingsvenster.

## <a name="next-steps"></a>Volgende stappen
- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [Microsoft PHP-stuurprogramma's voor SQL Server](https://github.com/Microsoft/msphpsql/)
- [Problemen melden of vragen stellen](https://github.com/Microsoft/msphpsql/issues)
- [Voorbeeld logica voor opnieuw proberen: flexibel verbinding maken met SQL via PHP][step-4-connect-resiliently-to-sql-with-php-p42h]


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

