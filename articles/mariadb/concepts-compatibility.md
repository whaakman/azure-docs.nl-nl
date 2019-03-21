---
title: Azure Database voor MariaDB-stuurprogramma's en compatibiliteit met hulpprogramma's voor beheer
description: In dit artikel beschrijft de MariaDB-stuurprogramma's en beheerprogramma's nodig die compatibel met Azure Database voor MariaDB zijn.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: 7a3d9a5f87a565625052fc54e3ecccc99fd928a7
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259490"
---
# <a name="mariadb-drivers-and-management-tools-compatible-with-azure-database-for-mariadb"></a>MariaDB-stuurprogramma's en beheerprogramma's compatibel zijn met Azure Database voor MariaDB

In dit artikel beschrijft de stuurprogramma's en beheerprogramma's nodig die compatibel met Azure Database voor MariaDB zijn.

## <a name="mariadb-drivers"></a>MariaDB-stuurprogramma 's

Azure Database voor MariaDB maakt gebruik van de community-editie van MariaDB-server. Daarom is het compatibel is met een groot aantal programmeertalen en stuurprogramma's. De API en het protocol van MariaDB zijn compatibel met die worden gebruikt door MySQL. Dit betekent dat connectors die met MySQL werken moeten ook worden gebruikt met MariaDB.

Het doel is de ondersteuning van de drie meest recente versies MariaDB stuurprogramma's en doorgaan met inspanningen met auteurs van de open-source-community voor het continu verbeteren van de functionaliteit en bruikbaarheid van MariaDB-stuurprogramma's. Een lijst met stuurprogramma's die zijn getest en zijn compatibel met Azure Database voor MariaDB 10.2 is opgegeven in de volgende tabel:

**Stuurprogramma** | **Koppelingen** | **Compatibele-versies** | **Incompatibele versies** | **Opmerkingen**
---|---|---|---|---
PHP | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | Voor PHP 7.0-verbinding met SSL MySQLi toevoegen MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT in de verbindingsreeks. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO-set: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` optie in op false.
.NET | [MySqlConnector op GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Installatiepakket van Nuget](https://www.nuget.org/packages/MySqlConnector/) | 0,27 en na | 0.26.5 en vóór |
MySQL Connector/NET | [MySQL Connector/NET](https://github.com/mysql/mysql-connector-net) | 8.0, 7.0, 6.10 |  | Een bug codering kan ertoe leiden dat de verbindingen niet op sommige UTF8-Windows-systemen.
Node.js |  [MySQLjs op GitHub](https://github.com/mysqljs/mysql/) <br> Installatiepakket van NPM:<br> Voer `npm install mysql` vanuit NPM | 2.15 | 2.14.1 en vóór
GA NAAR | https://github.com/go-sql-driver/mysql/releases | 1.3, 1.4 | 1.2 en vóór | Gebruik `allowNativePasswords=true` in de verbindingsreeks voor versie 1.3. Versie 1.4 bevat een oplossing en `allowNativePasswords=true` is niet langer vereist.
Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 en vóór |
Java | https://downloads.mariadb.org/connector-java/ | 2.1, 2.0, 1.6 | 1.5.5 en vóór |

## <a name="management-tools"></a>Beheerhulpprogramma's

Het voordeel van de compatibiliteit is een uitbreiding tot database beheerprogramma's ook. Uw bestaande hulpprogramma's moeten blijven werken met Azure Database voor MariaDB, zolang de databasebewerkingen binnen de grenzen van de gebruikersmachtigingen werkt. Drie algemene database beheerprogramma's die zijn getest en zijn compatibel met Azure Database voor MariaDB 10.2 worden vermeld in de volgende tabel:

| | **MySQL Workbench 6.x en hoger** | **Navicat 12** | **PHPMyAdmin 4.x en hoger**
---|---|---|---
Maken, bijwerken, lezen, schrijven, verwijderen | X | X | X
SSL-verbinding | X | X | X
SQL-Query is automatisch voltooid | X | X |
Gegevens importeren en exporteren | X | X | X
Exporteren naar meerdere indelingen | X | X | X
Back-up en herstel |  | X |
Parameters van de Server weergeven | X | X | X
Clientverbindingen weergeven | X | X | X

## <a name="next-steps"></a>Volgende stappen

- [Oplossen van verbindingsproblemen met Azure Database voor MariaDB](howto-troubleshoot-common-connection-issues.md)