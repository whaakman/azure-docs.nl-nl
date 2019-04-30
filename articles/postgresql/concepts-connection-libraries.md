---
title: Verbindingsbibliotheken voor Azure Database for PostgreSQL
description: Dit artikel wordt beschreven verschillende bibliotheken en stuurprogramma's die ontwikkelaars als gebruiken kunnen codering toepassingen verbinding maken en te vragen uit Azure Database voor PostgreSQL.
services: postgresql
author: WenJason
ms.author: v-jay
manager: digimobile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
origin.date: 02/28/2018
ms.date: 12/03/2018
ms.openlocfilehash: 0e762a2d7cf82e2957fb276fcea0a20553f719e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60559769"
---
# <a name="connection-libraries-for-azure-database-for-postgresql"></a>Verbindingsbibliotheken voor Azure Database for PostgreSQL
In dit artikel geeft een lijst van bibliotheken en stuurprogramma's die ontwikkelaars gebruiken kunnen voor het ontwikkelen van toepassingen verbinding maken met en query uitvoeren op Azure Database voor PostgreSQL.

## <a name="client-interfaces"></a>Clientinterfaces
De meeste taal-clientbibliotheken waarmee verbinding wordt gemaakt met PostgreSQL-server zijn externe projecten en onafhankelijk van elkaar worden gedistribueerd. De bibliotheken die worden vermeld, worden ondersteund op de platformen Windows, Linux en Mac voor het verbinden met Azure Database voor PostgreSQL. Enkele voorbeelden van de Quick Start vindt u in het gedeelte volgende stappen.

| **Taal** | **Client-interface** | **Als u meer informatie** | **Downloaden** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2.0-compatibel | [Downloaden](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://secure.php.net/manual/en/book.pgsql.php) | Database-extensie | [Installeren](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [PG npm-pakket](https://www.npmjs.com/package/pg) | Pure JavaScript-niet-blokkerende client | [Installeren](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | Type 4 JDBC-stuurprogramma | [Downloaden](https://jdbc.postgresql.org/download.html)  |
| Ruby | [Pg gem](https://deveiate.org/code/pg/) | Ruby Interface | [Downloaden](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Aan de slag | [Pakket pq](https://godoc.org/github.com/lib/pq) | Pure Go postgres-stuurprogramma | [Installeren](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](https://www.npgsql.org/) | ADO.NET Data Provider | [Downloaden](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC-stuurprogramma | [Downloaden](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Primaire interface voor C-taal | Inbegrepen |
| C++ | [libpqxx](http://pqxx.org/) | Nieuwe stijl C++-interface | [Downloaden](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Volgende stappen
Lees deze snelstartgidsen over hoe u verbinding maken met en query uitvoeren op Azure Database for PostgreSQL met behulp van de gewenste taal:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md)
