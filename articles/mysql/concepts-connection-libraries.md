---
title: Verbindingsbibliotheken voor Azure Database for MySQL
description: In dit artikel geeft een lijst van elke tapewisselaar of het stuurprogramma die client-programma's gebruiken kunnen bij het verbinden met Azure Database voor MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 14515aefe9635160cf99a630b0742d23352532cf
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985961"
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>Verbindingsbibliotheken voor Azure Database for MySQL
In dit artikel geeft een lijst van elke tapewisselaar of het stuurprogramma die client-programma's gebruiken kunnen bij het verbinden met Azure Database voor MySQL.

## <a name="client-interfaces"></a>Clientinterfaces
MySQL biedt standaard databaseconnectiviteit stuurprogramma voor het gebruik van MySQL met toepassingen en hulpprogramma's die compatibel met industrienormen ODBC- en JDBC zijn. Elk systeem dat met ODBC of JDBC werkt kunt MySQL gebruiken.

| **Taal** | **Platform** | **Aanvullende resources** | **Downloaden** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [Systeemeigen MySQL-stuurprogramma voor PHP - mysqlnd](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [Downloaden](https://secure.php.net/downloads.php) |
| ODBC | Windows, Linux, Mac OS X- en Unix-platforms | [MySQL Connector/ODBC-handleiding voor ontwikkelaars](https://dev.mysql.com/doc/connector-odbc/en/) | [Downloaden](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [Handleiding voor ontwikkelaars van MySQL Connector/Net](https://dev.mysql.com/doc/connector-net/en/) | [Downloaden](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Platformonafhankelijk | [MySQL Connector/J 5.1 Developer Guide](https://dev.mysql.com/doc/connector-j/5.1/en/) | [Downloaden](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sidorares/node-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [Downloaden](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [Handleiding voor ontwikkelaars van MySQL Connector/Python](https://dev.mysql.com/doc/connector-python/en/) | [Downloaden](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [Handleiding voor ontwikkelaars van MySQL Connector/C++](https://dev.mysql.com/doc/connector-cpp/en/) | [Downloaden](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [Handleiding voor ontwikkelaars van MySQL Connector/C](https://dev.mysql.com/doc/connector-c/en/) | [Downloaden](https://dev.mysql.com/downloads/connector/c/)
| Perl | Windows, Linux, Mac OS X- en Unix-platforms | [DBD::MySQL](https://metacpan.org/pod/DBD::mysql) | [Downloaden](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Volgende stappen
Lees deze snelstartgidsen over hoe u verbinding maken met en query uitvoeren op Azure Database for MySQL met behulp van de gewenste taal:

[PHP](./connect-php.md) | [Java](./connect-java.md) |  [.NET (C#)](./connect-csharp.md) | [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Ruby](./connect-ruby.md) | [C++](connect-cpp.md) | [Go](./connect-go.md)

