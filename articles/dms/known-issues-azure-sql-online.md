---
title: Artikel over bekende problemen/migratiebeperkingen met online migratie naar Azure SQL Database | Microsoft Docs
description: Meer informatie over bekende problemen/migratiebeperkingen met online migratie naar Azure SQL Database.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: douglasl
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: 816b67488acc567d81bf1916735d13c0e480fe5d
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53719554"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-db"></a>Bekende problemen/migratiebeperkingen met online migratie naar Azure SQL DB

Bekende problemen en beperkingen die zijn gekoppeld aan online migraties van SQL Server naar Azure SQL Database worden hieronder beschreven.

### <a name="migration-of-temporal-tables-not-supported"></a>Migratie van tijdelijke tabellen niet ondersteund

**Symptoom**

Als uw brondatabase uit een of meer tijdelijke tabellen bestaat, uw databasemigratie is mislukt tijdens de 'alle gegevens worden geladen'-bewerking en ziet u mogelijk het volgende bericht:

{"resourceId": "/subscriptions/<subscription id>/resourceGroups/migrateready/providers/Microsoft.DataMigration/services/<DMS Service name>", "errorType": "Database migratie error", "errorEvents": "[" vastleggen functies kunnen niet worden ingesteld. Retcode is: SqlState SQL_ERROR geretourneerd: 42000 NativeError: 13570 bericht: [Microsoft] [SQL Server Native Client 11.0] [SQL Server] op het gebruik van replicatie wordt niet ondersteund met de tijdelijke systeemversietabel ' [-toepassing. Steden], regel: 1 kolom: -1 "]"}
 
 ![Voorbeeld van de tijdelijke tabel fouten](media/known-issues-azure-sql-online/dms-temporal-tables-errors.png)

**Tijdelijke oplossing**

1. De tijdelijke tabellen niet vinden in het schema van de gegevensbron met behulp van de onderstaande query.
     ``` 
     select name,temporal_type,temporal_type_desc,* from sys.tables where temporal_type <>0
     ```
2. Uitsluiten van deze tabellen vanuit de **migratie-instellingen configureren** blade waarop u tabellen voor migratie opgeven.

3. De migratieactiviteit opnieuw.

**Bronnen**

Zie voor meer informatie het artikel [tijdelijke tabellen](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables?view=sql-server-2017).
 
### <a name="migration-of-tables-includes-one-or-more-columns-with-the-hierarchyid-data-type"></a>Migratie van tabellen bevat een of meer kolommen met het gegevenstype hierarchyid

**Symptoom**

Mogelijk ziet u een SQL-uitzondering voorstellen 'ntext is niet compatibel met hierarchyid' tijdens de bewerking 'alle gegevens worden geladen':
     
![Voorbeeld van hierarchyid-fouten](media/known-issues-azure-sql-online/dms-hierarchyid-errors.png)

**Tijdelijke oplossing**

1. Zoek de gebruikerstabellen die kolommen met het gegevenstype hierarchyid met behulp van de onderstaande query.

      ``` 
      select object_name(object_id) 'Table name' from sys.columns where system_type_id =240 and object_id in (select object_id from sys.objects where type='U')
      ``` 

 2. Uitsluiten van deze tabellen vanuit de **migratie-instellingen configureren** blade waarop u tabellen voor migratie opgeven.

 3. De migratieactiviteit opnieuw.

### <a name="migration-failures-with-various-integrity-violations-with-active-triggers-in-the-schema-during-full-data-load-or-incremental-data-sync"></a>Migratie-fouten met verschillende integriteit schendingen met actieve triggers in het schema tijdens "alle gegevens worden geladen" of "incrementele gegevenssynchronisatie"

**Tijdelijke oplossing**
1. Zoek de triggers die momenteel actief zijn in de brondatabase met behulp van de volgende query:
     ```
     select * from sys.triggers where is_disabled =0
     ```
2. Uitschakelen van de triggers op de brondatabase met behulp van de stappen in het artikel [DISABLE TRIGGER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017).

3. Voer de migratieactiviteit opnieuw uit.

### <a name="support-for-lob-data-types"></a>Ondersteuning voor LOB-gegevenstypen

**Symptoom**

Als de lengte van het grote Object (LOB)-kolom groter dan 32 KB is, mogelijk gegevens op de doelopslaglocatie ophalen afgekapt. U kunt de lengte van LOB-kolom met behulp van de onderstaande query controleren: 

``` 
SELECT max(len(ColumnName)) as LEN from TableName
```

**Tijdelijke oplossing**

Als u een LOB-kolom die groter is dan 32 KB, neem dan contact op met het technische team op [ dmsfeedback@microsoft.com ](mailto:dmsfeedback@microsoft.com).

### <a name="issues-with-timestamp-columns"></a>Problemen met de timestamp-kolommen

**Symptoom**

DMS migreren de waarde timestamp; niet DMS genereert in plaats daarvan een nieuwe tijdstempelwaarde in de doeltabel.

**Tijdelijke oplossing**

Als u DMS voor het migreren van de waarde van de exacte tijdstempel opgeslagen in de brontabel nodig hebt, neem dan contact op met het technische team op [ dmsfeedback@microsoft.com ](mailto:dmsfeedback@microsoft.com).

### <a name="data-migration-errors-do-not-provide-additional-details-on-the-database-detailed-status-blade"></a>Fouten bij de migratie van gegevens bieden aanvullende informatie op de blade Database gedetailleerde status weergegeven.

**Symptoom**

Wanneer u de migratie-fouten in de status van Databases detailweergave optreden, selecteert de **fouten bij de migratie van gegevens** koppeling in het bovenste lint bieden aanvullende informatie die specifiek zijn voor de migratie-fouten mogelijk niet.

![fouten bij de migratie van de gegevens er zijn geen details-voorbeeld](media/known-issues-azure-sql-online/dms-data-migration-errors-no-details.png)

**Tijdelijke oplossing**

Als u details van de specifieke fout, de volgende stappen uit te voeren.

1. Sluit de blade Database gedetailleerde status weergegeven om het scherm van de migratie van activiteit weer te geven.

     ![migratie-activiteitenscherm](media/known-issues-azure-sql-online/dms-migration-activity-screen.png)

2. Selecteer **Zie foutdetails** om specifieke foutberichten die u helpen bij het oplossen van fouten bij de migratie weer te geven.
