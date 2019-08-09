---
title: Artikel over bekende problemen/migratie beperkingen met online migraties naar Azure Database for MySQL | Microsoft Docs
description: Meer informatie over bekende problemen/migratie beperkingen met online migraties voor Azure Database for MySQL.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 08/06/2019
ms.openlocfilehash: fc5565ab9e3be21b96ce5aa5a938cf22ec3caeb0
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848489"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-db-for-mysql"></a>Bekende problemen/migratie beperkingen met online migraties naar Azure DB voor MySQL

Bekende problemen en beperkingen die zijn gekoppeld aan online migraties van MySQL naar Azure Database for MySQL worden beschreven in de volgende secties.

## <a name="online-migration-configuration"></a>Configuratie van online migratie

- De versie van de MySQL-bron server moet versie 5.6.35, 5.7.18 of hoger zijn
- Azure Database for MySQL ondersteunt:
  - MySQL-Community-editie
  - InnoDB-engine
- Migratie van dezelfde versie. Het migreren van MySQL 5,6 naar Azure Database for MySQL 5,7 wordt niet ondersteund.
- Binaire logboek registratie inschakelen in my. ini (Windows) of my. cnf (UNIX)
  - Stel Server_id in op een wille keurig getal dat groter is dan of gelijk is aan 1, bijvoorbeeld Server_id = 1 (alleen voor MySQL 5,6)
  - Log-bin = \<pad > instellen (alleen voor MySQL 5,6)
  - Set binlog_format = Row
  - Expire_logs_days = 5 (alleen aanbevolen voor MySQL 5,6)
- De gebruiker moet de rol ReplicationAdmin hebben.
- Sorteringen die zijn gedefinieerd voor de bron-MySQL-data base zijn dezelfde als die zijn gedefinieerd in doel Azure Database for MySQL.
- Het schema moet overeenkomen tussen de bron-MySQL-data base en de doel database in Azure Database for MySQL.
- Het schema in de doel Azure Database for MySQL mag geen refererende sleutels hebben. Gebruik de volgende query om refererende sleutels te verwijderen:
    ```
    SET group_concat_max_len = 8192;
    SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
    FROM
    (SELECT 
    KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
        FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
        WHERE
          KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
          AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
      AND KCU.REFERENCED_TABLE_SCHEMA = ['schema_name']) Queries
      GROUP BY SchemaName;
    ```

    Voer het 'drop foreign key'-script (de tweede kolom) uit in het queryresultaat.
- Het schema in de doel Azure Database for MySQL mag geen triggers hebben. Triggers in doel database weghalen:
    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="datatype-limitations"></a>Beperkingen van gegevens typen

- **Beperking**: Als er een JSON-gegevens type is in de MySQL-bron database, mislukt de migratie tijdens continue synchronisatie.

    **Tijdelijke oplossing**: Wijzig het JSON-gegevens type in middel grote tekst of LONGTEXT in de bron-MySQL-data base.

- **Beperking**: Als er geen primaire sleutel voor tabellen is, mislukt de continue synchronisatie.

    **Tijdelijke oplossing**: Stel tijdelijk een primaire sleutel voor de tabel in voor de migratie om door te gaan. U kunt de primaire sleutel verwijderen nadat de gegevens migratie is voltooid.

## <a name="lob-limitations"></a>LOB-beperkingen

Large Object LOB-kolommen zijn kolommen die groot kunnen worden uitgebreid. Voor MySQL worden middel grote tekst, LONGTEXT, blob, Mediumblob, Longblob, etc., een deel van de gegevens typen van LOB zijn.

- **Beperking**: Als er LOB-gegevens typen worden gebruikt als primaire sleutel, mislukt de migratie.

    **Tijdelijke oplossing**: Vervang de primaire sleutel door andere gegevens typen of kolommen die niet LOB zijn.

- **Beperking**: Als de LOB-kolom (lengte van Large Object) groter is dan 32 KB, kunnen gegevens op het doel worden afgekapt. U kunt de lengte van de LOB-kolom controleren met behulp van deze query:
    ```
    SELECT max(length(description)) as LEN from catalog;
    ```

    **Tijdelijke oplossing**: Als u een LOB-object hebt dat groter is dan 32 KB, neemt u contact op met het technische team om [Azure data base](mailto:AskAzureDatabaseMigrations@service.microsoft.com)-migraties te vragen. 

## <a name="limitations-when-migrating-online-from-aws-rds-mysql"></a>Beperkingen bij het online migreren van AWS RDS MySQL

Wanneer u probeert een online migratie uit te voeren vanaf AWS RDS MySQL naar Azure Database for MySQL, kunt u de volgende fouten tegen komen.

- **Fout:** {0}De data base heeft een refererende sleutel (s) op het doel. Corrigeer het doel en start een nieuwe gegevensmigratie. Uitvoeren onder script op doel om de refererende sleutel (s) weer te geven

  **Beperking**: Als u refererende sleutels in uw schema hebt, mislukken de eerste lading en doorlopende synchronisatie van de migratie.
  **Tijdelijke oplossing**: Voer het volgende script in MySQL Workbench uit om het script voor verwijdering van refererende sleutels en het script voor toevoeging van refererende sleutels te extraheren:

  ```
  SET group_concat_max_len = 8192; SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery FROM (SELECT KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC WHERE KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA AND KCU.REFERENCED_TABLE_SCHEMA = 'SchemaName') Queries GROUP BY SchemaName;
  ```

- **Fout:** {0}De data base bestaat niet op de server. Opgegeven MySQL-bronserver is hoofdlettergevoelig. Controleer de databasenaam.

  **Beperking**: Bij het migreren van een MySQL-database naar Azure met behulp van de opdrachtregelinterface (CLI) kunnen gebruikers deze fout krijgen. De service kan de data base niet vinden op de bron server, omdat u mogelijk een onjuiste database naam hebt opgegeven of omdat de data base niet aanwezig is op de vermelde server. Opmerking database namen zijn hoofdletter gevoelig.

  **Tijdelijke oplossing**: Geef de exacte database naam op en probeer het opnieuw.

- **Fout:** Er zijn tabellen met dezelfde naam in de data base {data base}. Azure Database for MySQL biedt geen ondersteuning voor hoofdlettergevoelige tabellen.

  **Beperking**: Deze fout treedt op als er twee tabellen met dezelfde naam in de brondatabase voorkomen. Azure Database for MySQL ondersteunt geen hoofdletter gevoelige tabellen.

  **Tijdelijke oplossing**: Werk de tabel namen bij zodat deze uniek zijn en probeer het opnieuw.

- **Fout:** De doel database {Data Base} is leeg. Migreer het schema.

  **Beperking**: Deze fout treedt op wanneer het doel Azure Database for MySQL data base niet het vereiste schema heeft. Schema migratie is vereist om het migreren van gegevens naar uw doel in te scha kelen.

  **Tijdelijke oplossing**: [Migreer het schema](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#migrate-the-sample-schema) van de bron database naar de doel database.

## <a name="other-limitations"></a>Andere beperkingen

- Een wachtwoord teken reeks met accolades {} aan het begin en einde van de teken reeks van het wacht woord wordt niet ondersteund. Deze beperking is van toepassing op beide verbindingen met de bron-MySQL en doel Azure Database for MySQL.
- De volgende DDLs worden niet ondersteund:
  - Alle partitie DDLs
  - Tabel neerzetten
  - Tabel naam wijzigen
- Met behulp van de *Alter table < table_name > kolom < column_name >* -instructie toevoegen om kolommen toe te voegen aan het begin of aan het midden van een tabel, wordt niet ondersteund. De *Alter table < table_name > kolom < COLUMN_NAME toevoegen >* voegt de kolom toe aan het einde van de tabel.
- Indexen die slechts op een deel van de kolom gegevens zijn gemaakt, worden niet ondersteund. De volgende instructie is een voor beeld van het maken van een index met slechts een deel van de kolom gegevens:

    ``` 
    CREATE INDEX partial_name ON customer (name(10));
    ```

- In het DMS zijn de limieten voor het migreren van data bases in één enkele migratie activiteit vier.
