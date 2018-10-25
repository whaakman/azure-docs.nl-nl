---
title: MySQL-stuurprogramma's en compatibiliteit met hulpprogramma's voor beheer
description: In dit artikel beschrijft de MySQL-stuurprogramma's en beheerprogramma's nodig die compatibel met Azure Database for MySQL zijn.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 9e56c2bd65f8a9a517a7cdebe02a1d051c689df6
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985882"
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>MySQL-stuurprogramma's en beheerprogramma's compatibel zijn met Azure Database for MySQL
In dit artikel beschrijft de stuurprogramma's en beheerprogramma's nodig die compatibel met Azure Database for MySQL zijn.

## <a name="mysql-drivers"></a>MySQL-stuurprogramma 's
Azure Database voor MySQL maakt gebruik van's werelds populairste community edition van de MySQL-database. Daarom is het compatibel is met een groot aantal programmeertalen en stuurprogramma's. Het doel is de ondersteuning van de drie meest recente versies MySQL-stuurprogramma's en doorgaan met inspanningen met auteurs van de open-source-community voor het continu verbeteren van de functionaliteit en bruikbaarheid van de MySQL-stuurprogramma's. Een lijst met stuurprogramma's die zijn getest en zijn compatibel met Azure Database for MySQL 5.6 en 5.7 is opgegeven in de volgende tabel:

| **Stuurprogramma** | **Koppelingen** | **Compatibele-versies** | **Uncompatible versies** | **Opmerkingen** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | https://secure.php.net/downloads.php | 5.5 5.6 7.x | 5.3 | Voor PHP 7.0-verbinding met SSL MySQLi toevoegen MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT in de verbindingsreeks. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO-set: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` optie in op false.|
| .Net | [MySqlConnector op GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Installatiepakket van Nuget](https://www.nuget.org/packages/MySqlConnector/) | 0,27 en na | 0.26.5 en vóór | |
| Nodejs |  [MySQLjs op GitHub](https://github.com/mysqljs/mysql/releases) <br> Installatiepakket van NPM:<br> Voer `npm install mysql` vanuit NPM | 2.15 | 2.14.1 en vóór | |
| GA NAAR | https://github.com/go-sql-driver/mysql/releases | 1.3 | 1.2 en vóór | Gebruik allowNativePasswords = true in de connection string |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 en vóór | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1 2.0 1.6 | 1.5.5 en vóór | |

## <a name="management-tools"></a>Beheerhulpprogramma's
Het voordeel van de compatibiliteit is een uitbreiding tot database beheerprogramma's ook. Uw bestaande hulpprogramma's moeten blijven werken met Azure Database voor MySQL, zolang de databasebewerkingen binnen de grenzen van de gebruikersmachtigingen werkt. Drie algemene database beheerprogramma's die zijn getest en zijn compatibel met Azure Database for MySQL 5.6 en 5.7 worden vermeld in de volgende tabel:

|                                     | **MySQL Workbench 6.x en hoger** | **Navicat 12** | **PHPMyAdmin 4.x en hoger** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Maken, bijwerken, lezen, schrijven, verwijderen | X | X | X |
| SSL-verbinding | X | X | X |
| SQL-Query is automatisch voltooid | X | X |  |
| Gegevens importeren en exporteren | X | X | X |
| Exporteren naar meerdere indelingen | X | X | X |
| Back-up en herstel |  | X |  |
| Parameters van de Server weergeven | X | X | X |
| Clientverbindingen weergeven | X | X | X |
