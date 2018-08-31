---
title: Transparante gegevensversleuteling in SQL datawarehouse (T-SQL) | Microsoft Docs
description: Transparante gegevensversleuteling (TDE) in SQL datawarehouse (T-SQL)
services: sql-data-warehouse
author: kavithaj
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: ccdba241a2921a59f7db9668ec2b6f0921aa9f44
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43307684"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Aan de slag met transparante gegevensversleuteling (TDE)
> [!div class="op_single_selector"]
> * [Beveiligingsoverzicht](sql-data-warehouse-overview-manage-security.md)
> * [Verificatie](sql-data-warehouse-authentication.md)
> * [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Versleuteling (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Vereiste machtigingen
Om in te schakelen transparante gegevensversleuteling (TDE), moet u een beheerder of lid zijn van de rol dbmanager.

## <a name="enabling-encryption"></a>Versleuteling is ingeschakeld
Volg deze stappen voor TDE inschakelen voor een SQL Data Warehouse:

1. Verbinding maken met de *master* database op de server die als host fungeert voor de database met behulp van een aanmelding die een beheerder of lid zijn van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het versleutelen van de database uit.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Als u versleuteling uitschakelt
Volg deze stappen voor TDE uitschakelen voor een SQL Data Warehouse:

1. Verbinding maken met de *master* database met behulp van een aanmelding die een beheerder of lid zijn van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het versleutelen van de database uit.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Een onderbroken SQL Data Warehouse moet voordat u wijzigingen in de TDE-instellingen worden hervat.
> 
> 

## <a name="verifying-encryption"></a>Controleren van versleuteling
Om te controleren of de versleutelingsstatus van de voor een SQL Data Warehouse, de volgende stappen uit te voeren:

1. Verbinding maken met de *master* of exemplaar in de database met behulp van een aanmelding die een beheerder of lid zijn van de **dbmanager** rol in de hoofddatabase
2. Voer de volgende instructie voor het versleutelen van de database uit.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Een resultaat van ```1``` geeft aan dat een versleutelde database ```0``` geeft aan dat een niet-versleutelde database.

## <a name="encryption-dmvs"></a>Versleuteling DMV 's
* [sys.databases.][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
