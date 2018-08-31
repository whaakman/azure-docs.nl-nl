---
title: Tekenreeksen voor databaseverbindingen voor Azure SQL Data Warehouse | Microsoft Docs
description: Verbindingsreeksen voor SQL Data Warehouse
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: de875731ffd2170f70d8034f0178d61351d86952
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43248018"
---
# <a name="connection-strings-for-azure-sql-data-warehouse"></a>Tekenreeksen voor databaseverbindingen voor Azure SQL Data Warehouse
U kunt verbinding maken met SQL Data Warehouse met diverse verschillende protocollen, zoals [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP] [ PHP] en [JDBC][JDBC]. Hieronder volgen enkele voorbeelden van tekenreeksen voor elk protocol voor verbindingen.  U kunt ook de Azure-portal gebruiken om te maken van de verbindingsreeks.  Voor het bouwen van de verbindingsreeks met de Azure-portal, gaat u naar de databaseblade onder *Essentials* klikt u op *databaseverbindingsreeksen tonen*.

## <a name="sample-adonet-connection-string"></a>Voorbeeld van ADO.NET-verbindingsreeks
```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## <a name="sample-odbc-connection-string"></a>Voorbeeld van ODBC-verbindingsreeks
```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## <a name="sample-php-connection-string"></a>Voorbeeld van PHP-verbindingsreeks
```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## <a name="sample-jdbc-connection-string"></a>Voorbeeld van JDBC-verbindingsreeks
```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

> [!NOTE]
> Overweeg de verbindingstime-out stellen aan 300 seconden om de verbinding met het overbruggen van korte perioden van niet-beschikbaarheid staat te stellen.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Als u wilt gaan met het uitvoeren van query's uw datawarehouse met Visual Studio en andere toepassingen, Zie [query's uitvoeren met Visual Studio][Query with Visual Studio].

<!--Image references-->

<!--Azure.com references-->
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx

<!--Other references-->
