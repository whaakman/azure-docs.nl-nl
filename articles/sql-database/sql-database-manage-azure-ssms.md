---
title: Een SQL-database beheren met SSMS | Microsoft Docs
description: Leer hoe u SQL Server Management Studio gebruikt om servers en databases van SQL Database te beheren.
services: sql-database
documentationcenter: .net
author: stevestein
manager: jhubbard
editor: tysonn
ms.assetid: da6f3608-5993-4134-a497-ff2811e9f31f
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/29/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: d9ff74a49742fa77f5989b8b05e0567e3ca81dc5
ms.openlocfilehash: 89cb8827745b31b3a77b64d5cafd586957d60d30


---
# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Beheren van de Azure SQL-database via SQL Server Management Studio
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [SSMS](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

U kunt SQL Server Management Studio gebruiken (SSMS) om servers en databases van SQL Database te beheren. In dit onderwerp worden algemene taken met SSMS uitgelegd. Voordat u begint, moet u al een server en database hebben gemaakt in Azure SQL Database. Zie [Uw eerste Azure SQL Database maken](sql-database-get-started.md) en [Verbinding maken en query’s uitvoeren met behulp van SSMS](sql-database-connect-query-ssms.md) voor meer informatie.

> [!TIP]
> Zie de [Zelfstudie Aan de slag](sql-database-get-started.md) voor een zelfstudie over het maken van een server, het maken van een op een server gebaseerde firewall, het weergeven van servereigenschappen, het maken van verbinding met behulp van SQL Server Management Studio, het uitvoeren van query's op de hoofddatabase, het maken van een voorbeelddatabase, het uitvoeren van query's op database-eigenschappen, het maken van verbinding met behulp van SQL Server Management Studio en het uitvoeren van query's op de voorbeelddatabase.
>

Het wordt aanbevolen dat u de nieuwste versie van SSMS gebruikt wanneer u met Azure SQL Database werkt. 

> [!IMPORTANT]
> Gebruik altijd de meest recente versie van SSMS, omdat deze voortdurend wordt verbeterd voor gebruik met de meest recente updates Azure en SQL Database. Zie [SQL Server Management Studio downloaden](https://msdn.microsoft.com/library/mt238290.aspx) om de meest recente versie te downloaden.
> 
> 

## <a name="create-and-manage-azure-sql-databases"></a>Azure SQL-databases maken en beheren
Terwijl u verbinding hebt met de **hoofd**database kunt u databases op de server maken en aanpassen of bestaande databases verwijderen. In de volgende stappen wordt beschreven hoe u verschillende algemene databasebeheertaken via Management Studio kunt uitvoeren. Om deze taken uit te voeren, moet u verbinding hebben met de **hoofd**database met de principal-aanmelding op serverniveau die u hebt gemaakt bij het instellen van uw server.

Als u een queryvenster wilt openen in Management Studio, opent u de map Databases, vouwt u de map **Systeemdatabases** uit, klikt u met de rechtermuisknop op **Hoofd** en klikt u vervolgens op **Nieuwe query**.

* Gebruik de instructie **CREATE DATABASE** om een database te maken. Zie [CREATE DATABASE (SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) voor meer informatie. Met de volgende instructie maakt u een database met de naam **myTestDB** en geeft u aan dat dit een Standard S0 Edition-database met een maximale standaardgrootte van 250 GB is.
  
      CREATE DATABASE myTestDB
      (EDITION='Standard',
       SERVICE_OBJECTIVE='S0');

Klik op **Uitvoeren** om de query uit te voeren.

* Gebruik de instructie **ALTER DATABASE** om een bestaande database te wijzigen, bijvoorbeeld als u de naam en editie van de database wilt wijzigen. Zie [ALTER DATABASE (SQL Database)](https://msdn.microsoft.com/library/ms174269.aspx) voor meer informatie. Met de volgende instructie wordt de editie van de database die u in de vorige stap hebt gemaakt, gewijzigd in Standard S1.
  
      ALTER DATABASE myTestDB
      MODIFY
      (SERVICE_OBJECTIVE='S1');
* Gebruik de instructie **DROP DATABASE** om een bestaande database te verwijderen. Zie [DROP DATABASE (SQL Database)](https://msdn.microsoft.com/library/ms178613.aspx) voor meer informatie. De volgende instructie verwijdert de **myTestDB**-database. Verwijder deze echter niet nu, omdat u deze gebruikt voor het maken van aanmeldingen in de volgende stap.
  
      DROP DATABASE myTestBase;
* De hoofddatabase bevat de **sys.databases**-weergave die u kunt gebruiken om details over alle databases weer te geven. Voer de volgende instructie uit om alle bestaande databases weer te geven:
  
      SELECT * FROM sys.databases;
* In SQL-Database wordt de instructie **USE** niet ondersteund voor het schakelen tussen databases. In plaats daarvan moet u rechtstreeks met de doeldatabase een verbinding maken.

> [!NOTE]
> Veel van de Transact-SQL-instructies die een database maken of wijzigen, moeten worden uitgevoerd binnen hun eigen batch en kunnen niet worden gegroepeerd met andere Transact-SQL-instructies. Zie de voor instructies specifieke informatie voor meer informatie.
> 
> 

## <a name="create-and-manage-logins"></a>Aanmeldingen maken en beheren
De **hoofd**database bevat aanmeldingen en informatie over welke aanmeldingen een machtiging hebben voor het maken van databases of andere aanmeldingen. U kunt aanmeldingen beheren door verbinding te maken met de **hoofd**database met de principal-aanmelding op serverniveau die u hebt gemaakt bij het instellen van uw server. U kunt de instructies **CREATE LOGIN**, **ALTER LOGIN** or **DROP LOGIN** gebruiken voor het uitvoeren van query's op de hoofddatabase die aanmeldingen overal op de server beheert. Zie [Databases en aanmeldingen beheren in Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336235.aspx) voor meer informatie. 

* Gebruik de instructie **CREATE LOGIN** om een aanmelding op serverniveau te maken. Zie [CREATE LOGIN (SQL Database)](https://msdn.microsoft.com/library/ms189751.aspx) voor meer informatie. Met de volgende instructie wordt een aanmelding met de naam **login1** gemaakt. Vervang **password1** door het wachtwoord van uw keuze.
  
      CREATE LOGIN login1 WITH password='password1';
* Gebruik de instructie **CREATE USER** om machtigingen op databaseniveau te verlenen. Alle aanmeldingen moeten worden gemaakt in de **hoofd**database. Voordat een aanmelding verbinding kan maken met een andere database, moet u deze machtigingen op databaseniveau verlenen met behulp van de instructie **CREATE USER** in die database. Zie [CREATE USER (SQL Database)](https://msdn.microsoft.com/library/ms173463.aspx) voor meer informatie. 
* Als u login1 machtigingen wilt verlenen voor een database met de naam **myTestDB**, voert u de volgende stappen uit:
  
  1. Om Objectverkenner te vernieuwen met de **myTestDB**-database die u hebt gemaakt, klikt u met de rechtermuisknop op de servernaam in Objectverkenner en klikt u vervolgens op **Vernieuwen**.  
     
     Als u de verbinding hebt verbroken, kunt u opnieuw verbinding maken door **Verbinding maken met Objectverkenner** in het menu Bestand te selecteren.
  2. Klik met de rechtermuisknop op de database **myTestDB** en selecteer **Nieuwe query**.
  3. Voer de volgende instructie uit voor de database myTestDB om een databasegebruiker met de naam **login1User** te maken die overeenkomt met de aanmelding op serverniveau **login1**.
     
         CREATE USER login1User FROM LOGIN login1;
* Gebruik de opgeslagen procedure **sp\_addrolemember** om het gebruikersaccount het juiste machtigingsniveau voor de database te geven. Zie [sp_addrolemember (Transact-SQL)](http://msdn.microsoft.com/library/ms187750.aspx) voor meer informatie. De volgende instructie geeft **login1User** alleen-lezen machtigingen voor de database door **login1User** aan de rol **db\_datareader** toe te voegen.
  
      exec sp_addrolemember 'db_datareader', 'login1User';    
* Gebruik de instructie **ALTER LOGIN** om een bestaande aanmelding te wijzigen, bijvoorbeeld als u het wachtwoord voor de aanmelding wilt wijzigen. Zie [ALTER LOGIN (SQL Database)](https://msdn.microsoft.com/library/ms189828.aspx) voor meer informatie. De instructie **ALTER LOGIN** moet worden uitgevoerd voor de **hoofd**database. Ga terug naar het queryvenster dat is verbonden met de database. De volgende instructie wijzigt de aanmelding **login1** om het wachtwoord opnieuw in te stellen. Vervang **newPassword** door het wachtwoord van uw keuze en **oldPassword** door het huidige wachtwoord voor de aanmelding.
  
      ALTER LOGIN login1
      WITH PASSWORD = 'newPassword'
      OLD_PASSWORD = 'oldPassword';
* Gebruik de instructie **DROP LOGIN** om een bestaande aanmelding te verwijderen. Wanneer een aanmelding op serverniveau wordt verwijderd, worden ook alle gekoppelde databasegebruikersaccounts verwijderd. Zie [DROP DATABASE (SQL Database)](https://msdn.microsoft.com/library/ms178613.aspx) voor meer informatie. De instructie **DROP LOGIN** moet worden uitgevoerd voor de **hoofd**database. Met deze instructie verwijdert u de aanmelding **login1**.
  
      DROP LOGIN login1;
* De hoofddatabase bevat de weergave **sys.sql\_logins** die u kunt gebruiken om aanmeldingen weer te geven. Voer de volgende instructie uit om alle bestaande aanmeldingen weer te geven:
  
      SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>SQL Database controleren met dynamische beheerweergaven
SQL Database ondersteunt verschillende dynamische beheerweergaven die u kunt gebruiken voor het controleren van een afzonderlijke database. Hieronder staan een paar voorbeelden van het type controlegegevens dat u kunt ophalen via deze weergaven. Zie [SQL Database controleren met dynamische beheerweergaven](https://msdn.microsoft.com/library/azure/ff394114.aspx) voor meer informatie en meer voorbeelden van het gebruik.

* Voor het uitvoeren van een query op een dynamische beheerweergave is een machtiging voor **VIEW DATABASE STATE** vereist. Om de machtiging **VIEW DATABASE STATE** te verlenen aan een specifieke databasegebruiker, maakt u verbinding met de database en voert u de volgende instructie uit voor de database:
  
      GRANT VIEW DATABASE STATE TO login1User;
* Bereken de databasegrootte met behulp van de weergave **sys.dm\_db\_partition\_stats**. De weergave **sys.dm\_db\_partition\_stats** haalt informatie over de pagina en het aantal rijen op voor elke partitie in de database, die u kunt gebruiken voor het berekenen van de grootte van de database. Met de volgende query wordt de grootte van de database in megabyte geretourneerd:
  
      SELECT SUM(reserved_page_count)*8.0/1024
      FROM sys.dm_db_partition_stats;   
* Gebruik de weergaven **sys.dm\_exec\_connections** en **sys.dm\_exec\_sessions** om informatie op te halen over huidige gebruikersverbindingen en interne taken die zijn gekoppeld aan de database. Met de volgende query wordt informatie over de huidige verbinding opgehaald:
  
      SELECT
          e.connection_id,
          s.session_id,
          s.login_name,
          s.last_request_end_time,
          s.cpu_time
      FROM
          sys.dm_exec_sessions s
          INNER JOIN sys.dm_exec_connections e
            ON s.session_id = e.session_id;
* Gebruik de weergave **sys.dm\_exec\_query\_stats** om cumulatieve prestatiestatistieken op te halen voor in de cache opgeslagen queryplannen. Met de volgende query wordt informatie opgehaald over de top vijf van query's gerangschikt op gemiddelde CPU-tijd.
  
      SELECT TOP 5 query_stats.query_hash AS "Query Hash",
          SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
          MIN(query_stats.statement_text) AS "Statement Text"
      FROM
          (SELECT QS.*,
          SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
          ((CASE statement_end_offset
              WHEN -1 THEN DATALENGTH(ST.text)
              ELSE QS.statement_end_offset END
                  - QS.statement_start_offset)/2) + 1) AS statement_text
           FROM sys.dm_exec_query_stats AS QS
           CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
      GROUP BY query_stats.query_hash
      ORDER BY 2 DESC;




<!--HONumber=Jan17_HO1-->


