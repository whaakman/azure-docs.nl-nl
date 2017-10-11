---
title: Transparante gegevensversleuteling in SQL datawarehouse (T-SQL) | Microsoft Docs
description: Transparante gegevensversleuteling (TDE) in SQL datawarehouse (T-SQL)
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: barbkess
editor: 
ms.assetid: 8ccefef3-1308-41ee-b336-5e491d1098ae
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 74c9032aababdce91ed617cd7a4c628915b42504
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Aan de slag met Transparent Data Encryption (TDE)
> [!div class="op_single_selector"]
> * [Overzicht van beveiliging](sql-data-warehouse-overview-manage-security.md)
> * [Verificatie](sql-data-warehouse-authentication.md)
> * [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Versleuteling (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Vereiste bevoegdheden
U moet een beheerder of als lid van de rol dbmanager zijn zodat Transparent Data Encryption (TDE).

## <a name="enabling-encryption"></a>Codering inschakelen
Volg deze stappen voor het inschakelen van TDE voor een SQL Data Warehouse:

1. Verbinding maken met de *master* database op de server die als host fungeert voor de database met een aanmelding die een beheerder of als lid van de **dbmanager** rol in de database master
2. Voer de volgende instructie voor het versleutelen van de database.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Codering uitschakelen
Volg deze stappen voor het uitschakelen van TDE voor een SQL Data Warehouse:

1. Verbinding maken met de *master* database met een aanmelding die een beheerder of als lid van de **dbmanager** rol in de database master
2. Voer de volgende instructie voor het versleutelen van de database.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Een onderbroken SQL Data Warehouse moet voordat u wijzigingen aanbrengt in de instellingen van TDE worden hervat.
> 
> 

## <a name="verifying-encryption"></a>Versleuteling controleren
Om te controleren coderingsstatus voor een SQL Data Warehouse, de volgende stappen uit te voeren:

1. Verbinding maken met de *master* of exemplaar in de database met behulp van een aanmelding die een beheerder of als lid van de **dbmanager** rol in de database master
2. Voer de volgende instructie voor het versleutelen van de database.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Een resultaat van ```1``` geeft aan van een versleutelde database ```0``` geeft een niet-versleutelde database.

## <a name="encryption-dmvs"></a>Versleuteling DMV 's
* [sys.databases.][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
