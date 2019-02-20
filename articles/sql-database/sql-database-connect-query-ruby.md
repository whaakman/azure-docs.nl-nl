---
title: Ruby gebruiken om een query uit te voeren voor een Azure SQL-database | Microsoft Docs
description: In dit onderwerp ziet u hoe u Ruby gebruikt om een programma te maken dat is verbonden met een Azure SQL-database, en hoe u een query voor deze database uitvoert met behulp van Transact-SQL-instructies.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ruby
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 02/12/2019
ms.openlocfilehash: bea32acf4028a4ab43fcc8cd967a31ea397babf3
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56234217"
---
# <a name="quickstart-use-ruby-to-query-an-azure-sql-database"></a>Quickstart: Ruby gebruiken om een query uit te voeren voor een Azure SQL-database

In deze snelstart wordt gedemonstreerd hoe u [Ruby](https://www.ruby-lang.org) gebruikt om verbinding te maken met een Azure SQL Database en hoe u met Transact-SQL-instructies een query uitvoert voor de gegevens.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze quickstart moet aan de volgende vereisten worden voldaan:

- Een Azure SQL-database. U kunt een van deze quickstarts gebruiken om een database te maken en vervolgens te configureren in Azure SQL Database:

  || Individuele database | Beheerd exemplaar |
  |:--- |:--- |:---|
  | Maken| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) |
  | Configureren | [IP-firewallregel op serverniveau](sql-database-server-level-firewall-rule.md)| [Connectiviteit vanaf een VM](sql-database-managed-instance-configure-vm.md)|
  |||[Connectiviteit vanaf locatie](sql-database-managed-instance-configure-p2s.md)
  |Gegevens laden|Adventure Works geladen volgens de quickstart|[Wide World Importers herstellen](sql-database-managed-instance-get-started-restore.md)
  |||Adventure Works herstellen of importeren vanuit een [BACPAC](sql-database-import.md)-bestand vanaf [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > De scripts in dit artikel zijn geschreven voor gebruik met de Adventure Works-database. Met een beheerd exemplaar moet u de Adventure Works-database in een exemplaardatabase importeren of de scripts in dit artikel wijzigen zodat deze de Wide World Importers-database gebruiken.
  
- Ruby en verwante software voor uw besturingssysteem:
  
  - **MacOS**: Installeer Homebrew, rbenv en ruby-build, Ruby, FreeTDS en TinyTDS. Zie stappen 1.2, 1.3, 1.4, 1.5 en 2.1 in [Ruby-apps maken met behulp van SQL Server in macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
  
  - **Ubuntu**: Installatievoorwaarden voor Ruby, rbenv en ruby-build, Ruby, FreeTDS en TinyTDS. Zie stappen 1.2, 1.3, 1.4, 1.5 en 2.1 in [Ruby-apps maken met behulp van SQL Server in Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).
  
  - **Windows**: Installeer Ruby, Ruby Devkit en TinyTDS. Zie [Uw ontwikkelomgeving configureren voor ontwikkeling in Ruby](/sql/connect/ruby/step-1-configure-development-environment-for-ruby-development).

## <a name="get-sql-server-connection-information"></a>SQL Server-verbindingsgegevens ophalen

Haal de verbindingsgegevens op die u nodig hebt om verbinding te maken met de Azure SQL-database. U hebt de volledig gekwalificeerde servernaam of hostnaam, databasenaam en aanmeldingsgegevens nodig voor de volgende procedures.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Navigeer naar de pagina **SQL-database** of **Met SQL beheerde exemplaren**.

3. Bekijk op de pagina **Overzicht** de volledig gekwalificeerde servernaam naast **Servernaam** voor een individuele database, of de volledig gekwalificeerde servernaam naast **Host** voor een beheerd exemplaar. Als u de servernaam of hostnaam wilt kopiëren, plaatst u de muisaanwijzer erboven en selecteert u het pictogram **Kopiëren**. 

## <a name="create-code-to-query-your-sql-database"></a>Code maken om query's uit te voeren op uw SQL-database

1. Maak in een tekst- of code-editor een nieuw bestand met de naam *sqltest.rb*.
   
1. Voeg de volgende code toe. Vervang de waarden van uw Azure SQL Database door `<server>`, `<database>`, `<username>` en `<password>`.
   
   >[!IMPORTANT]
   >Voor de code in dit voorbeeld worden de voorbeeldgegevens gebruikt van AdventureWorksLT, die u als bron kunt kiezen bij het maken van uw database. Als in uw database andere gegevens staan, kunt u tabellen uit uw eigen database gebruiken in de SELECT-query. 
   
   ```ruby
   require 'tiny_tds'
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   client = TinyTds::Client.new username: username, password: password, 
       host: server, port: 1433, database: database, azure: true
   
   puts "Reading data from table"
   tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
           ON pc.productcategoryid = p.productcategoryid"
   result = client.execute(tsql)
   result.each do |row|
       puts row
   end
   ```

## <a name="run-the-code"></a>De code uitvoeren

1. Voer de volgende opdracht uit op een opdrachtprompt:

   ```bash
   ruby sqltest.rb
   ```
   
1. Controleer of de bovenste twintig rijen voor Categorie/Product in uw database worden geretourneerd. 

## <a name="next-steps"></a>Volgende stappen
- [Uw eerste Azure SQL Database ontwerpen](sql-database-design-first-database.md).
- [GitHub-opslagplaats voor TinyTDS](https://github.com/rails-sqlserver/tiny_tds).
- [Problemen melden of vragen over TinyTDS stellen](https://github.com/rails-sqlserver/tiny_tds/issues).
- [Ruby-stuurprogramma voor SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/).
