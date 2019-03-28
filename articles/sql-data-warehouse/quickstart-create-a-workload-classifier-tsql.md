---
title: 'Quickstart: Maken van een classificatie van de werkbelasting van - T-SQL | Microsoft Docs'
description: T-SQL gebruiken om te maken van een classificatie van de werkbelasting met hoge urgentie
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 8ae9ad4d1101c3b42c46c51546ac82e5b4a134c8
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519847"
---
# <a name="quickstart-create-a-workload-classifier-using-t-sql-preview"></a>Quickstart: Maken van een werkbelasting classificatie met T-SQL (Preview)

In deze snelstartgids maakt snel u een classificatie van de werkbelasting met hoge urgentie van de Directeur van uw organisatie. Deze classificatie werkbelasting kunt CEO query's hebben voorrang op andere query's met lagere prioriteit in de wachtrij.

> [!Note]
> Classificatie van de werkbelasting is beschikbaar in SQL Data Warehouse Gen2.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

> [!NOTE]
> Het maken van een SQL Data Warehouse kan leiden tot een nieuwe factureerbare service.  Zie [Prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) voor meer informatie.
>
>

## <a name="prerequisites"></a>Vereisten

In deze snelstartgids wordt ervan uitgegaan dat u hebt al een SQL datawarehouse en dat u databasemachtigingen voor CONTROL hebt. Gebruik [Maken en verbinden - portal](create-data-warehouse-portal.md) om een datawarehouse met de naam **mySampleDataWarehouse** te maken.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-login-for-theceo"></a>Aanmelding voor TheCEO maken

Maak een aanmelding van de SQL Server-verificatie in de `master` database met behulp van [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql) voor 'TheCEO'.

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'TheCEO')
BEGIN
CREATE LOGIN [TheCEO] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>Gebruiker maken

[Gebruiker maken](/sql/t-sql/statements/create-user-transact-sql?view=azure-sqldw-latest), "TheCEO" in mySampleDataWarehouse

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'THECEO')
BEGIN
CREATE USER [TheCEO] FOR LOGIN [TheCEO]
END
;
```

## <a name="create-a-workload-classifier"></a>Een workload classificatie maken

Maak een [werkbelasting classificatie](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) voor 'TheCEO' met hoge urgentie.

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO];
CREATE WORKLOAD CLASSIFIER [wgcTheCEO]
WITH (WORKLOAD_GROUP = 'xlargerc'
      ,MEMBERNAME = 'TheCEO'
      ,IMPORTANCE = HIGH);
```

## <a name="view-existing-classifiers"></a>Bestaande classificaties weergeven

```sql
SELECT * FROM sys.workload_management_workload_classifiers
```

## <a name="clean-up-resources"></a>Resources opschonen

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO]
DROP USER [TheCEO]
;
```

U bent in rekening gebracht voor datawarehouse-eenheden en gegevens die zijn opgeslagen in uw datawarehouse. Deze compute- en opslagresources worden apart in rekening gebracht.

- Als u de gegevens in de opslag wilt houden, kunt u het berekenen onderbreken wanneer u het datawarehouse niet gebruikt. Onderbreekt Reken-, u betaalt alleen voor de opslag van gegevens. Wanneer u klaar om te werken met de gegevens bent, hervat u compute.
- Als u in de toekomst geen kosten meer wilt hebben, kunt u de datawarehouse verwijderen.

Volg deze stappen voor het opschonen van resources.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), Selecteer op uw datawarehouse.

    ![Resources opschonen](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Als u wilt onderbreken, selecteert u de **onderbreken** knop. Als het datawarehouse is onderbroken, ziet u een knop **Start**.  Als u wilt hervatten compute, selecteer **Start**.

3. Als het datawarehouse, zodat u niet in rekening voor berekenen of opslaan gebracht bent, selecteert u **verwijderen**.

4. Als u wilt verwijderen van de SQL-server die u hebt gemaakt, selecteert u **mynewserver-20180430.database.windows.net** in de vorige afbeelding, en selecteer vervolgens **verwijderen**.  Wees voorzichtig met verwijderen. Als u de server verwijdert, worden ook alle databases verwijderd die zijn toegewezen aan de server.

5. Als u wilt verwijderen van de resourcegroep, selecteer **myResourceGroup**, en selecteer vervolgens **resourcegroep verwijderen**.

## <a name="next-steps"></a>Volgende stappen

U hebt nu een classificatie van de werkbelasting. Enkele query's worden uitgevoerd als TheCEO om te zien hoe ze uitvoeren. Zie [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) om query's en het belang toegewezen weer te geven.

Zie voor meer informatie over het beheer van SQL Data Warehouse workload [SQL Data Warehouse Workload belang](sql-data-warehouse-workload-importance.md) en [SQL Data Warehouse Workload classificatie](sql-data-warehouse-workload-classification.md).
