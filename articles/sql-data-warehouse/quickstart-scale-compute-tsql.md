---
title: 'Snelstartgids: Compute in Azure SQL Data Warehouse - T-SQL uitbreiden | Microsoft Docs'
description: T-SQL-opdrachten voor het schalen van rekenresources door dwu's aan te passen.
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: ''
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/16/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 1591192c72f5bf201dbbef80acc5895c8324fca4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-using-t-sql"></a>Snelstartgids: Scale compute in Azure SQL Data Warehouse met T-SQL

Scale compute in Azure SQL Data Warehouse met T-SQL en SQL Server Management Studio (SSMS). [Uitschalen compute](sql-data-warehouse-manage-compute-overview.md) voor betere prestaties of schaal back-berekeningen voor het opslaan van kosten. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

Download en installeer de nieuwste versie van [SSMS](/sql/ssms/download-sql-server-management-studio-ssms.md) (SQL Server Management Studio).

Dit wordt ervan uitgegaan dat u hebt voltooid [Snelstartgids: maken en koppelen - portal](create-data-warehouse-portal.md). Na het voltooien van de Quick Start maken en verbinding maken die u weet hoe u verbinding maken met de: gemaakt van een datawarehouse met de naam **mySampleDataWarehouse**, een firewallregel waarmee de client toegang tot de server, geïnstalleerd gemaakt.
 
## <a name="create-a-data-warehouse"></a>Een datawarehouse maken

Gebruik [Snelstartgids: maken en koppelen - portal](create-data-warehouse-portal.md) voor het maken van een datawarehouse met de naam **mySampleDataWarehouse**. Voltooi de Quick Start om te controleren of u een firewallregel hebt en verbinding kan maken met uw datawarehouse van SQL Server Management Studio.

## <a name="connect-to-the-server-as-server-admin"></a>Als serverbeheerder verbinding maken met de server

In deze sectie wordt gebruikgemaakt van [SSMS](/sql/ssms/download-sql-server-management-studio-ssms.md) (SQL Server Management Studio) om een verbinding tot stand te brengen met de Azure SQL-server.

1. Open SQL Server Management Studio.

2. Voer in het dialoogvenster **Verbinding maken met server** de volgende informatie in:

   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | Servertype | Database-engine | Deze waarde is verplicht |
   | Servernaam | De volledig gekwalificeerde servernaam | Hier volgt een voorbeeld: **mynewserver-20171113.database.windows.net**. |
   | Verificatie | SQL Server-verificatie | SQL-verificatie is het enige verificatietype dat in deze zelfstudie is geconfigureerd. |
   | Aanmelden | Het beheerdersaccount voor de server | Dit is het account dat u hebt opgegeven tijdens het maken van de server. |
   | Wachtwoord | Het wachtwoord voor het beheerdersaccount voor de server | Dit is het wachtwoord dat u hebt opgegeven tijdens het maken van de server. |

    ![verbinding maken met server](media/load-data-from-azure-blob-storage-using-polybase/connect-to-server.png)

4. Klik op **Verbinden**. Het venster Objectverkenner wordt geopend in SSMS. 

5. Vouw **Databases** uit in Objectverkenner. Vouw vervolgens **mySampleDatabase** uit om de objecten in uw nieuwe database weer te geven.

    ![databaseobjecten](media/create-data-warehouse-portal/connected.png) 

## <a name="view-service-objective"></a>De servicedoelstelling weergeven
De service-instelling voor het beoogde bevat het nummer datawarehouse Units voor het datawarehouse. 

De huidige datawarehouse-eenheden voor uw datawarehouse weergeven:

1. Onder de verbinding met **mynewserver 20171113.database.windows.net**, vouw **systeemdatabases**.
2. Met de rechtermuisknop op **master** en selecteer **nieuwe Query**. Een nieuwe queryvenster wordt geopend.
3. Voer de volgende query in de weergave van de dynamische Beheerweergave sys.database_service_objectives te selecteren. 

    ```sql
    SELECT
        db.name [Database]
    ,   ds.edition [Edition]
    ,   ds.service_objective [Service Objective]
    FROM
        sys.database_service_objectives ds
    JOIN
        sys.databases db ON ds.database_id = db.database_id
    WHERE 
        db.name = 'mySampleDataWarehouse'
    ```

4. De volgende resultaten weergeven **mySampleDataWarehouse** heeft een serviceniveaudoelstelling van DW400. 

    ![Huidige dwu's weergeven](media/quickstart-scale-compute-tsql/view-current-dwu.png)


## <a name="scale-compute"></a>De schaal van Compute aanpassen
In SQL Data Warehouse, kunt u vergroten of verkleinen van rekenresources door datawarehouse units aan te passen. De [maken en Connect - portal](create-data-warehouse-portal.md) gemaakt **mySampleDataWarehouse** en met 400 dwu's wordt geïnitialiseerd. De volgende stappen aanpassen de dwu's voor **mySampleDataWarehouse**.

Datawarehouse units wijzigen:

1. Met de rechtermuisknop op **master** en selecteer **nieuwe Query**.
2. Gebruik de [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) T-SQL-instructie voor het wijzigen van de servicedoelstelling. Voer de volgende query om te wijzigen van de servicedoelstelling in DW300. 

```Sql
ALTER DATABASE mySampleDataWarehouse
MODIFY (SERVICE_OBJECTIVE = 'DW300')
;
```

## <a name="check-data-warehouse-state"></a>Controleer de status van datawarehouse

Als een datawarehouse is onderbroken, kunt u geen verbinding maken voor het met T-SQL. Overzicht van de huidige status van het datawarehouse, kunt u een PowerShell-cmdlet. Zie voor een voorbeeld [Controleer de status van datawarehouse - Powershell](quickstart-scale-compute-powershell.md#check-data-warehouse-state). 

## <a name="check-operation-status"></a>Controleer de bewerkingsstatus van

Voor informatie over verschillende bewerkingen op uw SQL Data Warehouse, kunt u de volgende query uitvoeren op de [sys.dm_operation_status bevat](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) DMV. Bijvoorbeeld, retourneert de bewerking en de status van de bewerking wordt IN_PROGRESS of voltooid.

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```


## <a name="next-steps"></a>Volgende stappen
U hebt nu geleerd hoe schalen compute voor uw datawarehouse. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.

> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
