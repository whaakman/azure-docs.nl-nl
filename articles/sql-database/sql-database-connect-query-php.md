---
title: Verbinding maken met Azure SQL Database via .PHP | Microsoft Docs
description: In dit artikel is een PHP-codevoorbeeld opgenomen dat u kunt gebruiken om verbinding te maken met Azure SQL Database en query&quot;s uit te voeren op gegevens.
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: meetb
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 5b90c9b49448c6bc225f9d6ba7227782b81fc5ed
ms.contentlocale: nl-nl
ms.lasthandoff: 06/15/2017


---
<a id="azure-sql-database-use-php-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Database: PHP gebruiken om verbinding te maken en query's uit te voeren op gegevens

In deze Quick Start ziet u hoe u [PHP](http://php.net/manual/en/intro-whatis.php) gebruikt om verbinding te maken met een Azure SQL-database en vervolgens Transact-SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen vanaf het Mac OS-, Ubuntu Linux- en Windows-platform.

<a id="prerequisites" class="xliff"></a>

## Vereisten

In deze Quick Start wordt dit gebruikt als basis voor het maken van de resources die u hebt gemaakt in een van deze Quick Starts:

- [Database maken - Portal](sql-database-get-started-portal.md)
- [Database maken - CLI](sql-database-get-started-cli.md)
- [Database maken - PowerShell](sql-database-get-started-powershell.md)

<a id="install-php-and-database-communications-software" class="xliff"></a>

## PHP en databasecommunicatiesoftware installeren

Voor de stappen in dit gedeelte wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met PHP, maar dat u geen ervaring hebt met het werken met Azure SQL Database. Als u niet bekend bent met ontwikkelen met PHP, gaat u naar [Een app bouwen met SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) en selecteert u **PHP**. Selecteer vervolgens uw besturingssysteem.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Open de terminal en voer de volgende opdrachten uit om **brew**, het **Microsoft ODBC-stuurprogramma voor Mac** en de **Microsoft PHP-stuurprogramma's voor SQL Server** te installeren. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql 
pecl install sqlsrv-4.1.7preview
pecl install pdo_sqlsrv-4.1.7preview
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Voer de volgende opdrachten uit om het **Microsoft ODBC-stuurprogramma voor Linux** en de **Microsoft PHP-stuurprogramma's voor SQL Server** te installeren.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev gcc g++ php-dev
sudo pecl install sqlsrv pdo_sqlsrv
sudo echo "extension= pdo_sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
sudo echo "extension= sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
```

<a id="windows" class="xliff"></a>

### **Windows**
- Installeer PHP 7.1.1 (x64) [met Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx?lang=). 
- Installeer [Microsoft ODBC Driver 13.1](https://www.microsoft.com/download/details.aspx?id=53339). 
- Download de niet-thread-veilige DLL-bestanden voor het [Microsoft PHP-stuurprogramma voor SQL Server](https://pecl.php.net/package/sqlsrv/4.1.6.1/windows) en plaats de binaire bestanden in de map PHP\v7.x\ext.
- Vervolgens bewerkt u het bestand php.ini (C:\Program Files\PHP\v7.1\php.ini) door de verwijzing naar het DLL-bestand toe te voegen. Bijvoorbeeld:
      
      extension=php_sqlsrv.dll
      extension=php_pdo_sqlsrv.dll

De DLL-bestanden moeten nu zijn geregistreerd bij PHP.

<a id="get-connection-information" class="xliff"></a>

## Verbindingsgegevens ophalen

Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure SQL-database. U hebt de volledig gekwalificeerde servernaam, databasenaam en aanmeldingsgegevens in de volgende procedures nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. Op de pagina **Overzicht** voor de database controleert u de volledig gekwalificeerde servernaam zoals in de onderstaande afbeelding wordt weergegeven. U kunt de cursor boven de servernaam houden om de optie **Klik om te kopiëren** naar boven te halen.  

   ![servernaam](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Als u de aanmeldingsgegevens voor de server bent vergeten, gaat u naar de SQL Database-serverpagina om de beheerdersnaam voor de server weer te geven en, indien nodig, het wachtwoord opnieuw in te stellen.     
    
<a id="select-data" class="xliff"></a>

## Gegevens selecteren
Gebruik de volgende code om op categorie een query uit te voeren voor de 20 populairste producten. Gebruik de functie [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) met de Transact-SQL-instructie [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql). De functie sqlsrv_query wordt gebruikt om een resultatenset voor een query op te halen uit een SQL-database. Deze functie accepteert een query en retourneert een resultatenset die met behulp van [sqlsrv_fetch_array()](http://php.net/manual/en/function.sqlsrv-fetch-array.php) kan worden herhaald. Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. 

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


<a id="insert-data" class="xliff"></a>

## Gegevens invoegen
Gebruik de volgende code om een nieuw product in te voegen in de tabel SalesLT.Product. Gebruik hiervoor de functie [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) met de Transact-SQL-instructie [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens. 

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
$tsql= "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";
$params = array("BrandNewProduct", "200989", "Blue", 75, 80, "7/1/2016");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) inserted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

<a id="update-data" class="xliff"></a>

## Gegevens bijwerken
Gebruik de volgende code om de gegevens in de Azure SQL-database bij te werken. Gebruik hiervoor de functie [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) met de Transact-SQL-instructie [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

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
$tsql= "UPDATE SalesLT.Product SET ListPrice =? WHERE Name = ?";
$params = array(50,"BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) updated" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

<a id="delete-data" class="xliff"></a>

## Gegevens verwijderen
Gebruik de volgende code om het nieuwe product te verwijderen dat u eerder hebt toegevoegd. Gebruik hiervoor de functie [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) met de Transact-SQL-instructie [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql). Vervang de parameters voor server, database, gebruikersnaam en wachtwoord door de waarden die u hebt opgegeven tijdens het maken van de database met de AdventureWorksLT-voorbeeldgegevens.

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
$tsql= "DELETE FROM SalesLT.Product WHERE Name = ?";
$params = array("BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) deleted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
```

<a id="next-steps" class="xliff"></a>

## Volgende stappen
- [Uw eerste Azure SQL-database ontwerpen](sql-database-design-first-database.md)
- [Microsoft PHP-stuurprogramma's voor SQL Server](https://github.com/Microsoft/msphpsql/)
- [Problemen melden/vragen stellen](https://github.com/Microsoft/msphpsql/issues)


