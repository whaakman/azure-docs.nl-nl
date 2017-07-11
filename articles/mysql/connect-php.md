---
title: Via PHP verbinding maken met Azure Database voor MySQL | Microsoft Docs
description: Deze snelstartgids bevat enkele voorbeelden van PHP-code die u kunt gebruiken om verbinding te maken met en gegevens op te vragen uit een Azure Database voor MySQL.
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 06/26/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: e3ac0e1813022d1b3544fc2c784719d6c98a0cf3
ms.contentlocale: nl-nl
ms.lasthandoff: 06/28/2017

---

<a id="azure-database-for-mysql-use-php-to-connect-and-query-data" class="xliff"></a>

# Azure Database voor MySQL: PHP gebruiken om verbinding te maken en gegevens op te vragen
In deze snelstartgids ziet u hoe u met behulp van een [PHP](http://php.net/manual/intro-whatis.php)-toepassing verbinding maakt met een Azure Database voor MySQL. U ziet hier hoe u SQL-instructies gebruikt om gegevens in de database op te vragen, in te voegen, bij te werken en te verwijderen. In dit artikel wordt ervan uitgegaan dat u bekend bent met het ontwikkelen met PHP, maar geen ervaring hebt met het werken met Azure Database voor MySQL.

<a id="prerequisites" class="xliff"></a>

## Vereisten
In deze snelstartgids worden de resources die in een van deze handleidingen zijn gemaakt, als uitgangspunt gebruikt:
- [Een Azure-database voor een MySQL-server maken met behulp van Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Een Azure-database voor een MySQL-server maken met behulp van Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

<a id="install-php" class="xliff"></a>

## PHP installeren
Installeer PHP op uw eigen server of maak een Azure-[web-app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) die PHP omvat.

<a id="macos" class="xliff"></a>

### MacOS
- [PHP versie 7.1.4](http://php.net/downloads.php) downloaden
- PHP installeren en de [PHP-handleiding](http://php.net/manual/install.macosx.php) bekijken voor verdere configuratie

<a id="linux-ubuntu" class="xliff"></a>

### Linux (Ubuntu)
- [PHP versie 7.1.4 non-thread safe (x64)](http://php.net/downloads.php) downloaden
- PHP installeren en de [PHP-handleiding](http://php.net/manual/install.unix.php) bekijken voor verdere configuratie

<a id="windows" class="xliff"></a>

### Windows
- [PHP versie 7.1.4 non-thread safe (x64)](http://windows.php.net/download#php-7.1) downloaden
- PHP installeren en de [PHP-handleiding](http://php.net/manual/install.windows.php) bekijken voor verdere configuratie

<a id="get-connection-information" class="xliff"></a>

## Verbindingsgegevens ophalen
Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure Database voor MySQL. U hebt de volledig gekwalificeerde servernaam en aanmeldingsreferenties nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links in Azure Portal op **Alle resources** en zoek de server die u hebt gemaakt (bijvoorbeeld **myserver4demo**).
3. Klik op de servernaam.
4. Selecteer de pagina **Eigenschappen** van de server. Noteer de **servernaam** en de **gebruikersnaam van de serverbeheerder**.
 ![Naam van Azure Database voor MySQL-server](./media/connect-php/1_server-properties-name-login.png)
5. Als u uw aanmeldingsgegevens voor de server bent vergeten, gaat u naar de pagina **Overzicht** om de aanmeldingsnaam van de serverbeheerder weer te geven en indien nodig het wachtwoord opnieuw in te stellen.

<a id="connect-and-create-a-table" class="xliff"></a>

## Verbinding maken en een tabel maken
Gebruik de volgende code om verbinding te maken en een tabel te maken met de SQL-instructie **CREATE TABLE**. 

Voor de code wordt gebruikgemaakt van de klasse **MySQL Improved extension** (mysqli) die deel uitmaakt van PHP. Met de code worden de methoden [mysqli_init](http://php.net/manual/mysqli.init.php) en [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) aangeroepen om verbinding te maken met MySQL. Daarna wordt de methode [mysqli_query](http://php.net/manual/mysqli.query.php) aangeroepen op de query uit te voeren. Vervolgens wordt methode [mysqli_close](http://php.net/manual/mysqli.close.php) aangeroepen om de verbinding te sluiten.

Vervang de parameters Host, Gebruikersnaam, Wachtwoord en db_name door uw eigen waarden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

// Run the create table query
if (mysqli_query($conn, '
CREATE TABLE Products (
`Id` INT NOT NULL AUTO_INCREMENT ,
`ProductName` VARCHAR(200) NOT NULL ,
`Color` VARCHAR(50) NOT NULL ,
`Price` DOUBLE NOT NULL ,
PRIMARY KEY (`Id`)
);
')) {
printf("Table created\n");
}

//Close the connection
mysqli_close($conn);
?>
```

<a id="insert-data" class="xliff"></a>

## Gegevens invoegen
Gebruik de volgende code om verbinding te maken en de gegevens in te voegen met de SQL-instructie **INSERT**.

Voor de code wordt gebruikgemaakt van de klasse **MySQL Improved extension** (mysqli) die deel uitmaakt van PHP. Met de code wordt gebruikgemaakt van de methode [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) om een invoeginstructie te maken. Daarna worden de parameters van elke ingevoegde kolomwaarde verbonden met de methode [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Met de code wordt de instructie uitgevoerd met de methode [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php). Daarna wordt de instructie gesloten met de methode [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Vervang de parameters Host, Gebruikersnaam, Wachtwoord en db_name door uw eigen waarden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Create an Insert prepared statement and run it
$product_name = 'BrandNewProduct';
$product_color = 'Blue';
$product_price = 15.5;
if ($stmt = mysqli_prepare($conn, "INSERT INTO Products (ProductName, Color, Price) VALUES (?, ?, ?)")) {
mysqli_stmt_bind_param($stmt, 'ssd', $product_name, $product_color, $product_price);
mysqli_stmt_execute($stmt);
printf("Insert: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

// Close the connection
mysqli_close($conn);
?>
```

<a id="read-data" class="xliff"></a>

## Gegevens lezen
Gebruik de volgende code om verbinding te maken en de gegevens te lezen met de SQL-instructie **SELECT**.  Voor de code wordt gebruikgemaakt van de klasse **MySQL Improved extension** (mysqli) die deel uitmaakt van PHP. Met de code wordt gebruikgemaakt van de methode [mysqli_query](http://php.net/manual/mysqli.query.php) om de SQL-query uit te voeren. Er wordt gebruikgemaakt van de methode [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) om de resulterende rijen op te halen.

Vervang de parameters Host, Gebruikersnaam, Wachtwoord en db_name door uw eigen waarden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close the connection
mysqli_close($conn);
?>
```

<a id="update-data" class="xliff"></a>

## Gegevens bijwerken
Gebruik de volgende code om verbinding te maken en de gegevens bij te werken met de SQL-instructie **UPDATE**.

Voor de code wordt gebruikgemaakt van de klasse **MySQL Improved extension** (mysqli) die deel uitmaakt van PHP. Met de code wordt gebruikgemaakt van de methode [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) om een bijwerkinstructie te maken. Daarna worden de parameters van elke bijgewerkte kolomwaarde verbonden met de methode [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Met de code wordt de instructie uitgevoerd met de methode [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php). Daarna wordt de instructie gesloten met de methode [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Vervang de parameters Host, Gebruikersnaam, Wachtwoord en db_name door uw eigen waarden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close the connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


<a id="delete-data" class="xliff"></a>

## Gegevens verwijderen
Gebruik de volgende code om verbinding te maken en de gegevens te lezen met de SQL-instructie **DELETE**. 

Voor de code wordt gebruikgemaakt van de klasse **MySQL Improved extension** (mysqli) die deel uitmaakt van PHP. Met de code wordt gebruikgemaakt van de methode [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) om een verwijderinstructie te maken. Daarna worden de parameters verbonden voor het Where-component in de instructie. Hiervoor wordt de methode [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php) gebruikt. Met de code wordt de instructie uitgevoerd met de methode [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php). Daarna wordt de instructie gesloten met de methode [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Vervang de parameters Host, Gebruikersnaam, Wachtwoord en db_name door uw eigen waarden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close the connection
mysqli_close($conn);
?>
```

<a id="next-steps" class="xliff"></a>

## Volgende stappen
> [!div class="nextstepaction"]
> [Een PHP- en MySQL-web-app bouwen in Azure](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)

