---
title: Verbinding maken met toepassingen met Azure Database for MySQL
description: Dit document geeft een lijst van de momenteel ondersteunde verbindingsreeksen voor toepassingen verbinding kunnen maken met Azure Database voor MySQL, met inbegrip van ADO.NET (C#), JDBC, Node.js, ODBC-, PHP, Python en Ruby.
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 06bd91adb0a86198f7709d0989624657ce00dfa9
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054037"
---
# <a name="how-to-connect-applications-to-azure-database-for-mysql"></a>Verbinding maken tussen toepassingen met Azure Database for MySQL
In dit onderwerp geeft een lijst van de tekenreeks verbindingstypen die worden ondersteund door Azure Database voor MySQL, samen met sjablonen en voorbeelden. Mogelijk hebt u verschillende parameters en instellingen in de verbindingsreeks.

- Als u het certificaat, Zie [over het configureren van SSL](./howto-configure-ssl.md).
- {your_host} = <servername>. mysql.database.azure.com
- {your_user}@{servername} = indeling van de gebruikers-id voor de verificatie correct.  Als u alleen de gebruikers-id gebruikt, mislukt de verificatie.

## <a name="adonet"></a>ADO.NET
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

In dit voorbeeld is de servernaam `mydemoserver`, de databasenaam is `wpdb`, de gebruikersnaam van de is `WPAdmin`, en het wachtwoord is `mypassword!2`. Als gevolg hiervan moet de verbindingsreeks:

```ado.net
Server= "mydemoserver.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@mydemoserver"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a>Node.js
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a>ODBC
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a>De verbinding details van de verbindingsreeks ophalen uit de Azure portal
In de [Azure-portal](https://portal.azure.com), gaat u naar uw Azure Database voor MySQL-server en klik vervolgens op **verbindingsreeksen** voor de tekenreekslijst voor uw exemplaar: ![deelvenster van de verbinding tekenreeksen in Azure Portal](./media/howto-connection-strings/connection-strings-on-portal.png)

De tekenreeks bevat informatie zoals het stuurprogramma, server en andere database verbindingsparameters. Deze voorbeelden voor het gebruik van uw eigen parameters, zoals de databasenaam van de en wachtwoord wijzigen. U kunt vervolgens deze tekenreeks verbinding maken met de server vanuit uw code en toepassingen.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over verbindingsbibliotheken [concepten - verbindingsbibliotheken](./concepts-connection-libraries.md).
