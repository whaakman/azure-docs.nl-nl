---
title: MySQL-stuurprogramma's en compatibiliteit van de hulpprogramma's voor beheer
description: In dit artikel beschrijft de MySQL-stuurprogramma's en beheerhulpprogramma's die compatibel met Azure voor MySQL-Database zijn.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 5fc13ef07b61feb9e9fdd73123a09daa61f6aaf3
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>MySQL-stuurprogramma's en compatibel zijn met Azure-Database voor de MySQL-beheerhulpprogramma 's
Dit artikel worden de stuurprogramma's en beheerhulpprogramma's die compatibel met Azure voor MySQL-Database zijn.

## <a name="mysql-drivers"></a>MySQL Drivers
Azure MySQL-Database maakt gebruik van de wereld populairste community edition van MySQL-database. Daarom is het compatibel is met een groot aantal programmeertalen en stuurprogramma's. Het doel is de ondersteuning van de drie meest recente versies MySQL-stuurprogramma's en inspanningen met auteurs van de open-source-community continu verbeteren de functionaliteit en bruikbaarheid van MySQL-stuurprogramma's gaan. Een lijst van stuurprogramma's die zijn getest en blijkt dat het niet compatibel met Azure-Database voor MySQL 5.6 en 5.7 vindt u in de volgende tabel:

| **Stuurprogramma** | **Koppelingen** | **Compatibele versies** | **Incompatibel versies** | **Opmerkingen** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | http://php.net/downloads.php | 5.5 5,6 7.x | 5.3 | Voor PHP 7.0 verbinding met SSL MySQLi, voegt u MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT in de verbindingstekenreeks. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO set: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` optie in op false.|
| .Net | [MySqlConnector on GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Installatiepakket van Nuget](https://www.nuget.org/packages/MySqlConnector/) | 0,27 en na | 0.26.5 en vóór | |
| Nodejs |  [MySQLjs on GitHub](https://github.com/mysqljs/mysql/releases) <br> Installatiepakket van NPM:<br> Voer `npm install mysql` van NPM | 2.15 | 2.14.1 en vóór | |
| GA | https://github.com/go-sql-driver/mysql/releases | 1.3 | 1.2 en vóór | Gebruik allowNativePasswords = true in de verbindingsreeks |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 en vóór | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1 2.0 1.6 | 1.5.5 en vóór | |

## <a name="management-tools"></a>Beheerhulpprogramma's
Het voordeel van compatibiliteit uitbreiden met database beheerprogramma's ook De bestaande hulpprogramma's moeten blijven werken met Azure Database voor MySQL, zolang de databasebewerkingen binnen de grenzen van de gebruikersmachtigingen werkt. Drie algemene database-beheerhulpprogramma's die zijn getest en blijkt dat het niet compatibel met Azure-Database voor MySQL 5.6 en 5.7 worden vermeld in de volgende tabel:

|                                     | **MySQL-Workbench 6.x en hoger** | **Navicat 12** | **PHPMyAdmin 4.x en hoger** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Maken, bijwerken, lezen, schrijven, verwijderen | X | X | X |
| SSL-verbinding | X | X | X |
| SQL-Query automatisch voltooien | X | X |  |
| Gegevens importeren en exporteren | X | X | X |
| Exporteren naar meerdere indelingen | X | X | X |
| Back-up en herstel |  | X |  |
| Parameters van de Server weergeven | X | X | X |
| Clientverbindingen weergeven | X | X | X |
