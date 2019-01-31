---
title: Replicatie configureren in Azure SQL Database Managed Instance | Microsoft Docs
description: Meer informatie over het configureren van transactionele replicatie in Azure SQL Database Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: allenwux
ms.author: xiwu
ms.reviewer: mathoma
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: b0188a0983ea18490f3997b857386e313daa58ed
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55467660"
---
# <a name="configure-replication-in-azure-sql-database-managed-instance"></a>Replicatie in Azure SQL Database Managed Instance configureren

Transactionele replicatie kunt u voor het repliceren van gegevens uit de SQL Server of Azure SQL Database Managed Instance-databases naar het beheerd exemplaar of wijzigingen aangebracht in uw databases in het beheerde exemplaar van andere SQL-Server, SQL Database single database of elastische pool, of andere Managed Instance. Replicatie is in de openbare preview op [Azure SQL Database Managed Instance](sql-database-managed-instance.md). Een beheerd exemplaar kan uitgever, distributor en subscriber-databases hosten. Zie [transactionele replicatie configuraties](sql-database-managed-instance-transactional-replication.md#common-configurations) voor beschikbare configuraties.

## <a name="requirements"></a>Vereisten

Uitgever en de distributor op Azure SQL Database is vereist:

- Azure SQL Database Managed Instance die zich niet in de Geo-DR-configuratie.

   >[!NOTE]
   >Azure SQL-Databases die niet zijn geconfigureerd met Managed Instance mag alleen bestaan uit abonnees.

- Alle exemplaren van SQL Server moeten zich in hetzelfde vNet.

- Connectiviteit maakt gebruik van SQL-verificatie tussen replicatie deelnemers.

- Een bestandsshare in Azure Storage-Account voor de replicatie-werkmap.

- Poort 445 (TCP uitgaand) moet worden geopend in de regels van de Managed Instance-subnet voor toegang tot de Azure-bestandsshare

## <a name="features"></a>Functies

Ondersteunt:

- De combinatie van de transactionele en momentopname replicatie van on-premises en Azure SQL Database Managed Instance-exemplaren.

- Abonnees kunnen worden on-premises, individuele database in Azure SQL Database of gepoolde databases in Azure SQL Database elastische pools.

- Eenzijdige of replicatie in twee richtingen.

De volgende functies worden niet ondersteund:

- Bij te werken abonnementen.

- Actieve geo-replicatie.

## <a name="configure-publishing-and-distribution-example"></a>Voorbeeld van de publicatie en distributie configureren

1. [Maken van een Azure SQL Database Managed Instance](sql-database-managed-instance-create-tutorial-portal.md) in de portal.
2. [Maak een Azure Storage-Account](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) voor de werkmap.

   Zorg ervoor dat de opslagsleutels kopiëren. Zie [opslagtoegangssleutels bekijken en kopiëren](../storage/common/storage-account-manage.md#access-keys
).
3. Maak een database voor de uitgever.

   Vervang in het van de voorbeeldscripts hieronder, `<Publishing_DB>` met de naam van deze database.

4. Maak een databasegebruiker met SQL-verificatie voor de distributor. Gebruik een beveiligd wachtwoord.

   In het van de voorbeeldscripts hieronder, gebruikt u `<SQL_USER>` en `<PASSWORD>` met dit Account voor SQL Server-database, gebruiker en wachtwoord.

5. [Verbinding maken met het beheerde exemplaar van SQL Database](sql-database-connect-query-ssms.md).

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
   
## <a name="see-also"></a>Zie ook

- [Transactionele replicatie](sql-database-managed-instance-transactional-replication.md)
- [Wat is een beheerd exemplaar?](sql-database-managed-instance.md)
