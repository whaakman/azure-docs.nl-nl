---
title: Een back-up herstellen voor beheerd exemplaar voor Azure SQL Database | Microsoft Docs
description: Herstel een back-up van de database voor een beheerd exemplaar voor Azure SQL Database met behulp van SSMS.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: bc27ece2eddc842a81698aaa685cbe6d63c6a1df
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912251"
---
# <a name="quickstart-restore-a-database-backup-to-an-azure-sql-database-managed-instance"></a>Snelstart: een back-up van de database herstellen voor een beheerd exemplaar voor Azure SQL Database

Deze snelstartgids laat zien hoe u een back-up van een database die is opgeslagen in Azure Blob Storage, kunt herstellen naar het beheerd exemplaar met behulp van het standaard back-upbestand van Wide World Importers. Deze methode vereist enige uitvaltijd. 

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

Lees het artikel [Managed Instance migration using DMS](../dms/tutorial-sql-server-to-managed-instance.md) (Migratie van een beheerd exemplaar via DMS) voor een zelfstudie over gebruik van de Azure Database Migration Service (DMS). Zie [Migratie van SQL Server-exemplaar naar beheerd exemplaar voor Azure SQL Database](sql-database-managed-instance-migrate.md) voor een bespreking van de diverse migratiemethoden.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstartgids geldt het volgende:
- De resources die zijn gemaakt in de snelstartgids [Een Azure SQL Managed Instance maken](sql-database-managed-instance-get-started.md) vormen het uitgangspunt.
- Vereist de nieuwste versie van [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) op uw on-premises clientcomputer
- Vereist een verbinding met uw beheerde exemplaar via SQL Server Management Studio Raadpleeg deze snelstartgidsen voor connectiviteitsopties:
  - [Verbinding maken met een Azure SQL Database Managed Instance vanaf een Azure-VM](sql-database-managed-instance-configure-vm.md)
  - [On-premises verbinding maken met een Azure SQL Database Managed Instance met een punt-naar-site-verbinding](sql-database-managed-instance-configure-p2s.md).
- Maakt gebruik van een vooraf geconfigureerd Azure-account voor blobopslag met daarin het back-upbestand Wide World Importers - Standard (gedownload van https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak)).

> [!NOTE]
> Zie [Back-up van SQL Server naar URL](sql-database-managed-instance-get-started-restore.md) voor meer informatie over het maken van een back-up van een SQL Server-database met behulp van Azure-blobopslag en een SAS (Shared Access Signature) en het herstellen van de back-up.

## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>De Wide World Importers-database herstellen op basis van een back-upbestand

Volg deze stappen om in SQL Server Management Studio de Wide World Importers-database te herstellen naar uw beheerde exemplaar op basis van het back-upbestand.

1. Open SQL Server Management Studio (SSMS) en maak verbinding met het beheerde exemplaar.
2. Open een nieuw queryvenster in SQL Server Management Studio.
3. Gebruik het volgende script om een referentie te maken in het beheerde exemplaar met behulp van het vooraf geconfigureerde opslagaccount en de SAS-sleutel.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases] 
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D' 
   ```

    ![referentie maken](./media/sql-database-managed-instance-get-started-restore/credential.png)

    > [!NOTE]
    > Verwijder altijd de **?** aan het begin van de gegenereerde SAS-sleutel.
  
3. Gebruik het volgende script om de SAS-referenties en geldigheid van de back-up te controleren. Geef daarbij de URL voor de container met het back-upbestand op:

   ```sql
   RESTORE FILELISTONLY FROM URL = 
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![lijst met bestanden](./media/sql-database-managed-instance-get-started-restore/file-list.png)

4. Gebruik het volgende script om de Wide World Importers-database te herstellen op basis van een back-upbestand. Geef daarbij de URL voor de container met het back-upbestand op:

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![De pagina Restore](./media/sql-database-managed-instance-get-started-restore/restore.png)

5. Als u de status van het herstellen wilt bijhouden, voert u de volgende query uit in een nieuwe querysessie:

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
   FROM sys.dm_exec_requests r 
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

6. Wanneer het herstellen is voltooid, bekijkt u het herstel in Objectverkenner. 

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Server Backup to URL Best Practices and Troubleshooting](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting) (Aanbevolen procedures en probleemoplossing voor back-up van SQL Server naar URL) voor het oplossen van problemen met back-up naar URL.
- Zie [Verbinding maken tussen uw toepassing en het beheerde exemplaar van Azure SQL Database](sql-database-managed-instance-connect-app.md) voor een overzicht van de verbindingsopties voor toepassingen.
- Als u query's wilt uitvoeren met een van uw favoriete hulpprogramma's of talen, raadpleegt u [Connect and query](sql-database-connect-query.md) (Verbinding maken en query uitvoeren).
