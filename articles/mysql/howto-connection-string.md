---
title: Verbinding maken met toepassingen met Azure-Database voor MySQL | Microsoft Docs
description: Dit document worden de momenteel ondersteunde verbindingsreeksen voor toepassingen om te verbinden met Azure-Database voor MySQL, met inbegrip van ADO.NET (C#), JDBC, Node.js, ODBC-, PHP, Python en Ruby.
services: mysql
author: mswutao
ms.author: wuta
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: c9fe0a892bd5e81d2b33987b6ca55ec753550a01
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-connect-applications-to-azure-database-for-mysql"></a>Verbinding maken tussen toepassingen met Azure-Database voor MySQL
Dit onderwerp worden de verbindingstypen tekenreeks die worden ondersteund door Azure Database voor MySQL, samen met sjablonen en voorbeelden. Mogelijk hebt u verschillende parameters en instellingen in de verbindingsreeks.

- Als u het certificaat, Zie [SSL configureren](./howto-configure-ssl.md).
- {your_host} = <servername>. mysql.database.azure.com
- {your_user}@{servername} = userID indeling voor verificatie correct.  Als u alleen de gebruikers-id gebruikt, mislukt de verificatie.

## <a name="adonet"></a>ADO.NET
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

In dit voorbeeld wordt de servernaam van de is `myserver4demo`, de databasenaam van de is `wpdb`, de gebruikersnaam is `WPAdmin`, en het wachtwoord is `mypassword!2`. Als gevolg hiervan moet de verbindingsreeks:

```ado.net
Server= "myserver4demo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@myserver4demo"; Pwd="mypassword!2"; SslMode=Required;
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

## <a name="get-the-connection-string-details-from-the-azure-portal"></a>Het ophalen van details over de verbindingstekenreeks van de Azure-portal
In de [Azure-portal](https://portal.azure.com), gaat u naar uw Azure-Database voor de MySQL-server en klik vervolgens op **verbindingsreeksen** voor de tekenreekslijst voor uw exemplaar: ![deelvenster van de verbinding tekenreeksen in Azure Portal](./media/howto-connection-strings/connection-strings-on-portal.png)

De tekenreeks bevat informatie zoals het stuurprogramma, server en andere database verbindingsparameters. Deze voorbeelden voor het gebruik van uw eigen parameters, zoals de databasenaam van de en wachtwoord wijzigen. U kunt vervolgens deze tekenreeks verbinding maken met de server van uw code en toepassingen.

## <a name="next-steps"></a>Volgende stappen
- Zie voor meer informatie over verbindingsbibliotheken [concepten - verbindingsbibliotheken](./concepts-connection-libraries.md).
