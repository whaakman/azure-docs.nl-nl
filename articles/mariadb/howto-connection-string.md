---
title: Verbinding maken met toepassingen met Azure Database voor MariaDB
description: Dit document geeft een lijst van de momenteel ondersteunde verbindingsreeksen voor toepassingen verbinding kunnen maken met Azure Database voor MariaDB, met inbegrip van ADO.NET (C#), JDBC, Node.js, ODBC-, PHP, Python en Ruby.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 706bf395df98bd78fa36273a4948a11d4b59067d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967928"
---
# <a name="how-to-connect-applications-to-azure-database-for-mariadb"></a>Verbinding maken tussen toepassingen met Azure Database voor MariaDB
In dit onderwerp geeft een lijst van de tekenreeks verbindingstypen die worden ondersteund door Azure Database voor MariaDB, samen met sjablonen en voorbeelden. Mogelijk hebt u verschillende parameters en instellingen in de verbindingsreeks.

- Als u het certificaat, Zie [over het configureren van SSL](./howto-configure-ssl.md).
- {your_host} [servernaam].mariadb.database.azure.com =
- {your_user}@{servername} = indeling van de gebruikers-id voor de verificatie correct.  Als u alleen de gebruikers-id gebruikt, mislukt de verificatie.

## <a name="adonet"></a>ADO.NET
```csharp
Server={your_host}; Port=3306; Database={your_database}; Uid={username@servername}; Pwd={your_password}; SslMode=Preferred;
```

In dit voorbeeld is de servernaam `mydemoserver`, de databasenaam is `wpdb`, de gebruikersnaam van de is `WPAdmin`, en het wachtwoord is `mypassword!2`. Als gevolg hiervan moet de verbindingsreeks:

```csharp
Server= "mydemoserver.mariadb.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@mydemoserver"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```java
String url ="jdbc:mariadb://{your_host}:3306/{your_database}?useSSL=true&trustServerCertificate=true"; myDbConn = DriverManager.getConnection(url, "{username@servername}", {your_password});
```

## <a name="nodejs"></a>Node.js
```javascript
var conn = mysql.createConnection({host: "{your_host}", user: "{your_username}", password: {your_password}, database: {your_database}, port: 3306, ssl:{ca:fs.readFileSync({ca-cert filename})}});
```

## <a name="odbc"></a>ODBC
```cpp
DRIVER={MARIADB ODBC 3.0 Driver}; Server="{your_host}"; Port=3306; Database={your_database}; Uid="{username@servername}"; Pwd={your_password}; sslca={ca-cert filename}; sslverify=1;
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "{your_host}", "{username@servername}", {your_password}, {your_database}, 3306);
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user="{username@servername}", password={your_password}, host="{your_host}", port=3306, database={your_database}, ssl_ca={ca-cert filename}, ssl_verify_cert=true)
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: "{username@servername}", password: {your_password}, database: {your_database}, host: "{your_host}", port: 3306, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA')
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a>De verbinding details van de verbindingsreeks ophalen uit de Azure portal
In de [Azure-portal](https://portal.azure.com), gaat u naar uw Azure Database voor MariaDB-server en klik vervolgens op **verbindingsreeksen** voor de tekenreekslijst voor uw exemplaar: ![deelvenster van de verbinding tekenreeksen in Azure Portal](./media/howto-connection-strings/connection-strings-on-portal.png)

De tekenreeks bevat informatie zoals het stuurprogramma, server en andere database verbindingsparameters. Deze voorbeelden voor het gebruik van uw eigen parameters, zoals de databasenaam van de en wachtwoord wijzigen. U kunt vervolgens deze tekenreeks verbinding maken met de server vanuit uw code en toepassingen.

<!-- 
## Next steps
- For more information about connection libraries, see [Concepts - Connection libraries](./concepts-connection-libraries.md).
- -->
