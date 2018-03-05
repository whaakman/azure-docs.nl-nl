---
title: Verbindingsbibliotheken voor Azure-Database voor PostgreSQL
description: In dit artikel beschrijft de verschillende bibliotheken en stuurprogramma's die ontwikkelaars wanneer gebruiken kunnen toepassingen verbinding maken en query uitvoeren op Azure-Database voor PostgreSQL coderen.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 7f0dd217a124f530ba009c9a5588691f604827e5
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Verbindingsbibliotheken voor Azure-Database voor PostgreSQL
Dit artikel worden de bibliotheken en stuurprogramma's die ontwikkelaars gebruiken kunnen voor het ontwikkelen van toepassingen verbinding te maken voor PostgreSQL query uitvoeren op Azure-Database.

## <a name="client-interfaces"></a>Clientinterfaces
De meeste taal clientbibliotheken waarmee verbinding met server PostgreSQL externe projecten zijn en onafhankelijk van elkaar worden gedistribueerd. De bibliotheken die worden vermeld worden op de Windows, Linux en Mac-platforms voor het verbinden met Azure-Database voor PostgreSQL ondersteund. Enkele voorbeelden van de Quick Start worden in de volgende sectie van de stappen vermeld.

| **Taal** | **Client-interface** | **Aanvullende informatie** | **Downloaden** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | API voor DB 2.0-compatibel | [Downloaden](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://php.net/manual/en/book.pgsql.php) | Database-uitbreiding | [Installeren](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [PG npm-pakket](https://www.npmjs.com/package/pg) | Niet-blokkerende client pure JavaScript | [Installeren](https://www.npmjs.com/package/pg) |
| Java | [JDBC](http://jdbc.postgresql.org/) | Type 4 JDBC-stuurprogramma | [Downloaden](https://jdbc.postgresql.org/download.html)  |
| Ruby | [PG gem](https://deveiate.org/code/pg/) | Ruby-Interface | [Downloaden](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Aan de slag | [Pq pakket](https://godoc.org/github.com/lib/pq) | Pure Ga postgres stuurprogramma | [Installeren](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](http://www.npgsql.org/) | ADO.NET Data Provider | [Downloaden](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC-stuurprogramma | [Downloaden](http://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Primaire C taal interface | Inbegrepen |
| C++ | [libpqxx](http://pqxx.org/) | Nieuwe stijl C++-interface | [Downloaden](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Volgende stappen
Lees deze snelstartgidsen voor het verbinding maken met en doorzoeken op Azure Database PostgreSQL met behulp van de taal van uw keuze:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md)
