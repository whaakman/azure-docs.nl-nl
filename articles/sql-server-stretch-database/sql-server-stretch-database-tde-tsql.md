---
title: Transparante gegevensversleuteling inschakelt voor Stretch-Database TSQL - Azure | Microsoft Docs
description: Transparante gegevensversleuteling (TDE) voor SQL Server Stretch Database in Azure TSQL inschakelen
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: anvang
ms.openlocfilehash: ed26c2b386e08b76f78b4a05e12c46d2b97c20f2
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Transparante gegevensversleuteling (TDE) inschakelen voor Stretch Database in Azure (Transact-SQL)
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transparent Data Encryption (TDE) beschermt tegen de dreiging van schadelijke activiteiten door realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust in te voeren zonder dat wijzigingen in de toepassing.

TDE versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel, naam van de databaseversleutelingssleutel. De databaseversleutelingssleutel is beveiligd met een ingebouwde servercertificaat. Het ingebouwde servercertificaat is uniek voor elke Azure-server. Microsoft draait automatisch deze certificaten ten minste om de 90 dagen. Zie voor een algemene beschrijving van TDE [Transparent Data Encryption (TDE)].

## <a name="enabling-encryption"></a>Codering inschakelen
Zodat TDE voor een Azure-database die de gegevens opslaat die zijn gemigreerd vanuit een Stretch geschikte SQL Server-database, het volgende doen:

1. Verbinding maken met de *master* database op de Azure-server die als host fungeert voor de database met een aanmelding die een beheerder of als lid van de **dbmanager** rol in de database master
2. Voer de volgende instructie voor het versleutelen van de database.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Codering uitschakelen
Schakel TDE voor een Azure-database die de gegevens worden opgeslagen die zijn gemigreerd vanuit Stretch geschikte SQL Server-database, het volgende doen:

1. Verbinding maken met de *master* database met een aanmelding die een beheerder of als lid van de **dbmanager** rol in de database master
2. Voer de volgende instructie voor het versleutelen van de database.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Versleuteling controleren
Om te controleren of coderingsstatus voor een Azure-database met opslaan van de gegevens gemigreerd vanuit een Stretch geschikte SQL Server-database, het volgende doen:

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

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
