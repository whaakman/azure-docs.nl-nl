---
title: Het oplossen van Azure SQL datawarehouse | Microsoft Docs
description: Problemen oplossen met Azure SQL datawarehouse.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 03/27/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 5115ffbc3568c87c37bae4a3e65c37f8504f1fb8
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541125"
---
# <a name="troubleshooting-connectivity-issues"></a>Het oplossen van problemen met de netwerkverbinding

Dit artikel worden algemene probleemoplossing technieken om verbinding te maken met uw SQL Data Warehouse.
- [Servicebeschikbaarheid van de controleren](./sql-data-warehouse-troubleshoot-connectivity.md#check-service-availability)
- [Controleer voor vergroten/verkleinen of onderbroken bewerking](./sql-data-warehouse-troubleshoot-connectivity.md#check-for-paused-or-scaling-operation)
- [Controleer de firewall-instellingen](./sql-data-warehouse-troubleshoot-connectivity.md#check-your-firewall-settings)
- [Controleer de instellingen van uw VNet/Service-eindpunt](./sql-data-warehouse-troubleshoot-connectivity.md#check-your-vnetservice-endpoint-settings)
- [Controleren op de nieuwste stuurprogramma 's](./sql-data-warehouse-troubleshoot-connectivity.md#check-for-the-latest-drivers)
- [Controleer uw verbindingsreeks](./sql-data-warehouse-troubleshoot-connectivity.md#check-your-connection-string)
- [Problemen met onregelmatige verbinding](./sql-data-warehouse-troubleshoot-connectivity.md#intermittent-connection-issues)
- [Veelvoorkomende foutberichten](./sql-data-warehouse-troubleshoot-connectivity.md#common-error-messages)

## <a name="check-service-availability"></a>Servicebeschikbaarheid van de controleren

Controleer of de service beschikbaar is. In de Azure-portal, gaat u naar de SQL datawarehouse die u probeert om verbinding te maken. Klik in het linkerdeelvenster van de inhoudsopgave, op **vaststellen en oplossen van problemen met**.

![Selecteer Resource health](./media/sql-data-warehouse-troubleshoot-connectivity/diagnostics-link.png)

De status van uw SQL datawarehouse worden hier weergegeven. Als de service niet wordt weergegeven als **beschikbaar**, controleert u verdere stappen.

![De service is beschikbaar](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health.png)

Als de status van uw Resource ziet u dat uw datawarehouse wordt onderbroken of schalen, volgt u de instructies voor het hervatten van uw datawarehouse.

![Service onderbroken](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health-pausing.png) aanvullende informatie over Resource Health vindt u hier.

## <a name="check-for-paused-or-scaling-operation"></a>Controleer voor vergroten/verkleinen of onderbroken bewerking

Controleer de portal om te zien of uw SQL datawarehouse is onderbroken of schalen.

![Service is onderbroken](./media/sql-data-warehouse-troubleshoot-connectivity/overview-paused.png)

Als u dat ziet uw service is onderbroken of schalen, controleert u dat deze niet bij uw onderhoudsplanning. In de portal voor uw SQL datawarehouse *overzicht*, ziet u de geselecteerde onderhoudsplanning.

![Overzicht onderhoudsplanning](./media/sql-data-warehouse-troubleshoot-connectivity/overview-maintance-schedule.png)

Neem anders contact op met uw IT-beheerder om te verifiëren dat dit onderhoud een geplande gebeurtenis niet. Volg de stappen voor het hervatten van de SQL datawarehouse, [hier](https://docs.microsoft.com/azure/sql-data-warehouse/pause-and-resume-compute-portal#resume-compute).

## <a name="check-your-firewall-settings"></a>Controleer de firewall-instellingen

SQL Database Warehouse communiceert via poort 1433.   Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, is uitgaand verkeer via poort 1433 mogelijk niet toegestaan vanwege de firewall van het netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server als uw IT-afdeling poort 1433 openstelt. Meer informatie over firewallconfiguraties vindt [hier](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure?toc=%2Fazure%2Fsql-data-warehouse%2Ftoc.json#manage-ip-firewall-rules-using-the-azure-portal).

## <a name="check-your-vnetservice-endpoint-settings"></a>Controleer de instellingen van uw VNet/Service-eindpunt

Als u fouten 40914 en 40615 ontvangt, Zie [beschrijving van de fout en de resolutie hier](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json#errors-40914-and-40615).

## <a name="check-for-the-latest-drivers"></a>Controleren op de nieuwste stuurprogramma 's

### <a name="software"></a>Software

Controleer of u de nieuwste hulpprogramma's verbinding maken met uw SQL datawarehouse:

* SSMS
* Azure Data Studio
* SQL Server Data Tools (Visual Studio)

### <a name="drivers"></a>Stuurprogramma's

Controleer of u de meest recente versies van stuurprogramma's.  Met behulp van een oudere versie van de stuurprogramma's kan leiden tot onverwacht gedrag als de oudere stuurprogramma's kunnen geen ondersteuning voor nieuwe functies.

* [ODBC](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server)
* [JDBC](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)
* [OLE DB](https://docs.microsoft.com/sql/connect/oledb/download-oledb-driver-for-sql-server)
* [PHP](https://docs.microsoft.com/sql/connect/php/download-drivers-php-sql-server)

## <a name="check-your-connection-string"></a>Controleer uw verbindingsreeks

Controleer of dat uw verbindingsreeksen juist zijn ingesteld.  Hieronder vindt u enkele voorbeelden.  U vindt meer informatie over [connection strings hier](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-connection-strings).

ADO.NET-verbindingsreeks

```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

ODBC-verbindingsreeks

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

PHP-verbindingsreeks

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

JDBC-verbindingsreeks

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="intermittent-connection-issues"></a>Problemen met onregelmatige verbinding

Controleer als u zware belasting op de server met een groot aantal aanvragen in de wachtrij ondervindt. U moet mogelijk uw datawarehouse voor aanvullende bronnen opschalen.

## <a name="common-error-messages"></a>Veelvoorkomende foutberichten

Fouten 40914 en 40615, Zie de [beschrijving van de fout en de resolutie hier](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json#errors-40914-and-40615).

## <a name="still-having-connectivity-issues"></a>Nog steeds problemen met de netwerkverbinding?
Maak een [ondersteuningsticket](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) , zodat het technische team u kan ondersteunen.
