<properties
   pageTitle="Verbinding maken met Azure SQL Data Warehouse | Microsoft Azure"
   description="Zoeken naar de servernaam en verbindingsreeks voor uw Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>


# Verbinding maken met Azure SQL Data Warehouse

In dit artikel wordt beschreven hoe u voor de eerste keer verbinding kunt maken met SQL Data Warehouse.

## Uw servernaam vinden

De eerste stap bij het maken van verbinding met SQL Data Warehouse is weten hoe u de naam van uw server kunt vinden.  De naam van de server in het volgende voorbeeld is bijvoorbeeld sample.database.windows.net. Ga als volgt te werk om de volledig gekwalificeerde servernaam te vinden:

1. Ga naar de [Azure Portal][].
2. Klik op **SQL-databases** 
3. Klik op de database waarmee u verbinding wilt maken.
4. Zoek de volledige servernaam.

    ![Volledige servernaam][1]

## Ondersteunde stuurprogramma's en verbindingsreeksen

Azure SQL Data Warehouse biedt ondersteuning voor [ADO.NET][], [ODBC][], [PHP][] en [JDBC][]. Klik op een van de bovenstaande stuurprogramma's om de meest recente versie en documentatie te vinden. Voor het automatisch genereren van de verbindingsreeks voor het stuurprogramma dat u gebruikt vanuit de Azure-portal, kunt u klikken op de optie **Databaseverbindingsreeksen tonen** uit het voorgaande voorbeeld.  Hier volgen ook enkele voorbeelden van hoe een verbindingsreeks er voor elk stuurprogramma uitziet.

> [AZURE.NOTE] Overweeg de verbindingstime-out in te stellen op 300 seconden. De verbinding blijft dan in stand tijdens korte perioden van niet-beschikbaarheid.

### Voorbeeld van ADO.NET-verbindingsreeks

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### Voorbeeld van ODBC-verbindingsreeks

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### Voorbeeld van PHP-verbindingsreeks

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### Voorbeeld van JDBC-verbindingsreeks

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## Verbindingsinstellingen

SQL Data Warehouse standaardiseert enkele instellingen tijdens het maken van de verbinding en het maken van objecten. Deze instellingen kunnen niet worden overschreven, en omvatten:

| Database-instelling       | Waarde                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | AAN                           |
| [QUOTED_IDENTIFIERS][] | AAN                           |
| [DATEFORMAT][]         | mdy                          |
| [DATEFIRST][]          | 7                            |

## Volgende stappen

Zie [Query’s uitvoeren bij Visual Studio][] als u verbinding wilt maken en een query wilt uitvoeren met Visual Studio. Zie [Verificatie met Azure SQL Data Warehouse][] voor meer informatie over verificatieopties.

<!--Articles-->
[Query’s uitvoeren bij Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Verificatie met Azure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure Portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png





<!--HONumber=Sep16_HO4-->


