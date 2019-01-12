---
title: Replicatie met Azure SQL Database Managed Instance | Microsoft Docs
description: Meer informatie over het gebruik van SQL Server-replicatie met Azure SQL Database Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: mathoma
manager: craigg
ms.date: 01/11/2019
ms.openlocfilehash: 5db9e1f348fed9592d33e9240180db7f2667bb56
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54244542"
---
# <a name="replication-with-sql-database-managed-instance"></a>Replicatie met SQL Database Managed Instance

Replicatie is beschikbaar voor openbare preview-versie op [Azure SQL Database Managed Instance](sql-database-managed-instance.md). Een beheerd exemplaar kan uitgever, distributor en subscriber-databases hosten.

## <a name="common-configurations"></a>Algemene configuraties

In het algemeen de uitgever en de distributor als beide zijn in de cloud of on-premises. De volgende configuraties worden ondersteund:

- **Uitgever met lokale distributeur voor beheerd exemplaar**

   ![Replication-with-Azure-SQL-DB-Single-Managed-Instance-Publisher-Distributor](./media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

   Uitgever en de distributor databases zijn geconfigureerd op een beheerd exemplaar van één.

- **Uitgever met externe distributor voor beheerd exemplaar**

   ![Replication-with-Azure-SQL-DB-separate-Managed-Instances-Publisher-Distributor](./media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

   Uitgever en de distributor zijn geconfigureerd op twee beheerde exemplaren. In deze configuratie:

  - Beide beheerde exemplaren zijn in hetzelfde vNet.

  - Er zijn zowel beheerde exemplaren op dezelfde locatie.

- **Uitgever en de distributor on-premises met-abonnee in beheerd exemplaar**

   ![Replication-from-on-premises-to-Azure-SQL-DB-Subscriber](./media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)

   In deze configuratie is een Azure SQL database een abonnee. Deze configuratie biedt ondersteuning voor migratie van on-premises naar Azure. In de rol van de abonnee vereist SQL-database niet beheerd exemplaar, maar u een SQL Database Managed Instance als een stap in de migratie van on-premises naar Azure gebruiken kunt. Zie voor meer informatie over Azure SQL Database-abonnees, [replicatie met SQL-Database](replication-to-sql-database.md).

## <a name="requirements"></a>Vereisten

Uitgever en de distributor op Azure SQL Database is vereist:

- Beheerd exemplaar voor Azure SQL Database.

   >[!NOTE]
   >Azure SQL-Databases die niet zijn geconfigureerd met Managed Instance mag alleen bestaan uit abonnees.

- Alle exemplaren van SQL Server moeten zich in hetzelfde vNet.

- Connectiviteit maakt gebruik van SQL-verificatie tussen replicatie deelnemers.

- Een bestandsshare in Azure Storage-Account voor de replicatie-werkmap.

- Poort 445 (TCP uitgaand) moet worden geopend in de regels van de Managed Instance-subnet voor toegang tot de Azure-bestandsshare

## <a name="features"></a>Functies

Ondersteunt:

- De combinatie van de transactionele en momentopname replicatie van on-premises en Azure SQL Database Managed Instance-exemplaren.

- Abonnees kunnen worden on-premises, individuele databases in Azure SQL Database of gepoolde databases in Azure SQL Database elastische pools.

- Eenzijdige of replicatie in twee richtingen

## <a name="configure-publishing-and-distribution-example"></a>Voorbeeld van de publicatie en distributie configureren

1. [Maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-create-tutorial-portal.md) in de portal.
2. [Maak een Azure Storage-Account](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) voor de werkmap.

   Zorg ervoor dat de opslagsleutels kopiëren. Zie [opslagtoegangssleutels bekijken en kopiëren](../storage/common/storage-account-manage.md#access-keys
).
3. Maak een database voor de uitgever.

   Vervang in het van de voorbeeldscripts hieronder, `<Publishing_DB>` met de naam van deze database.

4. Maak een databasegebruiker met SQL-verificatie voor de distributor. Gebruik een beveiligd wachtwoord.

   In het van de voorbeeldscripts hieronder, gebruikt u `<SQL_USER>` en `<PASSWORD>` met dit Account voor SQL Server-database, gebruiker en wachtwoord.

5. [Verbinding maken met het beheerde exemplaar van SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms).

6. Voer de volgende query uit om toe te voegen van de distributor en de distributiedatabase.

   ```sql
   USE [master]
   GO
   EXEC sp_adddistributor @distributor = @@ServerName;
   EXEC sp_adddistributiondb @database = N'distribution';
   ```

7. Voor het configureren van een uitgever als de database van een opgegeven distributiepuntengroep wilt gebruiken, bijwerken en voer de volgende query uit.

   Vervang `<SQL_USER>` en `<PASSWORD>` met de SQL Server-Account en het wachtwoord.

   Vervang `\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>` met de waarde van uw opslagaccount.  

   Vervang `<STORAGE_CONNECTION_STRING>` met de verbindingsreeks uit de **toegangssleutels** tabblad van uw Microsoft Azure storage-account.

   Na het bijwerken van de volgende query uitvoeren.

   ```sql
   USE [master]
   EXEC sp_adddistpublisher @publisher = @@ServerName,
                @distribution_db = N'distribution',
                @security_mode = 0,
                @login = N'<SQL_USER>',
                @password = N'<PASSWORD>',
                @working_directory = N'\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>',
                @storage_connection_string = N'<STORAGE_CONNECTION_STRING>';
   GO
   ```

8. Configureer de uitgever voor replicatie.

    Vervang in de volgende query `<Publishing_DB>` met de naam van de publisher-database.

    Vervang `<Publication_Name>` met de naam voor de publicatie.

    Vervang `<SQL_USER>` en `<PASSWORD>` met de SQL Server-Account en het wachtwoord.

    Na het bijwerken van de query uitvoeren om de publicatie te maken.

   ```sql
   USE [<Publishing_DB>]
   EXEC sp_replicationdboption @dbname = N'<Publishing_DB>',
                @optname = N'publish',
                @value = N'true';

   EXEC sp_addpublication @publication = N'<Publication_Name>',
                @status = N'active';

   EXEC sp_changelogreader_agent @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>';

   EXEC sp_addpublication_snapshot @publication = N'<Publication_Name>',
                @frequency_type = 1,
                @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>'
   ```

9. Het artikel, het abonnement en de agent voor push-abonnement toevoegen.

   Als u wilt deze objecten toevoegen, bijwerken met het volgende script.

   - Vervang `<Object_Name>` met de naam van de publicatie-object.
   - Vervang `<Object_Schema>` met de naam van het schema van de gegevensbron.
   - Vervang de andere parameters punthaken `<>` zodat deze overeenkomen met de waarden in de vorige scripts.

   ```sql
   EXEC sp_addarticle @publication = N'<Publication_Name>',
                @type = N'logbased',
                @article = N'<Object_Name>',
                @source_object = N'<Object_Name>',
                @source_owner = N'<Object_Schema>'

   EXEC sp_addsubscription @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @destination_db = N'<Subscribing_DB>',
                @subscription_type = N'Push'

   EXEC sp_addpushsubscription_agent @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @subscriber_db = N'<Subscribing_DB>',
                @subscriber_security_mode = 0,
                @subscriber_login = N'<SQL_USER>',
                @subscriber_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>'
   GO
   ```

## <a name="limitations"></a>Beperkingen

De volgende functies worden niet ondersteund:

- Bij te werken abonnementen

- Actieve geo-replicatie

## <a name="see-also"></a>Zie ook

- [Wat is een beheerd exemplaar?](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)
