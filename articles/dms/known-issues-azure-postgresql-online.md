---
title: Artikel over bekende problemen/migratiebeperkingen met online migratie naar Azure Database for MySQL | Microsoft Docs
description: Meer informatie over bekende problemen/migratiebeperkingen met online migratie naar Azure Database voor MySQL.
services: database-migration
author: HJToland3
ms.author: scphang
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/22/2018
ms.openlocfilehash: e30ffe2dd79d55e856ef297608745b60578cf7e7
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46131299"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-db-for-postgresql"></a>Bekende problemen/migratiebeperkingen met online migratie naar Azure DB voor PostgreSQL

Bekende problemen en beperkingen die zijn gekoppeld aan online migraties van PostgreSQL met Azure Database for PostgreSQL worden in de volgende secties beschreven. 

## <a name="online-migration-configuration"></a>Configuratie voor de online migratie
- De bron PostgreSQL-Server versie 9.5.11, 9.6.7 of 10.3 moet worden uitgevoerd of hoger. Zie voor meer informatie het artikel [PostgreSQL-Database-versies ondersteund](1.2.%09https:/docs.microsoft.com/azure/postgresql/concepts-supported-versions).
- Alleen dezelfde versie migraties worden ondersteund. Bijvoorbeeld: migreren PostgreSQL 9.5.11 met Azure Database for PostgreSQL 9.6.7 wordt niet ondersteund.
- Om logische replicatie in te schakelen de **source PostgreSQL postgresql.conf** bestand, stelt u de volgende parameters:
    - **wal_level** = logische
    - **max_replication_slots** = [Max. aantal databases voor migratie]; als u migreren, 4 databases wilt, de waarde ingesteld op 4
    - **max_wal_senders** = [aantal databases gelijktijdig worden uitgevoerd]; de aanbevolen waarde is 10
- DMS-agent-IP toevoegen aan de bron PostgresSQL pg_hba.conf
    1. Noteer de DMS-IP-adres als u klaar bent met het inrichten van een exemplaar van DMS.
    2. Het IP-adres toevoegen aan het bestand pg_hba.conf zoals wordt weergegeven:

        host alle 172.16.136.18/10 md5 host replicatie postgres 172.16.136.18/10 md5

- De gebruiker moet de machtiging supergebruiker hebben op de server die als host fungeert voor de brondatabase
- Afgezien van dat ENUM in het schema van de bron-database, moeten de bron- en database-schema's overeenkomen.
- Het schema in de doel-Azure Database for PostgreSQL mag geen refererende sleutels. Gebruik de volgende query refererende sleutels verwijderen:

    ```
                                SELECT Queries.tablename
           ,concat('alter table ', Queries.tablename, ' ', STRING_AGG(concat('DROP CONSTRAINT ', Queries.foreignkey), ',')) as DropQuery
                ,concat('alter table ', Queries.tablename, ' ', 
                                                STRING_AGG(concat('ADD CONSTRAINT ', Queries.foreignkey, ' FOREIGN KEY (', column_name, ')', 'REFERENCES ', foreign_table_name, '(', foreign_column_name, ')' ), ',')) as AddQuery
        FROM
        (SELECT
        tc.table_schema, 
        tc.constraint_name as foreignkey, 
        tc.table_name as tableName, 
        kcu.column_name, 
        ccu.table_schema AS foreign_table_schema,
        ccu.table_name AS foreign_table_name,
        ccu.column_name AS foreign_column_name 
    FROM 
        information_schema.table_constraints AS tc 
        JOIN information_schema.key_column_usage AS kcu
          ON tc.constraint_name = kcu.constraint_name
          AND tc.table_schema = kcu.table_schema
        JOIN information_schema.constraint_column_usage AS ccu
          ON ccu.constraint_name = tc.constraint_name
          AND ccu.table_schema = tc.table_schema
    WHERE constraint_type = 'FOREIGN KEY') Queries
      GROUP BY Queries.tablename;
    
    ```

    Voer de drop refererende sleutel (dit is de tweede kolom) in het queryresultaat.

- Het schema in de doel-Azure Database for PostgreSQL mag geen geen triggers. Gebruik de volgende triggers in doeldatabase uitschakelen:

     ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
     ```

## <a name="datatype-limitations"></a>Beperkingen van gegevenstype

- **Beperking**: als er een ENUM-gegevenstype in de bron-PostgreSQL-database is, wordt de migratie mislukken tijdens de continue synchronisatie.

    **Tijdelijke oplossing**: wijzigen ENUM-gegevenstype naar teken variëren in Azure Database for PostgreSQL.

- **Beperking**: als er geen primaire sleutel voor tabellen, doorlopende synchronisatie mislukken.

    **Tijdelijke oplossing**: een primaire sleutel voor de tabel voor de migratie om door te gaan tijdelijk in te stellen. Nadat de migratie van gegevens is voltooid, kunt u de primaire sleutel verwijderen.

## <a name="lob-limitations"></a>LOB-beperkingen
Grote Object (LOB)-kolommen zijn kolommen die grote kunnen groeien. Voorbeelden van LOB-gegevenstypen zijn voor PostgreSQL, XML, JSON, afbeeldingen, tekst, enzovoort.

- **Beperking**: als LOB-gegevenstypen worden gebruikt als primaire sleutels, dan mislukt de migratie.

    **Tijdelijke oplossing**: primaire sleutel van de vervangen met andere gegevenstypen of kolommen die geen LOB.

- **Beperking**: als de lengte van het grote Object (LOB)-kolom groter dan 32 KB is, gegevens op het doel kan worden afgekapt. U kunt de lengte van LOB-kolom met behulp van deze query controleren:

    ```
    SELECT max(length(cast(body as text))) as body FROM customer_mail
    ```

    **Tijdelijke oplossing**: hebt u LOB-object dat groter is dan 32 KB, neem dan contact op met de engineering-team op [ dmsfeedback@microsoft.com ](mailto:dmsfeedback@microsoft.com).

- **Beperking**: als er LOB-kolommen in de tabel, en er geen primaire sleutel ingesteld voor de tabel is, gegevens voor deze tabel niet kan worden gemigreerd.

    **Tijdelijke oplossing**: een primaire sleutel voor de tabel voor de migratie om door te gaan tijdelijk in te stellen. Nadat de migratie van gegevens is voltooid, kunt u de primaire sleutel verwijderen.

## <a name="postgresql10-workaround"></a>Tijdelijke oplossing PostgreSQL10
PostgreSQL 10.x verschillende wijzigingen aanbrengt in pg_xlog mapnamen en kan daarom veroorzaakt migratie niet wordt uitgevoerd zoals verwacht. Als u van PostgreSQL migreren wilt 10.x met Azure Database for PostgreSQL 10.3, voer het volgende script op de bron-PostgreSQL-database om de functie wrapper rond pg_xlog functies te maken.

```
BEGIN;
CREATE SCHEMA IF NOT EXISTS fnRenames;
CREATE OR REPLACE FUNCTION fnRenames.pg_switch_xlog() RETURNS pg_lsn AS $$ 
   SELECT pg_switch_wal(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_replay_pause() RETURNS VOID AS $$ 
   SELECT pg_wal_replay_pause(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_replay_resume() RETURNS VOID AS $$ 
   SELECT pg_wal_replay_resume(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_is_xlog_replay_paused() RETURNS boolean AS $$ 
   SELECT pg_is_wal_replay_paused(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlogfile_name(lsn pg_lsn) RETURNS TEXT AS $$ 
   SELECT pg_walfile_name(lsn); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_last_xlog_replay_location() RETURNS pg_lsn AS $$ 
   SELECT pg_last_wal_replay_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_last_xlog_receive_location() RETURNS pg_lsn AS $$ 
   SELECT pg_last_wal_receive_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_flush_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_flush_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_current_xlog_insert_location() RETURNS pg_lsn AS $$ 
   SELECT pg_current_wal_insert_lsn(); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlog_location_diff(lsn1 pg_lsn, lsn2 pg_lsn) RETURNS NUMERIC AS $$ 
   SELECT pg_wal_lsn_diff(lsn1, lsn2); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_xlogfile_name_offset(lsn pg_lsn, OUT TEXT, OUT INTEGER) AS $$ 
   SELECT pg_walfile_name_offset(lsn); $$ LANGUAGE SQL;
CREATE OR REPLACE FUNCTION fnRenames.pg_create_logical_replication_slot(slot_name name, plugin name, 
   temporary BOOLEAN DEFAULT FALSE, OUT slot_name name, OUT xlog_position pg_lsn) RETURNS RECORD AS $$ 
   SELECT slot_name::NAME, lsn::pg_lsn FROM pg_catalog.pg_create_logical_replication_slot(slot_name, plugin, 
   temporary); $$ LANGUAGE SQL;
ALTER USER PG_User SET search_path = fnRenames, pg_catalog, "$user", public;

-- DROP SCHEMA fnRenames CASCADE;
-- ALTER USER PG_User SET search_path TO DEFAULT;
COMMIT;
```

## <a name="other-limitations"></a>Andere beperkingen
- Naam van de database kan niet een puntkomma (;) bevatten.
- Wachtwoordtekenreeks waarvoor openen en sluiten accolades {} wordt niet ondersteund. Deze beperking geldt voor zowel verbinding maken met PostgreSQL bron en doel-Azure Database voor PostgreSQL.
- Een vastgelegde tabel moet een primaire sleutel hebben. Als een tabel geen primaire sleutel, wordt het resultaat van de record verwijderen en UPDATE-bewerkingen onvoorspelbaar zijn.
- Bijwerken van een segment van de primaire sleutel wordt genegeerd. In dergelijke gevallen worden toepassen van deze update geïdentificeerd door het doel als een update die is niet alle rijen bijgewerkt en resulteert in een record die is geschreven naar de uitzonderingentabel.
- Migratie van meerdere tabellen met dezelfde naam maar met een andere aanvraag (bijvoorbeeld Tabel1, Tabel1 en Tabel1) kan onvoorspelbaar gedrag veroorzaken en daarom niet ondersteund.
- Wijzig de verwerking van [maken | ALTER | VERVOLGKEUZELIJST] tabel DDLs worden ondersteund, tenzij ze zijn ondergebracht in een blok van de hoofdtekst van de binnenste functie/procedure of in andere geneste constructies. De volgende wijziging worden bijvoorbeeld niet vastgelegd:

    ```
    CREATE OR REPLACE FUNCTION pg.create_distributors1() RETURNS void
    LANGUAGE plpgsql
    AS $$
    BEGIN
    create table pg.distributors1(did serial PRIMARY KEY,name varchar(40)
    NOT NULL);
    END;
    $$;
    ```

- Wijziging verwerking (doorlopende synchronisatie) van afkappen bewerkingen wordt niet ondersteund. Migratie van gepartitioneerde tabellen wordt niet ondersteund. Wanneer een gepartitioneerde tabel wordt gedetecteerd, wordt de volgende dingen gebeuren:
    - De database rapporteert een lijst met bovenliggende en onderliggende tabellen.
    - De tabel wordt gemaakt op het doel als een normale tabel met dezelfde eigenschappen als de geselecteerde tabellen.
    - Als de bovenliggende tabel in de brondatabase dezelfde primaire sleutel-waarde als de onderliggende tabellen heeft, kunt u een 'dubbele key'-fout wordt gegenereerd.
- In DMS is de limiet van databases voor het migreren van de van een enkele migratieactiviteit vier.