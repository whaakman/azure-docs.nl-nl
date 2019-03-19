---
title: Replicatie naar Azure SQL Database | Microsoft-Docs'
description: Meer informatie over het gebruik van SQL Server-replicatie met Azure SQL Database, individuele databases en databases in elastische pools
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
ms.date: 01/25/2019
ms.openlocfilehash: b9d6569504b5352c6187afe12d903c986019c517
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57860700"
---
# <a name="replication-to-sql-database-single-and-pooled-databases"></a>Replicatie naar SQL Database single en gepoolde databases

SQL Server-replicatie kan worden geconfigureerd met één en gepoolde databases op een [SQL Database-server](sql-database-servers.md) in Azure SQL Database.  

## <a name="supported-configurations"></a>**Ondersteunde configuraties:**
  
- De SQL Server mag een exemplaar van SQL Server on-premises of een exemplaar van SQL Server wordt uitgevoerd in een Azure-machine in de cloud wordt uitgevoerd. Zie voor meer informatie, [SQL Server op Azure Virtual Machines overzicht](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/).  
- De Azure SQL-database moet een push-abonnement van een SQL Server-publisher.  
- De distributiedatabase en de replicatieagents kunnen niet worden geplaatst op een Azure SQL database.  
- Momentopname en één richting transactionele replicatie worden ondersteund. Transactionele peer-to-peer-replicatie en samenvoegreplicatie worden niet ondersteund.
- Replicatie is beschikbaar voor openbare preview-versie van Azure SQL Database Managed Instance. Beheerd exemplaar kan uitgever, distributor en subscriber-databases hosten. Zie voor meer informatie, [replicatie met SQL Database Managed Instance](replication-with-sql-database-managed-instance.md).

## <a name="versions"></a>Versies  

- De uitgever en de distributeur moeten ten minste op een van de volgende versies zijn:  
- SQL Server 2017 (14.x)
- SQL Server 2016 (13.x)
- SQL Server 2014 (12.x) SP1 CU3
- SQL Server 2014 (12.x) RTM CU10
- SQL Server 2012 (11.x) SP2 CU8 of SP3
- -Replicatie configureren met een oudere versie willen kan resulteren in fout MSSQL_REPL20084 (het proces kan geen verbinding met abonneeserver.) en MSSQL_REPL40532 (server kan niet worden geopend \<naam > door de aanmelding is aangevraagd. De aanmelding is mislukt.).  
- Voor het gebruik van alle functies van Azure SQL Database, moet u de nieuwste versies van [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) en [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  
  
## <a name="remarks"></a>Opmerkingen

- Replicatie kan worden geconfigureerd met behulp van [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) of door het uitvoeren van Transact-SQL-instructies op de publisher. U kunt replicatie niet configureren met behulp van de Azure-portal.  
- Replicatie kunt aanmeldingen voor SQL Server-verificatie alleen verbinding maken met een Azure SQL database gebruiken.
- Gerepliceerde tabellen moeten een primaire sleutel hebben.  
- U moet een bestaande Azure-abonnement hebben.  
- De Azure SQL database-abonnee kan zich in andere regio's.  
- Eén publicatie op SQL Server kan zowel Azure SQL Database en SQL Server (on-premises en SQL Server in een virtuele machine van Azure)-abonnees ondersteunen.  
- Replicatiebeheer, bewaking, en het oplossen van de on-premises SQL Server moet worden uitgevoerd.  
- Alleen push-abonnementen naar Azure SQL Database worden ondersteund.  
- Alleen `@subscriber_type = 0` wordt ondersteund in **sp_addsubscription** voor SQL-Database.  
- Azure SQL Database biedt geen ondersteuning voor bidirectionele, direct, worden bijgewerkt of peer-to-peer-replicatie.

## <a name="replication-architecture"></a>Replicatiearchitectuur  

![replication-to-sql-database](./media/replication-to-sql-database/replication-to-sql-database.png)  

## <a name="scenarios"></a>Scenario's  

### <a name="typical-replication-scenario"></a>Typische replicatiescenario  

1. De publicatie van een transactionele replicatie op een on-premises SQL Server-database maken.  
2. Op de on-premises SQL Server gebruikt de **Wizard nieuwe abonnement** of Transact-SQL-instructies voor het maken van een abonnement op Azure SQL Database-push.  
3. Met één en gepoolde databases in Azure SQL Database is de initiële gegevensset een momentopname die is gemaakt door de Snapshot Agent en gedistribueerd en toegepast door de Agent voor softwaredistributie. Met de database van een beheerd exemplaar, kunt u ook een databaseback-up seeding van de database op de abonnee te gebruiken.

### <a name="data-migration-scenario"></a>Scenario voor migratie van gegevens  

1. Transactionele replicatie gebruiken om gegevens te repliceren van een on-premises SQL Server-database naar Azure SQL Database.  
2. Omleiden van de client of de middelste laag toepassingen om bij te werken van de Azure SQL database-exemplaar.  
3. Stop de SQL Server-versie van de tabel bijwerken en verwijderen van de publicatie.  

## <a name="limitations"></a>Beperkingen

De volgende opties worden niet ondersteund voor Azure SQL Database-abonnementen:

- Groepen bestandskoppeling kopiëren  
- Tabel partitieschema's kopiëren  
- Index partitieschema's kopiëren  
- Kopiëren door de gebruiker gedefinieerde statistieken  
- Kopieer de standaard-bindingen  
- Regel bindingen kopiëren  
- Indexen voor volledige tekst kopiëren  
- XML-XSD kopiëren  
- XML-indexen kopiëren  
- Machtigingen voor kopiëren  
- Ruimtelijke indexen kopiëren  
- Gefilterde indexen kopiëren  
- Kopiëren van gegevens compressiekenmerk  
- Kenmerk voor sparse kolom kopiëren  
- Filestream converteren naar MAX-gegevenstypen  
- Hierarchyid converteren naar MAX-gegevenstypen  
- Ruimtelijke gegevenstypen MAX converteren  
- Uitgebreide eigenschappen kopiëren  
- Machtigingen voor kopiëren  

### <a name="limitations-to-be-determined"></a>Beperkingen worden bepaald

- Sortering kopiëren  
- Uitvoering in een geserialiseerde transactie van de Serviceprovider  

## <a name="examples"></a>Voorbeelden

Maak een publicatie en een push-abonnement. Zie voor meer informatie:
  
- [Maken van een publicatie](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Maken van een Push-abonnement](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/) met behulp van de naam van de Azure SQL Database-server als de abonnee (bijvoorbeeld **N'azuresqldbdns.database.windows.net'**) en de naam van de Azure SQL-database als de doeldatabase (voor voorbeeld **AdventureWorks**).  

## <a name="see-also"></a>Zie ook  

- [Transactionele replicatie](sql-database-managed-instance-transactional-replication.md)
- [Maken van een publicatie](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Een Push-abonnement maken](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Typen van de replicatie](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Bewaking (replicatie)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Initialiseren van een abonnement](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
